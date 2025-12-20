---
title: DVWA Walkthrough XVII - Cryptography
date: 2025-12-20 13:00:02 +0200
categories: [Walkthrough, DVWA]
tags: [walkthrough, dvwa, burpsuite, cryptography]
author: diego
description: A walkthrough of the Damn Vulnerable Web Application (DVWA) module 17, Cryptography.
image:
  path: /assets/img/posts/dvwa/dvwa.png
  alt: DVWA
---

# **Cryptography**
## What's this?
Cryptography vulnerabilities in web applications arise from weak or improperly implemented encryption, hashing, or key management, exposing sensitive data like passwords or sessions. PortSwigger Academy covers issues such as using outdated algorithms (e.g., MD5, DES), missing salts, or predictable randomness, which attackers exploit to decrypt or forge data. In DVWA, this module demonstrates flawed crypto practices like weak RC4 or ECB mode, vulnerable across security levels.

Poor cryptography leads to data breaches, credential theft, and session hijacking, enabling attackers to impersonate users or access confidential information. It results in regulatory fines and loss of trust when sensitive data is exposed in transit or at rest.

## Objective
Each level has its own objective but the main goal of this module is to **exploit weak cryptographic implementations**.

## **Security: Low**
> **Help**  
> The thing to notice is the mention of encoding rather than encryption, that should give you a hint about the vulnerability here.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/cryptography/source/low.php).
{: .prompt-info }

This is pretty straightforward: the message is encoded, not encrypted. Copy the intercepted message, decode it using the same page's functionality, and you'll get the password:
![Crypto Low Done](assets/img/posts/dvwa/17_low.png)

## **Security: Medium**
> **Help**  
> The tokens are encrypted using an Electronic Code Book based algorithm (AES-128-ECB). In this mode, the clear text is broken down into fixed sized blocks and each block is encrypted independently of the rest. This results in a cipher text that is made up from a number of individual blocks with no way to tie them together. Worse than this, any two blocks, from any two clear text inputs, are interchangeable as long as they have been encrypted with the same key. In our example, this means you can take blocks from the three different tokens to make your own token.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/cryptography/source/medium.php).
{: .prompt-info }

1. The application uses `AES-128-ECB`, so the ciphertext is divided into fixed blocks encrypted independently. The token splits into four parameters, each encrypted separately then concatenated: 32 hex chars (or 24 base64) per parameter.
2. We can divide the three ciphertexts:
   1. Sooty (admin), session expired:
```
e287af752ed3f9601befd45726785bd9 <- user
b85bb230876912bf3c66e50758b222d0 <- expiry
837d1e6b16bfae07b776feb7afe57630 <- level
5aec34b41499579d3fb6acc8dc92fd5f <- bio
cea8743c3b2904de83944d6b19733cdb
48dd16048ed89967c250ab7f00629dba
```
   2. Sweep (user), session expired:
```
3061837c4f9debaf19d4539bfa0074c1 <- user
b85bb230876912bf3c66e50758b222d0 <- expiry
83f2d277d9e5fb9a951e74bee57c77a3 <- level
caeb574f10f349ed839fbfd223903368 <- bio
873580b2e3e494ace1e9e8035f0e7e07
``` 
   3. Soo (user), session valid:
```
5fec0b1c993f46c8bad8a5c8d9bb9698 <- user
174d4b2659239bbc50646e14a70becef <- expiry
83f2d277d9e5fb9a951e74bee57c77a3 <- level
c9acb1f268c06c5e760a9d728e081fab <- bio
65e83b9f97e65cb7c7c4b8427bd44abc
16daa00fd8cd0105c97449185be77ef5
``` 
1. Now we can mix and build our token. For this recipe we need: `Sweep` user + `Soo` expiry + `Sooty` level + `Sweep` bio:
```
3061837c4f9debaf19d4539bfa0074c1174d4b2659239bbc50646e14a70becef837d1e6b16bfae07b776feb7afe57630caeb574f10f349ed839fbfd223903368873580b2e3e494ace1e9e8035f0e7e07
```

![Crypto Medium Done](assets/img/posts/dvwa/17_medium.png)
_Done!_

## **Security: High**
> **Help**  
> The system is using AES-128-CBC which means it is vulnerable to a padding oracle attack.
{: .prompt-tip }

> Check the source code [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/cryptography/source/high.php).
{: .prompt-info }

**I highly recommend going through Cryptocat's walkthrough video as well as Cryptopals' post for a deeper understanding of the attack (see [References](#references)).**

1. Base64-decode the `iv` (reveals `1234567812345678`) from the captured token.
2. We can use the DVWA `oracle_attack.php` script. You can found in the help section or [here](https://github.com/digininja/DVWA/blob/master/vulnerabilities/cryptography/source/oracle_attack.php). We will also need [token_library_high.php](https://github.com/digininja/DVWA/blob/master/vulnerabilities/cryptography/source/token_library_high.php).  
```bash
php oracle_attack.php --iv="MTIzNDU2NzgxMjM0NTY3OA==" --token="PhQwGVA3q+T2mT+L3Pe5Vg==" --url="http://192.168.1.131:4280/vulnerabilities/cryptography/source/check_token_high.php"
```
1. After executing the script, we get a new token as admin Geoffrey:  
```
Response from server:
array(3) {
  ["status"]=>
  int(200)
  ["user"]=>
  string(8) "Geoffery"
  ["level"]=>
  string(5) "admin"
}
Hack success!
The new token is:
{"token":"PhQwGVA3q+T2mT+L3Pe5Vg==","iv":"MTIzNDU2NzsxMjM0NTY3OA=="}
```
The `iv` is flipped to `1234567;12345678`.
![Crypto High Done](assets/img/posts/dvwa/17_high.png)
_Admin access obtained!_
4. We can play with the modified character and obtain other users in the application:
![More users](assets/img/posts/dvwa/17_users.png)
_User George_

## References
- [How many numbers in the encryption key](https://support.google.com/messages/thread/325074166/how-many-numbers-in-the-encryption-key)
- [Cryptocat's walkthrough video](https://www.youtube.com/watch?v=7WySPRERN0Q)
- [Cryptopals: Exploiting CBC Padding Oracles](https://www.nccgroup.com/research-blog/cryptopals-exploiting-cbc-padding-oracles/)

<br><br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
