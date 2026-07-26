---
name: update
description: Use when the user invokes /update or asks to refresh, rebuild, reconcile, or audit the vault's INDEX.md tree and guidance file. Full filesystem reconciliation (folder indexes bottom-up, root last); on first run creates CLAUDE.md from GUIDE.md. Detects file drift since the last run by content hashing (added/deleted/renamed/edited), journals the session to chat-history/, and rewrites the state manifest. Touches only index, guidance, state, and journal files — never the user's notes.
---

# /update — Reconcile the Vault, Detect Drift, Journal the Session

The vault is navigated through a tree of `INDEX.md` files and the agent boots from a root guidance file (`CLAUDE.md`). Both drift as notes are created, edited, renamed, moved, or deleted — including edits made outside the agent. This skill reconciles the entire index tree, refreshes the guidance file, detects exactly what changed since it last ran, and records the session — so navigation and memory stay reliable even after many sessions without running.

**Trigger:** the user types `/update`, or asks to refresh / rebuild / reconcile / audit the vault indexes or guidance file.

> Installed for Claude Code: guidance file is `CLAUDE.md`, skills dir is `.claude/skills/`, invoked as `/update`. Canonical source: root `update.md`.

## This is always a full reconciliation

Do not rely on remembered changes or timestamps. Re-inventory the filesystem from scratch every run. That is what makes the skill trustworthy after long gaps.

## Runtime — zero install

Hashing needs no installed software. Use the first available tool: `shasum -a 256` → `sha256sum` → (Windows) `certutil -hashfile <f> SHA256` or PowerShell `Get-FileHash -Algorithm SHA256` → (last resort) `python3 -c "import hashlib,sys;print(hashlib.sha256(open(sys.argv[1],'rb').read()).hexdigest())"`. Git is not required.

## Procedure

### 1. Detect file drift

- Read `.brain-state.json` at the vault root if it exists (the baseline manifest).
- Compute the **sha256** of every knowledge file. **Exclusions (definite):** `.git/`, `.obsidian/`, the harness config dir (`.claude/`), `.brain-state.json`, `chat-history/`, `docs/`, caches, and generated dependencies.
- Compare current hashes to the manifest and classify:

  | Condition | Classification |
  |---|---|
  | in manifest, absent on disk | deleted |
  | on disk, absent from manifest | added |
  | same path, different hash | content edited |
  | an added + a deleted sharing an identical hash | renamed / moved |
  | no manifest at all | first run — establish baseline, no diff |

### 2. Report drift

Surface a concise changelog, e.g. *"Since last update on 2026-07-20: 2 edited, 1 added, 1 deleted, 1 renamed,"* with the file list. On first run, say a baseline is being established.

### 3. First run: no guidance file yet

If there is no `CLAUDE.md`, create it from `GUIDE.md`:

1. Detect the harness and choose its guidance filename (`CLAUDE.md` / `AGENTS.md` / `GEMINI.md`). If undetectable, default to `AGENTS.md` and say so.
2. Read `GUIDE.md` (the vault-root template) as the structure and rules.
3. Resolve every `{{PLACEHOLDER}}` against the vault's actual contents (`SOUL.md`, `USER.md`, the working brain, the `INDEX.md` tree, installed skills). Do **not** invent missing identity details: if a referenced file does not exist, adapt to what the vault actually contains and note the absence. Ask the user only for a genuinely user-owned fact (e.g. an agent name) that cannot be derived.
4. Substitute harness conventions (skills dir, invocation prefix).
5. Write the resolved content and remove the installation-template notice from the generated file. Leave `GUIDE.md` in place as the portable template.

On later runs the guidance file exists, so step 5 below *refreshes* it instead.

### 4. Reconcile the index tree (targeted by the drift)

- Use the drift list to re-inspect only changed/added files when writing their one-line index descriptions.
- Create or update folder-level `INDEX.md` files from the deepest folders upward; each describes the folder's purpose, summarizes and links its files and immediate subfolders, preserves useful hand-written notes, and states when it must be updated.
- Remove index entries for deleted/moved content; correct descriptions whose purpose materially changed.
- Update the root `INDEX.md` after all folder indexes are correct.
- A full-inventory verification pass remains the safety net so the reconciliation is complete even if the manifest was stale or missing.

### 5. Refresh the guidance file

Refresh `CLAUDE.md` so it stays consistent with the reconciled vault: update its map of folders and skills, and re-derive content drawing on `SOUL.md`, `USER.md`, the index tree, the working brain, or installed skills. Preserve hand-written guidance and user edits; change only what genuinely drifted; never overwrite user content without surfacing the change.

### 6. Journal the session

Append a dated entry to `chat-history/` (the session journal):

- One markdown note per day: `chat-history/YYYY-MM-DD.md`. If the skill runs more than once in a day, append a new `## HH:MM` section to that day's note.
- **Scope: current session only.** Journal the session in which this skill runs; sessions where it was never run are not recorded (their file drift is still caught here as a net diff).
- Entry shape:

```markdown
# YYYY-MM-DD — session log

## HH:MM
**Drift since last update:** <edited/added/deleted/renamed file list, from step 1>.
**Session notes:** <your summary of what was discussed / decided / done>.
```

### 7. Snapshot the manifest

Rewrite `.brain-state.json` with the current hashes and timestamp, resetting the baseline for next run:

```json
{
  "schema": 1,
  "generated_at": "YYYY-MM-DDTHH:MM:SS",
  "git_head": "<short SHA, only if the vault is a git repo; omit otherwise>",
  "files": {
    "<path>": { "size": <bytes>, "mtime": "<iso>", "sha256": "<hex>" }
  }
}
```

### 8. Verify and report

Re-inventory the filesystem and confirm every knowledge file/folder is represented and no entry is stale. Report which indexes changed, whether the guidance file was created/refreshed, what was journaled, and any unresolved ambiguity.

## Scope guardrails

- Touch only `INDEX.md` files, the guidance file, `.brain-state.json`, and `chat-history/` — **never edit the user's notes.**
- Do not modify harness config internals; only the guidance file.

## Git is optional

Durability comes from files on disk, not git. `.brain-state.json` and `chat-history/` persist because they are written to the folder. If the vault uses git, add `.brain-state.json` to `.gitignore` and commit `chat-history/`; if not, both simply live in the folder. `git_head` is recorded only when git is present, as reporting context — never the change oracle.

## Edge cases & limitations

- **First run:** no manifest → baseline only; journal notes "baseline established, no prior state."
- **Corrupt / old-schema manifest:** treat as first run, warn, re-establish baseline.
- **Rename + edit in the same gap:** hashes won't match → shown as delete+add, not rename. Reconcile still lands correctly (re-inspects the "added" file); only the label is less precise.
- **Drift is a net diff:** multiple edits to one file collapse to one "changed"; a file added then deleted within the gap nets to invisible. Sufficient for index reconciliation.
- **Conversation history is current-session-only** (see step 6).

## When to run

Run near the end of a substantive session, after significant restructuring, or at the **start** of a session to catch up on drift. Do not claim it runs automatically unless the environment provides an explicit hook.
