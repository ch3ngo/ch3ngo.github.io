---
title: PortSwigger Walkthrough - File path traversal, validation of start of path
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, path traversal]
author: diego
description: Walkthrough of PortSwigger's 'File path traversal, validation of start of path' lab.
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
Some applications try to lock down path traversal by requiring the filename to start with a specific base directory, such as `/var/www/images/`. The idea is that if the path starts where it should, it is safe. The flaw is that a path can start correctly and still traverse outside it: `/var/www/images/../../../etc/passwd` satisfies the prefix check but resolves to `/etc/passwd` once the OS canonicalizes it.

Checking the prefix of a user-supplied path is not the same as canonicalizing the path first and then checking. These applications do the former.

## Objective
The application requires the filename to start with `/var/www/images/`. Supply a path that satisfies the prefix check but traverses out of the base directory to read `/etc/passwd`.

> [PortSwigger's lab link](https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path)
{: .prompt-info }

## Walkthrough
Catch an image request in Burp, which will look like:

```
GET /image?filename=/var/www/images/45.jpg
```

Note that the full path is already in the parameter here. Send it to Repeater and change the value to:

```
GET /image?filename=/var/www/images/../../../etc/passwd
```

The prefix check passes because the path starts with `/var/www/images/`. The OS then resolves the traversal sequences: `images/` goes up to `www/`, up to `var/`, up to `/`, then follows `etc/passwd`. The response contains the contents of `/etc/passwd`.

Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/path-traversal/05-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
