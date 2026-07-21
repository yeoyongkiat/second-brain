# Second Brain

A guided setup for an interactive, [Obsidian](https://obsidian.md)-compatible **Second Brain** and AI agent environment, designed by **Yeo Yong Kiat**.

Point your AI coding agent at [`second-brain-initialisation.md`](second-brain-initialisation.md) and it will walk you — conversationally, one stage at a time — through building a personal knowledge vault that an AI agent can navigate efficiently.

The initialisation doc is **harness-agnostic**: it describes *what* to build and lets each agent translate the mechanics into its own conventions. It works with **Codex, Claude Code, Gemini CLI, and OpenCode** — each creates its own native instructions file (`AGENTS.md` / `CLAUDE.md` / `GEMINI.md`) and packages the skills as its own native skills or custom commands.

## What it sets up

- **`SOUL.md`** — the canonical definition of your agent's role, identity, personality, communication style, and boundaries.
- **`USER.md`** — confirmed facts about you, so the agent's help stays relevant.
- **The harness instructions file** (`AGENTS.md`, `CLAUDE.md`, or `GEMINI.md`) — startup context and operating rules the agent reads at the start of every session.
- **`INDEX.md` files** — a root map plus per-folder indexes that let the agent find information through progressive disclosure, without loading the whole vault into context.
- **Default folders** — `people/`, `ideas/`, `projects/`, `meetings/` (all customisable during setup).

### Resulting structure

```text
Second Brain/
├── AGENTS.md   # or CLAUDE.md / GEMINI.md — your harness's instructions file
├── SOUL.md
├── USER.md
├── INDEX.md
├── people/    └── INDEX.md
├── ideas/     └── INDEX.md
├── projects/  └── INDEX.md
└── meetings/  └── INDEX.md
```

## Skills it creates

Each is packaged in your harness's native format and invoked its own way (e.g. `$update` in Codex, `/update` in Claude Code or Gemini CLI).

- **`update`** — refreshes the harness instructions file and all `INDEX.md` files after you add, move, rename, or delete files, without touching your notes.
- **`summarise`** — recursively reads a folder and produces a context-sensitive, Obsidian-linked `SUMMARY.md`, tracking changes via a `.summary-manifest.json` so later runs only re-read what actually changed.

## Prerequisites

- [Obsidian](https://obsidian.md) with the **Terminal** community plugin.
- An AI coding agent — **Codex, Claude Code, Gemini CLI, or OpenCode** — that discovers a project instructions file at session start.

## Getting started

1. Clone this repo (or copy `second-brain-initialisation.md`) into the folder you want as your Second Brain root.
2. Launch your AI agent from that folder.
3. Ask it to follow `second-brain-initialisation.md`, and answer its questions as it guides you through each stage.

> Launch future sessions from the Second Brain root so your harness's instructions file (`AGENTS.md` / `CLAUDE.md` / `GEMINI.md`) is reliably discovered.

## Design principles

- **Progressive disclosure** — load only what the current task needs, conserving context and reducing invented connections.
- **Human-in-the-loop** — the agent explains what it's about to do and waits for confirmation on every choice; it never overwrites existing files without showing you the change first.
- **Facts over assumptions** — nothing uncertain is recorded as fact, and sensitive details are saved only when you explicitly ask.
