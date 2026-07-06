---
title: How to run a local coding agent with Gemma 4 and Pi
source: https://patloeber.com/gemma-4-pi-agent/
author:
published: 2026-04-27
created: 2026-06-17
description: Set up Gemma 4 running in LM Studio, connected to Pi as the terminal agent
tags:
  - clippings
  - ArtificialIntelligence
---
![pi-agent](https://github.com/user-attachments/assets/b9db418f-6633-4b46-a363-bf10a288928e)

I've been playing around with running coding agents fully locally. The setup I landed on is:

**LM Studio + Pi agent + Gemma 4 26B A4B (Q4\_K\_M)**

Gemma 4 running in LM Studio, connected to Pi as the terminal agent. It works surprisingly well, and this post walks through how to set it up.

Here's what we'll cover:

1. Install LM Studio
2. Download Gemma 4
3. Start a local server
4. Configure context size
5. Install Pi
6. Connect Pi to your local model
7. Add skills and extensions

## 1) Install LM Studio

You need something to serve the model locally. I'm using [LM Studio](https://lmstudio.ai/) here — it's a desktop app that handles model downloads, quantization, and exposes a local OpenAI-compatible API server. Download it from [lmstudio.ai](https://lmstudio.ai/) (macOS, Windows, Linux).

[Ollama](https://ollama.com/) and [llama-server](https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md) (part of llama.cpp) work just as well if you prefer a CLI-first workflow. All three expose an OpenAI-compatible endpoint, so Pi doesn't care which one you use.

The rest of this guide uses LM Studio, but the Pi configuration works with any of them — just swap out the server configuration.

## 2) Download Gemma 4

[Gemma 4](https://ai.google.dev/gemma/docs/core) is Google's latest open-weight model family, released under the [Apache 2.0 license](https://ai.google.dev/gemma/apache_2). Compared to earlier Gemma versions, it's a real step change for coding and agentic use cases — it now has native function calling, system prompt support, and thinking modes, which makes it a genuinely good model for local coding agents. The family includes four sizes:

| Model Size | Architecture Type | Context Length |
| --- | --- | --- |
| Gemma 4 E2B | Dense | 128K tokens |
| Gemma 4 E4B | Dense | 128K tokens |
| Gemma 4 26B A4B | Mixture of Experts (MoE) | 256K tokens |
| Gemma 4 31B | Dense | 256K tokens |

**My recommendation: go with the 26B A4B.** It's a Mixture-of-Experts model, which means it has 26B total parameters but only activates 4B per token. In practice, you get the quality of a much larger model with inference speeds closer to a small one. It handles text, image understanding, function calling, and thinking modes — which is exactly what you want for a coding agent.

That said, the E4B is surprisingly capable for its size. If you're short on VRAM, it's worth trying — but it does need more guidance and more specific prompts to get good results.

To download it, open LM Studio, search for `gemma-4-26b-a4b`, and download a quantized GGUF version (e.g., Q4\_K\_M). Choose the quantization based on your available VRAM:

| Quantization | Download Size | Quality |
| --- | --- | --- |
| Q4\_K\_M | 18 GB | Good balance |
| Q6\_K | 24 GB | Higher quality |
| Q8\_0 | 28 GB | Near-original |

![gemma4](https://github.com/user-attachments/assets/2e0495a4-8568-42a5-88ad-2f1ded8ea774)

> **Note:** Even though the model only activates 4B parameters per token, all 26B parameters must be loaded into memory for fast routing. That's why VRAM requirements are closer to a dense 26B model.

If you're on a Mac, you can also check out the MLX versions of Gemma 4. MLX is natively optimized for Apple Silicon and can be faster than the GGUF format on M-series chips.

## 3) Start the server in LM Studio

Once the model is downloaded:

1. Go to the **Developer** tab in LM Studio
2. Select your downloaded Gemma 4 model
3. Click **Start Server**

![server1](https://github.com/user-attachments/assets/b61974b1-5e03-4229-8060-70407b651a81)

The server runs at `http://localhost:1234` by default and exposes an OpenAI-compatible API.

You can verify it's running:

```bash
curl http://localhost:1234/v1/models
```

## 4) Configure context size and GPU offload

Before you start working, check the context size and GPU offload settings under **Model Settings** in the Developer tab.

Context size directly impacts VRAM usage. The model supports up to 256K tokens, but you probably don't need all of that for coding tasks. More context = more VRAM on top of the base model weights.

| Use Case | Context Size | Additional VRAM (approx.) |
| --- | --- | --- |
| Small edits, single files | 16K | ~1 GB |
| Standard coding sessions | 64K | ~4 GB |
| Multi-file refactors | 128K | ~8 GB |
| Full repo context | 256K | ~16 GB |

I'd recommend going with **128K** if your VRAM allows it. Coding agents tend to accumulate a lot of context over a session — file contents, tool outputs, conversation history — and running out of context mid-task is annoying.

Pi has built-in session management that helps here. `/compact` summarizes older messages to free up context. `/new` starts a fresh session. `/tree` lets you navigate the session history and jump back to any previous point. `/fork` creates a new session from a past message, which is great when you want to branch off in a different direction without losing your history.

Also check the **GPU Offload** setting. This controls how many layers are loaded onto the GPU vs. kept in system RAM. More layers on GPU = faster inference, but requires more VRAM. If your GPU can't fit the entire model, LM Studio will split it between GPU and CPU — it'll still work, just slower for the CPU portion. I keep this at maximum (30 for the 26B A4B).

If you're running into out-of-memory issues, lower the context size first.

![server2](https://github.com/user-attachments/assets/e96ce539-4c44-4500-8fad-ffb68dd2364d)

## 5) Install Pi

[Pi](https://pi.dev/) is a minimal terminal coding harness by [Mario Zechner](https://github.com/badlogic). The core is deliberately small — the model gets four tools (`read`, `write`, `edit`, `bash`) and that's it.

You can customize it with extensions, skills, prompt templates, and themes. It's also token efficient and the system prompt is small, so you can do actual context engineering. That matters a lot when you're running a local model.

```bash
npm install -g @mariozechner/pi-coding-agent
```

## 6) Configure the local model in Pi

Create (or edit) the file `~/.pi/agent/models.json` to point Pi at your local LM Studio server:

```json
{
  "providers": {
    "lmstudio": {
      "baseUrl": "http://localhost:1234/v1",
      "api": "openai-completions",
      "apiKey": "lm-studio",
      "models": [
        {
          "id": "google/gemma-4-26b-a4b",
          "input": [
            "text",
            "image"
          ]
        }
      ]
    }
  }
}
```

> **Note:** Set the model `id` to match the exact model name shown in LM Studio's server tab.

Then launch Pi and select your local model:

```bash
pi
# Use /model to switch to your local LM Studio model
```

That's it. You now have a local coding agent running entirely on your machine.

![pi-start](https://github.com/user-attachments/assets/be9cb5b6-e745-45f7-8e8c-415b9ed4015e)

## 7) Skills

Skills are on-demand capability packages that extend what Pi can do. They follow the [Agent Skills standard](https://agentskills.io/) and are just Markdown files with instructions.

Install community skills via git:

```bash
# User-level (available in all projects)
git clone https://github.com/badlogic/pi-skills ~/.pi/agent/skills/pi-skills

# Or project-level
git clone https://github.com/badlogic/pi-skills .pi/skills/pi-skills
```

Some skills I find useful:

- [liteparse](https://github.com/run-llama/llamaparse-agent-skills/blob/main/skills/liteparse/SKILL.md): Fast local document parsing (PDFs, DOCX, PPTX and more). Especially handy with Gemma since it can only understand images — liteparse converts documents to a format the model can actually work with.
- [frontend-slides](https://github.com/zarazhangrui/frontend-slides): Create presentation slides in HTML.
- [pi-skills](https://github.com/badlogic/pi-skills): A collection of skills for pi-coding-agent.
- [grill-me](https://github.com/mattpocock/skills/blob/main/grill-me/SKILL.md): Get grilled to work out and iterate on an idea.
- [gemini-skills](https://github.com/google-gemini/gemini-skills): Skills for the Gemini API, SDK and model interactions.

Invoke a skill during a session with `/skill:name`, or let the agent discover and use them automatically.

## 8) Extensions

Extensions are TypeScript modules that go deeper — custom tools, commands, UI components, permission gates, even sub-agents.

One thing to know: **Pi runs YOLO by default.** It will execute bash commands without asking. That's fast but can be risky, especially with a local model that might hallucinate a destructive command. The [permission-gate](https://github.com/badlogic/pi-mono/blob/HEAD/packages/coding-agent/examples/extensions/permission-gate.ts) extension helps — it prompts you for confirmation before running potentially dangerous commands. It's not a full security sandbox though. If you want something more robust, check out [cco](https://github.com/nikvdp/cco) (runs commands in a container) or the [sandbox extension](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent/examples/extensions/sandbox).

```bash
# Load an extension with --extension flag
pi --extension examples/extensions/permission-gate.ts

# Or copy to extensions directory for auto-discovery
cp permission-gate.ts ~/.pi/agent/extensions/
```

That's the full setup. Once it all clicks, it's a surprisingly capable workflow — and it's nice knowing everything runs on your own hardware. Happy local building!

## Resources

- [Gemma 4 documentation](https://ai.google.dev/gemma/docs/core)
- [Gemma 4 model card](https://ai.google.dev/gemma/docs/core/model_card_4)
- [A Visual Guide to Gemma 4](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-gemma-4)
- [LM Studio](https://lmstudio.ai/)
- [Pi coding agent](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent)
- [Pi skills docs](https://github.com/badlogic/pi-mono/blob/HEAD/packages/coding-agent/docs/skills.md)
- [Pi extensions docs](https://github.com/badlogic/pi-mono/blob/HEAD/packages/coding-agent/docs/extensions.md)

## Acknowledgements

Thanks to my colleague [Ian](https://x.com/IanBallantyne) who helped me find a great setup. He also created a similar [video guide](https://x.com/googlegemma/status/2047406475631624285) showing how to set up Gemma 4 with LM Studio & OpenCode.