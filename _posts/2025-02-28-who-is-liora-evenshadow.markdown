---
layout: post
title: Who is Liora Evenshadow? Your custom offline $0 cost AI whisperer that knows your secrets.
date: 2025-02-25 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: RAG.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [LMStudio, AnythingLLM, IDE, LLM, ContinuePlugin]
---

Initially published on [*Medium*](https://lmstudio.ai/)

>  **You don’t call it.** You just type your sentence, and when the cursor stops, suddenly, the next line appears by itself, as if someone already knew what you were going to write.
>  It is here. It is always here. No network. No server. It knows what you need. It knows **only** what it should know — **and it will never tell anyone.**

There are already plenty of articles about various tools, but I would like to share my invaluable experience of integrating solutions like [**LM Studio**](https://lmstudio.ai/) and [**AnythingLLM**](https://github.com/Mintplex-Labs/anything-llm) (along with *VS Code IDE* and the [Continue plugin](https://www.continue.dev/) ). This experience could be highly useful for a wide range of potential users, including **beginner business analysts, testers, support engineers**, and others…
>  What I want? **I want a local LLM that I can instantly feed with interesting to me knowledge without the need for expensive training.**

Among the vast number of existing online tools, there is often no **AI-based solution** that fully meets all your needs.

Now, imagine you are a beginner, but you want to provide an **AI tool** with a bit of context — such as a **set of documents** for analysis and a **specialist’s instruction manual** in the form of one or multiple documents — to help it:

* **Conduct business analysis** of requirements based on the given context.

* **Design test cases** using both the provided context and specific expertise from the instruction manual.

* **Perform static code analysis** to identify vulnerabilities according to your unique security guidelines.

How do you think the **ideal solution** capable of handling any of these tasks should look like?

Here’s the set of requirements that, in my view, brings us as close as possible to the perfect solution for my use cases:

 1. **Security** — You don’t want to send sensitive case data over the internet to a third-party provider.

 2. **Local access** — You want to interact with the model on your local computer or within your isolated intranet or private virtual network.

 3. **Dynamic knowledge** — You need the model not only to rely on its pre-trained knowledge but also to quickly acquire information about new contexts relevant to you.

 4. **No costly training** — You don’t have the resources to invest in expensive machine learning or mastering its complex methodologies.

 5. **Instant updates** — You dream of a setup where any useful material you come across — be it online or a book stored on your local drive — can instantly be transferred into your model’s knowledge.

 6. **Flexible model selection** — You want the freedom to choose any model that best suits your task and hardware.

 7. **Easy deployment and integration** — You need to have unified deployment way to replicate it and to support anywhere.

 8. **Cost** — solution is better to be non-expensive or free.

Below is an example of my journey of assembling solution architecture from existing tools.

![](https://cdn-images-1.medium.com/max/4360/1*2B7l7IqojD_sk2JjsSrUow.png)

As precondition I need some LLM runtime which supports RAG 
*RAG (Retrieval-Augmented Generation) is an AI framework that enhances text generation by retrieving relevant information from external sources before generating responses, improving accuracy and reducing hallucinations.*

Then…

 1. [**LM Studio**](https://lmstudio.ai/) — has it’s own RAG but it requires some additional work to make it usable outside the [**LM Studio**](https://lmstudio.ai/) UI chat itself. The feature I need to take it from is ability to download any preferred model and spin it up locally.

 2. [**AnythingLLM**](https://github.com/Mintplex-Labs/anything-llm) — amazing tool:
    1. It’s able to use runtimes from a range of providers including our local LM Studio one. 
    2. Has it’s own vector DB and bundle out of the box features that handle RAG accurately.
    3. Has useful long memory option and ability to organize RAG context into different workspaces.
    4. Equipped with ability to fill in RAG context from files, folders and even WEB-page via Browser extension.

 3. Docker — to spin up [**AnythingLLM**](https://github.com/Mintplex-Labs/anything-llm) with API-server enabled.

 4. [*VS Code IDE *](https://code.visualstudio.com/)and the [Continue plugin](https://www.continue.dev/) to research if the RAG knowledge will be available to access context-based knowledge via [**AnythingLLM**](https://github.com/Mintplex-Labs/anything-llm) API-server. Of course the [@Codebase](https://docs.continue.dev/customize/deep-dives/codebase) *Continue plugin* indexing mechanism is enough in a lot of cases but I want to have a real and flexible RAG on top of a base.

So lets begin with deployment/integration/verification scenario…
- Run ‘deepseek’ based LLM inside LMStudio as server ([**HOW TO**](https://lmstudio.ai/docs/api))
![](https://cdn-images-1.medium.com/max/2000/1*xsoSL3Y9nptzokUQ7d09pg.png)
- Run Anything LLM inside Docker engine ([**HOW TO**](https://docs.anythingllm.com/installation-docker/local-docker)).
- Correctly set up LLM Provider
![](https://cdn-images-1.medium.com/max/2000/1*Nj-YRUcFXh3qR9BZxjD08A.png)

- Create new workspace with name Test
![](https://cdn-images-1.medium.com/max/2000/1*IV8Pww3m5vNdLJSCxc089g.png)

- Enable API access with API-key ([**HOW TO**](https://docs.useanything.com/features/api))

- Let’s do some **reverse engineering** to extract the **workspace ID** we’ll use and enriching with new knowledge from: 
🔗[**http://localhost:3001/api/workspaces**](http://localhost:3001/api/workspaces)
- Grab Open AI Compatible endpoint root from:
🔗[**http://localhost:3001/api/docs/#/**](http://localhost:3001/api/docs/#/)
- Install [*Continue plugin*](https://www.continue.dev/) in the [VS Code IDE](https://code.visualstudio.com/) and adjust plugin *[config.json]* configuration:

```
    {
      ...
      "models": [
        {
          "apiBase": "http://localhost:3001/api/v1/openai/",
          "model": "test",
          "title": "AnythingLLM",
          "provider": "openai",
          "apiKey": "F2J195C-2R0MVP0-QYDH124-50WKQV0"
        }
      ],
      ...
    }
```
- Install [AnythingLLM Browser Companion](https://chromewebstore.google.com/search/AnythingLLM%20Browser%20Companion) extension to your browser. We will use it to quickly verify our RAG-equipped instrument is really able to access and operate with some unique information.
- Create some unique information and publish it to the WEB (you can simply use any local document you prefer. WEB example is to just for some clarity of illustration. In my case I just published [Google Spreadsheet](https://docs.google.com/spreadsheets/d/e/2PACX-1vTTh01ofUemqi1jNGk_V0Ktk4f6fiIgob2PqdoGRijVE_gJiEVAosbXiGPEseB7EyV7vBVw6lialYqF/pubhtml) contains information about fictional character **Liora Evenshadow**)

![](https://cdn-images-1.medium.com/max/2000/1*ADcpHWM1y8Oe-JpKvYrRhA.png)

**— — — — — — — — — —== NOW THE MAGIC BEGINS == — —— — — — — — —**
- Embed information about our character into workspace

![](https://cdn-images-1.medium.com/max/2000/1*8yBrT0beKtoM2HdDAV-inA.png)

### Who is Liora Evenshadow? ###
Let’s try to give this answe just inside the VS Code Continue plugin chat

![](https://cdn-images-1.medium.com/max/2384/1*ANZqgfoU7zWtuz0jx_DjXw.png)

## Summary

I hope you don’t find this article too **superficial**, as the integration of tools like **LM Studio** and **Anything LLM** may seem obvious to some users. However, my goal was simply to convey the idea that **ready-to-use solutions** already exist — solutions that can be seamlessly integrated into an infinite number of workflows, making them accessible to specialists of all kinds and “calibers”.

In my case, this resulted in a **fully functional and secure offline LLM** with **zero cost** — that one who knows **who Liora Evenshadow is… and a lot of my other secrets.** 😉

## P.S.

This **wasn’t supposed to work out of the box… but it did.** 🚀🚀🚀

## What was I missing in AnythingLLM?

Maybe I just didn’t find it in the official documentation, but one thing remains unclear to me — **how can I manage workspace contexts through OpenAI API-compatible endpoints?** Ideally, I’d like to build a **flexible system** where different **contexts** are routed to different **groups**, allowing that knowledge to be separated **at the client level (**IDE plugin ****in my case**).**

But that’s another time story… One thing is definitely clear!

### AnythingLLM+LM Studio is a game-changer combination to me for today. 🚀🚀🚀

*Solutions investigated: LMStudio, AnythingLLM, Ollamа, Jan, GPT4All, LocalAI, (n) of IDE plugins. 
Illustrations: app.cloudcraft.co + black-forest-labs/FLUX.1-schnell*
