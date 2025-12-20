---
title: DVWA Walkthrough V - File Upload
date: 2025-12-20 13:00:14 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, file upload, shell, reverse]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 5, File Upload.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, explore the [comprehensive DVWA walkthrough](/posts/dvwa_walkthrough/) or browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

# **File Upload**
## What's this?
File upload vulnerabilities occur when web applications accept and process user-uploaded files without proper validation, allowing malicious content like webshells or executables to be stored and executed. PortSwigger Academy explains that flaws include missing MIME type checks, unsafe file extensions, or predictable storage paths, enabling server-side execution. In DVWA, this module simulates unrestricted uploads across security levels, demonstrating real-world risks like PHP shell injection.

Malicious uploads lead to remote code execution, backdoor persistence, or complete server compromise, often resulting in data breaches or ransomware deployment.

## Objective
The main goal of this module is to **execute any PHP function of your choosing on the target system thanks to this vulnerability**. In our case, we are going to obtain a **reverse shell**.

## **Security: Low**
> **Help**  
> Low level will not check the contents of the file being uploaded in any way. It relies only on trust.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/upload/source/low.php).
{: .prompt-info }

This security level is straightforward. You can upload any kind of file without restrictions other than file size. In this case, I am using PentestMonkey’s PHP reverse shell.
1. Modify the `ip` and `port` variables in the reverse shell payload and upload the file:
![Reverse Shell Config](assets/img/posts/dvwa/5_revshell.png)
![File uploaded](assets/img/posts/dvwa/5_upload_1.png)

1. Once uploaded, start a listener in your machine: `nc -lvp 1234`  
And load the uploaded file in your browser at `http://192.168.1.145:4280/hackable/uploads/pentestmonkey-php_revshell.php`  
You will receive a reverse shell:
![FU Low Done](assets/img/posts/dvwa/5_low.png)

## **Security: Medium**
> **Help**  
> When using the medium level, it will check the reported file type from the client when its being uploaded.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/upload/source/medium.php).
{: .prompt-info }

1. At this security level, we can no longer upload files that are not images:
![Only images](assets/img/posts/dvwa/5_img.png)
2. We can rename our payload to `pentestmonkey-php_revshell.php.jpeg` and upload it while Burp interception is enabled. In the intercepted request, we modify the filename back to a `.php` file. The server then accepts the upload:
![Request intercept](assets/img/posts/dvwa/5_req.png)
![FU Medium Done](assets/img/posts/dvwa/5_medium.png)
1. The rest of the process is the same: once uploaded, start a listener on your machine (`nc -lvp 1234`) and access the uploaded file from your browser at: `http://192.168.1.145:4280/hackable/uploads/pentestmonkey-php_revshell.php`. You will receive your reverse shell.

## **Security: High**
> **Help**  
> Once the file has been received from the client, the server will try to resize any image that was included in the request.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/upload/source/high.php).
{: .prompt-info }

If we try to upload our payload as in the previous levels, it will be rejected. It appears that the server is now checking the file contents. We can bypass this by modifying the first line of the file so the application treats it as a legitimate image:  

![Legit File](assets/img/posts/dvwa/5_legit.png)
![File uploaded](assets/img/posts/dvwa/5_uploaded.png)

At this point, we can’t obtain a reverse shell just by directly accessing the uploaded file. We need to exploit another vulnerability from a different module. In this case, we use the file inclusion vulnerability with the following payload: `http://192.168.1.131:4280/vulnerabilities/fi/?page=file1.php%0A/../../../hackable/uploads/pentestmonkey-php_revshell.php.jpeg`.  
![FU High Done](assets/img/posts/dvwa/5_high.png)

Voilà!

## References
- [pentestmonkey PHP Reverse Shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)
- [PortSwigger File Upload](https://portswigger.net/web-security/file-upload)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
