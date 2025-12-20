---
title: DVWA Walkthrough IX - Weak Session IDs
date: 2025-12-20 13:00:10 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, session, id]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 9, Weak Session IDs.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, explore the [comprehensive DVWA walkthrough](/posts/dvwa_walkthrough/) or browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

# **Weak Session IDs**
## What's this?
Weak session ID vulnerabilities occur when session tokens lack sufficient randomness, length, or entropy, making them predictable or guessable by attackers. PortSwigger Academy explains that flawed generation (e.g., sequential numbers, timestamps, or weak hashes like MD5) enables session hijacking via prediction or brute force. In DVWA, this module demonstrates predictable session cookies across security levels, from incrementing counters to time-based values.

Weak sessions enable unauthorized account access, data theft, or privilege escalation without credentials, compromising user sessions entirely.

## Objective
The main goal of this module is to **work out how the ID is generated**.

## **Security: Low**
> **Help**  
> The cookie value should be very obviously predictable.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/weak_id/source/low.php).
{: .prompt-info }

Clicking `Generate` sets a new session cookie each time. If we take a look at Burp’s HTTP history, we can see these cookies and verify that they are simply sequential numbers:
![SES Low Done](assets/img/posts/dvwa/9_low.png)

## **Security: Medium**
> **Help**  
> The value looks a little more random than on low but if you collect a few you should start to see a pattern.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/weak_id/source/medium.php).
{: .prompt-info }

At this security level, the session cookie looks slightly more elaborate, but also familiar. If we click `Generate` several times, we can see that the cookie value keeps incrementing. This is because it is based on the Unix epoch timestamp.

If we inspect the cookie value, we can correlate it with the exact time the cookie was generated, which matches the response timestamp:

![Cookie values](assets/img/posts/dvwa/9_medium.png)
![Timestamp](assets/img/posts/dvwa/9_timestamp.png)

## **Security: High**
> **Help**  
> First work out what format the value is in and then try to work out what is being used as the input to generate the values.  
> Extra flags are also being added to the cookie, this does not affect the challenge but highlights extra protections that can be added to protect the cookies.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/weak_id/source/high.php).
{: .prompt-info }

Now the cookie looks even more complex:
![Cookie value](assets/img/posts/dvwa/9_cookie.png)  

However, we can verify that it is an MD5 hash:  
![MD5 hash](assets/img/posts/dvwa/9_md5.png)  
Using Burp’s Sequencer, we can collect multiple cookie values for further analysis:
![Cookies list](assets/img/posts/dvwa/9_cookie_list.png)  
Finally, using John the Ripper, we can crack the hashes and confirm that the original value is still just a sequential number, this time MD5-hashed:
![Sequential](assets/img/posts/dvwa/9_high.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
