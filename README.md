[简体中文](README.zh-CN.md) | **English**

# A+Prompt · Dual-Engine Image Prompt Optimizer

Turns one image-generation prompt (or an image-editing request) into **two** ready-to-paste production prompts:

- **Nano Banana 2 / Pro edition** — written to Google DeepMind's official Prompt Guide
- **GPT Image 2 edition** — written to OpenAI's official Image Prompting guide

Same target image, each written the way its own model wants to be told. Works in any agent environment that supports skills or custom instructions (Claude, ChatGPT, DSH, …).

## The problem it solves

Most people write image prompts by spreading an element checklist evenly — one line for the subject, one for the scene, one for style, one for lighting — so every prompt ends up the same shape and none of them is good.

This skill forces two things:

1. **Rank before you write.** Every information dimension is graded first (A = expand, B = one line, C = omit). Only what the user named, mentioned first, or that dominates the frame gets expanded; everything else becomes a single phrase or disappears entirely.
2. **Write to each vendor's actual spec.** Nano Banana Pro reasons about composition and lighting before it generates, so it wants an art director's verbal brief. GPT Image 2 runs on OpenAI's reasoning stack, so it wants layered structure plus direct commands. One picture, two registers.

It also ships an **anti-slop pass**: hollow adjectives that don't render (stunning, cinematic, masterpiece, high quality) get replaced with concrete visual parameters (overcast soft light, 2.39:1 anamorphic widescreen, ISO 400 film grain).

## Features

- Covers four task shapes: text-to-image, image editing, generation with reference images, and multi-image composite editing
- Separate generation and editing playbooks; editing is forced into "lock first, then change" and "one change at a time"
- Hard length cap: each prompt stays under 2000 characters, trimmed C-grade → B-grade dimensions first
- Deliberately minimal edit lock list: only the edited subject's identity traits plus the elements directly tied to the change
- Built-in spec cheat sheet for both models (reference-image limits, resolutions, transparency support)

## Install

Drop this repository into your skills directory. With DSH:

```bash
git clone https://github.com/jeathan/aprompt-skill.git
# then copy it into DSH's skills directory
cp -r aprompt-skill ~/.dsh/skills/aprompt
```

Windows PowerShell:

```powershell
git clone https://github.com/jeathan/aprompt-skill.git
Copy-Item -Recurse .\aprompt-skill "$env:USERPROFILE\.dsh\skills\aprompt"
```

Claude Code users can place it at `~/.claude/skills/aprompt/`.

> Name the directory `aprompt` so it matches the `name` field in `SKILL.md`.

## Usage

Paste a prompt in chat and ask for it to be optimized, type `/aprompt`, or attach an image together with your edit request.

Output is always structured as:

1. `Nano Banana 2 / Pro edition` — code block
2. `GPT Image 2 edition` — code block
3. 2–4 sentences: what changed and why, how the ranking was decided, and why the two versions differ

## Repository layout

```
.
├── SKILL.md                        # main entry: workflow, both playbooks, anti-slop, output shape
├── references/
│   ├── nano-banana-2-pro.md        # Google DeepMind guide + fal.ai notes (offline reference)
│   └── gpt-image-2.md              # OpenAI Image Prompting guide notes (offline reference)
├── README.md                       # English (this file)
├── README.zh-CN.md                 # 简体中文
└── LICENSE
```

The two files under `references/` are offline references the agent reads while working — no network access needed.

## Sources & acknowledgements

The reference files are distilled from these public sources:

- Google DeepMind — Gemini Image Prompt Guide
- Google blog — Start building with Nano Banana 2 Lite (2026-06)
- fal.ai — Nano Banana Pro Prompting Guide (John Ozuysal, 2026-06)
- OpenAI — Image Prompting guide (platform.openai.com/docs/guides/image-prompting)
- OpenAI Cookbook — image generation best practices (2026-04)

Model specs and official guidance change as vendors ship updates; this repository reflects the state at the time of writing.

> Note: `SKILL.md` and the two `references/` files are written in Chinese — that is the language the skill operates in and the language the target models are prompted in. This README is the English entry point to the project.

## License

MIT
