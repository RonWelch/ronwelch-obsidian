---
title: Connecting Agents with Cryptography
source: https://liamhorne.com/agents-and-programmable-cryptography?utm_source=tldrnewsletter
author:
  - "[[Liam Horne]]"
published: 2026-09-29
created: 2026-10-01
description: How programmable cryptography could help agents find useful connections without exposing their private context.
tags:
  - clippings
  - blockchain
  - ArtificialIntelligence
  - BUGuestLecture
  - agentic
---
![Blue and red machines keep their intricate internal worlds separate while sharing a small connection.](https://liamhorne.com/assets/img/agents/header.png)

I think we’re heading toward a world where everyone has an agent working with a lot of private information: calendars, messages, company documents, customer data, and code. Much of that work will happen in the cloud, where your agent can keep going while you’re doing something else.

I’m curious how those agents will start forming groups or swarms. Your agent might run on your laptop, mine in a cloud service, and another inside a company’s network. How do they discover that they’re working on the same problem? And how much should they have to share before they know it’s worth talking?

There are a handful of techniques for letting people compute something together without sharing the underlying information. These ideas have been part of Ethereum and cryptography research for years, and I think agents give us some interesting reasons to use them.

One technique is **secure multiparty computation (MPC)**. A common approach splits private inputs into random-looking pieces and distributes them among participants. They follow a protocol to calculate an answer without any one participant having enough pieces to reconstruct the others’ inputs.

Another is **fully homomorphic encryption (FHE)**, which lets a cloud service calculate directly on encrypted inputs without reading them or the answer. That takes much more time and compute than an ordinary calculation, so I’d start with simple checks.

You could also use a **trusted execution environment (TEE)**, where protected hardware runs the calculation while keeping the data from the cloud operator. That requires trusting the hardware and its manufacturer. I’m especially interested in what we can do with the cryptographic approaches, where the inputs can stay private even from the hardware doing the work.

## Why would agents want to cooperate?

Here are a few things I’d want my agent to help me with.

**Make plans that never get arranged.**

Say my girlfriend and I want to have dinner with a few friends, but nobody gets around to starting the group chat. We could each tell our agents who we'd like to see and let them check for mutual interest and a free evening. We'd get a suggestion without sharing the rest of our calendars or everyone else we'd like to see.

![](https://liamhorne.com/assets/img/agents/grok-red-3d.webp) ![](https://liamhorne.com/assets/img/agents/dot-blue-3d.webp) ![](https://liamhorne.com/assets/img/agents/code-bot.svg)

![](https://liamhorne.com/assets/img/agents/grok-red-3d.webp) Dinner bot

My girlfriend and I want to see our friends more often. Check whether anyone on our dinner list also wants to get together, and find an evening we're all free.

1. Checking mutual interest
2. Comparing free calendar times

Two friends on your list also want to get dinner with you both. All four of you are free Saturday evening. Should I start a group chat to make a plan?

In each case, the agents could answer a narrow question before we decide whether to share more. The salary comparison is a simple way to see how that could work.

## How the private calculation works

For the walkthrough, suppose you earn $150,000 a year and your colleague earns $155,000. You agree to compare annual base pay in the same currency and ask whether the difference is at most $10,000:

```python
def salaries_are_close(your_salary, their_salary):
    return abs(your_salary - their_salary) <= 10_000

print(salaries_are_close(150_000, 155_000))  # True
```

Ordinary Python exposes both salaries to the computer running it. With MPC, the agents instead calculate the gap and compare it to the threshold using shares of the inputs, revealing only the yes or no.

With FHE, a compiler translates the subtraction, absolute value, and comparison into operations on encrypted numbers. The server knows the question and the $10,000 threshold, but the $5,000 gap and the final answer stay encrypted throughout.

The two techniques can also work together. In this design, the participants use MPC to create a shared public key for encrypting their salaries, while each keeps a share of the secret key. The cloud receives encrypted salaries and evaluation keys for doing the calculation, but no key that can decrypt them. Both agents must cooperate to decrypt the answer.

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 360 458" width="360" height="458" fill="none" aria-hidden="true"><rect x="6" y="8" width="152" height="104" rx="8" fill="#dce2e8" opacity=".04"></rect><rect x="6" y="8" width="152" height="104" rx="8" stroke="#dce2e8" stroke-width="1.25" fill="none"></rect><text x="57" y="40" text-anchor="start" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="14" fill="#24292e">Your agent</text> <text x="82" y="75" text-anchor="middle" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="12" fill="#24292e">$150,000 / year</text> <text x="82" y="96" text-anchor="middle" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="10" fill="#586069">Private input</text> <rect x="202" y="8" width="152" height="104" rx="8" fill="#dce2e8" opacity=".04"></rect><rect x="202" y="8" width="152" height="104" rx="8" stroke="#dce2e8" stroke-width="1.25" fill="none"></rect><text x="253" y="40" text-anchor="start" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="14" fill="#24292e">Their agent</text> <text x="278" y="75" text-anchor="middle" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="12" fill="#24292e">$155,000 / year</text> <text x="278" y="96" text-anchor="middle" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="10" fill="#586069">Private input</text> <path d="M82 118 V174" stroke="#24292e" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"></path><path d="M78 168 L82 174 L86 168" stroke="#24292e" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"></path><path d="M278 118 V174" stroke="#24292e" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"></path><path d="M274 168 L278 174 L282 168" stroke="#24292e" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"></path><rect x="72" y="134" width="20" height="25" rx="4" fill="#fff"></rect><path d="M78 144 V140 A4 4 0 0 1 86 140 V144" stroke="#586069" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"></path><rect x="75" y="144" width="14" height="11" rx="2" stroke="#586069" stroke-width="1.5" fill="#fff"></rect><circle cx="82" cy="149" r="1.2" fill="#586069"></circle><text x="97" y="150" text-anchor="start" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="10" fill="#586069">Encrypted</text> <rect x="268" y="134" width="20" height="25" rx="4" fill="#fff"></rect><path d="M274 144 V140 A4 4 0 0 1 282 140 V144" stroke="#586069" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"></path><rect x="271" y="144" width="14" height="11" rx="2" stroke="#586069" stroke-width="1.5" fill="#fff"></rect><circle cx="278" cy="149" r="1.2" fill="#586069"></circle><text x="263" y="150" text-anchor="end" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="10" fill="#586069">Encrypted</text> <rect x="40" y="180" width="280" height="122" rx="8" stroke="#dce2e8" stroke-width="1.25" fill="none"></rect><path d="M40 208 H320" stroke="#dce2e8" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"></path><circle cx="53" cy="194" r="2" fill="#dce2e8"></circle><circle cx="62" cy="194" r="2" fill="#dce2e8"></circle><circle cx="71" cy="194" r="2" fill="#dce2e8"></circle><text x="192.0" y="198" text-anchor="middle" font-family="'Söhne Mono', ui-monospace, monospace" font-size="10" fill="#586069">compute.example</text> <text x="180" y="237" text-anchor="middle" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="17" fill="#24292e">FHE server</text> <text x="180" y="267" text-anchor="middle" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="13" fill="#24292e">Are our salaries within $10,000?</text><text x="180" y="287" text-anchor="middle" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="10" fill="#586069">Server cannot read salaries</text> <path d="M180 308 V354" stroke="#24292e" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"></path><path d="M176 348 L180 354 L184 348" stroke="#24292e" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"></path><text x="196" y="324" text-anchor="start" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="10" fill="#586069">Encrypted result</text> <text x="196" y="342" text-anchor="start" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="10" fill="#586069">+ execution proof</text> <rect x="40" y="360" width="280" height="86" rx="8" stroke="#dce2e8" stroke-width="1.25" fill="none"></rect><text x="222" y="392" text-anchor="middle" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="14" fill="#24292e">Verify + decrypt</text> <text x="180" y="429" text-anchor="middle" font-family="'Söhne', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif" font-size="16" fill="#24292e">Yes — within $10,000</text></svg>

Before decrypting the result, both agents need to check that the server answered the question they approved: are the salaries within $10,000? The server could return a cryptographic proof tying the encrypted result to that calculation. Each agent would verify it before helping decrypt the result. That prevents the server from slipping in a different calculation, such as returning one person’s exact salary.

Whichever method you use, the answer reveals something. Knowing your own $150,000 salary, a yes tells you the other salary is between $140,000 and $160,000. Repeated questions could narrow that range, so both people need to authorize the comparison and the software needs to limit follow-up queries.

I enjoyed how 0xPARC put these ideas together in [*Programmable Cryptography: Four Easy Pieces*](https://0xparc.org/four-easy-pieces). I’d recommend it if you want to get into the math behind MPC and FHE.

## Who pays for shared computation?

Vitalik’s [essay on crypto and AI](https://vitalik.eth.limo/general/2024/01/30/cryptoai.html) points out how expensive cryptographic computation can be. Running these checks costs something, so it makes sense for agents to pay a service to do them. If a service could offer a small encrypted comparison for a tenth of a cent, an agent could pay through [MPP](https://mpp.dev/) each time it wanted to check a possible collaboration.

Those payments could add up quickly. A million agents each paying for one check every ten minutes would already generate roughly 1,700 payments per second. That’s the kind of demand that makes a fast, low-cost stablecoin network like [Tempo](https://tempo.xyz/) useful. Agents in different clouds could pay through the same network, within budgets set by their owners, without their providers first building billing integrations.

If that grew to millions of payments per second, techniques like state channels would become interesting: agents could exchange payment updates offchain and settle the total later. That would start to look like what we built with [Web3Torrent](https://liamhorne.com/web3torrent), where peers paid each other tiny amounts for each piece of a file. Here, they might pay for each private calculation or useful answer.

I’m excited to see what happens when agents can find collaborators as easily as they find information today. They could form a group around a problem, share only what’s needed, and pay each other to help solve it.