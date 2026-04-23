---
description: Process one or all notes in inbox/ into atomic Zettelkasten notes, then move originals to trashbin/
---

Process inbox notes into atomic Zettelkasten notes.

**Usage:**
- No arguments → process all `.md` and `.txt` files currently in `inbox/`
- With argument → process only `inbox/$ARGUMENTS`

---

## Workflow

Repeat the steps below for each file being processed. Complete one file fully before starting the next.

### Step 1 — Read context

- Read the file to process
- Read `CLAUDE.md` in the vault root to determine:
  - Vault type: **personal** or **work**
  - For work vaults: the client name
  - Any vault-specific conventions

### Step 2 — Classify and understand the content

- Identify the core concept(s) in the file
- If vault type cannot be determined from `CLAUDE.md`: **ask the user**
- If the file contains multiple distinct concepts that deserve separate notes: **ask the user before splitting**
- If the content language is ambiguous (e.g., title-only file, mixed languages): **ask the user what language to write the note in**

### Step 3 — Research (when applicable)

Spawn the `researcher` subagent for notes containing:
- Domain concepts (finance, law, medicine, music theory, tech, etc.)
- Factual claims or definitions worth verifying
- Technical guides or setup instructions

Pass to the researcher: the topic and the key claims or facts present in the note.
Apply CORRECTIONS and high-value ADDITIONS from the researcher's output before writing the note.

Skip research for: personal opinions, recipes, meeting notes, code snippets, task lists, Jira tickets.

### Step 4 — Determine destination

Choose or create a subfolder based on the note's topic. Use the vault type from `CLAUDE.md`:

**Personal vault** — organize by topic:
- Examples: `finance/`, `learning/music/`, `health/`, `productivity/`
- Use an existing folder when a good fit exists; scan the vault first
- Create a new folder only when no existing one fits — if unsure, **ask the user to confirm the folder name**
- Never create a folder just for one file unless it will clearly grow

**Work vault** — organize by tool or domain:
- Examples: `tealium/`, `meetings/`, `resources/`, `jira-tickets/`
- Jira tickets specifically: `jira-tickets/<TICKET-ID> - <short-title>.md`
- If the client name is missing and `CLAUDE.md` does not declare one: **ask the user**

### Step 5 — Write the atomic note

One concept per file. Filename: `kebab-case-concept-title.md`

**Personal note frontmatter:**
```yaml
---
title: "Descriptive title"
tags: [<topic>, <subtopic>]
created: YYYY-MM-DD
type: note
source: "URL, book, or course name"
---
```
Omit `source` if there is none.

**Work note frontmatter:**
```yaml
---
title: "Descriptive title"
tags: [<tool-or-topic>]
created: YYYY-MM-DD
type: note
client: "ClientName"
project: "project name"
---
```
Omit `project` if not applicable. Valid types: `note | jira | snippet | doc | reference`.

Write a clear, atomic note body focused on a single concept. End the note with an empty `## Related` section — it will be populated in the next step.

### Step 6 — Add wikilinks

After writing the note, search the vault for notes that share tags, keywords, or related concepts:
- Add `[[wikilinks]]` under `## Related` in the new note pointing to related existing notes
- Add a `[[wikilink]]` back to the new note in the `## Related` section of each related note

**Never create a broken link.** Only link to files that exist in the vault right now.

### Step 7 — Update the original inbox file

Add processing metadata to the original file. If the file has YAML frontmatter, add these fields to it. If it has no frontmatter, prepend a new frontmatter block:

```yaml
---
processed: YYYY-MM-DD
created_notes:
  - "[[note-title-1]]"
  - "[[note-title-2]]"
---
```

List every note created from this file under `created_notes`.

### Step 8 — Move to trashbin

Move the file from `inbox/<filename>` to `trashbin/<filename>`.
The user will delete it manually when ready.

---

## When to stop and ask the user

Always ask before:
- Creating a folder you are not sure about
- Splitting a note when the right split point is unclear
- Writing a work note when the client is unknown
- Linking to a file that does not exist yet
- Choosing between two equally valid topic folders
