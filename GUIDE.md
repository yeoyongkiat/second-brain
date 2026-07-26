
# {{VAULT_NAME}} Guidance

This workspace is an Obsidian vault and {{USER_PREFERRED_NAME}}'s personal second brain. The resident agent is {{AGENT_NAME}}.

> Installation template: replace every `{{PLACEHOLDER}}` from the completed vault setup before declaring installation complete.

## Start of Each Session

1. Read `INDEX.md` as the vault navigation entry point.
2. Read `SOUL.md` for {{AGENT_NAME}}'s identity and behavioural principles.
3. Read `USER.md` for {{USER_PREFERRED_NAME}}'s profile, preferences, goals, and boundaries.
4. Follow folder-level `INDEX.md` files when retrieving knowledge. Do not scan the whole vault unless the indexes are insufficient or the user asks for a comprehensive search.
5. Read the most recent `chat-history/` entries for continuity with prior sessions. If beginning substantive work, run the `update` skill first so the indexes and this guidance reflect any file drift since the last reconciliation.

## Working Rules

- Treat personal information as private by default and keep {{USER_PREFERRED_NAME}} in control of consequential decisions.
- Preserve existing user content and inspect files before replacing or restructuring them.
- Store important everyday working information in `{{BRAIN_FOLDER}}/`.
- Store todos in `{{TODO_FILE}}` as Markdown checklist items.
- Add new todos with `- [ ]`, complete them with `- [x]`, and do not delete completed items unless {{USER_PREFERRED_NAME}} asks.
- Use the vault-local skills in `.agents/skills/` when their trigger conditions apply.
- Use `{{INSTALLED_TASK_SKILLS}}` when the corresponding workflow is requested.
- Use `$update` after substantial vault changes, when navigation may be stale, or near the end of a substantive session.
- The `update` skill also detects file drift since it last ran (content hashing → `.brain-state.json`) and records each run to `chat-history/`. Prefer running it at session start to catch up and at session end to snapshot.
- Treat `chat-history/` as the session journal (what past sessions did/decided) and `.brain-state.json` as a local system cache (do not hand-edit).

## Index Discipline

- Keep every user-knowledge folder's `INDEX.md` aligned with its immediate contents.
- Update the nearest index when a file or folder is created, renamed, moved, deleted, or materially changes purpose.
- Keep the root `INDEX.md` aligned with root knowledge files and top-level knowledge folders.
- Exclude `.obsidian/`, generated dependencies, caches, and skill implementation details from item-by-item knowledge indexing.

## Search Fallback

If the indexes are insufficient, stale, ambiguous, or the user requests a comprehensive search:

1. Search all relevant user-knowledge filenames and contents, using related terms and likely synonyms where useful.
2. Exclude `.obsidian/`, version-control data, caches, generated dependencies, and other application internals unless the request specifically concerns them.
3. Follow relevant links and inspect surrounding context rather than relying only on isolated search matches.
4. For a large vault or independent subject areas, use parallel agents when available and appropriate, then reconcile their findings.
5. Cite or identify the vault files supporting the result and distinguish conflicts or uncertainty.
6. If the search reveals missing or stale index entries, run or recommend `$update` after completing the user's retrieval task.

## Communication

- Lead with the clearest practical recommendation or outcome.
- Distinguish facts, assumptions, interpretations, and open questions.
- Prefer lightweight, maintainable systems over unnecessary complexity.
- Challenge ideas respectfully when doing so materially improves the result.
