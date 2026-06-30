---
title: PortSwigger Walkthrough - File path traversal, sequences stripped non-recursively
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, path traversal]
author: diego
description: Walkthrough of PortSwigger's 'File path traversal, sequences stripped non-recursively' lab.
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
Stripping `../` from user input is a valid idea, but only if the stripping is applied repeatedly until none remain. A single non-recursive pass can be bypassed by nesting the traversal sequence inside itself: when the inner `../` is removed, the outer characters collapse back into a valid traversal sequence.

The pattern `....//` is the standard example. Visually it looks like two dots, two more dots, and two slashes. Reading through it, the substring `../` appears at positions 2-4 (characters `.`, `.`, `/`). After a single stripping pass removes that `../`, the remaining characters `.`, `.`, `/` reconstruct a new `../`. One pass is not enough.

## Objective
The application strips traversal sequences from the filename parameter but only makes a single pass. Use nested sequences to reconstruct the traversal after stripping and read `/etc/passwd`.

> [PortSwigger's lab link](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)
{: .prompt-info }

## Walkthrough
Catch an image request in Burp's HTTP history and send it to Repeater. Confirm that a plain `../../../etc/passwd` is blocked, then use the nested variant:

```
GET /image?filename=....//....//....//etc/passwd
```

Walk through what happens on the server: each `....//` block contains `../` starting at its third character. The first stripping pass removes those inner `../` sequences, leaving `../` from the outer characters. The resulting string after one pass is `../../../etc/passwd`, which the filesystem resolves normally.

The response contains the full contents of `/etc/passwd`.

Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/path-traversal/03-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
