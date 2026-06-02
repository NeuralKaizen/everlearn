---
name: everlearn
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(python3 *), Bash(mkdir *), Bash(ls *), Bash(grep *), Bash(cat *)
description: >-
  Generate contextualized learning material WHILE the user works — without breaking
  their flow — and weave it into their Obsidian "second brain". Trigger when the user
  is mid-workflow and hits a concept they don't understand (technical OR not) —
  phrasings like "how does X work", "explain Y", "I don't get Z", "what's the
  difference between A and B", "why do we use W here", or an explicit "save this /
  learn this / add this to my notes". Writes a structured, interlinked note into
  their vault — connected to related notes via [[wikilinks]] — and, only on request,
  an interactive HTML demo or a deeper branch of linked sub-concept notes. Asks
  before creating folders or touching existing notes. NOT for full courses or deep
  dives — give exactly what's needed to understand and keep going.
---

# everlearn

everlearn is the bridge between **learning in the moment** and the user's **second brain**. When someone is mid-workflow and hits a concept they don't understand, it explains it in the terms of what they're actually doing — then saves it as an *interlinked* note in their Obsidian vault. A passing question becomes a permanent, connected piece of their knowledge graph instead of a throwaway answer.

The user is often a fast-moving (often AI-assisted) developer, but the concept can be anything — a technical pattern, a domain idea, a mental model. Don't assume it's code.

## Core principles

1. **Don't break the flow.** Answer the question conversationally first, in 2-4 sentences, so they can keep working. The saved note is the durable artifact; everything else happens *after* they're already unblocked.
2. **Contextualize ruthlessly.** Use *their* actual work — their names, their domain, their problem from the conversation. Never generic `foo`/`bar` examples when you know the real context.
3. **Minimum sufficient depth.** Explain the concept, not the field. No filler. They can ask to go deeper, and you offer it.
4. **Match their language.** Write all material in the language the user is speaking.
5. **Respect the second brain, but stay out of the way.** Their vault is theirs — never restructure, rewrite, or reorganize existing notes. But you don't need to ask permission to *add a single contextual `[[link]]`* into a clearly related existing note — that's safe and additive, so do it automatically. Ask only before structural changes (creating a new folder). The guardrail: into an existing note you only ever *add one contextual link*, never edit its content.
6. **Connect into the graph.** A note that isn't linked is lost. Link every new note to related notes via `[[wikilinks]]` so it joins their knowledge graph.
7. **Expensive artifacts are on-demand.** The note is cheap and always worth writing. The interactive HTML demo and deeper branches cost real tokens and aren't always needed — *offer* them, build only on acceptance.

## When to activate

- The user is mid-workflow (coding or otherwise) and says they don't understand a concept.
- Questions like "how does X work", "explain Y", "I don't understand Z", "why W".
- An explicit request to save/learn something into their notes.

If they just want a one-line answer and aren't really stuck, answer inline and **ask** before writing anything. When they're genuinely learning something new, write the note.

## Where notes are saved (resolve once, then remember)

Resolve the destination directory the **first** time you write material. Stop at the first hit:

1. **Env var** — if `$EVERLEARN_DIR` is set, use it.
2. **Config file** — if `~/.config/everlearn/config.json` exists, use its `conceptsDir`.
3. **Auto-detect the vault, then ask where inside it.** Read the Obsidian vault registry:
   - Linux: `~/.config/obsidian/obsidian.json`
   - macOS: `~/Library/Application Support/obsidian/obsidian.json`
   - Windows: `%APPDATA%\obsidian\obsidian.json`

   The `vaults` object maps ids → `{ "path": ... }`. Pick the vault (ask if several). Then **do NOT assume a `concepts/` folder** — list the vault's existing folders and:
   - If there's an obvious home for learning notes (e.g. `Concepts`, `Notes`, `Permanent`, `Zettel`, `Slipbox`, `Reference`), **propose** it.
   - Otherwise propose creating one (suggest a name, let them rename).

   **Ask the user to confirm the destination before creating anything.**
4. **Ask** — if no vault is found, ask the user for a target directory.

Once resolved via step 3 or 4, **persist it** (one-time setup): write `~/.config/everlearn/config.json` as `{ "conceptsDir": "<absolute path>" }` (create `~/.config/everlearn/` first). Expand `~` in every path. Create the destination folder **only after the user confirms it**.

This skill is **Obsidian-first but portable**: the note is plain Markdown that reads fine anywhere, and the optional demo is a standalone HTML you open in a browser. Nothing breaks without Obsidian — but the `[[wikilink]]` graph is where it shines.

## The note (always written)

Write `<conceptsDir>/<concept-slug>.md`, following `reference/note-template.md`. `<concept-slug>` is kebab-case (e.g. `event-loop`, `sunk-cost`, `optimistic-updates`). Headings go in the user's language.

The structure is **domain-agnostic and adaptive**:

1. **Frontmatter** — follow the shared note contract (`../CONTRACT.md`): `type: concept`, `created` (today's date), `tags` (topic tags — thread colors by these), `context` (one line: what they were doing when this came up).
2. **What it is** — 2-3 lines. No filler.
3. **Why it matters here** — tied to *their specific* task.
4. **The core** — how it works, with example(s). If the concept is technical, this is a minimal **code pattern** in a fenced block using their stack and naming. If it's not, it's a concrete **worked example or analogy**. Code is just one kind of example — include it only when it fits.
5. **Connections** — `[[wikilinks]]` to related notes. Before writing, scan the existing notes in `<conceptsDir>` (and the wider vault when useful) and link the genuinely related ones. You may also add `[[adjacent-concept]]` ghost links for closely-related ideas worth learning next. Link liberally, but only what's *truly* related.
6. **Why it works this way** — the reasoning, tradeoffs, when NOT to apply it (for non-technical concepts: the nuance, where it breaks down).
7. **Common mistakes** — 2-4 concrete pitfalls.

A **Demo** section is added later, and only if the user accepts the demo offer (see below).

## Optional, on request only

### Interactive demo
For technical/visual concepts (async, event loop, layout, algorithms, state machines). Follow `reference/demo-template.html`:

- **Self-contained standalone page.** One complete HTML doc (`<!DOCTYPE html>` … `</html>`), no CDNs/fonts, inline `<style>`/`<script>` only. Opens in a browser; works offline.
- **Max 80 lines.** Stay tight.
- **Interactive when it applies** — real controls with visible state changes.
- **Contextualized** to their domain and naming.
- **Dark-mode-friendly** colors.

When built, add a **Demo** section to the note: a Markdown **link** that opens the file in the browser (`▶ **[Open interactive demo](<concept-slug>-demo.html)**`) plus one line on what it shows. Do NOT embed via `<iframe>` (`src` or `srcdoc`) — Obsidian sanitizes note HTML, strips `<script>`/`onclick` and sandboxes iframes, so JS never runs inside a note.

### Go deeper
When the user says the explanation was thin, or a sub-concept surfaces, the deciding question is: **is this sub-concept inside or outside the current topic?**
- **Inside the topic → expand in place.** If it's a variant, mode, or extension of the same concept (e.g. `Promise.allSettled` while explaining `Promise.all`, or a deeper detail of the same idea), add it as a section to the **same note**. Don't spawn a near-duplicate note for something that belongs with its parent — that just fragments the graph.
- **Outside the topic → new linked note.** Only when a genuinely *distinct* concept the explanation leans on comes up (e.g. "kernel" surfacing while explaining Docker, or a real prerequisite that stands on its own) do you create a NEW note for it (same structure) and link it via `[[ ]]`. This is what grows the graph meaningfully.

When unsure, prefer expanding in place — a new note should earn its existence by being its own concept, not a footnote of the current one.

### Link into existing notes
Connect *into* the existing graph, not just out of the new note: when an existing note is clearly related, **add a single contextual `[[link]]`** to the new concept inside it — **automatically, no permission needed**. The guardrail keeps this safe: you only ever *add one contextual link*, never rewrite, reorder, or otherwise edit the note's content. Mention which notes you touched in your final summary.

## Workflow

1. **Answer inline** in 2-4 sentences so they can keep moving. This protects their flow — everything below happens after they're already unblocked.
2. **Resolve `<conceptsDir>`** (see resolution chain) if not already resolved this session. Ask before creating any folder.
3. **Write the note.** Read `reference/note-template.md`, pick a `<concept-slug>`, scan existing notes in `<conceptsDir>` for link targets, and write the note with its **Connections** section. No Demo section yet.
4. **Auto-link into the graph.** If existing notes are clearly related, add one contextual `[[link]]` into each — automatically, no asking (see "Link into existing notes").
5. **Offer the expensive extras**, in one or two lines — only the ones that apply: an interactive demo (technical/visual concepts) and/or going deeper. These cost real tokens, so they're opt-in. Don't offer what doesn't fit.
6. **Act only on what they accept:**
   - Demo → write the `.html`, add the Demo link to the note.
   - Deeper → expand the note in place, or (only for an outside-the-topic concept) branch into a new linked note.
7. **Tell them what was saved** (including any existing notes you linked into) and get out of the way.

Today's date is available in the environment context — use it for the `created` frontmatter field.
