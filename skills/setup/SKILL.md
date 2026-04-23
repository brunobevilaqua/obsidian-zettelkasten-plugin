---
description: Initialize a new Obsidian vault with the required folders and Claude Code permissions
---

Set up this Obsidian vault for Zettelkasten note processing.

## Steps

### 1. Create required folders
Create the following directories if they do not already exist:
- `inbox/` — user drops raw files here for processing
- `trashbin/` — processed inbox files are moved here; user deletes manually

Do not create any other folders. All content folders are created on-demand when processing notes.

### 2. Create `.claude/settings.local.json`
If the file does not exist, create it with:
```json
{
  "permissions": {
    "allow": [
      "WebFetch(domain:help.obsidian.md)",
      "WebFetch(domain:obsidian.md)",
      "WebFetch(domain:github.com)",
      "WebFetch(domain:raw.githubusercontent.com)"
    ]
  }
}
```
If the file already exists, merge these entries into the existing `allow` array without removing any existing entries.

### 3. Create `.gitignore`
If a `.gitignore` does not exist, create one with:
```
.DS_Store
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/plugins/*/data.json
```
If it already exists, skip this step.

### 4. Confirm setup
Tell the user setup is complete and remind them to:
- Create a `CLAUDE.md` in the vault root declaring the vault type (see template below)
- Drop files into `inbox/` and run `/obsidian-zettelkasten:process-inbox` to process them

## CLAUDE.md template to share with user

For a **personal vault**:
```markdown
# Personal Vault

This is a personal Zettelkasten vault. All notes are personal.
```

For a **work vault**:
```markdown
# Work Vault — ClientName

All notes in this vault are work notes for client ClientName.
```
