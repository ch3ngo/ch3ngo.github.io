---
title: PortSwigger Walkthrough - File path traversal, sequences blocked with absolute path bypass
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, path traversal]
author: diego
description: Walkthrough of PortSwigger's 'File path traversal, sequences blocked with absolute path bypass' lab.
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
A common first attempt at defending against path traversal is to block `../` sequences outright. The logic is: if you cannot go up the directory tree, you cannot escape the intended path. This is incomplete. If the application also accepts absolute paths, an attacker can simply supply the full target path directly, bypassing the traversal check entirely. No `../` needed.

## Objective
The application strips traversal sequences from the filename parameter. Use an absolute path to read `/etc/passwd` directly.

> [PortSwigger's lab link](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)
{: .prompt-info }

## Walkthrough
Catch an image request in Burp's HTTP history, something like:

```
GET /image?filename=45.jpg
```

Send it to Repeater. First confirm the defense is there by trying `../../../etc/passwd`: you will get an error or the default 404-style response, meaning the traversal sequences are being stripped or rejected.

Now try an absolute path instead:

```
GET /image?filename=/etc/passwd
```

The response returns the full contents of `/etc/passwd`. The application's input handling blocked relative traversal but passed the value directly to the underlying file read function, which happily accepted an absolute path.

Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/path-traversal/02-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
