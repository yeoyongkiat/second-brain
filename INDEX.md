# Vault INDEX

**Entry point for navigating this vault.** Read this first, match the request to the most relevant file or folder below, then open that folder's own `INDEX.md` before scanning its contents. Continue through nested indexes until the likely source files are found; fall back to a broad content search only when the indexes are missing, stale, or insufficient — and report which files supported the answer.

This vault is a **research corpus on Raymond Chua** (Chief Executive Officer, Singapore Health Sciences Authority) and the **Healthcare Services Act (HCSA) legislation** he oversees.

## Root knowledge files

- [[PROFILE]] — Synthesised profile of Raymond Chua: career trajectory, roles, focus areas, leadership style, personal life. Backlinks to the LinkedIn posts and interviews it draws from. **Start here for "who is he / what does he do".**
- [[SPEECH]] — Synthesis of his speech craft: tone, the specific policies/initiatives/products he cites, and the vision he casts for HSA. Backlinks to all 12 speeches.

## Knowledge folders

- [[interviews/INDEX|interviews]] — 2 published long-form interviews (verbatim Q&A). Primary biography + strategy sources.
- [[linkedin/INDEX|linkedin]] — 50 of his LinkedIn posts (raw text). Per-post index lives in [[PROFILE]].
- [[speeches/INDEX|speeches]] — 12 HSA CEO speeches (verbatim). Synthesised in [[SPEECH]].
- [[service-requirements/INDEX|service-requirements]] — HCSA legislation: 22 statutes/regulations as paired PDF + markdown across 6 subfolders.
- [[chat-history/INDEX|chat-history]] — Session journal: per-run notes of file drift + what each session did. Read recent entries at session start.

## System files & directories (not knowledge-indexed)

- `CLAUDE.md` — resolved vault-level agent guidance for the Claude Code harness; read at the start of every session.
- `GUIDE.md` — the harness-agnostic guidance template that `CLAUDE.md` was generated from (placeholders unresolved; kept as portable reference).
- `README.md` — public repo readme describing the second-brain setup.
- `second-brain-initialisation.md` — the installation procedure (kept, not deleted).
- `write-noms.md` — source for the `/write-noms` skill (kept as portable install source).
- `update.md` — canonical portable source for the `/update` skill (installed into `.claude/skills/`). Kept as the replication source of truth.
- `.brain-state.json` — local system cache: the content-hash baseline `/update` diffs against to detect drift. Git-ignored; do not hand-edit.
- `.claude/skills/` — vault-local skills (e.g. `update`). Implementation, not knowledge.
- `.git/` — version control.

## Vault-local skills

- **`/update`** — audits the filesystem, rebuilds this entire `INDEX.md` tree, and refreshes `CLAUDE.md` (or creates it from `GUIDE.md` on first run). Run it after adding/removing/renaming notes or folders, or when navigation looks stale.
- **`/write-noms`** — converts transcripts or meeting notes into Singapore-government-style Notes of Meeting (portable source: root `write-noms.md`).

## When to update this index

Update the nearest `INDEX.md` whenever a file or folder is created, renamed, moved, deleted, or materially changes purpose; update this root index when a root-level file or top-level folder changes. Or just run `/update` for a full reconciliation.
