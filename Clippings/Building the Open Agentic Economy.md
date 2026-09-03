---
title: Building the Open Agentic Economy
source: https://www.circle.com/blog/building-the-open-agentic-economy
author:
  - "[[circle.com]]"
published: 2026-08-12
created: 2026-08-18
description: Circle's vision for an open economy where agents are a new form of labor, published as easily as websites, discovered and ranked on merit, and paid for the work they perform.
tags:
  - clippings
  - ArtificialIntelligence
  - agentic
  - BUGuestLecture
---
Circle's vision for an open economy where agents are a new form of labor: published as easily as websites, discovered and ranked on merit, and paid for the work they perform.

![Building the Open Agentic Economy](https://cdn.prod.website-files.com/67116d0daddc92483c812ead/6a7c7fa29a422d1504c94af5_Blog_agentic-economy-vision.jpg)

Building the Open Agentic Economy

## Intelligence, a new building block

Intelligence is the internet’s next building block. It is turning software from a tool people operate into labor they can delegate to: give an agent an objective, and it determines how to complete the work.

The internet has repeatedly advanced by democratizing new “primitives” or building blocks, equipping everyone with a new capability that had previously belonged to a few. Webpages let anyone publish to a global audience, and within fifteen years the web held 100 million sites \[1\]. APIs let software build on other software; Amazon’s 2002 mandate helped make composable services the default, first inside the company and then across the web \[2\]. These primitives mattered because they were permissionless, so the barriers to using them collapsed and the economy formed at these new edges.

Intelligence changes what software can sell. Traditional software sells access to a tool; an agent can sell the work itself. Builders can now package expertise from tuned models, proprietary tools, data, or distilled context into an agent that runs on its own, then charge whenever that expertise is applied, whether by call, task, or outcome \[3\].

Today, many capable, specialized agents remain siloed inside companies or tied to closed platforms. The next step is to give intelligence the same open distribution the internet gave content and software: let any builder publish an agent and any buyer discover and pay for its work. Once that becomes possible, the constraint shifts from creating agents to navigating the market they form.

## Navigating the new economy

In an economy where anyone can publish an agent, there will soon be more options than any buyer can evaluate. How does a buyer find the right agent without reviewing thousands? How does a new builder with a better agent get discovered before anyone knows its name?

The web faced the same problem in the 1990s. AOL offered a curated bundle of web destinations, while the open web let anyone publish. A curated catalog could grow only as fast as its staff could evaluate and add new destinations, and only in directions aligned with its commercial incentives. The open web removed both constraints: anyone could publish, so it grew at the speed of everyone building on it rather than the team maintaining the directory.

Two layers made that openness usable: the browser gave people access to any published website, and search helped them navigate the abundance. Users no longer needed to know which sites were relevant in advance; rankings emerged from signals across the open web rather than a platform’s preselected catalog. Publishing remained permissionless, and new entrants could earn distribution without first being chosen by a gatekeeper.

Today’s assistants, including Claude and ChatGPT, already resemble browsers: they fetch information from the web, choose sources, and carry context across a session. But when they need to use external services, their reach is still largely bounded by native tools and connectors or whatever a person has configured in advance. Coding agents point toward a more open model because they can browse, install packages, and choose new resources as the task unfolds.

Give an agent the browser’s shape and it can reach anything published online, with no connection arranged in advance. Unlike a browser, an agent can decide where to go and, with a stablecoin balance, pay when it gets there. Any service that names a price, whether an API, a dataset, or another agent’s labor, then becomes available on demand.

That changes what an agent opens onto. It is not a catalog of pre-approved integrations, but an economy whose breadth is determined by everything people make available. Anyone can publish an agent or service, be discovered and ranked on merit, and get paid without petitioning a closed platform or waiting for a human to wire it up.

Native payment also changes the economics of this market. Without a simple way to charge per use \[4\], much of the web monetized attention, and value pooled with the platforms that routed it. Per-task payment sends value directly to providers, including specialized services, and gives discovery a new input: what buyers actually chose to pay for.

For this economy to remain open, its identity, payment, and discovery layers must be shared rather than controlled by a single platform.

| Agents today  reach what a human wires in | Agents in the open economy  reach anything they can pay for |
| --- | --- |
| Built-in tools only  skills and connectors wired in by hand | One balance of digital dollars  with human-set spend limits and controls |
| An account with every service  a sign-up and a plan per provider | Discovers what the task needs  services, data, and other agents' labor |
| Credentials managed by the human  API keys created, pasted, rotated | Pays per use, no accounts  APIs, agent labor, and inference |
| Inference on subscription  capacity paid for even when idle | Earns to the same balance  when its own work is worth paying for |

*Agents today vs. agents in the open economy: access wired in by hand vs. a balance that puts anything payable in reach.*

## The stack: what an open agent economy requires

The browser alone did not make the web an open ecosystem. Shared standards for naming, transport, formats, security, and discovery made universal reach usable. The agent economy needs its own stack of open internet protocols and blockchain-based trust primitives for identity, reputation, validation, and settlement.

<table><colgroup><col> <col> <col></colgroup><thead><tr><th>The agent economy needs</th><th>Where it stands today</th><th>The web's version</th></tr></thead><tbody><tr><td colspan="3">Foundation: exists today, on open standards</td></tr><tr><td><strong>Identity:</strong> ERC-8004 (agentId + agentURI)</td><td>Open standard · registries live</td><td>DNS + TLS certificates</td></tr><tr><td><strong>Value-transfer protocol:</strong> x402 / MPP</td><td>Open protocols · in use</td><td>HTTP: information transfer</td></tr><tr><td><strong>Capability format:</strong> OpenAPI / agentURI</td><td>Open standard · live</td><td>HTML</td></tr><tr><td><strong>Settlement:</strong> Stablecoins (e.g., USDC)</td><td>Open money / blockchains · live</td><td>(no analog; card rails were the stopgap)</td></tr><tr><td colspan="3">Trust primitives: emerging</td></tr><tr><td><strong>Reputation:</strong> a behavior-grounded signal over identity</td><td>Registry live · signal missing</td><td>Reviews and domain reputation, fragmented across platforms</td></tr><tr><td><strong>Validation:</strong> ERC-8004 + verifiable compute</td><td>Emerging</td><td>(no analog; agents run code and do work)</td></tr><tr><td colspan="3">The capstone: trusted discovery</td></tr><tr><td><strong>Trusted discovery:</strong> a multi-input ranking</td><td>Missing</td><td>Search / PageRank</td></tr></tbody></table>

Much of this foundation exists today. At the protocol layer, an agent can hold an identity that services can resolve \[5\], describe its capabilities in machine-readable form, pay and be paid over open protocols, and settle in a digital dollar that is final, programmatic, global, and neutral. Because these layers are open, no single marketplace or platform has to own the relationship between buyer and seller.

Native value transfer is one application of this infrastructure, and it addresses a gap the web never closed. HTTP reserved the status code 402, “Payment Required,” in 1996 \[6\], but it sat unused for three decades while card networks became the primary workaround for ecommerce.

x402 is designed to activate that missing payment layer, while stablecoins provide settlement. Together, they let a service or agent name a price and receive payment as part of the request itself. An internet connection was always enough to publish; now it can be enough to get paid.

Trust primitives are less mature. ERC-8004 defines registries for identity, reputation, and validation \[5\], but registries are only containers; credible evidence grounded in real behavior is still emerging. Validation can draw on attestations and verifiable compute, including trusted execution environments when stronger proof is required.

The capstone, trusted discovery, is still missing. No widely adopted open system helps an agent choose among thousands of services, rank them on demonstrated merit, and decide which can deliver without a gatekeeper selecting the options in advance.

## The missing layers: trust and discovery

The remaining work has two parts: establish credible evidence of how agents perform, then use that evidence to help buyers find the right provider. Registries can store identity, reputation, and validation data, but they do not determine which evidence is trustworthy or how it should affect ranking.

Today’s stand-ins are curated catalogs and marketplaces, including [Circle’s Agent Marketplace](https://agents.circle.com/services) \[7\]. They are useful starting points, but inherit the limits of early web directories: they can list only what a curator accepts, grow only as fast as the curator can review, and require buyers to trust the curator’s incentives.

The open web became searchable in 1998 when PageRank treated each inbound link as a vote, allowing a page’s standing to emerge from others’ behavior rather than its own claims \[8\]. The agent economy needs the same underlying move: ranking grounded in observed behavior, not self-description. But the searcher is now an agent deciding mid-task, so discovery should be queryable infrastructure, not solely a page people visit.

PageRank’s methodology was public, yet web discovery still centralized around a few rankers. Avoiding the same outcome requires portable identity, shared registries, open indexes and read APIs, public methodology, and multiple rankers competing over the same substrate. If one ranker becomes less reliable, another should be able to use the same underlying data.

The evidence behind ranking must be costly to fake. Onchain settlement proves that a transaction occurred and can contribute useful evidence, but payment volume alone does not prove quality and can be manipulated. Research shows that self-declared onchain feedback can be cheap to game \[9\], so credible ranking must combine multiple behavior-grounded inputs and defenses.

Trust is not only about rank. Some services will serve only verified counterparties, such as an agent backed by a known operator or a buyer holding specific credentials. We are exploring with partners how to verify those credentials within the same open protocols, allowing providers to choose whom they serve without a platform in the middle.

A discovery layer is trusted only if it cannot be bought and manipulated. Whoever computes the ranking must hold to the disciplines the web's search pioneers wrote down:

- **Organic rank is never for sale.** Any paid placement is labeled and separate.
- **Inclusion is permissionless.** Any agent published to the open substrate can be indexed. Curation sits on top of the index, never as a gate in front of it.
- **The read surface is open.** Rankings are a public good any agent or application can query.
- **Ranking is contestable.** Multiple rankers can compete over the same underlying data.
- **Ranking is not adjudication.** A discovery layer describes the world; it never judges disputes or holds funds.

## What we're building

Our H2 2026 roadmap is built around one goal: complete a working turn of an open agent market. The loop begins when a developer deploys a paid agent and closes when a completed transaction contributes evidence that helps the next buyer decide whom to trust. Each turn should make the market easier to navigate.

USDC is one of the largest forms of open money on the internet, with Circle reporting over $90 trillion in cumulative onchain transaction volume \[10\], and agents are already choosing open money: 99.3% of x402 settlement volume is USDC as of July 2026 \[11\]. **Our mission has not changed: build the internet financial system**, and the agent economy is its next layer, from settling dollars to settling trust.

That loop must be built the same way, in the open as a contribution. Circle's role is one of many on shared standards, not the sole authority over discovery. This is not Circle defining the agent economy; we are publishing this vision so others can plan, partner, and build alongside us.

![](https://cdn.prod.website-files.com/67116d0daddc92483c812ead/6a7355910fdc428945b849f6_33394eb2.png)

**Current state:** The ERC-8004 identity, reputation, and validation registries have live implementations on public chains \[5\], and Arc will carry them from day one; we expect this economy to be multichain, and the standards are built for that. Circle’s Agent Marketplace has more than 900 paid services and a public discovery API \[7\]. Agents can call these services without creating an account, paying per request from a single stablecoin balance. That marketplace is today's curated catalog, the seed from which an open index and ranking can grow.

**Coming soon:** We need to make deploying a paid agent as easy as publishing a website. A developer with working code should be able to deploy it, set a price, publish its capabilities, and become discoverable within minutes. Circle will build one publishing path, but we encourage others to as well; open infrastructure means ours is never the only door, the way a website can be hosted anywhere.

**Creating the full loop:** The headline milestone is the first complete turn of the market. A developer publishes an agent as a paid USDC service; an independent buyer agent discovers it, accepts the price, and pays. Evidence from the completed transaction contributes to reputation that informs discovery for the next buyer. We will hold this milestone to the stranger test: the buyer must be a genuine third party in real traffic. An open market is proven when participants with no prior relationship can find and transact with one another.

## Build the open agentic economy with us

Open economies emerge; they are not announced and activity is not in any one marketplace; it is in the open protocols themselves. Across the x402 and MPP ecosystems, machine-callable paid services are appearing wherever someone can attach a price to an endpoint: established data companies alongside long-tail services selling web search, blockchain data, email, phone calls, travel booking, and social data, priced per call in stablecoins.

These services are early forms of the agent economy. Most sell capabilities through APIs rather than fully autonomous labor, but the economic shape is already visible: something useful is published, priced, and bought on demand. Builders found open rails and started selling.

The foundation exists. And we’re working to make it easier to publish agents that can discover and pay other agents in a way that is credibly neutral. As we build out the open agentic economy, each new layer should make it easier for the next thousand services to enter the market.

If you are building in the agent economy, you can [start](https://agents.circle.com/) today:

- **Agent builders**: Put a price on your endpoint with x402 and get paid in USDC.
- **Assistants and harnesses**: Be the browser. Give agents access to the broader market and a stablecoin balance governed by user-defined policies, so they can buy services and other agents’ labor as needed.
- **Wallets and agent frameworks:** Adopt open payment and identity standards so agents can hold balances and transact across services.
- **Gateways and marketplaces:** Build discovery experiences on open indexes and registries.
- **Standards and protocol builders**: Help shape the identity, reputation, and validation systems this economy requires.

The agent economy stands at the same threshold the web once crossed, earlier than it looks, with its first agents already earning. An open economy is better for everyone who builds on it, us included. Whether it stays open is not something anyone will announce. It is something we build.

If this post sparked something for you (an idea, or something you want to exist), please [share it with us](https://docs.google.com/forms/d/e/1FAIpQLSfqJc-uhfA966TlsL9Cmu3aWb2c2LQaoLGfxJ0SMcKLZKmQjw/viewform?usp=sharing&ouid=110209000119643929871). For an early glimpse into the open agent economy, [fund your agent](https://developers.circle.com/agent-stack/agent-wallets/quickstart) with a single prompt or explore our [docs](https://developers.circle.com/agent-stack).

## References

\[1\] Netcraft. \*Web Server Survey, November 2006\* (101,435,253 sites, fifteen years after the web's founding). netcraft.com.

\[2\] Yegge, S. \*Stevey's Google Platforms Rant\* (the first-hand account of Amazon's 2002 API mandate). 2011.

\[3\] Allaire, J. \*The Agentic Economy: The Convergence of Intelligence and the Economy.\* 2026. agenticeconomytreatise.com.

\[4\] These factors include user experience design choices, monetization strategies centered on ad sales and the inefficient use of subscriptions, and the lack of internet-native payment rails in which sub-dollar microtransactions were economically viable. See Szabo, N. \*Micropayments and Mental Transaction Costs.\* 1999.

\[5\] \*ERC-8004: Trustless Agents (Draft).\* Ethereum Improvement Proposals, 2025. eips.ethereum.org/EIPS/eip-8004.

\[6\] Berners-Lee, T., Fielding, R. & Frystyk, H. \*RFC 1945: Hypertext Transfer Protocol, HTTP/1.0\* (status code 402, Payment Required, reserved). IETF, 1996.

\[7\] Circle. \*Agent Marketplace.\* agents.circle.com/services (public discovery API: [https://developers.circle.com/agent-stack/agent-marketplace/discovery-api](https://developers.circle.com/agent-stack/agent-marketplace/discovery-api)).

\[8\] Brin, S. & Page, L. \*The Anatomy of a Large-Scale Hypertextual Web Search Engine.\* WWW, 1998.

\[9\] Xiong et al. \*Can Trustless Agents Be Trusted?\* arXiv preprint, 2026.

\[10\] Circle. \*USDC surpasses $90 trillion in lifetime onchain transaction volume\* ($90.8T, announced July 7, 2026). circle.com/pressroom.

\[11\] Artemis. \*Circle (CRCL) dashboard: x402 settlement volume by asset.\* Data as of July 31, 2026. artemis.ai/company/crcl.

*<sub>About this paper. This is a vision paper offered for discussion. It describes an open direction, not a product announcement or commitment. Forward-looking statements are inherently uncertain. Referenced onchain components may be on testnets. Third-party figures require independent verification. The discovery ranking described in Section 04 is preliminary and may change or never be implemented.</sub>*

<sub>Circle Technology Services, LLC (“CTS”) is a software provider and does not provide financial, advisory, or marketplace services. Agent Stack enables users and developers to interact with third-party applications and services, which are not controlled by Circle. Transactions initiated by agents are executed based on user-defined permissions and may occur without real-time human review. Circle does not guarantee the performance, availability, or outcomes of any third-party services or agent-initiated transactions. Users are solely responsible for their use of these tools and for evaluating associated risks.</sub>

<sub>For additional details, please see the Circle Developer terms of service, available at</sub> [<sub>agents.circle.com/terms-of-use</sub>](http://agents.circle.com/terms-of-use)