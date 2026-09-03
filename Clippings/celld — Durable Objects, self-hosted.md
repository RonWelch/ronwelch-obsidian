---
title: celld — Durable Objects, self-hosted
source: https://celld.dev/
author:
published:
created: 2026-09-03
description:
tags:
  - clippings
  - cloudflare
  - actormodel
---
## How it works

The bucket is the coordinator — no membership protocol, no failure detector, no consensus. Ownership is a record in your bucket, claimed with one atomic write. celld continuously replicates each cell's SQLite state to that bucket as LTX segments.

<svg viewBox="0 0 420 680" role="img" aria-label="Schematic. A boundary labelled 'your infrastructure' contains a celld node dense with live cells above your S3 bucket, which records who owns each cell and holds cell state as SQLite plus tens of thousands of inactive cells. Traffic enters and leaves only through this boundary. The node exchanges state with the bucket, and requests and responses reach the node from outside the boundary." style="display: block;"><defs><marker id="ahg-p" markerWidth="9" markerHeight="9" refX="6.5" refY="3" orient="auto-start-reverse"><path d="M0,0 L6.5,3 L0,6 Z"></path></marker><pattern id="live-p" width="8" height="8" patternUnits="userSpaceOnUse"><rect width="5.5" height="5.5" rx="1.2"></rect></pattern><pattern id="hib-p" width="8" height="8" patternUnits="userSpaceOnUse"><rect x=".4" y=".4" width="5.5" height="5.5" rx="1.2"></rect></pattern></defs><rect x="10" y="62" width="400" height="596" rx="10" fill="none" stroke="currentColor"></rect><text x="24" y="86" style="font-size: 15px;" fill="currentColor">YOUR INFRASTRUCTURE</text> <line x1="230" y1="30" x2="230" y2="118" marker-end="url(#ahg-p)" stroke="currentColor" stroke-opacity="0.2"></line><line x1="340" y1="120" x2="340" y2="30" marker-end="url(#ahg-p)" stroke="currentColor" stroke-opacity="0.2"></line><text x="230" y="16" text-anchor="middle" style="font-size: 12.5px;" fill="currentColor">requests</text> <text x="340" y="16" text-anchor="middle" style="font-size: 12.5px;" fill="currentColor">responses</text> <g><rect x="30" y="120" width="360" height="88" rx="6" fill="none" stroke="currentColor"></rect><text x="50" y="144" style="font-size: 15px;" fill="currentColor">celld on its own VM</text> <rect fill="url(#live-p)" x="48" y="160" width="317.5" height="21.5"></rect><text x="50" y="198" style="font-size: 13px;" fill="currentColor">each cell is one sqlite database</text></g> <line x1="210" y1="208" x2="210" y2="256" stroke="currentColor" stroke-opacity="0.2"></line><text x="222" y="236" style="font-size: 12.5px;" fill="currentColor">LTX</text> <g><rect x="30" y="256" width="360" height="350" rx="8" fill="none" stroke="currentColor"></rect><text x="210" y="284" text-anchor="middle" style="font-size: 15px;" fill="currentColor">YOUR S3 BUCKET</text> <text x="210" y="304" text-anchor="middle" style="font-size: 13px;" fill="currentColor">SQLite / LTX</text> <rect fill="url(#hib-p)" x="48" y="320" width="318.5" height="254.5"></rect><text x="210" y="592" text-anchor="middle" style="font-size: 13px;" fill="currentColor">10,000s of inactive cells</text></g></svg> Reliability

Durable Objects is a strong programming model. celld keeps that model while moving placement, state, and operational evidence into infrastructure you choose.

**What changes when you run the model yourself.**

PLACEMENT

#### No shared machine to lose

A cell's identity isn't fused to a machine — ownership is a lease in your bucket, granted by compare-and-swap. Lose a node and another acquires the lease and restores the cell in seconds: your fleet reading your storage, not a vendor restoring a placement you can't see.

BLAST RADIUS

#### A failure domain you choose

Your fleet still depends on its machines, network, and bucket provider. What changes is tenancy: no shared Durable Objects scheduler or placement layer can couple your application to another customer's workload.

LEGIBILITY

#### A failure you can read

When a cell misbehaves the evidence is on your disk — the ownership record, the SQLite and LTX files, and the logs. You answer “what happened to my cell” with sqlite3 and grep, not a status page that declines to say.

Self-hosting is not automatically more reliable. It makes the failure domain explicit and inspectable: your nodes, your bucket provider, and your operational choices.

**And to be clear about the tone:** we love Cloudflare — this very page is served by a Cloudflare Worker. The Durable Objects model — a single-threaded object with its own storage, addressed by name — is one of the best primitives distributed systems has gained in years, and that design is **Kenton Varda** 's and the **Cloudflare Workers** team's. celld is a love letter to their idea; a primitive this good deserves to run anywhere.

celld= [V8](https://v8.dev/) + [SQLite](https://www.sqlite.org/) + [LTX](https://github.com/superfly/ltx)

a stateful distributed system that rests entirely on S3

LTX is Litestream's replica format, from [Ben Johnson](https://github.com/benbjohnson/litestream)