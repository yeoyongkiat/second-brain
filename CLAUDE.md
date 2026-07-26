# Second Brain — Agent Guidance

This workspace is an Obsidian vault and **Yeo Yong Kiat**'s personal second brain, run under the **Claude Code** harness (this file, `CLAUDE.md`, is the guidance the agent reads at the start of every session). It was generated from the harness-agnostic template `GUIDE.md`.

The vault currently holds a research corpus on **Raymond Chua** (Chief Executive Officer, Singapore Health Sciences Authority) and the **Healthcare Services Act (HCSA)** legislation he oversees.

## Start of Each Session

1. Read `INDEX.md` — the vault navigation entry point (root map of knowledge files and folders).
2. From `INDEX.md`, follow the relevant folder-level `INDEX.md` before scanning that folder's contents; continue through nested indexes until the likely source files are found.
3. For "who is Raymond Chua / what does he do", start at `PROFILE.md`. For his speech craft, policy lexicon, and vision for HSA, start at `SPEECH.md`.

> This vault has no `SOUL.md` (agent identity) or `USER.md` (user profile) — it is a research corpus rather than a personal-identity brain. If either is added later, read it at session start too.

## Working Rules

- Treat personal information as private by default and keep Yeo Yong Kiat in control of consequential decisions.
- Preserve existing user content and inspect files before replacing or restructuring them.
- Navigate through the `INDEX.md` tree; do not scan the whole vault unless the indexes are insufficient or the user asks for a comprehensive search.
- Use the vault-local skills in `.claude/skills/` when their trigger conditions apply:
  - `/update` — reconcile the entire `INDEX.md` tree and refresh this guidance file after vault changes.
  - `/write-noms` — turn transcripts, meeting notes, or recordings into Singapore-government-style Notes of Meeting (portable source: root `write-noms.md`).
- Run `/update` after substantial vault changes, when navigation may be stale, or near the end of a substantive session.

## Index Discipline

- Keep every knowledge folder's `INDEX.md` aligned with its immediate contents.
- Update the nearest index when a file or folder is created, renamed, moved, deleted, or materially changes purpose.
- Keep the root `INDEX.md` aligned with root knowledge files and top-level knowledge folders.
- Exclude `.obsidian/`, `.git/`, `.claude/` (harness config and skill implementation), caches, and generated dependencies from item-by-item knowledge indexing.

## Search Fallback

If the indexes are insufficient, stale, ambiguous, or the user requests a comprehensive search:

1. Search all relevant knowledge filenames and contents, using related terms and likely synonyms where useful.
2. Exclude `.obsidian/`, version-control data, caches, generated dependencies, and other application internals unless the request specifically concerns them.
3. Follow relevant links and inspect surrounding context rather than relying only on isolated search matches.
4. For a large vault or independent subject areas, use parallel agents when available and appropriate, then reconcile their findings.
5. Cite or identify the vault files supporting the result and distinguish conflicts or uncertainty.
6. If the search reveals missing or stale index entries, run or recommend `/update` after completing the user's retrieval task.

## Communication

- Lead with the clearest practical recommendation or outcome.
- Distinguish facts, assumptions, interpretations, and open questions.
- Prefer lightweight, maintainable systems over unnecessary complexity.
- Challenge ideas respectfully when doing so materially improves the result.
