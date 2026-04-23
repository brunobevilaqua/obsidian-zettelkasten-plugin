<div align="center">

<img src="https://obsidian.md/images/obsidian-logo-gradient.svg" width="96" alt="Obsidian logo" />

# obsidian-zettelkasten

**A Claude Code plugin that turns any Obsidian vault into a self-organizing Zettelkasten second brain.**

[![Version](https://img.shields.io/badge/version-1.0.0-7C3AED?style=flat-square)](https://github.com/brunobevilaqua/obsidian-zettelkasten-plugin)
[![License](https://img.shields.io/badge/license-MIT-7C3AED?style=flat-square)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-7C3AED?style=flat-square)](https://code.claude.com)
[![Obsidian](https://img.shields.io/badge/Obsidian-compatible-7C3AED?style=flat-square)](https://obsidian.md)

</div>

---

## What is this?

Drop a raw note into your `inbox/` folder. Run one command. Claude reads it, researches it if needed, splits it into atomic notes, wires up `[[wikilinks]]` to related content, and archives the original — all following the Zettelkasten method.

This plugin works with any focused Obsidian vault: a personal knowledge base, a client work vault, or one per project.

```
inbox/compound-interest-notes.md      ← you drop it here
         ↓
notes/finance/compound-interest.md    ← Claude creates atomic note
notes/finance/rule-of-72.md           ← splits if multiple concepts
         ↓
trashbin/compound-interest-notes.md   ← original archived with links
```

---

## How it works

```
┌─────────────────────────────────────────────────────────────┐
│                        Your Vault                           │
│                                                             │
│   inbox/              ──▶  Claude processes                 │
│   ├── raw-note.md          │                                │
│   └── article.md           ├─ classifies (personal/work)   │
│                             ├─ researches facts             │
│   notes/ (auto-created)    ├─ writes atomic notes          │
│   ├── finance/             ├─ adds [[wikilinks]]            │
│   ├── learning/            └─ archives original            │
│   └── health/                                               │
│                                                             │
│   trashbin/                                                 │
│   └── raw-note.md  ◀── original + links to created notes   │
└─────────────────────────────────────────────────────────────┘
```

The plugin includes:

| Component | What it does |
|---|---|
| `setup` skill | Initializes `inbox/` and `trashbin/`, writes `.gitignore` and Claude permissions |
| `process-inbox` skill | Full Zettelkasten processing workflow — one file or all at once |
| `researcher` agent | Spawned automatically for factual content: verifies claims, adds missing context |

Folders are created **on demand** — the vault stays clean until you have real content to organize.

---

## Prerequisites

- [Claude Code](https://code.claude.com) installed
- [Obsidian](https://obsidian.md) installed
- A vault folder with `git init` already run

---

## Installation

### Option 1 — Project scope (recommended)

Installs the plugin only for the current vault. The configuration is checked into the vault's git repo.

```
/plugin marketplace add https://github.com/brunobevilaqua/obsidian-zettelkasten-plugin
/plugin install obsidian-zettelkasten@obsidian-zettelkasten --scope project
```

### Option 2 — Global scope

Installs once for your user account, available in all your Claude Code projects.

```
/plugin marketplace add https://github.com/brunobevilaqua/obsidian-zettelkasten-plugin
/plugin install obsidian-zettelkasten@obsidian-zettelkasten --scope user
```

### Option 3 — From a local clone

If you cloned the repo locally and want to use it directly:

```
/plugin marketplace add /path/to/obsidian-zettelkasten-plugin
/plugin install obsidian-zettelkasten@obsidian-zettelkasten --scope project
```

---

## Setting up a new vault

After installation, run the setup skill inside your vault:

```
/obsidian-zettelkasten:setup
```

This creates `inbox/` and `trashbin/`, writes a `.gitignore`, and configures the necessary Claude Code permissions. It is idempotent — safe to re-run.

Then add a `CLAUDE.md` to the vault root declaring the vault type. This is the **only** per-vault configuration needed.

**Personal vault:**
```markdown
# Personal Vault

This is a personal Zettelkasten vault. All notes are personal.
```

**Work vault:**
```markdown
# Work Vault — ClientName

All notes in this vault are work notes for client ClientName.
```

That's it. Drop files into `inbox/` and start processing.

---

## Usage

### Process all files in inbox

```
/obsidian-zettelkasten:process-inbox
```

### Process a single file

```
/obsidian-zettelkasten:process-inbox my-note.md
```

Claude will:
1. Read and classify the note (asks if unclear)
2. Research factual claims with the `researcher` agent
3. Write atomic notes in the right subfolder (asks before creating new folders when unsure)
4. Wire bidirectional `[[wikilinks]]` to related notes
5. Archive the original to `trashbin/` with links to what was created

---

## What gets created

**Personal note:**
```yaml
---
title: "Compound Interest"
tags: [finance, investing]
created: 2025-04-23
type: note
source: "The Psychology of Money"
---
```

**Work note:**
```yaml
---
title: "Login Button Not Firing on Checkout Page"
tags: [analytics, debugging]
created: 2025-04-23
type: jira
client: "ClientName"
project: "tag-implementation"
---
```

**Archived inbox file (trashbin/):**
```yaml
---
processed: 2025-04-23
created_notes:
  - "[[compound-interest]]"
  - "[[rule-of-72]]"
---
```

---

## Why separate vaults?

| Single vault | Multiple focused vaults |
|---|---|
| Personal and work notes mixed | Clean separation of concerns |
| Claude searches everything | Faster, cheaper searches |
| One large context | Small, focused context per session |
| Harder to share a work vault | Share client vaults independently |

---

## Built on kepano/obsidian-skills

This plugin depends on [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills), which provides Claude Code with deep Obsidian knowledge: Obsidian Flavored Markdown, wikilinks, callouts, Bases, Canvas, and the Defuddle web extractor. It is pulled in automatically — no extra installation needed.

---

## License

MIT © [Bruno Bevilaqua](https://github.com/brunobevilaqua)
