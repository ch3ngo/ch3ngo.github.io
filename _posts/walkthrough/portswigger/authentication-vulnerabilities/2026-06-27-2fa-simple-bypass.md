---
title: PortSwigger Walkthrough - 2FA Simple Bypass
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, authentication, 2fa]
author: diego
description: Walkthrough of PortSwigger's '2FA Simple Bypass' lab.
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
Two-factor authentication adds a second verification step after the password. The idea is that even if the password is compromised, the attacker still needs the second factor. That assumption falls apart the moment the server does not actually enforce the second step.

In some implementations, the session is considered "partially authenticated" after the password check, and protected pages only verify that you passed step 1, not that you also completed step 2. In those cases, 2FA is essentially decorative: you can skip the verification page entirely by navigating directly to where you want to go.

## Objective
The victim's credentials are known (`carlos:montoya`), but you do not have access to their 2FA code. Access Carlos's account page without it.

> [PortSwigger's lab link](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass)
{: .prompt-info }

## Walkthrough
Start by logging in as `wiener:peter` with the proxy active to observe the normal flow. After the password step, we land on `/login2` where the 2FA code is requested, we enter it, and we end up on `/my-account?id=wiener`. Straightforward.

Now log out and log in as `carlos:montoya`. We land on `/login2` again with no way to get the code. Instead of trying to guess it, just change the URL in the browser from `/login2` to `/my-account`.

The server does not verify that we completed the 2FA step. The session cookie set after the password step is enough to access the account page directly. We land on Carlos's profile without ever touching the verification form.

Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/auth/14-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
