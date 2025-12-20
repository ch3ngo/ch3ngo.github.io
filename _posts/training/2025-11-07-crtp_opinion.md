---
title: Certified Red Team Professional (CRTP) review
date: 2025-11-07 18:30:00 +0200
categories: [Certification, Active Directory]
tags: [training, opinion, active directory]
author: diego
description: How I got CRTP certified. Information, my experience and some (I hope) useful tips.
image:
  path: /assets/img/posts/crtp/crtp.png
  alt: CRTP Certificate
---

If you are thinking about taking the [Certified Red Team Professional (CRTP) from Altered Security](https://www.alteredsecurity.com/adlab) challenge, this can interest you.

![CRTP](assets/img/posts/crtp/crtp.png)
_CRTP certificate_

## **Background**
By day, I work as an Ethical Hacker, a role I've held for approximately **2 years and a half**. In my spare time, however, I am not a caped crusader like Batman, but rather an enthusiastic learner. Previously working in infosec prior to earning the Certified Red Team Professional (CRTP) certification provided me a solid foundation. I saw this as a challenge and very good opportunity to expand my knowledge and skills within Active Directory environments.

Before CRTP, I had already obtained some certifications such as **Certified Ethical Hacker (CEH Master)** from EC Council, **Certified Red Team Analyst (CRTA)** from CyberWarfare Labs, or **eLearn Junior Penetration Tester v2**. Although my background included some related experience, Active Directory was not necessarily the most covered topic.

## **Course review**
### Introduction
From the course syllabus and as a summary, CRTP preparation will teach you:
1. **Active Directory Enumeration**: Collect domain information from scripts, built-in tools, BloodHound.
2. **Offensive Tradecraft (PowerShell and .NET)**: Use and adapt PowerShell and .NET tools for stealthy enumeration and bypass controls and enhanced logging.
3. **Local Privilege Escalation**: Identify and exploit Windows local vectors to obtain local administrator rights to pivot.
4. **Domain Privilege Escalation**: Find and extract high-privilege credentials, perform replay attacks, Kerberoast, ACL manipulation, AdminSDHolder, Golden/Silver/Diamond tickets, etc.
5. **Domain Persistence and Dominance**.
6. **Cross Trust Attacks**: Abuse trust keys and krbtgt to move between domains or forests and elevate from child-domain admin to enterprise/forest admin.
7. **Abusing Active Directory Certificate Services**.
8. **Defenses and bypass**: MDE EDR, MDI, Architecture and Work Culture Changes.
9. **Defenses and Bypass**: Monitoring and Deception.

While some aspects may not be comprehensively covered in the material, **the fundamentals and essential concepts are indeed extensively discussed**. If your goal is to gain a solid understanding of Active Directory, this will serve as a strong foundation. At the end, the most important topic that is Active Directory, is covered fantastically.

Regardless of which purchase option you choose, you'll have **lifetime access to the course material**. This means that you can get any updates made to the techniques, tools, PDFs, etc. Additionally, every purchase option comes with **one certification exam attempt** and you will have the opportunity to **start your lab within three months of making the purchase**.
### What I did
After passing CEH Master certification, I didn't want to rush into CRTP. I purchased the 30-day lab option and took my time to look into the course material. My work timeline until I got CRTP certified was something like this:
1. **Review course slides**: To get an overview of the concepts, techniques, etc. I went through the whole course slides until I understood (more or less) everything.
2. **Read the lab manual**: Understand the goals, procedures and requirements for each lab, this is key for a successful exam attempt.
3. **Start and complete the lab**: Take your time to execute all tasks. Be sure to understand the commands and their purposes and get all flags. It is important to understand what you can do at every step, what you need to advance, etc.
4. **Take the exam**: Well, in my case this was repeated twice as I will explain later but, once you have a solid base, reserve a day for yourself and take the exam confidently.

Obviously, while I was doing every step I was taking **detailed notes** that could not only help me for the exam but also for my day-to-day work.
## **The exam**
The exam consists in a lab environment made up of **six different machines**. You’ll have RDP access to one machine and must gain execution privileges on the remaining hosts and retrieve a final flag. You get **25 hours total**: 24 hours of exam time plus about an extra hour to account for lab generation (it usually takes around 10–15 minutes).

When you finish the lab, you must submit a **detailed report** describing every step you took to compromise each host. This report cannot be auto-generated, you need to explain what you did, why you used each tool, and include reasoning for your decisions. The report must be written in English and it’s also a good idea (highly recommended) to add relevant sections such as recommendations for mitigating the vulnerabilities you found.
### My exam
I had to take the exam twice because of my own mistakes. The most important tip (which I’ll list later) is to **perform thorough enumeration**. Don’t assume anything, really enumerate everything. Don’t be like me and waste 10 hours on your first attempt because you skipped proper enumeration. When I finally started making progress, I was exhausted and couldn’t think clearly. I hadn’t slept enough and ran out of time. Even though I knew I had failed, I prepared the report so I would have the template ready for my next attempt and sent it in. They confirmed receipt, and four days later they told me I had failed. After a one-month cooldown period I could try again, so I waited and retook the exam.

The second time I was much better prepared. I learned from my mistakes and got things working from the start. I kept pushing until I needed a break, it’s really important to **clear your head and take breaks**. After a walk I came back focused, completed the remaining tasks, and finished the exam in under nine hours.

Anxious to get the certification, I wrote the report immediately and finished it within a couple of hours. I shared it, and the next day I received an ACK. A few days later (about four, I think) I got confirmation that I had passed, and two days after that the certificate appeared in my Accredible account. And that’s the story of how I met your mother, I mean, how **I got the CRTP**.
## **Tips**
- **Enumerate. enumerate, and enumerate**  
Enumeration is absolutely fundamental. As I mentioned earlier, **don’t assume anything**. Make sure you’ve enumerated everything you can. Each step you complete will reveal new information, and if you’re not seeing it, it’s probably because you missed something during enumeration. Also, make sure you’re using your tools properly, sometimes they won’t show the results you expect, but their logs might. Use advanced options and flags. **Enumerate EVERYTHING!**
- **Take breaks**  
It sounds cliché (and it is) but taking breaks and clearing your mind when you feel overwhelmed can make all the difference. Sometimes you’re not stuck because you’re doing something wrong, but simply because you’re mentally saturated and can’t think straight. **Go for a walk, stretch a bit, drink some water, and come back with a fresh mind.**
- **Don't overcomplicate things**  
During the exam, you won’t need to do anything radically different from what you did during the lab practice. Most of the techniques you’ll need are already covered in the course, and that’s one of the things that makes this certification great. Don’t overthink or try crazy new stuff: review your notes. 99% of the time, the answer will already be there.
- **Right before your eyes**  
Sometimes you’ll underestimate a finding that turns out to be crucial for moving forward. This ties closely to the first two tips: you might overlook something because you’re tired or because you didn’t use a tool properly during enumeration. **Don’t panic**. Review what you’ve already done carefully.
- **Screenshots and notes**  
It’s really important to take high-quality notes and screenshots, as you’ll need both for your report and in case you need to redo any steps. High-quality work requires high-quality reporting, and that applies not only to this certification.

## **Summary**
That’s it! I’ve tried to summarize what you need to know before taking the course: my work methodology, my experience, and some (hopefully) useful tips to help with your prep. If you’d like to discuss anything further or have any questions, feel free to reach out! 

> I won’t share any specific details about the exam or provide direct help with it, so please avoid those kinds of requests.
{: .prompt-warning}

**Chaaaaao chao chao chao**

## **Helpful resources**
These sites also have resources available for more technologies, so feel free to keep them for your future experiences!
- [Active Directory Exploitation Cheat Sheet](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)
- [The Hacker Recipes](https://www.thehacker.recipes/)
- [HackTricks for Active Directory](https://book.hacktricks.wiki/en/windows-hardening/active-directory-methodology/index.html)
- [Red Team Notes](https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse)
- Altered Security course resources
- [Report Template](https://github.com/didntchooseaname/Altered-Security-Reporting), I used it as inspiration but you can implement it in your sysreptor.

> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
{: .prompt-tip }
