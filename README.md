# Loom

*Weave what you learn into a second brain — without breaking your flow.*

You're deep in a build. You hit a concept you don't get, make a decision whose reasoning
you'll forget by next week, learn something the hard way. Normally that knowledge either
**evaporates** — or pulls you out of flow to go document it.

**Loom** is a collection of [Claude Code](https://claude.com/claude-code) skills that catch
it *in the moment*, file it into your Obsidian vault as interlinked notes, and hand it back
when you need it. A passing question becomes a permanent, connected part of your knowledge
graph — and you never left your terminal.

Six small skills, one job each. They compose through a shared [note contract](CONTRACT.md),
so your vault stays a **graph**, not a pile.

## The skills

| Skill         | Role        | What it does                                                |
|---------------|-------------|-------------------------------------------------------------|
| **everlearn** | capture     | Explain a concept in *your* context → `concepts/` (+ optional interactive demo) |
| **decide**    | capture     | Record a decision: context, alternatives, the tradeoff → `decisions/` |
| **log**       | capture     | One-line capture of a learning, error, or reflection → `logs/` |
| **arc**       | context     | A living hub per project — status + everything captured under it → `projects/` |
| **recall**    | retrieval   | Answer from *your own* notes: "why do I do X?", "what did I learn about Y?" |
| **thread**    | maintenance | Color the vault by topic, and tend its health |

## In practice

```
you ⟩ wait, I don't really get how Promise.all works
```
**everlearn** answers in two lines so you keep coding, then quietly writes
`concepts/promise-all.md` — explained around *your* code, linked to related notes, with an
optional interactive demo.

```
you ⟩ let's go with Postgres over Mongo for the catalog
```
**decide** records `decisions/postgres-over-mongo.md` — the context, the alternatives you
ruled out, and the tradeoff you accepted.

```
you ⟩ why did I pick Postgres?
```
**recall** reads your vault and answers from your *own* decision, citing
`[[postgres-over-mongo]]` — not generic knowledge.

Three weeks later, none of it is lost. It's a connected graph you can walk.

## How it fits together

```
  ┌─ capture · in the flow ───────────────┐
  │  everlearn → concepts/                 │
  │  decide    → decisions/                │   every note carries
  │  log       → logs/                     │   type + tags + project
  └────────────────────────────────────────┘
                  │  all interlinked via [[wikilinks]]
                  ▼
  ┌─ context ───────────┐ ┌─ retrieve ────────┐ ┌─ maintain ────────┐
  │ arc → projects/     │ │ recall            │ │ thread            │
  │ living hubs that    │ │ answers from      │ │ colors by topic,  │
  │ aggregate by project│ │ your own notes    │ │ tends vault health│
  └─────────────────────┘ └───────────────────┘ └───────────────────┘
```

## The contract

Every note shares a small frontmatter, which is what lets the skills compose
(see [CONTRACT.md](CONTRACT.md)):

```yaml
---
type: concept | decision | log | project
created: 2026-06-02
tags: [async, postgres]
project: "[[my-app]]"   # optional — ties the note to a project
---
```

`thread` colors by these, `recall` queries them, `arc` aggregates by `project`.

## Install

```bash
git clone https://github.com/NeuralKaizen/Loom.git
for s in everlearn decide log arc recall thread; do
  ln -s "$(pwd)/Loom/$s" ~/.claude/skills/$s
done
```

- These are **personal skills** — available in *every* project, not just one.
- Each declares its `allowed-tools`, so it runs with **minimal permission prompts**.
- On first use, Loom finds your Obsidian vault and remembers it — zero config.

## Requirements

- [Obsidian](https://obsidian.md) (any vault) — the second brain.
- [Claude Code](https://claude.com/claude-code) — runs the skills.

> Obsidian-first, but portable: the notes are plain Markdown, and the optional demos are
> standalone HTML. Nothing breaks without Obsidian — but the `[[wikilink]]` graph is where
> it shines.

## Philosophy

Learning shouldn't interrupt building — and it shouldn't evaporate either. Loom delivers the
knowledge contextualized to what you're doing, then files it, linked, into the brain you
already trust.

## License

MIT — see [LICENSE](LICENSE).
