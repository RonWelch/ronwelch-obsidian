---
title: How ChatGPT Wallet works
source: https://x.com/alfongj/article/2121682952157315530
author:
  - "[[Alfonso (@alfongj)]]"
published: 2026-11-14
created: 2026-11-16
description: On Friday, OpenAI launched ChatGPT Wallet. It lets a dot, OpenAI's always-on agent, check out on a merchant's website and pay with your own ...
tags:
  - clippings
  - agentic
  - ArtificialIntelligence
  - BUGuestLecture
---
![Image](https://pbs.twimg.com/media/HXG7I22XkAIzeIR?format=jpg&name=large)

On Friday, OpenAI launched ChatGPT Wallet. It lets a dot, OpenAI's always-on agent, check out on a merchant's website and pay with your own credit card.

I think it's the most proper implementation of agent payments so far. I'm biased, because it follows the same architecture we use for agent cards at Crossmint... but that also means I can explain how it works :)

## Benefits of this design

Your agent pays with your card, and it never holds your card.

At checkout, the card network generates a substitute card number with its own security code and expiration date, for that one purchase. The dot types that into the merchant's form. Your real number stays in a vault that neither the dot nor the merchant can read.

![Image](https://pbs.twimg.com/media/HXG2LP0WYAAWvNx?format=jpg&name=large)

For you, that means two things. The charge lands on your own card, so you keep your points and your dispute rights. And if the agent gets prompt-injected, or the merchant gets breached, what leaks is a number capped at the amount you already approved.

It's also good for card issuers, especially in comparison with wallets like Stripe's Link, which hand the agent a virtual card of their own and then charge your real card in a second transaction. With those, your bank sees the wallet as the merchant, and loses sight of what you bought and where. Here the issuer authorizes the real purchase at the real merchant, and the customer relationship stays with the bank.

For merchants, it's not great (yet) but it's decent too. Compared with staged wallets with virtual cards, here they typically would pay less interchange, and also have higher visibility on who is the person buying behind the agent card.

While early, we think this is the right long term model.

Plug: If you're building an agent and want the same thing, you can get it running with Crossmint in under a day: [docs.crossmint.com/agents](https://docs.crossmint.com/agents)

## What it looks like as a user

You add a card once. On mobile it's Settings, then Wallet, then Add Card. The number goes into a secure form, never into the chat.

Then you ask your dot for something. Say, a pair of trail running shoes under $200. The dot browses, picks a store, fills the cart and reaches checkout.

Before any money moves, you get a review screen: the merchant, the items, the total, the card, plus shipping and any recurring terms. You approve with Face ID or a fingerprint. If the price or the merchant changes after that, the dot has to ask again.

![Image](https://pbs.twimg.com/media/HXG19IhW8AEtckZ?format=jpg&name=large)

On your statement you see a normal purchase at that store, and you can do returns as usual through the merchant, and disputes through your bank. The one visible difference is that the last four digits on the receipt may not match your physical card.

The launch starts limited: US only, ChatGPT Pro subscribers, 18 or older, and only inside dots. And it works with credit cards only, from American Express, Mastercard and Visa. No debit, and Chase cards need an extra approval in the Chase app. All of this will be evolving in the next few months.

## Under the hood

There are four parties between your request and the merchant's payment form.

![Image](https://pbs.twimg.com/media/HXG3qSMWkAEsABX?format=jpg&name=large)

Diagram of steps behind a ChatGPT wallet purchase

**1.** **Saving the card to the vault:** When you add a card, you type it into a form hosted by OpenAI's payment service provider. OpenAI's privacy notice says it receives a tokenized credential plus the network, issuer, card type and last four digits. It never receives the full number or the security code, so the model can't leak it.

**2\. Enrollment in agentic tokens.** The card gets registered with its network's agent program: Visa Intelligent Commerce, Mastercard Agent Pay, or American Express Agentic Commerce Experiences (ACE). When you do this, the bank behind your card may ask you to verify the card (e.g. with OTP).

**3\. The agent requests a credential.** When you approve a purchase, the network issues what OpenAI calls a payment token: a card number, security code and expiration date that look like a real card number but only works for your purchase. It's generated per purchase, and the network enforces a spend limit to the amount you approved. At this time ChatGPT uses one credential per checkout, but they claim they may support doing multiple orders with one in the future (this is allowed by card networks).

**4\. The agent checks out.** The dot types that credential into a payment form in the website you are buying, the same way you would as a human. Then the merchant runs a normal card authorization, and their processor sees a regular payment.

And note what OpenAI is not doing: it isn't a bank, an issuer, a payment network or a merchant, and it doesn't hold the funds. The money moves from your issuer to the merchant on existing card rails.

## Identity: Know your agent, and the human behind it

Getting a card credential to work is half of the problem. The other half is getting the agent to actually pass through a checkout built to stop bots.

The dot runs in a browser in OpenAI's cloud, and to a merchant's fraud stack, that looks like a headless browser on a data center IP that has never visited before... Which looks quite similar to a card-testing / stolen-card attack.

To help anti-fraud tools distinguish its traffic from malicious actors, OpenAI answers with two sets of headers.

![Image](https://pbs.twimg.com/media/HXG5CU1WoAAXOSP?format=jpg&name=large)

KYC and KYH headers

The first is **Web Bot Auth**. It's an IETF draft, started at Cloudflare, built on HTTP Message Signatures (RFC 9421). Each request from the cloud browser carries a Signature, a Signature-Input and a Signature-Agent header set to "[https://chatgpt.com](https://chatgpt.com/)". The merchant's CDN fetches OpenAI's public keys from a well-known URL and checks the signature to ensure it's legit. Akamai, Cloudflare, HUMAN and Vercel already verify it out of the box.

This tells the merchant which agent platform is knocking, although it says nothing about the person who sent it.

The second header fixes that: OpenAI is also rolling out user signals in the Forwarded header: the user's IP address, a masked IP prefix, the network's ASN, country, locale, time zone and the app they're using.

Forwarded: asn=64496;ip-prefix="192.0.2.0/24";user-agent="Example/130"

With this, a fraud model can now score the human in Brooklyn, and trust its attestation if it trusts OpenAI to be truthful. OpenAI says it's working with Forter, Riskified, Sardine and Signifyd, Adyen, [Checkout.com](https://checkout.com/) and Stripe to ensure they all respect it.

## Drawbacks of this design

This model is the right long term architecture for agentic commerce, but it has real tradeoffs today.

First, sites can and still block agents. From Amazon or United, who do it on purpose, to many others do it by accident, with bot rules written for scrapers.

Second, a face ID prompt on every purchase means your agent can't buy anything while you sleep.

Third, card coverage is partial.

Finally, browser checkout is slow and expensive in tokens.

But there are paths to address all of those.

## You can build this yourself

We think this is the right model for agentic commerce. It gives your bank visibility into the real purchase, gives the merchant a customer it can recognize, and gives the agent a credential that can't spend more than you approved. It's also the most natural one for users, because it's the card they already have.

But it took OpenAI a dedicated team, months of work and a long list of vendors: a vault, three card networks, issuers, bot verification and fraud providers. And it will continue to require them work to add more payment methods, evolved commerce protocols, KYA signals, and more.

If you're building an agent, consumer or commercial, you don't need to repeat that. Crossmint's Agent Cards SDK runs on the same pieces: a PCI-compliant vault, Visa Intelligent Commerce and Mastercard Agent Pay, with Amex and others coming soon. The cards those programs don't cover yet still work today, through an encrypted card fallback.

Your user saves a card once and approves a spending limit with an amount, an expiration and a merchant if you want one. Your agent gets a one-time card number for each payment. Our checkout browser can fill it into any merchant's form, and, as a bonus, the same SDK gives your agent a stablecoin wallet for machine payments.

You can integrate it in under a day. If you're interested, [check out our docs](https://docs.crossmint.com/agents/cards/overview) or DM me.