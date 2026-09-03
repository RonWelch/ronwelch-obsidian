---
title: Compression is prediction
source: https://ngrok.com/blog/compression-is-prediction
author:
  - "[[Annie Sexton]]"
published: 2026-08-10
created: 2026-08-17
description: "Compression and LLMs are trying to solve the exact same problem: predicting what comes next. Learn the fundamentals of compression and how better prediction leads to better shrinkage."
tags:
  - clippings
  - ArtificialIntelligence
---
I was reading about compression recently when I stumbled upon something crazy: that compressors and LLMs are, at their core, trying to solve *the exact same problem*.

In this post, I’m going to walk us through the basics of compression to understand its deep relationship with language modeling. It’s probably going to blow your mind.

## How compression works

There are many ways of shrinking data. Take *minification*, for example: it works by stripping code down to the bare minimum that machines need to parse. Human-readable variables are reduced to single letters; whitespace and comments are removed.

Click “Minify” to see it in action:

Original source, 156 characters:

```
// Sum every number in the list
function sumNumbers(numbers) {
  let total = 0;
  for (const number of numbers) {
    total += number;
  }
  return total;
}
```

Minified to 62 characters — 60 percent smaller — by removing the comment, shortening the variable names to single letters, and stripping the whitespace, braces, and semicolons.

The resulting file is considerably smaller, and yet you’d almost never hear minification mentioned in the field of data compression. Why is that?

Minification is fairly straightforward: it just tosses out any syntax that’s not required by machines. But “true” compression relies on *redundancy* to condense data.

Consider the string: there’s a *lot* of redundancy here. We could encode this as a shorter string by noting the total run of each character in order:

Using standard 8-bit ASCII encoding, our original string requires **224 bits**, whereas our compressed string (“A9B4C2D1A3D9”) needs only **96**. Not bad!

The above technique is just one compression method (it’s called **run-length encoding**), but we can do much better. Actual compressors like gzip, Brotli, etc, rely on several methods to shrink data. Let’s take a look.

## The anatomy of a compressor

There are roughly three “organs” of modern compression tools: transforms, models, and entropy coders. I’m talking about these terms as if they were clear and distinct things, but the lines can get a little blurry, and they are *rarely* used in isolation.

Transforms

Model

Entropy Coder

`100101110`

Transforms are the preprocessing steps that make our data easier to compress. The method we saw earlier (run-length encoding) is an example of a transform, but it’s worth noting that transforms don’t *always* shrink the data. Sometimes they can be used to create *more* redundancy, and the more redundancy, the more we can compress later on. We aren’t going to focus on transforms in this article, but they’re still an important part of any compression tool.

Models describe the *shape* of our data based on the frequencies of each **symbol** (whatever unit we’re using to look for redundancies: letters, numbers, tokens, or even binary code). For now, you can think of a model as a table that maps each **symbol** to its **probability**, but as we’ll see later on, they can get *a lot* more sophisticated.

Here’s an example based on our earlier string:

| Symbol | Probability |
| --- | --- |
| A | 0.429 |
| D | 0.357 |
| B | 0.143 |
| C | 0.071 |

Entropy coders are almost always the final step in any compression algorithm and are what produce the final compressed artifact: a **raw bitstream**, which is just a bare sequence of bits with none of the structure a file format would wrap around it.

I want to focus on the last two steps, because this is important. Our data model hands the entropy coder a set of **probabilities** to encode your data as efficiently as possible. Probabilities go in, compressed bitstream comes out:

Model

| Symbol | Probability |
| --- | --- |
| A | 0.429 |
| D | 0.357 |
| B | 0.143 |
| C | 0.071 |

Entropy Coder

`100101110`

Now, let’s be honest: this is all still a bit hand-wavy. What does an entropy coder even DO with all these probabilities? How does that help it do the squishing?

## Squishing data with probabilities

Every entropy coder is a unique snowflake, and the way they use probabilities to compress your data differs *wildly*. To keep things simple, we’re going to focus on just one for now: **arithmetic coding**. I’m choosing it because it best illustrates how better *probabilities* make for better *compression*.

It’s also just really neat.

### Arithmetic coding

What if I told you that you could represent an entire dataset with a *single number*? Does this sound crazy? I thought so too, but that’s exactly what **arithmetic coding** promises.

Let’s say we want to compress the string. We can find the probabilities of each symbol (character) by dividing the total count by the total length of the string, which is 7:

| Symbol | Probability |
| --- | --- |
| A | 0.571 |
| B | 0.286 |
| C | 0.143 |

We can represent these probabilities on a range from 0-1.

With this setup, we’re ready to do the actual compressing.

For each symbol in our string, starting with “A”, we shrink our range to fit within *that symbol’s section.* Importantly, we’re still dividing that new range with the same probabilities, but they now have *new, smaller* ranges.

Click the arrows to encode each symbol and see how the range shrinks over time:

<svg aria-hidden="true" viewBox="49 -71.2 862 344.8" preserveAspectRatio="xMidYMid meet" style="font-family:monospace"><g style="transform-box:view-box;transform-origin:0 0"><g transform="translate(52 0) scale(1)"><defs><clipPath id="_R_4faahl5_-0"><rect x="0" y="0" width="856" height="72" rx="10"></rect></clipPath></defs><g clip-path="url(#_R_4faahl5_-0)"><rect x="0" y="0" width="489.1428571428571" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="489.1428571428571" y="0" width="244.57142857142856" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="733.7142857142857" y="0" width="122.28571428571428" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect></g><line x1="489.1428571428571" y1="0" x2="489.1428571428571" y2="72" stroke-width="2" stroke="#a3a3a3"></line><line x1="733.7142857142857" y1="0" x2="733.7142857142857" y2="72" stroke-width="2" stroke="#a3a3a3"></line><rect x="0" y="0" width="856" height="72" rx="10" fill="none" stroke-width="2" stroke="#a3a3a3"></rect><text x="244.57142857142856" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px" fill="#6b7280">A</text> <text x="611.4285714285713" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px" fill="#6b7280">B</text> <text x="794.8571428571428" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px" fill="#6b7280">C</text> <text x="0" y="78" font-size="15" text-anchor="start" dominant-baseline="hanging" fill="#737373">0</text> <text x="489.1428571428571" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" fill="#737373">0.571</text> <text x="733.7142857142857" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" fill="#737373">0.857</text> <text x="856" y="78" font-size="15" text-anchor="end" dominant-baseline="hanging" fill="#737373">1</text></g></g></svg>

Range: \[0.00000, 1.00000)

Once we run out of symbols, we end up with a *teeny weeny baby range*: **\[0.38730, 0.38855)**.

**The mixed brackets are intentional.** Square brackets \[ \] mean endpoint *included*, round brackets ( ) mean endpoint *excluded*. So \[0, 1) is “all numbers from 0 to 1, including 0 but excluding 1”.

The final number that will represent our *entire data* can be any number in this range, and ideally, it should be the number that requires the fewest bits possible. You can calculate this with a [bit of math](https://devblogs.microsoft.com/oldnewthing/20160222-00/?p=93061), but because I’m nice I’ll just give you the answer: **0.3876953125**. So let’s compare: Our original string,, in its raw 8-bit ASCII code requires 56 bits in total, whereas our final number requires only 10.

Our final number is *not* a floating point—it’s a **binary fraction**. Floating points are binary fractions too, but they come in fixed widths, so you’d pay 32 or 64 bits whether you need them or not. Ours only needs 10.

So, we have our magical number, but how do we use this to decode our original message? Buckle up, this is going to seem like a magic trick.

### Decompressing arithmetic codes

In addition to our magic number, our decompressor also receives the same probabilities we used to compress so it can rebuild that starting range of \[0, 1). To decode our original message, it finds which section our magic number falls into and records that symbol. Then it shrinks the range to fit within that section, and repeats the whole process.

Give it a try:

<svg aria-hidden="true" viewBox="49 -71.2 862 344.8" preserveAspectRatio="xMidYMid meet" style="font-family:monospace"><g style="transform-box:view-box;transform-origin:0 0"><g transform="translate(52 0) scale(1)"><defs><clipPath id="_R_5jaahl5_-0"><rect x="0" y="0" width="856" height="72" rx="10"></rect></clipPath></defs><g clip-path="url(#_R_5jaahl5_-0)"><rect x="0" y="0" width="489.1428571428571" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="489.1428571428571" y="0" width="244.57142857142856" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="733.7142857142857" y="0" width="122.28571428571428" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect></g><line x1="489.1428571428571" y1="0" x2="489.1428571428571" y2="72" stroke="#a3a3a3" stroke-width="2"></line><line x1="733.7142857142857" y1="0" x2="733.7142857142857" y2="72" stroke="#a3a3a3" stroke-width="2"></line><rect x="0" y="0" width="856" height="72" rx="10" fill="none" stroke="#a3a3a3" stroke-width="2"></rect><text x="244.57142857142856" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill: #6b7280; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px">A</text> <text x="611.4285714285713" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill: #6b7280; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px">B</text> <text x="794.8571428571428" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill: #6b7280; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px">C</text> <text x="0" y="78" font-size="15" text-anchor="start" dominant-baseline="hanging" style="fill:#737373">0</text> <text x="489.1428571428571" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" style="fill:#737373">0.571</text> <text x="733.7142857142857" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" style="fill:#737373">0.857</text> <text x="856" y="78" font-size="15" text-anchor="end" dominant-baseline="hanging" style="fill:#737373">1</text></g></g></svg>

Range: \[0.00000, 1.00000)

Pretty neat, huh?

We’ve now seen how an entropy coder can compress our data using a set of probabilities. As cool as arithmetic coding is (it’s not just me, right?), much of the heavy-lifting comes from the model. Remember: compression loves *redundancy*. Given this, what do you think would happen if our symbols had more repetition?

## How probabilities affect compression

Here’s a new string where the letter A dominates, with a probability of 0.833.

| Symbol | Probability |
| --- | --- |
| A | 0.833 |
| B | 0.083 |
| C | 0.083 |

It turns out, this skewed probability distribution makes a *big* difference. Let’s see how it stacks up against our old string when we apply arithmetic coding:

|  |  |  |
| --- | --- | --- |
| Length | 7 symbols | 12 symbols |
| Raw ASCII | 56 bits | 96 bits |
| Compressed output size | ~10 bits | ~10 bits |
| Avg bits / symbol | 1.38 bits | 0.82 bits |
| Final number | 0.3876953125 | 0.1474609375 |

Our first string managed to compress to an average of 1.38 bits/symbol, whereas our longer string compressed to 0.82 bits/symbol. When your data is more skewed (i.e. the higher the probabilities of some of your symbols), the better the compression ratio.

This **avg bits/symbol** is a very important number. It’s called entropy, and it is the *bedrock* of compression.

“Wait, isn’t entropy a physics thing?” you might ask. Yes! But what we’re talking about is **Shannon entropy**, which is related to data compression (in the field of information theory). What’s cool is that its mathematical formula is nearly identical to the Gibbs formula for entropy in thermodynamics. Wild, huh?

## Entropy

Consider the following sentence:

“Yesterday I saw an animal when I was walking downtown. It was a **\_\_\_\_\_**.”

How many guesses do you think it would take you to fill in the blank? If it was a common animal like *bird*, you might get it on the first try. But what if the answer was *bear*? That would probably take quite a few guesses.

Let’s say these are the possible answers, along with their probabilities written as fractions:

1/2

1/4

1/8

1/16

1/16

bird

squirrel

cat

fox

bear

Knowing the probabilities, we can actually calculate how many guesses it would take to guess correctly, on average, per animal.

Now, notice that each animal is *half as likely* as the one before, with the exception of fox and bear (these are probabilities, so our numbers need to add up to 1). If we were to guess each animal in order, from most probable to least, we’d have a 50/50 chance of being right each time. As such, we can determine the number of guesses it would take to guess a given animal (on average) using a yes/no decision tree. We start with the most likely animal at the top, and work our way down:

<svg aria-hidden="true" viewBox="-146 -7.76 526 688.76" preserveAspectRatio="xMidYMid meet" style="font-family:monospace"><g><path d="M 0 15 V 36 Q 0 50 -14 50 H -64 Q -78 50 -78 64 V 125" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M -82 117.5 L -78 123.5 L -74 117.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 0 15 V 36 Q 0 50 14 50 H 64 Q 78 50 78 64 V 141" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 74 133.5 L 78 139.5 L 82 133.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 78 177 V 198 Q 78 212 64 212 H 14 Q 0 212 0 226 V 287" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M -4 279.5 L 0 285.5 L 4 279.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 78 177 V 198 Q 78 212 92 212 H 142 Q 156 212 156 226 V 303" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 152 295.5 L 156 301.5 L 160 295.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 156 339 V 360 Q 156 374 142 374 H 92 Q 78 374 78 388 V 449" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 74 441.5 L 78 447.5 L 82 441.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 156 339 V 360 Q 156 374 170 374 H 220 Q 234 374 234 388 V 465" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 230 457.5 L 234 463.5 L 238 457.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 234 501 V 522 Q 234 536 220 536 H 170 Q 156 536 156 550 V 611" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 152 603.5 L 156 609.5 L 160 603.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 234 501 V 522 Q 234 536 248 536 H 298 Q 312 536 312 550 V 611" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 308 603.5 L 312 609.5 L 316 603.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><rect x="-98.4" y="77" width="40.8" height="24" rx="6" fill="Canvas"></rect><text x="-78" y="89" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">yes</text></g> <g><rect x="62.4" y="77" width="31.2" height="24" rx="6" fill="Canvas"></rect><text x="78" y="89" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">no</text></g> <g><rect x="-20.4" y="239" width="40.8" height="24" rx="6" fill="Canvas"></rect><text x="0" y="251" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">yes</text></g> <g><rect x="140.4" y="239" width="31.2" height="24" rx="6" fill="Canvas"></rect><text x="156" y="251" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">no</text></g> <g><rect x="57.6" y="401" width="40.8" height="24" rx="6" fill="Canvas"></rect><text x="78" y="413" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">yes</text></g> <g><rect x="218.4" y="401" width="31.2" height="24" rx="6" fill="Canvas"></rect><text x="234" y="413" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">no</text></g> <g><rect x="135.6" y="563" width="40.8" height="24" rx="6" fill="Canvas"></rect><text x="156" y="575" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">yes</text></g> <g><rect x="296.4" y="563" width="31.2" height="24" rx="6" fill="Canvas"></rect><text x="312" y="575" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">no</text></g> <text x="0" y="0" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">bird?</text><text x="78" y="162" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">squirrel?</text><text x="156" y="324" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">cat?</text><text x="234" y="486" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">fox?</text><g><rect x="-144" y="131" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect> <text x="-78" y="153" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">BIRD</text> <text x="-78" y="171" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">1 guess</text></g> <g><rect x="-66" y="293" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect><text x="0" y="315" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">SQUIRREL</text> <text x="0" y="333" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">2 guesses</text></g> <g><rect x="12" y="455" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect><text x="78" y="477" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">CAT</text> <text x="78" y="495" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">3 guesses</text></g> <g><rect x="90" y="617" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect><text x="156" y="639" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">FOX</text> <text x="156" y="657" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">4 guesses</text></g> <g><rect x="246" y="617" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect><text x="312" y="639" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">BEAR</text> <text x="312" y="657" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">4 guesses</text></g></svg>

Let’s get back to compression. Symbols with higher probabilities help us compress better, and we see the same pattern in our decision tree: the more probable animals require fewer guesses. If we treat the animals as *symbols* and swap the yes’s and no’s for *1’s and 0’s*, the number of guesses becomes exactly the number of *bits* needed to represent each one. If we record the 1’s and 0’s we take to reach each animal you’ll see that the more common animals get shorter “codewords” (unique sequences of bits), and rarer animals get longer ones.

<svg aria-hidden="true" viewBox="-146 -7.76 526 688.76" preserveAspectRatio="xMidYMid meet" style="font-family:monospace"><g><path d="M 0 15 V 36 Q 0 50 -14 50 H -64 Q -78 50 -78 64 V 125" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M -82 117.5 L -78 123.5 L -74 117.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 0 15 V 36 Q 0 50 14 50 H 64 Q 78 50 78 64 V 141" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 74 133.5 L 78 139.5 L 82 133.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 78 177 V 198 Q 78 212 64 212 H 14 Q 0 212 0 226 V 287" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M -4 279.5 L 0 285.5 L 4 279.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 78 177 V 198 Q 78 212 92 212 H 142 Q 156 212 156 226 V 303" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 152 295.5 L 156 301.5 L 160 295.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 156 339 V 360 Q 156 374 142 374 H 92 Q 78 374 78 388 V 449" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 74 441.5 L 78 447.5 L 82 441.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 156 339 V 360 Q 156 374 170 374 H 220 Q 234 374 234 388 V 465" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 230 457.5 L 234 463.5 L 238 457.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 234 501 V 522 Q 234 536 220 536 H 170 Q 156 536 156 550 V 611" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 152 603.5 L 156 609.5 L 160 603.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><path d="M 234 501 V 522 Q 234 536 248 536 H 298 Q 312 536 312 550 V 611" fill="none" stroke-width="2" stroke-linecap="round" stroke="#d4d4d4"></path><path d="M 308 603.5 L 312 609.5 L 316 603.5 Z" stroke-width="3" stroke-linejoin="round" fill="#d4d4d4" stroke="#d4d4d4"></path></g><g><rect x="-90" y="75" width="24" height="28" rx="6" fill="Canvas"></rect><text x="-78" y="89" font-size="20" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">1</text></g> <g><rect x="66" y="75" width="24" height="28" rx="6" fill="Canvas"></rect><text x="78" y="89" font-size="20" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">0</text></g> <g><rect x="-12" y="237" width="24" height="28" rx="6" fill="Canvas"></rect><text x="0" y="251" font-size="20" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">1</text></g> <g><rect x="144" y="237" width="24" height="28" rx="6" fill="Canvas"></rect><text x="156" y="251" font-size="20" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">0</text></g> <g><rect x="66" y="399" width="24" height="28" rx="6" fill="Canvas"></rect><text x="78" y="413" font-size="20" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">1</text></g> <g><rect x="222" y="399" width="24" height="28" rx="6" fill="Canvas"></rect><text x="234" y="413" font-size="20" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">0</text></g> <g><rect x="144" y="561" width="24" height="28" rx="6" fill="Canvas"></rect><text x="156" y="575" font-size="20" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">1</text></g> <g><rect x="300" y="561" width="24" height="28" rx="6" fill="Canvas"></rect><text x="312" y="575" font-size="20" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">0</text></g> <text x="0" y="0" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">bird?</text><text x="78" y="162" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">squirrel?</text><text x="156" y="324" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">cat?</text><text x="234" y="486" font-size="16" font-weight="600" text-anchor="middle" dominant-baseline="central" style="fill:#404040">fox?</text><g><rect x="-144" y="131" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect> <text x="-78" y="153" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">BIRD</text> <text x="-78" y="171" font-size="15" font-weight="700" letter-spacing="1" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">1</text></g> <g><rect x="-66" y="293" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect><text x="0" y="315" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">SQUIRREL</text> <text x="0" y="333" font-size="15" font-weight="700" letter-spacing="1" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">01</text></g> <g><rect x="12" y="455" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect><text x="78" y="477" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">CAT</text> <text x="78" y="495" font-size="15" font-weight="700" letter-spacing="1" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">001</text></g> <g><rect x="90" y="617" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect><text x="156" y="639" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">FOX</text> <text x="156" y="657" font-size="15" font-weight="700" letter-spacing="1" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">0001</text></g> <g><rect x="246" y="617" width="132" height="62" rx="6" fill="Canvas" stroke="Canvas" stroke-width="1"></rect><text x="312" y="639" font-size="15" font-weight="700" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">BEAR</text> <text x="312" y="657" font-size="15" font-weight="700" letter-spacing="1" text-anchor="middle" dominant-baseline="central" style="fill:currentColor">0000</text></g></svg>

Assigning codewords to symbols like this is actually another type of entropy coder called **Huffman coding**, which is used in popular tools like gzip and Brotli. Instead of encoding our data into a single number, like with **arithmetic coding**, the Huffman method creates codewords to represent each symbol.

But there’s a problem: what happens when our probabilities aren’t neatly divided in half? If *cat* had a probability of 0.3973, then the likelihood of the answer being a *cat* or *not a cat* isn’t 50/50 anymore. Every path down the tree is a whole number of “guesses”, so we’re forced to round, and rounding means paying for bits we don’t need. How can we tell the *absolute fewest* number of bits required to represent a given symbol?

Turns out we can calculate this with a little bit of math:

number of bits

\=

Quick refresher: **logarithms** are the reverse of exponents. For example, asks “What is 2 to the power of 4?”. On the flip side, asks “2 to the power of *what number* equals 16?”

If we plug in our animal probabilities, you’ll see we get the same number of *bits* as *guesses* from our decision tree:

| Symbol |  | Bits |
| --- | --- | --- |
| bird |  | 1 |
| squirrel |  | 2 |
| cat |  | 3 |
| fox |  | 4 |
| bear |  | 4 |

If we get the average of all our symbols, that tells us our entropy.

The most important thing to understand about entropy is that it’s the *floor*. This is the smallest number of bits per symbol we can achieve for a given set of data. It ain’t getting any more squished.

Note: this floor only applies when you don’t want to *lose data*, but compressors like JPEG or MP3 *can* get smaller by throwing out details that won’t be missed. This is called **lossy compression**. Everything discussed here is about **lossless** compression, where no data is lost, but both rely on models and probabilities to shrink data.

But wait, if there’s really a *limit* to how much you can compress data, why isn’t there just one mega God-compressor that we use on everything? Well, that’s because entropy is specific to a set of *probabilities*. If we can make our probability distribution more skewed, we can compress things more.

But how do we do that?

## Context matters

Up until now, we’ve been working with a very simple type of model that only cares about a symbol’s frequency. `count / total_symbols` = its probability.

But *context* can greatly affect a symbol’s probability. For example, in the entire English language, the letter U has a probability of ~0.028. However, when preceded by a Q, this shoots up to ~0.999.

Wowza.

On top of that, *higher probabilities* compress into *fewer bits*. We saw this before in the **arithmetic coding** section, but now we can prove it with math:

- **U:** ≈ 5.158 bits
- **U (preceded by Q):** ≈ 0.001 bits

Using a single context to determine the probability of a symbol is called an **order-1** model. It answers the question, “Given (some context), what is the probability of (symbol)?” With **order-1**, you factor in the previous symbol as your context, but you could expand this to **order-2**, **order-3**, **order-4**, and so on, which look at the previous N symbols.

But how do we feed this into an entropy coder? Previously our model was just a table of probabilities per symbol, but with context, we suddenly have a *whole set* of tables, one for each preceding symbol. So what do we do?

Let’s see what happens when we apply **arithmetic coding** to the string “TO BE OR NOT TO BE” using an **order-1** model. Notice that with each symbol we encode, our new ranges contain a different set of probabilities.

Give it a try:

<svg aria-hidden="true" viewBox="49 -71.2 862 344.8" preserveAspectRatio="xMidYMid meet" style="font-family:monospace"><g style="transform-box:view-box;transform-origin:0 0"><g transform="translate(52 0) scale(1)"><defs><clipPath id="_R_b3aahl5_-0"><rect x="0" y="0" width="856" height="72" rx="10"></rect></clipPath></defs><g clip-path="url(#_R_b3aahl5_-0)"><rect x="0" y="0" width="142.66666666666666" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="142.66666666666666" y="0" width="190.2222222222222" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="332.88888888888886" y="0" width="237.7777777777778" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="570.6666666666666" y="0" width="95.1111111111111" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="665.7777777777777" y="0" width="95.1111111111111" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="760.8888888888888" y="0" width="47.55555555555555" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect><rect x="808.4444444444445" y="0" width="47.55555555555555" height="72" style="fill: #f3f4f6; transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;"></rect></g><line x1="142.66666666666666" y1="0" x2="142.66666666666666" y2="72" stroke-width="2" stroke="#a3a3a3"></line><line x1="332.88888888888886" y1="0" x2="332.88888888888886" y2="72" stroke-width="2" stroke="#a3a3a3"></line><line x1="570.6666666666666" y1="0" x2="570.6666666666666" y2="72" stroke-width="2" stroke="#a3a3a3"></line><line x1="665.7777777777777" y1="0" x2="665.7777777777777" y2="72" stroke-width="2" stroke="#a3a3a3"></line><line x1="760.8888888888888" y1="0" x2="760.8888888888888" y2="72" stroke-width="2" stroke="#a3a3a3"></line><line x1="808.4444444444445" y1="0" x2="808.4444444444445" y2="72" stroke-width="2" stroke="#a3a3a3"></line><rect x="0" y="0" width="856" height="72" rx="10" fill="none" stroke-width="2" stroke="#a3a3a3"></rect><text x="71.33333333333333" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px" fill="#6b7280">T</text> <text x="237.7777777777778" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px" fill="#6b7280">O</text> <text x="451.7777777777777" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:13px" fill="#6b7280">(space)</text> <text x="618.2222222222222" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px" fill="#6b7280">B</text> <text x="713.3333333333333" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px" fill="#6b7280">E</text> <text x="784.6666666666666" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px" fill="#6b7280">R</text> <text x="832.2222222222222" y="36" font-weight="700" text-anchor="middle" dominant-baseline="central" style="transition: fill 400ms cubic-bezier(0.33, 1, 0.68, 1) 300ms;font-size:40px" fill="#6b7280">N</text> <text x="0" y="78" font-size="15" text-anchor="start" dominant-baseline="hanging" fill="#737373">0</text> <text x="142.66666666666666" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" fill="#737373">0.167</text> <text x="332.88888888888886" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" fill="#737373">0.389</text> <text x="570.6666666666666" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" fill="#737373">0.667</text> <text x="665.7777777777777" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" fill="#737373">0.778</text> <text x="760.8888888888888" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" fill="#737373">0.889</text> <text x="808.4444444444445" y="78" font-size="15" text-anchor="middle" dominant-baseline="hanging" fill="#737373">0.944</text> <text x="856" y="78" font-size="15" text-anchor="end" dominant-baseline="hanging" fill="#737373">1</text></g></g></svg>

Range: \[0.00000, 1.00000)

Ok, but how much does using order-N models actually impact compression?

Take a look:

|  | (no context) | order-1 |
| --- | --- | --- |
| Length | 18 symbols | 18 symbols |
| Raw ASCII | 144 bits | 144 bits |
| Compressed output size | ~47 bits | ~21 bits |
| Avg bits / symbol | 2.59 bits | 1.16 bits |
| Final number | 0.049991400929 | 0.058705 |

Wow! Using an **order-1** model cut our compressed output by more than *half!* Clearly, adding context gives us stronger probabilities. In other words, it helps us *predict* what symbol comes next.

Do you know what else is really good at prediction?

## Language modeling and compression

To say that there’s an overlap between LLMs and compression would be a huge understatement. In fact, in 2023, [Google DeepMind released a paper](https://arxiv.org/abs/2309.10668) arguing that language modeling and compression are *two views of the same thing*.

This might seem like an odd claim. After all, when you think of using LLMs, you probably think of typing a prompt into an AI chatbot and it responding with an answer. How is that *compression*?

Well, it’s not, but stick with me.

You might have heard LLMs described as “fancy autocomplete”, and this is essentially true. When you submit a prompt to an LLM, that becomes the context the model uses to return a *set of probabilities* for the next possible words. It then chooses one of those options and appends it to the context. Rinse and repeat. That’s how LLMs generate text.

Give it a whirl:

PROMPT

TheraininSpain

Predicted words
- falls0.65
- stays0.15
- comes0.10
- pours0.06
- goes0.04

While we’re here, let’s get some terminology straight. With LLMs, what it returns aren’t technically “words” but **tokens**: numbers that represent words or parts of words. Tokens are the vocabulary an LLM uses to parse context and generate responses.

Now consider this: while entropy coders are what produce the final raw bitstream, there’s nothing in them that you can tweak to get better results. They are fixed, deterministic, and lossless. If you want better compression, you need to tweak the model so we get higher probabilities per symbol. In other words, we need a better *predictor*. And when it comes to prediction, LLMs are basically as good as it gets:

Model (LLM)

| Symbol (token) | Probability |
| --- | --- |
| falls | 0.65 |
| stays | 0.15 |
| comes | 0.10 |
| pours | 0.06 |
| goes | 0.04 |

Entropy Coder

`100101110`

Using LLMs for compression is similar to how they’re used to generate text, except that we *don’t choose the next word*. Why? Because we’re not trying to generate *new* text. We already know what the next word is! Here’s how it works: based on the previous tokens (i.e. based on the *context*), the model says, “These are the tokens I think come next, and their probabilities.” Then it looks at what the *real* next symbol is. Whatever probability the model assigned is what determines the cost, in bits. If the model is well-trained, the token it thinks has the highest probability will be the *actual* next symbol.

As you click through the demo, notice how the total bits (at the top) increases based on the probability for each token encoded. Again, the number of bits required to represent each token is determined by .

0.00 bits

PROMPT

TheraininSpainfallsmainlyontheplain

Predicted words

- The0.45
- A0.25
- In0.15
- It0.10
- This0.05

Now, if the model is *not* well-trained, it pays a price. For example, if our context is “The rain in”, a poorly trained model might give “Bermuda” a probability of 0.82, but the *actual* next word is “Spain”, which it assigned a probability of 0.02. Remember, lower probabilities require more bits, so the model is dinged for guessing wrong:

Bermuda = 0.29 bits

Spain = 5.64 bits

We can see these differences with **arithmetic coding** as well. Remember: when we encode each symbol, we’re left with a smaller and smaller range. Encoding symbols with *small* probabilities (like when our model makes poor guesses) makes our ranges *even tinier*. Our final number needs to fit inside those ranges, and the smaller the range, the more *precision* is needed. More precision = more digits = more bits.

Good model

0.61328125

(9 bits)

Bad model

0.8193759582936763763427734375

(28 bits)

The actual total is rounded up, since computers can’t do “partial” bits.

That said, even archaic LLMs that are considered crappy by today’s standards can achieve some impressive compression ratios. Here’s how an **order-1** model stacks up against GPT-2 with arithmetic coding in compressing a famous Charles Dickens quote:

“It was the best of times, it was the worst of times, it was the age of wisdom, it was the age of foolishness, it was the epoch of belief, it was the epoch of incredulity, it was the season of Light, it was the season of Darkness.”

- order-1434 bits · 24% of original
- GPT-2176 bits · 10% of original

So if LLMs are so great at compression, why aren’t we using them everywhere?

## Compression in the wild

Unfortunately, how good a model is at compressing alone doesn’t give us the full picture. See, the goal of compression tools isn’t just to shrink data as much as possible. It’s to shrink the data as much as possible, *given certain resource constraints.*

Take HTTP responses: when your browser requests a webpage, it sends a header like `Accept-Encoding: gzip, br`, telling the server which compression formats it can decode (gzip, Brotli, etc). The server picks one to compress the response before sending it.

Let’s assume a server uses gzip to compress its response. When your browser receives this response, it uses a small, built-in model to decode the gzip-compressed bitstream into HTML, CSS, and JavaScript. The overhead is *tiny*. If we were to instead use an LLM for this job, both the browser *and* the server would need a copy of the LLM, which could be multi-gigabytes large. That’s a high price for good compression, and we haven’t even *run* the thing. Compressing (and decompressing) data would demand a *lot* of resources and degrade page load speed to an *unusable degree*. Imagine: for every stylesheet, every script, every JSON payload running an LLM to compress and decompress. Yuck.

For a task as trivial as squishing HTTP responses, LLMs are comically overkill: once you factor in the model’s size, you’d be shipping gigabytes to save a few KB. But even if you were trying to compress datasets that dwarf the size of the LLM, the astronomical amount of compute required would *still* make this impractical.

## Two sides, same coin

Compressing data down to its entropy is, at this point, a solved problem. **Arithmetic coding**, developed in the late 1970s, lands within a couple bits of the limit, and these days entropy coders compete on speed and memory, not ratio.

The open question is how small we can make our entropy. Better models—better *predictors* —help us lower this number. LLMs are fantastic at this (setting aside the overhead cost), but what’s really interesting is that they’re trained to minimize that exact *bits-per-symbol number*. With LLMs this is called **cross-entropy**, but it’s the same underlying formula. So while in compression entropy measures how small we can *shrink* things, in language modeling, it’s a number we reduce to make our model *better at prediction*. If you’d like to dig into the nitty-gritty of this, check out [this article](https://colah.github.io/posts/2015-09-Visual-Information/) by Chris Olah.

At the end of the day, though, both LLMs and compression algorithms are predictors. They’re two expressions of the same underlying math. Compression is prediction, and LLMs are compressors.