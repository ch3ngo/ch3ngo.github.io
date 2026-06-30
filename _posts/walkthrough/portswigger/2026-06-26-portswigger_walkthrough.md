---
title: PortSwigger Academy Labs Walkthrough
date: 2026-06-28 14:00:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp]
author: diego
description: A walkthrough series of PortSwigger Web Security Academy labs, covering the topics included in the Burp Suite Certified Practitioner (BSCP) exam.
image:
  path: /assets/img/posts/portswigger/portswigger.png
  alt: PortSwigger Web Security Academy
---

> To browse all labs in this series, visit the [full PortSwigger series](/categories/dvwa/).
{: .prompt-info }

> All testing shown in this series is performed against PortSwigger Academy's intentionally vulnerable labs.  
> Do not apply these techniques to systems you do not own or have explicit written permission to test.
{: .prompt-warning }

## **Introduction**
### What's this?
PortSwigger Web Security Academy is a free online training platform created by the makers of Burp Suite. It covers a wide range of web application security topics through guided reading material and hands-on labs that simulate real-world vulnerabilities. Each topic includes a theory section explaining how the vulnerability works, its root causes, and its potential impact, followed by one or more labs that require you to exploit it.

This series documents my walkthrough of the Web Security Academy labs as I prepare for the **Burp Suite Certified Practitioner (BSCP)** certification. The BSCP is an exam that tests your ability to find and exploit web vulnerabilities in a realistic time-constrained environment using Burp Suite. It covers a broad set of vulnerability classes and requires both solid methodology and good tool fluency.

I am focusing on the labs included in the official BSCP learning paths, skipping expert-level labs that are out of scope for the certification for now. The coverage will grow as I work through more topics.

### Who is this for?

This walkthrough is aimed at:
- Students and professionals preparing for the BSCP exam
- Pentesters and red teamers revisiting web application fundamentals
- Anyone learning web security through hands-on labs

Familiarity with HTTP, how web applications work, and basic Burp Suite usage is assumed. If you are completely new to Burp, the setup section below will get you started.

### How to use this series?
- Each vulnerability category is documented in its own post
- Within each post, labs are documented individually with objective, approach, and key takeaways
- Labs can be followed in order or referenced independently
- Screenshots and payloads are included for clarity
- Small differences may appear between lab runs as PortSwigger randomizes some lab values

---

## **Burp Suite Setup**
The labs run in your browser, intercepted through Burp Suite. If you have not set it up yet, here is the quick version:

1. Download Burp Suite from [PortSwigger](https://portswigger.net/burp/downloads). The Community edition works for most labs but for the actual BSCP exam you need Professional.
2. Open Burp and confirm the proxy listener is active under `Proxy > Proxy Settings`. The default is `127.0.0.1:8080`.
![Burp Proxy](/assets/img/posts/portswigger/00_proxy_setup.png)
3. Install the **FoxyProxy** browser extension. Add a new proxy entry:
   - **Name**: `burp`
   - **Type**: `HTTP`
   - **Hostname**: `127.0.0.1`
   - **Port**: `8080`
![FoxyProxy](/assets/img/posts/portswigger/00_foxyproxy.png)
4. With the proxy enabled in FoxyProxy, navigate to `https://burp` in your browser. Download the Burp CA certificate and install it in your browser's certificate store (or Windows certificate manager).
5. That's it. Your browser traffic will now route through Burp and appear in the Proxy history.

> Make sure to set the proxy filter to show all requests, including image requests (`Proxy > HTTP history > Filter`). Some labs, like path traversal, inject the vulnerability through image-loading endpoints that are hidden by default filters.
{: .prompt-tip }
---
## **Labs Walkthrough**

> This is a Work In Progress, if you are looking for something that should be here it should be in process. You can always ping me and ask!
{: .prompt-info }

### Authentication Vulnerabilities
Username enumeration, brute-force protection bypasses, 2FA flaws, password reset logic errors, and stay-logged-in cookie abuse.

1. [Username enumeration via different responses](/posts/username-enum-different-responses)
2. [Username enumeration via subtly different responses](/posts/username-enum-subtly-different-responses)
3. [Username enumeration via response timing](/posts/username-enum-response-timing)
4. [Broken brute-force protection, IP block](/posts/broken-brute-force-ip-block)
5. [Username enumeration via account lockout](/posts/username-enum-account-lockout)
6. [2FA simple bypass](/posts/2fa-simple-bypass)
7. [2FA broken logic](/posts/2fa-broken-logic)
8. [Brute-forcing a stay-logged-in cookie](/posts/brute-forcing-stay-logged-in-cookie)
9. [Offline password cracking](/posts/offline-password-cracking)
10. [Password reset broken logic](/posts/password-reset-broken-logic)
11. [Password reset poisoning via middleware](/posts/password-reset-poisoning-via-middleware)
12. [Password brute-force via password change](/posts/password-brute-force-via-password-change)

### Path Traversal
Reading arbitrary files on the server by manipulating file path parameters, and bypassing common defenses like sequence stripping and extension validation.

1. [File path traversal, simple case](/posts/path-traversal-simple-case)
2. [File path traversal, sequences blocked with absolute path bypass](/posts/path-traversal-sequences-blocked-absolute-path-bypass)
3. [File path traversal, sequences stripped non-recursively](/posts/
4. path-traversal-sequences-stripped-non-recursively)
5. [File path traversal, sequences stripped with superfluous URL-decode](/posts/path-traversal-sequences-stripped-superfluous-url)
6. [File path traversal, validation of start of path](/posts/path-traversal-validation-start-path)
7. [File path traversal, validation of file extension with null byte bypass](/posts/path-traversal-validation-file-extension-null-byte-bypass)

### Server-Side Request Forgery (SSRF)
Forcing the server to make requests to internal or external systems, bypassing access controls and exposing internal infrastructure.

> Coming soon.
{: .prompt-info }

### Cross-Origin Resource Sharing (CORS)
Misconfigured CORS policies that allow untrusted origins to read sensitive cross-domain responses.

> Coming soon.
{: .prompt-info }

---

## References
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [BSCP exam information](https://portswigger.net/web-security/certification)
- [Burp Suite documentation](https://portswigger.net/burp/documentation)
- AI (Claude, Perplexity, local models)

> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
