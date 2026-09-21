---
name: math-notation
description: House style for writing mathematical notation in Markdown (LaTeX delimiters, symbol conventions per course — group/ring/field theory, automata and Turing machine tuples, Big-O, formal grammars, logic). Use whenever any study-system agent writes a formula, theorem, or formal definition.
license: MIT
compatibility: opencode
metadata:
  audience: course-agents
  domain: study-system
---

## What I do

I keep math notation consistent across every course's notes so formulas render
correctly and mean the same thing everywhere they're used.

## Delimiters

- Inline math: `$...$` — e.g. `$a \equiv b \pmod{n}$`.
- Display math: `$$...$$` on its own line for anything with fractions, sums,
  multi-line derivations, or matrix/tuple notation.
- Never leave a formula as plain-text pseudo-math (`a^2 + b^2 = c^2` typed with
  `^` outside `$...$`) — always wrap it.
- For a multi-step derivation, use an `aligned` block so `=` signs line up:

```
$$
\begin{aligned}
f(n) &= 2f(n/2) + n \\
     &= 4f(n/4) + 2n \\
     &= \dots \\
     &= n \log n
\end{aligned}
$$
```

## Per-course symbol conventions

**Abstract Math (Groups/Rings/Fields)**

- Groups: $(G, \cdot)$ or $(G, +)$; identity $e$ (or $0$ additively);
  subgroup $H \le G$; normal subgroup $N \trianglelefteq G$; quotient
  $G/N$; order $|G|$, $\mathrm{ord}(g)$; homomorphism $\varphi: G \to H$;
  kernel $\ker\varphi$.
- Rings: $(R, +, \cdot)$; ideal $I \trianglelefteq R$; quotient ring $R/I$;
  units $R^\times$.
- Fields: $\mathbb{F}_q$ or $GF(q)$ for a finite field of order $q$; field
  extension $\mathbb{F} \subseteq \mathbb{K}$, written $\mathbb{K}/\mathbb{F}$.

**Computability Theory**

- A Turing machine as the 7-tuple
  $M = (Q, \Sigma, \Gamma, \delta, q_0, q_{accept}, q_{reject})$; always define
  every symbol the first time a TM appears in a file.
- Languages: $L(M)$; decidable class often written informally as "R" or
  "decidable", recognizable as "RE"/"recognizable" — say which convention the
  course uses and stick to it in that file.
- Reductions: $A \le_m B$ (many-one/mapping reduction).

**Syntax & Semantics**

- Grammar $G = (N, \Sigma, P, S)$; production $A \to \alpha$.
- Operational semantics rules as inference rules:

```
$$
\dfrac{e_1 \Downarrow v_1 \qquad e_2 \Downarrow v_2}{e_1 + e_2 \Downarrow v_1 + v_2}
$$
```

**Parallel & Distributed Processing**

- Big-O family: $O(\cdot)$, $\Omega(\cdot)$, $\Theta(\cdot)$.
- Speedup $S(p) = T_1 / T_p$; Amdahl's law
  $S(p) = \dfrac{1}{(1-f) + f/p}$ — always state what $f$ means in that file.

**AI**

- Search: state $s$, action $a$, path cost $g(n)$, heuristic $h(n)$,
  evaluation $f(n) = g(n) + h(n)$.
- Probability/logic as standard: $P(A \mid B)$, $\forall$, $\exists$,
  $\models$.

## General rules

- Define every symbol on first use in a file — don't assume the reader
  remembers a convention from a different lecture.
- Prefer a small glossary table (see `lecture-notes-writer`'s "Key
  definitions" section) over re-explaining notation inline every time.
- If OCR produced a formula you're not fully confident in (see `pdf-ocr`),
  say so rather than silently presenting a guess as certain.
