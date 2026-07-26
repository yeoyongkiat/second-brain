---
name: update
description: Use when the user invokes /update or asks to refresh, rebuild, reconcile, or audit the vault's INDEX.md navigation tree. Performs a complete filesystem reconciliation of every index (folder indexes bottom-up, root last) and refreshes the root guidance file (CLAUDE.md / AGENTS.md / GEMINI.md) to match — or, on first run when no guidance file exists, creates it from the GUIDE.md template for the harness in use. Adds new entries, removes deleted ones, preserves hand-written guidance, and verifies coverage. Touches only index and guidance files, never notes.
---

# /update — Reconcile the Vault Index Tree & Guidance File

The vault is navigated through a tree of `INDEX.md` files (root + one per knowledge folder), and the agent boots from a root guidance file (`CLAUDE.md` here; `AGENTS.md` or `GEMINI.md` on other harnesses). Both drift out of date as notes are created, edited, renamed, moved, or deleted. `/update` reconciles the **entire** index tree and refreshes the guidance file so navigation and startup context stay reliable — even after many sessions without running.

**Trigger:** the user types `/update`, or asks to refresh / rebuild / reconcile / audit / fix the vault indexes or guidance file.

## This is always a full reconciliation

Do not rely on remembered changes, timestamps, or what you think changed this session. Re-inventory the filesystem from scratch every time. That is what makes `/update` trustworthy after long gaps.

## First run: no guidance file yet

If the vault has **no** guidance file for the current harness (no `CLAUDE.md` / `AGENTS.md` / `GEMINI.md`), do not just refresh — **create** it from the `GUIDE.md` template:

1. **Detect the harness** and pick its native guidance filename: Claude Code → `CLAUDE.md`; Gemini CLI → `GEMINI.md`; Codex / OpenCode → `AGENTS.md`. If undetectable, default to `AGENTS.md` and say so.
2. **Read `GUIDE.md`** (the vault root's harness-agnostic template) as the structure and rule set for the new file.
3. **Resolve every `{{PLACEHOLDER}}`** against the vault's actual contents — `SOUL.md`, `USER.md`, the working brain, the `INDEX.md` tree, and the installed skills. Do **not** invent missing identity details: if a referenced file (e.g. `SOUL.md`, `USER.md`, `brain/`) does not exist, adapt the template to what the vault actually contains rather than inventing it, and note the absence. Ask the user only when a genuinely user-owned fact (like an agent name) is required and cannot be derived.
4. **Substitute harness conventions** — skills directory (`.claude/skills/`, `.agents/skills/`, etc.) and invocation prefix (`/name` vs `$name`) — to match the detected harness.
5. **Write the resolved content** to the harness's guidance filename and remove the installation-template notice from the generated file. Leave `GUIDE.md` itself in place as the portable template.
6. Then continue with the normal reconciliation below. On every subsequent run the guidance file already exists, so step 7 *refreshes* it instead of recreating it.

## Procedure

1. **Inventory the filesystem.** Walk the vault and list every user-knowledge file and folder. Exclude application internals from item-by-item indexing: `.obsidian/`, `.git/`, `.claude/` (and other harness config/skill implementation), caches, and generated dependencies. These may be mentioned as system directories in the root index, but are not knowledge-indexed.
2. **Read every existing `INDEX.md`** and the root guidance file. Note the hand-written guidance and folder-specific rules in each so they can be preserved.
3. **Identify gaps.** Find user-knowledge folders with no `INDEX.md`, and index/guidance entries that no longer match anything on disk.
4. **Inspect contents where needed.** When a filename does not make a file's purpose clear, open the note (frontmatter + opening lines) to write an accurate one-line description.
5. **Rebuild folder indexes bottom-up.** Create or update folder-level `INDEX.md` files from the deepest folders upward. Each must describe the folder's purpose, summarize and link to its files and immediate subfolders, point to nested indexes where applicable, preserve useful hand-written notes, and state when it must be updated.
6. **Update the root `INDEX.md`** after all folder indexes are correct.
7. **Refresh the root guidance file** (`CLAUDE.md` / `AGENTS.md` / `GEMINI.md`) so it stays consistent with the reconciled vault: update its map of folders and skills, and re-derive any content that draws on `SOUL.md`, `USER.md`, the index tree, the working brain (`brain/`), or the installed skills. **Preserve hand-written guidance and user edits; change only what has genuinely drifted, and never overwrite user content without surfacing the change.** If no guidance file exists yet, create it from `GUIDE.md` per the **First run** section above instead of refreshing.
8. **Apply deltas:** add entries for new files/folders, **remove** entries for deleted or moved content, and correct descriptions when a purpose has materially changed.
9. **Re-inventory and verify.** Walk the filesystem again and confirm every knowledge file/folder is represented and no entry is stale. Do not claim an index is complete unless its directory was actually inspected.
10. **Report** which indexes changed, whether the guidance file was refreshed, and any unresolved ambiguity.

## Scope guardrails

- Touch only `INDEX.md` files and the root guidance file — **never edit the user's notes.**
- Do not modify harness config internals (e.g. `.claude/settings.json`); only the root guidance file (`CLAUDE.md`).

## Conventions

- Use Obsidian links for knowledge notes where practical, e.g. `[[PROFILE]]` or `[[interviews/INDEX|interviews]]`.
- Keep each summary brief enough to scan but specific enough to route retrieval.
- Leaf folders that only hold paired source files (e.g. a PDF + its markdown) may be summarized in their parent index rather than each getting its own `INDEX.md` — note this choice in the parent.

## When to run

Recommend running `/update` near the end of a substantive session, after significant restructuring, or whenever navigation looks stale. Do not claim it runs automatically unless the environment provides an explicit end-of-session hook.
