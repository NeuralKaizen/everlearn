---
name: arc
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(python3 *), Bash(mkdir *), Bash(ls *), Bash(grep *), Bash(cat *)
description: >-
  Maintain the living HUB for a project — its context, lifecycle, and links to everything
  captured under it. Part of the loom second-brain collection. Trigger to CREATE
  ("arrancá / creá / registrá el proyecto X") or UPDATE ("actualizá el proyecto X", "X
  pasó a shipped / en pausa", "cerrá el proyecto", "¿en qué está X?"). Keeps ONE living
  note per project in the vault's `projects/` folder — status active|paused|shipped|
  abandoned — and aggregates the decisions, logs, and concepts that point at it via
  `project:`. It UPDATES an existing note over time rather than capturing once. NOT for a
  single decision/log/concept (those are decide/log/everlearn).
---

# arc

arc maintains the **living hub** for each project — the note that holds its context, its lifecycle, and links to everything captured under it. Where everlearn / decide / log capture atoms in the flow, arc is the **slow, evolving** layer: few notes, large, updated over time. It's the project's arc, from start to ship.

It's also the aggregator: notes carrying `project: "[[name]]"` (decisions, logs, concepts) are gathered here, grouped by type — turning scattered captures into a project's story. See `../CONTRACT.md`.

## Core principles

1. **One living note per project.** arc updates it over time; it never spawns a new note per change.
2. **Lifecycle first.** The hub always shows where the project is: `active | paused | shipped | abandoned`.
3. **Aggregate, don't duplicate.** Link the related notes (by `project:` backlink); don't restate their content.
4. **Match their language.**
5. **It's a MOC, not an essay.** Curated links + a thin layer of context — not long prose.
6. **Respect the second brain.** arc fully owns its own project notes. In OTHER notes it only ever adds the single `project:` field (when one clearly belongs and lacks it) — never rewrites them.

## When to activate

- **Create:** "arrancá / creá / registrá el proyecto X".
- **Update:** "actualizá el proyecto X", "X pasó a shipped / en pausa", "cerrá el proyecto", "¿en qué está X?".
- When a `project: "[[X]]"` is referenced across notes but no hub exists, offer to create it.
- NOT for atoms — a single decision/log/concept is decide/log/everlearn, not arc.

## Where notes are saved

Resolve the vault root (`~/.config/loom/config.json` → `vault`; else bootstrap from `conceptsDir`'s parent; else detect/ask). Write to `<vault>/projects/<slug>.md`, creating `projects/` if missing. `<slug>` is kebab-case of the project name.

## The note

Follow `reference/project-template.md`. Headings in the user's language. Loose lists (blank line between bullets).

1. **Frontmatter** — per `../CONTRACT.md`: `type: project`, `created`, `status` (active|paused|shipped|abandoned), `tags`, `context`. (No `project:` field — it *is* the project.)
2. **Estado** — current status + one line on where it's at.
3. **Objetivo** — what success looks like.
4. **Contexto** — what it is and why it exists.
5. **Decisiones / Aprendizajes / Conceptos** — aggregated `[[links]]` to notes pointing here, grouped by type, one line each.
6. **Próximo** — next steps and open questions.

## Aggregation (the signature move)

To build or refresh the aggregation sections, scan the vault for notes whose frontmatter has `project: "[[<this-project>]]"`, and list them under **Decisiones** (`type: decision`), **Aprendizajes** (`type: log`), **Conceptos** (`type: concept`). On update, refresh these lists — add the new ones, keep the curation. If a note clearly belongs but lacks the `project:` field, you may add that single field to it (a one-line frontmatter add, automatic — never rewrite its body).

## Workflow

- **Create:** resolve vault → ensure `projects/` → scaffold the hub (status `active`, objetivo, contexto, labeled aggregation sections, próximo) → run aggregation to pull in notes already pointing at it → tell them.
- **Update:** open the existing hub → apply the change (status, milestone, próximo) → refresh aggregation → tell them what changed.

Today's date is available in the environment context — use it for `created`.
