---
name: flashcard-generator
description: Generate spaced-repetition flashcards (Q/A pairs, Anki-importable CSV) from an existing notes.md or sheet solutions. Use when the student asks for flashcards, wants to "drill" a lecture, or before Anki-style spaced-repetition review.
license: MIT
compatibility: opencode
metadata:
  audience: course-agents
  domain: study-system
---

## What I do

I turn already-written lecture notes into atomic recall-testing flashcards,
in both a human-readable Markdown form and an Anki-CSV form.

## When to use me

- On request: "make flashcards for lecture 4", "turn the ring theory unit into
  cards I can drill".
- Run after `lecture-notes-writer` (or `summary-generator`) has produced
  content to draw from — don't invent flashcards from the raw PDF directly,
  since the notes have already done the work of identifying what's important.

## Output location & naming

- `level-04/semester-01/<course>/lectures/lecture-NN/flashcards.md` — human-readable.
- `level-04/semester-01/<course>/lectures/lecture-NN/flashcards.csv` — two columns,
  `front,back`, no header row, ready for Anki's "Basic" note type import.

## Card-writing rules

- **Atomic**: one fact, one definition, or one small step per card. If a
  concept needs three things memorized, that's three cards, not one card with
  a three-part answer.
- **Active recall phrasing**: the front is a question or a fill-in-the-blank,
  never a statement to passively re-read. "What does the Halting Problem
  reduce _from_ in the standard undecidability proof?" not "The Halting
  Problem reduces from...".
- **Cover the whole hierarchy**: definitions, theorem statements, "when to use
  which algorithm/technique" decision cards, and worked-example-style cards
  ("what's the first step in reducing A to B?").
- **No ambiguous answers**: avoid fronts with more than one reasonable
  correct-sounding answer; if a term is overloaded across courses, name the
  course/context on the front.
- **Math/code on cards**: keep LaTeX/code short enough to read in a flashcard
  UI — one formula or one short snippet, not a derivation.
- Tag each card's source lecture in the Markdown version (not the CSV) so the
  student can trace a card back to the notes: `<!-- source: lecture-04 -->`.

## Markdown format

```markdown
## Lecture NN — <topic>

**Q:** <question>
**A:** <answer>

**Q:** <question>
**A:** <answer>
```

## CSV format

```csv
"<question, commas inside must stay quoted>","<answer>"
```
