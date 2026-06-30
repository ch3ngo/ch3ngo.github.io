---
title: PortSwigger Walkthrough - Password reset broken logic
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
Password reset flows are full of logic flaws, and this is one of the most straightforward ones. The reset token proves you received an email to a specific address. What it should not do is let you decide which account to reset. If the username is a separate, client-supplied parameter in the reset request, you can use your own valid token to reset anyone else's password. The token and the account are not bound to each other server-side.

## Objective
Reset Carlos's password without access to his email, then log in and access his account page. Credentials `wiener:peter` are provided.

> [PortSwigger's lab link](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-broken-logic)
{: .prompt-info }

## Walkthrough
Go to the login page and click **Forgot password?**. Request a reset for `wiener`, open the email via the exploit server's email client, and follow the reset link. Fill in a new password and submit with the proxy active.

Inspect the `POST /forgot-password` request. The body will look like:

```
temp-forgot-password-token=TOKEN&username=wiener&new-password-1=newpass&new-password-2=newpass
```

There it is: `username=wiener` as a plain POST parameter. The server is trusting the client to specify which account is being reset. Send this request to Repeater, change `username=wiener` to `username=carlos`, set any new password for both fields, and send.

The server resets Carlos's password. Log in as `carlos` with the new password we set and the lab is done.

![Lab solved confirmation](/assets/img/posts/portswigger/auth/22-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
