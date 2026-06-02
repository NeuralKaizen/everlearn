---
name: thread
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(python3 *), Bash(mkdir *), Bash(ls *), Bash(grep *), Bash(cat *)
description: >-
  Maintain the loom second-brain vault: COLOR it by topic and TEND its health. Part of
  the loom collection. Trigger on "thread", "coloreá/ordená el vault", "pintá el grafo",
  "revisá la salud del vault". Paint: gives each topic tag a stable color (palette in
  ~/.config/loom/colors.json) and applies it to the Obsidian graph view (graph.json
  color-groups) and to notes (a CSS snippet + cssclasses). Tend: reports vault health —
  orphan notes, contract gaps (missing type/tags), dangling project: links, heavy
  uncreated ghost links — with handoffs to arc/everlearn. It edits .obsidian/ config and
  adds a cssclasses field to notes; otherwise it reads and suggests.
---

# thread

thread is loom's maintenance layer — it runs over the *whole* vault, on demand, to keep it legible. Two jobs:

- **Paint** — give every topic a stable color and show it in the graph and on the notes, so your second brain reads at a glance.
- **Tend** — surface what's drifting: orphan notes, notes that break the contract, links that point nowhere.

It's deliberate, not in-the-flow. It reads `type` and `tags` (see `../CONTRACT.md`).

## Core principles

1. **Stable colors.** A topic keeps its color across runs — persist the palette, never reshuffle an assigned one.
2. **Merge, don't clobber.** When writing `.obsidian/` config, preserve everything else; only set what thread owns.
3. **Tend is mostly read.** Report health and hand off (to arc / everlearn); only auto-fix trivia when asked.
4. **Be honest about touch.** Paint edits `.obsidian/` config and adds one `cssclasses` field per note. Say so.
5. **Match their language.**

## Paint — color by topic

### The palette
Maintain `~/.config/loom/colors.json` as `{ "<topic>": "#hex", ... }`. For each topic tag in the vault, reuse its existing color; if it's new, assign the next unused color from `reference/palette.md`. **Never change an already-assigned color.** The user may override any entry.

### Graph view → `<vault>/.obsidian/graph.json`
Read the existing file (or start from `{}`). Set `colorGroups` to one group per topic tag, **preserving every other key**:
```json
{ "query": "tag:#<topic>", "color": { "a": 1, "rgb": <hex-as-decimal-int> } }
```
`rgb` is the hex color as a decimal integer — compute it (`#89b4fa` → `9024762`). The user must reload Obsidian (or reopen the graph) to see the change.

### Notes (CSS) → `<vault>/.obsidian/snippets/loom-colors.css`
Write a snippet coloring each note by a `loom-<topic>` cssclass:
```css
/* loom · thread — topic colors (generated; do not edit by hand) */
.loom-<topic> .inline-title, .loom-<topic> h1 { color: #hex; }
```
Enable it: add `"loom-colors"` to `enabledCssSnippets` in `<vault>/.obsidian/appearance.json` (merge). Then add `cssclasses: ["loom-<primary-topic>"]` to each note's frontmatter, using its **first** topic tag — a single-field add, never touching the body. Skip notes that have no tags.

## Tend — vault health

Scan the vault and report a concise list (don't fix unless asked):

- **Orphans** — notes with no `[[links]]` in or out (lost in the graph).

- **Contract gaps** — notes missing `type` or `tags`.

- **Dangling project** — `project: "[[X]]"` where `projects/X.md` doesn't exist → suggest **arc** create it.

- **Heavy ghost links** — `[[X]]` referenced ≥2 times but never created → suggest **everlearn** capture it.

For each: the count, the files, and the handoff. Auto-fix only trivia (e.g. an obvious missing tag), and only if the user asks.

## Workflow

1. Resolve the vault root (`~/.config/loom/config.json` → `vault`).
2. **Paint:** gather all topic tags → update the palette → write graph.json color-groups (merge) → write + enable the CSS snippet → add `cssclasses` to notes missing it.
3. **Tend:** run the health scan → report with handoffs.
4. Tell them what changed, and remind them to **reload Obsidian** for the graph colors.

If the user asks only to paint or only to tend, do just that.
