---
title: PortSwigger Walkthrough - Broken brute-force protection, IP block
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, authentication, brute force]
author: diego
description: Walkthrough of PortSwigger's 'Broken brute-force protection' lab.
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
Not all brute-force protections are created equal. This one counts failed login attempts and blocks the IP after hitting a threshold, which sounds reasonable on paper. The catch is that a successful login resets the counter. If an attacker controls a valid account, they can interleave their own successful logins between attack attempts and keep the counter at zero indefinitely. The lockout never triggers.

## Objective
Exploit the logic flaw in the brute-force protection to enumerate Carlos's password without getting blocked. Credentials `wiener:peter` are provided.

> [PortSwigger's lab link](https://portswigger.net/web-security/authentication/password-based/lab-broken-bruteforce-protection-ip-block)
{: .prompt-info }

## Walkthrough
The lab blocks the IP after 3 failed attempts. The trick is to make 2 attempts against Carlos and then log in successfully as Wiener before the third, which resets the counter. Repeat this pattern across the entire password wordlist and we never get blocked.

Spoiler: this can be done in several ways and all of them are valid, so if you want to see a different approach, there are other walkthroughs out there. This is the one I went with.

We need two wordlists that iterate in sync. For the password list, we take the [candidate passwords wordlist](https://portswigger.net/web-security/authentication/auth-lab-passwords) and insert `peter` (Wiener's password) after every 2 entries. You can do this with a quick sed command:

```bash
# passwords.txt is PortSwigger's candidate passwords list
sed -i '0~2a\peter' passwords.txt
```

For the username list, we need a matching sequence of `carlos, carlos, wiener` repeating 50 times (so 150 entries total):

```bash
yes "carlos
carlos
wiener" | head -n 150 > users.txt
```

Now send a login attempt to Intruder, set the attack type to **Pitchfork**, and add payload positions on both `username` and `password`. Load `users.txt` as payload set 1 and the modified `passwords.txt` as payload set 2. Pitchfork iterates both lists in parallel so the interleaving pattern stays intact.

Start the attack. Filter by status code `302` to find the successful Carlos login. The valid password is `thomas`.

Log in as `carlos:thomas` and the lab is done.

![Lab solved confirmation](/assets/img/posts/portswigger/auth/13-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
