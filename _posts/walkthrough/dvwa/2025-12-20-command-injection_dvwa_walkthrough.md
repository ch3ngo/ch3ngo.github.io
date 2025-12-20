---
title: DVWA Walkthrough II - Command Injection
date: 2025-12-20 13:00:17 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, command injection]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 2, Command Injection.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, explore the [comprehensive DVWA walkthrough](/posts/dvwa_walkthrough/) or browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

# **Command Injection**
## What's this?
Command injection vulnerabilities allow attackers to execute arbitrary operating system commands through an application by injecting malicious input into system calls. PortSwigger Academy describes this as occurring when user-supplied data is passed unsanitized to functions like `exec()` or `system()`, enabling command chaining with separators like `;`, `|`, or `&&`.​ In DVWA, the module simulates unsafe execution of commands like ping, vulnerable across security levels.

Command injection grants OS-level access, enabling data theft, file manipulation, privilege escalation, or full server compromise with the app's privileges. It often leads to remote code execution and lateral movement in networks.

## Objective
The main goal of this module is to **find out the user of the web service on the OS, as well as the machine's hostname via RCE**. 
## Notes
Cheatsheet of the operators that can be used and what each does:
- `&`: Runs a command in the background and, as a side effect, it states the end of the first command and the second one.
- `&&`: Executes the second command **only if the first one succeeds**.
- `|`: The output of the first command is the input to the second one.
- `||`: Executes the second command only if the first one fails.
- `;`: Separates two commands and allows them to run like they were on two lines.
- `: Every command inside backticks is evaluated before the external one.

## **Security: Low**
> **Help**  
> This allows for direct input into one of many PHP functions that will execute commands on the OS. It is possible to escape out of the designed command and executed unintentional actions.  
> This can be done by adding on to the request, "once the command has executed successfully, run this command". 
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/exec/source/low.php).
{: .prompt-info }

Any of the operators could work here as there are no restrictions:
![CI Low Done (I)](assets/img/posts/dvwa/2_low_1.png)
![CI Low Done (II)](assets/img/posts/dvwa/2_low_2.png)

## **Security: Medium**
> **Help**  
> The developer has read up on some of the issues with command injection, and placed in various pattern patching to filter the input. However, this isn't enough.  
> Various other system syntaxes can be used to break out of the desired command.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/exec/source/medium.php).
{: .prompt-info }

Some of the operators are blacklisted, but not all of them, we can still do the same thing using a different operator:
```bash
0 | whoami #Get the user of the web service on the OS
0 | hostname #Get the machine's hostname
```

## **Security: High**
> **Help**  
> In the high level, the developer goes back to the drawing board and puts in even more pattern to match. But even this isn't enough.    
> The developer has either made a slight typo with the filters and believes a certain PHP command will save them from this mistake.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/exec/source/high.php).
{: .prompt-info }

Now the blacklist is extensive, almost complete. If we take a close look, `| ` is blacklisted instead of `|`, so we can exploit this misconfiguration:
```bash
0 |whoami #Get the user of the web service on the OS
0 |hostname #Get the machine's hostname
```

![alt text](assets/img/posts/dvwa/2_high.png)

## References
- [PortSwigger Command Injection](https://portswigger.net/web-security/os-command-injection)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
