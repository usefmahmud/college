# College

Level-04 lecture notes, flashcards, quizzes, and summaries — generated and maintained by [OpenCode](https://opencode.ai) agents.

## Courses

| Course                                        | Folder                                |
| --------------------------------------------- | ------------------------------------- |
| Parallel and Distributed Processing           | `level-04/semester-01/parallel/`      |
| Syntax and Semantics of Programming Languages | `level-04/semester-01/syntax/`        |
| Artificial Intelligence                       | `level-04/semester-01/ai/`            |
| Computability Theory                          | `level-04/semester-01/computability/` |
| Object-Oriented Programming in C++            | `level-04/semester-01/oop/`           |
| Abstract Mathematics                          | `level-04/semester-01/math/`          |

## File structure

Each lecture and sheet gets its own folder:

```
lectures/
  lecture-01/
    lecture-01.pdf        # source PDF from the lecturer
    notes.md              # full detailed notes
    summary.md            # one-page cheat sheet
    flashcards.md         # Q/A pairs for spaced repetition
    flashcards.csv        # Anki-importable version
    quiz.md               # practice exam with answer key

sheets/
  sheet-01/
    sheet-01.pdf
    solved.md             # worked solutions
```

Course-level files sit in the course root:

```
parallel/
  course-cheatsheet.md    # master cheat sheet (all lectures combined)
  unit-quiz-<topic>.md    # multi-lecture practice quizzes
```

## How it works

This repo uses OpenCode with custom **skills** and **agents** (in `.opencode/`) to automate note generation:

1. A lecture PDF is placed in `lectures/` (or `sheets/`).
2. The course agent extracts text via `pdf-ocr` (pdftotext / tesseract).
3. `lecture-notes-writer` produces structured notes with definitions, proofs, examples, and diagrams.
4. `summary-generator`, `flashcard-generator`, and `quiz-generator` produce additional study materials from the notes.

### Skills

| Skill                  | Purpose                                                     |
| ---------------------- | ----------------------------------------------------------- |
| `pdf-ocr`              | Extract text from PDFs (direct extraction or OCR fallback)  |
| `lecture-notes-writer` | Write full structured lecture notes                         |
| `summary-generator`    | Compress notes into a one-page cheat sheet                  |
| `flashcard-generator`  | Generate Anki-compatible flashcards                         |
| `quiz-generator`       | Generate practice quizzes with answer keys                  |
| `math-notation`        | LaTeX conventions across all courses                        |
| `diagram-generator`    | Mermaid diagram templates (automata, sequences, flowcharts) |

### Agents

| Agent                  | Course                                        |
| ---------------------- | --------------------------------------------- |
| `parallel-distributed` | Parallel and Distributed Processing           |
| `syntax-semantics`     | Syntax and Semantics of Programming Languages |
| `computability`        | Computability Theory                          |

Dependencies are installed automatically by the agents when needed.
