---
title: DVWA Walkthrough XII - Stored Cross Site Scripting (XSS)
date: 2025-12-20 13:00:07 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, xss, stored]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 12, Stored Cross Site Scripting (XSS).
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, explore the [comprehensive DVWA walkthrough](/posts/dvwa_walkthrough/) or browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

# **Stored Cross Site Scripting (XSS)**
## What's this?
Stored XSS occurs when malicious scripts are injected into persistent storage like databases via user inputs (e.g., comments, profiles) and served to all subsequent visitors, executing in their browsers. PortSwigger Academy describes this persistent variant as more dangerous than reflected XSS since it affects multiple users automatically without needing phishing links. In DVWA, it simulates unsafe storage and output of inputs across security levels.

Stored XSS compromises all site visitors, enabling mass session theft, data exfiltration, or worm-like propagation until remediation.

## Objective
The main goal of this module is to **redirect everyone to a web page of your choosing**.
## Notes
Anytime you need to clear the guestbook, access the module with security level impossible and clear it.

## **Security: Low**
> **Help**  
> Low level will not check the requested input, before including it to be used in the output text.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/xss_s/source/low.php).
{: .prompt-info }

At this security level, we have a guestbook where we can leave messages without any security measures in place. We can simply post a comment containing a payload, and every time a user visits the page, the JavaScript code will execute:
```html
<script>window.location='https://ch3ngo.github.io'</script>
```
When we try to submit the payload, it gets cut off halfway. This happens because the input field has a length restriction. We can bypass it by modifying the HTML on the fly using the browser’s developer tools:
![Modify maxlength](assets/img/posts/dvwa/12_modify.png)

After removing the restriction, we can submit the payload successfully. Any user who visits the guestbook page will now be automatically redirected to the attacker-controlled site.

## **Security: Medium**
> **Help**  
> The developer had added some protection, however hasn't done every field the same way.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/xss_s/source/medium.php).
{: .prompt-info }

In this case, the message field is properly protected, so we can’t exploit it directly. However, the name field is still vulnerable. We can abuse it by using an `<svg>` tag to execute JavaScript code. As before, we need to remove the maxlength parameter from the HTML:
```html
<svg/onload=window-location='https://ch3ngo.github.io'>
```
![SXSS Medium Done](assets/img/posts/dvwa/12_medium.png)

## **Security: High**
> **Help**  
> The developer believe they have disabled all script usage by removing the pattern "<s\*c\*r\*i\*p\*t".
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/xss_s/source/high.php).
{: .prompt-info }

This security level can be exploited in the same way as the medium one. The code was only modified to apply stricter `<script>` tag filtering, but it still fails to prevent alternative Cross-Site Scripting techniques.

## References
- [PortSwigger Stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
