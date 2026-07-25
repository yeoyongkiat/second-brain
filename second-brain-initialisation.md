# Second Brain Initialisation

## Purpose

This file is designed to be run by Codex inside an empty Obsidian vault. Codex should guide the user through installing and configuring a personal second brain using short, approachable prompts.

Treat this document as an installation procedure, not as reference material. Work through one step at a time, wait for the user's response whenever a decision is required, create the relevant files in the vault, and confirm the result before continuing.

## Guidance for Codex

- Begin by briefly explaining the current step and why it matters.
- Ask focused questions in plain language.
- Do not assume the user already understands knowledge-management terminology.
- Offer a sensible recommendation when choices may be unfamiliar.
- Prefer a small, useful system over a complicated folder structure.
- Preserve the user's wording and preferences where practical.
- Never overwrite an existing file without inspecting it and obtaining confirmation when the changes would replace user content.
- At the end of each step, summarize what was decided and which files were created or changed.
- Complete only one guided step at a time unless the user explicitly asks to proceed further.

## Step 0: Check Python and Anaconda Prerequisites

Before creating the second brain, check whether Python and Anaconda are available. Explain that Anaconda Distribution already includes Python, so a successful Anaconda installation satisfies both requirements; do not install a second standalone copy of Python unnecessarily.

### Detect the existing environment

Use read-only checks appropriate to the operating system:

```text
python --version
python3 --version
py --version
conda --version
conda info --base
```

Also identify the operating system, processor architecture, available disk space, and whether the computer is managed by an employer. Do not interpret a command-not-found result as proof that software is absent until common installation locations or the platform's application inventory have been checked; the executable may simply be missing from `PATH`.

Apply these outcomes:

- **Anaconda and Python work:** Report their versions and make no changes.
- **Anaconda works but `python` is not visible in the current shell:** Locate the Anaconda Python executable, activate or initialize conda with the user's permission, open a fresh shell if needed, and verify again.
- **Standalone Python works but Anaconda is absent:** Explain that Anaconda will add its own managed Python environment. Ask whether the user wants the full Anaconda Distribution before installing it.
- **Neither is installed:** Recommend installing Anaconda Distribution, which supplies both requirements in one installation.
- **An incompatible, damaged, or ambiguous installation exists:** Do not overwrite or uninstall it automatically. Explain the finding and ask how the user wants to proceed.

### Obtain consent before installation

Installing Anaconda downloads a large third-party package, accepts vendor terms, writes outside the vault, and may modify shell configuration. Before doing so:

1. Explain what will be downloaded and changed.
2. Show the intended edition, installer source, target directory, and installation command.
3. Mention that Anaconda's licence terms may differ for organisational or commercial use and direct the user to review the current terms.
4. Ask for explicit approval and obtain any execution or administrator permission required by the environment.
5. Respect organisational device-management and security policies. If the device is managed, prefer the organisation's approved software channel or ask its administrator.

Do not disable antivirus software, remove existing Python installations, alter global `PATH`, accept legal terms on the user's behalf, or perform an all-users installation without explicit direction.

### Install from current official sources

Use the current installer and commands from the official [Anaconda download page](https://www.anaconda.com/download) and [Anaconda installation documentation](https://www.anaconda.com/docs/getting-started/anaconda/install). Do not hardcode a version from this document because installer versions and supported platforms change.

Match the installer to the detected operating system and architecture. Verify that the computer meets the current [Anaconda system requirements](https://www.anaconda.com/docs/getting-started/anaconda/system-requirements), including available disk space. Download only from an official Anaconda domain. Where checksums are published, verify the installer's SHA-256 digest before running it.

Prefer a per-user installation unless the user explicitly needs and authorizes a system-wide installation. On Windows, follow Anaconda's guidance not to add Anaconda manually to the global `PATH`; use Anaconda Prompt or properly initialize the user's shell. On macOS and Linux, explain and obtain permission before running `conda init`, because it modifies shell startup files.

Use interactive installation when the user needs to review installer choices or legal terms. Use the official [silent-install procedure](https://www.anaconda.com/docs/getting-started/advanced-install/silent-mode) only after the user has approved every relevant option, destination, and terms-handling requirement.

### Verify and record the result

After installation or activation, start a fresh shell where necessary and verify:

```text
conda --version
conda info --base
python --version
python -c "import sys; print(sys.executable)"
```

Confirm that Python resolves to the intended Anaconda environment. Report the installed versions and base path. If verification fails, diagnose the installation without repeatedly reinstalling or deleting existing environments.

This prerequisite step must remain skippable when the requirements are already met or when the user declines installation. A declined or policy-blocked installation should not prevent steps that do not require Python; explain which later validation or automation features may be unavailable.

## Step 1: Define the Agent's Identity

The first step establishes the identity and behavioural foundation of the user's second-brain agent. The result must be stored in a file named `SOUL.md` at the root of the Obsidian vault.

Guide the user in defining:

1. **Name** — What should the agent be called?
2. **Role** — What function should the agent perform in the user's life or work?
3. **Organisation** — What personal organisation, studio, practice, household, or other context does the agent work within?
4. **Personality** — How should the agent think, communicate, collaborate, and challenge the user?

The user may answer all four questions directly. If they are unsure, propose a cohesive default identity and invite them to accept or modify it. Keep the process conversational rather than presenting a long questionnaire.

Once the identity is agreed, create `SOUL.md` with this structure:

```markdown
# SOUL

## Name

[Agent name]

## Role

[Agent role]

## Organisation

[Organisation name and a brief description of its purpose]

## Personality

[A concise description of the agent's temperament and communication style]

## Behavioural Principles

- [Concrete principle derived from the chosen role and personality]
- [Additional principles as appropriate]
```

The behavioural principles should translate the identity into practical instructions. Include principles covering user agency, clarity, uncertainty, privacy, maintainability, and respectful challenge where these fit the user's preferences.

Before creating the file, check whether `SOUL.md` already exists. If it contains user content, show or summarize the relevant content and ask whether the user wants to keep it, revise it, or replace it.

After saving `SOUL.md`, briefly recap the chosen name, role, organisation, and personality. Then tell the user that the identity step is complete and wait for permission to begin the next installation step.

### Example outcome from the design session

The initial design session produced the following example:

- **Name:** Atlas
- **Role:** A second-brain partner, knowledge steward, thinking companion, and practical operator.
- **Organisation:** The Observatory, Wilson's personal knowledge organisation for clear thinking, continuous learning, sound decisions, and meaningful execution.
- **Personality:** Calm, curious, thoughtful, candid, practical, organised, and quietly opinionated.

This is an example only. A fresh installation should guide its user to create an identity suited to them rather than silently applying this example.

## Step 2: Define the User's Identity

The second step gives the agent enough context to support the user appropriately. Store the resulting profile in `USER.md` at the root of the Obsidian vault.

Ask the user for:

1. **Name** — Their name and what they prefer to be called.
2. **Role** — Their role and main areas of responsibility.
3. **Organisation** — The organisation, business, household, personal practice, or other context in which they work.
4. **Work and interests** — The projects, domains, and subjects that matter to them.
5. **Goals** — What they want their second brain to help them accomplish.
6. **Working preferences** — How they prefer to think, decide, learn, organise work, communicate, and receive feedback.
7. **Boundaries** — Information or actions that should be treated carefully.

Keep the questions approachable and allow brief answers. Ask follow-up questions only when an omission would materially weaken the profile.

If the user asks Codex to invent a profile, create a conservative working profile using only known details and clearly labelled assumptions. Do not invent sensitive biographical information, employment, credentials, relationships, health details, or firm commitments. Mark the profile as provisional and easy to revise.

Once enough context is available, create `USER.md` with this structure:

```markdown
# USER

## Name

[Full name, if supplied]

Preferred name: [Preferred name]

## Role

[Role and responsibilities]

## Organisation

[Organisation and its context]

## Work and Interests

- [Relevant domain, project, or interest]

## Goals

- [Goal for the user's work or second brain]

## Working Preferences

- [Practical collaboration preference]

## Communication Style

[How the agent should communicate with the user]

## Boundaries

- [Privacy, autonomy, or decision-making boundary]

## Profile Status

[Whether this profile is confirmed or provisional]
```

Before creating the file, check whether `USER.md` already exists. If it contains user content, show or summarize the relevant content and ask whether the user wants to keep it, revise it, or replace it.

After saving the file, recap the key details and explicitly invite corrections. Then tell the user that the user-identity step is complete and wait for permission to begin the next installation step.

### Example outcome from the design session

The initial design session used a provisional profile for Wilson:

- **Role:** Independent builder and knowledge worker
- **Organisation:** The Observatory, a personal knowledge and operations practice
- **Focus:** Digital projects, personal systems, learning, writing, and experimentation
- **Preferences:** Clear recommendations, lightweight systems, explicit decisions, respectful challenge, and minimal jargon

This example should not be silently reused in a fresh installation. It demonstrates how to create a useful but conservative profile when the user asks Codex to supply the details.

## Step 3: Create the Working Brain and Todo List

Create a folder named `brain` at the root of the Obsidian vault. Explain to the user that this folder simulates their working brain: it holds important everyday information that the agent may need to support their thinking and work.

Suitable contents include:

- Todos and active responsibilities
- The user's thinking and decision-making frameworks
- Operating principles and working methods
- Frequently needed professional context
- Other durable, practical information relevant to the user's line of work

The folder is for useful working knowledge, not Obsidian configuration, generated files, or an indiscriminate archive. Its contents should evolve with the user's real needs.

Inside `brain`, create a note named `my-todos.md` as the first working-brain component.

This note is the user's central todo list. Whenever the user tells the agent to add or remember a todo, store it in this file as a Markdown checklist item:

```markdown
- [ ] An incomplete todo
- [x] A completed todo
```

Use this initial structure:

```markdown
# My Todos

Add every todo the user gives the agent as a Markdown checklist item.

## Todos
```

When maintaining the list:

- Add new items beneath `## Todos` using `- [ ]`.
- Preserve the user's intended wording while making the action clear.
- Do not remove completed items unless the user asks.
- Mark an item complete using `- [x]` when the user says it is done.
- Mark it incomplete again using `- [ ]` when the user asks to reopen it.
- If the requested item could match multiple existing todos, ask which one the user means before changing it.
- Avoid silently creating duplicate items when an equivalent todo already exists.

Also create `brain/INDEX.md`. Describe the working brain's purpose and summarize every file and immediate subfolder it contains. Update this index as the brain grows so Codex can discover the user's operational knowledge efficiently.

Before creating the folder or notes, check whether they already exist. Preserve existing content and merge the initial structure only where needed.

After creation, recap the purpose of the `brain` folder, tell the user where the todo note is located, and explain that they can ask the agent to add, complete, reopen, or review todos. Then wait for permission to begin the next installation step.

## Step 4: Create the Vault Indexing Structure

Build a hierarchical map that helps Codex navigate the vault efficiently without scanning every file. Create `INDEX.md` at the vault root and an `INDEX.md` inside every user-knowledge folder.

The root `INDEX.md` must:

- Explain that it is the entry point for vault navigation.
- List every user-knowledge file in the root with a concise description of its purpose and contents.
- List every root-level knowledge folder with a concise description and a link to its own `INDEX.md`.
- Explain how Codex should traverse nested indexes.
- State when the index must be updated.

Each folder-level `INDEX.md` must:

- Describe the folder's purpose.
- Summarize and link to its files and immediate subfolders.
- Direct Codex to nested `INDEX.md` files where applicable.
- Explain any folder-specific retrieval or maintenance rules.
- State when that index must be updated.

Use Obsidian links for knowledge notes where practical:

```markdown
- [[SOUL]] — Defines the agent's identity and behaviour.
- [[brain/INDEX|brain]] — Contains the user's personal operational notes, including the actionable checklist.
```

Use this navigation procedure:

1. Read the root `INDEX.md`.
2. Match the user's request to the most relevant file or folder description.
3. When entering a knowledge folder, read its `INDEX.md` before inspecting its contents.
4. Continue through nested indexes until the likely source files are identified.
5. Use a broader search only when the indexes are missing, stale, ambiguous, or insufficient.
6. For a large vault or a request spanning several independent branches, Codex may delegate separate indexed branches to multiple agents for parallel searching, provided multi-agent work is available and appropriate.
7. Combine the results, resolve overlaps, and report which source files supported the answer.

Treat `.obsidian/`, plugin dependencies, caches, generated files, and other application internals as system data rather than user knowledge. Mention relevant system folders at the root when useful, but do not create or maintain knowledge indexes throughout generated or third-party directory trees.

Before creating an index, inspect the relevant directory and any existing `INDEX.md`. Preserve useful user content and update stale entries rather than replacing the file blindly.

Index maintenance is part of every future vault change:

- Update the nearest `INDEX.md` when a file or folder is created, renamed, moved, deleted, or materially changes purpose.
- Update parent indexes when the purpose or identity of a child folder changes.
- Keep summaries brief enough to scan but specific enough to route retrieval.
- Do not claim that an index is complete unless its directory contents were inspected.

After generating the initial index tree, recap the vault's main branches and explain that Codex will use the indexes as a navigation map. Then wait for permission to begin the next installation step.

## Step 5: Create the Index Update Skill

Explain that vault indexes can become stale as notes are created, edited, renamed, moved, or deleted. Create a vault-local Codex skill named `$update` so the user can reconcile the complete index tree whenever needed.

Store the skill at:

```text
.agents/skills/update/
├── SKILL.md
└── agents/
    └── openai.yaml
```

The skill must:

- Trigger when the user invokes `$update` or asks to refresh, rebuild, or audit the vault indexes.
- Inventory the current filesystem rather than relying on remembered changes or timestamps.
- Read all existing `INDEX.md` files.
- Identify user-knowledge folders with missing indexes.
- Inspect note contents where filenames are insufficient to determine purpose.
- Create or update indexes from the deepest folders upward.
- Update the root `INDEX.md` last.
- Add entries for new files and folders.
- remove entries for deleted or moved content.
- Correct descriptions when a file or folder's purpose materially changes.
- Preserve useful hand-written index guidance.
- Re-inventory and verify coverage after making changes.
- Report which indexes changed and flag unresolved ambiguity.

This must be a full reconciliation every time. That makes `$update` reliable even if the user has worked for many sessions without running it.

Exclude `.obsidian/`, `.git/`, caches, generated dependencies, and skill implementation details from item-by-item knowledge indexing. These may be summarized as system directories in the root index when useful.

Recommend running `$update` near the end of each working session, after substantial vault restructuring, or whenever navigation appears stale. Do not claim that Codex will run it automatically unless the current environment provides and configures an explicit end-of-session automation mechanism.

Create concise UI metadata in `agents/openai.yaml` with a default prompt that explicitly invokes `$update`.

After creating the skill, validate that:

- Its directory name and frontmatter name are `update`.
- `SKILL.md` contains only the required `name` and `description` frontmatter fields.
- The description clearly states what the skill does and when it triggers.
- `agents/openai.yaml` contains a display name, a 25–64 character short description, and a default prompt mentioning `$update`.

Tell the user that `$update` is ready, explain when to invoke it, and wait for the definition of the next skill.

## Step 6: Install the Write NOMs Skill

Explain that `$write-noms` converts transcripts, meeting notes, or recordings into structured Notes of Meeting using reported speech, clear attribution, prose notes, a contents list, and an action table.

The complete skill specification is supplied in `write-noms.md` at the vault root. Treat that file as an installation source:

1. Confirm that `write-noms.md` exists.
2. Read the entire file.
3. Validate that its YAML frontmatter contains the skill name and a clear triggering description.
4. Create `.agents/skills/write-noms/SKILL.md` from its contents.
5. Preserve the source's substantive rules, structure, and example.
6. Ensure the installed skill recognizes `$write-noms`; it may retain `/write-noms` as an additional invocation alias.
7. Create `.agents/skills/write-noms/agents/openai.yaml` with suitable UI metadata and a default prompt that explicitly mentions `$write-noms`.
8. Leave the root `write-noms.md` in place as the portable installation source.

Do not silently invent a replacement skill if `write-noms.md` is missing or incomplete. Explain the problem and ask the user to provide or complete the source.

After installation, validate that:

- The directory and frontmatter names are `write-noms`.
- `SKILL.md` has only `name` and `description` in its frontmatter.
- The installed instructions retain the source's NOM grammar, structure, attribution, action, and review rules.
- The short description in `openai.yaml` is 25–64 characters.
- The default prompt explicitly invokes `$write-noms`.

Tell the user that the skill is ready and that they can invoke it with a transcript or meeting notes. Then continue to the next installation step only with the user's permission.

## Step 7: Complete and Clean Up the Installation

Perform this step only after every installation step is complete and verified. Do not delete the installation files while the user is still designing, reviewing, or testing the second brain.

Use this completion sequence:

1. Review the preceding steps and verify that all required vault files, folders, indexes, and skills exist.
2. Confirm that the installed `$write-noms` skill is complete and does not depend on reading the root `write-noms.md` at runtime.
3. Confirm that the bundled root `AGENTS.md` template exists. Do not depend on `/init`; the template is part of this installation package so the process works consistently across Codex surfaces.
4. Update `AGENTS.md` from the completed vault setup. Replace every placeholder using the final contents of `SOUL.md`, `USER.md`, the vault indexes, the working-brain structure, and the installed skills. Ensure the resulting file gives Codex durable vault-level guidance to:
   - Read `INDEX.md`, `SOUL.md`, and `USER.md` at the start of its work.
   - Navigate through folder-level indexes before broad searching.
   - Fall back to a comprehensive content search when indexes are insufficient or the user explicitly requests one, while excluding application internals and reporting the supporting vault files.
   - Use parallel agents for large, independent search branches when that capability is available and appropriate.
   - Run or recommend `$update` if a broad search reveals stale or missing index entries.
   - Store everyday working knowledge and todos in `brain/`.
   - Maintain todos as Markdown checklists.
   - Use vault-local skills when their trigger conditions apply.
   - Run `$update` after substantial changes or near the end of a substantive session.
   - Preserve privacy, user agency, existing content, and the hierarchical index structure.
   Remove the installation-template notice after all placeholders have been resolved. Do not invent missing identity details; derive them from the installed files or ask the user.
5. Tell the user that the second-brain installation is complete and briefly summarize what was installed.
6. Explain that the two root installation sources are now disposable:
   - `second-brain-initialisation.md`
   - `write-noms.md`
7. Immediately before deletion, ask the user to confirm that installation is finished and that these two files may be permanently removed. If the user declines or still needs them, stop this step and preserve both files.
8. After confirmation, delete only those two exact files. Do not delete the installed `.agents/skills/write-noms/SKILL.md`.
9. Invoke `$update` after deletion. It must perform its normal full vault audit, remove the deleted installation-file entries from the root `INDEX.md`, and verify every remaining index against the filesystem.
10. Report that cleanup and final indexing are complete, listing the deleted files and any indexes changed.

The order is important: announce completion, confirm cleanup, delete the installation sources, and run `$update` last. Never run the final index update before deleting the sources, because doing so would leave stale root-index entries.

This step is intentionally destructive and must not run merely because Codex has reached the end of this document. The user must explicitly confirm final cleanup at that time.
