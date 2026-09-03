---
title: Ante
source: https://docs.antigma.ai/
author:
published:
created: 2026-08-11
description: "Ante — a ghost in your shell: self-contained, self-organizing, benchmarked in public"
tags:
  - clippings
  - ArtificialIntelligence
  - agentic
---
**A ghost in your shell.** Ante is a self-contained coding agent that lives in your terminal and self-organizes. One ~15MB Rust binary from [Antigma Labs](https://antigma.ai/), zero runtime dependencies, built to get the most out of any model.

It works like Claude Code or Codex, with none of their dependencies or model constraints. It can also be the optimized core for [building your own harness](https://docs.antigma.ai/usage/serve) and high-performing assistants.

Every agent claims to be good. Here are numbers you can check:[Continuously evaled, in public](https://antigma.ai/eval)

[

#1 same-model agent on Terminal-Bench 2.1: for every model we run, no other harness scores higher with that model. Every result pins the exact downloadable build and links the raw Harbor run. Live at antigma.ai/eval.

](https://antigma.ai/eval)[

A fraction of the footprint

Across the same 20 parallel tasks, Ante uses ~7× less peak memory, ~9× less average CPU, and ~5× less disk I/O than Claude Code.

](https://docs.antigma.ai/benchmarks/compare_table)[

Natively offline

A built-in inference engine runs GGUF models locally. No API key, no account, and model calls stay on your machine.

](https://docs.antigma.ai/local/overview)

The three are one design decision. An agent you can verify, afford, and run anywhere is light enough to run by the *thousands*: the substrate for self-organizing intelligence. See [Philosophy](https://docs.antigma.ai/start/philosophy).

### Beyond the headline numbers

- **Zero vendor lock-in.** Bring your own API key, subscription, or local model. Switch between 12+ providers freely — Anthropic, OpenAI, Gemini, Grok, Open Router, and more. No account required, no telemetry gate, no walled garden. Not even ours.
- **[Multi-agent orchestration](https://docs.antigma.ai/experimental/agent-org).** Coordinate multiple agents that self-organize around complex tasks.
- **Extensible.** Custom [skills](https://docs.antigma.ai/extend/skills), [sub-agents](https://docs.antigma.ai/extend/subagents), [MCP](https://docs.antigma.ai/extend/mcp), and persistent [memory](https://docs.antigma.ai/extend/memory) across sessions.
- **Obsessed with simplicity and quality.** In a world of abundance, maintaining quality and a high level of trust is more valuable than ever.

### How it works

Ante runs as a client-daemon architecture. The daemon manages structured turn lifecycles — prompt, tool calls, confirmation, execution — channeling model power through a safe, predictable loop. The client can be an interactive TUI, a headless CLI, an external process, or a channel bot via `ante gateway`.

Architecturally, Ante is moving toward an agent-centric runtime where long-lived agents coordinate through message passing — an actor model with hierarchical supervision, isolated state, and clear ownership of resources per agent.

Ante can also run as a long-lived server (`ante serve`) that external clients drive through a structured JSONL protocol — ideal for building editor plugins, web UIs, and custom integrations.

> [!-warning] -warning
> warning
> 
> Ante is currently in **preview** and under active development. Expect breaking changes, experimental features, and incomplete functionality. Currently only **macOS** and **Linux** are supported.

## Next steps[Quickstart](https://docs.antigma.ai/start/quickstart)

[

Install Ante and run your first prompt in under a minute.

](https://docs.antigma.ai/start/quickstart)[

Benchmarks

How we benchmark what we ship — and the live Terminal-Bench results.

](https://docs.antigma.ai/benchmarks/eval)[

Philosophy

The product and engineering principles we optimize for.

](https://docs.antigma.ai/start/philosophy)[

Interactive TUI

Learn to use the rich terminal interface.

](https://docs.antigma.ai/usage/tui)[

Local Models

Run local models natively, from just-a-provider to fully self-contained.

](https://docs.antigma.ai/local/overview)[

Server Mode

Drive Ante programmatically over stdio or WebSocket.

](https://docs.antigma.ai/usage/serve)[

Gateway Mode

Run Ante as a Slack or Discord bot.

](https://docs.antigma.ai/usage/gateway)[

Everyday Recipes

Task-focused guides for common TUI workflows.

](https://docs.antigma.ai/usage/providing-context)[

Providers

Pick and configure your model provider.

](https://docs.antigma.ai/usage/providers)[

Core Concepts

Understand sessions, tasks, turns, and the protocol.

](https://docs.antigma.ai/reference/core-concepts)[

Agent Organization

How Ante self-organizes agents for complex tasks.

](https://docs.antigma.ai/experimental/agent-org)[

Antix Enterprise

Scale your AI with Antigma's enterprise LLM Proxy and Identity Provider.

](https://docs.antigma.ai/antix/introduction)