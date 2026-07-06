---
title: a coding agent with the IDE wired in
source: https://omp.sh/
author:
  - "[[can1357]]"
published:
created: 2026-07-06
description: "Oh My Pi (omp) is a coding agent for the terminal: subagents, plan mode, LSP, DAP, hindsight memory, hashline edits, and time-traveling rules — with a native Rust engine doing the heavy lifting."
tags:
  - clippings
  - ArtificialIntelligence
  - vibecoding
  - agentic
  - Programming
  - harness
---
§ 01oh my pi · the harness

## A coding agent with the IDE wired in.

The most capable agent surface that ships. Continuously tuned by real-world use — complete out of the box, unapologetically native. Windows-native, no WSL.

macOS · Linux

`curl -fsSL https://omp.sh/install | sh`

MIT · macOS · Linux · Windows · bun ≥ 1.3.14 · [read the source ↗](https://github.com/can1357/oh-my-pi)

40+

providers

32

built-in tools

14

lsp ops

28

dap ops

100k+

rust core

every tool, benchmaxxed

## Every tool, benchmaxxed.

Edits land on the first attempt. Reads summarize instead of dumping the file, and searches come back before the model notices. Pick any model — omp will get it right.

additions over Pi

## The Pi you love,with batteries included.

Originally built on [Mario Zechner](https://github.com/mariozechner) ’s wonderful [Pi](https://github.com/badlogic/pi-mono), omp adds everything you’re missing.

1. § 01eval
	### Code execution w/ tool-calling
	Most harnesses give the agent a Python sandbox and call it done. Ours runs persistent Python and a Bun worker, and either kernel can call back into the agent’s own tools — read, search, task — over a loopback bridge. The agent loads a CSV with tool.read from inside Python, charts it from JavaScript, and never leaves the cell.
	backends
	py (subprocess) · js (Bun Worker)
	magics
	%pip %time %%bash!cmd
	bridge
	read · write · display · tool.\<name>
	persistence
	session-scoped kernels · per-cell reset
	![omp TUI: a single eval session with `[1/2] pandas describe` (Python) printing a real DataFrame.describe() table, followed by `[2/2] top scorer` (JavaScript) running a reduce. Footer: 'Both kernels ran in one session.'](https://omp.sh/captures/eval.webp)
	omp · eval
2. § 02lsp
	### LSP wired into every write
	Ask for a rename and you get a rename. The call goes through workspace/willRenameFiles, so re-exports, barrel files, and aliased imports update before the file moves. Everything your IDE knows, the agent knows.
	operations
	14
	languages
	53 servers
	format
	on-write (textDocument/formatting)
	rename
	willRenameFiles · #N selector
	omp · lsp
	![omp TUI: `LSP references` returns five hits across three files for the symbol `formatBytes`, then `LSP rename` applies the change with edits to format.ts/report.ts/cli.ts, then a `Search formatBytes 0 matches` confirmation. Final line: 'Rename complete. Five edits across three files…'.](https://omp.sh/captures/lsp.webp)
3. § 03dap
	### Drives a real debugger
	A C binary segfaults: the agent attaches lldb, steps to the bad pointer, reads the frame. A Go service hangs: it attaches dlv and walks the goroutines. A Python process is wedged: debugpy, pause, inspect, evaluate. Most agents are still sprinkling print statements.
	operations
	28
	adapters
	14 bundled (lldb-dap · dlv · debugpy · …)
	attach
	stdio · unix · TCP · pid
	gate
	debug.enabled
	omp · dap
	![omp TUI: a live lldb-dap session against a native binary at /tmp/omp-native/demo. Adapter=lldb-dap, Status=stopped, Frame=xorshift32, Instruction pointer 0x10000055C, Location demo.c:6:10. Debug scopes and Debug variables cards show locals (x = 57351) and the agent confirms the math: x went from 7 → 57351 (= 7 ^ (7<<13)).](https://omp.sh/clips/dap-poster.webp)
4. § 04ttsr
	### Time-traveling stream rules
	Your rules sit dormant until the model goes off-script. A regex match aborts the stream mid-token, injects the rule as a system reminder, and retries from the same point. You get course-correction without paying context tax on every turn. Injections survive compaction, so the fix sticks.
	trigger
	regex on text · thinking · tool streams
	cost
	0 tokens until match
	modes
	interrupt × repeat × context
	persistence
	survives compaction
	![omp TUI: agent reading src.rs and about to write Box::leak when the request aborts (red `Error: Request was aborted`), an amber `⚠ Injecting rule: box-leak` card injects the rule body `Don't reach for Box::leak in production code paths`, and the agent then course-corrects by proposing `Arc<str>` and asking the user to confirm.](https://omp.sh/clips/ttsr-poster.webp)
	omp · ttsr
5. § 05subagents
	### First-class subagents
	Split a job across workers and get typed results back. task fans out into isolated worktrees, each worker runs its own tool surface, and the final yield is a schema-validated object the parent reads directly. No prose to parse, no merge conflicts between siblings, no orphaned edits.
	bundled agents
	8
	channel
	irc · DM, broadcast, reply
	isolation
	8 backends (CoW-first) · per-subagent worktree
	concurrency
	default 32 · 0 = unlimited
	![omp TUI showing `task` spawning two subagents `ComponentsExports` and `RoutesExports`, the constraints block requiring an IRC DM between peers, the per-subagent status cards with cost and duration, and a final Findings section listing both exports plus an honest 'IRC coordination note' about a one-sided handshake.](https://omp.sh/clips/irc-poster.webp)
	omp · subagents
6. § 06advisor
	### A second model, watching every turn.
	Pair a reviewer model to the 'advisor' role and it reads every turn the main agent takes, injecting notes inline — a quiet aside, a concern, or a hard blocker. It runs on its own context and its own model, so it catches what the doer rushed past. The main agent sees the note and course-corrects, or tells you why it won't.
	role
	advisor · any model
	severity
	aside · concern · blocker
	delivery
	interrupt or non-blocking
	scope
	main + subagents (opt-in)
	![omp TUI: /advisor status shows the advisor running on openai-codex/gpt-5.5; after the main agent scopes a catch to ENOENT instead of swallowing every error, an amber 'Advisor 1 note (concern)' card warns the fix no longer matches the user's literal acceptance criterion.](https://omp.sh/clips/advisor-poster.webp)
	omp · advisor
7. § 07collab
	### Hand someone the link, they're in.
	/collab puts your live session on a relay and hands back a link — and a QR. A teammate joins from another terminal with omp join, or just opens it in a browser. Share read-write to pair on the same agent, or /collab view for a read-only link anyone can watch but no one can steer. Frames are sealed client-side; the relay never sees your keys.
	join
	terminal · browser · QR
	modes
	read-write · view-only
	crypto
	AES-256-GCM · key client-side
	relay
	wss · default my.omp.sh
	![omp TUI: /collab view prints 'Collab session started!' with an omp join command, a my.omp.sh browser link, the note 'Anyone with this link can watch the session but cannot prompt the agent', and a large scannable QR code.](https://omp.sh/clips/collab-poster.webp)
	omp · collab
8. § 08web
	### Read a pdf on arxiv, why not?
	web\_search chains fourteen ranked providers and hands whatever URLs it finds straight to read. Arxiv PDFs, GitHub pages, Stack Overflow threads come back as structured markdown with anchors intact — the same tool surface you use on local files. Cite, follow, quote, never lose where you came from.
	providers
	14 (auto · chained)
	extractors
	arxiv · github · so · registries
	format
	markdown · anchors preserved
	fallback
	auto chain on miss
	omp · web
	![omp TUI: web_search returns 10 ranked Perplexity sources for inference-time compute scaling, the agent picks an arxiv paper, calls read https://arxiv.org/pdf/2604.10739v1, and summarizes the paper's headline result with real numbers (DeepSeek-R1-32B peak at 12K tokens, 55.8% AIME accuracy).](https://omp.sh/clips/web-poster.webp)
9. § 09native-rs
	### Unapologetically native. Even on Windows.
	Other agents shell out to rg, grep, find, and bash. On many machines those binaries don’t exist, and on the ones where they do, every call costs a fork-exec round-trip. omp links the real implementations into the process. ripgrep, glob, find: in-process. brush is the bash, with sessions that survive across calls. The same omp binary runs on macOS, Linux, and Windows — no WSL bridge.
	rust loc
	100,000+
	crates
	pi-natives · pi-shell · pi-ast · pi-iso · pi-walker · +vendor
	platforms
	linux · darwin · win32 (x64 + arm64)
	shared cache
	1000ms TTL across grep / glob / lsp
10. § 10review
	### Code review with priorities and a verdict
	Get a clear verdict on whether the change ships, with every issue ranked P0 through P3 and scored for confidence. /review spawns dedicated reviewer subagents that sweep branches, single commits, or uncommitted work in parallel. You tackle what blocks release first; nothing important hides in a wall of prose.
	command
	/review
	priorities
	P0 · P1 · P2 · P3 + confidence
	tool
	report\_finding
	verdict
	correct · incorrect
11. § 11hashline
	### Hashline: edit by content hash
	Perfect edits, fewer tokens. The model points at anchors instead of retyping the lines it wants to change, so whitespace battles and string-not-found loops just stop happening. Edit a stale file and the anchors diverge — we reject the patch before it corrupts anything. Grok 4 Fast spends 61% fewer output tokens on the same work.
	hash
	4-hex tag · xxHash32 · whole-file snapshot
	benchmark
	16 models · 180 tasks · 3 runs
	grok-code-fast
	6.7% → 68.3%
	grok-4-fast
	−61% output tokens
12. § 12github-fs
	### GitHub is just another filesystem
	Other harnesses bolt on gh\_issue\_view, gh\_pr\_view, gh\_search — each with its own parameters the agent has to learn and you have to debug. We skipped that. read already handles paths; PRs are paths. One interface to teach the model, one surface to keep correct.
	schemes
	issue:// · pr:// · pr://N/diff/M
	listings
	?state?limit?author?label
	cache
	sqlite · soft + hard TTL
	refresh
	background, never blocks
13. § 13memory
	### mnemopi: a memory system on your disk
	The agent remembers your codebase between sessions — now backed by mnemopi, a local SQLite store with vector embeddings and graph tools. It writes facts mid-run with retain, pulls them back with recall, synthesizes with reflect, and revises them by id with memory\_edit. Scoping is global, per-project, or per-project-tagged, and delegated subagents inherit the parent's memory state.
	tools
	retain · recall · reflect · memory\_edit
	backend
	memory.backend = mnemopi
	store
	local SQLite · embeddings · graph
	scope
	global · per-project · tagged
14. § 14acp
	### ACP: editor-drivable agent
	Run omp inside Zed and you get the same agent you drive from the terminal — reading the buffer you’re actually looking at, writing through the editor’s save path, spawning shells in the editor’s terminal. Destructive tools pause for a permission prompt you can answer once and forget. No bridge, no plugin, no second brain to keep in sync.
	command
	omp acp
	transport
	JSON-RPC over stdio
	gating
	session/request\_permission · allow\_always
	extensions
	\_omp/\* (6 methods)
15. § 15discovery
	### Inherits what your other tools already wrote
	Every other agent ships an importer and expects you to convert. omp reads the eight formats already on disk in their native shape — Cursor MDC, Cline.clinerules, Codex AGENTS.md, Copilot applyTo, and the rest. No migration script, no YAML-to-TOML port, no "supported subset" footnotes. The config your team wrote last quarter still works tonight.
	sources
	8 tools
	surfaces
	MCP · rules · skills · hooks · commands
	attribution
	\_source · \_shadowed · priority
	manager
	/extensions
16. § 16commit
	### omp commit: atomic splits, validated messages
	omp reads the working tree through git\_overview, git\_file\_diff, and git\_hunk, then splits unrelated changes into atomic commits ordered by their dependencies. Cycles are rejected before anything is written. Source files score above tests, docs, and configs, so the headline commit is the one that matters. Lock files are excluded from analysis entirely.
	command
	omp commit
	modes
	agentic · --legacy
	splits
	atomic + topological order
	validates
	11 conventional types · filler · meta

full changelog & details · [README › highlights ↗](https://github.com/can1357/oh-my-pi/blob/main/README.md#highlights)

shipped recently

## 15.5 → 15.11, the short version.

Live session sharing over an E2E-encrypted relay, every account's rate limits in one command, compaction rendered as dense images, a local memory system, rules derived from the agent's own slip-ups, on-device housekeeping — the wave that landed since the last cut.

collab/collab · /join

### Live session sharing, sealed end to end

`/collab` prints a room#key link; `/join` renders the same session natively in another TUI — guests prompt, interrupt, and drive the host's subagents while machine-mutating commands stay host-only. AES-256-GCM end to end, keys only in the URL fragment, view-only links for watchers, and browser guests join the same link with no install.

usageomp usage

### Every account's limits, one command

`omp usage` draws a bar per limit window for every provider and signed-in account — amounts, reset times, plan, a `✦ saved resets` marker — and `--history` adds sparklines. `/usage reset` redeems a saved Codex reset by hand; opt-in `codexResets.autoRedeem` spends one only when the weekly limit blocks a turn and no other account can take it.

snapcompactno summarizer turn

### Compaction: instant, local, free

`compaction.strategy = snapcompact` replaces the summarizer with a deterministic on-device render — pixel-font PNGs the model reads back near-verbatim at [about a third of the input price](https://blog.can.ac/2026/06/10/snapcompact/). `snapcompact.shape` auto-tunes the frame per model; `/context` shows the estimated savings.

memorymnemopi

### A memory system on your disk

mnemopi is a local SQLite memory backend (`memory.backend = mnemopi`) with vector embeddings and graph tools. The agent curates facts with `retain`, `recall`, `reflect`, and `memory_edit` — update, forget, or invalidate by id; scoping runs global, per-project, or per-project-tagged, and delegated subagents inherit the parent's memory state.

workflowsparallel · pipeline

### Workflows and LLM-as-judge, in a cell

The `eval` kernel orchestrates. `completion()` runs a stateless graded call — your judge; `agent()` dispatches a subagent under a JSON-Schema contract; `parallel()` and `pipeline()` fan work through bounded pools and stage barriers; a per-turn `budget` caps spend. The `workflowz` keyword turns the whole task into that contract.

compactionshake · handoff

### Four compaction strategies, one knob

Pick how context is reclaimed with `compaction.strategy`. `shake` surgically elides heavy tool results and large blocks with no LLM cut-point — joined by `handoff` and `snapcompact` over the original `context-full`.

rules/omfg · builtin

### Rules derived from a mistake

`/omfg <complaint>` — type what annoyed you. omp drafts a time-traveling stream rule from the conversation where things went wrong, checks the condition against that same transcript (would it have fired?), shows a live draft panel you can amend, and registers it on save under `.omp/rules/`. Bundled TypeScript and Rust convention rules ship in the box (`ttsr.builtinRules`).

on-devicetitles · memory

### A local model does the housekeeping

One shared tiny-model worker handles session titling and mnemopi memory extraction and consolidation — all on local transformers.js models (`qwen3-1.7b`, `gemma-3-1b`, `qwen2.5-1.5b`, `lfm2-1.2b`), GPU-first via `providers.tinyModelDevice`, pulled with `omp tiny-models download`. None of it leaves the machine.

sessionsRedis · SQL

### Server-side session stores

Session JSONL no longer has to sit on local disk. `RedisSessionStorage` (bun:redis) and `SqlSessionStorage` (Postgres, MySQL/MariaDB, or SQLite — dialect auto-detected) are `SessionStorage` backends with an in-memory mirror and queued async writes, exported for SDK consumers.

thinkingauto → xhigh

### auto thinking, per turn

The `auto` level classifies each real user turn and resolves a concrete low → xhigh effort — cheap turns stay cheap, hard ones get the budget. Runs online through `smol` or an opt-in on-device classifier via `providers.autoThinkingModel`.

providers40+ · one /model

### Grok, OpenRouter, Wafer

`/login` adds xAI Grok over OAuth (Grok-4.x), OpenRouter by validated key-paste with `:nitro` /`:floor` /`:exacto` routing, and Wafer Pass + Serverless. Still 40+, one `/model` away.

voice & imagetts · image

### Grok speaks, and draws

A `tts` tool speaks through Grok Voice (ara, eve, leo, rex, sal — WAV or MP3) and `generate_image` draws through `grok-imagine-image`, both in the same agent loop.

vaultvault://

### Obsidian vaults as files

`vault://<vault>/<path>` reads and edits Obsidian notes through the same `read`, `edit`, and `search` tools, gated behind `vault.enabled`.

editsreplace block N:

### Edit whole blocks, not line ranges

`replace block N:` and `delete block N` resolve a full syntactic construct through tree-sitter and rewrite its whole span without counting the closing line — a verb-based v4 grammar over opaque snapshot tags the model can't guess from file content.

orchestrateorchestrate

### One word, parallel subagents

Drop the standalone word `orchestrate` — like `ultrathink` — and omp switches into a multi-phase, parallel-subagent contract, with a live agent roster you can view-as into (`Ctrl+S`).

shellbash · zsh · fish

### Completions that never drift

`omp completions bash|zsh|fish` prints a script generated from live command and flag metadata; `--model` and `--resume` complete against the catalog and your on-disk sessions.

pluginsgithub:user/repo

### Install straight from a repo

`omp plugin install github:user/repo` — plus GitLab/Bitbucket shorthand and full git URLs — and a top-level `omp install`. The `omp-plugins` provider discovers skills, hooks, tools, commands, rules, and `.mcp.json`.

searchsearch omp://

### Search across internal URLs

`search` now resolves internal URLs like `omp://` as targets — scanning in-memory resources alongside files, honoring line-range suffixes, and expanding a match across multiple internal documents.

full changelog · [coding-agent CHANGELOG ↗](https://github.com/can1357/oh-my-pi/blob/main/packages/coding-agent/CHANGELOG.md)

compaction · instant, local, free

## Instant. Local. Free. Compaction used to make you pick two.

Every other harness compacts by calling a model: you wait out a summarizer turn, pay for it, and hope the rewrite kept the facts you'll need later. omp deletes that step. When the window fills, snapcompact renders the history into dense pixel-font PNGs in one deterministic pass and hands them back as images the model reads near-verbatim.

- instant
	A render pass, not a model turn. There is no summarizer to wait on.
- local
	Rendered on-device, deterministically. Your history is never shipped off to a side model to be rewritten — no LLM pass decides which facts survive.
- free
	Zero compaction spend — no model call to bill. Reading the archive back costs [about a third of the input price](https://blog.can.ac/2026/06/10/snapcompact/) of the text it carries.

The mechanics: a 1568×1568 frame carries [~40,000 characters](https://blog.can.ac/2026/06/10/snapcompact/) in a 6×10 pixel font — roughly [10k tokens](https://blog.can.ac/2026/06/10/snapcompact/) of text, billed under Anthropic's pixel formula as [3,279 image tokens](https://blog.can.ac/2026/06/10/snapcompact/). The receipts on the right show what survives the trip, against the prose summaries that shredded it.

> "The PNG isn't a picture of your context — it converges to being your context."

White-box evidence on Qwen2.5-VL-7B: cross-carrier retrieval matches the right question [12/12](https://blog.can.ac/2026/06/10/snapcompact/) from layer 2; question-similarity geometry correlates at [r=0.94](https://blog.can.ac/2026/06/10/snapcompact/) whichever carrier delivered the context.

shipped · settings

- `compaction.strategy = snapcompact`
	archive history as dense images — joins context-full, handoff, shake, off
- `snapcompact.shape`
	frame shape auto-picked per model; winning eval variants ship as presets
- `/context`
	shows the estimated savings before compaction runs
- `/settings`
	renders a live shape preview
- `snapcompact.systemPrompt · snapcompact.toolResults`
	experimental inline imaging beyond history
- `vision required`
	text-only models fall back to context-full

[compaction reference →](https://omp.sh/docs/compaction)

live session sharing

01 · guests drive

### Full links drive; view-only links watch.

Guests on a full link prompt the agent — every transcript entry carries an author badge — interrupt with `Esc`, and work the Agent Hub against the host's subagents: live table, chat and steer, kill, revive, transcript viewing. A view-only link admits watchers — the host verifies a write token at join and rejects prompting, interrupting, and agent control without it; read-only peers are badged in the participants list. Anything that mutates the host machine stays host-only: `/model`, `/compact`, bash `!`, python `$`, skills.

02 · sealed end to end

### The key never touches the wire.

Every payload is sealed with AES-256-GCM before it touches the socket. Links come in two strengths: a full link packs the 32-byte room key plus a 16-byte write token into the URL fragment (48 bytes, base64url); a view-only link carries the bare key. Fragments never appear in HTTP requests, so the relay sees neither. What it does see: room ids, connection counts, opaque ciphertext frames and their sizes, and a 4-byte routing prefix. Possession of the link is the trust boundary.

03 · browser guests

### The same link opens in a browser.

`collab-web` is a standalone web client for the same links — no omp install. Live transcript with streaming text, thinking, and tool cards, a subagent panel, and a composer with the same guest powers. It ships as static `dist/`; the key stays in the fragment and the page talks only to the relay.

commands

- `/collab` start sharing — prints the room#key link
- `/collab view` print a view-only link for watchers
- `/join <link>` enter from any directory, any machine
- `/leave` restore the guest's previous session
- `/collab status` link + participants
- `/collab stop` close the room

Link format: `<room>#<key>` — `mgAYTZwEnpRQtca0CTgn-Q#gdJUbTovD94o…` — everything after `#` stays in the client.

every surface is a URL

## Read PRs. Walk skills.Pull JSON out of subagents.

Twelve internal schemes — `pr://`, `issue://`, `agent://`, `skill://`, `rule://`, and the rest — resolve transparently inside every FS-shaped tool the agent already calls. `read pr://1428` returns the same shape as `read src/foo.ts`. `search` walks a diff like a directory. `agent://<id>/findings.0.path` pulls a field out of a subagent’s output by path.

There is no `gh view`, no `skill show`, no `agent get`. The tool count stays flat because the URL space carries the variation — and the model already knows `read`.

PRs and issues cache on disk; second reads come back free. Selectors compose — `pr://1428/diff/2:50-100` lands on lines 50–100 of the second changed file. `:50+150`, `:raw`, `:conflicts` behave on URLs the way they behave on paths.

![omp TUI: ✓ Read pr://can1357/oh-my-pi/1063, then ✓ Read pr://can1357/oh-my-pi/1063/diff/1 with the diff body, hunk headers and added lines. Final summary lists changed file with [MODIFIED] (+12 -0).](https://omp.sh/captures/pr.webp)

call sequence · captured read pr:// / / · then /diff/

previews & conflicts · captured

## Nothing wide ships without a handoff.

Two flows, captured straight out of omp. Merges become writable URLs — `conflict://N` with `@ours` / `@theirs` / `@base` shorthands. Codemods stage a preview the agent has to `resolve` before anything lands. Both reuse `read`, `write`, and `ast_edit` — no merge UI, no codemod runner. The screenshots below are the actual TUI output.

01 · conflict://

### One conflict, one URL.

Each merge conflict becomes one URL. The agent writes `@theirs`, `@ours`, or `@base` to `conflict://N` and the file resolves cleanly. Bulk form: `conflict://*`.

omp · vhs

![omp TUI: ✓ Read src/session.ts (⚠ 1 conflict), then ✓ Write conflict://1 · 1 line with content @theirs, then a confirmation 'Resolved.'](https://omp.sh/clips/conflict-poster.webp)

02 · ast\_edit · resolve

### Preview, then accept.

`ast_edit` returns a (proposed) card with the replacement count. The change is staged. The agent calls `resolve` with a reason; the TUI turns it into an Accept card and the disk move happens — atomic, all or nothing.

omp · vhs

![omp TUI: ✓ AST Edit: console.log($X) (proposed) 3 replacements · 1 file, then ✓ Accept: 3 replacements in 1 file (AST Edit), followed by 'Applied 3 replacements in src/auth.ts.'](https://omp.sh/clips/codemod-poster.webp)

also stagedmulti-target ast\_edit · custom-tool previews · plan approval gate · discard is single-callno partial applies, ever

built-in tools

## Whatever the task needs, it's already in the box.

32 tools live in the same namespace as `read` and `bash`. Pin the active set with `--tools read,edit,bash,…` and the rest stay hidden but indexed — `search_tool_bm25` pulls them back in mid-session when `tools.discoveryMode` says so.

Files & search

read

files, dirs, archives, SQLite, PDFs, notebooks, URLs, and internal:// schemes through one path.

write

create or overwrite a file, archive entry, or SQLite row.

edit

hashline patches with content-hash anchors and stale-anchor recovery.

ast\_edit

structural rewrites previewed before apply, via ast-grep.

ast\_grep

structural code queries over 50+ tree-sitter grammars.

search

regex over files, globs, and internal URLs.

find

glob-based path lookup; reach for search when you need content matches.

Runtime

bash

workspace shell, with optional PTY or background-job dispatch.

eval

persistent Python and JavaScript cells with shared prelude and tool re-entry.

ssh

one remote command against a configured host.

Code intelligence

debug

drive a DAP session — breakpoints, stepping, threads, stack, variables.

Coordination

task

fan out subagents in parallel, optionally workspace-isolated.

irc

short prose between live agents in this process.

todo

ordered mutations over the session todo list with phase tracking.

job

wait on or cancel background jobs.

ask

structured follow-up questions for interactive runs.

Misc

resolve

apply or discard a queued preview action.

search\_tool\_bm25

BM25 over the hidden tool index; activates top matches mid-session.

Outside the box

browser

Puppeteer tabs over headless Chromium or CDP-attached apps.

web\_search

one query across configured providers, returning answer plus citations.

github

GitHub CLI ops — repo, PR, issues, code search, Actions run-watch.

generate\_image

generate or edit raster images via Gemini, GPT, or xAI Grok image models.

inspect\_image

vision-model analysis of a local image file.

render\_mermaid

Mermaid source to terminal-friendly ASCII or PNG.

tts

speak text through xAI Grok Voice — five built-in voices, WAV or MP3.

Memory & state

checkpoint

mark conversation state for a later collapse-and-report.

rewind

prune exploratory context, keep a concise report.

retain

queue durable facts into the active memory bank (mnemopi or Hindsight).

recall

search the memory bank for raw memories.

reflect

ask the memory backend to synthesize an answer over the bank.

memory\_edit

update, forget, or invalidate a recalled memory by id.

Setting-gated, off by default: `github`, `inspect_image`, `render_mermaid`, `tts`, `checkpoint`, `rewind`, `search_tool_bm25`, `retain`, `recall`, `reflect`, `memory_edit`. Flip them on once, scoped per project.

[Full reference →](https://omp.sh/docs/tools)

models & providers

## Forty-plus providers, hundreds of models, one /model away.

Roles route work by intent. `default` for normal turns. `smol` for cheap subagent fan-out. `slow` for deep reasoning. `plan` for plan mode. `commit` for changelogs. Override at launch with `--smol`, `--slow`, or `--plan`; cycle through the configured models for the active role with Ctrl + P.

group A

### Frontier APIs

Direct APIs and gateways. Mix providers per role.

- Anthropicoauth
- OpenAI
- OpenAI Codexoauth
- Google Gemini
- Google Antigravityoauth
- xAIoauth
- Mistral
- Groq
- Cerebras
- Fireworks
- Together
- Hugging Face
- NVIDIA
- OpenRouter
- Synthetic
- Vercel AI Gateway
- Cloudflare AI Gateway
- Perplexityoauth

group B

### Coding plans

Subscription-routed. /login attaches the session.

- Cursoroauth
- GitHub Copilotoauth
- GitLab Duo
- Gemini CLIoauth
- Kimi Codeplan
- Moonshot
- MiniMax Coding Planplan
- MiniMax Coding Plan CNplan
- Alibaba Coding Planplan
- Qwen Portal
- Z.AI / GLM Coding Planplan
- Xiaomi MiMo
- Qianfan
- NanoGPT
- Venice
- Kilo
- ZenMux
- OpenCode Go
- OpenCode Zen
- Wafer Passplan
- Wafer Serverless

group C

### Run it yourself

OpenAI-compatible /v1/models. Local instances skip the key.

- Ollamalocal
- Ollama Cloud
- LM Studiolocal
- llama.cpplocal
- vLLMlocal
- LiteLLM

Custom providers

Declare anything that speaks `openai-completions`, `openai-responses`, `openai-codex-responses`, `azure-openai-responses`, `anthropic-messages`, `google-generative-ai`, or `google-vertex` in `~/.omp/agent/models.yml`.

Fallback chains

Per-role chains under `retry.fallbackChains`. When the primary throws 429s or hits a quota wall, the next entry takes the rest of the turn — restored on cooldown.

Path-scoped models

Scope entries in `enabledModels` or `disabledProviders` with `paths:` to pin a different model list on one repo without touching the global config.

Round-robin credentials

Stack API keys per provider and the runtime rotates with session affinity and per-credential backoff. Useful when one key would burn its quota by lunch.

[full provider & routing reference ↗](https://omp.sh/docs/providers) /docs/providers

search & retrieval

## Fourteen backends. One tool the agent already knows.

`web_search` is built in, not bolted on. `auto` walks a fourteen-provider chain; pin one by name if you already pay for it. Behind every hit, site-aware extraction turns GitHub, registries, arXiv, Stack Overflow, and docs into structured markdown — anchors and link targets survive.

01

### Search providers

Fourteen backends. Pin one, or let auto walk the chain in order.

- autochain
- exaEXA\_API\_KEY
- braveBRAVE\_API\_KEY
- jinaJINA\_API\_KEY
- kimiMOONSHOT\_API\_KEY
- zaiZAI\_API\_KEY
- anthropicoauth
- perplexityPERPLEXITY\_API\_KEY
- geminioauth
- codexoauth
- tavilyTAVILY\_API\_KEY
- parallelPARALLEL\_API\_KEY
- kagiKAGI\_API\_KEY
- syntheticSYNTHETIC\_API\_KEY
- searxngself-hosted

02

### Specialised handlers

The agent gets structured content, not stripped HTML.

- Code hostsgithub, gitlab
- Package registriessee list below
- Research sourcesarxiv, semantic scholar
- Forumsstack overflow, reddit, hn
- Docsmdn, readthedocs, docs.rs

package registries

npm ·PyPI ·crates.io ·Hex ·Hackage ·NuGet ·Maven ·RubyGems ·Packagist ·pub.dev ·Go packages

03

### Security databases

Vuln lookups answer with vendor data, not blog summaries.

- NVD
	national vulnerability database
- OSV
	open source vuln feed
- CISA KEV
	known exploited vulns

Pages convert to markdown with link structure intact. The agent can cite, follow, and quote without losing anchors.

[web\_search reference ↗](https://omp.sh/docs/tools#web_search)

the browser is a tool

## Drives a real browser.Or your Slack?

Stealth’s on by default, so pages see a normal user instead of a headless bot. The same API drives any Electron app in place — point it at Slack and the agent reads your DMs the way it reads the web.

![omp TUI driving the browser tool: opens https://duckduckgo.com in a tab named 'web', observes the DOM, types 'omp coding agent github' into the searchbox, waits for navigation, calls tab.screenshot which renders the results page inline (the actual DuckDuckGo results image appears in the terminal), then extracts and reports the top result as can1357/oh-my-pi.](https://omp.sh/captures/browser.webp)

omp · browser · duckduckgo

also driveslocalhost dev servers · Electron apps via app.path · existing Chromium via cdp\_urltabs persist across tool calls

native engine

## 100,000+ lines of Rust, doing the work other harnesses shell out for.

Seven crates plus a vendored shell and coreutils, one platform-tagged N-API addon. Search, shell, AST, highlight, PTY, image decode, BPE counting — all in-process on the libuv pool. No fork/exec on the hot path.

platformslinux-x64linux-arm64darwin-x64darwin-arm64win32-x64crates · pi-natives · pi-shell · pi-ast · pi-iso · pi-walker · +vendor

| Module | What it does | Powered by | ~LoC |
| --- | --- | --- | --- |
| shell | Embedded bash · persistent sessions · timeout/abort · process-tree control · custom builtins | brush-shell (vendored) | 31,130 |
| coreutils | ls · find · grep · sort · tail · wc · fd — in-process builtins, no fork, no PATH | uutils (vendored) · grep-searcher | 28,910 |
| minimizer | Trims command output before the model reads it · per-tool detectors · staged filter pipeline | in-tree | 23,880 |
| walker | One traversal fast path — glob · fd · grep candidates · workspace scans · mtime-keyed cache | ignore · globset | 4,910 |
| iso | Workspace isolation shim · apfs · btrfs · zfs · reflink · overlayfs · projfs · rcopy | in-tree PAL | 3,420 |
| ast | ast-grep pattern matching and structural rewrites | ast-grep-core · tree-sitter | 2,370 |
| grep | Regex search · parallel/sequential · glob & type filters · fuzzy find | grep-regex · grep-searcher | 2,140 |
| text | ANSI-aware width · truncation · column slicing · SGR-preserving wrap | unicode-width · segmentation | 1,640 |
| snapcompact | Compaction frames — text in, pixel-font PNG out, deterministic, no model call | embedded BDF · TTF fonts | 1,440 |
| keys | Kitty keyboard protocol with xterm fallback · PHF perfect-hash lookup | phf | 1,330 |
| summary | Tree-sitter structural source summaries with elision controls | tree-sitter · ast-grep-core | 1,250 |
| pty | Native PTY allocation for sudo · ssh interactive prompts | portable-pty | 470 |
| block | Syntactic block resolution behind hashline's.BLK edit ops | tree-sitter | 460 |
| highlight | Syntax highlighting · 11 semantic categories · 30+ aliases | syntect | 400 |
| appearance | Mode 2031 + native macOS dark/light via CoreFoundation FFI | core-foundation | 290 |
| task | Blocking work on libuv thread pool · cancellation · timeout · profiling | tokio · napi | 250 |
| power | macOS power-assertion API for idle/system/display-sleep prevention | IOKit FFI | 200 |
| prof | Circular buffer profiler with folded-stack and SVG flamegraph output | inferno | 170 |
| clipboard | Text copy and image read from system clipboard · no xclip/pbcopy | arboard | 80 |
| tokens | O200k / Cl100k BPE token counting · both tables embedded | tiktoken-rs | 40 |
| sixel | Terminal image rendering · decode PNG · JPEG · WebP · GIF · resize · SIXEL encode | icy\_sixel · image | 30 |
| html | HTML to Markdown with optional content cleaning | html-to-markdown-rs | 30 |

## Four entry points: interactive, one-shot, RPC, and ACP.

Same engine, four wrappers. `omp` runs the TUI. `omp -p` answers a single prompt and exits. The Node SDK embeds the session in your process. `omp --mode rpc` and `omp acp` hand the wheel to another program over stdio.

00 · interactive

### When in doubt, the agent asks.

The TUI is the default surface. Tool calls render as cards, edits preview before they land, and ambiguity routes through the `ask` tool — a structured option picker the agent can call mid-turn. The keyboard handles the rest.

The same prompt cards surface over ACP, so editors get the picker without writing one.

omp · ask

![omp TUI: the ask tool renders an option picker with three choices, a (Recommended) badge on the first, and 'up/down navigate · enter select · esc cancel' footer.](https://omp.sh/captures/ask.webp)

### Embed in Node

`@oh-my-pi/pi-coding-agent`

Node and TypeScript hosts pull the engine in directly. The package exposes `ModelRegistry`, `SessionManager`, `createAgentSession`, and `discoverAuthStorage`; the session emits typed events you subscribe to.

```
import {
  ModelRegistry, SessionManager,
  createAgentSession, discoverAuthStorage,
} from "@oh-my-pi/pi-coding-agent";

const auth = await discoverAuthStorage();
const models = new ModelRegistry(auth);
await models.refresh();

const { session } = await createAgentSession({
  sessionManager: SessionManager.inMemory(),
  authStorage: auth, modelRegistry: models,
});
await session.prompt("list .ts files");
```

### Drive over stdio

`omp --mode rpc`

For non-Node embedders, or when you want process isolation. NDJSON commands in, response and event frames out. `--mode rpc-ui` adds tool cards, selectors, and dialogs as `extension_ui_request` frames the host must answer.

```
$ omp --mode rpc --no-session
> {"id":"r1","type":"prompt","message":"list .ts files"}
< {"id":"r1","type":"response", ...}
> {"id":"r2","type":"set_model","provider":"anthropic","modelId":"sonnet-4.5"}
> {"id":"r3","type":"abort"}
```

03 · acp

### Speak to editors

`omp acp`

The [Agent Client Protocol](https://github.com/zed-industries/agent-client-protocol) over JSON-RPC. When the editor advertises capabilities, tool I/O routes through it and writes are gated by `session/request_permission`.

- `bash` `terminal/create + terminal/output`
- `read` `fs/read_text_file`
- `write` `fs/write_text_file`
- `edit, bash` `session/request_permission`

pick it up

## A harness worth keeping is one you don’t outgrow.

omp is a fork of [Pi](https://github.com/badlogic/pi-mono) by [Mario Zechner](https://github.com/mariozechner), rewritten as a coding-first surface: sessions, subagents, slash commands, extensions — all TypeScript, all MIT, all on [GitHub](https://github.com/can1357/oh-my-pi). Shape it from config, hook it from outside, or read the source when you need to.

macOS · Linux

`curl -fsSL https://omp.sh/install | sh`

primitives

An extension is a TypeScript module. Same tool API, same slash-command registry, same hotkey table, same TUI primitives the built-ins use. Nothing is reserved.

discovery

On first run omp inherits whatever is already on disk: rules, skills, and MCP servers from.claude,.cursor,.windsurf,.gemini,.codex,.cline,.github/copilot, and.vscode. No migration script.

extensibility

Ask omp to write the piece you’re missing, then `/reload-plugins`. Keep it local, ship it in a `marketplace`, or publish it to npm.