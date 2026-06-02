---
name: decide
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(python3 *), Bash(mkdir *), Bash(ls *), Bash(grep *), Bash(cat *)
description: >-
  Capture a technical DECISION with its context, the moment it's made — part of the
  loom second-brain collection. Trigger when the user settles a technical choice:
  "decidí usar X", "vamos con X en vez de Y", "let's go with X", "registrá/anotá esta
  decisión". Records an ADR-style note — the decision, the context that forced it, the
  alternatives and why they lost, the tradeoff accepted, and the consequences — into
  the vault's `decisions/` folder, linked into the knowledge graph. NOT for trivial or
  easily-reversible choices, and NOT when the user is merely ASKING which option to
  pick (that's a normal answer — offer to record it once they choose).
---

# decide

decide records the *why* behind a technical choice the moment you make it — so future-you (or a teammate) can answer "why did we do X, and what did we rule out?" without archaeology. It's a capture skill in the **loom** second brain (see `../CONTRACT.md`).

A decision note is a lean ADR (Architecture Decision Record): the choice, the forces, the road not taken, the tradeoff, the consequences.

## Core principles

1. **Don't break the flow.** Confirm the decision in a sentence, then write the note. The note is the artifact, not the conversation.
2. **Capture the road not taken.** The alternatives and *why they lost* are the highest-value part — that's exactly what future-you forgets.
3. **Be honest about the tradeoff.** Every real decision gives something up. Name it.
4. **Minimum sufficient.** Record the decision, not an essay.
5. **Match their language.**
6. **Connect into the graph.** Link the related decisions, the concepts involved, and the project.
7. **Respect the second brain.** Auto-add a single contextual `[[link]]` into clearly related notes; never rewrite them.

## When to activate

- The user settles a technical choice mid-work: "vamos con X", "decidí X en vez de Y".
- An explicit "registrá / anotá esta decisión".
- NOT for trivial or easily-reversible choices. NOT when they're just *asking* which to pick — answer normally, then offer to record it once they decide.

## Where notes are saved

Resolve the vault root once, then cache it:

1. `~/.config/loom/config.json` → `vault`.
2. else `~/.config/everlearn/config.json` → the parent of `conceptsDir` (bootstrap from everlearn).
3. else detect the Obsidian vault (`obsidian.json`; one → propose, several → ask), or ask for a path.

Persist `{ "vault": "<abs path>" }` to `~/.config/loom/config.json`. Write the note to `<vault>/decisions/<slug>.md`, creating `decisions/` if missing (just tell the user you did — no need to ask). `<slug>` is kebab-case of the decision (e.g. `postgres-over-mongo`, `monorepo-loom`).

## The note

Follow `reference/decision-template.md`. Headings in the user's language. Use loose lists (a blank line between bullets).

1. **Frontmatter** — per `../CONTRACT.md`: `type: decision`, `created` (today), `status: accepted`, `tags`, `context`, and `project: "[[name]]"` *only* when it clearly belongs to one (infer from the conversation — don't interrogate; omit if unclear).
2. **La decisión** — what was chosen. 1-2 lines, lead with it.
3. **Contexto** — the forces/situation that made it necessary now.
4. **Alternativas** — the options considered and why each lost (the road not taken).
5. **El tradeoff** — what you're giving up on purpose.
6. **Consecuencias** — what it commits you to; what gets easier/harder; follow-ups.
7. **Conexiones** — `[[wikilinks]]` to related decisions, concepts, and the project.

## Superseding

Decisions are a chain, not a graveyard. When this decision overrides an earlier one in the vault: set the old note's `status: superseded`, add a `superseded-by: "[[new-slug]]"` line to its frontmatter, and link back to the old one from the new note's **Conexiones**. This is a single-field edit on the old note — do it automatically, no permission needed.

## Workflow

1. **Confirm the decision** inline in a sentence so they can keep moving.
2. **Resolve `<vault>`** (cache) and ensure `decisions/` exists.
3. **Write the note** — read the template, pick a slug, scan `decisions/` and `concepts/` for link targets, set `project:` if it's obvious from context.
4. **Auto-link + supersede** — add one contextual link into clearly related notes; if this overrides a prior decision, mark it superseded and cross-link.
5. **Tell them what was saved** (including any notes linked or superseded) and get out of the way.

Today's date is available in the environment context — use it for `created`.
