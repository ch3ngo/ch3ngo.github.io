---
title: DVWA Walkthrough XVIII - API Security
date: 2025-12-20 13:00:01 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, api]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 18, API Security.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---
> To further your understanding of DVWA, explore the [comprehensive DVWA walkthrough](/posts/dvwa_walkthrough/) or browse the [full DVWA series](/categories/dvwa/) to master every vulnerability level.
{: .prompt-info }

# **API Security**
## What's this?
API security focuses on protecting application programming interfaces from attacks that exploit how data and functionality are exposed over HTTP (typically REST, JSON, or GraphQL). PortSwigger defines API testing as identifying hidden or lightly documented endpoints, understanding their behavior, and probing them for common web issues like access control flaws, injection, and business logic vulnerabilities. Poorly secured APIs can expand the attack surface significantly, especially when they expose backend functionality that the front-end does not directly show to users.

## Objective
Each level has its own objective but the main goal of this module is to **exploit weak API implementations**.

## **Security: Low**
> **Help**  
> The call being made by the JavaScript is for version 2 of the endpoint, could there be other, earlier, versions available?
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/api/source/low.php).
{: .prompt-info }

When we access the module and take a look at the HTTP history in Burp, we can see an API call being made to the `/vulnerabilities/api/v2/user/` endpoint. The first thing to check is whether there’s an earlier version available:
![API Low Done](assets/img/posts/dvwa/18_low.png)
And there it is. The `v1` endpoint happily leaks the users’ password hashes as well.

## **Security: Medium**
> **Help**  
> Look at the call made by the site, but also look at the swagger docs and see if there are any other parameters you might be able to add that are not currently passed. 
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/api/source/medium.php).
{: .prompt-info }

If we look at the API call used to change our name, we can see that it’s a `PUT` request where we specify the new value:
![PUT request](assets/img/posts/dvwa/18_put.png)
The [`PUT` request method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/PUT) creates a new resource or replaces a representation of the target resource with the request content. The difference between PUT and POST is that PUT is idempotent: calling it once is no different from calling it several times successively (there are no side effects).
With that in mind, let’s try replacing our user level (1) with the admin level (0):
![Change level](assets/img/posts/dvwa/18_level.png)
Doing it directly like this doesn’t work, so let’s try the same thing again, but this time by intercepting the request and modifying the parameters on the fly:
![API Medium Done](assets/img/posts/dvwa/18_medium.png)

## **Security: High**
> **Help**  
> Import the four health calls into your testing tool of choice and make sure they are running properly. When they are all working, test them for vulnerabilities.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/api/source/high.php).
{: .prompt-info }

First, we can download the `openapi.yml` document and inspect it to understand the available endpoints. You’ll want to take note of the different health-related calls. I’m using **Burp with the OpenAPI Parser extension**, but you can also use Postman, ZAP, or any other tool you’re comfortable with. For convenience, download the file, **update the base URL to match your setup**, and then load it into your tool.
When sending requests to Repeater, remember to **change the protocol to `HTTP/1.1` and set the body encoding to JSON**:
![OpenAPI Parser](assets/img/posts/dvwa/18_openapi.png)  
![Changes to request](assets/img/posts/dvwa/18_changes.png)
The endpoints we're interested in are:

- `/vulnerabilities/api/v2/health/echo`
- `/vulnerabilities/api/v2/health/connectivity`
- `/vulnerabilities/api/v2/health/status`
- `/vulnerabilities/api/v2/health/ping`  

Now we can start analyzing each endpoint to see if anything stands out. The most interesting one is `/vulnerabilities/api/v2/health/connectivity`, since it takes an address as a parameter. Let’s try changing the target host:
![Target changed](assets/img/posts/dvwa/18_target.png)

It looks like the endpoint is actually pinging the host we specify. That’s a good sign, so let’s see if we can push it further and try command injection:
![Command injection](assets/img/posts/dvwa/18_ci.png)
_cURL executed_
![API High Done](assets/img/posts/dvwa/18_high.png)
_whoami executed_

All done!

## References
- [PortSwigger API Testing](https://portswigger.net/web-security/api-testing)
- [PUT Request Method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/PUT)
- [Cryptocat walkthrough video](https://www.youtube.com/watch?v=c_6RaCekH40)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
