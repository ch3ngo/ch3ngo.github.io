---
title: Certified WiFiChallenge Professional (CWP) review
date: 2026-01-26 19:15:00 +0200
categories: [Certification, Wi-Fi]
tags: [training, opinion, wi-fi]
author: diego
description: How I got CWP certified. Information, my experience and some (I hope) useful tips.
image:
  path: /assets/img/posts/cwp/cwp.png
  alt: WiFiChallenge Academy
---

If you're considering the [Certified WiFiChallenge Professional (CWP) from WiFiChallenge](https://academy.wifichallenge.com/courses/certified-wifichallenge-professional-cwp), this review might help you decide.

![CWP](assets/img/posts/cwp/cwp_cert.png)
_CWP certificate_

## **Background**
By day, I work as an Ethical Hacker, a role I've held for approximately **3 years**. In my spare time, however, I am not a caped crusader like Batman, but rather an enthusiastic learner. Previously working in infosec prior to earning the Certified WiFiChallenge Professional (CWP) certification provided me a solid foundation. I saw this as a challenge and very good opportunity to expand my knowledge and skills within Wi-Fi environments. It is important to highlight that this was not my strongest ability as I have not done much Wi-Fi network security testing in the past and I saw this as a really good oportunity.

## **Course review**
### Introduction
This course is created by [Raúl Calvo](https://www.linkedin.com/in/raulcalvolaorden/) and is available in both English and Spanish. I took the Spanish version, so my review is based on that content. I'm confident the English version is equally well-produced.

From the course syllabus and as a summary, CWP preparation will teach you:
1. **Wi-Fi Network Theory**: Fundamental concepts of IEEE 802.11 networks, including how Wi-Fi operates, frame types, and the main security and encryption models used in wireless communications.
2. **WiFiChallenge Lab - Getting Started**: How to access, configure, and navigate the WiFiChallenge Lab environment to complete hands-on wireless security challenges.
3. **Linux Fundamentals**: Essential Linux command-line skills, networking basics, and remote access techniques required for Wi-Fi testing and analysis.
4. **Wi-Fi Networks in Linux**: Managing wireless interfaces in Linux, enabling monitor mode, creating and connecting to networks, and capturing wireless traffic for analysis.
5. **Offensive Wi-Fi Security**: Practical offensive techniques used to discover, analyze, and attack wireless networks.
This includes reconnaissance and attacks against OPN, OWE, WEP, PSK, and SAE networks, as well as management-frame reconnaissance, management-based attacks, and wireless attack detection.
6. **Advanced Wi-Fi Enterprise Attacks** (Indirect MGT Attacks): Advanced attack techniques targeting enterprise Wi-Fi deployments, focusing on indirect management attacks and complex authentication environments.
7. **Real-World Experience**: Applying wireless attack techniques in realistic scenarios, considering hardware limitations, signal behavior, and professional assessment practices.
8. **Securing Wi-Fi Networks**: Defensive strategies and best practices for hardening Wi-Fi networks against common and advanced wireless attacks.

The content is comprehensive and well-organized. You'll never feel lost about what you're learning or why it matters. I'd say the course builds you from the ground up to conduct **professional Wi-Fi security audits**.

Beyond the core curriculum, the course provides practical checklists you can use during real assessments, antenna recommendations tailored to different needs, detailed guidance on configuring your hardware for various scenarios, and plenty of actionable information to prepare you for real-world Wi-Fi security testing. It's this blend of theory and practical detail that makes the course so effective.

The lab setup is straightforward and effective. It runs on a VM or Docker, everything is self-contained locally, and there are no external dependencies or surprise costs. You can follow the course instructions or practice independently. Of the 29 exercises, almost all have clear explanations in both text and video formats, though a few solutions could be more detailed.

### Pricing

There are several pricing options, all with lifetime access to updated material:
- **199€** (recommended): Includes one exam attempt
- **249€**: Includes two exam attempts, though I don't think you'll need a second one if you take the course seriously
- **Exam only**: 99€ per attempt if you want to practice independently first

You can also register on the [labs platform](https://lab.wifichallenge.com/challenges) to validate flags without purchasing the full course.

I recommend the 199€ package. You get excellent material, genuine learning value, and you're supporting an instructor who provides a far better alternative to most commercial courses at a fraction of the price.

**Important note on exam vouchers:** When you purchase the course, you won't receive the exam voucher immediately. You'll get it once you complete the course and mark everything as finished. The voucher is valid for one year.

![Course options](/assets/img/posts/cwp/course_options.png)
_Price options_

> 💡 **Tip: Get a discount**  
> Here's something I wish I'd known earlier: complete the WiFiChallenge Lab feedback form on the platform, and you'll get a **10% discount code**!
{: .prompt-info }

### My approach
I was preparing for another certification when a colleague got his CWP and started praising it. Since Wi-Fi security wasn't my strong suit, I decided to jump in without postponing. I committed the time and chose the 199€ option.

My study method was straightforward: follow the course as designed. Work through the theory, then practice immediately with the labs. Complete each challenge, capture the flags, and move forward. No overthinking—just absorb the concepts and apply them.

Before the exam, I reviewed the challenges and reinforced the key concepts: identifying different network types, determining which attacks apply in each scenario, and understanding the prerequisites for each technique.

So you can imagine a timeline, I purchased it 13th January and did the exam 24th January. So you could say I got it in less than two weeks.

## The exam
Once you finish the CWP course and labs, the next and final step is the CWP practical exam. The exam is designed to validate that you can apply everything you’ve learned in a realistic scenario, not just recall theory. It consists of a **6-hour hands-on assessment** in a dedicated remote lab where you must successfully compromise at least **4 out of 5 Wi-Fi access points**, using the **same tools, techniques, and methodologies practiced** during the course.

During the exam, you’ll connect via **SSH** to the exam environment and perform real attacks against different Wi-Fi configurations. For each compromised network, you must **prove access** by connecting to the Wi-Fi and retrieving a flag from an internal web service. Clear documentation is required, and you’ll submit a short professional **report** (templates are provided) within **24 hours after the exam ends**, demonstrating both technical skill and proper reporting. Just like a real-world wireless security assessment.

What makes the CWP exam stand out is that it mirrors real offensive Wi-Fi work: time pressure, multiple targets, tool selection, and clear evidence. Passing the exam shows that you’re not just familiar with Wi-Fi attacks, but capable of executing them end-to-end in a controlled, professional manner.

Once you send the report, you will have to wait (approximately) **2 working days** to get the results and any feedback on your report.

## **TL;DR**
The **CWP is an excellent certification** for anyone serious about Wi-Fi security testing. The course is comprehensive, well-organized, and strikes the perfect balance between theory and practical application. You get **detailed checklists**, **hardware recommendations**, **configuration guidance**, and **everything you need to conduct real-world Wi-Fi audits**. This is not just pass an exam.

At **199€**, it's a genuine bargain compared to other professional certifications. The labs are self-contained and locally hosted, and the practical exam is realistic and fair. I completed the entire course and exam in less than two weeks, but take your time and focus on understanding concepts rather than rushing.

**My recommendation:** Get the **199€ package**. **Do the labs thoroughly**, **take notes**, **review before the exam**, and **prioritize enumeration above all else**. The instructor has created something special here: a course that respects your time and delivers real value. If you're considering getting into Wi-Fi security professionally, **this is the way to start**.

> I won't share any specific details about the exam or provide direct help with it, so please avoid those kinds of requests.
{: .prompt-warning}

**Chaaaaao chao chao chao**

> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
