---
title: Agentic Commerce for Dummies
source: https://agentcash.dev/blog/agentic-commerce-for-dummies
author:
  - "[[David Pawlan]]"
published: 2026-07-14
created: 2026-07-16
description: Learn what agentic commerce is, how x402 and MPP enable AI agents to buy online, why stablecoins matter, and how businesses and users can participate.
tags:
  - clippings
  - agentic
  - ArtificialIntelligence
  - BUGuestLecture
  - x402
---
A plain-language guide to agentic commerce: how AI agents discover, buy, and pay for goods and services using x402, MPP, stablecoins, and AgentCash.

What is Agentic Commerce?

A plain-language guide to the third evolution of commerce, and how to participate in it.The Cheat Sheet (if you read nothing else)

- Agentic commerce = your AI agent buying things for you on the internet.
- It runs on payment protocols: [x402](https://x402.org/) (from Coinbase) and [MPP](https://stripe.com/blog/machine-payments-protocol) (from Stripe and Tempo). A business wraps an API with one of these to open itself up to agents, the same way it once added credit-card checkout.
- It's open. Anyone can list a service and start selling immediately. See who has, at [x402scan](https://www.x402scan.com/) / [mppscan](https://mppscan.com/).
- Tiny, one-cent purchases (microtransactions) run on stablecoins, because card fees make them impossible. Larger purchases can still use a card.
- To try it yourself, your agent will need a wallet.

## About this guide (and the "Foolish Assumptions")

If you’ve been hearing the phrase "agentic commerce" and quietly nodding without fully knowing what it means, this is for you. You are not a dummy. You’re just early, which is the best spot to be.Here is everything I’m assuming about you:

- You know what the internet is
- You have purchased something online
- You have heard the words "AI agent" thrown around

That’s it. Every other term, I’ll define as we go. You can read this top to bottom, or jump to any section you want. Nothing here requires the section before it.Full disclosure before we start: I work at [Merit Systems](https://merit.systems/). We live and breathe agentic commerce. It's our goal to lead the innovation wave of this space. If you have an interest in agentic commerce or want to get involved as we sit at the infancy of the industry, please reach out to me. I'd love to take you out for coffee in NYC and geek out on agentic commerce.Anyways, let's dive in.

## What agentic commerce actually is

Agentic commerce is just agents discovering and purchasing goods or services, both physical and digital, on behalf of humans. Think of it as the next evolution of commerce:

- First, all commerce was in person. You went to the store.
- Then it evolved into e-commerce. You used the internet to shop.
- Now it’s agentic commerce. Your agent does the shopping for you, on the internet.

> 🧵Remember: In one line, agentic commerce is the proceess of agents discovering and purchasing goods and services on your behalf. That sentence is the whole idea.

## First, what is an AI agent?

An AI agent is software that acts on your behalf. It is lines of code that run a program capable of executing tasks and producing outcomes for you. When you tell it to go do something, it goes and does it (i.e. write me an email reply to XYZ). The cool part about AI agents is that we live in a world where you can create agents without personally writing a line of code. Tools like Claude Code or Codex allow us to program insane things with natural language.

## Why this couldn’t exist until now

Agentic commerce was not possible before because there wasn’t an easy way for agents to buy and sell goods and services.Look at how the internet is built today. In almost every case, you have to create an account or buy a subscription before you can transact with an online service. So if you wanted your agent to go buy something from a store, or find someone's email address, first it would have to do one of these:

- Create an account with name, email, and phone number
- Input your credit card information
- Purchase a whole subscription just to get API access
- Configure an API manually

The interface of the internet was built for humans, with beautiful websites and buttons that we could click on. That made complete sense when humans were the main vehicle of commerce, but it starts to break down the moment agents are the ones doing the work for you. When you tell your agent to go buy something or find an email address, you don’t want it stopping to create an account, buy a subscription, and configure an API key for every single service it touches.You just want it to get in, do what it needs to do, and get out.

## The internet today is a Costco. Agentic commerce turns it into a mall.

The subscription economy is a little predatory. You create accounts, you buy subscriptions, and you get locked into each merchant's rules. The real world does not work that way. When you walk into a mall, you do not create an account or buy a subscription to grab a T-shirt from Nike. You walk in, go to the store you want, buy what you want, and leave.That’s what agentic commerce enables. It’s an open marketplace where the incentives finally align with you, the end user. Agentic commerce is turning today's internet–which behaves like a Costco or Sam's Club–into a traditional mall.

> 💡Tip: Whenever agentic commerce gets confusing, come back to the mall. Walk in, buy the one thing, walk out. No membership card. That’s the entire user experience we are building toward.

## How it works: payment protocols

For an agent to actually pull this off, a few technologies have to be in place. The first is a payment protocol. It is how an agent knows what is available and what to pay for.[Coinbase](https://www.coinbase.com/) built x402. [Stripe](https://stripe.com/) and [Tempo](https://tempo.xyz/) built MPP. These payment protocols let your agent pay any site that accepts them, one request at a time rather than a subscription. The purchase happens instantly, and it charges on a per-use basis instead of a subscription.When you tell your agent "buy me a flight" or "find this person's email address," your agent finds the service that delivers that outcome and executes the transaction over x402 or MPP.For a business, listing on x402 or MPP is just opening your services up to agents. It’s the same pattern businesses have always followed every time a new way to get paid comes along:

- Cash — the original, in person.
- Checks — used for centuries.
- Credit and debit cards — since the 1950s (later made easy to accept online by processors like Stripe).
- Bank transfers (ACH) — since the 1970s (later made easy to connect online by [Plaid](https://plaid.com/)).
- Online wallets like PayPal — since the late 1990s, when e-commerce took off.
- And now agents — via x402 or MPP.

Each one was just a business opening itself up to a new way to get paid. x402 and MPP are simply the newest ones.

> 🧵Remember: These are not fringe experiments. x402 comes from Coinbase, and MPP comes from Stripe and Tempo, some of the biggest names in financial technology. Exposing your API so an agent can buy from you is genuinely easy to do.

## Where you can see who is participating

If you want to see every business that has listed its services and entered the world of agentic commerce, look at [x402scan](https://www.x402scan.com/) or [mppscan](https://mppscan.com/). They show you:

- every merchant that has opened its doors to agents
- the transaction activity across the whole payment protocol

> 💡Tip: x402scan and mppscan are the best "is this real yet?" gut-check. Go look at the live merchant list before you decide agentic commerce is still theoretical.

## The crux: open agentic commerce

Here is the fundamental point. x402 and MPP emphasize open agentic commerce. Anyone can go on and list a service to be bought or sold.Think of it like Google and the open internet. Anyone can buy a domain, stand up a website, and put whatever they want on it, because it is their digital asset. Open agentic commerce works the same way. Any person in the world is free to list their business on x402 or MPP and open it up to the agentic economy. There is no governing body telling you that you have to be a certain size or only do a certain kind of business.Contrast that with closed commerce, like Amazon:

- Amazon (closed): to sell, you get approved and jump through Amazon's hoops first, because Amazon controls that marketplace.
- Open agentic commerce (open): no governing body controls the marketplace. Anyone can participate.

## How you actually engage: two sides

There are two ways in, the supply side and the demand side.Supply side (you are the merchant). This is what we just covered. You list your business or service on x402 or MPP and open it up for agents to buy.1.Add pay-per-call to your APIA few lines of code enable per-request pricing. AI Agents pay automatically. No subscriptions, no invoices.2. Publish a discovery specificationAdd a simple file that tells AI Agents what your API does, what it costs, and how to call it.3. Reach agents everywhereAI Agents discover your API and start calling it. You get immediate distribution to Claude, Cursor, Codex, and every agent on x402 or MPP.

> 💡Tip: You do not have to pick sides. A business can list on x402, MPP, or both, the same way a store can accept both Visa and Mastercard.

Demand side (you are the buyer). This is how you get your agent to actually purchase things for you. Your agent needs three things:

- a wallet
- a currency (stablecoins like USDC)
- access to the payment protocols (x402 or MPP), so it can reach every listed business.

The easiest way to get all three is a pre-configured service like [AgentCash](https://agentcash.dev/). AgentCash arms your AI agent with a wallet and gives it access to over 4,000 tools across the internet, because it’s connected to everything listed on x402scan and MPPscan.Installing it is a single command in your terminal and your agent is onboarded with a wallet:npx agentcash@latest onboard

## Why stablecoins

The microtransaction layer of agentic commerce runs on stablecoins, and the reason is fees. (Bigger purchases can still ride on a card, as you will see below.)With a credit card, you pay roughly 30 cents plus 2.9% per transaction. That fee structure defeats the entire point of a microtransaction. Imagine walking into a mall and paying a $5 fee every time you bought a single T-shirt, so you were pushed to buy 20 shirts at once just to make it worth it.That’s basically how the internet works today. If you want one person's email address, you buy a whole month's subscription. Card fees make seamless, one-at-a-time, few-cent transactions impossible. Stablecoins carry near-zero network fees on modern chains like Base, Solana, and Tempo, so you can transact one item at a time, seamlessly.

> ⚠️Warning: Don’t let the word "crypto" scare you off. These are not meme coins, and this is not Bitcoin. A stablecoin is pegged to a real-life asset.

Stablecoins like [USDC](https://www.circle.com/usdc) are backed one-to-one by dollar-equivalent reserves, meaning cash and short-term US Treasuries held in parity with the tokens in circulation.That’s what makes it stable.

## Will stablecoins actually catch on?

One big signal of where this is heading: Open USD recently launched its own stablecoin, backed by Mastercard, Visa, Coinbase, Stripe, BlackRock, and a consortium of 140-plus companies. And over 300B of US dollars have already been tokenized into major stablecoins like USDC (Circle) and USDT (Tether).When we see signals from mainstream institutions adopting stablecoins, it’s a sign of confidence in the direction of agentic commerce.⚙️Technical Stuff (skip this if you just want to buy things). There are four ways to collateralize stablecoins:1. Fiat / physical currency, like the US dollar.2. Commodities, like gold or silver.3. Other cryptocurrencies, like Ethereum or Bitcoin, held in an over-collateralized amount to absorb volatility.4. Algorithmically, expanding and contracting the coin's supply to hold the one-to-one peg as price fluctuates.The trade-off with stablecoins:

- Pro: they unlock microtransactions, which is the whole point.
- Con: onboarding is a little less familiar if you have never touched the crypto world.

> 💡Tip: A full "Stablecoins 101" is coming separately. This guide stays focused on agentic commerce, so if the stablecoin part is what's really nagging you, that companion piece is where to go deeper.

## Getting your wallet funded

You create your wallet and onboard. The easiest path has three steps:

- Connect an account you may already have, like Coinbase.
- Transfer stablecoins from Coinbase (or wherever your wallet lives) into your AgentCash wallet, either by entering your wallet address or connecting it directly.
- That’s it. Your agent can now spend.

It is much easier than it sounds, and it feels scarier than it actually is.⚙️Technical Stuff. There are also newer paths being adopted. You can go through KYC (“know your customer,” the standard financial identity check) and then use a credit or debit card to buy stablecoins.

> 💡Tip: Whether card funding is available to you (versus transferring stablecoins directly) depends on your onramp provider and where you live, since crypto rules still vary by jurisdiction. If a card path is not offered, transferring stablecoins directly always works, and it is usually cheaper anyway.

For a larger purchase like a flight, your agent may prompt you to use a system like Stripe and a credit card. The agent still runs the entire transaction end to end, and simply asks for your card to complete that final step.

## How do I avoid getting scammed?

Once people understand the open part, the questions come fast. If anyone can list a service and there are thousands of tools out there:

- How does my agent pick a good one?
- How do I avoid getting spammed or scammed?
- How do I stop my agent from spending $500 of my money without asking?

This is exactly where a tool like [AgentCash](https://agentcash.dev/) earns its place, because AgentCash sits as the arbiter of agentic commerce. (Full disclosure, as promised up top: AgentCash is Merit Systems' product, and Merit is where I work. Other agent wallets exist too. Here is specifically what an arbiter does.) Two things are happening.1/ Safety and control. AgentCash has guardrails built into the wallet itself:

- Any transaction over $5 is flagged and will not go through without your explicit permission.
- You can set that threshold wherever you want, whether that is 50 cents, $2.50, or $10.
- Before it transacts, it tells you exactly what it is about to buy and what it will cost, so you have full transparency on the service and the price.

2/ Quality curation. This is the real meat of being an arbiter. AgentCash sits as the marketplace with visibility into all of those tools. You do not know all of them. We do. So when you ask for something, we serve your agent the highest-quality service for the job, judged on real signals like:

- Trusted transaction volume
- Successful transactions
- Uptime
- Semantic search

The analogy is Google. Google is the arbiter of information. You search a query and Google surfaces the result that best answers it, and you keep coming back because you trust that it will.If Google served you nothing but spam, you would stop using it. AgentCash serves the same function for buying. It is your search engine for the agentic economy. Ask it to find someone's email address and it surfaces a high-quality, proven tool rather than leaving you to stumble into something that does not deliver.Remember: AgentCash is doing two jobs at once. It is the safety mechanism (spend limits, approval prompts, full transparency) and the quality filter (surfacing only proven, legitimate services). Think of it as the search engine for the agentic economy.

## Ten Things to Remember About Agentic Commerce

- Agentic commerce is the third evolution of commerce. First was in person, then online, now agents buying for you.
- An AI agent is just software that acts on your behalf and does the shopping.
- The old internet runs on accounts and subscriptions, which is a terrible fit for agents.
- Agentic commerce turns the internet from a Costco into a mall. You walk in, buy one thing, and walk out.
- It runs on payment protocols: x402 (Coinbase) and MPP (Stripe and Tempo).
- It’s open. Anyone can list a service and no gatekeeper decides who’s allowed in.
- You can see who’s participating on x402scan and mppscan.
- Stablecoins are a fundamental currency of agentic commerce
- To participate as a buyer, your agent needs a wallet, and the easy button is AgentCash.
- AgentCash is the arbiter: it keeps you safe with spend limits and surfaces the highest-quality service, like a search engine for buying.

## Words You Now Know

- Agentic commerce — agents buying goods and services on your behalf.
- AI agent — software that executes tasks and outcomes for you.
- Payment protocol — the rails that let an agent see what's for sale and pay for it (x402, MPP).
- x402 / MPP — the two protocols, from Coinbase and from Stripe and Tempo, respectively.
- x402scan / mppscan — directories showing every listed merchant and the transaction volume.
- Open agentic commerce — a marketplace anyone can list on, with no gatekeeper.
- Stablecoins — digital coins backed one-to-one by dollar-equivalent reserves (cash and short-term Treasuries, commodities, etc.).
- Microtransaction — a tiny purchase (a penny, a few cents) that card fees would otherwise make impossible.
- KYC — "know your customer," the standard financial identity check.
- AgentCash — Merit's tool that gives your agent a wallet and acts as the safe, curated arbiter.

## The one-line version

Commerce went from in person, to online, to agents buying for you.x402 and MPP make the internet an open marketplace agents can shop in, stablecoins make one-cent purchases actually work, and AgentCash gives your agent a wallet plus a trusted arbiter so it buys the right thing safely.

Welcome to Open

### Give your agent a wallet

One balance, USDC on Base · Solana · Tempo. Let your agent pay for any payment-protected API — no API keys.