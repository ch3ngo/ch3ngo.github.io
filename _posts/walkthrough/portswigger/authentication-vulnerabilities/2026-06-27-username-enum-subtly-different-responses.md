---
title: PortSwigger Walkthrough - Username enumeration via subtly different responses
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, authentication, user enumeration]
author: diego
description: Walkthrough of PortSwigger's 'Username enumeration via subtly different responses' lab.
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
Same concept as the [previous lab](/posts/username-enum-different-responses), but this time the developer made more of an effort. The error messages look identical to the naked eye: "Invalid username or password." But one of them is missing the trailing period. That single character is enough.

This lab demonstrates that you do not need an obvious difference to leak username information. Even tiny, nearly invisible inconsistencies in response content can be detected automatically and exploited. Sorting by length would still work here, but for responses this similar, Burp's `Grep - Extract` feature is the cleaner tool.

## Objective
Enumerate a valid username through a subtle difference in the error message, brute-force the password, and log in.

> [PortSwigger's lab link](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-subtly-different-responses)
{: .prompt-info }

## Walkthrough
The setup is identical to the previous lab: capture a `POST /login` request, send it to Intruder, set the payload position on the username, and load the [candidate usernames wordlist](https://portswigger.net/web-security/authentication/auth-lab-usernames).

Before starting the attack, go to `Settings` in the Intruder tab and find the `Grep - Extract` section. Click `Add`, then fetch a response from the target and select the content of the error message element:

```
Invalid username or password.
```

Click OK. Intruder will now pull that exact string from every response into its own column, making comparison trivial.

![Grep-Extract configuration in Burp Intruder](/assets/img/posts/portswigger/auth/04-grep-extract.png)

Start the attack. Once it finishes, sort by the extracted column. Every row will show `Invalid username or password.` except one, which shows `Invalid username or password` without the period. That is the valid username: `applications`.

![Results showing the valid username with the missing period](/assets/img/posts/portswigger/auth/05-valid-username.png)

Now fix `username=applications` in the request, move the payload position to the password, and load the [candidate passwords wordlist](https://portswigger.net/web-security/authentication/auth-lab-passwords). Run the attack and look for the `302` redirect. The credentials are `applications:andrew`.

Log in and the lab is done.

![Lab solved confirmation](/assets/img/posts/portswigger/auth/06-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
