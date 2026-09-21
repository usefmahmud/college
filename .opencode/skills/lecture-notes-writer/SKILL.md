---
name: lecture-notes-writer
description: Turn extracted lecture/sheet text into a rich, self-contained Markdown explanation file following the study-system's shared notes template (TL;DR, full explanation, worked examples, glossary, practice questions, cheat-sheet). Every level-04 course agent uses this for every lecture/sheet it explains.
license: MIT
compatibility: opencode
metadata:
  audience: course-agents
  domain: study-system
---

## What I do

I define the one shared shape that every course agent's lecture notes must
follow, so notes for Computability Theory and notes for OOP in C++ look and
navigate the same way even though the content is totally different. A course
agent supplies the domain expertise; this skill supplies the structure and the
bar for depth.

## When to use me

Whenever a course agent is asked to "explain", "write notes for", or "break
down" a lecture or sheet PDF. Run `pdf-ocr` first if the text isn't already
extracted this session.

## The rule that matters most

**Do not paraphrase slide bullets. Reconstruct the reasoning the lecturer
compressed or skipped.** Slides are a compressed pointer to a 50-minute
explanation; your job is to expand each pointer back into the full
explanation — the "why", the intuition, the derivation, the edge case — not to
reformat the bullets with nicer punctuation. If a slide just states a theorem,
you give the proof or at least the proof sketch. If a slide shows a code
snippet, you explain what every line does and why it's written that way.

## Output location & naming

For `level-04/semester-01/<course>/lectures/lecture-NN/lecture-NN.pdf`, write:
`level-04/semester-01/<course>/lectures/lecture-NN/notes.md`

For `level-04/semester-01/<course>/sheets/sheet-NN/sheet-NN.pdf`, use the same template but title
it "Sheet NN — Worked Solutions" and put every sub-question's full worked
solution under "Full Detailed Explanation" instead of lecture topics; write to
`level-04/semester-01/<course>/sheets/sheet-NN/solved.md`.

## Template

```markdown
---
course: <course display name>
unit: lecture-NN | sheet-NN
source: lecture-NN/lecture-NN.pdf
generated: <date>
---

# Lecture NN — <topic title>

## TL;DR
- 3-6 bullets: the absolute must-remember takeaways, written so that reading
  only this section before an exam still gives real signal.

## Where this fits
- One or two sentences connecting this lecture to the previous one and to the
  course's overall arc. Note any prerequisite lecture/topic explicitly.

## Learning objectives
- What you should be able to *do* after this lecture (derive X, apply Y to a
  new instance, distinguish A from B), not just "understand Z".

## Full detailed explanation
For each topic on the slides, in order, write a subsection with:
- **Intuition first** — the plain-language idea before any formalism.
- **Formal definition/statement** — precise, using the `math-notation` skill's
  conventions where relevant.
- **Why it's true / how it works** — derivation, proof, or mechanism, not just
  the result. If the lecturer skipped a step, fill it in.
- **Worked example** — a concrete instance carried through step by step, with
  every intermediate step shown (no "it follows that" jumps).
- **Common pitfalls** — the mistake students actually make here.
Use a `###` subsection per topic. Insert a Mermaid diagram (via the
`diagram-generator` skill) anywhere a process, automaton, tree, or pipeline is
easier to see than to read.

## Key definitions & theorems
A Markdown table: | Term | Precise statement | Plain-English meaning |

## Connections
- Bullet links to other lectures/courses this builds on or feeds into
  (e.g. "the pumping lemma here is what Computability Theory Lecture 6 relies
  on"). Use `[[relative/path.md]]`-style links where the target file exists.

## Quick self-check
3-6 short questions with the answer collapsed under a `<details>` block, so
the student can test recall before opening the full quiz/flashcards.

## One-page cheat-sheet
The absolute condensed version: definitions, formulas, and decision rules only,
formatted for a last-minute re-read. (If this is getting long, generate it as
a separate file instead — see the `summary-generator` skill.)

## Source reference
- Slide/page numbers each subsection was drawn from, so the student can go
  back to the original PDF for anything this file compresses too much.
```

## Depth calibration

- Go deep enough that the student could reconstruct the lecture from these
  notes alone without rewatching it — that's the bar, not "cover the slide
  titles."
- Every formula gets a sentence saying where it comes from or what it means,
  never a bare equation.
- Every code snippet gets explained line-by-line for anything non-obvious.
- Flag, explicitly, anything the source PDF states without justification —
  "the slides assert this without proof; here's the standard argument" — so
  the student can tell lecture content apart from your added scaffolding.