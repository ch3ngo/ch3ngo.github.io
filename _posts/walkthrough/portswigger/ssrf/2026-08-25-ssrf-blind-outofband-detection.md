---
title: PortSwigger Walkthrough - Blind SSRF with out-of-band detection
date: 2026-08-25 20:00:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, ssrf]
author: diego
description: Walkthrough of PortSwigger's 'Blind SSRF with out-of-band detection' lab.
image:
  path: /assets/img/posts/portswigger/portswigger.png
  alt: PortSwigger Web Security Academy
---

> To browse all labs in this series, visit the [full PortSwigger series](/categories/portswigger/).
{: .prompt-info }

> All testing shown in this series is performed against PortSwigger Academy's intentionally vulnerable labs.  
> Do not apply these techniques to systems you do not own or have explicit written permission to test.
{: .prompt-warning }

## What's this?
Blind SSRF happens when you can make the app issue a request to a URL you control, but the response never comes back to you. You can't read anything, but you can still prove the vulnerability exists (and sometimes chain it further) by watching for the out-of-band interaction on your end. Burp Collaborator is built exactly for this: it hands you a unique domain, and if that domain gets a DNS lookup or an HTTP hit from the target's infrastructure, you know the request went out.

A pretty common source of this kind of blind SSRF is analytics software that logs and then fetches the `Referer` header, on the assumption that it's just tracking incoming links.

## Objective
This site's analytics software fetches whatever URL is in the `Referer` header when a product page loads. Prove SSRF by making it hit the public Burp Collaborator server.

> [PortSwigger's lab link](https://portswigger.net/web-security/ssrf/blind/lab-out-of-band-detection)
{: .prompt-info }

## Walkthrough
Open any product page and send the request to Repeater. Swap the `Referer` header for a Burp Collaborator payload URL:

```
Referer: http://<your-subdomain>.oastify.com
```

Send it, then check the Collaborator client for interactions. Sure enough, an HTTP request rolls in from the lab's server, confirming it fetched our URL server-side.

![Collaborator interaction confirming the out-of-band SSRF](/assets/img/posts/portswigger/ssrf/07-lab-solved.png)
![alt text](image.png)

That interaction is the whole proof. No data comes back in the response, we can't read anything from it directly here, but we've confirmed the server-side request happens, which in a real engagement would be the starting point for internal network probing or chaining into something with actual impact. Lab solved.

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
