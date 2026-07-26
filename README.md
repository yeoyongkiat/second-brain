# Second Brain

A guided setup for an interactive, [Obsidian](https://obsidian.md)-compatible **Second Brain** and AI agent environment, designed by **Yeo Yong Kiat**.

Point your AI coding agent at [`second-brain-initialisation.md`](second-brain-initialisation.md) and it walks you — conversationally, one step at a time — through building a personal knowledge vault that an AI agent can navigate efficiently.

The initialisation doc is **harness-agnostic**: it describes *what* to build and lets each agent translate the mechanics into its own conventions. It works with **Codex, Claude Code, Gemini CLI, and OpenCode** — each writes its own native guidance file (`AGENTS.md` / `CLAUDE.md` / `GEMINI.md`) and installs the skills as its own native skills or custom commands. A **Step 0.5** has the agent detect its harness and set those conventions before building anything.

## What it sets up

Working through the installation (Steps 0–7):

- **`SOUL.md`** — your agent's identity: name, role, organisation, personality, and behavioural principles. *(Step 1)*
- **`USER.md`** — a profile of you: role, work, goals, working preferences, and boundaries, so the agent's help stays relevant. *(Step 2)*
- **`brain/`** — your "working brain" of durable, everyday knowledge, seeded with **`my-todos.md`** (a Markdown checklist the agent maintains) and its own `INDEX.md`. *(Step 3)*
- **`INDEX.md` tree** — a root map plus a per-folder index, so the agent finds information through progressive disclosure instead of loading the whole vault into context. *(Step 4)*
- **The harness guidance file** (`CLAUDE.md` / `GEMINI.md` / `AGENTS.md`) — startup context and operating rules the agent reads at the start of every session, generated from everything above. *(Step 7)*

### Resulting structure

```text
Second Brain/
├── CLAUDE.md          # or AGENTS.md / GEMINI.md — your harness's guidance file
├── SOUL.md            # agent identity
├── USER.md            # your profile
├── INDEX.md           # root navigation map
├── brain/
│   ├── my-todos.md    # the agent-maintained checklist
│   └── INDEX.md
└── <your knowledge folders>/
    └── INDEX.md       # one index per knowledge folder
```

## Skills it installs

Each is authored once as a portable `SKILL.md` and installed in your harness's native format, invoked its own way (e.g. `/update` in Claude Code, Gemini CLI, or OpenCode; `$update` in Codex).

- **`update`** — re-inventories the vault and reconciles the entire `INDEX.md` tree after you add, move, rename, or delete files, without touching your notes. *(Step 5)*
- **`write-noms`** — converts transcripts, meeting notes, or recordings into structured **Notes of Meeting**: reported speech, clear attribution, prose notes, a contents list, and an action table. Installed from the bundled `write-noms.md`. *(Step 6)*

## Prerequisites

- [Obsidian](https://obsidian.md) (with a terminal plugin if you want to run the agent from inside Obsidian).
- An AI coding agent — **Codex, Claude Code, Gemini CLI, or OpenCode** — that discovers a project guidance file at session start.
- *Optional:* Python via Anaconda (checked in Step 0). It is needed only for later validation and automation features; the installation continues without it.

## Getting started

1. Clone this repo (or copy `second-brain-initialisation.md`) into the folder you want as your Second Brain root.
2. Launch your AI agent from that folder.
3. Ask it to follow `second-brain-initialisation.md`, and answer its questions as it guides you through each step.

> Launch future sessions from the Second Brain root so your harness's guidance file (`CLAUDE.md` / `AGENTS.md` / `GEMINI.md`) is reliably discovered.

## Design principles

- **Progressive disclosure** — the agent loads only what the task needs via the `INDEX.md` tree, conserving context and reducing invented connections.
- **Human-in-the-loop** — the agent explains each step and waits for confirmation; it never overwrites an existing file without showing you the change first.
- **Harness-agnostic** — one procedure, native output: each tool produces its own guidance file and skill format.
- **Facts over assumptions** — nothing uncertain is recorded as fact, and sensitive details are saved only when you explicitly ask.
