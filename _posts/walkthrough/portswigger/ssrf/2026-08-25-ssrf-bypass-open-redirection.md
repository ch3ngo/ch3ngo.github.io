---
title: PortSwigger Walkthrough - SSRF with filter bypass via open redirection vulnerability
date: 2026-08-25 20:00:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, ssrf]
author: diego
description: Walkthrough of PortSwigger's 'SSRF with filter bypass via open redirection vulnerability' lab.
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
Sometimes the SSRF-vulnerable parameter is strictly validated (say, it only accepts URLs on the app's own domain) but the app has an unrelated open redirect somewhere else. If the HTTP client making the back-end request follows redirects, you can point the filter at a URL it's happy to allow, let that URL redirect you, and land wherever you actually wanted to go in the first place. The filter never sees the real destination.

## Objective
The stock checker is restricted to only access the local application. Find an open redirect on the app first, then chain it to reach `http://192.168.0.12:8080/admin` and delete `carlos`.

> [PortSwigger's lab link](https://portswigger.net/web-security/ssrf/lab-ssrf-filter-bypass-via-open-redirection)
{: .prompt-info }

## Walkthrough
Check the "Next product" button on any product page. It sends a `GET` request with a `path` parameter, and that request comes back as a redirect. Send it to Repeater and try setting `path` to an arbitrary external URL:

```
GET /product/nextProduct?path=http://192.168.0.12:8080/admin
```

The response `Location` header echoes it straight back:

![Open redirect confirmed via the Location header](/assets/img/posts/portswigger/ssrf/05-open-redirect.png)

Confirmed open redirect. Now chain it into the stock check functionality, which only allows local (same-app) URLs. Instead of pointing `stockApi` at the target directly, point it at the app's own redirect endpoint, with the real target hidden inside the `path` parameter:

```
stockApi=/product/nextProduct?path=http://192.168.0.12:8080/admin
```

The filter sees a local, same-origin path and lets it through. The back-end HTTP client follows the redirect and lands on the internal admin panel anyway. Extend it to delete the user:

```
stockApi=/product/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos
```

Lab solved.

![Lab solved confirmation](/assets/img/posts/portswigger/ssrf/06-lab-solved.png)

The filter did exactly what it was told: check that the URL starts local. It just never accounted for that URL redirecting somewhere else entirely.

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
