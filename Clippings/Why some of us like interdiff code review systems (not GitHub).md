---
title: Why some of us like "interdiff" code review systems (not GitHub)
source: https://gist.github.com/thoughtpolice/9c45287550a56b2047c6311fbadebed2
author:
  - "[[262588213843476]]"
published:
created: 2026-07-27
description: Why some of us like "interdiff" code review systems (not GitHub) - index.md
tags:
  - clippings
  - git
  - gerrit
  - stackedprs
---
## Why some of us like "interdiff" code review

I am currently in the process of evaluating [Gerrit Code Review](https://www.gerritcodereview.com/) for work. Gerrit is an open source code review tool, as the name might imply, and works with Git repositories. What that means is that given some repository, you can:

1. Write patches to the codebase, and submit them for review
2. Other people look at the code you wrote
3. They leave comments telling you to fix various problems
4. You might even fix some of them
5. Code review is a pretty good idea, in general
6. If it's open source, someone might merge your code, and you let out a sigh of relief, knowing that you are off the hook and have increased their responsibilities and technical debt for all time
7. If it's open source, you then disappear into an eternal void, never to be seen or heard from again by the maintainer

There are many tools you can use to increase the burden on all future developers like this, such as:

- Gerrit
- GitHub (the website you are on)
- Phabricator (RIP)
- Uploading `.patch` files into a bug tracker and waiting until the maintainer sees it, downloads it, and reads it
- Emailing someone via `git send-email` telling them to pull from a `git://` URL that is hosted on a 7 year old "server" somewhere in your house and then reading the email they send back
- Implainting the idea in your coworkers head with a long discussion so that they end up implementing it while you do something else

I have done all of these, both as the person writing *and* reviewing the patches, and they are all workable to various degrees.

Some open source projects also use Gerrit, such as the Go programming language. Some, like KDE and LLVM, used Phabricator. We have entertained the idea of using Gerrit for [Jujutsu](https://github.com/martinvonz/jj). Linux, the kernel itself, uses the email workflow in a particular and special way. Most projects use GitHub, because it is easy and has zero activation energy.

But why are these tools important? Doesn't everyone just use GitHub? Isn't that good enough? Are all these really the same? No, they are not.

## The ideal patch series

The following image indicates a series of 3 patches, to be submitted to a software project. They have a typical child/parent relationship, like any linear sequence of commits in Git would have. We call this a *series* because it isn't just a *patch*. It's a "series" of "three" "patches." Get it?

This series represents, believe it or not, something close to the *ideal* patch series — for the author, the reviewer, and the future schmuck who will inherit your codebase. You can ignore the exact numbers for a second and whether 500LOC is too much (we'll get back to it.)

[![image](https://private-user-images.githubusercontent.com/3416/304284460-8c866c0e-0d8e-43d5-90cb-647c29799cc2.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDI4NDQ2MC04Yzg2NmMwZS0wZDhlLTQzZDUtOTBjYi02NDdjMjk3OTljYzIucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MWE4YzU2MDI2NzA1ODY5MDkyYTk4OWY4MWQ2YzJmNGVlOTRhZjlhOGFjM2Q1NDE0ZWI1OTkxYmQxZTA4MTEzOCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.dbtL2CsIH1hoWntlvUggXy_unMwPfgkrv2b5eKEdrT0)](https://private-user-images.githubusercontent.com/3416/304284460-8c866c0e-0d8e-43d5-90cb-647c29799cc2.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDI4NDQ2MC04Yzg2NmMwZS0wZDhlLTQzZDUtOTBjYi02NDdjMjk3OTljYzIucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MWE4YzU2MDI2NzA1ODY5MDkyYTk4OWY4MWQ2YzJmNGVlOTRhZjlhOGFjM2Q1NDE0ZWI1OTkxYmQxZTA4MTEzOCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.dbtL2CsIH1hoWntlvUggXy_unMwPfgkrv2b5eKEdrT0)

1. You're going to do a "thing", but notice you can clean up some code, coincidentally.
2. You write a bunch of code — in this case, you add a new API to the codebase. Maybe it's a fast data structure, intended to replace a slower one.
3. You migrate the users of the old API to the new API. Your program is now faster.

The most important point is that changes are:

1. Logically separated. And,
2. I can read the code *as if* each patch was individually applied in the series. So I start with the first patch, then the second, then the third. The series represents an *evolution of the code*, step by step.

The specifics here aren't too important. Sometimes, there will be no refactoring. Sometimes, it will be 100 lines. Sometimes, it will be 500 lines because it's a core API change and you have to break every call site and fix them. And so on and so forth.

But even if this is the ideal series in a logical sense, it still needs to go through code review. Which brings us to...

## The GitHub school of code review: "diff soup"

GitHub encourages you to do code review by *adding new commits on top of the original commits* to address reviews.

GitHub encourages this both explicitly but also implicitly, for a few reasons and due to the way the UX is designed.

Two of your coworkers, Alice and Bob, leave comments on the GitHub review. They tell you to make various fixes and adjustments, which you dutifully perform.

Also, you didn't add tests to the new API yet; it worked before, but everyone wants tests of course. So you need to add those on top as well.

Also, you made a small tweak after all that, a change to the implementation API, which is very small. Maybe you noticed a simple tweak in your fast, cutting edge data structure. Surely it can't a chain reaction that leads massive regression in production that will cause a pager to light up at 3am, right? It's only a `+/- 2 lines` delta! That's silly.

This is now what your commit series has become, and this is what you will push to your branches on GitHub for review. The original 3 commits, then several "address review" commits (orange), and finally your regression (red).

[![image](https://private-user-images.githubusercontent.com/3416/304287432-c4e37fa8-111e-4323-8834-e9b1973181eb.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDI4NzQzMi1jNGUzN2ZhOC0xMTFlLTQzMjMtODgzNC1lOWIxOTczMTgxZWIucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NzE5MjVjZDRjOThmYjYxZWNiNjkyODE4Yjk1YjZlZmNhMDVhMWJiNGYwOGE4ZjdkNjE5M2RmMzI1ZmFiNWUyNSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.aC9Zo4dai9J9Q78_WrQXcknvF8rG5EhikcTjOFs1nA8)](https://private-user-images.githubusercontent.com/3416/304287432-c4e37fa8-111e-4323-8834-e9b1973181eb.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDI4NzQzMi1jNGUzN2ZhOC0xMTFlLTQzMjMtODgzNC1lOWIxOTczMTgxZWIucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NzE5MjVjZDRjOThmYjYxZWNiNjkyODE4Yjk1YjZlZmNhMDVhMWJiNGYwOGE4ZjdkNjE5M2RmMzI1ZmFiNWUyNSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.aC9Zo4dai9J9Q78_WrQXcknvF8rG5EhikcTjOFs1nA8)

The black lines represent edges in the commit graph, i.e. the things that show up in `git log`. While the dotted colored lines represent the "implicit dependencies" caused by review. Bob for example told you to change one extra call site in the third commit (blue dotted line) and one thing in the refactored code (purple dotted line). Alice however told you to fix something in the API you added, *and* fix something in the refactoring commit (purple and green dotted lines.) Your freshly written tests are really just related to your lack of tests in the new API (green dotted line.) Finally, the `"minor"` change is a tweak to the new API code (red dotted line).

This is what your code has become, and this *sucks*. It sucks because:

1. The implicit relationship between the "fix review" commits and the original is not really visible. Only the actual parent/child relationship between Git commits remains, but that isn't the whole story, because now you don't know *why* these commits are related. (This is essentially a loss of "provenance.")
	- Notice how all the original commits were a single color, because they had one "purpose." But the orange commits have *multiple* arrows, with different colors, establishing implicit relationships. The fact that a single orange commit touches multiple original commits means that the conceptual model is now more complex; some commits represent single changes, while others might address multiple changes at once.
		- You can fix this by having each Orange commit only address exactly one complaint-per-change, i.e. only one dotted line leaving it. However, now you have 6 fixup commits instead of 4.
2. `git blame` now sucks completely because changes overlap. `blame` works on the level of a line, so if `"fix alice review"` changes a single bit in a line that came from `"minor refactoring"`, that line is now misattributed. You have to go N commits deeper to find the real change.
3. `git bisect` now also sucks. When you do bisection on these commits, the `"minor"` commit may not be the actual root cause commit; it may only be a *trigger* for a bug that existed from the moment the new API was added. It's unclear whether `"minor"` or `"new API"` are at fault, but realistically you probably have to just revert both after figuring it out because the word `"minor"` does not tell you anything about why the change was made.
	- Backing out both changes may not always be desirable or even *possible* if, for example, the new API is a massive performance uplift and backing it out will cause cascading downstream effects.
		- And also this regression may only pop up weeks later.
		- If I am your coworker, and I do `git bisect` on a 3am production issue to find the root cause of a regression, and I land on a commit that says `"minor"` from the above PR that landed 3 weeks ago, and it has your name on it? You are going to get a phone call from me within seconds where I will give you a [monologue about how scary I can be](https://www.youtube.com/watch?v=jZOywn1qArI).
		- Even if the `"minor"` commit was never added, maybe the regression would still happen, and the above might all still be true, but at least you didn't have to spend an extra 15 minutes doing archaeology bullshit while you are in a P1 conference call with 5 other people.

Just to be clear, this example is what it looks like **when only two reviews take place**. If your review is cycling 5 times, well, the above becomes far, far worse. And yes, in open source contexts, you will often go back and forth multiple times with someone, because they may not have context you do, and there may be no point in doing further review until other nits are addressed. Many projects like this go through multiple review cycles.

Part of the problem, the reason why this happens, is due to some design and UX flaws:

- **The new commits are the only way to do incremental reviews on GitHub**. In the above model, if you don't want to review the 500 lines of code in the new API over and over again, the only way to do it is by adding new commits on top, and viewing those. It's much easier to read the `"fix alice review"` patch, which is `+/- 10 lines`, than it is to read the new API — which is 500 LOC.
- **Github always shows you the whole diff by default**: When you have a PR on GitHub and view the "diff" tab, it shows you *all* the commits crammed together as a single diff; it is the moral equivalent of running `git diff master..foo-branch` and looking at that. But that isn't how people write changes, and it isn't how people read them either. This behavior *further* encourages you to add new commits on top of old ones, and then the reviewers just read those commits individually because the alternative is to re-read things you already reviewed, constantly.
- **It's difficult to view the differences between anything except branches**. In general, all of GitHub's whole UX works on the named branch model — and to a large extent so does Git itself. Because of that, it's difficult to use workflows like `git rebase`, because force-pushing a branch removes the old branch entirely. And at that point, you can't do things like `git range-diff` between branches.
	- Actually, this isn't fully true. If you force push on GitHub, you *can* see *a diff*, if you click on this weird little un-identifiable "Compare" button that pops up on the "Force Push" line that appears in a PR: [![image](https://private-user-images.githubusercontent.com/3416/305154958-1b81aeab-e1fc-4d03-8d42-c096fcb2c795.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNTE1NDk1OC0xYjgxYWVhYi1lMWZjLTRkMDMtOGQ0Mi1jMDk2ZmNiMmM3OTUucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ODA5OTVkZTNhMWQ2MGY3MDQ4ZWVmNzVkYTQ2ODY3OWVmMmRkYTcxZTY2N2UyN2ExZmZmZjdmN2I5MjYzMDk4YyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.pluHpgJAtqsTy583z0Xjy_ppPVBWM-oPBex9cNapicw)](https://private-user-images.githubusercontent.com/3416/305154958-1b81aeab-e1fc-4d03-8d42-c096fcb2c795.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNTE1NDk1OC0xYjgxYWVhYi1lMWZjLTRkMDMtOGQ0Mi1jMDk2ZmNiMmM3OTUucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ODA5OTVkZTNhMWQ2MGY3MDQ4ZWVmNzVkYTQ2ODY3OWVmMmRkYTcxZTY2N2UyN2ExZmZmZjdmN2I5MjYzMDk4YyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.pluHpgJAtqsTy583z0Xjy_ppPVBWM-oPBex9cNapicw)
		- Except it this button only shows you *the whole diff* from the previous branch head to the current branch head. Again, this means you end up re-reading
		- You're on your own figuring out the Commit IDs and punching them into the URL bar if you want something more granular.
		- Why is this button so hard to see? Nobody knows.

I call this review model "diff soup", because that's what it is: GitHub just shows you a big bundle of changes mixed together in a big bowl of gruel, and then you are expected to live with it and shovel it down your throat. Beggars can't be choosers.

## A better way: "interdiff" review (AKA git range-diff)

The idea is actually simple: instead of publishing new commits on *top* of the three original commits, just publish a new *version* of the three commits, addressing the changes.

So you start off with "version 1" or "v1" of your patch, like above.

[![image](https://private-user-images.githubusercontent.com/3416/304319502-ae00c29d-7157-4cdf-9f49-f5f4c557e41a.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDMxOTUwMi1hZTAwYzI5ZC03MTU3LTRjZGYtOWY0OS1mNWY0YzU1N2U0MWEucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NzA2NzJjYTU2ZGNlMzA4NjJkNzExMmE5NjcxOWU3MzI3MjQ0YjdlYWYxOTBhYWY3NTlhMTQwMzMzYjk3OTQyZSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.EdIcnZJA9SVRF9PilBpjA7XcRbFgsgS_FkakXkvS9JY)](https://private-user-images.githubusercontent.com/3416/304319502-ae00c29d-7157-4cdf-9f49-f5f4c557e41a.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDMxOTUwMi1hZTAwYzI5ZC03MTU3LTRjZGYtOWY0OS1mNWY0YzU1N2U0MWEucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NzA2NzJjYTU2ZGNlMzA4NjJkNzExMmE5NjcxOWU3MzI3MjQ0YjdlYWYxOTBhYWY3NTlhMTQwMzMzYjk3OTQyZSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.EdIcnZJA9SVRF9PilBpjA7XcRbFgsgS_FkakXkvS9JY)

In Git terminology, the orange `v1` on the side is, roughly speaking, the name of a branch, though normally it would be something like `aseipp/new-foobar-api` or something.

Next, Bob leaves comments, asking you to fix the refactoring and update a new call site (before Alice is done.) So you address his review by updating the first commit and third commit. This creates a new series, which would be "version 2", which we send out wholesale:

[![image](https://private-user-images.githubusercontent.com/3416/304319742-b7b4aef4-5bc0-4c12-aaaa-14e87b9fbe2e.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDMxOTc0Mi1iN2I0YWVmNC01YmMwLTRjMTItYWFhYS0xNGU4N2I5ZmJlMmUucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9N2M2YTliYjc3MDkyOTY5NTUxNzAxMjNjOGVjYjk4OTMwOTEyNzNjYmFjOWI5M2M0NDM5ZmM3NWViMTMxMjY1MiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.oZ2LeaZXUs31k1EWN61N9nZL6N5lFZzwYtkADnA1ALo)](https://private-user-images.githubusercontent.com/3416/304319742-b7b4aef4-5bc0-4c12-aaaa-14e87b9fbe2e.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDMxOTc0Mi1iN2I0YWVmNC01YmMwLTRjMTItYWFhYS0xNGU4N2I5ZmJlMmUucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9N2M2YTliYjc3MDkyOTY5NTUxNzAxMjNjOGVjYjk4OTMwOTEyNzNjYmFjOWI5M2M0NDM5ZmM3NWViMTMxMjY1MiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.oZ2LeaZXUs31k1EWN61N9nZL6N5lFZzwYtkADnA1ALo)

There is now a `v2` branch, containing 3 commits, and the patches are updated as Bob wanted them.

The orange dotted arrows take place of the previous colored dotted arrows; they effectively represent the fact the given commit has "evolved" and changed. Note that there are only dotted orange lines, and no other colors; the "implicit relationship" established by the earlier graph is no longer a concern, because there aren't any extra commits. The relationship between `v1` and `v2` is obvious. Commit A version 1 becomes Commit A version 2, and so on and so forth. The entire issue somewhat vanishes.

We repeat this process to address Alice's concerns, to add tests, and to introduce the `"minor"` regression. The result looks like this, where we end up with a final version of the three patches, version 5:

[![image](https://private-user-images.githubusercontent.com/3416/304320399-1c40e4c4-e7dd-443a-9078-501da9b0e612.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDMyMDM5OS0xYzQwZTRjNC1lN2RkLTQ0M2EtOTA3OC01MDFkYTliMGU2MTIucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NzY4YzM0OGRiMzdjMjNmOWVlNGI1MTNlMTE5NzdkZDBiZTIzNDM1ZTk2YWVkMWNmYTQ1MWZkYzA2MGU0ZTM4ZCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.IejypKepzgRx8o7kZyCu82hyk5fNVLGUp1ie_vwQbxw)](https://private-user-images.githubusercontent.com/3416/304320399-1c40e4c4-e7dd-443a-9078-501da9b0e612.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODUxNTQ0NDQsIm5iZiI6MTc4NTE1NDE0NCwicGF0aCI6Ii8zNDE2LzMwNDMyMDM5OS0xYzQwZTRjNC1lN2RkLTQ0M2EtOTA3OC01MDFkYTliMGU2MTIucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDcyNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA3MjdUMTIwOTA0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NzY4YzM0OGRiMzdjMjNmOWVlNGI1MTNlMTE5NzdkZDBiZTIzNDM1ZTk2YWVkMWNmYTQ1MWZkYzA2MGU0ZTM4ZCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.IejypKepzgRx8o7kZyCu82hyk5fNVLGUp1ie_vwQbxw)

The entire evolutionary process of the series has been inverted. At the end of this process, there are still only three commits, that address all review comments. Note that:

- `git blame` will now assign lines to changes *with less noise*. You no longer have to worry about running `git show abcdefg` on a blamed line and seeing it came from `"fix alice review"`, it will come from a change like the first commit, `"refactor and deduplicate controller code"` or whatnot.
- `git bisect` is now far more likely to tell you the new API code introduced this regression, without having to dig further.

Two of your best tools now work *much* more reliably because there is simply a better signal to noise ratio. But there's a very important and subtle difference that only the code *reviewer*, not *author*, can appreciate, which is...

**You now can review code incrementally, and not re-read the whole diff**

When Alice leaves her review on v2, telling you to change the green commit with 500 lines of code, you will eventually respond by doing so. But for Alice, **re-reading the 500 lines of code again in `v3` is a waste of time**. She wants to see an *incremental diff* that can prove that you actually listened to her. This means she might only need to read a 50 line diff — a 10x difference in code.

Alice can use a tool called `git range-diff` to do this, like so:

```
git range-diff \
  main..v1 \
  main..v2
```

In English, this means "Take the 3 commits from branch `v1`, and show the pairwise diff between the 3 commits from branch `v2`." So if `v1` has the following commit IDs:

```
A -> B -> C
```

And `v2` has the commit IDs:

```
X -> Y -> Z
```

You will see the changes between commit 1 `diff(A, X)`, followed by commit 2 `diff(B, Y)`, and commit 3 `diff(C, Z)`.

In contrast, **Github always shows you `diff(main, C)` or `diff(main, Z)`**, i.e. it shows you the entire branch as one diff, AKA diff soup.

This is the essence of "interdiff code review." You

- Don't publish new changes on top, you publish *new versions*
- You don't diff between the base branch and the tip of the developer's branch, you diff between *versions of commits*
- Now, reviewers get an incremental review process, while authors don't have to clutter the history with 30 "address review" noise commits.
- Your basic diagnostic tools work better, with a better signal-to-noise ratio.

## Interlude: Strategies for merging patches

TODO: Explain how the above is independent of your merge strategy (e.g. `git rebase` on tip versus multi-parent `git merge` commits).

## Interlude: Can you please just tell me if git rebase is evil or not so that we can derail the entire discussion over it?

It's fine. Just don't use it on public branches that you expect others to base their own commits off of. It's that simple.

Interdiff review systems typically encourage smaller, more "juicy" patches that land in the main branch more quickly than the alternative. You don't have to wait on all 5 commits to be ready to go; maybe the first 3 are OK, and the last 2 need more work. You merge 3 out of 5.

The intent of the system is that others will just base their work off the main branch, mitigating the need to have cases where you merge a remote that merged 5 remotes that merged... and so on and so forth. So you don't have long lived patches, typically, and most users don't base branches off other branches.

> [!note] Note
> Some projects tend to explicitly publish branches off of other branches, or merge public branches across repos. The most famous example is the Linux kernel. Therefore, most Linux developers will use `git rebase` to *create* a patch series and refine it, but they won't force-push any public branches after they create them, because they might get merged into someone elses tree (without them knowing.) They will just create new branches with new commits.