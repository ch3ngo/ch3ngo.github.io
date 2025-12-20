---
title: DVWA Walkthrough VI - Insecure CAPTCHA
date: 2025-12-20 13:00:13 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, captcha]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 6, Insecure CAPTCHA.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, explore the [comprehensive DVWA walkthrough](/posts/dvwa_walkthrough/) or browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

# **Insecure CAPTCHA**
## What's this?
Insecure CAPTCHA vulnerabilities arise when CAPTCHA implementations lack proper server-side validation or use weak client-side checks, allowing attackers to bypass automated detection via manipulation or automation tools. PortSwigger notes that flawed CAPTCHAs fail to prevent bots from brute-forcing logins or spam forms, often relying solely on JavaScript verification. In DVWA, this module demonstrates CAPTCHA evasion through request tampering across security levels.

Bypassed CAPTCHAs enable automated attacks like brute force logins or spam floods, leading to account takeovers or denial-of-service.

## Objective
The main goal of this module is to **change the user's password because of the poor CAPTCHA system**.
## Configuration
To configure the **reCAPTCHA** you will need to get your keys:
1. Visit the link: [https://www.google.com/recaptcha/admin/create](https://www.google.com/recaptcha/admin/create)
2. Label: `dvwa`
3. reCAPTCHA type: `Challenge (v2)` &rarr; `"I'm not a robot" Checkbox`
4. Domains: `192.168.1.131` (or 127.0.0.1 or the IP/domain you are using)
5. Submit

Now, you will need to edit DVWA's configuration file `config.inc.php`. If, like me, you have your DVWA in Docker, you can copy the file, edit it locally and copy it back into Docker:
1. `docker cp <container_name>:/var/www/html/config/config.inc.php ./config.inc.php`
2. Add the site key to config's `RECAPTCHA_PUBLIC_KEY`.
3. Add the secret key to config's `RECAPTCHA_PRIVATE_KEY`.
4. `docker cp ./config.inc.php <container_name>:/var/www/html/config/config.inc.php`

Now you are set to start the module!

## **Security: Low**
> **Help**  
> The issue with this CAPTCHA is that it is easily bypassed. The developer has made the assumption that all users will progress through screen 1, complete the CAPTCHA, and then move on to the next screen where the password is actually updated. By submitting the new password directly to the change page, the user may bypass the CAPTCHA system.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/captcha/source/low.php).
{: .prompt-info }

1. Start by checking how the application works. First, we pass the CAPTCHA challenge, and then we change the password in a two-step process that is not well protected.
![Step 1](assets/img/posts/dvwa/6_1.png)
![Step 2](assets/img/posts/dvwa/6_2.png)
2. If we jump straight to step 2, we can change the password without having to solve the CAPTCHA.

![CAPTCHA Low Done](assets/img/posts/dvwa/6_low.png)

## **Security: Medium**
> **Help**  
> The developer has attempted to place state around the session and keep track of whether the user successfully completed the CAPTCHA prior to submitting data. Because the state variable is on the client side, it can also be manipulated by the attacker.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/captcha/source/medium.php).
{: .prompt-info }

The medium security level behaves almost the same as the low level. The only difference is the addition of an extra parameter, `passed_captcha`, used to verify whether the challenge was completed. Since this parameter is client-controlled, we can tamper with it and change the password while skipping the CAPTCHA:
![CAPTCHA Medium Done](assets/img/posts/dvwa/6_medium.png)

## **Security: High**
> **Help**  
> There has been development code left in, which was never removed in production. It is possible to mimic the development values, to allow invalid values in be placed into the CAPTCHA field.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/captcha/source/high.php).
{: .prompt-info }

The high security level is more challenging. The CAPTCHA response is included directly in the request, but the vulnerability lies in a developer note left in the HTML source:
```html
<!-- **DEV NOTE** Response: 'hidd3n_valu3 && User-Agent: 'reCAPTCHA' **/DEV NOTE** -->
```
![DEV NOTE](assets/img/posts/dvwa/6_note.png)

By changing the request values to those mentioned in the developer note, we can bypass the CAPTCHA and change the password:
- `g-recaptcha-response=hidd3n_valu3`
- `User-Agent: reCAPTCHA`
![CAPTCHA High Done](assets/img/posts/dvwa/6_high.png)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
