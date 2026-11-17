---
title: Payments are the new ads
source: https://x.com/sytaylor/article/2117641856716595362
author:
  - "[[Simon Taylor (@sytaylor)]]"
published: 2026-11-03
created: 2026-11-09
description: The fight between network tokens and balances could reshape who owns the financial customer.Agentic commerce sucks for most people.For examp...
tags:
  - clippings
  - agentic
  - ArtificialIntelligence
  - BUGuestLecture
---
![Image](https://pbs.twimg.com/media/HWRdBZbWUAAx_ro?format=jpg&name=large)

The fight between network tokens and balances could reshape who owns the financial customer.

Agentic commerce sucks for most people.

For example Scott Wessman [picked his Amex in Muse](https://x.com/scottew/status/2117013565102264346). Link charged the Amex, then paid Best Buy with a Visa card of its own. When one item failed, Best Buy asked for the security code on that Visa, and he had no way to find it and nobody to call.

Most people aren’t going to put up with that. Six in ten UK adults told YouGov they’d [stop using an AI shopping agent after one mistake](https://www.businesswire.com/news/home/20260628382944/en).

Scott asked for the obvious fix, which is to use the card he picked and let the store charge it.

That’s not happening because the banks, card networks and merchants haven’t figured out how liability will work. Today’s solution, sticking a virtual card in front of your actual card is causing breaks in the experience.

Hold that idea in your head for a second and consider that Stripe Link users via Instinct spend on average $1,300 per month. There’s a signal here that when a user is willing to put up with it, agentic commerce is incredibly useful.

What happens next is a foot race to replace the virtual cards (or giving your agent your 16 digit card number). There are two approaches, and I suspect we end up with both.

1. The banks, networks and merchants figure out liability and **network tokens** like we did for Apple Pay or
2. Agents get their own **stablecoin balance,** card (and even network token), so they never need to wait for your bank.

And this battle is urgent. The big platforms will keep pushing, because AI is already cutting into the ad revenue that pays for them.

In this one:

- Four ways an agent pays
- The one-time card is a bridge
- **Network tokens** worked for Apple Pay
- **Stablecoin** **balances** have given the agent its own money
- Every platform will embrace agentic traffic
- Agents need to become useful and monetize

## Four ways an agent pays

X user [vigstar1](https://x.com/vigstar1) [mapped](https://x.com/vigstar1/status/2116973323460735200) the four ways an agent pays on card rails today.

![Image](https://pbs.twimg.com/media/HWRdKZ-X0AAkUbP?format=jpg&name=large)

The first two are how most agentic commerce payments happen today. And they’re the ones that break most often.

- **The agent types in your real card.** Your bank sees the shop and thinks you made the purchase. But the merchant sees a robot inserting raw card details and it looks like a fraud risk.
- **The agent pays with a one-time card.** Your bank sees [a charge from the platform](https://www.verygoodsecurity.com/blog/posts/how-virtual-cards-are-used-in-agentic-commerce) that made the virtual card (e.g. Stripe), not what you bought. This is what happened to Scott Wessman where security breaks.
- **The agent pays with a network token issued to it by the bank, which is where Mastercard wants to go.** Your bank sees the shop again. This isn’t widely supported by banks yet, because they’re unclear on how the liability will work.
- **The agent spends a balance you loaded up front.** (e.g. Stripe balance on Instinct) Your bank sees one top-up and never sees the agent. The liability in this case all lives with whomever is holding the balance for the agent.

Often Instinct stores your real card, security code included, and then types it into the checkout. Elon Musk told people they could [send Grok Bot a photo of theirs](https://x.com/elonmusk/status/2109027407185277123). This seems remarkably insecure in the long term.

Gabe Suby noted from Mastercard’s earnings that Mastercard doesn’t see virtual cards as a long-term solution:

> Oct 30
> 
> .@Mastercard ceo on yesterday's q3 call, about one-time virtual cards for ai agents: "a bridge", "not a long-term solution that we will support" > the network wants its own token all the way to the merchant > shop pay already runs on it, inside muse from meta, claude duet and

That leaves two options.

1. Network tokens
2. Stablecoin balances

I think the answer is AND, not OR. Your bank card and an agent’s balance could both use network tokens. What interests me is how much spending stablecoin balances can win while the banks are still working out liability, and how much stays there once they catch up.

## The one-time card was a bridge

The UX of virtual cards for agents fails to deliver on a major benefit of agents. Them waiting to buy something when its on sale or launched.

For example, [Tom Noyes](https://x.com/noyesclt/status/2115380476135297091) tweeted that it defeats the purpose of an agent. He says “nobody wants to approve concert tickets at 2 a.m.” And yes, I barely get enough sleep as it is!

It’s also what went wrong with the Best Buy order. The reality is agentic commerce consistently breaks when merchants aren’t ready. If there’s no network token available, and Link isn’t supported, it falls back to that one-time card to try and make the purchase work.

Swapping in a different card also breaks your card’s rewards and muddies its purchase protection.

It’s also not a particularly profitable endeavor. [vigstar1](https://x.com/vigstar1) [estimates](https://x.com/vigstar1/status/2115969694889025817) Stripe keeps about 40 cents on a $100 purchase paid from a credit card, before its own costs. There’s a lesson from history here, it rhymes with what happens in the early days of mobile commerce (there’s a term from the past).

Before we had Apple Pay, we had “staged wallets.” PayPal and the old Google Wallet used versions of that model: the wallet sat between your card and the merchant, leaving the funding bank with less detail about the purchase. In 2013 Mastercard [introduced a fee](https://blog.mondato.com/what-is-the-impact-of-mastercards-new-digital-wallet-fee/) for some of those “staged” wallets.

## Network tokens worked for Apple Pay

Apple did a lot of things right with Apple Pay.

Back then we turned 16 digit card numbers into device tokens (DPANs) and **network tokens.** The little image of the card you see in your Apple Pay wallet is representing that secure “token” that is bound to your device.

Chase was one of the [six launch banks](https://www.apple.com/newsroom/2014/10/16Apple-Pay-Set-to-Transform-Mobile-Payments-Starting-October-20/) in 2014, Apple [reportedly took 0.15%](https://www.theregister.com/2014/09/13/apple_to_get_15_cents_for_every_100_dollar_payment_on_its_pay_service_says_ft/) of each credit card purchase, and one bank executive said at the time that the deal “still puts us at the centre of payments.” Every purchase also needed your fingerprint, so your bank knew you were there. It reduced fraud, it had clear liability protections, and over time it reduced the amount of cash payments and increased the card volume, creating new revenue for banks.

We’re not there yet with agents. We’re still so very early.

Agents don’t yet have a universal equivalent to the device token (DPAN). An agent token. The card networks have proposed their approaches, but until liability is resolved, the banks aren’t rushing to adopt.

That agreement is what takes time.

Prashant Sharma of JPMorgan Payments [told American Banker](https://www.americanbanker.com/payments/news/inside-jpmorgan-payments-agentic-commerce-strategy) in May, “Ideally, the entity who made a mistake should be liable,” and “I don’t think that we have an answer at this particular point.”

The problem with agents is there’s no authentication moment like Face ID with Apple Pay.

Visa’s answer is asking users to approve with a passkey, but right now, according to [vigstar1](https://x.com/vigstar1) they’re also giving every major agent platform a waiver to that requirement. And, quelle surprise, [Chase cards cannot be added](https://agentcard.com/info/blog/solution-recommendation/why-visa-intelligent-commerce-only-works-on-a-small-percentage-of-cards-and-what) in this way to many of those agent platforms.

Chase is big. They can afford to wait.

But the market has already found another solution: giving an agent its own balance to just do things.

## Give the agent its own money

Instinct just launched Balance. Users can set a budget for their agent to make purchases on their behalf without having to step in and approve. You can set limits and budgets for each purchase, for example, when a ticket pre-sale launches.

The example in the tweet below shows someone adding $700 to their agent’s balance, asking to buy their favorite lunch every day for a week, and then get concert tickets.

> Oct 26
> 
> Give your Instinct a budget Built with @Stripe and @link, we're launching Link balance in @instinct. You can now set a budget for your Instinct to make purchases on your behalf, and Instinct does the rest. You choose how much money to add and set a limit for each purchase. Your

[Noyes](https://x.com/noyesclt/status/2115380476135297091) also says what your bank sees now.

> With a prefunded balance, the consumer’s bank sees one load, authenticated by the consumer, and never sees the agent.

With a balance, you make the permission question much simpler. Instead of asking if the agent has permission to buy this thing, this one time, you just ask if there’s enough money. If there’s $637 left, the agent can spend at most $637. (h/t [Ronak Daya](https://x.com/RonakDaya/status/2115896517261394010))

Of course, a balance doesn’t prove the agent bought what you asked for, which is why the banks are rightly worried about liability.

For Link Balance, the agent gets stablecoins and a wallet (by Privy on Tempo\*). Every purchase still runs through the card networks. But the agent doesn’t need a bank account.

It’s a card for acceptance, a stablecoin for the balance and stored value.

Stripe (or whomever makes the stablecoin balance card) makes more money this way too. When you top up with a debit card from a bigger bank (>$10 billion assets), Durbin caps the fee at 21 cents plus 0.05% and a cent for fraud prevention, so a $100 top-up costs Stripe about 27 cents.

The agent then spends on a Lead Bank debit card, and Lead Bank is small enough to be exempt from the cap, so the card earns about $1.95 per $100 spent. That leaves about $1.70 per $100 for Stripe and Lead Bank to share before costs, more than four times what [vigstar1](https://x.com/vigstar1) estimates a one-time card leaves.

Blocking agents from logging into your bank doesn’t solve this. [JPMorgan can stop an agent logging in with your password](https://www.fintechbrainfood.com/p/jpmorgan-and-citi-blocks-ai-agents). It doesn’t (yet) stop you topping up a Link balance with your Chase debit card, unless it starts declining its own customer’s card.

The money stops being a deposit once it’s in the balance.

It’s sitting as a stablecoin in a wallet somewhere else, which means the banks become less relevant in agentic anything.

## Every Platform will embrace agentic traffic

Merchants are moving faster than banks.

a16z’s Olivia Moore [wrote this](https://x.com/omooretweets/status/2116250489532887406) about OpenTable integrating with Instinct:

> Oct 30
> 
> The dominoes are starting to fall on legacy platforms embracing agent traffic… With the exception of Amazon and their massive physical logistics as a moat, I think ultimately every platform is going to have to move this direction

Instinct’s founder, Noah Shinn, [agreed](https://x.com/noahrshinn/status/2116626173212123177?a=lsT-cF2H4DZEObpm8uM&s=19). He says businesses now come to Instinct to learn how to rebuild for agents, because people using agents buy more and stick around longer.

This is an enormous market signal.

Even the mighty Walmart is embracing agents. It announced on 21 October that each customer agent will get [its own profile inside your Walmart account](https://thenextweb.com/news/walmart-ai-agents-shop-agent-profiles), and you can approve every purchase or set a limit like $50, $100 or $200. Walmart told customers to think of it “like adding someone as a cardholder on your credit card”.

And the demand is there.

Stripe says people who buy through Instinct [spend over $1,300 a month](https://threadreaderapp.com/thread/2093637347510309331.html) there on average. Muse shoppers bought more than Meta expected, and Meta had to [ask Stripe to raise its limits](https://thenextweb.com/news/meta-muse-10-million-users-stripe-limits-profit).

People with agents are high spenders, and over time that’s becoming attractive to merchants, which is why they’re starting to support them more and more.

Yes there are still holdouts like Amazon (see [agent wars](https://www.fintechbrainfood.com/p/agent-wars))

But they can afford to hold out until the game on the field suits them more.

**The way banks regain relevance is by stepping in and owning an agent’s mistakes** (as [Nikil Konduru argues](https://www.linkedin.com/pulse/intent-toll-booth-nikil-konduru-qycfe/)). Banks have the interchange to pay for it, the underwriting and a disputes team. If agents sent over what you asked for before paying, banks would get the Apple Pay deal again, with your instructions doing the job of the fingerprint.

For the banks, they could lean on the card networks to introduce fees for virtual cards or “staged” agent balances, but something feels different this time.

**The card networks view teams like Meta and Instinct as issuers in their own right**.

There’s a very real possibility the agents with balances get their own network tokens.

## Agents need to become useful and monetize

The more useful the agent becomes, the more of your spending it can bring through that relationship. The more spending it brings through, the more payments revenue it can get.

**But the more agents get used, the less effective the ads business becomes for Google or Meta.** Google said on its latest earnings call that lower monetization in AI Mode contributed to slower Search growth. If you assume that trend continues, the ad-revenue fuelled hyperscalers need to chase two things.

1. Compute revenue
2. Services revenue (like payments)

While [Google Cloud revenue doubled](https://thenextweb.com/news/google-cloud-revenue-doubles-q3-2026) to [$30.7bn](https://x.com/search?q=%2430.7bn&src=cashtag_click). The growth is moving from ads to compute already.

Emad Mostaque said on the Moonshots podcast that we could reach peak ad-revenue growth next year. And if agents are bypassing ads and getting you the best thing, they need a way to monetize helping you.

Payments revenue is a great way to do that.

This is Meta’s explicit monetization strategy.

Zuckerberg said Meta [plans to charge businesses a fee](https://fortune.com/2026/10/22/mark-zuckerberg-meta-muse-stripe-credit-limit-shopping/) when Muse helps complete a sale, and he expects Muse to make more per user than Meta’s apps do today. If Muse is buying things on your behalf, it’s intermediating between the customer and the bank or merchant.

With a balance, some of your money is already in the app.

When your agent knows what you want, Google and Meta might need to serve you fewer ads. But each match could be much more useful, more likely to convert, and worth more to the business paying for it. Less volume, much higher prices, and something that feels much less like an ad.

Payments are another way to monetize that relationship and make up for any revenue shortfall. Then matching what you want with someone who can deliver it would be another. But that’s a whole nother essay.

I don’t think anyone can afford to be caught on their heels. These agent platforms are shipping features at a speed unlike any time I can remember.

Banks could work out how to support agent payments, only to find their customers are already used to keeping money and managing their finances somewhere else.

Would you leave a balance with your agent? Reply and tell me.

ST.