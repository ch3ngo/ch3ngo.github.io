---
title: DVWA Walkthrough XVI - Open HTTP Redirect
date: 2025-12-20 13:00:03 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, http, open redirect]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 16, Open HTTP Redirect.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, explore the [comprehensive DVWA walkthrough](/posts/dvwa_walkthrough/) or browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

# **Open HTTP Redirect**
## What's this?
Open HTTP redirect vulnerabilities allow attackers to manipulate redirect parameters, forcing users to arbitrary external sites without validation of the destination URL. PortSwigger Academy describes this as untrusted input directly controlling Location headers or JavaScript redirects, commonly exploited in phishing by mimicking trusted domains. In DVWA, it simulates unsafe redirect handling across security levels.

Open redirects enable phishing, OAuth token theft, and SSRF chaining, tricking users into credential submission or internal network access.

## Objective
The main goal of this module is to **abuse the redirect page to move the user off the DVWA site or onto a different page on the site than expected**.

## **Security: Low**
> **Help**  
> The redirect page has no limitations, you can redirect to anywhere you want.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/open_redirect/source/low.php).
{: .prompt-info }

If we navigate through the application and inspect the flow using Burp, we can see a request that includes a `redirect` parameter.
![Redirect parameter](assets/img/posts/dvwa/16_low_redirect_1.png)
We can send this request to Repeater and modify the parameter to any URL we want, redirecting the user to an arbitrary page:
![OR Low Done](assets/img/posts/dvwa/16_low.png)

## **Security: Medium**
> **Help**  
> The code prevents you from using absolute URLs to take the user off the site, so you can either use relative URLs to take them to other pages on the same site or a Protocol-relative URL.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/open_redirect/source/medium.php).
{: .prompt-info }

1. If we try the same approach, we get an error indicating that **absolute URLs are not allowed**:
![Open Redirect Error](assets/img/posts/dvwa/16_or_error.png)
2. To bypass this restriction, we can simply remove the `https:` scheme. The redirect is then successfully performed:
![OR Medium Done](assets/img/posts/dvwa/16_medium.png)

## **Security: High**
> **Help**  
> The redirect page tries to lock you to only redirect to the info.php page, but does this by checking that the URL contains "info.php".
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/open_redirect/source/high.php).
{: .prompt-info }

At this level, redirects are supposedly limited to the `info.php` page. However, since the server only checks whether the URL contains the string `info.php`, we can redirect to any URL that includes it.

By adding a dummy GET parameter containing `info.php` to the target URL, the redirect is accepted:

![OR High Done](assets/img/posts/dvwa/16_high.png)
![Redirect](assets/img/posts/dvwa/16_high_2.png)

## References
- [StackHawk Open Redirect](https://www.stackhawk.com/blog/what-is-open-redirect/)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
