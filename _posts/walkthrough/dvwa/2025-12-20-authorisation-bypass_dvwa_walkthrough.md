---
title: DVWA Walkthrough XV - Authorisation Bypass
date: 2025-12-20 13:00:04 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, auth, bypass]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 15, Authorisation Bypass.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---

# **Authorisation Bypass**
## What's this?
Authorization bypass vulnerabilities occur when an application fails to properly enforce access controls, allowing users to access resources or perform actions beyond their privileges. PortSwigger Academy describes this as flawed authorization mechanisms where attackers manipulate parameters like user IDs or roles to view others' data or escalate privileges. In DVWA, this simulates insecure direct object references (IDOR) or missing permission checks across security levels.
Authorization bypass enables data theft, privilege escalation, and full system compromise, often leading to breaches of sensitive information or unauthorized modifications.

## Objective
The main goal of this module is to **test the user management system at all security levels to identify any areas where authorisation checks have been missed**. Basically, take a note of the calls made as an admin and try to replicate them logged in as a different user.
## Configuration / Notes
For a second user, use gordonb:abc123

## **Security: Low**
> **Help**  
> Non-admin users do not have the 'Authorisation Bypass' menu option.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/authbypass/source/low.php).
{: .prompt-info }

1. The module exposes two endpoints under `/authbypass`: `/get_user_data.php` and `change_user_details.php`.
2. Even if we log in as a non-admin user, we can manually browse to the module path and access it directly:
![AB Low Done](assets/img/posts/dvwa/15_low.png)

## **Security: Medium**
> **Help**  
> The developer has locked down access to the HTML for the page, but have a look how the page is populated when logged in as the admin.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/authbypass/source/medium.php).
{: .prompt-info }

1. After modifying the security level, we can now check if we have access to the page but no luck. We get an `Unauthorised` message.
2. Instead, we can try accessing the endpoints used to populate the page and update the user information directly. These endpoints aren’t properly secured, so we still have access to them:
![AB Medium Done (I)](assets/img/posts/dvwa/15_m_get.png)
![AB Medium Done (II)](assets/img/posts/dvwa/15_m_post.png)

## **Security: High**
> **Help**  
> Both the HTML page and the API to retrieve data have been locked down, but what about updating data? You have to make sure you test every call to the site.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/authbypass/source/high.php).
{: .prompt-info }

This level is very similar to the previous one. The main difference is that the user is no longer authorised to access `get_user_data.php`, but it’s still possible to call `change_user_details.php` and update the data anyway.

## References
- [PortSwigger Access control vulnerabilities and privilege escalation](https://portswigger.net/web-security/access-control)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
