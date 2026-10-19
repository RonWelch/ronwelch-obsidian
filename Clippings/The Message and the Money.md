---
title: The Message and the Money
source: https://fintechtakes.com/p/the-message-and-the-money
author:
  - "[[Alex Johnson]]"
published: 2026-04-30
created: 2026-10-19
description: "My view on stablecoins has always been this: they’re crypto, stripped of speculation. Remove the trading, the moonshots, the get-rich-quick schemes, and what you’re left with is a genuinely novel form of payments infrastructure: a new primitive for moving value that happens to be fast, programmable, and borderless. However, just because stablecoins have been stripped […]"
tags:
  - clippings
  - ArtificialIntelligence
  - agentic
  - BUGuestLecture
---
My view on stablecoins has always been this: **they’re crypto, stripped of speculation**.

Remove the trading, the moonshots, the get-rich-quick schemes, and what you’re left with is a genuinely novel form of payments infrastructure: a new primitive for moving value that happens to be fast, programmable, and borderless.

However, just because stablecoins have been stripped of speculative trading does not mean that they are free from other forms of speculation.

Crypto advocates *love* to speculate on the disruptive potential of stablecoins. And their favorite target is the card networks. Stablecoins, the story goes, are going to kill Visa and Mastercard; routing around interchange, zeroing out the 2-3% fee, and liberating merchants from the tyranny of the payment rails.

It’s a [s](https://x.com/SamBroner/status/1867258439195668973) [atisfying story](https://x.com/SamBroner/status/1867258439195668973):

The only problem is that it’s wrong.

Not in the conclusion that stablecoins represent a meaningful payments primitive, but in its diagnosis of where the inefficiency actually lives.

### Chesterton’s FenceCopy anchor linkCopied

In 1916, the writer G.K. Chesterton introduced a thought experiment that has become one of the most useful ideas in policy and system design.

Imagine you’re walking along a road and you come across a fence running across it. It seems pointless. It’s in the way. Your instinct is to tear it down. Chesterton argued that you should stop. Before you remove anything, you need to understand why it was built. If you can articulate a clear reason, and if that reason no longer applies, then by all means, take it down.

However, if you can’t explain why it’s there, you don’t yet understand the situation well enough to be making changes. The fence may be doing something you can’t see.

This is, I’d argue, the primary flaw in most payment card disruption narratives, including and especially those proposed by crypto advocates. They look at a 2-3% fee, assume it’s a pointless fence erected by greedy middlemen, and reach for a sledgehammer.

What they miss is that there are actually two fences.

### AuthorizationCopy anchor linkCopied

In September of 1958, Bank of America did something audacious bordering on reckless: it mailed 65,000 pre-activated credit cards to households in Fresno, California, without anyone asking for them.

The “Fresno Drop,” as it has since become known, was the brainchild of Joseph P. Williams, director of Bank of America’s customer services research group. His goal was simple: for BankAmericard to replace the tangle of individual store credit accounts that most Americans juggled with a single card that worked everywhere.

The product — the first-ever general-purpose credit card — experienced plenty of problems early on (*fraud and excessive delinquency rates, most notably*), but it eventually became a massive success for Bank of America. So much so that, by 1966, the bank was licensing BankAmericard to other banks across the country.

And this created a problem, as an executive at National Bank of Commerce in Seattle recounts:

> There were no electronic systems for authorizing transactions. Each merchant had a floor limit, beneath which no authorization was required. It took criminals no time at all to pattern such limits and accurately assess the degree of risk in each merchant location. A transaction over the floor limit required that a merchant employee telephone the bank with which they had contracted. An employee of the merchant bank then made a long-distance call to the card-issuing bank, where an employee manually looked up the customer’s account in huge, computer-printed, paper ledgers, determined if the sale could be authorized, gave an authorization number to the inquiring bank, who passed it along to the merchant. Meanwhile, the customer waited angrily or was required to return later. The card-issuing bank posted a hold on the customer account for the amount of the sale, to be released when the sales draft appeared weeks, often months, later.
> 
> Merchants swiftly realized it was prudent to obtain an authorization in advance for every potential sale, however slight the chance it would be completed. Customers’ lines of credit were swiftly absorbed by holds for sales never completed and they were denied credit they should have had. System authorization costs soared since merchants made local calls, while banks absorbed all long-distance bank-to-bank calls.
> 
> There was no Internet, no electronic entry of data, no CRT screens for electronic examination of accounts, and no dispersed computing power. All data entry required keypunching each digit of information (every letter and every number) into a four-by-six inch piece of cardboard by a clunking mechanical punching typewriter the size of a large refrigerator. The punched cards were then put through an elongated card reader twice the size of the keypunch machine to capture the data on magnetic tape, then fed into a van-sized computer for posting to customer accounts. The data were returned to tape and finally sent into a large mechanical printer to produce huge binders of customer records.
> 
> Primitive and cumbersome as the system was, it performed well enough to allow the massive outpouring of cards to gain considerable consumer and merchant acceptance. As acceptance skyrocketed, the number of transactions flowing between banks exploded. The clearing system swiftly disintegrated under the volume.

By 1968, the system was close to collapse. Licensee banks were threatening to abandon BankAmericard entirely. Bank of America called a meeting in Columbus, Ohio, and out of that meeting came the decision to spin off the card program into a cooperative jointly owned by all the member banks.

That executive from National Bank of Commerce, whom I quoted earlier? His name was Dee Hock, and he was put in charge of the new cooperative venture.

Hock’s genius, as it turned out, wasn’t technical. It was organizational. A card network is fundamentally a coordination problem. Solving it required banks to cooperate on infrastructure while still competing on products. These were institutions that fundamentally distrusted each other and competed for the same customers. Hock had to convince them to build something together anyway.

His solution was what he later called a “chaordic” organization — a system that is both chaotic and ordered — where member banks would cooperate on shared rules and infrastructure while fiercely competing on products, pricing, and customers. No single bank would own or control the network. All of them would.

By 1973, Hock’s chaordic organization (National BankAmericard Inc.) had digitized and automated the authorization process. This allowed for 24/7/365 transaction processing, reduced authorization times from 5 minutes to 50 seconds, enabled overnight transaction clearing, and cut postage and labor costs by $17 million (*roughly $79 million in today’s dollars*) in the first year alone. The system eventually rebranded to Visa in 1976 to shed its association with a single bank and to project an identity that could work anywhere in the world.

Meanwhile, a competing coalition of regional banks had been building its own version since 1966 under the name Interbank, which would eventually become Mastercard. By the 1970s, you had two global, interoperable, rules-based payment networks competing with each other. Each built by groups of banks that ferociously competed with each other, but held together by shared economic interest and an extraordinary set of governance agreements.

What these networks did (and still do today) is remarkable in a way that’s easy to take for granted.

Every time you tap your card at a merchant, a request travels from that merchant’s point-of-sale terminal to their acquiring bank, from the acquiring bank through the card network to your issuing bank, which checks your available spending power, assesses fraud signals, and sends back an authorization response. All this happens in under two seconds, across institutions that may have no direct relationship with each other, in any of 200+ countries, 24 hours a day, 365 days a year.

The network enforces shared rules for chargebacks, dispute resolution, fraud liability, and data security. It maintains regulatory credibility across jurisdictions. It provides the trust infrastructure that lets a merchant in Tokyo accept a card issued by a credit union in Boise, Idaho, without knowing anything about that credit union or needing a relationship with it.

This is the air traffic control system of payments. It is extraordinarily complex. It required decades of painstaking institutional construction. Getting it wrong means planes fall out of the sky, or in this case, fraud runs rampant, merchants stop accepting cards, and the whole system falls apart.

The 2-3% that issuers collect in interchange — which funds the rewards programs, fraud protection, and credit risk that consumers enjoy — is not a toll extracted by greedy middlemen (*which isn’t to say that Visa and Mastercard don’t flex their duopolistic power to their own benefit … they certainly do*).

Instead, interchange fees should be thought of as the operating cost of one of the most sophisticated trust networks ever built by human beings. It is, to be frank, impressive that it only costs 2-3%.

None of this is to say the authorization layer is perfect. It was designed decades ago, and it has begun to show its age in places.

The message format underpinning Visa and Mastercard’s authorization layer — ISO 8583, a standard that dates to 1987 — was built for a world of magnetic stripes and dial-up terminals. The result is a data layer that is remarkably thin by modern standards: merchant names get truncated to 25 characters, there’s no line-item or SKU-level data transmitted at the point of sale, and the information that flows between institutions is a fraction of what a modern payments system would capture if built from scratch today. Looking further ahead, the speed at which AI agents and machine-to-machine commerce will execute transactions will eventually stress authorization infrastructure that was optimized for humans tapping cards, not software executing thousands of micro-purchases per second. These are real limitations, and they’ll need to be addressed. But they’re arguments for evolving the authorization layer, not abandoning it.

The people who want to route around Visa and Mastercard with stablecoins haven’t really grappled with what they’d be routing around.

Building a new global authorization layer — with shared fraud rules, dispute resolution, regulatory credibility in every jurisdiction, and merchant acceptance at 150+ million locations — is not a problem that blockchain, by itself, can solve.

It’s an organizational and governance problem, and it took Dee Hock thirty years to solve it.

### SettlementCopy anchor linkCopied

Authorization was the first and most immediate problem that Dee Hock had to solve.

The second was settlement.

Settlement is what happens after the transaction is authorized. It’s the actual movement of money: the clearing of the transaction through the network, the transfer of funds from your issuing bank to the merchant’s acquiring bank, and the books finally balanced on both sides.

To settle accounts in the early BankAmericard days, the merchant bank had to physically mail crates of paper receipts and sales drafts to the cardholders’ banks across the country almost daily. The cardholders’ banks then had to manually match those drafts with customer accounts, reimburse the merchant bank, and bill the cardholders. Handling settlements across 150 banks, millions of cardholders, and billions of dollars without computers was daunting. Transactions piled up, customers went unbilled, and balancing issues were constant.

Dee Hock and Visa solved for these operational inefficiencies by turning to what was (at the time) the most sophisticated technology available: mainframe computers. Computer-based batch processing replaced paper crates. ACH — the Automated Clearing House network, built in the early 1970s — replaced the postal system as the underlying rail for moving money between banks.

It was, at the time, a meaningful improvement.

The trouble is that it’s now fifty years later, and the settlement layer of the modern card system is still, at its core, running on that same basic architecture.

When you tap your card at checkout, the authorization happens in under two seconds. But the actual movement of money is a completely separate process, one that happens hours later, in batches, on a schedule that stops on weekends and federal holidays. Captured transactions are grouped by daily cutoff time, sent through clearing, and the issuer transfers funds to the acquiring bank, typically within one to three business days.

Authorization happens at the speed of light. Settlement happens at roughly the speed of a 1970s back-office operation.

This is the baggage handling system of payments. Your flight lands in six hours. Your suitcase arrives in two days. Sometimes three. And sometimes it ends up in Frankfurt.

Nobody designed it to be this way. It’s a patchwork of legacy infrastructure built at different times, by different operators, with different standards, never quite unified into a coherent whole. The fact that it’s attached to a brilliant air traffic control system doesn’t make it a brilliant system. It just means that the two get conflated, which is exactly why so many people misdiagnose where the problem lives.

For domestic transactions, the multi-day settlement window is aggravating. Merchants factor it into their cash flow planning. Issuers hold capital buffers to bridge the float. For businesses managing tight cash flow, the difference between T+1 and T+2 settlement can require an additional $500K to $1M in credit facilities. This is not a fee anyone chooses to pay, but a structural cost of operating on rails that were never designed for the speed at which the network itself runs.

For global transactions, the problem is an order of magnitude larger. To understand why, you need to understand what happens when the settlement layer has to cross borders.

Imagine you’re running a large global money movement business, sending value from senders in the United States to recipients in Mexico, the Philippines, Nigeria, India, dozens of corridors, dozens of currencies. The authorization side of this is fairly well-solved. The settlement side is a completely different animal.

There is no single network that processes payments worldwide. Instead, most cross-border transactions rely on a system of correspondent banking; a web of intermediary banks maintaining accounts with each other across jurisdictions, each holding pre-funded reserves in foreign currencies to facilitate transfers. Before any money moves, you have to pre-position it. You fund accounts in foreign currencies across dozens of banks in dozens of countries, sitting idle, waiting to be used. If you over-fund a corridor, that capital earns nothing. If you underfund it, payments fail or get delayed. A treasury desk tries to predict future payment flows and balance the books in real time. A payment from a US bank to a Brazilian bank might flow through two or three correspondent banks before reaching the final recipient, with each hop adding time, fees, and FX conversion risk.

The BIS and World Bank have both [documented](https://www.bis.org/cpmi/publ/d147.htm) the [structural costs](https://documents.worldbank.org/en/publication/documents-reports/documentdetail/113021467990964789/withdraw-from-correspondent-banking-where-why-and-what-to-do-about-it) of this system at length. Collectively, banks hold hundreds of billions of dollars in pre-funded nostro and vostro accounts around the world, capital that sits idle, earning minimal returns purely to keep the pipes flowing. That is not interchange. That is not fraud protection. That is not dispute resolution. It is the pure cost of operating on a settlement infrastructure that was not designed for the world it now serves.

It’s important to remember that this is not a problem created by Visa or Mastercard. It predates them. The correspondent banking system was the only available infrastructure for moving value across borders when the global financial system was being built, and it hardened into the architecture of international finance over the course of the 20th century. It persists not because it’s optimal (*it absolutely isn’t*), but because the fundamental economic reality hasn’t changed: for any alternative system to succeed, it needs to reach a similar scale, but banks won’t adopt new systems until they connect enough counterparties to be useful. It’s a network effects problem. Everyone is trapped.

I spoke recently with an executive at a major global payments company, one that moves serious volume across dozens of international corridors. He put it in concrete terms: the correspondent banking layer, with its blend of pre-funding requirements, multi-day funding cycles, and FX treasury operations, costs his company somewhere in the range of 3-6% of capital cost across the business. At the scale this company operates, that’s not a rounding error. It’s an enormous structural drag, and crucially, it has nothing to do with the value the authorization layer provides.

It’s just the bill for the baggage handling.

### The Message and the MoneyCopy anchor linkCopied

Dee Hock had a vision that extended well beyond the operational problems he was solving in the 1960s and 1970s.

As he worked through the chaos of the early BankAmericard years, he kept returning to a more fundamental question: what is money, really?

Not coins, not paper, not credit cards. Strip it down to its essence, and money is information. It is a record of value, a guaranteed claim, a unit of trust between two parties. And if money is fundamentally information, then there is no reason, in principle, why it shouldn’t move the way information moves.

Here’s Hock in his absolutely incredible book — [*One From Many: VISA and the Rise of Chaordic Organization*](https://www.amazon.com/One-Many-VISA-Chaordic-Organization/dp/1576753328):

> Money would become nothing but alphanumeric data in the form of arranged energy impulses. It would move around the world at the speed of light at miniscule cost by infinitely diverse paths throughout the entire electromagnetic spectrum. Any institution that could move, manipulate, and guarantee alphanumeric data in the form of arranged energy in a manner that individuals customarily used and relied upon as a measure of equivalent value and medium of exchange was a bank. It went even beyond that. Inherent in all this might be the genesis of a new form of global currency.

He built half of that vision.

The authorization layer — VisaNet, the network rules, the global trust infrastructure — moves at something very close to the speed of light. A transaction is authorized in under two seconds across 200+ countries. The message, in other words, moves the way Hock imagined.

The money does not.

Settlement still runs on ACH batch cycles. Cross-border value still pools in pre-funded accounts across correspondent banking chains. The money moves at the speed of 1970s back-office infrastructure, because that’s the technology that existed when the system was built, and because nothing since has been sufficiently compelling to rip it out and replace it.

Until now.

Stablecoins are, in some ways, Hock’s vision for the settlement layer finally made technically possible. Guaranteed alphanumeric data, anchored to the dollar, programmable, moving across borders in seconds, settling finally.

The message and the money, at last, moving at the same speed.

Which is why, before we get into how this actually works in practice, it’s worth making sure we’re on the same page about what stablecoins actually are.

### A Quick Primer on StablecoinsCopy anchor linkCopied

A stablecoin is a cryptocurrency whose value is pegged to a stable asset, most commonly the US dollar. Unlike Bitcoin or Ethereum, which fluctuate wildly in price, a dollar-pegged stablecoin like USDC (issued by Circle) or USDT (issued by Tether) is always worth approximately one dollar.

The stability is maintained through reserves: the issuer holds actual dollars (or dollar-equivalent assets like Treasury bills) backing every coin in circulation. The result is something that behaves like a digital dollar. It can move across borders in seconds, settle on a blockchain without a bank in the middle, and be programmed with rules about how and where it can be spent, all while holding its value the way a dollar does.

That combination of stability, global interoperability, and programmability is what makes stablecoins interesting as payments infrastructure, rather than just another speculative asset.

### Stablecoins as a Card Settlement InfrastructureCopy anchor linkCopied

So what does it actually look like when you plug stablecoins into the settlement layer without touching the authorization layer?

The place to start is with Visa and Mastercard, because the companies’ choices over the last few years are one of the clearest signals that the settlement layer is still a work in progress.

In December of 2025, Visa [enabled](https://investor.visa.com/news/news-details/2025/Visa-Launches-Stablecoin-Settlement-in-the-United-States-Marking-a-Breakthrough-for-Stablecoin-Integration/default.aspx) direct on-chain settlement, allowing issuer and acquirer partners to settle obligations to the card network in USDC on the Solana blockchain rather than through traditional ACH rails (*the program had already reached a $3.5 billion annualized run rate before Visa even launched it formally in the US*).

More notably, Visa has opened membership to crypto-native companies ([*including Rain*](https://www.businesswire.com/news/home/20250324739515/en/Rain-Announces-$24.5-Million-in-Funding-Led-by-Norwest-to-Expand-Stablecoin-Powered-Card-Issuing-Globally)), allowing them to connect directly to Visa’s network without routing through a traditional issuing bank intermediary. That’s not a small thing. Visa spent decades carefully managing who got direct access to its network. The fact that it’s extending that access to stablecoin-native issuers tells you something about how Visa thinks the settlement layer should evolve.

Mastercard is moving in the same direction. In March 2026, Mastercard [launched](https://www.mastercard.com/global/en/news-and-trends/stories/2026/mastercard-crypto-partner-program.html) its Crypto Partner Program — a global initiative bringing together more than 100 crypto-native companies (*including Rain*), payment providers, and financial institutions, with the explicit goal of connecting digital asset infrastructure to established card rails. The program is built around Mastercard’s Multi-Token Network, its platform for real-time settlement across digital asset types, and is designed to let participants shape the direction of future Mastercard products at the intersection of stablecoins and global payments.

The two card giants are, in other words, racing toward the same conclusion: the settlement layer needs an upgrade, and stablecoins are the most credible candidate to deliver it.

The mechanics of how this works are worth understanding. A stablecoin-backed card presents to a merchant exactly like any other payment card. The authorization layer — the fraud rules, the dispute resolution, the regulatory credibility, the 150+ million merchant locations — is identical. What’s different is what happens underneath, after the authorization. When a cardholder funds their account with stablecoins, those funds are true funds: available instantly, settled on-chain, with complete line-of-sight from origin to destination. There’s no T+1 or T+2 window during which the issuer is extending implicit credit against a float. There’s no risk buffer required against the possibility that the settlement rails fail to deliver.

This changes the economics in a few concrete ways.

The most immediate is capital efficiency. The credit collateral that issuers must hold as a buffer against float risk — money that sits idle, earning nothing, existing purely to manage a multi-day settlement window — can be dramatically reduced when settlement is instant and final. For a business moving serious volume, that’s a significant amount of capital that can be deployed into more productive places.

The second is FX. As we discussed earlier, correspondent banking doesn’t just create capital costs. It puts FX conversion under the control of intermediaries, with margins embedded at each step in the journey. Move settlement on-chain, and you can start setting better exchange rates, blending yield from stablecoin holdings with FX economics in ways that simply aren’t possible when a series of correspondent banks control the terms.

The third benefit plays out at the consumer level, particularly for people in countries with volatile currencies. A stablecoin-backed card lets someone receive remittances in digital dollars, hold them in a stablecoin account that maintains USD value, and spend them via Visa wherever they need to, without converting to local currency until the moment of a specific transaction. The same treasury logic that makes stablecoins attractive for institutions plays out for individuals who have historically had no good way to hold dollar-denominated value without a US bank account.

### Two Ways to Think About DisruptionCopy anchor linkCopied

There are two fundamentally different ways to think about stablecoins as a disruptive force in payments.

The first is to treat them as a self-contained alternative to the existing system; a way to rewire the front, the middle, and the end of payments all at once. Issue a stablecoin. Drive consumer adoption. Drive merchant adoption. Make all the plumbing in between work.

This *feels* the most disruptive. It makes for a great story for investors. However, building a two-sided, closed-loop payments network is extraordinarily difficult.

You need consumers before merchants care, and merchants before consumers care, and you need to build the trust and regulatory infrastructure that the card networks spent fifty years constructing, and you need to do it while competing against an entrenched system that already works.

PayPal, which has more advantages for this kind of play than almost any other company on earth, is still trying to figure out how to make its global closed-loop payment strategy, powered by PYUSD, a reality. That tells you something about the degree of difficulty.

The second way to think about the disruptive potential of stablecoins is through an infrastructure lens. In this view, stablecoins are a new settlement primitive that makes existing systems work better, rather than a replacement for those systems.

This requires being honest about what stablecoins do well, what they don’t do well, and what they could theoretically do well if not for existing infrastructure that already does it well. It means acknowledging that the authorization layer built by the card networks works well and that the right move is to upgrade the layer underneath it, not tear the whole thing down.

That is the stablecoin-backed card story. It doesn’t ask merchants to change behavior. It doesn’t ask consumers to learn something new. It plugs a better settlement primitive into a network that already has 150+ million merchant locations and decades of accumulated trust from all participants.

The authorization layer stays as it is: functional, load-bearing, and worth 2-3%.

The settlement layer finally catches up.

This is profoundly disruptive. And if Dee Hock — who spent his career insisting that the function of money matters more than its form, and that the best infrastructure is the kind nobody notices — could weigh in, I suspect he’d agree.

---

**About Sponsored Deep Dives**

Sponsored Deep Dives are essays sponsored by a very-carefully-curated list of companies (selected by me), in which I write about topics of mutual interest to me, the sponsoring company, and (most importantly) you, the audience. If you have any questions or feedback on these sponsored deep dives, please DM me on [Twitter](https://twitter.com/AlexH_Johnson) or [LinkedIn](https://www.linkedin.com/in/alexhjohnson/).

Today’s Sponsored Deep Dive was brought to you by [Rain](https://www.rain.xyz/?utm_source=newsletter&utm_medium=email&utm_campaign=fintech_takes_april_26).

![](https://workweek-production.imgix.net/media/fintechtakes/c25325b9-da5b-4e25-b4db-e1a437905fbd/rain.png)

Rain’s stablecoin-native infrastructure powers everything from on-ramps and wallets to cards and off-ramps. Whether you’re a fintech, platform, or global institution, Rain gives you the tools to offer modern financial experiences without the baggage of legacy infrastructure.

---