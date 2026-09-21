---
name: summary-generator
description: Condense an already-written lecture-NN-notes.md into a one-page, last-minute-review summary or exam cheat-sheet. Use after lecture-notes-writer has produced full notes, or when the student explicitly asks for a shorter version / cheat-sheet / TL;DR of a lecture, sheet, or whole course.
license: MIT
compatibility: opencode
metadata:
  audience: course-agents
  domain: study-system
---

## What I do

I compress full lecture notes down to what fits on one screen without losing
anything a student would actually need in the last hour before an exam.

## When to use me

- Right after `lecture-notes-writer` finishes a lecture, if the student wants
  a standalone condensed file (common when the full notes run long).
- On request: "summarize lecture 5", "give me a cheat sheet for the whole
  ring theory unit", "condense sheet 3's solutions".
- Building a per-course master cheat-sheet by concatenating per-lecture
  summaries before an exam.

## Output location & naming

Single lecture: `level-04/semester-01/<course>/lectures/lecture-NN-summary.md`
Whole-unit/course: `level-04/semester-01/<course>/course-cheatsheet.md` (append a new
`## Lecture NN` section each time you're asked to extend it, don't overwrite
previous lectures' sections).

## Compression rules

- Start from the **existing** `lecture-NN-notes.md` if it exists — don't
  re-derive from the raw PDF, that's `lecture-notes-writer`'s job and
  duplicating it wastes the depth already captured.
- Keep: every definition, every formula, every named theorem/algorithm,
  decision rules ("use X when Y, use Z when W"), and the one-line intuition
  for each — the compressed memory hook, not the full explanation.
- Cut: intuition build-up, full proofs/derivations (keep proof *technique
  name* only, e.g. "proof by diagonalization"), narrative connective tissue,
  and the source-reference section.
- Format as dense bullets and tables, not paragraphs. Bold every term being
  defined. One line per fact where at all possible.
- Preserve any Mermaid diagram from the source notes that captures a whole
  process in one glance (state diagrams, decision flowcharts) — these compress
  better than prose.
- Target length: short enough to review in under 5 minutes per lecture.

## Course-level cheat-sheet assembly

When asked for a whole-course cheat-sheet, organize by topic (not
chronologically by lecture) if topics recur across lectures — e.g. group every
mention of "mutual exclusion algorithms" together even if it was taught across
two lectures. Add a short "how topics connect" map at the top.