---
title: PortSwigger Walkthrough - Basic SSRF against another back-end system
date: 2026-08-25 20:00:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, ssrf]
author: diego
description: Walkthrough of PortSwigger's 'Basic SSRF against another back-end system' lab.
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
Not every SSRF target is the server itself. Sometimes the app can reach other back-end systems that live on a private IP range and aren't directly reachable by regular users. These internal systems tend to have a much weaker security posture, because whoever built them assumed the network topology alone would protect them (spoiler: it doesn't, if the front-end app can be tricked into proxying requests for you).

If you know (or can guess) the internal subnet, you can use the vulnerable parameter as a free port/host scanner and go hunting for whatever's sitting there.

## Objective
Same stock check feature as before. This time, use it to scan the internal `192.168.0.X` range for an admin interface listening on port `8080`, then delete the user `carlos`.

> [PortSwigger's lab link](https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-backend-system)
{: .prompt-info }

## Walkthrough
Same "Check stock" `POST` request as the previous lab, but this time send it to Intruder instead of Repeater. We know the target subnet and port, so set the payload position on the last octet:

```
stockApi=http%3A%2F%2F192.168.0.§1§%3A8080%2Fadmin
```

Use a numeric payload from 1 to 255 and launch the attack. Most responses come back empty or with an error, but one host, `192.168.0.157`, returns a `200 OK`.

![Intruder scan hitting a live admin host](/assets/img/posts/portswigger/ssrf/02-network-scan.png)

Send the request to Repeater and point `stockApi` straight at that host:

```
stockApi=http://192.168.0.157:8080/admin
```

The admin panel loads. Same as before, appending the delete path finishes the job:

```
stockApi=http://192.168.0.157:8080/admin/delete?username=carlos
```

Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/ssrf/03-lab-solved.png)

Nice, easy internal port/service sweep using the vulnerable app as our proxy.

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
