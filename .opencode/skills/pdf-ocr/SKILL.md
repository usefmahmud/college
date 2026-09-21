---
name: pdf-ocr
description: Extract clean text from lecture/sheet PDFs under level-04 (including scanned slides, image-heavy decks, and mixed Arabic/English pages) using pdftotext, pdftoppm and tesseract. Use before writing notes whenever a PDF's text layer is missing, garbled, or the slides are equation/diagram-heavy.
license: MIT
compatibility: opencode
metadata:
  audience: course-agents
  domain: study-system
---

## What I do

I turn a lecture/sheet PDF into text a course agent can actually read and reason
about, and I flag the parts that OCR cannot be trusted on (equations, diagrams,
hand-drawn figures) so those get transcribed by eye instead of guessed at.

## When to use me

- Any time a course agent is asked to explain `lecture-NN.pdf` or `sheet-NN.pdf`
  and hasn't already extracted its text this session.
- Any time direct extraction returns near-empty or clearly garbled output
  (common for scanned slides, slides exported as flattened images, or
  photographed whiteboards).

## Procedure

**0. Ensure the per-lecture folder exists and the PDF is inside it.**

```bash
mkdir -p "level-04/semester-01/<course>/lectures/lecture-NN"
mv "level-04/semester-01/<course>/lectures/lecture-NN.pdf" \
   "level-04/semester-01/<course>/lectures/lecture-NN/" 2>/dev/null || true
```

If the PDF is already inside `lectures/lecture-NN/`, the `mv` is a no-op.
Sheets follow the same pattern: `sheets/sheet-NN/sheet-NN.pdf`.

**1. Try direct text extraction first (fast, exact, no OCR errors).**

```bash
pdftotext -layout "level-04/semester-01/<course>/lectures/lecture-NN/lecture-NN.pdf" \
               "level-04/semester-01/<course>/lectures/lecture-NN/lecture-NN.raw.txt"
wc -w "level-04/semester-01/<course>/lectures/lecture-NN/lecture-NN.raw.txt"
```

`-layout` preserves column/table positioning, which matters for slides with
side-by-side diagrams+bullets or code blocks.

**2. Judge the quality before trusting it.**

Red flags that mean you need OCR instead:

- Word count is near zero relative to the number of pages.
- Output is dominated by single characters, `(cid:NN)` tokens, or repeated
  garbage symbols (classic sign of a custom/embedded font with no text layer).
- Math-heavy lines look like scrambled symbol soup.

**3. Fallback: rasterize pages, then OCR each page.**

```bash
mkdir -p /tmp/ocr/lecture-NN
pdftoppm -r 300 -png "level-04/semester-01/<course>/lectures/lecture-NN/lecture-NN.pdf" /tmp/ocr/lecture-NN/page
for f in /tmp/ocr/lecture-NN/page-*.png; do
  tesseract "$f" "${f%.png}" --psm 6
done
cat /tmp/ocr/lecture-NN/page-*.txt > "level-04/semester-01/<course>/lectures/lecture-NN/lecture-NN.raw.txt"
```

- `--psm 6` ("assume a uniform block of text") is a good default for slide
  bullets; try `--psm 4` for multi-column slides.
- If tools are missing, install them once: `apt-get install -y poppler-utils tesseract-ocr`
  (or `tesseract-ocr-ara` if Arabic text appears on slides, then run
  `tesseract "$f" "${f%.png}" -l eng+ara --psm 6`).

**4. Never trust OCR for math, diagrams, or hand-drawn figures.**

Tesseract regularly mangles subscripts, Greek letters, summation/product
signs, and arrows. For any slide containing formulas, automata diagrams,
circuit/pipeline diagrams, or graphs:

- Open the rasterized page image directly with the `view` tool and read it
  visually.
- Transcribe the math by hand into proper LaTeX (see the `math-notation`
  skill) rather than copying OCR output.
- Redraw diagrams as Mermaid (see the `diagram-generator` skill) instead of
  describing a mangled OCR dump of a picture.

**5. Use the raw extraction, then clean up.**

Read the `.raw.txt` file to produce structured notes via `lecture-notes-writer`,
then **delete all temporary artifacts** once the notes file is written:

```bash
rm -f "level-04/semester-01/<course>/lectures/lecture-NN/lecture-NN.raw.txt"
rm -rf /tmp/ocr/lecture-NN
```

Never hand `.raw.txt` to the student as-is — it always gets rewritten into the
structured notes format. The raw file is a working artifact, not a deliverable.

## Common pitfalls

- Running OCR on a PDF that already had a perfectly good text layer (slow and
  worse quality than direct extraction — always try `pdftotext` first).
- Trusting OCR'd numbers/formulas verbatim in a math-heavy course — always
  cross-check against the rasterized image.
- Forgetting `-layout`, which causes tables and two-column slides to interleave
  into nonsense.
- Silently dropping pages that failed OCR — report which page numbers were
  unreadable so the student knows to check the original PDF.
