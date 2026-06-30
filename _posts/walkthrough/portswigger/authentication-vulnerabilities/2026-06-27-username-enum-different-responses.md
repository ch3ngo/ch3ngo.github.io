---
title: PortSwigger Walkthrough - Username enumeration via different responses
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, authentication, user enumeration]
author: diego
description: Walkthrough of PortSwigger's 'Username enumeration via different responses' lab.
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
Username enumeration is often the very first step when attacking an authentication system. Instead of brute-forcing both username and password at the same time, an attacker first confirms which usernames actually exist, then focuses the password attack on those. This turns a two-variable problem into a one-variable one and cuts the time to access down significantly.

One of the most common (and easiest to fix) ways an application leaks this is through different error messages. "Invalid username" and "Incorrect password" are functionally identical to a legitimate user who just mistyped something, but to an attacker they are a map. One tells you the username is wrong, the other tells you the username is right but the password is not. That is all you need to start enumerating.

## Objective
This lab is vulnerable to username enumeration and password brute-force attacks.

> [PortSwigger's lab link](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses)
{: .prompt-info } 

There is an account with a predictable username and password, both findable in PortSwigger's candidate wordlists. Enumerate a valid username, brute-force its password, and log in.

## Walkthrough
With the proxy active, navigate to `My account` and make any login attempt. We just need to capture the `POST /login` request in Burp's proxy history, so the credentials do not matter at all here. Once we have it, send it to Intruder (`Ctrl+I` or right-click → Send to Intruder).

In the Intruder tab, clear all the auto-detected positions and set the payload position on the username value only, leaving the password as anything. Load the [candidate usernames wordlist](https://portswigger.net/web-security/authentication/auth-lab-usernames) from the lab page and start the attack.

Once the attack finishes, sort the results by `Length`. One response will have a noticeably different length. When we inspect it, the error message reads `Incorrect password` instead of `Invalid username`, which confirms the username exists. In this case, the valid username is `at`.

![Intruder attack results showing the length difference](/assets/img/posts/portswigger/auth/02-intruder-attack.png)

Now go back to the Intruder configuration, fix `username=at` in the request body, move the payload position to the password value, and load the [candidate passwords wordlist](https://portswigger.net/web-security/authentication/auth-lab-passwords). Start the attack again and sort by `Status`: the successful login produces a `302` redirect with a different length. The valid password is `moscow`.

Log in as `at:moscow` and the lab is solved.

![Lab solved confirmation](/assets/img/posts/portswigger/auth/03-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
