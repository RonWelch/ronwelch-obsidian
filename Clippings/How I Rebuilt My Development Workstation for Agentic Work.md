---
title: How I Rebuilt My Development Workstation for Agentic Work
source: https://spin.atomicobject.com/rebuilding-development-workstation/?utm_source=tldrnewsletter
author:
  - "[[Brian May]]"
published: 2026-08-08
created: 2026-08-10
description: My previous workstation optimized for focusing on one task at a time. My new setup optimizes for coordinating multiple parallel threads.
tags:
  - clippings
  - Programming
  - Unix
  - llms
---
### Article summary

‘For most of my eight-year career, my development environment was never really designed. It accumulated.

Every new project, language, and workflow added another alias, plugin, editor setting, or command-line utility. Some of those additions became indispensable. Others solved a problem I had once and then lingered in my configuration for years. Eventually, parts of the setup referenced tools that were unused, unsupported, or no longer even installed.

It was time for a reset.

The motivation was not just cleaning up old configuration. The way I develop software is changing. My previous workstation optimized for finding the best tool for each job and focusing on one task at a time. My new setup optimizes for coordinating multiple parallel threads of work—often with one or more agents working in each thread.

Managing several agents across different projects is already mentally expensive. Spreading them across separate applications, windows, editors, and chat interfaces adds even more overhead. I wanted a shared environment where the project, branch, shell, editor, agent, and review workflow could stay together.

The result is a workstation that is terminal-native, agentic-first, and built around composable tools.

Agentic-first does not mean agent-only. I still write code, investigate problems, and make decisions myself. It means the environment treats an agent as a normal participant in the development workflow rather than something bolted onto the side of it.

## The Terminal as a Shared Foundation

The terminal is the common layer beneath the entire setup.

That does not mean every development task must happen in a terminal. Browsers, debuggers, and specialized tools still have their place. The terminal is useful here because it gives both me and an agent access to the same set of primitives: files, processes, Git repositories, search tools, scripts, and sessions.

The TL;DR of my stack is WezTerm, zsh, tmux, Neovim, and Codex. I also rely heavily on additional tools like ripgrep, fzf, LazyGit, and RevDiff.

## WezTerm

I am not completely sold on [WezTerm](https://wezterm.org/index.html) as my terminal foundation yet.

My terminal is intentionally barebones: a font, a color scheme, some padding, and a small visual distinction for inactive panes. That is about it. Coming from the feature rich of world of Warp, this feels like a hard reset.

```lua
local wezterm = require("wezterm")
local config = wezterm.config_builder()

config.color_scheme = "SpaceGray Eighties"
config.font = wezterm.font("Hack Nerd Font", { weight = "DemiBold" })
config.font_size = 18.0

config.inactive_pane_hsb = {
  saturation = 0.0,
  brightness = 0.5,
}

return config
```

What I do like is that WezTerm is configurable with Lua scripts. If I eventually want the terminal itself to react to projects, sessions, or other state, I have a real scripting language available instead of endless settings menus and unknown configuration formats.

For now, WezTerm mostly stays out of the way. The rest of the stack is responsible for organizing what happens inside it.

## Cleaning Up zsh

I have used zsh and [Oh My Zsh](https://ohmyz.sh/) for a long time, so this part of the rebuild was more subtraction than experimentation.

Years of shell configuration had left behind commented examples, duplicated initialization, environment-specific paths, and plugins I no longer needed. The goal was to clean that up while keeping the parts of the shell I actually use.

The remaining setup includes vim keybindings, a small custom prompt, shared history, syntax highlighting, fzf-powered history search, tab completion for several tools, and a handful of functions and aliases.

I also cache the output of shell initialization commands that would otherwise run every time a new shell starts:

```bash
Bash
```

```bash
#.zshrc
plugins=(evalcache git)

typeset -U path PATH

_evalcache direnv hook zsh
_evalcache ~/.local/bin/mise activate zsh
_evalcache zoxide init zsh
```

Shell startup time is not the most important performance metric on my workstation, but tmux and parallel workflows make me create shells more frequently. Small delays become much more noticeable when repeated throughout the day.

[ripgrep](https://github.com/burntsushi/ripgrep) and [fzf](https://github.com/junegunn/fzf) also fit the broader theme of the setup. They are fast, focused tools that work independently but become more valuable when composed with other tools.

## tmux is Where Work Lives

[tmux](https://github.com/tmux/tmux/wiki) is the layer that turns my terminal from a window containing a shell into a workspace.

Each substantial unit of work can have its own session. I use session to group processes, working directory, editor, and agent together. I can move between projects or work streams without reconstructing the state of each one or trying to remember which terminal tab belongs to which branch.

My tmux usage is still relatively modest. I use sessions, windows, panes, keyboard navigation, copy mode, and a few visual indicators. I am not attempting to turn it into an operating system.

Most of the configuration is made up of sensible defaults that work for me:

```
# Bind prefix to Control-Space
unbind C-b
set -g prefix C-Space

# Visually coherent pane splitting
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"

# Navigate panes with vim-like movements
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
```

New panes inherit the current working directory, and the navigation uses the same directional keys I already use in Vim. Pane borders display the window name, while inactive panes are visually muted.

The higher-leverage features are often even simpler. Using my tmux prefix followed by \`w\` gives me a tree of the sessions and windows I currently have open. That has become one of the main ways I move between parallel threads of work. Instead of locating the right application window and then reconstructing its purpose, I navigate directly to a named session.

```
# Session navigation
bind w choose-tree -Zw
```

I would not recommend that everyone immediately jump into tmux. It has a steep learning curve, and many of its concepts and defaults are unfamiliar until the mental model clicks. Even with my deliberately minimal usage, though, I am finding a lot of power in it.

The important feature is not that tmux lets me split a terminal. Any modern terminal can do that. The important feature is that sessions are durable, addressable, and controllable from the command line. That makes them useful building blocks for automation.

## A Worktree per Thread

Running several agents in parallel creates a Git problem before it creates an AI problem.

Git worktrees are already a familiar solution for many developers working across multiple branches at once. They allow each branch to have its own working directory without requiring a separate clone of the repository.

I keep a dedicated worktree directory for each project. It is usually just called \`worktrees\` and sits next to the main project repository:

```
project/
worktrees/
  feature-one/
  fix-something/
  experiment/
```

This gives every thread of work an isolated branch and filesystem while keeping those working directories in a predictable place.

This also means it is more important than ever to have a local development environment that can run multiple instances without collisions. Ports, databases, container names, queues, and other shared resources all become potential sources of interference once several branches are running at the same time. But that is a topic for another blog post.

I wrote a custom utility that orchestrates Git worktrees, tmux, and Codex in the way that works for me. It gives me a quick way to turn a branch into an isolated workspace with a corresponding tmux session and an agent ready to work inside it.

The implementation is specific to my preferences, but the larger idea is more important than the script itself:

```
branch
  └── Git worktree
        └── tmux session
              ├── shell
              └── agent
```

This small amount of orchestration removes enough friction that creating a fresh workspace becomes the default. There is little incentive to let unrelated work share a branch, directory, or terminal context.

## Moving from Vim to Neovim

I have been a Vim user for a long time, but this is my first real experiment with [Neovim](https://neovim.io/).

The transition to Neovim wasn’t too dramatic considering it is completely compatible with vanilla vim. But it gave me an opportunity to reconsider my assumptions about what an editor should and shouldn’t provide. I am using Lua for configuration and include the following plugins:

- [Telescope](https://github.com/nvim-telescope/telescope.nvim) for finding files, buffers, and text
- [Oil](https://github.com/stevearc/oil.nvim) for navigating and editing directories
- [Treesitter](https://github.com/nvim-treesitter/nvim-treesitter) for parsing, highlighting, and indentation
- the built-in LSP client with a few third-party language servers
- text completion through [blink.cmp](https://github.com/saghen/blink.cmp)
- Gitsigns, Fugitive, Diffview, and LazyGit integration
- shared directional navigation between Neovim and tmux

I still occasionally miss IDE interactions like the ability to preview all references to a symbol or add breakpoints inline. I’m sure I will find plugins and mappings that close more of those gaps over time. I am also trying not to re-create an entire IDE immediately. Part of this experiment is discovering which IDE features I genuinely depend on and which ones were simply available.

Neovim’s biggest advantage in this setup is not that it is categorically better than an IDE. It is that the editor can live inside the same tmux session as the shell, running processes, and agent responsible for that task while still providing some quality of life improvements over vanilla vim.

## Different Git Tools for Different Kinds of Review

I still use command-line Git for most Git operations. It is fast, predictable, scriptable, and deeply embedded in my muscle memory. In most cases, the CLI is still the clearest way for me to express exactly what I want to do.

That does not mean every Git interaction is best represented as a sequence of commands.

[lazygit](https://lazygit.dev/) is useful when I want a visual overview of repository state or need to perform an interactive Git operation. It is particularly good for human-driven work where seeing branches, commits, files, and hunks together is more useful than constructing the equivalent CLI command.

[revdiff](https://github.com/umputun/revdiff) is something I discovered recently and am still trying out. I have been using it to review agent-produced changesets and provide feedback across an entire diff. Instead of stopping at the first issue, sending a comment, waiting for a change, and repeating the process, I can review the complete result and return an aggregated set of feedback.

In practice, the tools have settled into different roles:

- lazygit helps with human review and visual git operations.
- revdiff helps me review an agent’s work and communicate what should change.
- CLI git remains the default for most direct repository operations.

There is lots of overlap between these tools, along with Neovim integrations such as Diffview and Gitsigns. I am fine with that. The goal is not to choose one Git interface and force every Git-shaped task through it. The goal is to have a small number of tools with clear roles.

## Optimizing for Coordination

My previous workstation optimized for doing one thing at a time with the best available tool.

That made sense when most work had a single foreground thread. In an agentic workflow, the scarce resource shifts. Producing another implementation is relatively cheap. Keeping several implementations isolated, understanding their status, reviewing their output, and giving each one useful feedback is harder.

The new setup optimizes more for that coordination problem.

A branch can become an isolated worktree. The worktree can become a tmux session. The session contains the human and agent tools needed for the task. The output can be inspected with normal Git tooling and reviewed with a tool designed for structured agent feedback.

Because the pieces expose command-line interfaces and operate on ordinary files and processes, they are available to both me and the agents. I do not need a separate automation story for each graphical application.

The setup is also intentionally unfinished. I may decide WezTerm is not the right terminal. My Neovim configuration will certainly evolve. I expect my tmux usage to change as I learn which patterns remain useful under real workloads. I will definitely add more custom utilities as I find new opportunities to orchestrate the tools around repetitive parts of the workflow.

## Redesigning a Workstation

Deliberately redesigning my workstation has also given me a greater appreciation for what these tools are doing for me. When configuration grows gradually over years, it is easy to stop seeing the individual decisions inside it. Useful behavior becomes invisible, obsolete behavior stays in place, and muscle memory hides how much the environment has accumulated.

Rebuilding the setup forced me to examine those decisions again. I had to decide which conveniences were genuinely valuable, which tools still matched the way I work, and which pieces existed only because they had always been there.

The goal is not a perfect or permanent collection of tools. It is a workstation whose structure matches the way I now work: several independent threads, a mixture of human and agent effort, and a need to move between them without losing the plot.