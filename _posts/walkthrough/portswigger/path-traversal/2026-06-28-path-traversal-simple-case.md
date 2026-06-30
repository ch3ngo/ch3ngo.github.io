---
title: PortSwigger Walkthrough - File path traversal, simple case
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, path traversal]
author: diego
description: Walkthrough of PortSwigger's 'File path traversal, simple case' lab.
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
Path traversal (also called directory traversal) is a vulnerability that allows an attacker to read arbitrary files on the server by manipulating a file path parameter. When an application uses user-supplied input to construct a file path and reads its contents, an attacker can inject sequences like `../` to navigate up the directory tree and reach files outside the intended directory.

The classic example: if the application loads images at `/var/www/images/<filename>` and does not validate the input, requesting `../../../etc/passwd` as the filename resolves to `/etc/passwd`, exposing the server's user list.

## Objective
The application loads product images via a parameter that is directly used to read files from disk. Read the contents of `/etc/passwd`.

> [PortSwigger's lab link](https://portswigger.net/web-security/file-path-traversal/lab-simple-case)
{: .prompt-info }

## Walkthrough
With the proxy active, open any product page. The page loads product images; we need to catch one of those image requests. In Burp's HTTP history, look for a `GET` request like:

```
GET /image?filename=45.jpg
```

The `filename` parameter is the target. Send the request to Repeater and modify the value:

```
GET /image?filename=../../../etc/passwd
```

Send it. The response body contains the contents of `/etc/passwd`, including system accounts and the `carlos` user.

No encoding, no tricks needed. The application passes the filename directly to the filesystem read with no sanitization.

Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/path-traversal/01-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
