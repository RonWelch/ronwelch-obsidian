---
title: "The Stablecoin Moment"
source: "https://x.com/zauthinc/article/2049887609552584800"
author:
  - "[[zauth (@zauthinc)]]"
published: 2026-04-30
created: 2026-05-04
description: "In 1997 the internet reserved a status code for payments and forgot about it for 28 years. That just changed. What we are covering:>The $32..."
tags:
  - "clippings"
---
![Image](https://pbs.twimg.com/media/HHIzFR3agAEIOf0?format=jpg&name=large)

In 1997 the internet reserved a status code for payments and forgot about it for 28 years. That just changed.

What we are covering:

\>The [$325B](https://x.com/search?q=%24325B&src=cashtag_click) moment >The law that changed everything >Why credit cards can't serve the agent economy >x402 >The four competing protocols >Mert's endgame thesis >What Armstrong, CZ, and Toly are all saying >The honest picture on adoption >Where this is going

## The $325 Billion Moment

Stablecoins just crossed $325 billion in total market cap.

That number alone does not tell the story. What tells the story is the velocity: **$46 trillion in transaction volume** processed last year. B2B stablecoin payment volume grew 87% in 2025, reaching $83.1 billion. Consumer-to-business payments grew 131%. USDC and USDT now have a combined market cap of $260 billion, roughly three times their 2023 value.

For context: Visa processed about $16 trillion in payments in all of 2024. Stablecoins are already moving three times Visa's volume.

But the more important question is not where stablecoins have been. It is where they are going, and why the smartest people in both AI and crypto are converging on the same answer.

## The Law That Changed Everything

On July 18, 2025, President Trump signed the GENIUS Act into law. It is the first comprehensive federal regulatory framework for stablecoins in U.S. history, and it removed the single biggest blocker to institutional adoption.

The core change is simple: stablecoins are now legally classified as **payment instruments.** Not securities or commodities, but payment instruments. Banks can now hold them, issue them, and settle with them. The infrastructure that had been waiting on the sidelines now has legal permission to move.

Issuers must maintain 1:1 reserves in liquid assets, publish monthly disclosures, and comply with standard anti-money laundering rules. The rules are clear. The rails are open.

Everything being built right now is being built on that foundation.

## Why Credit Cards Cannot Serve the Agent Economy

This is the part most people miss.

The stablecoin narrative has historically been about cross-border payments, DeFi, and remittances. Those use cases are real. But the structural opportunity being built right now is something different: payments for a world where software pays software.

AI agents cannot open bank accounts. They have no SSN, no passport, no legal identity, and no way to satisfy KYC requirements at a traditional financial institution. Coinbase CEO [@brian\_armstrong](https://x.com/@brian_armstrong) put it plainly on [X](https://x.com/brian_armstrong/status/2031021867973194172):

> "Very soon there are going to be more AI agents than humans making transactions. They can't open a bank account, but they can own a crypto wallet. Think about it."

The fee structure of traditional payment rails makes the problem worse. Card interchange runs 1.5 to 3.5% plus **$0.30 per transaction.** That fixed floor destroys any micropayment business model before it starts. If an AI research agent calls a specialized data API fifty thousand times in a session, running those calls through a card service costs more than 100 times the value of each individual transaction. The economics do not work.

Stablecoin rails flip this entirely. For example, [@solana](https://x.com/@solana) processes transactions at $0.00025 each. x402, Coinbase's payment protocol built on top of HTTP, settles in under 200ms for approximately $0.0001 per transaction. A session of fifty thousand API calls costs five dollars, not thousands.

Stripe co-founder [@collision](https://x.com/@collision) said in February he expects a "torrent of agentic commerce" to emerge, enabled by stablecoins and high-speed blockchains. The infrastructure is being built specifically for this moment.

## x402: The Payment Primitive Being Built Into the Internet

In 1997, the HTTP specification reserved status code 402 "Payment Required" for future use. It sat dormant for twenty-eight years. Nobody had figured out how to make internet-native payments actually work, so the placeholder just sat there.

[@coinbase](https://x.com/@coinbase) **activated it in May 2025**.

x402 is an open payment protocol that embeds stablecoin payments directly into HTTP, the same protocol that powers every website you visit. The mechanic is simple: a server returns a 402 status code alongside payment requirements. The client, whether a human developer or an AI agent, pays in USDC. The server verifies and delivers. No accounts, sessions, checkout forms, or API keys.

The x402 team at Coinbase laid out the vision when they introduced the protocol:

> "Agents need atomic payments, programmable policies, and composable wallets. Ethereum and stablecoins give them exactly that."

They imagined everything from self-driving taxis paying tolls autonomously to apps permanently storing data on-chain with stablecoin micropayments. The core team: [@programmer](https://x.com/@programmer) [@johnp2879](https://x.com/@johnp2879) [@carsonroscoe7](https://x.com/@carsonroscoe7) [@R\_onchain](https://x.com/@R_onchain) [@MurrLincoln](https://x.com/@MurrLincoln) and [@kleffew94](https://x.com/@kleffew94)

Erik Reppel, creator of x402 and Head of Engineering at Coinbase's Developer Platform, described the bigger picture to [Coindesk](https://www.coindesk.com/business/2026/03/14/ai-developers-may-not-be-keen-on-crypto-but-stablecoins-are-the-secret-to-agentic-finance-crypto-insiders-say):

> "I think the thing people haven't quite realized is that we're going to break the fundamental economic model of the internet, moving from browsers and you visiting the website of the person who's publishing content, to consuming things through your agents and your chat interface."

On stablecoins specifically:

> "Anyone can program stablecoins. Anyone in the world can spin up as many wallets as they want, and use wallets as the way to fully isolate funds for an agent. What we want is agents to have isolated, programmable funds, where your agent can't spend into your credit card limit and can't access your credit card."

On latent demand:

> "There's probably a very large latent demand for products that just wasn't expressible because of how API keys, subscriptions, and fees on microtransactions has worked."

The institutional backing assembled around x402 is hard to ignore. The x402 Foundation now operates under the Linux Foundation, with support from [@Google](https://x.com/@Google), [@Microsoft](https://x.com/@Microsoft), [@awscloud](https://x.com/@awscloud), [@Visa](https://x.com/@Visa), [@Mastercard](https://x.com/@Mastercard), [@Cloudflare](https://x.com/@Cloudflare), [@Shopify](https://x.com/@Shopify), [@stripe](https://x.com/@stripe), [@circle](https://x.com/@circle), [@SolanaFndn](https://x.com/@SolanaFndn), and [@AmericanExpress](https://x.com/@AmericanExpress).

Coinbase just launched [Agentic.market](https://agentic.market/) as a discovery layer, giving humans and AI agents access to thousands of x402-compatible services with zero API keys required. As of launch, approximately **69,000 active AI agents have processed over 165 million transactions totaling $50 million in volume.**

Brian Armstrong on April 20:

> "We launched [Agentic.market](https://agentic.market/) to give agents a discovery layer to find and integrate x402 services seamlessly. Add the skill to your agent. And list your services to start earning."

On [April 16](https://coinedition.com/agentic-commerce-could-drive-digital-dollar-demand-says-armstrong/):

> "Agentic commerce isn't priced in yet. Machine-to-machine payments will increase demand for the digital dollar beyond current estimates. The agentic economy could be larger than the human economy. We're building the infrastructure for both at Coinbase."

## The Four Protocols Fighting to Become the Standard

x402 is not the only protocol. Four distinct standards are emerging, each with different assumptions.

**x402 (Coinbase):** Crypto-native, permissionless, pay-per-request. Best for agent-to-agent and API micropayments. Settles in under 200ms at ~$0.0001. 165M+ transactions processed. Supported across [@base](https://x.com/@base), [@solana](https://x.com/@solana), [@0xPolygon](https://x.com/@0xPolygon), [@ethereum](https://x.com/@ethereum), [@arbitrum](https://x.com/@arbitrum), [@SkaleNetwork](https://x.com/@SkaleNetwork), and more.

**ACP (Stripe + OpenAI):** The Agentic Commerce Protocol. Powers Instant Checkout inside ChatGPT. Merchant-focused and checkout-oriented. Early partners include Microsoft Copilot, [@AnthropicAI](https://x.com/@AnthropicAI), [@perplexity\_ai](https://x.com/@perplexity_ai), [@vercel](https://x.com/@vercel), [@Lovable](https://x.com/@Lovable), and [@Replit](https://x.com/@Replit).

**AP2 (Google):** The Agent Payments Protocol. Enterprise trust and cryptographic authorization. Emphasizes auditable trails and delegation. x402 operates as the stablecoin settlement layer within AP2.

**MPP (Stripe + Tempo):** The Machine Payments Protocol, live since March 18, 2026. Session-based and streaming. Bridges stablecoins and fiat by letting agents pre-approve a spending limit and stream micropayments. Connects crypto rails with Visa and Mastercard.

These are not mutually exclusive. x402 handles machine-to-machine micropayments. ACP handles merchant checkout. AP2 handles enterprise authorization. MPP bridges agents into fiat infrastructure. The likely outcome is coexistence.

## Mert's Endgame Thesis

Mert Mumtaz, [@mert](https://x.com/@mert), is the co-founder and CEO of Helius, the leading Solana developer infrastructure company and a clear voice on where the stablecoin market actually ends up.

His read is not about which stablecoin wins. His read is that the question becomes irrelevant.

In September 2025, as the Hyperliquid USDH bidding war exposed how commoditized the stablecoin issuance game had become, Mert posted a thesis that has aged well:

> "The eventual endgame is that you don't see the ticker at all. The apps will just display 'USD' instead of USDC, USDT, or USDX, and they will swap everything in the backend via a standardized interface." [source](https://www.tradingview.com/news/cointelegraph:01f808bac094b:0-the-endgame-for-us-dollar-stablecoins-is-no-tickers-web3-exec/)

This is the abstraction layer thesis. Stablecoins win not by one issuer dominating but by the category becoming invisible infrastructure. The user sees "USD." The AI agent sees "USDC on Solana" or whatever rail is cheapest at that millisecond. The plumbing routes automatically.

Mert also flagged the liquidity fragmentation risk: as more companies issue their own stablecoins and payment chains, capital gets trapped in those ecosystems. The solution is exchanges and protocols that accept all stablecoins and convert in the backend. That is already happening.

## What the Loudest Voices Are Saying

The convergence of opinion across people who are rarely aligned on anything is worth noting.

**Brian Armstrong (Coinbase CEO)**:

> "Very soon there are going to be more AI agents than humans making transactions. They can't open a bank account, but they can own a crypto wallet."

Coinbase's Agentic Wallets have processed 50 million machine-to-machine transactions since late 2025. Stablecoin payments is one of his top three priorities for 2026. [@cz\_binance](https://x.com/@cz_binance) **(Binance founder)** on [March 9](https://x.com/cz_binance/status/2030984617109664061), one of the most shared posts in crypto this year:

> "AI agents will make 1 million times more payments than humans, and they will use crypto." In an interview with Chamath Palihapitiya: "They will book restaurants. They'll pay for hotels for us. AI agents don't have an identity, forcing them to use digital assets like stablecoins that can operate autonomously, at high frequency."

[@toly](https://x.com/@toly) (Solana cofounder):

> "$1T+ stables" by 2026.

A bet that the stablecoin market triples in under a year, driven by agent commerce and institutional on-ramps. JPMorgan estimated $500-600B by 2028. Toly is calling more than double that by year end.

[@jerallaire](https://x.com/@jerallaire) **(Circle CEO)** on the Q1 2026 earnings call:

> "Stablecoins are becoming the native currency of machine-to-machine commerce. We're building a new internet financial system at the convergence of AI, stablecoins, and blockchain."

[@RyanSeanAdamsX](https://x.com/@RyanSeanAdamsX) **+** [@TrustlessState](https://x.com/@TrustlessState) **(Bankless)** in their [2026 predictions breakdown](https://www.bankless.com/podcast/12-big-crypto-predictions-for-2026-ryan-david): David summarized the consensus from Galaxy, Coinbase, a16z, and Bitwise:

> "The broad strokes of everyone's prediction is that the real payment rails come in 2026." Ryan: "It's a congressional bill. It's going to trillions of dollars. It powers everything. That's going to be the future."

[@a16zcrypto](https://x.com/@a16zcrypto) in their [2026 trends piece](https://a16zcrypto.com/posts/article/trends-stablecoins-rwa-tokenization-payments-finance/)

> "In 2026, emerging primitives like x402 make settlement programmable and reactive: agents paying each other for data, GPU time, or API calls instantly and permissionlessly, without invoicing, reconciling, or batching."

Their broader thesis named a shift from Know Your Customer to **Know Your Agent** as the identity framework the industry needs

## The Honest Picture on Adoption

The narrative is running ahead of the commerce. That is important to say clearly.

According to [@artemis](https://x.com/@artemis), x402 processes around $50,000 in daily volume. The protocol has support from every major player, but real, organic commerce-driven volume is still early.

Early-stage infrastructure always looks like this. TCP/IP did not have meaningful traffic on day one. The comparison that matters is the merchant wave. a16z said it cleanly: "Cards serve every merchant a processor can underwrite. Stablecoins serve every merchant a processor cannot." The businesses that will use stablecoin rails largely do not exist yet. They are being started right now by developers who have never set up a merchant account and never plan to.

Stablecoin supply is projected to reach **$420 billion by year end,** representing 56% growth from today. Agentic payments and machine-to-machine commerce are cited as the primary growth drivers.

## Where This Is Going

The rails are stablecoins. The protocol is x402 - intertwined with adjacent developments. The law is the GENIUS Act. The infrastructure that needed to exist now exists. What comes next is verification, trust, and ultimately commerce at a scale the internet has never seen.

The internet was built without a native payment layer. AI agents exposed that gap. Stablecoins are filling it.