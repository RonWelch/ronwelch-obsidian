---
title: Money’s Public Cloud Moment
source: https://fintechtakes.com/articles/2026-10-15/moneys-public-cloud-moment/?utm_source=tldrnewsletter#55294c15-c12f-41d6-bd23-897b9ca9c257-link
author:
  - "[[Alex Johnson]]"
published: 2026-10-15
created: 2026-10-19
description: It’s difficult to imagine today, but for most of my career in financial services, the term “public cloud” was legitimately anxiety-inducing. For bankers, the idea of putting their intellectual property and customer data on infrastructure that they didn’t own, in data centers they’d never set foot in, run by a company that also sold books […]
tags:
  - clippings
  - ArtificialIntelligence
  - agentic
  - BUGuestLecture
---
It’s difficult to imagine today, but for most of my career in financial services, the term “public cloud” was legitimately anxiety-inducing.

For bankers, the idea of putting their intellectual property and customer data on infrastructure that they didn’t own, in data centers they’d never set foot in, run by a company that also sold books and patio furniture felt reckless. As late as 2015, questions like, “ [What if cloud providers are *more* secure than banks?](https://www.americanbanker.com/news/what-if-cloud-providers-are-more-secure-than-banks)” were considered heterodox.

When banks started to tiptoe away from on-prem deployments and towards the cloud, progress was slow and incremental. Most of them insisted on a private cloud — dedicated servers, single-tenant, walled off from everyone else — because it felt safer. And it was, a little. But a private cloud throws away most of what makes the cloud worth having: The elasticity, the scale, the ability to spin something up in minutes instead of months. It’s the cloud with the good parts removed.

Then, all of a sudden, the dam broke.

Capital One started moving to Amazon’s public cloud in 2014 and, by 2021, closed its last data center to become the first big U.S. bank to go [all-in on the public cloud](https://www.capitalone.com/tech/cloud/serverless-first-strategy/). The journey wasn’t without a few bumps in the road. In 2019, Capital One suffered one of the [largest breaches in banking history](https://www.cnn.com/2019/07/29/business/capital-one-data-breach), the result of a misconfigured firewall at the bank, rather than a vulnerability in Amazon Web Services. However, rather than sending banks scurrying backwards to private clouds and on-prem deployments, the breach sharpened regulators’ focus on public cloud computing and motivated them to clarify how banks’ compliance obligations applied to this new deployment model. This, ironically, made operating in the public cloud safer and lower risk for banks moving forward.

Why am I telling you this?

Because everything that happened to banks and cloud computing — the initial resistance, the private-cloud half-measure, the capitulation once the public cloud became overwhelmingly better, and the new rulebook regulators had to write in response — is now happening to the way money moves across borders.

Stablecoins are the public cloud moment for global money movement.

### Money Has Two Layers

In a [previous essay](https://fintechtakes.com/articles/2026-04-30/the-message-and-the-money/), I borrowed an idea from Dee Hock, the founder of Visa:

**Strip money down to its essence and it’s just information, a record of value moving between two parties.**

Hock’s insight was that if money is information, it should move the way information moves. He built half of that vision. The message — the authorization that flies from a card terminal to your bank and back in under two seconds, anywhere on earth — moves at close to the speed of light. The money — the actual settlement — still crawls along on batch cycles and correspondent-banking plumbing designed in the 1970s.

To return to our analogy, every large company’s tech stack has two basic layers: A network layer that carries the instructions, and a compute layer that does the actual work. In payments, authorization is the network layer. Settlement — where money genuinely moves and the books balance — is the compute layer.

Visa and Mastercard spent decades turning authorization into a single rulebook that works instantly across 200-plus countries. The compute layer never made that leap. It stayed on-premises, in the most literal sense; a separate build, bank by bank, country by country, each institution standing up its own settlement machinery the way every company in the 1990s ran its own server room.

When people say stablecoins are going to “disrupt” global payments networks like Visa and Mastercard, they’re usually aiming at the wrong layer. VisaNet works well enough. Stablecoins are the cloud moment for the compute layer. The chance to stop rebuilding the settlement rack in every country and rent a single shared one instead.

### The Cost of On-Prem

To understand why this matters, we need to think, in concrete terms, about the cost.

Start with correspondent banking, the system that moves money across borders today. To send value from the U.S. to, say, the Philippines, you don’t just send it. You pre-position it. Banks hold money in accounts in foreign currencies, in dozens of countries, sitting idle, waiting to be used. Over-fund a corridor and that capital earns nothing. Under-fund it and payments fail. Collectively, the industry parks hundreds of billions of dollars in these accounts purely to keep the pipes full and the machinery working smoothly.

Trace a single payment and the waste becomes obvious. A dollar leaving a U.S. bank for a merchant in Southeast Asia might touch two or three correspondent banks on the way, each one debiting and crediting accounts, each one taking a small cut, each one adding a day.

Somewhere in that chain the dollars get converted into the local currency — often by passing through a third currency first, because no direct relationship exists between the two ends — and every conversion is a spread that somebody else pockets.

If you’ve ever run IT infrastructure, you’ll immediately recognize this for what it is. It’s the rack of servers humming along at 20% utilization that you bought anyway, because you couldn’t risk running out of capacity when traffic spiked. It’s over-provisioning.

An executive at one large global payments company told me that between the idle pre-funding, the multi-day cycles, and the FX costs, his correspondent-banking setup costs the business somewhere in the range of 3–6% of its capital.

Then there’s the schedule. Your settlement rail stops on weekends and bank holidays. A long holiday closure can leave a company with nine figures of value hanging in the wind, unsettled, because the bank on the other end is simply closed. Bigger companies will keep big pots of money staged around the world just to absorb that risk.

Stack it all up and you understand why, historically, only the largest companies on earth routinely moved money across a large number of different countries. The economics didn’t scale. Every new country required a fresh install: A new bank partner, a new onboarding, a new integration, a new set of local licenses.

### The Private Cloud of Money

If stablecoins are the public cloud of payments, and correspondent banking is on-prem, then the payments equivalent of private cloud is tokenized deposits.

In simple terms, tokenized deposits are a representation of a bank’s own deposits as tokens on a blockchain that it controls. It allows the bank in question to move its own money across its own accounts, instantly, around the clock. That might not sound like all that much, but for sufficiently large and complex banks, the internal operational efficiency gains can be meaningful. J.P. Morgan’s platform, [Kinexys](https://www.jpmorgan.com/payments/newsroom/harvard-business-school-case-study-kinexys), has processed more than $4 trillion since 2020 and now averages north of $7 billion a day. That is real volume, settling in real time, all inside the bank’s own regulated walls.

The problem is that tokenized deposits only move money between accounts on that one bank’s ledger. They can’t reach a counterparty who isn’t a customer. This is the same essential limitation of private cloud: Dedicated, controlled, efficient within its own four walls, but cut off from the network effects that make the shared version powerful.[^1]

In payments, we are starting to see the emergence of similar collaborative (but still fundamentally closed) networks, such as the [tokenized deposit network](https://www.forbes.com/sites/digital-assets/2026/07/28/americas-biggest-banks-are-building-one-deposit-token-history-is-the-hard-part/) that JPMorgan, Bank of America, Citi, and Wells Fargo are building together through the bank-owned Clearing House. These bank consortium efforts are clearly a defensive response to the growth of open-network stablecoins, which tells you something about the direction that the market naturally wants to go.

### The Public Cloud of Money

In order for public cloud computing to finally get traction with banks, it needed two things. It needed to become sufficiently safe (thus limiting banks’ downside) and overwhelmingly better (thus giving banks enough motivation to make the switch).

On the safety front, the growth of public cloud providers outside of financial services played an important role. Hyperscalers pointing multi-billion-dollar budgets at one problem (how to keep their public cloud environments secure and performant) trumped the investments that any one bank (even the biggest) could make to their own infrastructure. Additionally, a marquee, heavily regulated institution (Capital One) went first, which, if you’ve spent any time around banks you’ll know, is a big unlock.[^2] Even when the 2019 breach happened, the failure being on the bank’s side rather than AWS’s side strangely helped to validate the model rather than indict it.

And on “overwhelmingly better,” at some point in the 2010s it became clear that a hyperscaler could provision in minutes what used to take months of hardware procurement and, as a result, ship improvements many times a day instead of once a quarter. This put frontier capabilities within reach — elastic scale that flexes with demand, managed data and machine-learning tools, an instant global footprint — that no single bank could build or keep current on its own. With on-prem and private cloud, you were frozen at whatever you last bought and paid to maintain. With the public cloud, the platform raced ahead on your behalf, and the distance between the two compounded every year you waited.

Stablecoins are clearing the same two bars.

On safety, the [GENIUS Act](https://www.paulhastings.com/insights/crypto-policy-tracker/the-genius-act-a-comprehensive-guide-to-us-stablecoin-regulation), signed in July 2025, did for stablecoins what certifications and a marquee early adopter did for public cloud computing in banking. Requirements to get licensed, hold one-to-one reserves in cash and Treasuries, make monthly disclosures, and implement real anti-money-laundering programs all serve to make stablecoins, as payments infrastructure, safe enough for an executive at a regulated incumbent to push for without feeling like they are gambling with their career.

On “overwhelmingly better,” the gains are the capital freed when settlement is instant and final, when FX risk can be managed directly on chain instead of through a web of expensive intermediaries, and when one settlement asset (a dollar-backed stablecoin) works everywhere.

Combined, these benefits make it much cheaper and much faster to operationalize cross-border money movement and to stand up new, multi-country financial products and services. For example, in researching this essay, I spoke with a major remittance provider that tried to launch a debit card. The traditional approach — working through an in-country sponsor-bank — got them into one country. Working with a stablecoin-native card issuer, the same company now offers a card in a dozen countries.

One country to twelve countries: That’s the moment when the velocity gap gets too wide to ignore.

The appetite for moving money across borders is remarkably consistent. People and businesses have always needed to pay each other across lines on a map. What’s changing now is the cost to serve them. Drop that cost far enough and whole categories of customers that were never worth a per-country build-out suddenly become viable. Even better, ==🟢new companies can start out being global on day one; they can have customers, contractors, and suppliers on four continents before they have fifty employees.==

That’s a huge shift in the status quo. The old model required companies to either stay small or spend years and millions wiring up a bank partner in every market. The new model removes those constraints.

### Visa and Mastercard are the New Microsoft

Every disruption story needs an incumbent willing to cannibalize itself before someone else does.

In computing, that was Microsoft: It built the lucrative on-prem software era, then under Satya Nadella, bet the company on the cloud in 2014, pushing its own customers off perpetual licenses and onto subscriptions. It was painful, controversial, and, as it turns out, exactly the right thing to do.

The card networks are running that same playbook. Visa and Mastercard built the authorization layer — the global rulebook, the fraud machinery, 150-million-plus merchants — and are now rebuilding the settlement layer underneath it, even though stablecoin settlement cuts against the float economics of many of their own issuers. Incumbents don’t do that unless they’ve decided the shift is real and would rather lead it than be disrupted by it.

The networks first cracked their doors to crypto-native, non-bank issuers back in 2020, quietly breaking a decades-old posture of letting only banks be principal members. Then they aimed at settlement itself. Visa ran its first stablecoin settlement pilot in 2021, added chains and acquirers, and by the end of 2025 was [settling USDC for US banks in its core operations](https://corporate.visa.com/en/sites/visa-perspectives/newsroom/visa-launches-stablecoin-settlement-in-the-united-states.html). Mastercard went further, [acquiring](https://thenextweb.com/news/mastercard-bvnk-18bn-stablecoin-acquisition) the stablecoin infrastructure firm BVNK for $1.8 billion. That is not dabbling. Those are trillion-dollar networks deciding the compute layer needs to be rebuilt and racing to be the ones who rebuild it.

This matters because the networks are the market’s comfort mechanism. A blessing from Visa and Mastercard tells every risk-averse compliance officer that stablecoin settlement is safe to touch.

### We Still Have Work to Do

One of the sources I spoke with while researching this essay summarized the benefits and challenges of the current moment we’re in, telling me that “it’s much easier to build a global KYC network than a global network of local bank partners.”

I think that’s the correct read.

Painstakingly assembling a decentralized network of bank partners around the world is prohibitively expensive for almost every company that needs to move money across borders (which, in 2026, is most companies).

By centralizing settlement on a dollar-denominated payment rail that is instant and global by default, we can significantly reduce those costs. However, it’s important to acknowledge that doing so will not magically make every single expense and point of friction in cross-border money movement disappear. Nor should we hope for that outcome.

For a good example, look at a common last mile of a cross-border payment.

A stablecoin can move from one side of the world to the other in seconds, but the moment it has to become local cash in someone’s hand, a licensed, in-country partner has to do the off-ramp, handling the currency conversion, the local licensing, the consumer-protection rules that govern money entering that particular economy. No payment rail (no matter how technically sophisticated) makes that step vanish, and we wouldn’t want it to. The checks that happen where money touches down in a local market are how anti-money-laundering, sanctions enforcement, and consumer protection actually function. That friction is important and durable. What stablecoins change isn’t whether those obligations exist. It’s where they sit. Instead of every company rebuilding the whole apparatus in every market, the burden concentrates at the conversion points and is handled by specialists who are close to the local rules.

The deeper challenge isn’t the costs and friction stablecoins leave behind. It’s the new risks they introduce.

Pulling what used to live in a thousand different places onto one shared infrastructure creates massive, rapidly compounding advantages. However, it is also, inescapably, what concentrates the risk. You trade a sprawling, redundant, inefficient web of correspondent banks for a handful of critical platforms.

This move towards a more efficient, centralized settlement system is inevitable. Stablecoins are already safe enough, and the economics already good enough, that the market is going to move in this direction regardless of how quickly regulators move with it. So the interesting question isn’t whether this happens. It’s whether the framework for deciding who’s accountable when something breaks can catch up to a thing that’s already underway.

Public cloud computing in banking faced exactly that question not long ago, and how it got answered tells us a lot about where we’re going with stablecoins and the work that still needs to be done.

After the Capital One breach made the stakes obvious, banking regulators didn’t ban public cloud computing; they spelled out who owns what. The [FFIEC’s 2020 cloud guidance](https://www.ffiec.gov/sites/default/files/media/press-releases/2020/2020-april-30-cloud-computing-statement.pdf) created an explicit shared-responsibility model for banks and their infrastructure providers. It articulated how the division of duties between a bank and its cloud provider had to be understood, written into the contract, and actively managed. The line differed depending on how much a bank chose to outsource. Rent the raw infrastructure and you own more of the controls (and the risk of them not working). Buy finished software that is delivered as a service and the provider owns more of the controls (and risk). However, one thing never changes: The bank retains overall responsibility for the safety and soundness of the whole arrangement.

You can outsource the controls. You cannot outsource the accountability.

This shared-responsibility model has worked well so far, but it’s important to note that one of the reasons for that is that everyone sits inside the same regulatory perimeter. The end customers using the public cloud environments are regulated banks. And one of my personal favorite banking laws — [the Bank Service Company Act](https://scholarship.law.duke.edu/cgi/viewcontent.cgi?article=4211&context=dlj) — empowers banking regulators to reach straight through the banks to examine the technology providers they rely on, as if the banks themselves were doing the work. The public cloud provider was reachable because their customers were regulated. The contract divided the labor and regulation stood behind it. That backbone is what turned private contractual agreements into something with teeth.

Now let’s apply that lens to stablecoins and cross-border money movement.

The GENIUS Act did the “safe enough” part and pulled stablecoin issuers inside the Bank Secrecy Act as a new kind of covered financial institution. But the specifics, the part that actually assigns the compliance duties, are still being written. FinCEN and OFAC [proposed](https://www.federalregister.gov/documents/2026/04/10/2026-06963/permitted-payment-stablecoin-issuer-anti-money-launderingcountering-the-financing-of-terrorism) AML and sanctions rules in April 2026; the July 2026 deadline to finalize them came and went; and as I write this, every major implementing rule is still sitting at the proposal stage. And those proposed rules land on the stablecoin issuer (the entity that mints the token). They don’t reach down the stack to the infrastructure provider, and they don’t reach the companies actually putting stablecoins to work out in the market at all.

That last part is key.

The shared-responsibility model for public cloud computing worked because the providers could always be reached through their customers who were, themselves, regulated banks. In stablecoins, the entities out at the edge — the non-bank fintech company, the wallet, the platform operating in another country, and eventually the person holding the tokens — are not banks. The shared-responsibility matrix can be drawn up on paper, in contracts between an infrastructure provider and its customers. What it cannot yet do is what the cloud version did: Rely on a regulator’s legal ability to reach every party in the chain.

So where does that leave us?

Well, to be honest, there’s plenty of work left to do.

But it’s the best kind of work, the kind we only get to do because the hard part is already behind us. The technology is ready. The business case is clear and compelling. And the pieces of an accountability framework are starting to come together. Rulemaking is underway and banks and other market incumbents are pressure-testing this new infrastructure at a scale that we haven’t seen before.

Cloud computing went through this exact journey — resistance, then clarity, then a shared rulebook everyone could build on — and came out the other side stronger for it. There’s no reason to think money won’t do the same.

---

**About Sponsored Deep Dives**

Sponsored Deep Dives are essays sponsored by a very-carefully-curated list of companies (selected by me), in which I write about topics of mutual interest to me, the sponsoring company, and (most importantly) you, the audience. If you have any questions or feedback on these sponsored deep dives, please DM me on [Twitter](https://twitter.com/AlexH_Johnson) or [LinkedIn](https://www.linkedin.com/in/alexhjohnson/).

Today’s Sponsored Deep Dive was brought to you by **[Rain](https://www.rain.xyz/?utm_source=newsletter&utm_medium=email&utm_campaign=fintech_takes_october_15)**.

![](https://fintechtakes.com/wp-content/uploads/sites/8/2026/10/rainlogo.png)

Strip money down to its essence and it’s just information, a record of value moving between two parties.

[Rain](https://www.rain.xyz/?utm_source=newsletter&utm_medium=email&utm_campaign=fintech_takes_october_15) is the global stablecoin payments platform for enterprises, neobanks, platforms, developers, and AI agents. Our technology allows partners to move, store, and use stablecoins instantly and compliantly through global payment cards, rewards, on/offramps, wallets, and cross-border rails. As both a Visa and Mastercard Principal Member, Rain issues cards that work at more than 175 million merchant locations in over 220 countries and territories. Built natively for stablecoins and trusted by more than 100 organizations worldwide, Rain delivers secure, scalable infrastructure that makes money move freely and instantly around the world.

---

[^1]: Banks also like deposit tokens for reasons that have nothing to do with the rest of us: They stay on the balance sheet, carry friendlier regulatory treatment, and can even pay interest, which a payment stablecoin under current U.S. law cannot. However, these are artificial regulatory tailwinds, and those can (and will) shift on a moment’s notice.

[^2]: The old joke in bank vendor circles is that banks always want to be first to go third.