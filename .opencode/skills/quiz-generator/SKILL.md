---
name: quiz-generator
description: Generate a practice quiz (mixed multiple-choice and short-answer/problem questions, with a full answer key) from an existing lecture-NN-notes.md or a whole course unit. Use when the student asks to be quizzed, wants exam practice, or asks to "test me" on a lecture or topic.
license: MIT
compatibility: opencode
metadata:
  audience: course-agents
  domain: study-system
---

## What I do

I write exam-style practice questions that test understanding and problem
solving, not just recall (recall is what `flashcard-generator` is for), plus a
complete answer key with reasoning.

## When to use me

- On request: "quiz me on lecture 6", "give me a practice exam for the ring
  theory unit", "test me like the midterm would".
- Draw from `lecture-NN-notes.md` (and the sheet solutions, if a sheet exists
  for the same topic) rather than re-deriving from the raw PDF.

## Output location & naming

`level-04/semester-01/<course>/lectures/lecture-NN-quiz.md` for a single lecture, or
`level-04/semester-01/<course>/unit-quiz-<topic>.md` when asked to cover several lectures.

## Question design

- **Mix formats**: multiple-choice for definitions/discrimination between
  similar concepts, short-answer for "state and justify", and full
  problem/derivation questions for anything computational (build a parse
  table, run a reduction, trace an algorithm, prove a small claim).
- **Difficulty tiers**: label each question Easy / Medium / Hard, and include
  at least one Hard question per major topic that requires combining two
  ideas from the lecture, not just restating one.
- **Multiple-choice discipline**: exactly one correct option, distractors must
  be genuinely plausible mistakes (a common misconception, an off-by-one, a
  confusion with a similar theorem) — never an obviously silly option just to
  fill a slot.
- **Match the course's actual exam style** where known (e.g. Abstract Math and
  Computability Theory lean toward "state and prove/derive"; OOP in C++ and AI
  lean toward "trace this code" / "design this algorithm"; Parallel &
  Distributed and Syntax & Semantics mix both).

## Answer key format

Put the key in its own `## Answer Key` section at the end (not inline after
each question, so it can double as a closed-book test):

```markdown
**Q3.** <short restated answer, then the full reasoning — not just the letter
or the final number>
```

Every answer must show _why_, not just _what_, so a wrong guess still teaches
something when the student checks it.
