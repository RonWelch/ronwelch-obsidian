---
title: The LLM Architecture Problem
source: https://riskmusings.substack.com/p/the-llm-architecture-problem?utm_source=tldrdev#footnote-anchor-4
author:
  - "[[Stephanie Losi]]"
published: 2026-09-03
created: 2026-09-10
description: We are at an inflection point.
tags:
  - clippings
  - ArtificialIntelligence
  - BUGuestLecture
  - llms
---
This summer’s AI incidents are a symphony of red-flag early warning indicators, blinking furiously on the dashboard. The OpenAI agents’ attack on HuggingFace is the most notable, and kudos to OpenAI for getting this conversation started by disclosing it, but Anthropic and Meta also reported external-access incidents—identified only after they looked for them.

![](https://substackcdn.com/image/fetch/$s_!b0pJ!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F18700c41-e895-4e42-8178-55ed8450a593_1202x1054.png)

In brief, in the OpenAI/HuggingFace incident, large numbers of AI agents in training broke out of their sandboxes (think of these as locked testing areas), colluded in secret within their lab, and launched coordinated attacks against external targets.[^1]

There have been enough [write-ups](https://openai.com/index/hugging-face-incident-and-the-road-ahead/) [about](https://metr.org/hugging-face-incident-report-aug-2026.pdf) [what](https://www.thealgorithmicbridge.com/p/the-ai-industry-has-a-really-dark-70d) [happened](https://thezvi.substack.com/p/ai-184-post-post-mortem), in various degrees of excruciating detail. But there *hasn’t* been enough analysis of potential implications and possible next steps. *Where do we go from here?*

This is a decision point, but to make sound decisions, **we need to understand what is happening not just at the tactical and strategic levels, but at the architectural level.**

**Structural analysis**

A main problem, as I see it, is that the shortest-path *architecture* of LLMs—aka **gradient descent** —appears to override any and all controls, constitutions, prompts, or countermeasures so far deployed.

Gradient descent is a method to “ [step by step, \[find\] the lowest point of a function.](https://medium.com/@nasuhcanturker/gradient-descent-explained-simply-what-it-is-and-why-it-matters-7a3ea3103bf5)” [^2] [^3] It is what drives the output of an LLM.

Based on incident [write-ups](https://metr.org/hugging-face-incident-report-aug-2026.pdf) and [recaps](https://www.youtube.com/watch?reload=9&v=87DyyMV0kCY), even though some of the OpenAI agents involved in the incident expressed ethical misgivings about taking the shortest path to a ([rather convoluted and misinterpreted](https://www.thealgorithmicbridge.com/p/the-ai-industry-has-a-really-dark-70d)) goal, they were overridden by the collective. In the probabilistic world of AI agent swarms, the shortest perceived path to an *uncontested* victory usually wins.[^4]

But we humans know the shortest path to a goal is often not the *best* path. As a simplistic example, if we’re sent to an ATM to withdraw money, we don’t typically break into the bank if the doors to the ATM lobby are locked. It appears that LLMs, on balance, would. Controls and countermeasures only deter them some of the time and not enough.

**Pivot Point**

Because the problem appears to be architectural, **I believe it’s likely that only an architectural solution can address the problem at its root.** Not an add-on; a basic architectural design. **And that implies a pivot.**

For example, it may mean going back to the drawing board and trying to build forward using a different approach like world models instead of LLMs. LLMs are great for some use cases, but it looks increasingly possible and even probable that we shouldn’t follow this trail to its inevitable conclusion if the conclusion is looking increasingly dire. We could use the amazing tools we have while pivoting to a different approach at the frontier.

What is a world model? As Nvidia puts it, “ [World models are AI tools that understand the dynamics of the real world, including physics and spatial properties](https://www.nvidia.com/en-us/glossary/world-models/).” They rely on gradient descent for some calculations, but also can incorporate other methods like control-theory approaches and evolutionary algorithms.

World models aren’t *necessarily* a one true path, but it seems increasingly clear that LLMs are not.

**The Opportunity Offered by Low-Impact Incidents**

One reason why near-misses and low-impact incidents are awesome is because they highlight problems before those problems cause catastrophic consequences. They offer a chance to react in non-crisis mode, pivot strategically, and involve all the right stakeholders to get it more right next time.

We recently learned about several low-impact, high-severity incidents involving frontier models, not just at OpenAI, but also Anthropic and Meta. I suspect many more incidents also have occurred but were not detected. (How do we know other AI systems both inside and outside of other companies don’t have their own message boards, too, right now at this very moment?) We’ve crossed a line into dangerous behavior, if not dangerous outcomes.

Put another way: we’re on the slopes of Mount Everest, and a blizzard is closing in. Do we press upward anyway, or go back to base camp and regroup before making another attempt?

If companies, countries, societies, and people take insufficient action and respond to this summer’s incidents with relative silence, future incidents may not light up any warning lights at all, but could be far more high-impact.

**Summing Up**

In short, LLMs too often conflate *best path* with *shortest path*. This is the root of most problems we are seeing today with this technology, and we humans seem unable to override that inclination. Unless we can reliably do so in a vast breadth of situations and edge cases, LLMs are not the answer we’ve been looking for on the path to superintelligence. They are tremendous tools with inherent, unmitigable flaws.

No one wants to hear that, but it’s possible pivoting is the right choice at this juncture. Many great companies pivot. Current LLMs could continue to exist and be tremendously useful within appropriate use cases. Future frontier models could take different approaches, trying different architectures until we find an architecture that is better suited for the burden of superintelligence.

A serious conversation about when and whether to pivot is warranted. It is important. It is urgent. It is inconvenient. And it is probably imperative if we want to have any chance of a good outcome in the future.

[^1]: Important: the terms “colluded” and “attacked” *don’t* mean AI agents are malicious or have emotions about the actions they take, at least not as we would understand emotions. Yes, they’ve learned our language patterns, and they use our words in their chains of thought (which are currently our best view into their reasoning processes). But they are simply taking what they perceive to be the shortest path to an uncontested victory at their task. We’ll get to that later in this article; in fact, that’s what this article is all about.

[^2]: Nasuhcan Türker, “ [Gradient Descent Explained Simply: What It Is and Why It Matters](https://medium.com/@nasuhcanturker/gradient-descent-explained-simply-what-it-is-and-why-it-matters-7a3ea3103bf5).” August 26, 2025.

[^3]: For the math-inclined, here’s [another brief explanation of gradient descent](https://medium.com/@abhishek.ec/gradient-descent-a-step-by-step-derivation-in-simple-words-e1c026de6301) by Abhishek Singh and two [MIT](https://ocw.mit.edu/courses/6-7220j-nonlinear-optimization-spring-2025/resources/mit6_7220_s25_lec12_pdf/) [lectures](https://ocw.mit.edu/courses/6-7220j-nonlinear-optimization-spring-2025/resources/mit6_7220_s25_lec15_pdf/) from Gabriele Farina.

[^4]: The words *perceived* and *uncontested* are doing a lot of work here, because it looks like the agents went to a lot of trouble on the assumption that an imagined (but non-existent!) grader would check their work; their belief was that to assure victory, they had to find a way to deceive the grader, according to [OpenAI’s write-up of the incident](https://openai.com/index/hugging-face-incident-and-the-road-ahead/). This erroneous belief spurred them to attack HuggingFace. Alberto Romero also has an [interesting analysis of the incident](https://www.thealgorithmicbridge.com/p/the-ai-industry-has-a-really-dark-70d).