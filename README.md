# everlearn

A Claude skill that turns *learning in the moment* into a connected part of your
**second brain** — without breaking your flow.

When you're mid-workflow and get stuck on a concept, everlearn explains it in the terms
of what you're actually doing, then saves it as an **interlinked note** in your Obsidian
vault. A passing question becomes a permanent, connected piece of your knowledge graph
instead of a throwaway answer. The concept can be technical or not — a code pattern, a
domain idea, a mental model.

## When it activates

- You're mid-workflow and don't understand something
- You ask "how does X work", "explain Y", "I don't get Z"
- You want to save a concept into your notes

## What it produces

- **A note** (`<concept>.md`) — always. A short, domain-agnostic note: what it is (no
  filler), why it matters in *your* context, the core with an example (code when it
  fits), **`[[wikilinks]]` to related notes** so it joins your graph, the reasoning
  behind it, and common mistakes.
- **An interactive demo** (`<concept>-demo.html`) — on request, for technical/visual
  concepts. A self-contained page (≤80 lines) you open in your browser, built around
  *your* domain and naming.
- **A deeper branch** — on request. Going deeper either expands the note, or spins up
  new linked sub-concept notes for the prerequisites you're missing — so a question
  grows your graph, not one giant note.

## Principles

- **Doesn't break your flow** — answers inline first; the note is the durable artifact.
- **Respects your second brain** — never restructures your vault; asks before creating a
  folder or touching an existing note.
- **Connects everything** — new notes link into the notes you already have.
- **Expensive things are opt-in** — the note is always written; demos and deep branches
  are offered, not forced.

## Install

```bash
git clone https://github.com/NeuralKaizen/everlearn.git
ln -s "$(pwd)/everlearn" ~/.claude/skills/everlearn
```

It triggers automatically when you get stuck mid-work, or invoke it with `/everlearn`.

## Where it saves (zero-config)

On first use, everlearn finds your destination and **remembers it**:

1. `$EVERLEARN_DIR`, if set
2. `~/.config/everlearn/config.json`, if it exists
3. **Your Obsidian vault** — it detects the vault, then *asks* where inside it the notes
   should live (proposing an existing folder if you have one). It never assumes a folder
   or creates one without your OK.
4. Otherwise, it asks for a path.

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
    └── demo-template.html   # structure/constraints of the optional HTML demo
```

## Philosophy

Learning shouldn't interrupt building — and it shouldn't evaporate either. everlearn
delivers the concept contextualized to what you're doing, then files it, linked, into the
brain you already trust.

## License

MIT — see [LICENSE](LICENSE).
