---
title: DVWA Complete Walkthrough
date: 2025-12-20 13:00:19 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa]
author: diego
description: A complete (more or less) walkthrough of the Damn Vulnerable Web Application (DVWA) for security testing and learning purposes.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

> All testing shown in this series is performed locally against DVWA, an intentionally vulnerable application.  
> Do not apply these techniques to systems you do not own or have explicit permission to test.
{: .prompt-warning }

## **Introduction**
### What's this?
Damn Vulnerable Web Application (DVWA) is a PHP/MariaDB web application that is, as the name suggests, intentionally vulnerable. Its main goal is to help security professionals test their skills and tools in a legal environment, assist web developers in better understanding how to secure web applications, and aid both students and teachers in learning about web application security in a controlled classroom environment.

The aim of DVWA is to practice some of the most common web vulnerabilities, across various difficulty levels, using a simple and straightforward interface. Please note that there are both documented and undocumented vulnerabilities in this software. This is intentional, and you are encouraged to try to discover as many issues as possible.

This series documents my walkthrough of DVWA modules, explaining how each vulnerability works, why it exists, and how it can be exploited across different security levels.

### Security levels
The DVWA server has four different security levels, which can be configured from the `DVWA Security` menu:
- **Low**: This security level is completely vulnerable and has no security measures at all. It's use is to be as an example of how web application vulnerabilities manifest through bad coding practices and to serve as a platform to teach or learn basic exploitation techniques.
- **Medium**: This setting is mainly to give an example to the user of bad security practices, where the developer has tried but failed to secure an application. It also acts as a challenge to users to refine their exploitation techniques.
- **High**: This option is an extension to the medium difficulty, with a mixture of harder or alternative bad practices to attempt to secure the code. The vulnerability may not allow the same extent of the exploitation, similar in various Capture The Flags (CTFs) competitions.
- **Impossible**: This level should be secure against all vulnerabilities. It is used to compare the vulnerable source code to the secure source code.  
Prior to DVWA v1.9, this level was known as 'high'.

![alt text](assets/img/posts/dvwa/0_levels.png)

In this series, we will only cover the first three security levels.

### Who is this for?

This walkthrough is aimed at:
- Students learning web application security
- Pentesters and red teamers refreshing fundamentals
- Anyone preparing for labs, certifications, or interviews

Basic knowledge of HTTP, HTML, and JavaScript is assumed.

### How to use this series?
- Each module is documented independently
- Modules can be followed in order or referenced individually
- Payloads and screenshots are provided for clarity
- Small differences may appear depending on browser or tool versions

## **Installation**
For this lab, I installed and used **DVWA v2.5 with Docker**. The setup is pretty straightforward, but I made a couple of changes so the application could be accessed from my local network, as I wanted to attack it from different machines.  
Download DVWA, either by downloading the ZIP file or using Git:
```bash
git clone https://github.com/digininja/DVWA.git
```
Inside the DVWA folder, modify the `compose.yml` file. Specifically, update the port binding: 

```yml
services:
  dvwa:
    build: .
    image: ghcr.io/digininja/dvwa:latest
    # [...]
    ports:
      - 0.0.0.0:4280:80 #Modified: 127.0.0.1 -> 0.0.0.0
    restart: unless-stopped
```
The virtual machines I used were configured in bridge mode, allowing them to access the local network.  
- As I am using Docker Desktop, from a terminal inside the DVWA folder, run: `docker compose up -d`  
- Navigate to `http://<your_ip>:4280/login.php` and log in using the default credentials: `admin:password`  
- On the first login, you will be redirected to a `Database Setup` page. Scroll to the bottom and click `Create / Reset Database`.  

That's it, you can now log in and start playing!

> Do not upload it to your hosting provider's public html folder or any Internet facing servers, as they will be compromised. It is recommended using a virtual machine or environment, which is set to NAT networking mode.
{: .prompt-danger }

## **Modules Walkthrough**
1. [Brute Force](/posts/brute-force_dvwa_walkthrough)
2. [Command Injection](/posts/command-injection_dvwa_walkthrough)
3. [Cross-Site Request Forgery (CSRF)](/posts/csrf_dvwa_walkthrough)
4. [File Inclusion](/posts/file-inclusion_dvwa_walkthrough)
5. [File Upload](/posts/file-upload_dvwa_walkthrough)
6. [Insecure CAPTCHA](/posts/insecure-captcha_dvwa_walkthrough)
7. [SQL Injection](/posts/sqli_dvwa_walkthrough)
8. [SQL Injection (Blind)](/posts/blind-sqli_dvwa_walkthrough)
9. [Weak Session IDs](/posts/weak-ids_dvwa_walkthrough)
10. [DOM Based Cross Site Scripting (XSS)](/posts/dom-xss_dvwa_walkthrough)
11. [Reflected Cross Site Scripting (XSS)](/posts/reflected-xss_dvwa_walkthrough)
12. [Stored Cross Site Scripting (XSS)](/posts/stored-xss_dvwa_walkthrough)
13. [Content-Security Policy Bypass](/posts/csp-bypass_dvwa_walkthrough)
14. [JavaScript Attacks](/posts/js-attacks_dvwa_walkthrough)
15. [Authorisation Bypass](/posts/authorisation-bypass_dvwa_walkthrough)
16. [Open HTTP Redirect](/posts/open-redirect_dvwa_walkthrough)
17. [Cryptography](/posts/cryptography_dvwa_walkthrough)
18. [API Security](/posts/api-sec_dvwa_walkthrough)

## References
- [DVWA official GitHub](https://github.com/digininja/DVWA)
- [PortSwigger Academy](https://portswigger.net/web-security)
- [CryptoCat DVWA walkthrough series](https://www.youtube.com/watch?v=GmWQ1VIjd2U&list=PLHUKi1UlEgOJLPSFZaFKMoexpM6qhOb4Q)
- [Charalampos Sapnias blog](https://cspanias.github.io/categories/dvwa/)
- AI (Perplexity, ChatGPT, local AI models, etc.)

> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
