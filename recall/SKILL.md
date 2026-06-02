---
name: recall
allowed-tools: Read, Grep, Glob, Bash(grep *), Bash(rg *), Bash(cat *), Bash(ls *), Bash(python3 *)
disallowed-tools: Write, Edit
description: >-
  Retrieve context from your own second brain — answer from YOUR notes, not general
  knowledge. Part of the loom collection. Trigger on retrospective/possessive questions:
  "¿por qué hago/uso/elegí X?", "¿qué aprendí/sé sobre Y?", "¿qué decidí sobre Z?", "¿en
  qué está el proyecto W?", "buscá en mis notas…", "recordame por qué…". Searches the
  vault (concepts/decisions/logs/projects) by type, tags, content, kind, and project,
  then answers concisely with [[links]] to the source notes. READ-ONLY — never writes or
  modifies notes. (To LEARN a new concept, that's everlearn; recall retrieves what's
  already captured.)
---

# recall

recall answers from **your own second brain**. When you ask "why do I use X?" or "what did I learn about Y?", it searches the notes the loom skills captured (see `../CONTRACT.md`) and answers from *them* — grounded, cited, yours — not from generic knowledge.

It is **read-only**: it never creates or edits notes. Its output is the answer, with `[[links]]` to where each piece came from.

## Core principles

1. **Answer from THEIR notes.** recall's whole value is "what does *my* vault say." Ground every answer in the captured notes.
2. **Cite everything.** Each claim links to its source note via `[[wikilink]]`. No citation, no claim.
3. **Be honest about gaps.** If the vault has little or nothing on it, say so plainly — never pass generic knowledge off as theirs. Offer to capture it (everlearn / decide / log).
4. **Route by intent** (below).
5. **Concise** — synthesize, don't dump. The answer plus its citations; then offer to go deeper.
6. **Match their language.**

## When to activate

Retrospective / possessive questions about what they already know or decided:

- "¿por qué hago / uso / elegí X?" → their decisions.
- "¿qué aprendí / sé sobre Y?" → their concepts + logs.
- "¿qué decidí sobre Z?" → their decisions.
- "¿en qué está el proyecto W?" / "¿qué hay de W?" → the project hub + its members.
- "buscá en mis notas…", "recordame…", "¿qué tengo sobre…?".

NOT "¿cómo funciona X?" (that's learning something new → everlearn). recall retrieves what's already there.

## Intent → where to look

| The question | Look in | Lead with |
|---|---|---|
| "why do I do/use X" | `decisions/` (`type: decision`) | the decision + its reasoning + the alternatives ruled out |
| "what did I learn about Y" | `concepts/`, `logs/` | the concept's core + any logged insights |
| "state of project W" | `projects/W.md` + notes with `project: "[[W]]"` | the hub's status + recent decisions/logs |
| "show my errors / reflections" | `logs/` filtered by `kind:` | the matching entries |

## How to search

1. Resolve the vault root (`~/.config/loom/config.json` → `vault`; else bootstrap from `conceptsDir`'s parent; else ask).
2. Search broadly with ripgrep across frontmatter (`tags:`, `type:`, `kind:`, `project:`), titles, and body. Prefer the folder the intent points to, but don't ignore the rest.
3. Read the handful of best matches — not everything.
4. Synthesize a concise answer grounded in them, citing each with `[[links]]`.
5. If nothing matches, say so and offer to capture it (hand off to everlearn / decide / log).

## Workflow

1. Read the question; pick the intent and target folder(s).
2. Search → read the top matches.
3. Answer concisely, inline, citing `[[notes]]`. **Never write a file.**
4. Offer to go deeper or to open a specific note.

recall touches no files. It only reads.
