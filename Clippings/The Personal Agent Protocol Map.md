---
title: The Personal Agent Protocol Map
source: https://x.com/sytaylor/article/2113710822731989172
author:
  - "[[Simon Taylor (@sytaylor)]]"
published: 2026-10-23
created: 2026-10-26
description: Banks are already blocking agents that log in using credentials their customers handed over.  JPMorgan says it is blocking shared-credential...
tags:
  - clippings
  - ArtificialIntelligence
  - agentic
  - BUGuestLecture
---
![Image](https://pbs.twimg.com/media/HVU32JsXsAEuNP1?format=jpg&name=large)

Banks are already blocking agents that log in using credentials their customers handed over. [JPMorgan says](https://www.jpmorganchase.com/content/dam/jpmc/jpmorgan-chase-and-co/investor-relations/documents/quarterly-earnings/2026/3rd-quarter/3q26-earnings-transcript.pdf) it is blocking shared-credential services that move money, while working on a way to do this correctly.

We’re entering a schism. Some businesses are building a front door for agents while others are blocking the ways they get in. [Walmart just announced](https://corporate.walmart.com/news/2026/10/Making-It-Possible-for-Agents-to-Shop-on-Behalf-of-Customers) an [MCP](https://modelcontextprotocol.io/docs/learn/server-concepts)\-based Agent Gateway, starting with a developer beta, and Agent Profiles due next month with spending limits.

On [Meta’s earnings call](https://s21.q4cdn.com/399680738/files/content_files/META-Q3-2026-Earnings-Call-Transcript.pdf), Zuckerberg said shopping was Muse’s most common use case, with an average purchase size over $200. This is absolutely a finance problem, and the prize for solving it is enormous.

[David Sacks argues](https://x.com/DavidSacks/status/2113317306398879787) that banks should support access customers can grant, limit, and revoke. I agree with the direction. The harder question is how.

## Your agent looks like a giant fraud and liability problem to your bank.

Handing over my login doesn’t tell the bank which agent is using it, what I asked it to do, or whether the action matches my instruction. A customer can approve access without approving everything an agent might attempt.

I feel a map coming on.

![Image](https://pbs.twimg.com/media/HVU4OOXXoAA3L4h?format=jpg&name=large)

There are four layers: **Entry, Identity, Tools, and Intent.**

## 1\. Entry: give the agent a supported way in.

![Image](https://pbs.twimg.com/media/HVU7uJ2WQAEo8lS?format=jpg&name=large)

[Personal Agent Protocol, or PAP (now called Poppy)](https://personalagentprotocol.org/docs/spec)**,** gives a business a way to publish how agents can interact with it: where to sign in, which interfaces it offers, and what access it will grant.

My agent discovers that information and uses a supported sign-in flow. I approve the access; the business decides what it allows. That access can be limited and revoked. Bank of America and Wells Fargo are [design partners](https://sierra.ai/blog/poppy), which means they’re helping shape it, rather than promising my agent can use their banking apps tomorrow.

[Visa’s Trusted Agent Protocol (TAP)](https://developer.visa.com/docs/trusted-agent-protocol/trusted-agent-protocol-specifications) lets a shopping agent introduce itself with signed information: “I’m a shopping agent approved through Visa’s program, I’m shopping for this customer, and here’s how they’ll pay.” The merchant can check that information before letting it continue. It still needs to check whether I approved the purchase.

[Web Bot Auth](https://developers.cloudflare.com/bots/reference/bot-verification/web-bot-auth/) lets the website check who operates the agent. Calling yourself “Muse” isn’t enough; the website checks the request’s signature against the operator’s published key. TAP uses this approach for shopping agents.

These are complementary: “hey, I’m a bot, I’m a nice bot, and I’d like to do a thing.”

## 2\. Identity: bring the proof the service needs.

![Image](https://pbs.twimg.com/media/HVU76zsXMAAYXPR?format=jpg&name=large)

[Proof’s x401](https://x401.proof.com/spec/0.1.0/) lets the service ask for evidence about the person behind the agent. The bank says what it needs, the agent presents a credential, and the bank checks it. “My agent says I’m Simon” is a claim; a signed credential from a trusted issuer gives it evidence.

[OpenID for Verifiable Presentations (OpenID4VP)](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) is how the wallet presents that credential. x401 asks for the evidence; OpenID4VP helps deliver it.

[FIDO standards underpin passkeys.](https://fidoalliance.org/passkeys/) I unlock mine with my face, fingerprint, or device PIN, and my device sends a signature the service can check. My biometric stays on my device. That authenticates me without handing the agent my password; it doesn’t establish my legal identity by itself.

An existing bank customer may only need to authenticate. A new customer or restricted service might need verified identity or eligibility too.

## 3\. Tools: give the agent a precise operation.

![Image](https://pbs.twimg.com/media/HVU8JqDWQAAVhsO?format=jpg&name=large)

**MCP** lets the bank describe what an agent can do and how to ask. “To freeze a card, call this tool and give me the card ID.” The agent sends the ID; the bank returns a result.

[WebMCP](https://developer.chrome.com/docs/ai/webmcp) offers structured tools through the browser, so the agent can use a freeze-card tool instead of guessing which button to click. **An API** supplies a direct software interface. MCP can describe an operation while an API does the work underneath.

[Agent2Agent (A2A)](https://a2a-protocol.org/latest/specification/) lets my agent hand a task to the business’s agent and follow its progress. It doesn’t establish that I authorized the task.

**The browser** is how agents will reach many services for the foreseeable future. They’ll click through pages because that’s what’s available, and an unexpected pop-up can derail the job.

The business still controls access. Discovering a “transfer money” tool doesn’t give my agent permission to empty my account.

## 4\. Intent: match the action to what I approved.

![Image](https://pbs.twimg.com/media/HVU8aiHW8AASsWQ?format=jpg&name=large)

That sinking feeling when you can’t find your phone is bad enough without notifying every bank one by one. I want my agent to freeze my cards and lock the device. “Freeze my card” isn’t permission to transfer my balance or change my account details.

A purchase adds terms. If I ask my agent to book a hotel, suitable means free weights above 25 lbs, a mini fridge, and free cancellation because travel changes at the last minute. I might approve one room, specific dates, and $200 a night including fees.

Approval is one step. The receiving service then needs to match the actual booking against those terms. A $190 room with $60 in extra fees is over my limit. Signing a payment won’t prove the hotel has the gym I wanted, either.

[x401 can also request a signed action.](https://www.proof.com/blog/proof-launches-x401-giving-ai-agents-a-way-to-prove-who-they-represent---and-what-theyre-authorized-to-do) I review the terms on my device, approve them, and the service checks the signed response before proceeding. This can cover account changes and documents as well as payments, where the provider supports it.

[Mastercard’s Verifiable Intent](https://github.com/agent-intent/verifiable-intent) gives the business signed evidence connecting my permission with the agent’s purchase. It checks that the signatures and transaction fit the limits, rather than taking the agent’s word for it.

[Google’s AP2](https://ap2-protocol.org/ap2/specification/) links checkout and payment permissions, called mandates. The merchant checks what the agent may buy; the payment provider checks whether it may use my payment method for that checkout.

[Visa Intelligent Commerce](https://developer.visa.com/use-cases/visa-intelligent-commerce-for-agents) supplies payment instructions and controls. I authenticate an instruction; the agent requests payment credentials, and Visa checks the request and the merchant’s payment against that instruction. TAP helps recognize the agent; these controls govern payment.

Europe’s [Strong Customer Authentication](https://eba.europa.eu/single-rule-book-qa/qna/view/publicId/2020_5133) offers a useful step-up comparison: for relevant remote payments, the approval is tied to the amount and payee. It doesn’t give us a universal permission for the whole lost-phone job.

## The business still has to say yes.

![Image](https://pbs.twimg.com/media/HVU5xD3XEAAyoKA?format=jpg&name=large)

Consumers ultimately need to be in control of what agents do.

Yes, each bank still has to grant access to its service. My instruction can’t give permission on the bank’s behalf. The protocols for all of this exist, they just need to be stitched together, and the agent needs to report those results so I know what’s protected and what still needs attention.

Your risk appetite may vary.

I’m happy letting an agent get on with purchases below a couple hundred dollars. But beyond that, I want it to check with me and leave an audit trail of a known agent that did, things.

If we have

1. Consumer permissions (via intents)
2. Linked to identities of agent makers, and humans.
3. That can request permissions from businesses and banks
4. And then there's an audit trail for the whole thing

We'll have not more unpermissioned agents.

Of course, the problem is "permissioned" often means "paid." [JPMorgan and Plaid’s data-access agreement](https://media.chase.com/news/jpmorganchase-plaid-extend-data-access-agreement) already includes pricing, and that agreement took almost a decade to get to the bottom of.

You think the banks won't hold out for more from Meta?

We’ve spent years making it easier for customers to click the right buttons. Now their agent is arriving with an instruction.

We need to make it possible to prove its authority and get the job done.

And that might mean the banks get paid along the way.

If you enjoyed this, I write at [brainfood.xyz](https://brainfood.xyz/) - check us out :)