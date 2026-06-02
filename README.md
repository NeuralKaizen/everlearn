# everlearn

A Claude skill that generates learning material **while you build** — without breaking your flow.

When you're constructing something and get stuck on a concept, everlearn gives you exactly
what you need to understand and keep going — not a full course. It's built for fast-moving,
AI-assisted developers who reach unfamiliar territory and need the concept explained **in the
terms of what they're already building**, then saved so they can return to it.

## When it activates

- You're mid-development and don't understand something
- You ask "how does X work", "explain Y", "I don't get Z"
- You want to save a concept into your notes

## What it produces — two files

- `<concept>.md` — a structured note: what it is (no filler), why it matters in *your* context,
  the key code pattern, an embedded interactive HTML demo, the design decisions behind the
  pattern, and common mistakes.
- `<concept>-demo.html` — a self-contained interactive demo (≤80 lines) that shows the concept
  visually using *your* domain, *your* variable names, *your* problem.

**Obsidian-first but portable:** in Obsidian the demo renders inline; in any other editor the
files are plain Markdown + a standalone HTML you can open in a browser. Nothing breaks without
Obsidian.

## Install

Clone and link the skill into your Claude skills directory:

```bash
git clone https://github.com/NeuralKaizen/everlearn.git
ln -s "$(pwd)/everlearn" ~/.claude/skills/everlearn
```

That's it. It triggers automatically when you get stuck mid-build, or invoke it explicitly with
`/everlearn`.

## Where it saves (zero-config)

On first use, everlearn resolves your destination directory automatically and **remembers it**:

1. `$EVERLEARN_DIR` environment variable, if set
2. `~/.config/everlearn/config.json`, if it exists
3. **Auto-detected Obsidian vault** (reads Obsidian's vault registry). One vault → it proposes
   `<vault>/concepts/`; several → it asks which.
4. If none of the above, it asks you for a path.

Once chosen, it writes `~/.config/everlearn/config.json` so you're never asked again.

### Override manually (optional)

```bash
mkdir -p ~/.config/everlearn
cp config.example.json ~/.config/everlearn/config.json
# then edit "conceptsDir"
```

## Structure

```
everlearn/
├── SKILL.md                 # skill definition + instructions
├── config.example.json      # optional manual config override
└── reference/
    ├── note-template.md     # structure of the concept note
    └── demo-template.html   # structure/constraints of the HTML demo
```

## Philosophy

Learning shouldn't interrupt building. The material arrives contextualized to what you're
doing, it's interactive, and it stays in your notes for when you need it again.

## License

MIT — see [LICENSE](LICENSE).
