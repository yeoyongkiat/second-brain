# chat-history — INDEX

The vault's **session journal**. Each note records what an `/update` run found and did: file drift since the previous run, plus a summary of what that session discussed or decided. Written by the `update` skill (step 6), one note per day (`YYYY-MM-DD.md`); multiple runs in a day append `## HH:MM` sections.

Read the most recent entries at the start of a session for continuity with prior work.

## Convention

- Filenames: `YYYY-MM-DD.md`, newest by date.
- Each entry has a **Drift since last update** line (from the hash diff) and a **Session notes** line (the agent's summary).
- Scope is current-session-only: sessions where `/update` was never run are not journaled.

## Maintenance

Written automatically by `/update`. Do not delete entries unless the user asks.
