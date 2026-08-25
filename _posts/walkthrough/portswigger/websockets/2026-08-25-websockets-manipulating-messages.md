---
title: PortSwigger Walkthrough - Manipulating WebSocket messages to exploit vulnerabilities
date: 2026-08-25 20:00:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, web sockets]
author: diego
description: Walkthrough of PortSwigger's 'Manipulating WebSocket messages to exploit vulnerabilities' lab.
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
WebSockets are long-lived, bidirectional connections initiated over HTTP, and basically every classic web vulnerability can show up in them too. If the app takes whatever comes through a WebSocket message and reflects it somewhere without sanitizing it, that's the same old injection problem, just riding a different transport. Burp Proxy has a dedicated WebSockets history tab, and you can intercept, modify, and replay messages from there the same way you'd work with regular HTTP requests.

## Objective
This shop has a live chat feature built on WebSockets. Messages you send are viewed in real time by a support agent. Trigger an `alert()` popup in the support agent's browser.

> [PortSwigger's lab link](https://portswigger.net/web-security/websockets/lab-manipulating-messages-to-exploit-vulnerabilities)
{: .prompt-info }

## Walkthrough
Open the live chat and try a basic payload straight away: `<script>alert(1)</script>`. Checking the WebSocket message in Burp's history shows the "dangerous" characters get HTML-encoded before being sent:

![Payload characters getting HTML-encoded](/assets/img/posts/portswigger/websockets/01-encoded-chars.png)

So `<script>` tags specifically are neutralized somewhere in the pipeline. Turn on proxy intercept, send a new chat message, and edit it in flight before it goes out, swapping in an event-handler-based payload instead of a `<script>` tag:

```html
<img src=1 onerror='alert(1)'>
```

Forward the modified message. The support agent's side renders it, the `onerror` handler fires, and the popup goes off.

![XSS](/assets/img/posts/portswigger/websockets/02-xss.png)
![Lab solved confirmation](/assets/img/posts/portswigger/websockets/03-lab-solved.png)

Same story as any reflected/stored XSS: whatever encoding was in place was targeting specific tags, not the underlying problem of untrusted input ending up in the DOM unescaped.

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
