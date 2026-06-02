# Loom

A collection of Claude skills that weave your Obsidian vault into a **second brain** —
capture knowledge in the flow, keep it organized, and pull it back when you need it.

Each skill owns one job. They compose through a shared [note contract](CONTRACT.md)
(`type` + `tags` in frontmatter), so they stay independent but interoperable — the
threads of one loom.

## The skills

| Skill         | Role        | What it does                                                |
|---------------|-------------|-------------------------------------------------------------|
| **everlearn** | capture     | Capture & expand a concept you're learning → `concepts/`    |
| **decide**    | capture     | Record a technical decision with its context → `decisions/` |
| **log**       | capture     | Quick capture of a learning, error, or reflection → `logs/` |
| **arc**       | context     | Track a project's context & lifecycle (living hub) → `projects/` |
| **recall**    | retrieval   | "Why do I do X?" / "What did I learn about Y?"              |
| **thread**    | maintenance | Keep notes organized and colored by topic across the vault  |

> Status: **everlearn**, **decide**, and **log** are built. **arc**, **recall**, **thread** in progress.

## How it fits together

```
   in the flow                          on demand
┌───────────────┐                   ┌──────────────┐
│  everlearn    │                   │   recall     │  reads
│  decide       │── write notes ──▶ │  (retrieve)  │ ◀── the
│  log          │   (type + tags)   └──────────────┘    graph
└───────────────┘                   ┌──────────────┐
                                    │   thread     │  colors + tidies
                                    │ (maintain)   │ ◀── by type/tags
                                    └──────────────┘
```

## Install

Clone once, then symlink the skills you want into your Claude skills directory:

```bash
git clone https://github.com/NeuralKaizen/Loom.git
ln -s "$(pwd)/Loom/everlearn" ~/.claude/skills/everlearn
# ...repeat per skill as they ship
```

## License

MIT — see [LICENSE](LICENSE).
