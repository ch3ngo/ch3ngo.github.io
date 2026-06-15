---
title: Certified Azure Red Team Professional (CARTP) review
date: 2026-06-15 17:15:00 +0200
categories: [Certification, Azure]
tags: [training, opinion, azure, cloud]
author: diego
description: How I got CARTP certified. Information, my experience and some (I hope) useful tips.
image:
  path: /assets/img/posts/cartp/cartp.png
  alt: CARTP Certificate
---

If you are thinking about taking the [Certified Azure Red Team Professional (CARTP) from Altered Security](https://www.alteredsecurity.com/cartp), this can interest you.

![CARTP](assets/img/posts/cartp/cartp.png)
_CARTP certificate_

## **Background**
By day, I work as an Ethical Hacker, a role I've held for approximately **4 years**. In my spare time, however, I am not a caped crusader like Batman, but rather an enthusiastic learner. Previously working in infosec prior to earning the Certified Azure Red Team Professional (CARTP) certification provided me a solid foundation. I saw this as a challenge and very good opportunity to expand my knowledge and skills within Azure and Cloud environments.
  
Before CARTP, I had already obtained some certifications such as **Certified Ethical Hacker (CEH Master)** from EC Council, **Certified Red Team Analyst (CRTA)** from CyberWarfare Labs, **eLearn Junior Penetration Tester v2**, and, most relevant here, **Certified Red Team Professional (CRTP)** from the same provider, Altered Security. That last one is important context because, as I will explain throughout this review, CRTP sets a very high bar and CARTP inevitably gets compared to it.

## **Course review**
### Introduction
From the course syllabus and as a summary, CARTP preparation will teach you:
1. **Azure Architecture and Concepts**: Understand how Azure is structured, including Entra ID, tenants, subscriptions, resource groups, RBAC, Managed Identities and the Azure Resource Manager model.
2. **Discovery and Reconnaissance**: Identify tenant information, enumerate services and subdomains without authentication, and validate email addresses against a target organization.
3. **Initial Access**: Perform realistic attack scenarios including Illicit Consent Grant attacks, phishing with Evilginx3, exploiting vulnerable App Services and abusing publicly exposed Blob Storage containers.
4. **Authenticated Enumeration**: Enumerate Entra ID objects (users, groups, applications, service principals, devices, roles) and Azure resources using the Mg module, Az PowerShell, Az CLI, ROADtools and AzureHound.
5. **Privilege Escalation**: Identify and abuse misconfigurations in Automation Accounts, Key Vaults, Enterprise Applications, ARM Templates and Function Apps to escalate privileges within an Azure environment.
6. **Lateral Movement**: Move across Azure environments by abusing VM features such as User Data and Custom Script Extensions, steal tokens via Pass-the-PRT, leverage Intune for remote script execution, and use Application Proxy to pivot towards on-premises resources.
7. **Hybrid Identity Attacks**: Understand and exploit the bridge between on-premises Active Directory and Azure, including AD Connect abuse to move between environments.
8. **Persistence**: Establish persistence in Entra ID and Azure through Service Principals, application credentials and other mechanisms.

While some aspects may not be as deeply covered as you might hope, **the fundamentals and essential concepts are extensively discussed**. If your goal is to understand how Azure and Entra ID environments can be attacked end to end, this course provides a solid map of that territory. Just, as I will explain, do not expect the same level of depth you would find in a course entirely focused on a single, decades-old technology like Active Directory.

Regardless of which purchase option you choose, you will have **lifetime access to the course material**, meaning any updates to techniques, tools and PDFs are included. Every option also comes with **one certification exam attempt** and you have **three months from purchase to activate your lab** (unless you get some kind of promotion like Black Friday in which case you have up to six months).

### Pricing

Altered Security usually offers discounts, like around Black Friday. I purchased the **30-day lab access + lifetime course material + one exam attempt** bundle during their Black Friday sale and then proceeded to wait six months before actually starting. I have been a bit lazy lately, sorry, nobody is perfect b\*\*\*h.

If you are planning to take this, keep an eye on their promotions. The discount can be significant and, given that lifetime access to material is included regardless of the option you pick, the main variable is how many lab days you realistically need.

![CARTP Pricing](assets/img/posts/cartp/cartp_pricing.png)
_CARTP Pricing_

### What I did
My approach was the same as with CRTP: review everything first, then practice, then exam.

1. **Review the course slides**: Go through all the theory until the concepts are reasonably clear. Azure has a lot of moving parts and it is worth spending time on the architecture before running a single command.
2. **Read the lab manual**: Understand each learning objective before sitting in front of the lab. Know what you are trying to achieve and why, not just how.
3. **Complete the labs**: Work through all the learning objectives methodically. The labs are, as always with Altered Security, well explained and structured. Take notes and screenshots of everything as you go.
4. **Take the exam**: Once you have solid notes and a clear understanding of the techniques, book a day for yourself and go for it.

Obviously, throughout every step I was taking **detailed notes** that would be useful both for the exam and for day-to-day work.

## **A few honest thoughts on the course**

Coming from CRTP, which is 100% focused on Active Directory and goes deep, CARTP feels different. Not bad, just different.

Azure and Entra ID are newer technologies. The attack surface is not as vertically deep as on-premises Active Directory, and the course reflects that. You will see a wide range of content, including phishing techniques, misconfiguration abuse across multiple services, and several kill chains for compromising an Azure tenant end to end, but a fair portion of it is more theoretical than hands-on. The course covers the breadth of what Azure attacks look like rather than going very deep into any single technique. Coming from CRTP, which felt laser-focused and technically dense throughout, this was a bit of an adjustment. I completely understand why, Azure is what it is, but it is worth setting expectations accordingly.

One specific thing worth calling out: at the end of the lab content there is a CTF. And that is essentially all the information you get about it. No hints, no description, no structure. Just a message along the lines of "there is a CTF, good luck." I still am not entirely sure what it is actually supposed to test or what the intended scope is. It felt a bit abandoned compared to the rest of the course, which is otherwise well organized but I guess it is ok?

That said, none of this takes away from the genuine value of the material. Given how many organizations are now running hybrid or fully cloud environments, understanding Azure attack paths is increasingly important for anyone working in red team or offensive security.

## **The exam**
The exam is a practical lab environment where you need to complete a series of objectives and retrieve flags. I was well prepared and it took me around six hours to complete it (+ whatever amount of time took me to prepare the report, there is plenty of time for this that's for sure).

The exam is not particularly complex. Importantly, a significant portion of what the course covers does not appear in the exam, which is understandable: including everything would require an extremely complex setup that would be fragile, time-consuming and likely frustrating to run at scale. The scope is kept focused, and if you have done the labs thoroughly and internalized the key techniques, you will be fine.

As with other Altered Security certifications, you need to submit a **detailed report** describing your methodology, the steps you took and your findings. High-quality notes and screenshots taken throughout the exam are essential for this. Do not leave documentation as an afterthought.

Once submitted, the review process takes a few days (5-7 working days) and you receive your result. I received my result exactly a week after I sent my report and a day later I received the certificate.

## **Tips**
- **Know your tools before the exam**  
Spend time with the tools during the labs: Az PowerShell, Az CLI, the Mg module, ROADtools. Know what each tool gives you and when to use it.
- **Enumerate everything, always**  
Same advice as CRTP: do not assume anything. Enumerate users, groups, applications, service principals, role assignments and resources at every scope level. Missing something during enumeration means missing an attack path.
- **Understand the kill chains**  
The course presents several end-to-end attack scenarios. Make sure you understand the logic behind each one, not just the individual commands.
- **Take breaks**  
It sounds cliché because it is true: if you get stuck, step away for a bit. A fresh perspective after a short break is worth way more than another hour staring at the same problem.
- **Notes and screenshots**  
Take them from the very start, for every step. You will need them for the report and you will regret it if you have to go back and redo something you forgot to document.

## **Summary**
That is it. CARTP is a solid certification for anyone working in or moving towards cloud security. **Would I recommend it? Absolutely**. Is it as deep or as impactful as CRTP? Honestly, not quite, but that is not a fair comparison. Azure is a different, newer attack surface and the course covers it well for what it is. Given the ongoing transition of organizations to cloud and hybrid Active Directory environments, having a structured understanding of Azure attack paths is genuinely valuable, especially for red teamers who already have a strong on-prem foundation and want to extend it to the cloud.

If you have already done CRTP and want to push your skills into the cloud, CARTP is the natural next step from the same provider. If you are new to Altered Security courses, know that the quality is consistently good and the material is well organized throughout.

> I won't share any specific details about the exam or provide direct help with it, so please avoid those kinds of requests.
{: .prompt-warning}

**Chaaaaao chao chao chao**

## **Helpful resources**
- [AzureHound for BloodHound](https://bloodhound.specterops.io/collect-data/ce-collection/azurehound)
- [HackTricks for Azure](https://book.hacktricks.wiki/en/pentesting-cloud/azure-security/index.html)
- [The Hacker Recipes for Azure](https://www.thehacker.recipes/)
- [Azure Built-in Roles Reference](https://azure.permissions.cloud/builtinroles)
- Altered Security course resources

> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
