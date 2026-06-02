---
name: log
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(python3 *), Bash(mkdir *), Bash(ls *), Bash(grep *), Bash(cat *)
description: >-
  Quickly capture a LEARNING, ERROR, or REFLECTION the moment it happens — the fastest,
  lightest capture in the loom second-brain collection. Trigger on "aprendí que X",
  "TIL ...", "me di cuenta de Y", "nota mental: Z", "se rompió por W", "perdí tiempo con
  V", "loguea/anota esto", "reflexión: ...". Writes one tiny note per entry to the
  vault's `logs/` folder — the insight, tagged and linked into the graph — with a `kind`
  of learning | error | reflection. NOT for concepts you want to understand deeply (that's
  everlearn) or technical decisions (that's decide).
---

# log

log is the fastest capture in **loom** (see `../CONTRACT.md`): a passing learning, a debugging scar, or a reflection — caught the moment it happens, before it evaporates. One tiny note per entry, tagged and linked, so it resurfaces later through recall and the graph.

Keep it *light*. A log is a moment, not an essay. If the thing deserves a real explanation, that's everlearn; if it's a decision, that's decide.

## Core principles

1. **Fast above all.** A short ack, then write. Don't interrogate, don't pad.
2. **The insight IS the title.** Title the note with the learning itself ("Promise.all es fail-fast"), not "log sobre promesas".
3. **Tag the kind.** `learning | error | reflection` — so recall can later surface "my errors" or "my reflections".
4. **Match their language.**
5. **Connect into the graph.** Even a one-liner links to the concept, decision, or project it touches.
6. **Respect the second brain.** Auto-add a single contextual `[[link]]` into a clearly related note; never rewrite it.

## When to activate

- "aprendí que…", "TIL…", "me di cuenta…", "resulta que…" → `kind: learning`
- "se rompió por…", "perdí tiempo con…", "el bug era…" → `kind: error`
- "nota mental:…", "creo que…", "me pregunto si…" → `kind: reflection`
- An explicit "loguea / anota esto".
- NOT for deep concepts (that's everlearn) or decisions (that's decide).

## Where notes are saved

Resolve the vault root: `~/.config/loom/config.json` → `vault` (else bootstrap from the parent of `conceptsDir` in `~/.config/everlearn/config.json`, else detect the Obsidian vault or ask; persist to the loom config). Write to `<vault>/logs/<slug>.md`, creating `logs/` if missing (just tell them). `<slug>` is a short kebab-case of the insight.

## The note

Follow `reference/log-template.md`. Headings in the user's language. Loose lists (blank line between bullets).

1. **Frontmatter** — per `../CONTRACT.md`: `type: log`, `created` (today), `kind` (learning | error | reflection — infer from phrasing), `tags`, `context`, and `project: "[[name]]"` only if obvious (omit otherwise).
2. **Title** — the insight itself, one line.
3. **Body** — 2-4 lines: what happened / what you learned / why it matters. No filler.
4. **Conexiones** — `[[wikilinks]]` to the concept, decision, or project it touches.

## Workflow

1. **Acknowledge** in a few words.
2. **Resolve `<vault>`**, ensure `logs/`.
3. **Write the note** — infer `kind`, pick a slug, scan `concepts/`, `decisions/`, `logs/` for link targets, set `project:` if obvious.
4. **Auto-link** one contextual link into a clearly related note.
5. **Brief confirm** and get out of the way.

Today's date is available in the environment context — use it for `created`.
