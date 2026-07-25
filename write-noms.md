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
  - Raymond Chua / Chief Executive (Health Sciences Authority)
  - Yeo Yong Kiat / Deputy Director (GovTech)

**Abbreviation rule:** Because attendees' full names, designations, and orgs are stated here, the body of the NOMs can use abbreviations: `Designation/Org`. For example:
- CE/HSA (refers to Raymond Chua)
- DD/GovTech (refers to Yeo Yong Kiat)

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
- Action items must be clear from the language in the Notes column (e.g., "CE/HSA would follow up with...", "DD/GovTech to circulate...")
- The For Action column states who is responsible - aligned to the specific bullet point where the action appears
- Can be a person, team, or organisation

---

## Example

**Title:** Discussion on HSA Digital Transformation Role

**Date:** 27 January 2026

**Attendees:**
- Raymond Chua / Chief Executive (Health Sciences Authority)
- Yeo Yong Kiat / Deputy Director (GovTech)

**Contents**
1. Yong Kiat's Concerns
   - *Job Scope Issues*
   - *Progression Issues*
2. New Role Proposal

| S/N | Notes | For Action |
|-----|-------|------------|
| 1 | Yong Kiat's Concerns ||
|   | *Job Scope Issues*<br><br>a. DD/GovTech explained that he wanted a role with concrete levers to effect change, rather than an advisory position.<br><br>b. CE/HSA acknowledged this and noted that the original corporate office role had resourcing constraints.<br><br>*Progression Issues*<br><br>a. DD/GovTech highlighted that JR10 would be a demotion from his current grade.<br><br>b. CE/HSA confirmed he would work to upgrade the post to JR9. | |
| 2 | New Role Proposal ||
|   | a. CE/HSA proposed a Director-level position for the Front-End Regulatory Services Office.<br><br>b. CE/HSA explained that the role would consolidate all stakeholder-facing regulatory functions.<br><br>c. DD/GovTech to consider the offer and revert. | DD/GovTech |

---

## Instructions

When the user invokes /write-noms:
1. Ask for the transcript or meeting notes (if not provided)
2. Ask for attendee details (names, designations, organisations) if not clear
3. Ask whether this was an ad-hoc or pre-tabled meeting
4. Generate the NOM following the structure above
5. Present for user review and refinement
