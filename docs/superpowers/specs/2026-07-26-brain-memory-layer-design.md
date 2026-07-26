# Second Brain — Cross-Session Memory Layer for `/update`

**Date:** 2026-07-26
**Status:** Approved design, pre-implementation

## Problem

The vault's navigation (`INDEX.md` tree) and guidance file (`CLAUDE.md` / `AGENTS.md` / `GEMINI.md`) only stay accurate when the user runs `/update`. But `/update` is manual, so a user may run it only every few sessions. In the gap:

1. **File/structure drift** — files are added, deleted, or renamed outside the agent (Obsidian, GitHub-web upload), silently invalidating the indexes.
2. **In-place content edits** — a file keeps its name and location but its *contents* change. Invisible from a directory listing; the agent has no way to notice.
3. **Conversation/decision history** — the agent has no record of what past sessions discussed, decided, or did, so a fresh session lacks continuity.

The agent, booting from stale indexes, "doesn't know what happened between sessions."

## Goals

- Detect all four change types since the last reconciliation: **added, deleted, renamed, content-edited** — reliably, regardless of how the change arrived.
- Give `/update` a precise "what changed since last time" diff, no matter how many sessions elapsed.
- Preserve a durable, human-readable record of what each session did/decided.
- No session hook. Detection and journaling happen inside `/update` when the user invokes it. (Explicit design decision — see Non-goals.)
- Ship the same capability into the second-brain installer so every new vault has it.

## Non-goals

- **No SessionStart hook / no continuous auto-detection.** The agent becomes aware of drift *at the moment `/update` runs*, not continuously. The mitigating convention is: run `/update` at the **start** of a session to get a catch-up briefing. This trades continuous awareness for a simpler, harness-portable, no-automation design.
- No perfect rename+edit detection (documented limitation below).
- No cross-machine sync of the detection baseline (it is a local cache).

## Runtime prerequisites (zero-install)

This layer adds **no new software** beyond what the second brain already needs. Target deployment is a **non-technical person's laptop**, so the bar is "works out of the box."

- **The only real prerequisite is the AI harness** (Claude Code / Codex / Gemini CLI / OpenCode) with **shell access enabled** — already required to run the vault at all. For non-technical users, the Claude Code **desktop app** is the gentlest path (no terminal setup).
- **Hashing needs nothing installed.** Every OS ships a sha256 tool. The skill uses a **fallback chain**, trying each until one works:

  | OS | Built-in tool |
  |---|---|
  | macOS | `shasum -a 256` |
  | Linux | `sha256sum` |
  | Windows | `certutil -hashfile <f> SHA256` or PowerShell `Get-FileHash` |
  | Any (last resort) | `python3 -c` with `hashlib` |

- **Git is NOT required.** See "Git is optional" below. Non-technical users never need git or GitHub.
- **Python / Anaconda is NOT required** — that is a separate, optional installer step for other features.
- **No jq / JSON tooling** — the agent reads and writes the JSON manifest itself as plain text.

### Git is optional

Durability comes from **files persisting on disk in the vault folder**, not from git. Across sessions, `.brain-state.json` and `chat-history/` are simply there because they were written to disk. Git, when present, adds version history and cross-machine sync — nothing the core mechanism depends on. Concretely:

- **With git:** add `.brain-state.json` to `.gitignore` (it churns every run); commit `chat-history/` so the journal syncs and versions.
- **Without git (non-technical default):** both just live in the folder. The manifest persists locally; `chat-history/` persists as plain notes. Sync, if wanted, is whatever the user already uses (Obsidian Sync, iCloud, Dropbox) — never a git requirement.

The `git_head` manifest field is populated only when git happens to be present, and is pure reporting context — never the change oracle.

## Design overview

Three pieces, all driven by `/update`:

1. **`.brain-state.json`** — a machine-local manifest of content hashes (git-ignored only when git is used): the baseline of "what the agent last saw."
2. **`/update`'s revised flow** — hash-diff against the manifest → report drift → targeted reconcile → journal → rewrite manifest.
3. **`chat-history/`** — a folder of dated markdown notes on disk: the durable record of file drift + session decisions (committed when git is used).

## Component 1 — `.brain-state.json` (state manifest)

- **Location:** vault root. It persists across sessions simply by being a file on disk — no git required.
- **If git is used:** add it to `.gitignore` (it churns a diff every run). **If git is not used:** it just lives in the folder, exactly as durable. If it is ever absent (fresh machine, deleted), the next `/update` re-establishes a baseline — graceful degradation, not an error.
- **Structure:**

```json
{
  "schema": 1,
  "generated_at": "2026-07-26T08:00:00",
  "git_head": "083087b",
  "files": {
    "PROFILE.md":          { "size": 21933, "mtime": "2026-07-25T14:03:00", "sha256": "…" },
    "speeches/2025-…​.md":  { "size": 8123,  "mtime": "2026-07-25T14:08:00", "sha256": "…" }
  }
}
```

- `git_head` is optional; recorded **only if** the vault happens to be a git repo, omitted otherwise. Pure reporting context, never the change oracle.
- `sha256` is the primary change signal. `size`/`mtime` are recorded for context and cheap pre-filtering, but the hash decides.

## Component 2 — Revised `/update` flow

`/update` remains a **full reconciliation** (its existing contract). The manifest makes it *targeted and memory-aware*. New ordered steps:

1. **Detect.** Compute the sha256 of every knowledge file. **Exclusions (definite):** `.git/`, `.obsidian/`, `.claude/` (and other harness dirs), `.brain-state.json`, `chat-history/` (written by `/update` itself — hashing it would self-trigger drift every run), the `docs/` design folder, caches, and generated deps. Compare to `.brain-state.json`:
   | Condition | Classification |
   |---|---|
   | in state, absent on disk | **deleted** |
   | on disk, absent from state | **added** |
   | same path, different hash | **content edited** |
   | an added + a deleted sharing an identical hash | **renamed / moved** |
   | no state file at all | **first run** — establish baseline, no diff |
2. **Report.** Surface a concise changelog: *"Since last /update on 2026-07-20: 2 edited, 1 added, 1 deleted, 1 renamed,"* with the file list.
3. **Reconcile (targeted).** Re-inspect only changed/added files to (re)write their index descriptions; remove deleted entries; rebuild folder indexes bottom-up, root last; refresh the guidance file. A full-inventory verification pass stays as the safety net so the reconciliation contract holds even if the manifest was stale or missing.
4. **Journal.** Append a dated entry to `chat-history/` (Component 3).
5. **Snapshot.** Rewrite `.brain-state.json` with current hashes + timestamp (and `git_head` only if git is present). This resets the baseline for the next run.
6. **Report out.** State which indexes changed, whether the guidance file was refreshed, what was journaled, and any unresolved ambiguity.

### Hashing implementation

- Algorithm: **sha256** of raw file bytes (works for markdown, CSV, PDF alike — detects source-PDF changes too).
- Tool: the zero-install fallback chain from **Runtime prerequisites** — `shasum -a 256` → `sha256sum` → `certutil`/`Get-FileHash` → Python `hashlib`. The skill specifies the algorithm and the fallback order, not a single fixed binary, so it works on any OS with no setup.
- Performance: hundreds of small files hash in well under a second; acceptable to run every `/update`.

### Edge cases

- **First run:** no manifest → baseline only; journal entry notes "baseline established, no prior state."
- **Corrupt / old-schema manifest:** treat as first run, warn the user, re-establish baseline.
- **Rename + edit in the same gap:** hashes won't match, so it appears as delete+add rather than rename. The reconcile still lands correctly (it re-inspects the "added" file); only the label is less precise. **Documented, not fixed.**
- **Binary/PDF unchanged:** hash stable → no false positive (a key advantage over mtime).

## Component 3 — `chat-history/` (durable session journal)

- **Location:** `chat-history/` at the vault root. Durable because it is **plain notes on disk** — no git needed. If git is in use, commit it so the journal versions and syncs; otherwise it persists as ordinary files.
- **Format:** one markdown note per day, `chat-history/YYYY-MM-DD.md`. If `/update` runs more than once in a day, append a new time-stamped `## HH:MM` section to that day's note rather than creating a second file.
- **Its own `INDEX.md`:** `chat-history/INDEX.md` describes the folder as the session log and lists the most recent entries; the root index links to it.
- **Entry shape:**

```markdown
# 2026-07-26 — session log

## 08:13
**Drift since last update:** edited PROFILE.md, SPEECH.md; added coe-catA-10yr.csv; deleted service-requirements/README.md.
**Session notes:** Generated CLAUDE.md from GUIDE.md; reconciled the index tree to the Claude Code harness; designed the cross-session memory layer.
```

- The **Drift** line is generated mechanically from the hash diff (Component 2, step 1).
- The **Session notes** line is the agent's own summary of what was discussed / decided / done, drawn from the conversation.

**Scope — current session only (best-effort).** The journal entry is written for **the session in which `/update` runs**. Sessions where the user never runs `/update` get **no conversation entry** — their decisions are not recorded. This is an accepted trade for simplicity and full harness portability. Note the asymmetry with file drift: because files persist on disk and are diffed against the manifest, **file changes from skipped sessions are still fully caught** on the next `/update` (as a net diff); only the *conversational* memory of those sessions is not preserved.

**Deferred alternative — transcript back-fill.** Harnesses such as Claude Code auto-persist every session as a timestamped transcript on disk (e.g. `~/.claude/projects/<vault>/*.jsonl`). A future version could have `/update` scan transcripts newer than the last journal entry and back-fill a dated entry for each un-journaled session, recovering skipped days. Deferred because it is harness-specific (transcript location/format varies) and heavier. It remains a clean upgrade path and does not conflict with this design.

## Component 4 — Session-start continuity (convention, no hook)

The habit is written into **`GUIDE.md` (the template)**, so every guidance file generated from it — `CLAUDE.md` / `AGENTS.md` / `GEMINI.md` — inherits it automatically rather than being patched by hand. The session-start instruction:

> At the start of a session, read the most recent `chat-history/` entries for continuity. If you are beginning substantive work, run `/update` first so the indexes and this guidance reflect any file drift since the last reconciliation.

Running `/update` at the **start** of a session becomes the habit that yields a catch-up briefing. This is the deliberate, hook-free, zero-install substitute for continuous awareness.

**Honest caveat — soft vs. hard.** A guidance-file instruction is *best-effort*: the agent reads it as context and generally follows it, but it is not guaranteed to fire the way a hook is. That is an accepted trade for portability and zero-install. If real-world use shows the habit is skipped too often, the set-aside **SessionStart hook** is the deterministic upgrade — still zero-install (a single config entry), so this design does not preclude it.

**Deliberately phrased as "read continuity, then update *if* starting real work"** — not "auto-run `/update` on every session start" — so a quick one-off question does not trigger a full reconcile + journal write.

## Wiring — files this touches

| File | Change |
|---|---|
| `.gitignore` | **only if the vault uses git:** add `.brain-state.json` (keep `chat-history/` tracked). Skipped entirely on git-less setups. |
| `update.md` (root, **new**) | the canonical portable skill source — full harness-neutral instructions (detect → report → reconcile → journal → snapshot; manifest; journal; exclusions; limitations). Source of truth for replication. |
| `.claude/skills/update/SKILL.md` | (re)installed **from `update.md`** by copy + convention substitution + wrapper — not hand-authored |
| `GUIDE.md` (template) | add the session-start habit here so all generated guidance files inherit it |
| `CLAUDE.md` (and any regenerated guidance file) | session-start read of `chat-history/`; describe `/update`'s drift+journal behavior (inherited from `GUIDE.md`) |
| `INDEX.md` (root) | list `chat-history/` (memory log); list `update.md` as the portable skill source; note `.brain-state.json` as a local system cache |
| `chat-history/INDEX.md` | new folder index |
| `second-brain-initialisation.md` (Step 5) | rewrite to **install update from the bundled `update.md`** (parallel to Step 6/write-noms), keeping prose as rationale only |

## Propagation & replication

The system is meant to be replicated across many computers, so the update skill must install **identically** everywhere. It therefore adopts the **bundle-copy model already used by write-noms** (Step 6), not prose regeneration.

**Canonical source of truth: a root `update.md`.** The complete, harness-neutral skill (all the detail in this spec — manifest schema, sha256 detect/diff + classification, hash fallback chain, `chat-history/` journal, exclusions, session-start convention) is authored **once** into a portable `update.md` at the vault root, mirroring how `write-noms.md` sits at the root today. Its frontmatter carries only `name` + `description`; the body is the full instructions written in harness-neutral terms (referring to `GUIDANCE_FILE`, `SKILLS_DIR`, `INVOKE` rather than hard-coding `CLAUDE.md` / `.claude/skills/` / `/update`).

**Installation = copy + substitute, never re-derive.** Installing on any machine means: copy `update.md` into that harness's `SKILLS_DIR` as `SKILL.md`, substitute the three harness conventions, and add the native wrapper. Because every machine copies the same canonical file, they all run a byte-identical skill — no drift.

**Step 5 of `second-brain-initialisation.md` is rewritten** from "author the update skill from this description" to "install the update skill from the bundled `update.md`" — exactly parallel to Step 6 for write-noms. The prose in Step 5 is retained only as human-readable rationale, not as the artifact agents rebuild from.

**Relationship of the files:**

```
update.md  (root, canonical portable source, harness-neutral)
   └── copied + conventions substituted + wrapper added ──▶  <SKILLS_DIR>/update/SKILL.md  (installed, per-harness)
```

Note: `update.md` is a normal root file, so the manifest hashes it. If the skill source itself changes, `/update` correctly reports that as drift.

## Known limitations (accepted)

1. Awareness of drift is reconciliation-time, not continuous (no hook — by choice).
2. Rename+edit in one gap is labeled delete+add.
3. The detection baseline is machine-local; a fresh copy of the vault (new machine, or `.brain-state.json` deleted) simply re-baselines on the first `/update` — no error, just no diff that one time.
4. **Conversation history is current-session-only.** Sessions where `/update` is never run leave no journal entry (file drift from them is still caught on the next run; only their conversational memory is lost). Transcript back-fill (deferred) would remove this limitation.
5. **File drift is captured as a net diff, not a per-session history.** Multiple edits to one file across skipped sessions collapse to a single "changed"; a file added then deleted within the gap nets to invisible. Sufficient for index reconciliation, which cares only about current state.
