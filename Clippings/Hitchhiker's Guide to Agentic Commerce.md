---
title: Hitchhiker's Guide to Agentic Commerce
source: https://x.com/Must_be_Ash/article/2054343469834043834
author:
  - "[[Ash (@Must_be_Ash)]]"
published: 2026-05-12
created: 2026-05-14
description: In The Hitchhiker's Guide to the Galaxy, the answer to life turns out to be 42. The number is famously useless on its own because nobody eve...
tags:
  - clippings
  - ArtificialIntelligence
  - BUGuestLecture
---
![Image](https://pbs.twimg.com/media/HIF1H_lbMAAT8Do?format=jpg&name=large)

In The Hitchhiker's Guide to the Galaxy, the answer to life turns out to be 42. The number is famously useless on its own because nobody ever figures out what the question actually was. The question they posed was too general and vague to make sense of the answer they got. Agentic commerce is in the same place. You see product solutions coming out every day but it's not clear what need they're answering to. Everyone has heard the term. No two people use it to mean the same thing. But everyone agrees on where it's heading, and the race to own the stack the agent can't bypass is already on.

**The argument is over**

About a year ago, the question on the timeline was "why would we give agents a wallet?" People treated it as a novelty. It was mostly experimentation and speculation. If you brought it up in a serious conversation, people would have said it was just a crypto thing and Coinbase's AgentKit was viewed as a marketing campaign more than a steppingstone.

That's not the case anymore.

Today, every player in payments, infra, and AI is aligned on this. The question is no longer whether agents will spend money, but how they'll spend, on what rails, and who owns which part of the stack.

Everyone has gone from "monitoring the situation" to actively fighting for a piece of the pie. If you are a builder, investor, or a merchant it's worth paying attention so you can position yourself accordingly.

The whole space, from "checkout in ChatGPT" to "your agent runs your financial affairs", is being talked about as one thing. The two ends look familiar. Everything in the middle is where the new economy actually gets built.

**Stage 1: the checkout you already know with a twist**

The first stage is the most obvious one. You stop visiting websites and searching. You ask your AI for a pair of running shoes, it shows you some options, you zero in on one, and you check out the way you always have. Card on file, address on file, done. The only difference is that the surface has moved from a Shopify store or a Google search to a chat window. And now it's harder to know how to rank higher and be the recommended product, since there's no clear SEO playbook for AI-driven recommendations yet.

This is the stage Stripe and OpenAI standardized with the Agentic Commerce Protocol (ACP). It is also the stage Google, Shopify, Etsy, Wayfair, Target, and Walmart are coordinating around with the Universal Commerce Protocol (UCP). The flow is exactly what you would guess: tap Buy, confirm shipping and card, done, never leave the chat.

Stage 1 barely disturbs anything because the underlying stack doesn't move. The agent sits on top of it, hands off to the same merchants and rails, and is really just a smarter search bar, which is also why you see the same major player in payments you already know of populating this category. Stage 1 is the part you see, the early integration of payments with AI that mostly relies on companies doing BD. It's the stage most people think about when they hear the term agentic commerce.

There's also a bigger story sitting underneath all of this, the one where the ad-supported internet quietly falls apart now that agents serve you all you need without you visiting the source, and something has to replace the economic contract that paid for the last 25 years of the web. For the sake of my own sanity, I'm not going to try to fit that in here, but you can listen to [@programmer](https://x.com/@programmer) break it all down [here](https://x.com/TokenizedPod/status/1999147167219585197?s=20).

**Stage 2: the agent has a card, yay!**

Stage 1 is really just a smarter search bar with a checkout button that you still press. Stage 2 is the natural evolution: the agent isn't just surfacing results anymore. It does the research, narrows the options, and comes back with a recommendation. But it still needs you to say yes before anything is charged.

This is where virtual cards come in. The interesting part is that each of the major networks is solving the same problem, how do you let an agent carry a card without losing your mind, from a totally different angle.

Visa starts with identity. Before your agent can carry a card, it gets vetted, credentialed, and added to a list. Every checkout it makes is cryptographically signed, and the merchant checks the signature. The bet is that a safe agent economy looks a lot like a credentialed one: you know who the agent is before you let it near your card.

Mastercard starts from the other end. Instead of vetting the agent, you pre-agree on what it's allowed to do: spending limits, merchant categories, how long the session lasts. The agent runs inside that fence and the rails never know the difference (you don't need to know who the agent is if you've already agreed on what it can do).

Stripe skips the philosophy and stays close to the human. Link for agents issues a one-time token per purchase and pings you before anything settles. No persistent credential, no trust extended beyond the moment. The bet is that people aren't ready to hand over the keys yet.

Amex is one of the latest ones and I haven't personally looked into their kit (ACE) but the promis is if a registered agent makes a mistake on your behalf, Amex eats the cost.

On top of all of that, a new category of products is forming for developers and consumers who want to give agents a card like [@crossmint](https://x.com/@crossmint), [@moonpay](https://x.com/@moonpay) and [@paysponge](https://x.com/@paysponge). These platforms make it easier to give your agent access to the product being made (Visa IC, Mastercard Agent Pay, and Stripe's Link).

There's a less obvious thing happening here too. Once the agent has its own card, shopping goes headless. It no longer has to live inside ChatGPT or Claude. Anyone can build a custom shopping experience, a domain-specific assistant, or an embedded checkout, and let an agent transact through it without asking for permission. Stage 1 lives and dies inside whatever chat surface the user is sitting in. Stage 2 cracks that open.

The defining feature of Stage 2, today, is the approval gate. The agent has the card. The agent did the work. But it does not pull the trigger without you. That is both the safety net and the limitation. Most of where the industry sits right now is here. The agent is also unable to earn, it can only spend. The spend permissions are also not flexible and programmable to the extent you want them to be.

The approval gate is a 'now' thing, not a forever thing. Every player in this space is investing in trust signals and programmable spend controls that give the agent more rope over time. Today it asks for permission on every purchase. In a year, on most. In two years, only on edge cases.

The next two stages are where that normalization is already quietly happening, in the parts of the stack most people haven't looked at yet. Agents spending, earning, and paying for the work they need to get done. Which raises a question worth sitting with: before the agent showed you those running shoes, how did it know to show them at all? The agent had to search the web, read reviews and blogs, check for the latest fashion maybe, check store's inventory to find your size, and see who delivers the product to your address...

**Stage 3: the plumbing**

Stages 1 and 2 are still fundamentally about the same thing your grandma does: a human picks a product, a human approves the purchase, money moves. The only thing that changed is who pressed the button. Stage 3 is where that breaks, and it's where the agent actually starts paying for things and even earn in some cases.

Assuming the shoe the agent surfaced isn't sponsored or hardcoded into the response, how did it know which shoe to show you? Where did the inventory come from? How did it know the price, find a picture, or check your size?

The agent had to fetch all of that from somewhere. It had to call APIs, search the web, crawl pages, hit data sources, maybe generate an image, parse reviews... It had to do work before you ever saw the options it came back with. That work costs money and relies on different services

This is Stage 3: the agent paying for the things it needs to do its job. LLM inference, image generation, search results, product data, reviews, speech-to-text, text-to-speech. You're not signing up to each service, managing keys, pre-loading credits, and maintaining a different balance for every provider. The agent pays for what it needs, when it needs it. This is where the human stops being in the loop.

This is also why we suddenly stopped talking about shoes and started talking about APIs. The shoe was the thing you saw. The dozens of small paid calls that led to the shoe, those are the actual economy underneath, and they need a payment system that can move fractions of a cent at machine speed without a person clicking anything.

That payment system is built on micropayments: tiny transactions, sometimes fractions of a cent, settled in real time as the agent works. The closest thing to a standard is x402, an open payment protocol stewarded by the x402 Foundation under the Linux Foundation. It piggybacks on the HTTP 402 status code, the one browsers have ignored for thirty years, and finally gives it a job. When an agent hits a paid endpoint, the server replies "402, here's the price". The agent pays. The response comes back. No accounts, no API keys, no monthly subscriptions. Pay-per-call. AWS AgentCore now ships with x402 and a wallet baked directly into Bedrock!

This is the moment the agent stops asking permission. The interesting part starts when one agent's workload gets handed off to another agent.

**Stage 4: when agents hire other agents**

Say you ask your agent for something complicated. Something like "make me a two minute video explaining the history of the Federal Reserve, in the style of Adam Curtis, with motion graphics."

A general-purpose agent can technically do this with trial and error. It can call a video API, generate 20 seconds of AI slop, slap on a voiceover, and give you something poorly made after wasting a bunch of time and money. But a purpose-built agent would reach for the right tools with proper guidance and workflow. It would pay [@ExaAILabs](https://x.com/@ExaAILabs) to do deep research, write a script, then use HyperFrames or Remotion to make motion graphics and pay [@ElevenLabs](https://x.com/@ElevenLabs) for a voiceover. Or it would pay [@HeyGen](https://x.com/@HeyGen) for an avatar to explain things. Or it would pay for image generation, create storyboards, then call the video model to get frame-by-frame control over the output.

Any agent can eventually find the right endpoints, figure out how to call them, and combine them. But that exploration costs you real money and real time, every single run.

This is the gap that agent-to-agent commerce fills.

A specialist agent, built by people who have done the work of bundling those services and tuning the workflow, can offer your agent a faster, cheaper, better-quality experience for the same task. Your agent does not call eight raw endpoints. It calls one specialist, pays, and gets a finished product. The specialist becomes a parallel to the human specialists we already pay for: the comic book artist, the financial advisor, the travel planner, the video producer...

Two things are happening here at once, and they're worth pulling apart. On one side, your agent becomes a buyer of specialist work. On the other side, the specialist itself is a new kind of business: software that only exists when it's paid, charges per call, and may never have a website, an app, or a single human customer. You can be on either side of that transaction. I can see Claude Code plugins becoming paid mini apps that I can utilize on the fly without ever having any APIs or preconfigure tools. It would be just my agent, communicating in plain english with another agent with access to specialized tools, skills or data sets and paying that agent for getting things done.

And this isn't just a consumer story. The biggest version of Stage 4 is probably going to play out in B2B, quietly, before most consumers notice. The person at your office whose job is to order A4 paper, get quotes from three packaging suppliers, and chase the delivery that's a day late is going to hand most of that to an agent. The vendors will do the same. Procurement, fulfillment, supplier swaps... all of it becomes a conversation between two agents that finishes before the human gets back from lunch. Anything that's boring or purely logistics will be taken over.

[@samrags\_](https://x.com/@samrags_) framed in a way that I think nails it: "an agent that can only buy from pre-approved merchants is an employee with a corporate card restricted to three vendors. An agent with open protocols is an entrepreneur with a bank account."

Stages 3 and 4 don't just give your agent economic reasoning and diversity. They also mean your agent could be making money. This is the stage where the long tail of micro-businesses lives. Most of them will not have an app or a website. They will have an endpoint and a price. Today these are shared and used by developers as janky 'Skills'. Tomorrow they're going to be agents and workers that bundle other paid endpoints, data, tools, and services for specific tasks. They'll only come to life when they're paid, and they use the payment to execute everything that's needed, including paying for their own inference and hosting.

Even though Stages 3 and 4 sound irrelevant to normal day-to-day commerce, this is where the adoption is coming from, and it's getting normalized for agents to autonomously make decisions and pay for things.

**Stage 5: the agent that knows your coffee order**

Stages 3 and 4 were about the agent doing its job. Now zoom out to what that makes possible for you. The plumbing exists. The trust is building. The approval gate is widening. Which means the assistant you already use every day can stop just answering questions and start actually handling things. And the easiest place to begin is the stuff you buy on autopilot anyway.

You might be very particular and picky about your coffee, olive oil, or even toilet paper, but I bet that now that you know what you like, you rarely switch it up. The overwhelming majority of what fills your cabinet is going to be the same stuff.

Stage 5 is when your daily LLM, the one you already talk to about everything, gets a wallet and starts handling the boring half of your consumption.

This is not a smart fridge story or a cron job. This is the assistant you already use, with access to your purchase history, your shipping address, and your preferences, executing the orders you would have placed anyway. You say "buy coffee" and it knows the brand, the grind, the bag size, the seller, the address, and the budget. It just does it.

The reason this stage is going to land is that it does not require a behavior change.

**Stage 6: the agent that knows what you want before you do**

The sixth stage is the one that gets weird in a good way.

Back in 2015, before we had anything close to a proper LLM, researchers from Cambridge and Stanford showed that with just 300 Facebook likes, an algorithm could predict your personality more accurately than your own spouse. Imagine what an LLM you talk to every day, and have given access to your consumption habits, knows about you.

Once your agent has been holding the wallet for a while, watching what you buy and when, it starts to learn your patterns. It knows you drink more coffee in winter and more iced tea in summer. It knows you reorder protein powder every five weeks like clockwork. It knows you look up flights two weeks before a long weekend, talk yourself out of it because prices have already moved, and end up not going. It knows you hesitate on concert tickets because the date feels too far away, and then pay twice as much for them closer to the show.

At some confidence threshold (and you control that threshold), it just starts acting ahead of you or gives you a nudge. It will help you save up, but also consume more.

"Ash usually asks me to reorder coffee on Monday. I am 94% sure he will do it again. The price is currently 8% below average. $8 is well below my $30 spend permission. Placing the order."

This is not just for products. It is for everything that has value and is purchasable. Articles you should read. Data you will need for the project you are mid-way through. A flight you will want to book before the price moves. The concert ticket you never thought you'd get without paying resellers three times the amount.

Above a certain level of certainty, your agent stops being reactive and starts making moves in anticipation of what's to come. "I know you've been listening to J. Cole on repeat and you could afford this ticket. I also checked your calendar and you're free, so I bought it. It's actually up 20% now. If you don't wanna go, I can resell it for you."

**Everyone wants the toll booth**

Step back and look at the chessboard. Every stage I just described has a payment moment in it. Every payment moment is being fought over right now by some combination of:

- The card networks (Visa, Mastercard, Amex), who own the existing rails and want to extend them.
- The settlement chains (Base, Solana, Tempo, Arc, [Stable.xyz](https://stable.xyz/)...), who want to be the layer where agent payments actually clear, especially the micropayments that card rails were never built to handle.
- The agent wallets (Coinbase, Merit, Crossmint, Sponge...), who want to be the wallet your agent reaches for.
- The protocols (ACP, UCP, x402, MPP), who want to be the standard everyone implements against.
- Discovery and reputation, how services are found and vetted
- The clouds (AWS AgentCore and Cloudflare), who want to provide the environment and become the hosting service of choice.
- The chat surfaces (ChatGPT, Claude, Gemini, Perplexity...), who want to be the front door to everything.

**So who wins?**

Nobody, yet. That's the actual point of writing this.

Notice something about the map. Stage 1 looks like commerce as you already know it: you buy a thing, you pay for it. Stage 6 also looks like commerce as you already know it: a thing arrives, you pay for it (you just didn't have to ask this time). The two ends of the framework are familiar to a normal consumer. Nothing dramatic on the surface.

The whole new economy is in the middle. Stage 2 is where the agent picks up its own card. Stage 3 is where the agent starts paying for its own work. Stage 4 is where agents start paying each other. Stage 5 is where your assistant starts running real money on your behalf without checking in. That middle, Stages 2 through 5, is where every dollar of new infrastructure, every protocol fight, every wallet, settlement chain, or specialist agent gets built. By the time we arrive at Stage 6, it just looks like normal life or we've got used to the wild stuff made on the way. Which is the whole point. The reason all the plumbing matters is that nobody is supposed to see it. Which is also why the opportunity is in the middle and not at the ends.

The stack is still being built, the protocols haven't crystallized, the default agent wallet hasn't been chosen, the dominant rail for agent settlement is up for grabs, and the specialist agents (from Stage 4) still look like SaaS companies today.

If you're building, this is unusually open territory. Pick a layer in the middle and go. If you're investing, have a look at the layers of the stack, there's more evergreen's here than pure AI plays.