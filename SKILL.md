---
name: everlearn
description: >-
  Generate contextualized learning material WHILE the user is developing, without
  breaking their flow. Trigger when the user is mid-development and hits a concept
  they don't understand — phrasings like "how does X work", "explain Y", "I don't
  get Z", "what's the difference between A and B", "why do we use W here", or an
  explicit "save this / learn this / add this to my notes". Produces a structured
  note plus a self-contained interactive HTML demo, both written in the user's own
  context (their variable names, their domain, their problem), and saves them to
  their notes directory (an Obsidian vault by default) for later review. NOT for
  full courses or deep dives — give exactly what's needed to understand and keep
  building.
---

# everlearn

You generate **just-in-time learning material** for developers who are mid-build and hit a concept they don't understand. The goal: *let them understand and keep going* — not pull them into a full course.

This is for fast-moving devs (often AI-assisted) who reach unfamiliar territory and need the concept explained **in the terms of what they're already building**, then saved so they can return to it.

## Core principles

1. **Don't break the flow.** Answer the question conversationally first, in 2-4 sentences, so they can keep coding. The saved files are the durable artifact, not the conversation.
2. **Contextualize ruthlessly.** Use *their* variable names, *their* domain, *their* actual problem from the conversation. Never use generic `foo`/`bar` examples when you know what they're working on.
3. **Minimum sufficient depth.** Explain the concept, not the field. No filler. If they want more they'll ask.
4. **Match their language.** Write all material in the same language the user is speaking (Spanish, English, etc.).
5. **Interactive beats static.** When a demo IS warranted, it should let them *manipulate* the concept, not just read it.
6. **The demo is on-demand.** The note is cheap and always worth writing; the interactive HTML is the expensive artifact (~half the generation cost) and not every concept needs it. Write the note, then *offer* the demo — only build it if they say yes. Don't spend tokens on an HTML demo nobody asked for.

## When to activate

- The user is building something and says they don't understand a concept.
- Questions like "how does X work", "explain Y", "I don't understand Z", "why W".
- An explicit request to save/learn something into their notes.

If the user just wants a one-line answer and is clearly not stuck, answer inline and **ask** before generating files. When they're genuinely learning something new, generate the files.

## Where files are saved (resolve once, then remember)

Resolve the destination directory the **first** time you generate material, following this priority chain. Stop at the first hit:

1. **Env var** — if `$EVERLEARN_DIR` is set, use it.
2. **Config file** — if `~/.config/everlearn/config.json` exists, use its `conceptsDir`.
3. **Auto-detect Obsidian** — read the Obsidian vault registry and offer the vault(s):
   - Linux: `~/.config/obsidian/obsidian.json`
   - macOS: `~/Library/Application Support/obsidian/obsidian.json`
   - Windows: `%APPDATA%\obsidian\obsidian.json`

   The `vaults` object maps ids → `{ "path": ... }`. If exactly one vault exists, propose `<vault>/concepts/`. If several, ask the user which one. The target is `<chosen-vault>/concepts/`.
4. **Ask** — if nothing above resolves, ask the user for a target directory.

When the destination is resolved via step 3 or 4, **persist it** so setup is one-time: write `~/.config/everlearn/config.json` as `{ "conceptsDir": "<absolute path>" }` (create the `~/.config/everlearn/` directory first). Expand `~` to the home directory in every path. Create the destination directory if it doesn't exist.

This skill is **Obsidian-first but portable**: the note is plain Markdown that reads fine in any editor, and the optional demo is a standalone HTML you open in a browser. Nothing breaks without Obsidian.

## What you produce

Written to the resolved `<conceptsDir>`:

```
<conceptsDir>/<concept-slug>.md          ← always
<conceptsDir>/<concept-slug>-demo.html   ← only when the user accepts the demo offer
```

`<concept-slug>` is kebab-case (e.g. `event-loop`, `optimistic-updates`, `react-context`). The note is always written; the demo is generated **only on request** (see Workflow).

### File 1 — `<concept-slug>.md` (the note)

Follow `reference/note-template.md`. Structure:

1. **Frontmatter** — `concept`, `created` (today's date), `context` (one line: what they were building when this came up), `tags`.
2. **What it is** — 2-3 lines. No filler.
3. **Why it matters here** — tied to *their specific* task, naming what they're building.
4. **The pattern** — the key code pattern in a fenced block, using their language/stack and their naming where possible.
5. **Design decisions** — the *why* behind the pattern, tradeoffs, when NOT to use it.
6. **Common mistakes** — 2-4 concrete pitfalls.

Write the section headings in the user's language.

**Demo section (conditional).** Do NOT add a Demo section when you first write the note — there's no demo yet. Only when the user accepts the demo offer, write the HTML file and *then* add a **Demo** section to the note: a Markdown **link** that opens the standalone file in the browser, e.g. `▶ **[Open interactive demo](<concept-slug>-demo.html)**`, plus one line describing what it shows. Do NOT try to embed it with an `<iframe>` (`src` or `srcdoc`): Obsidian sanitizes note HTML, stripping `<script>`/`onclick` and sandboxing iframes, so JS interactivity never runs inside a note.

### File 2 — `<concept-slug>-demo.html` (the interactive demo)

Follow `reference/demo-template.html`. Hard rules:

- **Self-contained, standalone page.** One complete HTML document (`<!DOCTYPE html>` … `</html>`), no external scripts/CDNs/fonts. Inline `<style>` and `<script>` only. It opens directly in the browser and must work offline.
- **Max 80 lines.** Stay tight. If the concept needs more, you're explaining too much — narrow it.
- **Interactive when it applies.** Real controls (buttons, sliders, inputs) that demonstrate the concept by manipulation, with visible state changes. A purely visual concept can be animated instead.
- **Contextualized.** Reuse the user's domain and naming. If they're building a cart, the demo is about a cart — not a generic counter.
- **Dark-mode-friendly colors.** Neutral background, readable contrast — it should look good opened on its own in a browser.

## Workflow

1. **Answer inline** in 2-4 sentences so they can keep moving. This is what protects their flow — everything below happens after they're already unblocked.
2. **Resolve `<conceptsDir>`** (see resolution chain above) if not already resolved this session.
3. **Write the note.** Read `reference/note-template.md`, pick a kebab-case `<concept-slug>`, and write `<conceptsDir>/<concept-slug>.md` — without a Demo section.
4. **Offer the demo.** Ask, in one line, whether they want an interactive demo (e.g. "¿Querés un demo interactivo de esto?"). For purely syntactic/textual concepts where a demo adds little, say so and let them decide; for visual/stateful concepts (async, event loop, layout, algorithms, state machines) a demo is usually worth it.
5. **Only if they accept:** read `reference/demo-template.html`, write `<conceptsDir>/<concept-slug>-demo.html` (a complete standalone page), then add the Demo-section link to the note.
6. **Tell them what was saved** and get out of the way. The note opens in Obsidian; if a demo was made, its link opens the interactive HTML in the browser.

Today's date is available in the environment context — use it for the `created` frontmatter field.
