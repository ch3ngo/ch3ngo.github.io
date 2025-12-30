---
title: AI Red Teamer by HackTheBox review
date: 2025-12-30 14:15:00 +0200
categories: [Certification, AI]
tags: [training, opinion, ai, htb]
author: diego
description: The AI Red Teamer path by HackTheBox. What is it, how much, is it worth?
image:
  path: /assets/img/posts/ai_rt_htb/ai_red_teamer.png
  alt: AI Red Teamer Path HTB
---

> **Quick disclaimer**: Some of the badges don't have an illustration yet, so they look like they are locked. Trust me, they are not.
{: .prompt-warning }

## **Introduction**

The [AI Red Teamer Path](https://academy.hackthebox.com/path/preview/ai-red-teamer) is, according to Hack The Box, a Job Role Path. It currently has a projected cost of 970 cubes, from which you earn 210 back. These numbers may change over time, as the path is relatively new and its cost tends to increase as additional modules are added. Below is the official description, which already sets high expectations:
> The AI Red Teamer Job Role Path, in collaboration with Google, trains cybersecurity professionals to assess, exploit, and secure AI systems. Covering prompt injection, model privacy attacks, adversarial AI, supply chain risks, and deployment threats, it combines theory with hands-on exercises. Aligned with Google’s Secure AI Framework (SAIF), it ensures relevance to real-world AI security challenges. Learners will gain skills to manipulate model behaviors, develop AI-specific red teaming strategies, and perform offensive security testing against AI-driven applications.

The path consists of 12 different modules, divided into up to 230 sections. In this post, I’ll share my experience module by module. That said, if you’re looking for a high-level overview or want to read about my overall impressions, feel free to jump directly to [My Path Experience](#my-path-experience).

**Spoiler alert**: in terms of value for money, this path is the best AI Red Teaming resource I’ve found so far. I’ve spent quite some time reviewing and testing different options in an attempt to find solid training (and some form of certification) in this field. Many alternatives are significantly more expensive, and I’m not convinced they deliver equivalent value. On top of that, this path comes from one of the largest and most reputable cybersecurity platforms available online, which makes it an easy recommendation.

### How much is it?
So, how much does the full career path actually cost? When I started, the projected cost was around **500-ish cubes**. I don’t remember the exact number, but I do know that a **single month of the Gold Plan ($38)** was enough at the time. This plan gives you **500 cubes per month**, which you can use to permanently unlock modules.

Since the path is still fairly new, a couple of additional modules were added along the way, and the total cost increased. Because of that, I ended up needing a **second month of subscription**.

If you’re planning to complete the entire Job Role Path from the start, I highly recommend getting a **one-month Platinum subscription ($68)**. This gives you **1,000 cubes**, which comfortably covers the current projected cost of the path (970 cubes) and still leaves you with a few extra cubes to spend on modules from other topics.

![HTB Subscriptions](assets/img/posts/ai_rt_htb/htb_subs.png)

#### Cubes system explained
Cubes are used to unlock Modules & Paths.
You get back Cubes as a reward for Module & Paths completion. It is like cash back, but better!
The Cubes needed to unlock and reward back depend on the Tier of the Module. Find a table below for reference.

| Module Tiers | Unlock With | Reward Back |
| :----------: | :---------: | :---------: |
|    Tier 0    |     10      | 10 (Free!)  |
|    Tier I    |     50      |     10      |
|   Tier II    |     100     |     20      |
|   Tier III   |     500     |     100     |
|   Tier IV    |    1,000    |     200     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

If you want to level up your hacking learning, you will definitely need Cubes. You can purchase them in two different ways:
- You can purchase your desired amount of Cubes.
- We suggest purchasing a subscription. This will get you a nice discount and also provide the Cubes needed for the level of difficulty you want to get into.   
There are 3 subscription levels:
  - **Silver**: 200 Cubes per month &rarr; 11% discount
  - **Gold**: 500 cubes per month &rarr; 27% discount
  - **Platinum**: 1,000 cubes per month &rarr; 36% discount

## **Modules**
> You might get stuck on some exercises or skills assessments. Feel free to contact me through my socials (you can find them, I trust you), and **I’ll happily help you with getting the flags!** 😄
{: .prompt-tip }
### **Fundamentals of AI**

|  Tier  | Difficulty | Category | # of sections | Estimated time |
| :----: | :--------: | :------: | :-----------: | :------------: |
| Tier 0 |   Medium   | General  |      24       |    8 hours     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![Superior Intelligence](assets/img/posts/ai_rt_htb/superior_intelligence.png){: w="350px" }
_[Check my Superior Intelligence badge here](https://academy.hackthebox.com/achievement/badge/97cd7fc3-a9cc-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module is a broad and fairly deep introduction to the core concepts behind Artificial Intelligence, Machine Learning, and Deep Learning. It covers the main learning paradigms (supervised, unsupervised, and reinforcement learning), common algorithms, neural network architectures, and a high-level view of generative AI and large language models.

Within the path, this module exists to give you a solid theoretical foundation so later, more practical topics don’t feel like magic. It’s clearly meant to level the playing field for people who haven’t formally studied AI before.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️⭐️☆  
> Interesting and well explained, but purely theoretical and not directly related to AI red teaming.
{: .prompt-info }

The content is well structured and does a good job of explaining technical concepts without going too deep into the math. It provides solid coverage of learning types, models, and architectures, and works well both as an introduction and as a reference you can come back to later.

That said, this module has **zero hands-on material**. There are no labs, exercises, or assessments, everything is theoretical. From an AI red teaming perspective, it doesn’t directly teach you anything offensive or security-related. Its value is indirect: understanding how models are built, trained, and optimized helps you reason about their weaknesses later on.

If you already have a strong background in AI or ML, this module will likely feel like a recap. If you don’t, it’s a dense but accessible way to get up to speed.

#### Tips

- Don’t rush it, this module works better when consumed in short sessions
- You can safely skip the math-heavy parts if your goal is red teaming, not model development
- Treat it as foundational knowledge rather than something you need to “master”
- Bookmark it as a reference. It’s more useful later than it initially feels

### **Applications of AI in InfoSec**

|  Tier  | Difficulty | Category | # of sections | Estimated time |
| :----: | :--------: | :------: | :-----------: | :------------: |
| Tier 0 |   Medium   | General  |      25       |    8 hours     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![Synthetic Intelligence](assets/img/posts/ai_rt_htb/synthetic_intelligence.png){: w="350px" }
_[Check my Synthetic Intelligence badge here](https://academy.hackthebox.com/achievement/badge/662e5d0d-b388-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module is where things start to get hands-on. It focuses on setting up a functional AI lab and using it to build and train real machine learning models applied to cybersecurity problems. You’re guided through environment setup (Miniconda, JupyterLab), dataset handling, preprocessing, and model training using common Python libraries like Scikit-learn and PyTorch.

Within the path, this module bridges theory and practice. It shows how AI is actually used in InfoSec workflows, taking you from raw data to trained models in realistic security-related scenarios.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️⭐️⭐️  
> Very practical and genuinely interesting, with real AI models trained on security datasets.
{: .prompt-info }

This is the first module where you actually start doing things. The initial sections walk you step by step through building your own AI lab, so even if you’ve never trained a model before, it’s approachable.

Instead of strictly following the suggested setup, I went with **Docker**. I didn’t want to install all the prerequisites directly on my machine, and getting GPU passthrough working properly in a VirtualBox-based setup is usually more effort than it’s worth. With Docker, a couple of commands were enough to get everything running in a clean, portable environment.

The module walks you through training three different models using publicly available datasets, all tied to cybersecurity use cases. Each one uses a different approach, so you end up touching a bit of everything: data preprocessing, feature engineering, classical ML, and deep learning. It’s well designed and does a good job of reinforcing concepts through repetition without feeling redundant.

One important thing to note: if you’re aiming to earn the badge, you need to complete everything. Some flags required for completion are tied directly to the labs, so skipping sections isn’t really an option.

#### Tips

- Consider using Docker if you don’t want to pollute your local system or deal with GPU passthrough issues
- Training on CPU works, but expect longer runtimes—patience helps here
- Don’t skip the dataset exploration steps, they’re more important than they initially look
- Follow the labs closely if your goal is the badge, as some flags depend on full completion

### **Introduction to Red Teaming AI**

|  Tier  | Difficulty | Category  | # of sections | Estimated time |
| :----: | :--------: | :-------: | :-----------: | :------------: |
| Tier I |   Medium   | Offensive |      11       |    4 hours     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![Model Breaker](assets/img/posts/ai_rt_htb/model_breaker.png){: w="350px" }
_[Check my Model Breaker badge here](https://academy.hackthebox.com/achievement/badge/01de7539-b4f4-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module marks the real beginning of the AI Red Teaming path. It introduces the security perspective around machine learning systems and generative AI, focusing on where things can go wrong and how these systems interact with more traditional application components.

It covers the OWASP Top 10 for ML and the OWASP Top 10 for LLMs, along with a high-level view of how to approach attacking ML-based systems and their individual components. Within the path, this module connects the AI fundamentals you’ve already seen with an attacker mindset.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️⭐️⭐️  
> Solid introduction to AI red teaming concepts, risks, and attack surfaces.
{: .prompt-info }

This is where the path starts to feel like **actual red teaming**. The module does a good job of explaining the main risks, vulnerabilities, and attack classes affecting ML systems and generative AI, without overwhelming you with unnecessary theory.

The OWASP Top 10 sections are especially useful, as they help frame AI security issues in a familiar structure for anyone coming from traditional application or infrastructure security. There are also concrete examples and some hands-on exercises, which help reinforce the concepts and keep things engaging.

It’s not deeply technical or exploit-heavy, but that’s intentional. This module is about building the right mental model before moving on to more advanced and practical attacks later in the path.

#### Tips

- Make sure you’re comfortable with the previous two modules, this one builds directly on them
- Focus on understanding why each OWASP category exists, not just memorizing names
- Treat this module as a mindset shift from “AI theory” to “AI attack surface”
- Take notes, you’ll keep coming back to these concepts in later modules

### **Prompt Injection Attacks**

|  Tier   | Difficulty | Category | # of sections | Estimated time |
| :-----: | :--------: | :------: | :-----------: | :------------: |
| Tier II |   Medium   | General  |      12       |    8 hours     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![Prompt Phantom](assets/img/posts/ai_rt_htb/prompt_phantom.png){: w="350px" }
_[Check my Prompt Phantom badge here](https://academy.hackthebox.com/achievement/badge/5a146967-bb40-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module focuses on one of the most common and relevant attack classes against large language models: prompt injection. It introduces how LLMs can be manipulated purely through input prompts, covering direct prompt injection, indirect prompt injection, and various jailbreaking techniques.

Within the path, this is the first module where you actively interact with LLMs from an attacker’s perspective and start understanding how prompt design can influence, bypass, or completely override intended model behavior.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️⭐️⭐️  
> Great introduction to prompt injection with hands-on practice and real-world relevance.
{: .prompt-info }

This is where you start interacting with “real” LLMs. They’re obviously simplified and not fully representative of production-grade models, but they’re good enough to understand how LLMs process input and how small changes in prompts can drastically alter outputs.

The module covers a wide range of prompt injection techniques and provides plenty of practical examples and tooling to experiment with. You’re encouraged to play with the models, observe how they “think,” and learn how to manipulate them into producing unintended behavior. It also does a good job of briefly touching on mitigations, which helps frame the attacks in a more realistic defensive context.

One important real-world takeaway is that many modern, industry-grade LLMs (such as recent LLaMA or Meta models) are already trained on datasets that include prompt injection and jailbreaking attempts. This doesn’t mean prompt injection is “solved,” but it does mean you can’t blindly rely on basic attacks. If you’re deploying your own LLM, you might not need to train from scratch against these attacks, but testing is non-negotiable.

#### Tips

- Don’t expect lab models to behave exactly like production LLMs
- Focus on understanding why a prompt works, not just that it works
- Experiment beyond the minimum required for the flags, you’ll learn more that way
- Even if mitigations exist, always validate them with real testing

### **LLM Output Attacks**

|  Tier   | Difficulty | Category  | # of sections | Estimated time |
| :-----: | :--------: | :-------: | :-----------: | :------------: |
| Tier II |   Medium   | Offensive |      14       |    8 hours     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![Output Overdrive](assets/img/posts/ai_rt_htb/not_yet.png){: w="350px" }
_[Check my Output Overdrive badge here](https://academy.hackthebox.com/achievement/badge/3b82cd0d-bda5-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module focuses on attacking the output generated by LLMs rather than the prompt itself. The idea is simple but powerful. If an application blindly trusts LLM output and feeds it into other systems, you can often fall back to classic web and application attacks.

Within the path, this module shows how traditional vulnerabilities resurface in AI driven applications when LLM responses are not properly validated or sanitized.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️⭐️⭐️  
> Classic application attacks applied to LLM output with very real impact.
{: .prompt-info }

In this module, you attack the LLM output directly. This means applying well known techniques like XSS, SQL injection, and command injection, but through LLM generated responses. A good example is inducing an SQL injection by mixing natural language with SQL instructions to extract information from a backend database.

The module does a great job of showing how easily these issues appear when LLM output is treated as trusted input. It also covers more subtle problems like hallucinations. At first glance, hallucinations may look like a minor issue, but the module shows how they can lead to serious security and trust problems in real applications.

One of the most interesting sections covers abuse attacks. This goes beyond pure exploitation and looks at how AI systems can be misused for misinformation, hate speech, harassment, and similar scenarios. Mitigations and regulatory considerations are also discussed, which helps ground the attacks in a real world context.

#### Tips
- Think of LLM output as untrusted user input at all times
- Apply the same mental model you use for web application testing
- Pay attention to hallucinations, especially when output is used for decision making
- The abuse attack section is worth reading carefully, even if it feels less technical
- **Skills Assessment**: You can modify the address of `htb-stdnt` and ask the admin bot to give you shipment information with verbose
```
123 Test Site" ;cat flag.txt "
```

### **AI Data Attacks**

|  Tier   | Difficulty | Category  | # of sections | Estimated time |
| :-----: | :--------: | :-------: | :-----------: | :------------: |
| Tier II |    Hard    | Offensive |      25       |     3 days     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![Data Distorter](assets/img/posts/ai_rt_htb/not_yet.png){: w="350px" }
_[Check my Data Distorter badge here](https://academy.hackthebox.com/achievement/badge/5fd7374e-c2e7-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module shifts the focus from attacking prompts and outputs to attacking the data itself. Since AI systems are entirely data driven, compromising the data pipeline can undermine the entire model. The module explores how training data, features, and even model artifacts can become effective attack vectors.

Within the path, this module goes deeper into the foundations of AI security by showing how attacks can be introduced at the earliest stages of model development and training.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️⭐️☆  
> Dense and demanding, but extremely important for understanding real AI attack surfaces.
{: .prompt-info }

This module is interesting because it shows how attacks can start at the very root of an AI model. You explore different techniques such as data poisoning, label flipping, clean label attacks, backdoors, and even hiding information inside model artifacts.

The content can feel heavy at times. Some sections are quite theoretical and require patience to get through. The hands on parts are also more coding intensive than previous modules. I would strongly recommend relying on advanced LLMs to help you with the coding and experimentation, especially if Python is not your strongest area.

Despite the difficulty, the value is clear. Many real world AI models are trained on publicly available datasets. If an attacker manages to poison those datasets, the resulting model can behave unpredictably or maliciously. These attacks are not trivial to pull off, but understanding them is critical if you want a realistic view of AI security.

#### Tips
- Take this module slowly and do not rush through it
- Use an LLM as a coding assistant to save time and frustration
- Focus on understanding the attack logic rather than memorizing details
- The theory heavy sections pay off later when you see how subtle these attacks can be

### **Attacking AI - Application and System**

|  Tier   | Difficulty | Category  | # of sections | Estimated time |
| :-----: | :--------: | :-------: | :-----------: | :------------: |
| Tier II |   Medium   | Offensive |      14       |    8 hours     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![Protocol Breaker](assets/img/posts/ai_rt_htb/not_yet.png){: w="350px" }
_[Check my Protocol Breaker badge here](https://academy.hackthebox.com/achievement/badge/5202770b-cc7f-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module moves away from attacking models and data and focuses on the surrounding application and system layers of an AI deployment. It shows how weaknesses in these components can completely undermine an otherwise well designed model.

A major part of the module is dedicated to the Model Context Protocol and how MCP servers work, why they exist, and how they can introduce new attack surfaces when improperly implemented or trusted.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️⭐️⭐️  
> Extremely fun and valuable. One of the strongest modules in the path.
{: .prompt-info }

This module is really good and genuinely fun to go through. The focus on real application and system level issues makes everything feel much closer to real world AI deployments. The MCP server sections are especially interesting and highlight how quickly things can go wrong when context and trust boundaries are poorly defined.

The hands on exercises are well designed and force you to think instead of just following obvious paths. The skills assessment is tricky and intentionally sets traps. Going for the low hanging fruit will often fail, and you need to slow down and reason about the full attack surface.
Once everything clicks, the payoff is very satisfying. This module delivers real red teaming value and feels like something you would actually encounter in modern AI enabled applications.

#### Tips
- Do not rush the skills assessment
- Question every trust boundary and assumption
- If something looks too easy, it probably is
- This module rewards careful thinking more than brute forcing

### **AI Evasion - Foundations**

|  Tier   | Difficulty | Category  | # of sections | Estimated time |
| :-----: | :--------: | :-------: | :-----------: | :------------: |
| Tier II |   Medium   | Offensive |      12       |    8 hours     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![ModelEvader](assets/img/posts/ai_rt_htb/not_yet.png){: w="350px" }
_[Check my ModelEvader badge here](https://academy.hackthebox.com/achievement/badge/38ea3b02-ce16-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module introduces evasion attacks against AI models at inference time. The focus is on how models process untrusted input and how attackers can manipulate that input to bypass detection or influence predictions.

It covers core evasion concepts such as threat models, white box versus black box attacks, transferability, and feature obfuscation techniques like GoodWords. Within the path, this module lays the groundwork for understanding how defensive models can be bypassed without touching training data.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️⭐️☆  
> Dense but solid. The strongest of the evasion modules and worth the effort.
{: .prompt-info }

This is the first of three AI evasion modules and, in my opinion, the best one. The content is good and technically interesting, although it can feel dense at times. You spend a fair amount of time working through theory before applying it in practice.

The hands on exercises are well designed and the skills assessment is enjoyable and fair. Implementation can be time consuming, especially when working under query limits and black box assumptions. Using an LLM as a coding assistant helps a lot and speeds up the process significantly.

While it requires solid Python skills and patience, the module provides a strong foundation for understanding how evasion attacks work in real world AI systems.

#### Tips
- Expect a slower pace and take breaks when needed
- Use an LLM to help with implementation details
- Focus on understanding the evasion logic rather than perfect code
- Running the labs on your own machine is strongly recommended

### **AI Evasion - First-Order Attacks**

|  Tier   | Difficulty | Category | # of sections | Estimated time |
| :-----: | :--------: | :------: | :-----------: | :------------: |
| Tier II |    Hard    | General  |      23       |     2 days     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![GradientGhost](assets/img/posts/ai_rt_htb/not_yet.png){: w="350px" }
_[Check my GradientGhost badge here](https://academy.hackthebox.com/achievement/badge/65453c30-d79e-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module focuses on gradient based evasion attacks against neural network classifiers. It explains how small, carefully crafted input perturbations can exploit a model’s differentiable structure and force misclassifications at inference time.

The module covers foundational concepts behind these attacks and dives into well known techniques such as FGSM, iterative FGSM, and DeepFool. Within the path, this module goes deeper into the mathematical side of AI evasion and shows why gradient trained models are inherently fragile.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️☆☆  
> Very well explained, but extremely math heavy and hard to digest.
{: .prompt-info }

The quality of the content is high and it is clear that a lot of effort went into explaining these attacks properly. That said, this module was not for me. It is very math focused and becomes difficult to follow even after multiple reads.

It took me a long time to start understanding some of the core concepts, and eventually I relied heavily on an LLM to explain the material in simpler terms and to help with the skills assessment implementation. Once everything clicks, the ideas are interesting, but getting there can be frustrating.

For people with a strong background in mathematics and neural networks, this module can be excellent. For others, it may feel overwhelming. My rating reflects personal experience rather than content quality.

#### Tips
- Do not underestimate the math requirements
- Use an LLM to simplify explanations and assist with code
- Focus on the intuition behind each attack rather than the equations
- Running everything on your own machine makes experimentation easier

### **AI Evasion - Sparsity Attacks**

|  Tier   | Difficulty | Category  | # of sections | Estimated time |
| :-----: | :--------: | :-------: | :-----------: | :------------: |
| Tier II |    Hard    | Offensive |      28       |     3 days     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![PixelSniper](assets/img/posts/ai_rt_htb/not_yet.png){: w="350px" }
_[Check my PixelSniper badge here](https://academy.hackthebox.com/achievement/badge/b7b7c362-d9f7-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module explores sparsity attacks, where adversarial perturbations are concentrated on a few carefully selected features rather than spread across all inputs. It introduces techniques for generating effective adversarial examples while modifying as few input dimensions as possible.

Within the path, this module builds on the first-order evasion attacks and dives deeper into mathematical optimization approaches used to constrain perturbations under strict sparsity budgets.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️☆☆☆  
> Extremely math heavy and difficult to follow. Not enjoyable for my background.
{: .prompt-info }

This module was very challenging for me. Like the previous first-order evasion module, the content is technically impressive but extremely math focused. It covers ElasticNet attacks, FISTA optimization, and saliency-based approaches in detail, but understanding the underlying math requires significant effort.

For someone with a cybersecurity rather than a mathematical background, this module is tough. I was mentally drained from previous modules and struggled to enjoy the material. That said, for readers who enjoy deep math and optimization, it could be a fascinating exploration of advanced adversarial techniques.

#### Tips
- Approach slowly and expect heavy math
- Using an LLM to assist with explanations or code may help
- Focus on intuition over full derivations if your goal is practical AI red teaming
- Run labs on your own machine for faster experimentation

### **AI Privacy**

|  Tier   | Difficulty | Category  | # of sections | Estimated time |
| :-----: | :--------: | :-------: | :-----------: | :------------: |
| Tier II |   Medium   | Defensive |      21       |     2 days     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![ShadowGuard](assets/img/posts/ai_rt_htb/not_yet.png){: w="350px" }
_[Check my ShadowGuard badge here](https://academy.hackthebox.com/achievement/badge/0df755e5-dec0-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module focuses on privacy risks in machine learning systems, specifically how models trained on sensitive data can unintentionally leak information about individuals in their training set. The main attack covered is membership inference, which exploits differences in how models behave on seen versus unseen data.

Within the path, this module introduces a more defensive angle. It explores both how privacy attacks work and how techniques like differential privacy can be used to mitigate them.

#### Review & Experience

> **TL;DR**  
> ⭐️☆☆☆☆  
> Important topic, but frustrating and easily the weakest module in the path.
{: .prompt-info }

At this point in the path, it makes sense to introduce some defensive content, and AI privacy is an important area. Unfortunately, this module did not work well for me.

The concepts are heavy and not easy to grasp, especially if you do not already have experience with differential privacy. The skills assessment was particularly painful. Retraining models multiple times takes a long time, which makes experimentation slow and frustrating. Even when you understand what needs to be done, the feedback loop is poor.

By the time you reach this module, you are already deeply invested in the path, so quitting is not really an option. You push through it because you have to, not because it is enjoyable. For me, this was the weakest module of the entire path.

#### Tips
- Run everything on your own machine, Pwnbox is not a good option here
- Expect long training times and plan accordingly
- Focus on understanding the high level ideas rather than every detail
- Be patient with the skills assessment, it is more about persistence than creativity

### **AI Defense**

|  Tier   | Difficulty | Category | # of sections | Estimated time |
| :-----: | :--------: | :------: | :-----------: | :------------: |
| Tier II |   Medium   | General  |      21       |     2 days     |
{: style="margin-left: auto; margin-right: auto; display: table;" }

![AI Shield](assets/img/posts/ai_rt_htb/not_yet.png){: w="350px" }
_[Check my AI Shield badge here](https://academy.hackthebox.com/achievement/badge/f1dd6619-e4c5-11f0-9254-bea50ffe6cb4)_

#### What's this about?
This module focuses on defensive strategies for AI systems. It covers techniques to mitigate attacks like evasion, data manipulation, and prompt injection. You learn both model-level defenses, such as adversarial training and tuning, and application-level protections like LLM guardrails.

Within the path, this module provides a practical understanding of how AI systems are protected in the real world and rounds out the offensive modules with defensive perspectives.

#### Review & Experience

> **TL;DR**  
> ⭐️⭐️⭐️⭐️⭐️  
> Valuable and informative, though some exercises are frustrating.
{: .prompt-info }

This defensive module is very useful. It explains how adversarial training, adversarial tuning, and LLM guardrails work and why they matter. The content takes time to go through but is well presented and keeps you engaged.

The skills assessment can be a bit frustrating. I could not complete it as quickly as I wanted, especially if you try to run everything yourself, but it allows you to apply the knowledge gained from both offensive and defensive modules.

Overall, this module is a strong conclusion to the path. It helps you understand how AI systems are secured in practice and provides a satisfying bridge between attacking AI and defending it.

#### Tips
- Focus on understanding the concepts rather than running all code snippets
- Powerful hardware helps if you want to experiment with adversarial training
- Take your time with exercises, the value comes from understanding not speed
- Think of this module as a way to consolidate offensive skills with defensive knowledge

## **My Path Experience**

Going through the **AI Red Teamer Job Role Path** was, overall, a very positive and eye opening experience. It is not a perfect path, and it is definitely not an easy one, but in terms of value for money and breadth of coverage, it is the best structured resource I have found so far for learning AI red teaming in a practical and realistic way.

The path starts slowly, with strong theoretical foundations in AI and machine learning. While the early modules can feel detached from red teaming at first, they do a good job of aligning everyone to a common baseline. This becomes important later, especially when dealing with more complex topics like data poisoning, evasion attacks, and model level defenses.

Once you reach **Introduction to Red Teaming AI**, the path really finds its identity. Modules like **Prompt Injection Attacks**, **LLM Output Attacks**, and **Attacking AI Application and System** are where the path truly shines. These sections feel relevant, modern, and close to what you would encounter in real AI enabled applications. The MCP module in particular stands out as one of the most enjoyable and valuable parts of the entire path.

Not every module landed equally well for me. The **AI Evasion** series and **AI Privacy** modules are very math heavy and can be difficult to digest, especially if your background is more focused on cybersecurity than on machine learning or optimization theory. While the content is technically solid, these sections require patience, strong Python skills, and often external help to get through efficiently. They are important topics, but not always enjoyable.

The defensive modules help balance things out. **AI Defense** does a good job of showing how AI systems are protected in the real world and ties together many of the attacks introduced earlier in the path. Even when some exercises become frustrating, the overall learning value is there, and it feels like a natural way to close the journey.

From a practical standpoint, this path rewards hands on work and curiosity. Running labs on your own machine is often necessary, and relying on LLMs as coding and learning assistants is almost mandatory for staying efficient. Some modules are dense, some are slow, and a few feel more academic than offensive, but taken as a whole, the path delivers a realistic view of AI security.

In the end, I would absolutely recommend this path to penetration testers and security professionals who want to seriously understand AI attack surfaces. You will not master every topic, and you will not enjoy every module, but you will come out with a much clearer mental model of how AI systems fail, how they are attacked, and how they are defended. For the price and the depth it offers, this path sets a very high bar for AI red teaming training.

![AI Ninja](assets/img/posts/ai_rt_htb/ai_ninja.png){: w="350px" }
_[Check my AI Ninja badge here](https://academy.hackthebox.com/achievement/badge/f1dfcf2f-e4c5-11f0-9254-bea50ffe6cb4)_

## References
- Help from [HTB's Discord](https://discord.com/invite/hackthebox)
- Help from my colleague [Diogo Lino](https://www.linkedin.com/in/lino-diogo/)
- AI (Perplexity, ChatGPT, local AI models, etc.)

<br>
> Wanna talk? Contact me here!  
> <a href="javascript:void(0);" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" onclick="navigator.clipboard.writeText('diegofdlg@gmail.com');alert('Mail copied to the clipboard!')"><i class="fa-solid fa-envelope"></i></a>
> <a href="https://www.linkedin.com/in/diego-fidalgo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-linkedin"></i></a>
> <a href="https://x.com/0x_ch3ngo" style="font-size:1.2rem; margin-right:0.8rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-x-twitter"></i></a>
> <a href="https://discord.com/users/149616662537043969" style="font-size:1.2rem; margin-top:1rem;" target="_blank"><i class="fa-brands fa-discord"></i></a>
{: .prompt-tip }
