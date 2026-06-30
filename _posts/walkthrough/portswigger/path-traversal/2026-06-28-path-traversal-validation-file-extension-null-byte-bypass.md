---
title: PortSwigger Walkthrough - File path traversal, validation of file extension with null byte bypass
date: 2026-06-28 13:30:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, path traversal]
author: diego
description: Walkthrough of PortSwigger's 'File path traversal, validation of file extension with null byte bypass' lab.
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
Some applications validate that the filename ends with an expected extension, like `.png` or `.jpg`, before passing it to the filesystem. This stops a raw `../../../etc/passwd` since it does not end with an image extension.

The classic bypass is the null byte: `%00`. In C-based string handling, a null byte marks the end of a string. When the application validates the extension, it sees the full string including the `.png` appended after `%00`. But when the underlying file read function processes it, the string terminates at the null byte, effectively truncating the extension away. The actual file opened is `../../../etc/passwd`.

This technique works in environments where the language passes file paths to system calls that use null-terminated C strings, which includes older PHP versions and several other server-side stacks.

## Objective
The application checks that the filename ends with an expected image extension. Use a null byte to satisfy the extension check while reading `/etc/passwd`.

> [PortSwigger's lab link](https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass)
{: .prompt-info }

## Walkthrough
Catch an image request in Burp and send it to Repeater. A plain traversal like `../../../etc/passwd` is rejected because it does not end with `.png` (or whatever the expected extension is). Append a null byte followed by a valid extension:

```
GET /image?filename=../../../etc/passwd%00.png
```

The validation logic reads the full string and sees `.png` at the end: check passed. The file system call reads up to the null byte and opens `../../../etc/passwd`. The response contains the file contents.

Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/path-traversal/06-lab-solved.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
