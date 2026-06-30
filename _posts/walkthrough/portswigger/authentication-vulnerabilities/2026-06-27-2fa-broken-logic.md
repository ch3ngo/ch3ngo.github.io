---
title: PortSwigger Walkthrough - 2FA Broken Logic
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, authentication, 2fa]
author: diego
description: Walkthrough of PortSwigger's '2FA Broken Logic' lab.
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
2FA flaw, this time a bit more subtle. The 2FA check is there and it runs. The problem is that the user identity for the 2FA step is read from a client-controlled cookie called `verify`, not from the authenticated session. An attacker can change this cookie to point at any account, force the server to generate a 2FA code for that account, and then brute-force the 4-digit code without ever needing the victim's password to complete the 2FA flow.

The verification is happening, just for the wrong person.

## Objective
Access Carlos's account page. Credentials `wiener:peter` and access to wiener's email are provided. Carlos will not log in himself.

> [PortSwigger's lab link](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-broken-logic)
{: .prompt-info }

## Walkthrough
Log in as `wiener:peter` with the proxy active. After the password step, inspect the cookies. We will see two: the regular `session` cookie and a `verify` cookie set to `wiener`. This `verify` cookie is what the server uses to know which user's 2FA code to check during the `/login2` step.

Go to the `GET /login2` request in the proxy history, send it to Repeater, change the `verify` cookie to `carlos`, and send it. This forces the server to generate a new 2FA code for Carlos's account, even though Carlos has not initiated any login.

![Modifying the verify cookie to carlos](/assets/img/posts/portswigger/auth/15-modify-cookie.png)

Now we need to brute-force that 4-digit code. Log in again as `wiener`, reach `/login2`, and capture the `POST /login2` request (the one that submits the code). Send it to Intruder and configure:
- Change the `verify` cookie to `carlos`
- Set the payload position on the MFA code field
- Payload type: `Numbers`, range `0000` to `9999`, min digits 4 (to preserve leading zeros)

![Intruder configuration for 2FA brute force](/assets/img/posts/portswigger/auth/16-intruder-config.png)

Now, I really thought that just changing the cookie and submitting a random code would work first, but we got an `Invalid security code` error (tbh I was a bit disappointed in myself). Brute-forcing 10,000 codes is the only way.

> Burp Suite Community Edition will be very slow here. 10,000 requests with CE's rate limiting is not a fun experience. If you have Professional, use it.
{: .prompt-warning }

Start the attack and look for the `302` redirect. That is the valid code.

![Intruder results showing the valid 2FA code](/assets/img/posts/portswigger/auth/17-intruder-results.png)

Open the successful request in the browser and we land on Carlos's account page. Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/auth/18-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
