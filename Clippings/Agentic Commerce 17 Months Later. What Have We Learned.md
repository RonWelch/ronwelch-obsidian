---
title: "Agentic Commerce: 17 Months Later. What Have We Learned?"
source: https://x.com/jas_shah/article/2118672498166215140
author:
  - "[[Jas Shah (@jas_shah)]]"
published: 2026-11-06
created: 2026-11-10
description: What were my thoughts on Agentic Commerce in June ‘25 and what has changed?What is Agentic Commerce?Why do I think it’s a no brainer for mer...
tags:
  - clippings
  - agentic
  - commerce
  - BUGuestLecture
---
![Image](https://pbs.twimg.com/media/HWYt5dMWEAArW2J?format=jpg&name=large)

- What were my thoughts on Agentic Commerce in June ‘25 and what has changed?
- What is Agentic Commerce?
- Why do I think it’s a no brainer for merchants?
- What’s The Klarna Effect and why does it apply to AC?
- What does the Agentic Commerce Stack look like?
- What problems do Agents solve for consumers?

**These questions and more will be answered in this week's edition.**

**Disclaimer: The meat of this was published last year and a refreshed deep dive will follow. Head to my Substack (Fintech: Under the Hood) to read more like this, and let me know if you want to see more insights published here.**

Hey Fintechers and Fintech newbies 👋🏽

17 months ago, just after an AI heavy Money 20/20 EU, I wrote a short dive into agentic commerce.

At the time, there was a lot of hype around agents taking over commerce and doing everything for consumers.

We’ve since seen Stripe and OpenAI launch the Agentic Commerce Protocol, a growing number of AI agents capable of making purchases, and more recently, Stripe Link Balance, which lets consumers allocate money for agents to spend on their behalf.

But recently fintech Twitter/X has had some things to say about some of the realities of an agentic commerce world.

- [Vignesh](https://x.com/vigstar1) from Meta dove into Link Balance as well as some of the economic hurdles yet to be mapped.

> Nov 1
> 
> thank you to everyone in the past week who took the time to share their views and observations on the space. understanding the broader ecosystem is critical to scaling agentic commerce. can't wait for us all to build a future we can be proud of!

- [Scott Wessman](https://x.com/scottew) with a plea to make agentic payments simpler

> Nov 1

- And probably my favourite rant which came from [Maia Bittner](https://x.com/maiab), who gave the hype and promise of hyper-convenient agentic commerce a strong dose of reality.

> Nov 3
> 
> re: agentic shopping I have NEVER in my life heard so much hype and discussion about a tech/product experience that basically does not work at all for me. I feel like I’m using theranos

As there are velocity of practical implementation conversations has increased, I thought this would be a good opportunity to revisit some of my original thinking.

Particularly three predictions:

1. **Payment processors would be among the biggest winners in agentic commerce.**
2. **Consumers wouldn’t want to surrender complete control over purchasing decisions.**
3. **Agentic commerce wouldn’t kill checkout. It would fundamentally change it.**

Some of those predictions are holding up well and others need a little more nuance.

There are a bunch more questions that have come up in my thinking over the past few months like:

- Where does payment method optimisation move to in an agentic world? i.e. what method to use when, and why to get the best for the merchant and consumer
- Who is solving the “unhappy paths” for agentic of which there are many
- Can we use the shift to agentic commerce to solve some of the problems payments already have around things like cross-border digital commerce, and consumer rewards without adding more complexity?

The list goes on A LOT longer.

I’m going deeper on all these questions in a future edition along with a clearer view of what a workable agentic stack looks like and which ‘Jobs’ are still ‘to-be-done’.

For now, I’m revisiting my original deep dive from June 2025, including how agents work, the problems they solve for consumers, and what the agentic commerce stack might look like.

**Here’s what to expect:**

- **What is an Agent?**
- **How does an Agent work?** Perception Reasoning Execution Feedback
- **The challenges consumers face** Choice Overload Lack of Personalisation Decision Fatigue
- **The Klarna Effect and how Agents can help merchants**
- **The Agentic Commerce Stack**
- **Interesting News 🗞:** Stripe Treasury and Plaid powers connectivity in Anthropic

**Now let’s get Under the Hood 🔎**

## What is an AI Agent? 🤖

An AI agent is an autonomous software entity that can perceive, decide, and act on behalf of a user OR organisation, often with minimal human intervention.

Unlike traditional automation, which follows static rules or prescription workflows, an agent is goal-oriented and adaptive. It can take inputs (like user preferences, real-time data, or external signals), reason about the best course of action, and then execute tasks across systems or platforms.

Think of it like this:

- A **search engine** finds you options.
- A **recommendation engine** suggests one.
- But an **agent** goes a step further. It books the ticket, negotiates the price, applies your loyalty points, and sends a calendar invite without you needing to ask twice.

And Agents can be:

- **Reactive** - Responding to user prompts
- **Proactive** - Anticipating needs and acting ahead of time OR
- **Collaborative** - Working with multiple agents or humans toward a shared objective.

## How agents work

An AI agent operates through a continuous loop of perceiving, reasoning, and acting, and the best way to see the value and broad application of AI Agents across use cases is to map the logic steps they go through, starting with Perception or Input.

**Perception 💽**

Agents sense context through APIs, sensors, or data feeds, including things like:

- **User preferences**, e.g. price guidelines, brand preferences, etc
- **Contextual signals**, e.g. location, calendar, time of day, account balances
- **External data sources**, e.g. product availability, price changes, interest rates, account data, payment methods

It also obtains the users' goal or infers it from past behaviour. Things like, “do my weekly shop for the cheapest price”, or “buy my favourite chinos twice a year”.

**Reasoning 🧠**

The agent uses AI, often a mix of machine learning, rules-based logic, and large language models, to:

- Analyse options
- Compare trade-offs
- Predict outcomes
- Make a decision that best aligns with the user’s goal

It may also ask clarifying questions or collaborate with other agents to complete a task.

**Action/Execution 💪🏽**

The agent takes action:

- It might purchase a product, shift funds, send a notification, or trigger another system.
- It does so via APIs, integrations, or its own internal capabilities.

**Feedback & Learning 👂🏼**

And finally, the bit that everyone forgets about, but one of the most crucial steps it observes outcomes after the action:

- Did the user approve?
- Was the goal met?
- Should the decision logic be updated?

![Image](https://pbs.twimg.com/media/HWYwBt-WMAAexR7?format=jpg&name=large)

**When you take a broader view and look at the process an AI agent goes through, you begin to see the applications of agents across several areas:**

- **Personal finance:** Agents that optimise your cash flow or move money across accounts based on live goals
- **SME operations:** Agents that manage inventory, payments, and supplier coordination
- **Financial Management:** Agents that manage a company’s finances and basically replace the concept of SaaS CFO-as-a-service
- **Consumer spending:** Agents that understand your household budget and subscribe to essentials at the best terms

There are many, many more, some of which I’ll dive into in future editions, but this one is, of course, about Agentic Commerce.

It’s clear that an agent would add value to the ecommerce experience, but what problems could they ACTUALLY solve for consumers, and what do consumers care about?

## The Challenges Agents Solve for Consumers

The e-commerce experience has definitely improved over the past decade, but just like in banking, where there’s a lot more choice, and everything is shifting to digital, that ends up creating different challenges for consumers.

**1\. Overload of Choice 🛍🛍🛍**

One positive of the shift to digital is brands are able to show more options where previously they were limited to how much floor space they had.

This has led to pages like this where there are so many options that the company resorts to creating an infinite scroll feed, or there are 10s of pages of 100s of options in each.

![Image](https://pbs.twimg.com/media/HWYwLweWIAITufW?format=jpg&name=large)

It means customers have too many choices, and too many options mean less clarity and endless scrolling, comparing, and researching. This is bad for the merchant and also bad for payment processors who lose processing volumes.

> 🧠 AI Agents help overcome this choice overload and can present a limited set of relevant options that customers are more likely to execute on. This can increase conversion for merchants and TPV for payment processors and lead to a better overall experience for the customer.

N.B. I actually wrote about the paradox of choice and its relevance to fintech in this edition, so you learn more about that here…

**2\. Lack of Personalisation 🤷🏼🤷🏼🤷🏼**

Another side effect of the e-commerce shift that many thought would be solved in the new digital age is the lack of personalisation. Platforms like Spotify promised AI-powered recommendations with high conversion, but as with many areas like music, travel, and retail, preferences recommendations aren’t just about surfacing options through a “You bought this, so you’ll probably like this similar item” method. These end up being generic and have low usage rates.

Because many of these offers are via email, they also feel irrelevant, and as they are not connected to shopping behaviours across stores (they are often limited to specific brands like ASOS, Shein, Zara, Lululemon, etc), they are often mistimed.

> 🧠 AI Agents can take personal data, shopping habits, and recently purchased items from SKU-level data and leverage user personas to create truly timely personalised recommendations during the e-commerce experience and via notifications/nudges in retail apps.

**3\. Decision Fatigue 🔴 🔵**

This is the area that has seen the most fintech disruption but has seen the amount of choice at the point of execution also lead to more decision fatigue.

On one level, more choice is great as it leads to greater competition and competitive pricing for consumers, but a side effect of that is having so much at the point of execution, like having to dig out a discount code, typing in a shipping address, picking a payment method from the 10 available, and so on, leads to decision fatigue and drop off at the most crucial last mile of the journey.

Think about the Red Pill and the Blue Pill choice in The Matrix.

![Image](https://pbs.twimg.com/media/HWbdJUPWcAAQTGd?format=jpg&name=large)

Based on all the factors, it’s a simplistic decision (the reasoning is complex, but the ultimate decision is simple).

**Red Pill or Blue Pill.**

Now imagine a scenario where there are 5 different options, each with a slightly different pathway. The decision becomes far more complex, takes longer and has greater potential for abandonment. The same principle can be applied to the checkout page.

> 🧠 AI Agents can use historical consumer behaviour, identity information, and payment preferences, automatically apply discount codes, and autofill lengthy personal information forms so that the decision for the customer becomes binary. Complete the transaction or cancel the transaction.

## The Klarna Effect 🛍

There’s a reason there’s a lot of buzz around Agentic Commerce. It’s not just because it solves some problems for consumers.

When I was on stage at London Tech Week (humble brag) talking about embedded opportunities in financial services, I spoke about the golden triangle of a successful embedded proposition.

Me on a panel talking about Embedded X in fintech

![Image](https://pbs.twimg.com/media/HWbdVNgWsAAx-XP?format=jpg&name=large)

That’s where the **embedder** of the product gets something (maybe access to a larger customer base), the **embedee** gets something (a digital journey or a boundary-pushing product to offer customers), and the **customer** gets something (a better product and/or experience).

It’s a Win-Win-Win scenario, and those are the embedded partnerships that tend to be successful as there is no imbalance of power and the customer benefits.

With Agentic Commerce, I see a similar 3xWin scenario that we saw when BNPL became mainstream where **merchants** were able to see tangible increases in basket sizes and conversion, **customers** were able to purchase items and spread the cost over longer periods of time and **BNPL providers** took a slice of the basket they’d help convert or increase.

In this case, **merchants** still get tangible increases in conversion, **customers** get more tailored, low admin experiences and products, and the companies that win will be those building or facilitating the building of AI Agents.

I do have a theory on they type of company that’ll be but first I want abstract and talk about the characteristics of the enablers of Agentic Commerce.

## Characteristics of the Winners in the Agentic Commerce Game

Based on how agents work, it’s fairly logical to map the characteristics and features of those organisations that will be successful in the Agentic Commerce world, and there are three that are particularly important.

**1\. Identity**

**Why?**

Understanding the consumer is foundational to building an effective agent. The agent needs to know who you are, what you value, and how you behave.

Organisations that already hold verified identity profiles and, crucially, understand the behavioural context (spending, location, life stage) have a significant head start. They can preload preferences, limits, and goals into an agent without requiring long onboarding journeys.

**2\. Historic & Real-Time Data**

**Why?**

Agents learn and improve by observing past patterns. Players with deep, structured behavioural data (spend patterns, browsing habits, category affinities, etc.) can build agents that are more accurate, faster to personalise, and better at anticipating user needs.

Without this history and access to real-time data, the agent has to start from scratch, and users are unlikely to tolerate long learning curves.

**3\. Tokenization Processing & Payments Infrastructure**

**Why?**

Agents need seamless, secure access to act on behalf of users, and that means being able to pay without friction. Players with tokenization processing capabilities and payment infrastructure can empower agents to transact safely and instantly without requiring repeated authentication.

Tokenization also allows for agent-led microtransactions, subscription adjustments, and rule-based spending, which are key features in agentic commerce.

Based on these desirable characteristics, I believe it’ll mainly be payment processors like Stripe, PayPal, Adyen and [Checkout.com](https://checkout.com/) that’ll lead the charge with Agentic Commerce with their access to customer identities through one-click checkout functionality, payment processing capability, usage of Visa and Mastercard’s Payment tokens (allowing for agentic, programmable payments) and access to enterprise data at scale.

I also think organisations like Plaid, who have network-powered identity, fraud, and data capabilities, with learnings from billions of data points, will be another dark horse in the Agentic development space as Identity and Data are two of their superpowers (I can see a world where a PP partners with Plaid to build agents for merchants).

From the payment processors, there has already been some progress.

Stripe has an [agent SDK](https://docs.stripe.com/agents?locale=en-GB)

Paypal has an [agent toolkit](https://developer.paypal.com/tools/agent-toolkit/)

…and others are mobilising.

![Image](https://pbs.twimg.com/media/HWbfD1tXcAEO85l?format=jpg&name=large)

## The Agentic Commerce Stack 🔍

Of course, I have a nice visual of what the Agentic Commerce stack will look like, which includes key players who fill roles in the building and running of AI Agents for commerce, and based on some of the companies already enabling this new world through vital infrastructure.

![Image](https://pbs.twimg.com/media/HWbfJISXsAAXx6y?format=jpg&name=large)

My quick sketch of the Agentic Commerce stack ~June 2025

**NOTE: This diagram will be updated in the next edition with the latest consumer, payments, and AI (Personal Agents of course), with more detail on where Link fits.**

There’s a glaring inclusion in the diagram that others aren’t talking about, which is the customer as the ultimate decision maker on the right-hand side.

I’m highlighting this for two reasons.

**1.** It’s not clear what the regulatory ramifications of an agent, even with tokenized payment information, making a payment on a customer's behalf will be. I think this will mean the final authentication will still remain with the customer.

and

**2.** All the proposals of agents searching for goods and making payments on a customer's behalf ignore one key element.

**Choice**.

(yes, another matrix reference incoming)

One of the core threads running through The Matrix is that, even in a world of AI where you’re directed down a particular path, the choice is what makes us human and that power of choice should ultimately remain with humans, and in this case, the consumer.

Giving agents agency is one thing. Letting them make the ultimate decision is another matter altogether. Only the One can make the final choice 😉 (the One being the customer, of course).

![Image](https://pbs.twimg.com/media/HWbfM90WQAAbEBS?format=jpg&name=large)

💡 The Matrix references are very relevant to the discussion around AI because the entire movie is based in a lot of sound computer science principles and many of the references to Agents, Archtects, and constructs have been lifted from computer science text books. It's why it's one of my favourite movies and also because it made being a programmer cool at a time when kids were telling me writing code isn't cool.

One other hypothesis I have is how it’ll impact the checkout page.

I believe the customer will still ultimately want to have the final say, even if that is just a simple click or Face ID, or whether that’s pre-authorisation of an amount to spend in a specific category, MCC, merchant or workflow, so Agentic Commerce won’t kill the checkout page. It’ll evolve it.

The future of commerce is agents working in the background to find the best deals, auto applying discounts, and filling out relevant payment information, but the origination will move from direct searches for brands to shopping via TikTok and purchasing directly through influencers content, finding goods through AI search engines, and automatic suggestions as soon as you land on company websites.

So no, the checkout page won’t be killed by Agentic Commerce. But it will move, probably to social media and AI search.

Agentic Commerce will be transformative, and even more so than Klarna was to basket conversion.

**The even bigger Agentic opportunity is in finance.**

SMBs have so many hurdles in obtaining finance, and a Finance Agent that scours all finance options, pulls in identity & KYB data, takes in the director's preferences, then sources & presents those options to decision-makers would transform the SMB finance industry.

That’s what I’ll cover in a future edition.

In the meantime, keep your eyes out for more agentic releases. My eyes are on the mortgage industry next…If I were a mortgage broker right now, I’d be very, very afraid.

That’s it for this slightly leaner edition. I hope you enjoyed it, and yes it probably has raised more questions than answers. Questions like, “How can Agents be leveraged in the investing experience?”, and “Does an Agentic with access to my bank accounts means there is no need for a PFM app?”. I will answer some of these soon. 😊

# The Agent of Change

In terms of what have we learned, I think many have learned that you can’t just “one-shot” a payments protocol for agents and expect it to be perfect first time. Every new innovation has gone through an early hype cycle with exploration, and then a settling down after some experimentation and calibration to a solution that truely solves the problem. We’re not there yet, but we are in the right direction, and during this period opinions are allowed to change.

Remember when folks were saying “cards are dead” for example?

One opinion that I still hold very strongly is that Agentic X (payments, finance, lending) moves the needle waaaay more for SMEs than it does for consumers.

For consumers, agents promise convenience.

For SMEs, they promise capacity.

The ability to automate the endless operational tasks, juggle disconnected systems, manage cash flow and get things done without hiring more people.

**For consumers, agentic AI gives us back time. For SMEs, it could fundamentally change the economics of running a business.**

There are a tonne of projects I’ve finished recently that give me that signal but before doing write ups on the SME angle to agentic, that there’s a lot more to explore on the consumer side:

- What do merchants need to do to make agentic commerce work, for example, what’s required for Maia’s agent to be able to transact with Sephora and solve for those unhappy paths?
- How do consumers manage consent, create rules, and a proper vault with limits, preferred payment methods (if you’re an industry vet, you get what I’m hinting at and I’ll reveal my next agentic deep dive)?
- To Vignesh’s point, what’s the economic model here in this hybrid world of stable + card rails? Or will this all shift to stable and was this the strategy all along?

I’ll get to these in typically under the hood fashion in the next few weeks, along with an update visual on what an agentic stack that solves at least some of these challenges will look like in 2027. And a deeper dive into Link

Unless the AI bubble bursts first. In which case, get ready for my next edition on why I think COBOL, on-prem servers and AS/400 terminals are the future of finance. 😜

> That’s it from me. I hope you agentic-retro edition👋🏽

> Remember to hit the thumbs up below, drop a comment (feedback, things I might have missed, or just a “nice job”) and share it with a friend or colleague. It helps get the word out. 😊

**See you next week 👋🏽**

**Jas.**

# Interesting News 🗞️

## Stripe brings Treasury to the UK

Stripe is moving further into business banking territory, launching **Stripe Treasury in the UK**.

UK businesses can now receive, hold, convert and spend money directly from their Stripe Dashboard, including:

- Multi-currency accounts supporting GBP, EUR and USD.
- Payouts to 160+ countries.
- Virtual and physical cards with 1% cashback on eligible purchases.
- Instant access to settled revenue.
- AI agents that can monitor balances, transactions and cash flow (public preview).

**An interesting stat:** Over half of UK startups that launched on Stripe in 2025 were processing cross-border payments within three months.

## Plaids ‘completes the set’ with Anthropic

Plaid now powers bank account connectivity for nearly every major AI lab (after becoming the bank account connector of choice for Anthropic), quickly emerging as the key trust and intelligence layer enabling contextualized, AI-powered financial conversations to happen securely at scale.

- Millions of people already turn to Claude for financial guidance.
- Anthropic joins more than 800 AI companies building on Plaid.
- In the U.S., nearly two-thirds of people connecting bank accounts to AI assistants through Plaid had never used a Plaid-powered personal finance app before.

Here’s what Plaid CTO Will Robinson had to say about their latest partnership:

Millions of people around the world use Claude, and personal finance is among the most common areas where they turn to it for guidance. Now, people can work through everyday tasks in Claude, like managing a budget or creating a strategy to pay off debt, with their real numbers in front of them.

Oh, and if you want to hear more from Will on his thoughts on Plaid and his predictions, check out this pod I recorded with him earlier this year 👇🏽

## 🎙️ Plaid CTO, Will Robinson: Shaping the future of financial infrastructure, Plaid’s north star metrics, efficient innovation flywheels, and AI (transaction models, MCP, & Agents)