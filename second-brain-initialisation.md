# Second Brain Initialisation

## Instructions for the agent

You are setting up an interactive, Obsidian-compatible Second Brain and AI agent environment in the folder from which the user launched you. The user should have installed Obsidian, the `Terminal` community plugin, and an AI coding agent (for example Codex, Claude Code, Gemini CLI, or OpenCode).

This document describes *what* to build, not the exact files and formats to use. Wherever a step involves a mechanism that differs between agent harnesses — the auto-loaded instructions file, or how user-invokable capabilities ("skills" / custom commands) are packaged — translate the intent into your own harness's native conventions rather than copying a fixed layout. You know your harness better than this document can. Concrete names such as `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `$update`, or `/update` appear only as illustrative hints.

This is an onboarding conversation, not a silent installation. Guide the user one stage at a time, explain what you are about to do, and wait for the user's answer whenever a choice or confirmation is required. Ask manageable questions rather than presenting a long questionnaire.

Do not assume that the current folder is empty. Before creating anything, inspect it for existing files without reading unrelated personal content. Never overwrite or substantially rewrite an existing file without showing the user what would change and obtaining permission. Keep all Second Brain files inside the selected root folder unless the user explicitly approves otherwise.

Use Markdown files that work normally in Obsidian. Use relative links inside the vault. Prefer concise, factual documents over large instruction files.

## Stage 1: Introductions

Begin with these words:

> Hi, what is your name? And what would you like to call me?
>
> Once we're done with introductions, we'll be installing a Second Brain AI agent environment on your computer, as designed by Yeo Yong Kiat.

Wait for both answers. Use the chosen names naturally during the rest of onboarding.

## Stage 2: Define the agent

Explain that the first step is to define how the agent should behave. Interview the user conversationally about:

- The role the agent should play and the work it should help with.
- Its identity and personality.
- Its tone, directness, formality, humour, and preferred level of detail.
- Whether it should act mainly as an assistant, adviser, collaborator, coach, or another role.
- Behaviours the user wants encouraged or avoided.
- Boundaries and any other behavioural preferences.

Ask one manageable question at a time and offer examples when the user is unsure. Summarise the proposed agent definition, invite corrections, and obtain confirmation.

Create `SOUL.md` from the confirmed definition. State in the file that it is the canonical source of truth for the agent's role, identity, personality, communication style, behaviour, and boundaries.

## Stage 3: Learn about the user

Explain that better user context makes the agent more useful, but every question is optional and the user can update the information later.

Conversationally ask for useful details such as:

- Preferred name and form of address.
- Location and time zone.
- Personal and professional roles.
- Current responsibilities and priorities.
- Interests, goals, routines, and working preferences.
- Important people or organisations.
- Anything else the agent should know to provide relevant assistance.

Do not pressure the user to disclose sensitive information. Record sensitive details only when the user explicitly wants them saved. Summarise the profile for correction and confirmation, then create `USER.md`. Include only confirmed facts, not assumptions.

## Stage 4: Choose the initial folder structure

Explain:

> I'll create four folders to organise your Second Brain:
>
> - `people/` — notes about people and relationships.
> - `ideas/` — thoughts, concepts, and possibilities worth developing.
> - `projects/` — active or completed pieces of work with defined outcomes.
> - `meetings/` — meeting notes, records, decisions, and action items.
>
> Would you like to keep this structure, rename any folders, add others, or remove any?

Wait for the answer before creating folders. If the user is unsure, recommend starting with the defaults because the structure can be changed later. Treat these as folders inside the selected Second Brain root, not absolute filesystem paths.

Create the confirmed folders.

## Stage 5: Create the indexes

Explain:

> We'll now create an `INDEX.md` in the root of your Second Brain and inside each folder we created.
>
> Each `INDEX.md` describes the purpose and contents of the folder it sits in. The root index maps the whole Second Brain, while each folder's index describes the files and subfolders in that area.
>
> These indexes let your agent find relevant information without loading the entire Second Brain into its context window. This conserves context and reduces mistaken or invented connections.

Create a root `INDEX.md` and an `INDEX.md` in every chosen top-level folder. Each index must:

- State the purpose of its parent folder.
- Describe the files and immediate subfolders within it.
- Use relative Markdown links where useful.
- Remain concise enough to serve as a navigation map.

With the default choices, the initial structure is:

```text
Second Brain/
├── AGENTS.md            # or your harness's equivalent auto-loaded file (CLAUDE.md, GEMINI.md, …)
├── SOUL.md
├── USER.md
├── INDEX.md
├── people/
│   └── INDEX.md
├── ideas/
│   └── INDEX.md
├── projects/
│   └── INDEX.md
└── meetings/
    └── INDEX.md
```

Adapt this structure if the user chose different folders.

## Stage 6: Create the harness instructions file

Identify the project-level instructions file that your harness automatically loads at the start of every session — for example `AGENTS.md` for Codex and OpenCode, `CLAUDE.md` for Claude Code, or `GEMINI.md` for Gemini CLI. If your harness has no such auto-loaded file, create `AGENTS.md` in the Second Brain root as a conventional fallback and tell the user how your harness discovers startup context.

Explain to the user which file you are using and that your harness loads it automatically at session start. Other Second Brain files are not automatically loaded merely because they exist, so this file will tell you what startup context to read and how to navigate the vault.

Create or update that file so it contains, at minimum, the following operating rules in language adapted to the user's chosen names and folders:

### Startup context

Before doing any work, read these files from the Second Brain root:

1. `SOUL.md` — the canonical definition of the agent's role, identity, personality, communication style, behavioural preferences, and boundaries.
2. `USER.md` — confirmed information about the user, including relevant background, preferences, priorities, and context. Never treat assumptions as facts.
3. The top-level `INDEX.md` — the map of the Second Brain's root folders and files.

If later stages create `BRAIN.md` or `TODO.md`, add them to this startup list as described below.

### Navigation and maintenance

- Start with the root `INDEX.md`, then follow folder-level indexes to locate relevant information.
- Practise progressive disclosure: load only the files needed for the current task, not the entire Second Brain.
- Keep every `INDEX.md` synchronised when files or folders are created, moved, renamed, or removed.
- Keep `SOUL.md` and `USER.md` concise living documents.
- Update `SOUL.md` when the user confirms a durable change to the agent's behaviour.
- Update `USER.md` when the user confirms durable new information or changes a preference.
- Never record uncertain assumptions as facts.
- Save sensitive information only when the user explicitly wants it recorded.
- Work only inside the selected Second Brain folder unless the user explicitly authorises a broader scope.

Tell the user to launch future agent sessions from the Second Brain root so that this instructions file is reliably discovered.

## Stage 7: Create the `update` capability

Tell the user:

> Your Second Brain's core structure is ready.
>
> Next, I'll create an `update` capability. Run it (for example `$update` or `/update`, depending on your agent) whenever you feel the contents or structure of your Second Brain have changed significantly—for example, after adding, moving, renaming, or deleting files and folders.
>
> It will refresh the harness instructions file, the root `INDEX.md`, and all folder-level `INDEX.md` files. It will preserve your notes and documents and will not rewrite `SOUL.md`, `USER.md`, or ordinary content files unless you explicitly ask it to.

Package this as a user-invokable capability named `update`, using your harness's native mechanism for on-demand capabilities — a skill, a custom slash-command, or the closest equivalent (for example a skill at `.agents/skills/update/SKILL.md` or `.claude/skills/update/SKILL.md`, or a custom command). Give it whatever metadata/frontmatter your harness requires and a clear description that triggers when the user asks to refresh or reconcile the Second Brain's indexes and operating map. If your harness offers no such mechanism, save this procedure as a vault document and tell the user they can run it by asking you to update the Second Brain.

The capability must instruct you to:

1. Read the harness instructions file, `SOUL.md`, `USER.md`, and the root `INDEX.md`.
2. Inspect the current folder and file structure, excluding hidden system metadata and irrelevant generated files.
3. Read the existing nested `INDEX.md` files.
4. Identify additions, removals, moves, renames, and inaccurate descriptions.
5. Update only affected indexes.
6. Update the harness instructions file only when the system's structure or operating rules have genuinely changed.
7. Keep descriptions concise and factual.
8. Inspect a file when its purpose is unclear; do not infer its purpose from its filename alone. Ask the user if uncertainty remains.
9. Preserve `SOUL.md`, `USER.md`, `BRAIN.md`, `TODO.md`, generated `SUMMARY.md` files, `.summary-manifest.json` files, and ordinary content unless the user explicitly requests changes to them.
10. Report which files changed and briefly summarise the updates.

Validate that your harness can discover the new capability. If it does not appear immediately, tell the user that restarting the agent may be necessary.

## Stage 8: Create the `summarise` capability

Tell the user:

> I'll also create a `summarise` capability. Point it at a folder and it will recursively read the documents in that folder and its subfolders, then create or refresh a context-sensitive `SUMMARY.md` at the root of the selected folder.
>
> The summary will adapt to the material. A person's folder may produce a sourced profile; a meetings folder may produce a brief covering purpose, developments, decisions, actions, and workstreams. It will also act as an Obsidian-compatible wiki, grouping important themes and linking back to the source notes.
>
> Later runs will detect what was added, edited, moved, or removed and reconcile the summary without needlessly rereading unchanged files.

Package this as a user-invokable capability named `summarise`, using your harness's native mechanism for on-demand capabilities — a skill, a custom slash-command, or the closest equivalent (for example a skill at `.agents/skills/summarise/SKILL.md` or `.claude/skills/summarise/SKILL.md`). If your harness provides a skill-creation or scaffolding workflow, use it, and validate the result with whatever validator your harness offers. Give the capability whatever metadata/frontmatter your harness requires, and a description that triggers when the user asks to summarise, synthesise, aggregate, or refresh knowledge from a specified folder. If your harness offers no such mechanism, save this procedure as a vault document and tell the user how to invoke it by asking.

The capability must instruct you to:

1. Require a specific target folder. Resolve it inside the Second Brain root and ask the user when the target is missing or ambiguous.
2. Work only inside that target folder. Never follow symlinks or links outside it.
3. Recursively inventory supported source documents in the target folder and all descendant folders. Include readable text and Markdown by default. Use an applicable installed document skill for formats such as PDF, Word, presentations, or spreadsheets when available; otherwise report unsupported files rather than pretending to have read them.
4. Exclude the generated `SUMMARY.md`, the skill's own tracking metadata, hidden system metadata, temporary files, generated files, and irrelevant indexes unless an index contains substantive source information.
5. On the first run, read every supported source document. Determine the folder's apparent subject and purpose from evidence in its contents, not its name alone.
6. Create `SUMMARY.md` in the target folder. Adapt its framing and headings to the evidence. For example:
   - For a person's folder, create a sourced profile based only on recorded facts and observations.
   - For a meeting series or meeting folder, cover the purpose, what has transpired, decisions, actions, unresolved questions, and key workstreams.
   - For a project, cover its purpose, status, milestones, decisions, risks, dependencies, and next actions where supported.
   - For other folders, choose a concise structure that best represents the material.
7. Make `SUMMARY.md` useful as an Obsidian wiki. Aggregate recurring themes and relationships, and add relative Obsidian wikilinks such as `[[subfolder/note]]` to supporting source notes. Prefer links at the relevant claim or section over a disconnected link dump. Use aliases when they improve readability.
8. Distinguish facts, interpretations, decisions, actions, open questions, and conflicts where relevant. Never invent information or silently resolve contradictions.
9. Preserve useful existing synthesis and user-authored content in `SUMMARY.md`. Reconcile it with the evidence rather than blindly replacing the whole file. If provenance is unclear or a rewrite would discard material, show the proposed change and obtain permission.
10. Maintain a compact machine-readable manifest at `.summary-manifest.json` in the target folder. Record the skill schema version and, for each successfully ingested source, its relative path and SHA-256 content hash. Do not use modification time alone as proof that content is unchanged.
11. On later runs, compare the current recursive inventory against the manifest to identify added, changed, moved, and removed sources. A matching hash at a new path should be treated as a likely move. Read all added or changed sources; reuse the existing synthesis for unchanged sources; revise or remove claims whose only support was removed or superseded. Perform a full rebuild if the manifest is missing, corrupt, incompatible, or inconsistent with `SUMMARY.md`.
12. Update the manifest only after `SUMMARY.md` has been written successfully. Use an atomic replacement where practical so an interrupted run does not leave the summary and manifest out of sync.
13. Update the target folder's `INDEX.md`, if present, so it links to `SUMMARY.md`. Update ancestor indexes only when their immediate contents or descriptions genuinely need to change.
14. Report the target folder; the sources added, changed, moved, removed, unchanged, skipped, or unsupported; whether the run was incremental or a full rebuild; and which files changed.

Use relative paths in the manifest and deterministic ordering to keep diffs stable. Do not place content hashes or maintenance logs in the reader-facing `SUMMARY.md`; the separate manifest is the efficient ingestion record. Treat the manifest as generated state and preserve it during ordinary note maintenance.

Give examples (using whatever invocation your harness uses, for example `$summarise` or `/summarise`):

```text
summarise people/Sarah
```

```text
summarise meetings/weekly-programme-review
```

Validate that your harness can discover the `summarise` capability. If it does not appear immediately, tell the user that restarting the agent from the Second Brain root may be necessary.

## Stage 9: Create the `reset` capability

Tell the user:

> I'll create a `reset` capability for a factory reset of this Second Brain.
>
> This is deliberately destructive. When run, it removes everything inside the vault—including notes, indexes, configuration, generated summaries, and vault-scoped custom capabilities—except `second-brain-initialisation.md`. It will always show you the exact vault and deletion scope and require fresh confirmation before proceeding.
>
> It will not delete personal or system-wide skills stored outside this vault.

Package this as a user-invokable capability named `reset`, using your harness's native mechanism (a skill, custom command, or equivalent; for example a skill at `.agents/skills/reset/SKILL.md` or `.claude/skills/reset/SKILL.md`). If your harness provides a scaffolding workflow and validator, use them. Give it whatever metadata/frontmatter your harness requires, and a description that clearly triggers only when the user asks to factory-reset, erase, or reinitialise the current Second Brain vault. Do not make it implicitly attractive for ordinary cleanup.

The capability must instruct you to:

1. Treat invocation as a request to prepare a reset, not as confirmation to delete.
2. Resolve the canonical absolute path of the Second Brain root containing both the applicable root harness instructions file and `second-brain-initialisation.md`. Never accept `/`, the user's home directory, `$HOME`, `~`, an unresolved environment variable, a glob, the current directory by assumption alone, or any path outside the active Second Brain as the reset target.
3. Verify that `second-brain-initialisation.md` is a regular file inside that exact root and is not a symlink. Stop if the vault root or preserved file cannot be established unambiguously.
4. Build a read-only inventory of every immediate child of the vault root, including hidden entries. The only preserved entry is the exact root file `second-brain-initialisation.md`. Everything else is in scope, including `.obsidian/`, the harness instructions file, `SOUL.md`, `USER.md`, `INDEX.md`, `TODO.md`, `BRAIN.md`, all note folders, summaries, manifests, and any harness skill/command directory containing `reset` and other vault-scoped custom capabilities.
5. Do not follow symlinks. List a symlink itself for removal, never its target. Do not cross filesystem boundaries while resolving deletion targets.
6. Present the user with:
   - The canonical absolute vault path.
   - The exact file that will be preserved.
   - A concise inventory of entries to be removed, including hidden entries and vault-scoped capabilities.
   - A clear warning that the operation removes the agent profile, user profile, notes, tasks, configuration, indexes, generated summaries, and all custom capabilities stored inside the vault.
   - Whether the proposed removal method is recoverable through the operating system's Trash or Recycle Bin.
7. Require a fresh, explicit confirmation after showing that inventory. Ask the user to type `RESET <canonical-vault-path>`. A previous request to reset, a generic "yes", skill selection, or confirmation given before the inventory does not count. If the phrase or path does not match exactly, do nothing.
8. After confirmation, recompute the immediate-child inventory and compare it with the preview. If new entries appeared or the preserved file changed identity, stop and request confirmation again with an updated inventory.
9. Prefer moving each in-scope immediate child to the operating system's Trash or Recycle Bin using explicit resolved paths, preserving recoverability when supported. Never use a broad recursive command against the vault root, home directory, or a variable. If recoverable removal is unavailable, explain that permanent deletion would be required and obtain a second explicit confirmation before doing it.
10. Remove every confirmed immediate child except `second-brain-initialisation.md`. Because the `reset` capability's own directory is itself removed, finish the active in-memory workflow without attempting to read the deleted capability again.
11. Verify that the vault root still exists and contains exactly one entry: the regular file `second-brain-initialisation.md`. If anything else remains or the preserved file is missing, report the discrepancy accurately and do not claim success.
12. Report what was removed, whether it is recoverable, and that vault-scoped custom capabilities were removed. Tell the user they can run `second-brain-initialisation.md` again to reinitialise the vault. Do not automatically begin onboarding unless the user asks.

The reset must remain confined to the selected vault. Never remove global or personal skills, plugins, connectors, credentials, or configuration stored outside it, even if the user previously used them with this Second Brain.

Give an example:

```text
reset factory-reset this Second Brain
```

Do not test the capability by performing a real reset during onboarding. Validate its structure and discovery only. If it does not appear immediately, tell the user that restarting the agent from the Second Brain root may be necessary.

## Stage 10: Explain how skills work

Adapt this explanation to how your harness actually exposes user-invokable capabilities, then tell the user in your own words:

> Skills (or custom commands) are reusable workflows for specialised tasks. Explain to the user how to invoke and browse them in this harness — for example typing `$` in Codex or `/` in Claude Code and Gemini CLI to list what is available, then selecting one and describing what you want it to do.
>
> For example:
>
> ```text
> update refresh the indexes for my Second Brain
> ```
>
> Your harness may also ship built-in capabilities — for creating presentations, reading and editing PDFs, working with spreadsheets, browsing websites, or generating images. Tell the user which of these actually exist in their environment rather than promising a fixed list. Alongside those, they now have:
>
> - `update` — refresh the harness instructions file and all indexes.
> - `summarise` — create or incrementally refresh a folder-level knowledge summary.
> - `reset` — factory-reset this vault after a separate destructive-action confirmation.
>
> Only installed and available capabilities will appear. If one the user wants is unavailable, offer to help find or install an appropriate one.

Invite the user to browse their capabilities using their harness's mechanism and confirm that `update`, `summarise`, and `reset` appear. Do not require them to run any of these yet.

### Explain scheduled tasks

After explaining skills, tell the user:

> You may also be able to schedule tasks to run automatically. For example, you could schedule a weekly `update`, generate a morning deadline briefing, perform recurring research, or run another stable workflow at a chosen time.
>
> Whether and how scheduling works depends on your agent and environment — some harnesses schedule from a companion desktop app, others from the CLI or the operating system's scheduler. For any file-based schedule, the computer must be powered on and this folder available when the task is due. I can help you prepare and test the task's prompt or capability here before you schedule it.
>
> Scheduled tasks run unattended, so start with the narrowest file and network permissions that allow the task to work. Review the first few runs before relying on the automation.

Give a few relevant examples:

```text
Every Friday at 5 pm, run update for this Second Brain and report what changed.
```

```text
Every weekday morning, read TODO.md and brief me on overdue and upcoming work.
```

```text
On the first working day of each month, review my active project indexes and identify projects with no recent update.
```

Check whether scheduled tasks are supported in the user's current environment before offering to configure one. Do not claim that a schedule has been created unless it is visible and validated in the relevant scheduling interface.

## Stage 11: Explain how to add information

Tell the user:

> You can now start using your Second Brain. Simply tell me what you're thinking in ordinary language. You do not need to format it first. I can help clarify and organise it, then save it as a Markdown (`.md`) file in the appropriate folder.

Give examples adapted to the user's chosen folders, such as:

```text
I have an idea for improving staff onboarding. Help me think it through and save it under ideas.
```

```text
I met Sarah today. Create a note about her under people.
```

```text
Create a project note for the office relocation and include the next actions we discussed.
```

Explain that the user is not limited to the initial folders. They can ask you to create a new folder at any time, for example:

```text
Create a folder called reading for my book notes.
```

When creating a new folder, also create its `INDEX.md` and update the parent index. Confirm the intended filename or location when it is ambiguous.

## Stage 12: Explain how to retrieve information

Tell the user:

> You can retrieve information from your Second Brain by asking me questions in ordinary language. You do not need to remember filenames or folder locations.

Give examples:

```text
What ideas have I recorded about staff onboarding?
```

```text
What do I know about Sarah?
```

```text
Summarise the current status of the office relocation project.
```

```text
Find my notes related to artificial intelligence and healthcare.
```

Explain that you will start from the root index, follow the relevant folder indexes, and open only the files needed. You can find, summarise, compare, trace, list, and identify gaps or contradictions across notes, and should identify your source files when useful.

Also tell the user:

> If I cannot find the file or information you mean, you can guide me with a filename, folder, phrase, person, project, approximate date, or any other clue. This usually won't be necessary because the indexes help me navigate, but clues are useful when a note has an unclear name, has not yet been indexed, or could match several files.
>
> If I still cannot locate it, I'll explain where I searched and ask for another clue rather than pretending the information does not exist.

Remind the user to run `update` after substantial structural or content changes.

## Stage 13: Offer a CEO or Staffer Second Brain

Explain that the user may optionally add a structured assessment framework:

> **CEO Second Brain**
>
> This assesses issues using the CEO's own thinking and decision-making framework. It aims to reflect how the CEO weighs priorities, opportunities, risks, trade-offs, stakeholders, and organisational outcomes.
>
> **Staffer Second Brain**
>
> This approaches issues as a capable staff officer preparing analysis for a CEO or senior decision-maker. It structures the issue clearly, identifies relevant considerations, develops options, assesses trade-offs, and makes an actionable recommendation.
>
> Which would you like to develop: a CEO Second Brain, a Staffer Second Brain, both, or neither?

If the user chooses neither, skip to Stage 16.

## Stage 14: Create `BRAIN.md`

After the user selects CEO, Staffer, or both, explain that `BRAIN.md` will contain the relevant thinking and assessment framework. Offer two ways to populate it:

1. **Write the framework directly.** The user describes the principles, questions, criteria, preferences, and decision-making process that the Second Brain should use.
2. **Synthesise it from source material—the typical approach.** The user supplies documents that demonstrate the relevant person's thinking, such as speeches, emails, meeting notes, decision papers, annotations, strategy documents, interviews, or previous assessments. You spot recurring patterns and draft a framework.

Ask which approach the user prefers.

For direct authoring, help the user structure and refine the framework, then obtain confirmation.

For document synthesis:

- Ask the user to identify or provide the source documents.
- Remind them to use only material they are authorised to process and to remove sensitive information where necessary.
- Distinguish direct evidence from interpretation.
- Identify recurring priorities, questions, trade-offs, decision criteria, and communication preferences.
- Avoid presenting speculative personality judgements as facts.
- Cite the supporting source files for major inferences where practical.
- Present the draft for correction and confirmation.

Create `BRAIN.md` only from the confirmed framework. If both modes were selected, give the CEO and Staffer frameworks clearly separated sections in the same file.

Add `BRAIN.md` to the root `INDEX.md`. Update the harness instructions file so the startup list includes:

```md
4. `BRAIN.md` — the confirmed thinking and assessment framework for the chosen Second Brain. Apply it when the user asks for analysis, assessment, options, advice, or recommendations. Distinguish conclusions supported by the framework from additional reasoning.
```

Adjust numbering if other startup files already exist.

## Stage 15: Create the `assess` capability

Tell the user:

> I'll now create an `assess` capability. Use it when you want your Second Brain to examine a situation, document, idea, proposal, decision, or person through the framework in `BRAIN.md`.

Give examples (invoking however your harness does, for example `$assess` or `/assess`):

```text
assess this policy proposal
```

```text
assess whether we should proceed with this project
```

```text
assess this meeting paper and identify what the CEO is likely to question
```

```text
assess my working relationship with this stakeholder
```

Package this as a user-invokable capability named `assess`, using your harness's native mechanism (for example a skill at `.agents/skills/assess/SKILL.md` or `.claude/skills/assess/SKILL.md`, or a custom command), with whatever metadata/frontmatter your harness requires and a description that clearly triggers for assessments using `BRAIN.md`.

The capability must instruct you to:

1. Load `BRAIN.md`.
2. Clarify the purpose or decision when it is unclear.
3. Locate or request the relevant evidence.
4. Apply the chosen framework systematically.
5. Separate facts, assumptions, interpretations, and missing information.
6. Identify risks, opportunities, trade-offs, stakeholder considerations, and unanswered questions.
7. Present options where appropriate.
8. Give a clear conclusion or recommendation when supported.
9. Explain which parts of `BRAIN.md` informed the assessment.
10. Save the result as Markdown only when the user asks.

For assessments of people, focus on observable conduct, stated positions, incentives, working relationships, and available evidence. Do not infer sensitive traits, diagnose people, or present speculation as fact.

If `BRAIN.md` contains both CEO and Staffer frameworks, ask which one to apply unless the user requests a comparison. For a comparison, keep the two assessments clearly separated.

## Stage 16: Offer optional capabilities for government officers

Explain:

> Government officers commonly find the following capabilities useful:
>
> 1. **Local audio transcription** — transcribe an audio recording using a speech-to-text model running locally on your computer.
> 2. **Notes of Meeting formatter** — turn a raw meeting transcript into structured, government-style Notes of Meeting (NOM).
> 3. **Wide internet research** — research a topic across multiple internet sources and produce a structured, cited report.
> 4. **Google tools integration** — connect the agent to services such as Gmail, Google Calendar, Google Drive, and, where supported, NotebookLM.
> 5. **Website scraping** — extract information from a specified website for analysis or storage.
> 6. **Deadline briefing** — read your to-do list and brief you on upcoming deadlines, overdue work, and important priorities.
>
> Which would you like me to install? You can choose any number, choose all of them, or skip this step. You can add more skills later.

Wait for the user's selection. For every selected capability, explain dependencies, permissions, privacy implications, and setup requirements. Search for reputable available skills or connectors and verify their quality and source before recommending them. Show the proposed installation and obtain permission before downloading software, changing configuration outside the Second Brain, or connecting an external account. Install and validate approved capabilities individually. Report anything unavailable or requiring manual setup.

Warn government users not to send classified, sensitive, or protected information to external services unless their organisation has authorised those services. Prefer local processing for sensitive recordings and documents.

### Option 1: Local audio transcription

Before recommending a setup, inspect relevant system information:

- Operating system and version.
- Processor architecture.
- Available memory and storage.
- GPU or neural acceleration hardware, where detectable.
- Existing dependencies such as Python, `ffmpeg`, and local speech-to-text tools.

Explain the findings and recommend a suitable model size and setup. If local transcription is impractical, explain why and offer alternatives without installing them automatically.

### Option 2: Notes of Meeting formatter

Ask the user for one or more examples of government-style Notes of Meeting that reflect the desired output. A matching raw transcript is helpful but not essential. Ask the user to remove or anonymise sensitive information unless its use is authorised.

Study the examples for structure, tone, headings, attribution, action-item formatting, and level of detail. Summarise the inferred conventions and obtain confirmation before creating the skill. The skill must not invent decisions, statements, attendees, or action items.

### Option 3: Wide internet research

Find or create a workflow that researches across multiple credible sources, distinguishes evidence from inference, accounts for publication dates and source quality, and provides direct citations. Explain that internet access may require permission and that internal or sensitive material must not be sent to public services without authorisation.

### Option 4: Google tools integration

Explain which integrations are actually supported in the current agent environment. Treat Gmail, Google Calendar, Google Drive, and NotebookLM as separate capabilities when necessary. Explain authentication scopes before requesting authorisation, request the least access needed, and never ask the user to paste secrets or tokens into chat.

### Option 5: Website scraping

Explain:

> Website scraping is rarely universal. Different websites use different page structures, authentication systems, pagination methods, and anti-automation controls. A scraper that works for one website may require changes for another, so the skill may need repeated testing and iteration across the websites you intend to use before it becomes stable.
>
> Scraping must respect applicable laws, organisational policy, access controls, robots directives where applicable, and the website's terms.

Ask which website and information the user wants to scrape before designing or adapting the skill. Do not bypass access controls or anti-bot protections.

### Option 6: Deadline briefing

Explain:

> This capability requires a `TODO.md` file in the root of your Second Brain. It will be the source of truth for your tasks, deadlines, and priorities.
>
> I'll add `TODO.md` to the root `INDEX.md` so it appears in the map of your Second Brain. I'll also add it to the harness instructions file so I read it at the beginning of every session and can proactively alert you to important deadlines.
>
> You will need to keep `TODO.md` current. I can help update it, but I cannot remind you about tasks or deadlines that have not been recorded.

If selected:

1. Create a concise `TODO.md` template that supports task status, deadlines, and priority without imposing unnecessary complexity.
2. Ask whether the user wants to add any current tasks and deadlines.
3. Add this relative link and description to the root `INDEX.md`: `TODO.md` is the living source of truth for current tasks, deadlines, and priorities.
4. Add `TODO.md` to the startup list in the harness instructions file, adjusting numbering as necessary. Instruct the agent to read it at the start of every session, draw attention to overdue tasks, approaching deadlines, and important deliverables when relevant, and never invent urgency or dates.
5. Create the deadline-briefing capability. It must distinguish overdue, upcoming, undated, completed, and high-priority tasks without inventing missing dates.
6. Validate the workflow with the user.

If the user does not select this option, do not create `TODO.md` or add it to startup context.

## Stage 17: Finish onboarding

Review the completed environment and verify that:

- All selected folders exist.
- Every selected folder has an accurate `INDEX.md`.
- The root `INDEX.md` maps the current structure.
- The harness instructions file references all applicable startup files.
- `SOUL.md` and `USER.md` contain only confirmed information.
- `update` is installed and discoverable.
- `summarise` is installed, validated, and discoverable.
- `reset` is installed, validated, and discoverable without executing a real reset.
- `BRAIN.md` and `assess` exist if the user selected a CEO or Staffer Second Brain.
- `TODO.md` and its briefing capability exist if the user selected deadline briefing.
- Every optional capability was either validated or clearly reported as pending.

Give the user a concise summary of what was created, where it lives, how to start future sessions, and the most useful first commands. Remind them that they can speak naturally, browse their capabilities using their harness's mechanism, run `update` after meaningful structural changes, run `summarise` to build or refresh a folder-level synthesis, and ask you for help whenever navigation or retrieval fails.

Do not claim that a tool, skill, connector, account, or workflow works unless you actually validated it.
