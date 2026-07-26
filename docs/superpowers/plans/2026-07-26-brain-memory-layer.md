# Brain Memory Layer Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Give `/update` a cross-session memory layer — content-hash drift detection (`.brain-state.json`) and a `chat-history/` session journal — packaged as a bundle-copy skill (`update.md`) that replicates identically across machines.

**Architecture:** A canonical harness-neutral `update.md` at the vault root is the single source of truth; it is installed into each harness's skills dir by copy + convention substitution. The skill hashes every knowledge file, diffs against a git-optional local manifest to classify add/edit/delete/rename, reconciles the index tree + guidance file (targeted by the diff), journals the session to `chat-history/`, then rewrites the manifest. Session-start continuity is a soft habit written into `GUIDE.md`, inherited by all generated guidance files. No hook, zero-install (OS-native sha256 via a fallback chain), git not required.

**Tech Stack:** Markdown skill/guidance files; OS-native hashing (`shasum`/`sha256sum`/`certutil`/`Get-FileHash`/python `hashlib`); JSON manifest read/written as text by the agent.

**Spec:** `docs/superpowers/specs/2026-07-26-brain-memory-layer-design.md`

---

## File structure

| File | Responsibility |
|---|---|
| `update.md` (root, **new**) | Canonical portable skill source. Full harness-neutral instructions. Source of truth for replication. |
| `.claude/skills/update/SKILL.md` (**rewrite**) | Installed skill — copy of `update.md` with Claude Code conventions substituted. |
| `GUIDE.md` (**edit**) | Add session-start habit + memory-layer working rules, inherited by generated guidance files. |
| `CLAUDE.md` (**edit**) | Session-start read of `chat-history/`; describe `/update` drift+journal behavior. |
| `chat-history/` (**new**) | Session journal folder (dated notes). |
| `chat-history/INDEX.md` (**new**) | Folder index for the journal. |
| `INDEX.md` (root, **edit**) | List `chat-history/`, `update.md`, and note `.brain-state.json`. |
| `.gitignore` (**edit**) | Ignore `.brain-state.json`. |
| `second-brain-initialisation.md` (**edit**, Step 5) | Rewrite to install update from bundled `update.md`. |
| `.brain-state.json` (root, **generated**) | Written by the first live `/update` run — the baseline manifest. |

---

### Task 0: Branch

- [ ] **Step 1: Create a feature branch**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
git checkout -b feat/brain-memory-layer
```
Expected: `Switched to a new branch 'feat/brain-memory-layer'`

---

### Task 1: Author the canonical `update.md` (portable source)

**Files:**
- Create: `update.md` (vault root)

- [ ] **Step 1: Write `update.md` with this exact content**

````markdown
---
name: update
description: Use when the user invokes the update skill or asks to refresh, rebuild, reconcile, or audit the vault's INDEX.md tree and guidance file. Full filesystem reconciliation (folder indexes bottom-up, root last); on first run creates the guidance file from GUIDE.md for the harness in use. Detects file drift since the last run by content hashing (added/deleted/renamed/edited), journals the session to chat-history/, and rewrites the state manifest. Touches only index, guidance, state, and journal files — never the user's notes.
---

# update — Reconcile the Vault, Detect Drift, Journal the Session

The vault is navigated through a tree of `INDEX.md` files and the agent boots from a root guidance file (`GUIDANCE_FILE`). Both drift as notes are created, edited, renamed, moved, or deleted — including edits made outside the agent. This skill reconciles the entire index tree, refreshes the guidance file, detects exactly what changed since it last ran, and records the session — so navigation and memory stay reliable even after many sessions without running.

**Trigger:** the user runs the update skill (`INVOKE`update), or asks to refresh / rebuild / reconcile / audit the vault indexes or guidance file.

## Harness conventions (portable source)

This is the canonical portable source. When installed, three tokens are substituted for the harness in use:

| Token | Claude Code | Codex / OpenCode | Gemini CLI |
|---|---|---|---|
| `GUIDANCE_FILE` | `CLAUDE.md` | `AGENTS.md` | `GEMINI.md` |
| `SKILLS_DIR` | `.claude/skills/` | `.agents/skills/` | `.gemini/commands/` |
| `INVOKE` | `/` | `$` | `/` |

## This is always a full reconciliation

Do not rely on remembered changes or timestamps. Re-inventory the filesystem from scratch every run. That is what makes the skill trustworthy after long gaps.

## Runtime — zero install

Hashing needs no installed software. Use the first available tool: `shasum -a 256` → `sha256sum` → (Windows) `certutil -hashfile <f> SHA256` or PowerShell `Get-FileHash -Algorithm SHA256` → (last resort) `python3 -c "import hashlib,sys;print(hashlib.sha256(open(sys.argv[1],'rb').read()).hexdigest())"`. Git is not required.

## Procedure

### 1. Detect file drift

- Read `.brain-state.json` at the vault root if it exists (the baseline manifest).
- Compute the **sha256** of every knowledge file. **Exclusions (definite):** `.git/`, `.obsidian/`, the harness config dir (`.claude/` / `.agents/` / `.gemini/`), `.brain-state.json`, `chat-history/`, `docs/`, caches, and generated dependencies.
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

If there is no `GUIDANCE_FILE`, create it from `GUIDE.md`:

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

Refresh `GUIDANCE_FILE` so it stays consistent with the reconciled vault: update its map of folders and skills, and re-derive content drawing on `SOUL.md`, `USER.md`, the index tree, the working brain, or installed skills. Preserve hand-written guidance and user edits; change only what genuinely drifted; never overwrite user content without surfacing the change.

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
````

- [ ] **Step 2: Verify frontmatter and required sections**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
head -3 update.md
grep -c -E '^### [1-8]\. ' update.md          # expect 8 numbered procedure steps
grep -q 'GUIDANCE_FILE' update.md && grep -q 'chat-history/' update.md && grep -q 'brain-state.json' update.md && echo "tokens+artifacts present ✓"
```
Expected: frontmatter starts with `---` then `name: update`; the grep count is `8`; and `tokens+artifacts present ✓`.

- [ ] **Step 3: Commit**

```bash
git add update.md
git commit -m "feat: add canonical portable update.md skill source with memory layer"
```

---

### Task 2: Install `update.md` → `.claude/skills/update/SKILL.md`

The installed skill is `update.md` with Claude Code conventions substituted (`GUIDANCE_FILE`→`CLAUDE.md`, `SKILLS_DIR`→`.claude/skills/`, `INVOKE`→`/`) and the portable-source "Harness conventions" table replaced by a one-line note.

**Files:**
- Modify: `.claude/skills/update/SKILL.md` (full rewrite)

- [ ] **Step 1: Overwrite `.claude/skills/update/SKILL.md` with the Claude-substituted copy**

Take the full `update.md` content from Task 1 and apply these substitutions throughout:
- Title line → `# /update — Reconcile the Vault, Detect Drift, Journal the Session`
- Replace the entire "## Harness conventions (portable source)" section with:
  `> Installed for Claude Code: guidance file is CLAUDE.md, skills dir is .claude/skills/, invoked as /update. Canonical source: root update.md.`
- Every `GUIDANCE_FILE` → `CLAUDE.md`; every `SKILLS_DIR` → `.claude/skills/`; every `INVOKE`update → `/update`; the harness config dir reference stays `.claude/`.

- [ ] **Step 2: Verify substitution left no tokens behind**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
grep -n -E 'GUIDANCE_FILE|SKILLS_DIR|INVOKE' .claude/skills/update/SKILL.md || echo "no tokens ✓"
grep -q '/update' .claude/skills/update/SKILL.md && grep -q 'CLAUDE.md' .claude/skills/update/SKILL.md && echo "claude conventions applied ✓"
head -3 .claude/skills/update/SKILL.md
```
Expected: `no tokens ✓`, `claude conventions applied ✓`, frontmatter intact (`name: update`).

- [ ] **Step 3: Commit**

```bash
git add .claude/skills/update/SKILL.md
git commit -m "feat: install update skill from update.md with memory layer (Claude Code)"
```

---

### Task 3: Validate the drift-detection algorithm (reference test)

Prove the classification rules from Task 1 step 1 are correct and unambiguous, using a throwaway reference implementation in the scratchpad. This is the one executable check.

**Files:**
- Create (scratchpad, not shipped): `/private/tmp/claude-501/-Users-yeoyongkiat-Desktop-second-brain/5578bb2a-4168-4aef-9ebe-eb8906356a43/scratchpad/test_drift.py`

- [ ] **Step 1: Write the failing test harness**

```python
import hashlib, os, json, tempfile, shutil

def sha(p):
    return hashlib.sha256(open(p, "rb").read()).hexdigest()

def manifest(root):
    m = {}
    for dp, _, fns in os.walk(root):
        for fn in fns:
            fp = os.path.join(dp, fn)
            rel = os.path.relpath(fp, root)
            m[rel] = sha(fp)
    return m

def diff(old, new):
    added   = [p for p in new if p not in old]
    deleted = [p for p in old if p not in new]
    edited  = [p for p in new if p in old and new[p] != old[p]]
    # rename = an added + a deleted sharing an identical hash
    renames = []
    for a in list(added):
        for d in list(deleted):
            if new[a] == old[d]:
                renames.append((d, a)); added.remove(a); deleted.remove(d); break
    return {"added": sorted(added), "deleted": sorted(deleted),
            "edited": sorted(edited), "renamed": sorted(renames)}

root = tempfile.mkdtemp()
for name, body in [("a.md", "alpha"), ("b.md", "bravo"), ("c.md", "charlie")]:
    open(os.path.join(root, name), "w").write(body)
old = manifest(root)

# mutate: edit a, delete b, add d, rename c -> e (identical content)
open(os.path.join(root, "a.md"), "w").write("alpha EDITED")
os.remove(os.path.join(root, "b.md"))
open(os.path.join(root, "d.md"), "w").write("delta")
os.rename(os.path.join(root, "c.md"), os.path.join(root, "e.md"))
new = manifest(root)

d = diff(old, new)
shutil.rmtree(root)
assert d["edited"]  == ["a.md"],           d
assert d["deleted"] == ["b.md"],           d
assert d["added"]   == ["d.md"],           d
assert d["renamed"] == [("c.md", "e.md")], d
print("drift classification correct:", json.dumps(d))
```

- [ ] **Step 2: Run it and confirm all classifications pass**

Run:
```bash
python3 /private/tmp/claude-501/-Users-yeoyongkiat-Desktop-second-brain/5578bb2a-4168-4aef-9ebe-eb8906356a43/scratchpad/test_drift.py
```
Expected: `drift classification correct: {"added": ["d.md"], "deleted": ["b.md"], "edited": ["a.md"], "renamed": [["c.md", "e.md"]]}` and a zero exit code (no AssertionError).

- [ ] **Step 3: No commit** (scratchpad artifact; not part of the vault).

---

### Task 4: Add the session-start habit + memory rules to `GUIDE.md`

**Files:**
- Modify: `GUIDE.md`

- [ ] **Step 1: Add a session-start step**

In `GUIDE.md` under "## Start of Each Session", append a new numbered item after the existing ones:
```markdown
5. Read the most recent `chat-history/` entries for continuity. If beginning substantive work, run `INVOKE`update first so the indexes and this guidance reflect any file drift since the last reconciliation.
```

- [ ] **Step 2: Add memory-layer working rules**

In `GUIDE.md` under "## Working Rules", add these bullets after the existing `$update` bullet:
```markdown
- The `update` skill also detects file drift since it last ran (content hashing → `.brain-state.json`) and records each run to `chat-history/`. Prefer running it at session start to catch up and at session end to snapshot.
- Treat `chat-history/` as the session journal (what past sessions did/decided) and `.brain-state.json` as a local system cache (do not hand-edit).
```

- [ ] **Step 3: Verify and commit**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
grep -q 'chat-history/' GUIDE.md && grep -q 'brain-state.json' GUIDE.md && echo "guide updated ✓"
```
Expected: `guide updated ✓`
```bash
git add GUIDE.md
git commit -m "feat: add session-start continuity habit and memory rules to GUIDE.md"
```

---

### Task 5: Update `CLAUDE.md` (resolved guidance file)

**Files:**
- Modify: `CLAUDE.md`

- [ ] **Step 1: Add the session-start read**

In `CLAUDE.md` under "## Start of Each Session", append:
```markdown
4. Read the latest `chat-history/` entries for continuity with prior sessions. If you are starting substantive work, run `/update` first to catch up on any file drift since the last reconciliation.
```

- [ ] **Step 2: Describe the new `/update` behavior**

In `CLAUDE.md`, under the `/update` bullet in "## Working Rules", replace that bullet with:
```markdown
  - `/update` — reconcile the entire `INDEX.md` tree and refresh this guidance file, **detect file drift since the last run** (content hashing → `.brain-state.json`), and **journal the session** to `chat-history/`. Run after vault changes, at session start to catch up, or near the end of a substantive session.
```

- [ ] **Step 3: Verify and commit**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
grep -q 'chat-history/' CLAUDE.md && grep -q 'brain-state.json' CLAUDE.md && echo "claude.md updated ✓"
```
Expected: `claude.md updated ✓`
```bash
git add CLAUDE.md
git commit -m "feat: wire chat-history continuity and drift/journal /update into CLAUDE.md"
```

---

### Task 6: Create `chat-history/` and its index

**Files:**
- Create: `chat-history/INDEX.md`
- Create: `chat-history/2026-07-26.md` (first journal entry — this session)

- [ ] **Step 1: Write `chat-history/INDEX.md`**

```markdown
# chat-history — INDEX

The vault's **session journal**. Each note records what an `/update` run found and did: file drift since the previous run, plus a summary of what that session discussed or decided. Written by the `update` skill (step 6), one note per day (`YYYY-MM-DD.md`); multiple runs in a day append `## HH:MM` sections.

Read the most recent entries at the start of a session for continuity with prior work.

## Convention

- Filenames: `YYYY-MM-DD.md`, newest by date.
- Each entry has a **Drift since last update** line (from the hash diff) and a **Session notes** line (the agent's summary).
- Scope is current-session-only: sessions where `/update` was never run are not journaled.

## Maintenance

Written automatically by `/update`. Do not delete entries unless the user asks.
```

- [ ] **Step 2: Write the first entry `chat-history/2026-07-26.md`**

```markdown
# 2026-07-26 — session log

## Session
**Drift since last update:** baseline established (first run of the memory layer); added CLAUDE.md, GUIDE.md-linked guidance, update.md, chat-history/.
**Session notes:** Generated CLAUDE.md from GUIDE.md; reconciled the index tree to the Claude Code harness. Designed and built the cross-session memory layer for `/update`: `.brain-state.json` content-hash drift detection, `chat-history/` session journal, session-start continuity habit in GUIDE.md, and the bundle-copy replication model via a canonical root `update.md`.
```

- [ ] **Step 3: Verify and commit**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
ls chat-history/INDEX.md chat-history/2026-07-26.md
```
Expected: both paths listed.
```bash
git add chat-history/
git commit -m "feat: create chat-history session journal folder + first entry"
```

---

### Task 7: Update root `INDEX.md`

**Files:**
- Modify: `INDEX.md`

- [ ] **Step 1: Add `chat-history/` under "## Knowledge folders"**

Add:
```markdown
- [[chat-history/INDEX|chat-history]] — Session journal: per-run notes of file drift + what each session did. Read recent entries at session start.
```

- [ ] **Step 2: Add `update.md` and `.brain-state.json` under "## System files & directories (not knowledge-indexed)"**

Add these two bullets:
```markdown
- `update.md` — canonical portable source for the `/update` skill (installed into `.claude/skills/`). Kept as the replication source of truth.
- `.brain-state.json` — local system cache: the content-hash baseline `/update` diffs against to detect drift. Git-ignored; do not hand-edit.
```

- [ ] **Step 3: Verify and commit**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
grep -q 'chat-history/INDEX' INDEX.md && grep -q 'update.md' INDEX.md && grep -q 'brain-state.json' INDEX.md && echo "root index updated ✓"
```
Expected: `root index updated ✓`
```bash
git add INDEX.md
git commit -m "docs: index chat-history, update.md source, and .brain-state.json"
```

---

### Task 8: Ignore `.brain-state.json` in git

**Files:**
- Modify: `.gitignore`

- [ ] **Step 1: Append the ignore rule**

Add to `.gitignore`:
```
# Local drift-detection cache — machine-local baseline, not shared
.brain-state.json
```

- [ ] **Step 2: Verify and commit**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
git check-ignore .brain-state.json && echo "ignored ✓"
```
Expected: prints `.brain-state.json` then `ignored ✓`.
```bash
git add .gitignore
git commit -m "chore: git-ignore .brain-state.json local cache"
```

---

### Task 9: Rewrite `second-brain-initialisation.md` Step 5 (bundle-copy install)

Rewrite Step 5 so it installs the update skill from the bundled `update.md` (parallel to Step 6 for write-noms), rather than describing it in prose for re-authoring.

**Files:**
- Modify: `second-brain-initialisation.md` (the "## Step 5" section)

- [ ] **Step 1: Replace the Step 5 body**

Replace the entire content of "## Step 5: Create the Index Update Skill" with:

```markdown
## Step 5: Install the Update Skill

The `update` skill reconciles the whole `INDEX.md` tree and the guidance file, detects file drift since it last ran (content hashing → `.brain-state.json`), and journals each run to `chat-history/`. It is invoked with your harness's `INVOKE` prefix (for example `/update`, or `$update` on Codex).

Like `write-noms` (Step 6), it is supplied as a **bundled portable source** — `update.md` at the vault root — so every install is identical and replicates across machines by copying, not by re-authoring. Treat that file as an installation source:

1. Confirm that `update.md` exists at the vault root.
2. Read the entire file.
3. Validate its YAML frontmatter contains `name: update` and a clear triggering description.
4. Create `<SKILLS_DIR>/update/SKILL.md` from its contents, substituting the harness conventions it documents: `GUIDANCE_FILE`, `SKILLS_DIR`, and the `INVOKE` prefix. Replace its "Harness conventions (portable source)" table with a one-line note stating the resolved conventions.
5. Preserve every substantive rule: the full-reconciliation contract, the sha256 drift detection and its classification table, the zero-install hash fallback chain, the first-run guidance-file creation from `GUIDE.md`, the `chat-history/` journal format, the exclusions, and the limitations.
6. Add whatever native wrapper or UI metadata your harness expects, with a default prompt that explicitly invokes the update skill.
7. Leave the root `update.md` in place as the portable installation source and replication source of truth.

Do not silently invent a replacement skill if `update.md` is missing or incomplete. Explain the problem and ask the user to provide or complete the source.

Also ensure the vault has a `chat-history/` folder with its own `INDEX.md` (the session journal the skill writes to), and — if the vault uses git — add `.brain-state.json` to `.gitignore`.

After installation, validate that:

- The directory and frontmatter names are `update`.
- `SKILL.md` has only `name` and `description` in its frontmatter.
- No unresolved `GUIDANCE_FILE` / `SKILLS_DIR` / `INVOKE` tokens remain.
- The installed instructions retain drift detection, journaling, first-run guidance creation, and the full-reconciliation contract.
- Any native short description is 25–64 characters and its default prompt invokes the update skill.

Tell the user the update skill is ready, explain when to invoke it (after changes, at session start to catch up, near session end to snapshot), and wait before continuing.
```

- [ ] **Step 2: Verify and commit**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
grep -q 'bundled portable source' second-brain-initialisation.md && grep -q 'update.md' second-brain-initialisation.md && grep -q 'chat-history/' second-brain-initialisation.md && echo "step 5 rewritten ✓"
```
Expected: `step 5 rewritten ✓`
```bash
git add second-brain-initialisation.md
git commit -m "docs: rewrite installer Step 5 to bundle-copy the update skill from update.md"
```

---

### Task 10: Run `/update` live to establish the baseline and close the loop

Exercise the whole skill end-to-end: it should detect this session's changes, reconcile, journal, and write `.brain-state.json`.

- [ ] **Step 1: Invoke the update skill**

Invoke `/update` (the freshly installed skill). It should: report "first run — establishing baseline" (no prior manifest), reconcile the indexes (already current), refresh `CLAUDE.md` if needed, append/confirm today's `chat-history/` entry, and write `.brain-state.json`.

- [ ] **Step 2: Verify the manifest was written and is valid JSON**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
python3 -c "import json;d=json.load(open('.brain-state.json'));print('schema',d['schema'],'files',len(d['files']))"
```
Expected: `schema 1 files <N>` with N ≈ the number of knowledge files (roughly 100+, excluding `.git/`, `.obsidian/`, `.claude/`, `chat-history/`, `docs/`).

- [ ] **Step 3: Verify exclusions held**

Run:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
python3 -c "import json;k=json.load(open('.brain-state.json'))['files'].keys();bad=[p for p in k if p.startswith('.git/') or p.startswith('.claude/') or p.startswith('chat-history/') or p.startswith('docs/') or p=='.brain-state.json'];print('leaked:',bad or 'none ✓')"
```
Expected: `leaked: none ✓`

- [ ] **Step 4: Prove re-run detects a real edit (round-trip test)**

Make a trivial content edit to a knowledge file, then confirm a second `/update` would classify it as edited. Quick manual check without mutating a real note — hash one file, compare:
```bash
cd /Users/yeoyongkiat/Desktop/second-brain
python3 -c "
import json,hashlib
m=json.load(open('.brain-state.json'))['files']
p='PROFILE.md'
cur=hashlib.sha256(open(p,'rb').read()).hexdigest()
print('matches manifest ✓' if m[p]['sha256']==cur else 'MISMATCH')
"
```
Expected: `matches manifest ✓` (confirms the stored hash equals the live file — the baseline is accurate).

- [ ] **Step 5: Commit the generated journal/index changes** (not `.brain-state.json` — it is git-ignored)

```bash
git add -A
git commit -m "chore: establish memory-layer baseline via first /update run"
```

---

## Notes for the executor

- `.brain-state.json` is git-ignored (Task 8); it will not appear in `git add -A`. That is correct.
- Tasks 4 and 5 both touch guidance; keep the `chat-history/` wording consistent between `GUIDE.md` (uses `INVOKE`update token) and `CLAUDE.md` (uses literal `/update`).
- If Task 10 step 1's live `/update` already writes today's `chat-history` entry, reconcile it with the Task 6 seed entry (append a `## HH:MM` section rather than duplicating the file).
