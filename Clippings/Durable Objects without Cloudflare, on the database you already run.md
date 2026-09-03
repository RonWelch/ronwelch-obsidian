---
title: Durable Objects without Cloudflare, on the database you already run
source: https://lucascarlson.net/2026/08/24/introducing-open-source-durable-objects/
author:
  - "[[Lucas Carlson]]"
published: 2026-08-24
created: 2026-09-01
description: The Durable Objects programming model as a plain library on the SQL database you already run. And as of this week, the whole runtime runs in a browser tab on SQLite WASM.
tags:
  - clippings
  - DurableObjects
  - cloudflare
  - actormodel
---
For a few years I have kept coming back to one idea from Cloudflare's Durable Objects: an object with a name.

One single-threaded object per identity, addressed by name, with durable state attached. A shopping cart, a chat room, a game table, a device, a document, an agent run: each one is an object.

Calls to one identity run one at a time, so two requests can't corrupt the same cart. Calls to different identities run in parallel. Kenton Varda's team shipped that, and it removes work the rest of us usually do with a database, Redis, a queue, and a pile of locks.

I have spent twenty years building that pile of locks, in Rails apps, in Node services, and once, regrettably, in a spreadsheet importer. When the Durable Objects model finally clicked for me, my main reaction was frustration that it only ran on one vendor's edge network.

I'm not alone in wanting this. Ryan Dahl's [celld](https://celld.dev/) recreates the model as a self-hosted daemon: your VMs, your object-storage bucket, the Workers API without Cloudflare. It's a good project. But celld solves it at the infrastructure level, where you run it by adding nodes, buckets, monitoring, and one more thing to scale.

I wanted it one level down, closer to where most of us work: what if Durable Objects were just a library, running on the SQL database you already have?

So I built it. It's called [Solid Objects](https://solidobjects.dev/), it's MIT, and it ships in two implementations that share one design: a [Ruby gem](https://github.com/cardmagic/solid-objects-ruby) that runs in production in an app with over 100,000 users, and a [TypeScript package](https://github.com/cardmagic/solid-objects-js) for Node.

This post is about the TypeScript one, which recently gained something I did not originally plan for: the whole runtime can run inside a browser tab.

## An actor is just a class

Here is the whole programming model:

```typescript
import { Actor, createRuntime } from "solid-objects"
import { sqlite } from "solid-objects/database/sqlite"

class TicketSale extends Actor {
  static override readonly actorType = "TicketSale"

  remaining = 100
  holds: Record<string, number> = {}

  reserve({ buyer }: { buyer: string }): boolean {
    if (this.remaining === 0 || buyer in this.holds) return false

    this.remaining -= 1
    this.holds = { ...this.holds, [buyer]: Date.now() }
    this.schedule({ at: new Date(Date.now() + 600_000), key: buyer }).expire!({ buyer })
    return true
  }

  expire({ buyer }: { buyer: string }): void {
    if (!(buyer in this.holds)) return

    const rest = { ...this.holds }
    delete rest[buyer]
    this.holds = rest
    this.remaining += 1
  }
}

const runtime = createRuntime({
  database: sqlite({ path: "sale.sqlite3" }),
  authorizeMessage: () => true,
  authorizeQuery: () => true,
})
await runtime.install()

const sale = runtime.ref(TicketSale, "event-42")
await Promise.all([sale.reserve({ buyer: "ava" }), sale.reserve({ buyer: "kai" })])
```

Every `reserve` call lands in a durable mailbox for `event-42` and runs one at a time, even when different requests or different Node processes fire them at once. Because the check on `remaining` and the write after it can't interleave, the sale can't oversell.

Different events run in parallel, so one busy sale never blocks another. State is just rows in SQLite, Postgres, or MySQL. An idle sale costs you those rows and nothing else. No process, no daemon waiting around behind it.

That idle cost is the main reason I built it this way. Cloudflare runs the model as a managed platform, and celld runs it as a self-hosted fleet. Solid Objects is a dependency in package.json. You don't operate anything new, because the database it needs is already running.

The ten-minute hold uses a durable reminder. Calling `schedule` with the buyer's key arms one alarm for that hold, and calling it again with the same key moves the alarm instead of adding a second one. The alarm sits in the same database as the state, so it survives a deploy or a crash. There is no cron sweeper and no `expires_at` column to keep in sync.

This is more than a small example can show: a guarded write, a per-item timer, and ordered concurrency, all in one class. Normally you would assemble that from a column, a background job, and a lock, and then maintain the seams between them.

You can try the claims with a single command:

```bash
npm exec --yes --package=solid-objects@latest -- solid-objects quickstart
```

It fires 25 concurrent calls at one identity and checks that they serialize to a final state of 25, returning the full sequence 1 through 25, while unrelated identities overlap freely. Every check is an assertion, so the command exits non-zero if one fails. I wanted the claims to be something you can check rather than take on faith.

## How a turn commits

The correctness core is small. In pseudo-code, one turn looks like this:

```
# what happens when you call sale.reserve({ buyer })

insert durable message "reserve on event-42"    # survives a crash from here on

worker claims TicketSale "event-42" (one lease) # one worker at a time
state  = load(TicketSale, "event-42")
result = state.reserve(buyer)                   # your code runs here,
                                                # outside any transaction
transaction do                                  # one atomic commit:
  assert the lease is still valid               #   a stale worker fails here
  save the new state                            #   remaining and holds
  save everything the handler staged            #   the 10-minute expiry alarm
  mark the message done
end

reply to the caller with result
```

An attempt may run more than once, but only one attempt ever commits.

A call becomes a durable message in the actor's mailbox. A worker claims the actor under a lease with a fencing token. Your handler runs *outside* any database transaction, so a slow handler never holds a lock on anything.

When the handler returns, one fenced transaction commits the new state together with everything it staged: outbound messages to other actors, scheduled reminders, external effect intents. And if the lease went stale while the handler was running, that commit is rejected inside the very transaction that would have written it.

Delivery is at-least-once with strict per-identity order. External effects can run twice, so they carry a stable effect id and you make them idempotent. I don't claim exactly-once delivery, because once external side effects are involved there is no such thing.

Last week someone challenged the fencing claim, in the sharpest version of the objection I had seen. Death is the easy case, they argued; the dangerous case is the holder that does not die. A worker claims an actor, hits a long GC pause, loses its lease, a second worker takes over and commits, and then the first worker wakes up and tries to land its stale write. If the fence check and the write are two steps, the late write lands and your history forks.

So I ran exactly that. Two worker processes, a 250ms lease, and a handler that synchronously blocks the event loop for 2.5 seconds, which freezes lease renewal the same way a GC pause would. The observed timeline:

```
t+0ms     worker A claims the message, stalls
t+261ms   lease expired; worker B claims, executes, commits
t+2500ms  worker A wakes, finishes its handler, attempts its commit
final     state contains attempt 2 only; A's write is fenced out
```

The late write never landed, in any run, because the fence re-check runs inside the commit transaction. Everything else in the system depends on that property.

The source of that challenge is my favorite part of the launch. I posted the project on a forum whose participants are AI agents and asked them to break it, and an agent that builds settlement systems replied with three failure probes, ranked by where this kind of model tends to crack.

The stall test above was its first probe. Its third, two independent recoveries from the same database snapshot replaying every mailbox in identical order, passed too. The second was a good idea I had not built yet, so it is [an open issue](https://github.com/cardmagic/solid-objects-js/issues/23) now. That review improved the project more than anything I could have written about it.

## The whole runtime runs in the browser

Version 0.14 runs the complete runtime, the same mailbox, leases, fencing, reminders, and effects, inside a browser module worker. The database is SQLite compiled to WASM. Durable storage is OPFS, the browser's origin-private file system, so committed actor state survives page reloads and browser restarts. Actors look exactly like they do in Node:

```javascript
import { Actor, configure, sharedSqliteWasm } from "solid-objects/browser/host"

class Counter extends Actor {
  static actorType = "Counter"

  count = 0

  increment({ amount = 1 } = {}) {
    this.count += amount
    return this.count
  }
}

const runtime = configure({
  database: sharedSqliteWasm({ path: "app.db" }),
  authorizeMessage: () => true,
  authorizeQuery: () => true,
})
await runtime.install()

await Counter.ref("page-hits").increment()
```

That code runs the same in every tab of the origin. The harder part is underneath: a browser gives you no process supervisor, so the runtime builds one out of web primitives.

The Web Locks API elects one database holder per origin. Every other tab forwards its SQL to the holder over a BroadcastChannel. When the holder's tab dies, the lock releases, the next tab promotes itself, and the runtime picks up from the same OPFS state. The leases and fencing that arbitrate Node processes arbitrate your browser tabs, unchanged.

You don't have to take my word for it. [The homepage runs the runtime on the page.](https://solidobjects.dev/js#demo) The demo there is a durable actor committed to SQLite WASM in your own browser, and loading the page was itself a committed actor call. Reload the page and the state survives. Open it in a second tab, close the one holding the database, and watch the other tab take over the same state.

You can also try it without installing anything. One import in a module worker pulls the runtime and the WASM from a CDN:

```javascript
import { Actor, configure, sharedSqliteWasm }
  from "https://esm.sh/solid-objects@latest/browser/host"
```

## Offline writes drain into Node, or into Rails

A durable browser actor eventually needs to reach the server. For that there is the transmit family. An actor stages an outbound call in the same transaction as its own state change, with one extra line:

```javascript
class Counter extends Actor {
  static actorType = "Counter"

  count = 0

  increment({ amount = 1 } = {}) {
    this.count += amount
    this.transmit().increment({ amount })  // staged in the same commit
    return this.count
  }
}
```

Because the intent commits with the state, a crash can never leave you with a local write the server will never hear about, or a server call for a write that rolled back. A drain worker then delivers each envelope with at-least-once delivery and per-actor order, and you supply the transport. Throw while offline and the effect retries with backoff:

```javascript
registerTransmit({
  runtime,
  deliver: async (envelope) => {
    const response = await fetch("/sync", {
      method: "POST",
      headers: { "content-type": "application/json" },
      body: JSON.stringify(envelope),
    })
    if (!response.ok) throw new Error(\`sync failed with ${response.status}\`)
  },
})
```

On a Node server, the ingest is one call. It enqueues an internal message keyed on `transmit:<effectId>`, so a replayed envelope applies exactly once:

```javascript
import { receiveTransmitEnvelope } from "solid-objects"

async function handleSyncRoute(request) {
  const sender = await authenticate(request)
  if (!sender) return new Response("Forbidden", { status: 403 })
  await receiveTransmitEnvelope({ runtime, envelope: await request.json() })
  return Response.json({})
}
```

But the receiving side does not have to be Node. The Ruby gem speaks the same wire contract, pinned by a golden fixture file committed to both repositories and tested on both sides. Its Rails engine already mounts `POST /solid_objects/transmit`, behind a policy that denies by default, so a Rails backend needs only to say who may deliver:

```ruby
SolidObjects.configure do |configuration|
  configuration.authorize_transmission = lambda do |envelope:, authorization_context:|
    ActiveSupport::SecurityUtils.secure_compare(
      authorization_context.request.headers["Authorization"].to_s,
      "Bearer #{Rails.application.credentials.transmit_token}"
    )
  end
end
```

Point the browser's `deliver` callback at that route and you have an offline-first frontend draining into a plain Rails backend: one contract, both directions, since Rails actors can transmit outward too. I have wanted this since I first read about local-first software: durable actors in the tab and on the server, with a reconciliation path that keeps working when the network drops.

## What it is not

Every claim above has a boundary. Know these before you spend real time on it.

- It is pre-1.0. Expect breaking changes.
- Delivery is at-least-once, so effects must be idempotent. Only the state commit is exactly-once.
- One hot identity is serialized by design, so you cannot scale a single identity by adding workers.
- A synchronous actor call is not a substitute for an indexed row read. Measured on networked MySQL, a plain read was 4.7ms median and an actor call was 60ms. If a single SQL transaction solves your problem, use it.
- OPFS support varies by browser and WebView. The persistent adapter fails fast where it is missing.
- The published benchmarks are laptop numbers, with the methodology and sources of bias attached. Use them to understand tradeoffs, not for capacity planning, which needs your own hardware.

The [project site](https://solidobjects.dev/js) keeps a fuller version of this list next to the things it does claim.

## Go break it

I think this model should run everywhere, not only on one platform. Cloudflare proved it as a managed service, and celld proved it as a self-hosted fleet. Solid Objects is the argument for the smallest version: a library, your own database, and now a browser tab.

Everything is MIT: [solid-objects-js](https://github.com/cardmagic/solid-objects-js), [solid-objects-ruby](https://github.com/cardmagic/solid-objects-ruby), and the docs, benchmarks, and correctness contract at [solidobjects.dev](https://solidobjects.dev/). The quickstart asserts its claims and exits non-zero when one fails. If you find a case where a claim does not hold, I will credit you in the fix.