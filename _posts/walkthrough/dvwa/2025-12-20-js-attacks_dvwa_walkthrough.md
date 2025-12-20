---
title: DVWA Walkthrough XIV - JavaScript Attacks
date: 2025-12-20 13:00:05 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, javascript]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 14, JavaScript Attacks.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, explore the [comprehensive DVWA walkthrough](/posts/dvwa_walkthrough/) or browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

# **JavaScript Attacks**
## What's this?
JavaScript attacks exploit client-side code flaws like DOM XSS, prototype pollution, or insecure eval() usage to execute malicious scripts in users' browsers. PortSwigger Academy covers vulnerabilities where unsanitized inputs reach dangerous sinks like document.write() or innerHTML, enabling payload execution without server involvement. In DVWA, this simulates unsafe JS handling of user data across security levels, mimicking real-world single-page app risks.

JavaScript attacks lead to session theft, keylogging, or phishing directly in victims' browsers, bypassing server defenses and enabling persistent malware.

## Objective
The main goal of this module is **submit the phrase "success"**.

## **Security: Low**
> **Help**  
> All the JavaScript is included in the page. Read the source and work out what function is being used to generate the token required to match with the phrase and then call the function manually.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/javascript/source/low.php).
{: .prompt-info }

1. When we input success, we get an error message: `Invalid token`. But what is this `token`? We can inspect it using Burp:
![Invalid token](assets/img/posts/dvwa/14_invalid_token.png)

2. By checking the source code, we can see how the `token` is generated. First, the phrase is encoded using ROT13, and then the result is hashed with MD5. Let’s do the same with `success` using [CyberChef](https://gchq.github.io/CyberChef/):
![CyberChef](assets/img/posts/dvwa/14_cyberchef.png)

3. Send the request with the resulting value as the `token`, and we’re done:
![JS Low Done](assets/img/posts/dvwa/14_low.png)

## **Security: Medium**
> **Help**  
> The JavaScript has been broken out into its own file and then minimized. You need to view the source for the included file and then work out what it is doing. Both Firefox and Chrome have a Pretty Print feature which attempts to reverse the compression and display code in a readable way. 
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/javascript/source/medium.php).
{: .prompt-info }

1. If we take a look at how the `token` is generated, it becomes fairly obvious. `XX` is added at the beginning, and then the phrase is reversed and wrapped again with `XX`. Therefore, the `token` should be: `XXsseccusXX`.
2. To better understand the process, we can use the Debugger in the browser’s developer tools and inspect the execution step by step:  
![Set breakpoint](assets/img/posts/dvwa/14_breakpoint.png)
_Set breakpoint and beautify_
![Phrase value](assets/img/posts/dvwa/14_phrase_value.png)
_Change `phrase` value to `success`_
![Steps](assets/img/posts/dvwa/14_steps.png)
_Advance step by step_

3. Send the request with the correct `token` and the challenge is solved:
![JS Medium Done](assets/img/posts/dvwa/14_medium.png)

## **Security: High**
> **Help**  
> The JavaScript has been obfuscated by at least one engine. You are going to need to step through the code to work out what is useful, what is garbage and what is needed to complete the mission. 
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/javascript/source/high.php).
{: .prompt-info }

1. The code is heavily obfuscated and difficult to read. We can use a [deobfuscator](https://thanhle.io.vn/de4js/) to obtain a cleaner version of the script.  
The deobfuscated output is more readable and provides some helpful hints, but we still need to debug it dynamically to fully understand the logic.  
1. We can use Burp to replace the original obfuscated JavaScript with the deobfuscated version. After starting an HTTP server to host our modified file, we configure a replace rule:
![Replace rule](assets/img/posts/dvwa/14_replace.png)
_Replace rule for deobfuscated code_
![Code replaced](assets/img/posts/dvwa/14_code_clean.png)
_Deobfuscated code_
1. We can further tune the proxy and browser settings:
![Proxy settings](assets/img/posts/dvwa/14_settings.png)
_Deobfuscated code_
1. Now we can start debugging. First, set the breakpoints, click submit, and step into the first breakpoint. This is where we need to set the `phrase` parameter:
![Phrase value](assets/img/posts/dvwa/14_phrase_value_2.png)
![Debug](assets/img/posts/dvwa/14_debug_high.png)
1. Once everything is in place, submit `success` and the challenge should be solved:
![JS High Done](assets/img/posts/dvwa/14_high.png)

## References
- [Charalampos Spanias blog](https://cspanias.github.io/posts/DVWA-Javascript/)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
