---
name: diagram-generator
description: House style for Mermaid diagrams used across course notes — automata/Turing-machine state diagrams, message-passing sequence diagrams, algorithm flowcharts, parse/derivation trees, and UML class diagrams. Use whenever a process, structure, or automaton is clearer as a picture than as prose.
license: MIT
compatibility: opencode
metadata:
  audience: course-agents
  domain: study-system
---

## What I do

I keep diagrams consistent and give a ready template per diagram type so
course agents don't have to invent Mermaid syntax from scratch each time.

## When to use me

Any time a topic is fundamentally about states, transitions, message flow,
tree structure, or class relationships — these render natively in Markdown
viewers/OpenCode and are far clearer than a paragraph describing the same
thing.

## Automata / Turing machines (Computability Theory)

```mermaid
stateDiagram-v2
  [*] --> q0
  q0 --> q1: 0
  q1 --> q1: 0
  q1 --> q2: 1
  q2 --> [*]
```

## Message passing / distributed algorithms (Parallel & Distributed)

```mermaid
sequenceDiagram
  participant P1
  participant P2
  participant P3
  P1->>P2: REQUEST(ts=3)
  P1->>P3: REQUEST(ts=3)
  P2-->>P1: REPLY
  P3-->>P1: REPLY
  Note over P1: enters critical section
```

## Algorithm flow / decision procedures (any course)

```mermaid
flowchart TD
  A[Start] --> B{Condition?}
  B -- yes --> C[Do X]
  B -- no --> D[Do Y]
  C --> E[End]
  D --> E
```

## Parse trees / derivations (Syntax & Semantics)

```mermaid
graph TD
  E1["E"] --> E2["E"]
  E1 --> Plus["+"]
  E1 --> T1["T"]
  E2 --> T2["T"]
  T2 --> id1["id"]
  T1 --> id2["id"]
```

## Class relationships (OOP in C++)

```mermaid
classDiagram
  class Shape {
    <<abstract>>
    +area() double
  }
  class Circle {
    -radius: double
    +area() double
  }
  Shape <|-- Circle
```

## Search / game trees (AI)

```mermaid
graph TD
  Root((Root, MAX)) --> A((A, MIN))
  Root --> B((B, MIN))
  A --> A1["3"]
  A --> A2["5"]
  B --> B1["2"]
  B --> B2["9"]
```

## Rules

- Keep diagrams small and single-purpose — one diagram per concept, not one
  giant diagram trying to show a whole lecture.
- Always caption a diagram with a one-line sentence explaining what to read
  off of it.
- For automata, label every transition with its input symbol; for sequence
  diagrams, label every message with what it actually carries (not just
  "message 1").
- If a diagram is a direct redraw of a slide figure, note that in the caption
  ("redrawn from slide 7") so the student can compare against the original.
