---
title: "Impeccable: Design skills for AI harnesses"
source: https://impeccable.style/#faq
author:
published:
created: 2026-04-08
description: 1 skill, 20 commands, and curated anti-patterns for impeccable frontend design. Works with Cursor, Claude Code, Gemini CLI, and Codex CLI.
tags:
  - clippings
---
[[Anthropic]]'s original frontend-design skill laid the foundation. Impeccable builds on it with curated patterns and anti-patterns across typography, color, layout, motion, and more.

Don't
- Use overused fonts—Inter, Roboto, Arial, Open Sans, system defaults
- Use monospace typography as lazy shorthand for "technical/developer" vibes
- Put large icons with rounded corners above every heading—they rarely add value and make sites look templated

Do
- Use a modular type scale with fluid sizing (clamp)
- Vary font weights and sizes to create clear visual hierarchy

Missing something? [Suggest a pattern →](https://github.com/pbakaus/impeccable/issues/new?labels=pattern&title=Pattern%20suggestion%3A%20)

One comprehensive skill with deep expertise, plus 20 commands that form the language of design.

Diagnostic

Quality

Adaptation

Enhancement

Intensity

System

install

$ `npx skills add pbakaus/impeccable`

Works with Cursor, Claude Code, Gemini CLI, Codex CLI, and more. Auto-detects your AI harness.

Other install methods

Claude Code plugin

$ `/plugin marketplace add pbakaus/impeccable`

Then open `/plugin` to install from Discover tab

Manual download Contains all provider directories. Extract to your project root.

Avoids conflicts with built-in commands or other skill packs

2

### Stay up to date

New skills, pattern updates, and design tips.

<iframe src="https://impeccablestyle.substack.com/embed" width="100%" height="150" frameborder="0"></iframe>

Keep the [command cheatsheet](https://impeccable.style/cheatsheet) handy for quick reference. To update skills, run `npx skills update`.

- New provider: **Trae** (China + International)
- `/critique` now scores against Nielsen's 10 heuristics, tests with persona archetypes, and assesses cognitive load
- `/audit` now scores 5 dimensions with P0-P3 severity ratings and structured action plans
- Improved skill descriptions for better agent auto-discovery
- Fixed invalid YAML frontmatter that broke GitHub preview and Codex loading ([#67](https://github.com/pbakaus/impeccable/issues/67))
- Codex CLI now uses correct `$` prefix for command references

View older releases

- `/typeset` now recommends fixed type scales for app UIs, reserving fluid typography for marketing/content pages

- 3 new skills: `/typeset` (fix typography), `/arrange` (fix layout & spacing), `/overdrive` (technically extraordinary effects, beta)
- Skills now auto-gather design context via `.impeccable.md` — run `/teach-impeccable` once, all skills benefit
- Deep linking to commands (`#cmd-overdrive`, etc.)

- Added OpenCode provider support
- Added Pi provider support
- Recategorized `/onboard` as an enhancement command

- Added Kiro support (`.kiro/skills/`)
- Restored prefix toggle — download `i-` prefixed bundles to avoid naming conflicts
- Audit and critique skills only suggest real, installed commands

- Unified skills architecture — commands are now skills with `user-invocable: true`
- Added VS Code Copilot and Google Antigravity support (`.agents/skills/`)
- New install flow: `npx skills add` as primary, universal ZIP as fallback
- Added universal ZIP containing all 5 provider directories
- Renamed `/simplify` to `/distill` to avoid Claude Code conflict

- Initial release with enhanced frontend-design skill
- 17 design commands: /polish, /audit, /distill, /bolder, and more
- Support for Cursor, Claude Code, Gemini CLI, and Codex CLI
- Interactive command cheatsheet

Where do I put the downloaded files?

The easiest way is `npx skills add pbakaus/impeccable` — it auto-detects your AI harness and places files correctly.

If you downloaded the **universal ZIP**, extract it to your **project root** (same level as your `package.json` or `src/` folder). It creates hidden folders for each supported tool: `.cursor/`, `.claude/`, `.gemini/`, `.codex/`, and `.agents/`.

Project-level installation takes precedence and lets you version control your skills.

How do I update to the latest version?

- **All tools:** `npx skills update` (or `npx skills check` to see what's new first)
- **Claude Code plugin:** Open `/plugin`, go to the Discover tab, and update from there
- **Manual ZIP:** Download the latest ZIP from above and extract to your project root, overwriting existing files

Your `.impeccable.md` design context file (created by `/teach-impeccable`) is never overwritten — your project context is preserved across updates.

Commands or skills aren't appearing. What do I do?

**For commands:** Type `/` in your AI harness and look for commands like `/audit`, `/polish`, etc. If they don't appear, double-check the files are in the correct location.

**For skills:** Skills are applied automatically when relevant. To verify, explicitly mention "use the frontend-design skill" in your prompt—this forces the AI to acknowledge and apply it.

**Tool-specific setup:**

- **Cursor:** Requires Nightly channel + Agent Skills enabled in Settings → Rules
- **Gemini CLI:** Requires `@google/gemini-cli@preview` + Skills enabled via `/settings`

I'm new to AI harnesses. Where do I start?

Skills and commands are intermediate features. If you're just getting started, learn the basics first:

- **[[Claude Code]]:** [Official Documentation](https://docs.anthropic.com/en/docs/claude-code)
- **[[Cursor]]:** [Cursor Docs](https://docs.cursor.com/)
- **[[Gemini CLI]]:** [Gemini CLI Docs](https://geminicli.com/docs/)
- **[[Codex CLI]]:** [Codex GitHub](https://github.com/openai/codex)

Once you're comfortable with basic prompting and code generation, come back and give Impeccable a try.

## Work with me

I help teams navigate AI transformation: upleveling developers, building AI-native workflows, and shipping products. Deep expertise in developer experience, design systems, and full-stack architecture.