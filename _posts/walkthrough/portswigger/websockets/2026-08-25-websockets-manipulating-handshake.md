---
title: PortSwigger Walkthrough - Manipulating the WebSocket handshake to exploit vulnerabilities
date: 2026-08-25 20:00:00 +0200
categories: [Walkthrough, PortSwigger]
tags: [walkthrough, portswigger, bscp, web sockets]
author: diego
description: Walkthrough of PortSwigger's 'Manipulating the WebSocket handshake to exploit vulnerabilities' lab.
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
Some WebSockets vulnerabilities don't live in the messages themselves, they live in the handshake that establishes the connection. Design flaws here tend to involve misplaced trust in HTTP headers (like `X-Forwarded-For`) to make security decisions, or session logic tied to the handshake. Burp Repeater lets you reconnect or clone a WebSocket connection and edit the handshake request before it fires, which is exactly what you need to poke at this kind of bug.

## Objective
Same live chat, this time protected by an aggressive but flawed XSS filter. Trigger an `alert()` popup in the support agent's browser.

> [PortSwigger's lab link](https://portswigger.net/web-security/websockets/lab-manipulating-handshake-to-exploit-vulnerabilities)
{: .prompt-info }

## Walkthrough
Send a chat message first to get a WebSocket handshake logged, then grab it from the WebSockets history and send it to Repeater. Try a basic XSS payload:

```html
<img src=1 onerror='alert(1)'>
```

The connection dies immediately:

![The connection gets terminated after the XSS attempt](/assets/img/posts/portswigger/websockets/04-attack-blocked.png)

Try reconnecting and you get an error: `This address is blacklisted`. So the filter isn't just inspecting messages, it flagged our IP and blocked reconnections from it. Click Reconnect anyway, but before sending, add an `X-Forwarded-For` header with an arbitrary IP that isn't ours:

```
X-Forwarded-For: 1.1.1.1
```

The handshake succeeds, the app trusts that header over the actual source IP for its blacklist check, and we're back in the chat. Now, since a plain `onerror` payload presumably got flagged by whatever's inspecting message content, send an obfuscated version instead, mixed case, backtick-style event call:

```html
<img src=1 oNeRrOr=alert`1`>
```

That one slides past the filter and pops.

![Lab solved confirmation](/assets/img/posts/portswigger/websockets/05-lab-solved.png)

Two separate flaws stacked: trusting a spoofable header for IP-based blocking, and a content filter that's pattern-matching specific casing/syntax instead of actually understanding what it's looking at.

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
