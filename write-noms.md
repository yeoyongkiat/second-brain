---
name: write-noms
description: Use when the user asks to generate Notes of Meeting (NOMs) from a transcript, meeting notes, or recording, or invokes /write-noms. Produces Singapore government-style reported-speech minutes with attribution, prose notes, a contents list, and an action table.
---

# /write-noms - Notes of Meeting

Generate government-style Notes of Meeting (NOMs) from a transcript or meeting notes.

---

## Grammar & Style Rules

| Element | Rule |
|---------|------|
| Tense | Reported speech |
| Person | Third person - be specific who said what |
| Voice | Active voice. NEVER use passive voice (e.g., "It was agreed...") - passive obscures attribution. Leave that choice to the human writer only. |
| Format | Prose, not bullet points |
| Tone | Neutral, factual, concise |
| Attribution | Always attribute positions/statements to specific people |

**Common constructions:**
- "[Person] highlighted/noted/clarified/explained that..."
- "[Person] agreed that..."
- "[Person] decided that..."
- "The meeting discussed..."
- "Action: [Person] to [verb] by [date]"

---

## Structure

### 1. Title
- **Ad-hoc/one-off meeting:** Describe what the meeting is about (e.g., "Expansion of Healthcare Subvention")
- **Regular meeting platform:** Name of the platform (e.g., "Weekly PS-Huddle")

### 2. Date
- Date of the meeting

### 3. Attendees
- Bullet point list, most senior person first
- Format: `Name / Designation (Organisation)`
- Examples:
  - Alex Tan / Chief Executive (Agency for Digital Services)
  - Jordan Lee / Deputy Director (Ministry of Finance)

**Abbreviation rule:** Because attendees' full names, designations, and orgs are stated here, the body of the NOMs can use abbreviations: `Designation/Org`. For example:
- CE/ADS (refers to Alex Tan)
- DD/MOF (refers to Jordan Lee)

### 4. Contents

Before the body table, include a simple contents list showing topics and sub-topics at a glance:

```
**Contents**
1. [Topic Title]
   - *Sub-topic 1*
   - *Sub-topic 2*
2. [Topic Title]
   - *Sub-topic 1*
...
```

### 5. Body

**Format:** Table with 3 columns: `S/N | Notes | For Action`

| Column | Description |
|--------|-------------|
| S/N | Serial number |
| Notes | Reported notes (prose, reported speech, active voice, attribution) |
| For Action | Person/team/org responsible for follow-up (corresponds to specific action items in Notes) |

**Two-row structure per topic:**
- **Row 1 (header):** S/N number + Topic title in Notes column. Merge Notes and For Action cells for this row.
- **Row 2 (content):** Blank S/N + detailed notes in Notes column, For Action as applicable.

**S/N labelling convention:**
- **Ad-hoc meeting (no pre-made agenda):** Aggregate organic discussions into logical topics
- **Pre-tabled meeting (with agenda):** Discussion proceeds linearly item by item

**Notes column enumeration:**
- Each idea discussed under a topic: `a.`, `b.`, `c.` etc.
- Sub-points if needed: `i.`, `ii.`, `iii.` etc.

**Sub-topic organisation:**
- Main topics (S/N) are often split into smaller sub-topics
- Sub-topics appear as *italics* (sub-header)
- Under each sub-topic, list notes as a., b., c. etc.
- Leave a blank line between sub-topics
- Continue until main topic is exhausted

**For Action column rules:**
- Action items must be clear from the language in the Notes column (e.g., "CE/ADS would follow up with...", "DD/MOF to circulate...")
- The For Action column states who is responsible - aligned to the specific bullet point where the action appears
- Can be a person, team, or organisation

---

## Example

**Title:** Rollout of the Community Grants Portal

**Date:** 15 March 2026

**Attendees:**
- Alex Tan / Chief Executive (Agency for Digital Services)
- Jordan Lee / Deputy Director (Ministry of Finance)

**Contents**
1. Portal Readiness
   - *System Testing*
   - *Onboarding Timeline*
2. Funding Approval

| S/N | Notes | For Action |
|-----|-------|------------|
| 1 | Portal Readiness ||
|   | *System Testing*<br><br>a. DD/MOF reported that user acceptance testing was complete, with no critical defects outstanding.<br><br>b. CE/ADS noted that two minor issues remained and would be resolved before launch.<br><br>*Onboarding Timeline*<br><br>a. DD/MOF proposed a phased onboarding beginning with pilot agencies.<br><br>b. CE/ADS agreed and asked for a detailed schedule. | |
| 2 | Funding Approval ||
|   | a. CE/ADS confirmed the platform budget had been approved for the financial year.<br><br>b. DD/MOF to circulate the finalised cost breakdown to stakeholders. | DD/MOF |

---

## Instructions

When the user invokes /write-noms:
1. Ask for the transcript or meeting notes (if not provided)
2. Ask for attendee details (names, designations, organisations) if not clear
3. Ask whether this was an ad-hoc or pre-tabled meeting
4. Generate the NOM following the structure above
5. Present for user review and refinement
