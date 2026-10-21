---
course: Abstract Algebra for Computer Science (Math 333)
unit: lecture-01
source: lecture-01/lecture-01.pdf
generated: 2026-10-21
---

# Lecture 01 — Proof Cheat-Sheet

Every proof from Lecture 1 as a fill-in template. Read the left column ("what you must
prove"), copy the middle column ("what you write"), and use the right column only when
stuck.

---

## 0. The universal opener

| Situation | First sentence you write |
|---|---|
| proving a $\forall$ statement | "Let $a,b,c\in G$ be arbitrary." |
| proving an implication | "Suppose $X$. We must show $Y$." |
| proving uniqueness | "Suppose $x$ and $x'$ both satisfy the defining property." |
| disproving a $\forall$ | "Take $a=\dots$; then …" (one witness is enough) |
| proving a set is a group | "We verify (G1)–(G3) in order." |

---

## 1. "$(G,*)$ is a group" — six-step ritual

Write the six headings. Do not merge them.

| # | Heading | What must appear |
|---|---|---|
| 1 | **Well defined** | only if elements are classes: assume $[a]=[a']$, $[b]=[b']$, show $[a*b]=[a'*b']$ |
| 2 | **Closed** | take general $a,b\in G$, show $a*b\in G$ (and *why*: e.g. $a,c\neq0\Rightarrow ac\neq0$) |
| 3 | **Associative** | $(a*b)*c = \dots = a*(b*c)$, naming which associativity of $\mathbb{R}$/$\mathbb{Z}$ you borrow |
| 4 | **Identity** | solve $a*c=a$ for $c$ → candidate $e$; then verify $e\in G$ and $a*e=e*a=a$ |
| 5 | **Inverse** | solve $a*c=e$ for $c$ → candidate; verify $c\in G$; verify $a*c=c*a=e$ |
| 6 | **Commutative** | only if "abelian" is claimed: $a*b=\dots=b*a$ |

Closing sentence: "Therefore $(G,*)$ is a[n abelian] group. $\blacksquare$"

**Never** write a step as "obvious". Each identity/inverse step must *show the algebra
that produced the candidate* — that is the mark.

---

## 2. "This is not a group" — one line per failure

| Failing axiom | Template | Witness to reuse |
|---|---|---|
| closure | "$x=\dots,\ y=\dots\in S$ but $x*y=\dots\notin S$" | $\mathbb{N}$: $3-5=-2$ |
| associative | "$(a*b)*c=\dots\neq\dots=a*(b*c)$" | $\mathbb{Z}$: $(1-2)-3=-4\neq2$ |
| identity | "the only candidate is $e=\dots\notin G$" | $(2\mathbb{Z},\cdot)$: $e=1\notin2\mathbb{Z}$ |
| inverse | "suppose $a*b=e$; then $\dots$, contradiction" | $\mathbb{Q}$: $0\cdot b=0\neq1$; $M_2$: $\det=0$ |
| commutative | "$AB=\begin{bmatrix}8&1\\9&0\end{bmatrix}\neq\begin{bmatrix}2&7\\3&6\end{bmatrix}=BA$" | $GL(2,\mathbb{R})$ |

Also state the positive part: "…although it is closed, associative and has identity $I_2$,
so it is a monoid."

---

## 3. Uniqueness of identity

> Let $e$ and $e_1$ be identities. Since $e$ is an identity, $e_1*e=e_1$. Since $e_1$ is an
> identity, $e_1*e=e$. Hence $e_1=e$. $\blacksquare$

## 4. Uniqueness of inverse

> Let $b,c$ both satisfy $a*b=e=b*a$ and $a*c=e=c*a$. Then
> $$b=b*e=b*(a*c)=(b*a)*c=e*c=c.$$
> Hence $b=c$. $\blacksquare$

**Annotate the four equalities:** identity → substitution $e=a*c$ → associativity →
$b*a=e$ → identity.

## 5. $(a^{-1})^{-1}=a$

**Method A (lecture).** From $a^{-1}(a^{-1})^{-1}=e$, multiply on the left by $a$:
$a\,a^{-1}(a^{-1})^{-1}=a\cdot e$, regroup as $(aa^{-1})(a^{-1})^{-1}=a$, so
$e(a^{-1})^{-1}=a$.

**Method B (faster).** $a$ is an inverse of $a^{-1}$; $(a^{-1})^{-1}$ is also an inverse of
$a^{-1}$; by uniqueness they are equal.

## 6. $(ab)^{-1}=b^{-1}a^{-1}$

**Method A (manipulate).** $(ab)(ab)^{-1}=e$; left-multiply by $a^{-1}$, regroup
$a^{-1}(ab)=b$, left-multiply by $b^{-1}$, get $(ab)^{-1}=b^{-1}a^{-1}$.

**Method B (universal, use this one).** Show both products equal $e$:
$$
(ab)(b^{-1}a^{-1})=a(bb^{-1})a^{-1}=aa^{-1}=e,\qquad
(b^{-1}a^{-1})(ab)=b^{-1}(a^{-1}a)b=b^{-1}b=e .
$$
Then cite **uniqueness of inverses**. $\blacksquare$

*(Method B also proves $c^{-1}=a^{-1}b^{-1}$ style claims instantly: to show
"$X^{-1}=Y$", prove $XY=e=YX$.)*

## 7. Cancellation

> Suppose $ac=bc$. Multiply on the right by $c^{-1}$:
> $(ac)c^{-1}=(bc)c^{-1}\Rightarrow a(cc^{-1})=b(cc^{-1})\Rightarrow ae=be\Rightarrow a=b.$

Mirror for $ca=cb$ with $c^{-1}$ on the **left**. $\blacksquare$

**Trap:** cancellation needs an inverse. In $(\mathbb{Z}_6,\cdot)$,
$2\cdot3\equiv2\cdot0$ but $3\not\equiv0$.

---

## 8. Equivalence relation — three blocks

| Property | Template | Key line |
|---|---|---|
| reflexive | let $a\in A$ | $\frac{a-a}{n}=0\in\mathbb{Z}$ |
| symmetric | suppose $a\equiv b$, say $\frac{a-b}{n}=t\in\mathbb{Z}$ | $\frac{b-a}{n}=-t\in\mathbb{Z}$ |
| transitive | suppose $a\equiv b$, $b\equiv c$, say $\frac{a-b}{n}=r,\ \frac{b-c}{n}=s$ | $\frac{a-c}{n}=r+s\in\mathbb{Z}$ |

Finish: "By (1), (2), (3), $R$ is an equivalence relation. $\blacksquare$"

## 9. Well-definedness of $[a]+[b]=[a+b]$ (the skipped step)

> Let $[a]=[a']$ and $[b]=[b']$. Then $a-a'=nt$, $b-b'=ns$ with $t,s\in\mathbb{Z}$, so
> $a=a'+nt$, $b=b'+ns$. Hence
> $$[a]+[b]=[a+b]=[a'+b'+n(t+s)]=[a'+b']=[a']+[b'],$$
> because $(a'+b'+n(t+s))-(a'+b')=n(t+s)$ is a multiple of $n$.

**Say out loud:** "different representatives give the same class." That is the entire
content.

## 10. $\mathbb{Z}_n$ has exactly $n$ classes

> Every $a$: division algorithm $a=nq+r$, $0\le r<n$ ⇒ $[a]=[r]$.
> No repeats: if $0\le r<s<n$ then $0<s-r<n$ ⇒ $n\nmid(s-r)$ ⇒ $[r]\neq[s]$. $\blacksquare$

## 11. $(\mathbb{Z}_5,+)$ is an abelian group

1. well defined (template §9 with $n=5$)
2. closed: $[a]+[b]=[a+b]$ is a class
3. associative: $[a]+([b]+[c])=[a+(b+c)]=[(a+b)+c]=([a]+[b])+[c]$ — borrowed from $\mathbb{Z}$
4. identity $[0]$: $[a]+[0]=[a+0]=[a]$
5. inverse $[-a]$: $[a]+[-a]=[a-a]=[0]$
6. commutative: $[a+b]=[b+a]$

**Associativity is never readable from a Cayley table** — only closure, identity,
inverses and commutativity are.

---

## 12. Reading a Cayley table (exam one-liners)

- closed ⇔ every cell is an element of $G$
- $e$ ⇔ the row (and column) that copies the header
- $a^{-1}$ ⇔ column where row $a$ meets $e$
- abelian ⇔ symmetric about the main diagonal
- associative ⇔ **cannot** be read off; prove it

---

## 13. Seven traps that cost marks

1. Saying "$\mathbb{Z}$ is a group" without the operation.
2. Proving associativity before closure.
3. Guessing the identity instead of solving $a*c=a$.
4. Verifying $a*e=a$ only on one side when the group is non-abelian.
5. Writing $(ab)^{-1}=a^{-1}b^{-1}$.
6. Calling well-definedness "closure" (or vice versa) for $\mathbb{Z}_n$.
7. Concluding "$X^{-1}=Y$" from $XY=e$ alone without citing uniqueness.

---

## 14. Additive ↔ multiplicative dictionary

| multiplicative | additive |
|---|---|
| $ab$, $a^{-1}$, $e$, $a^n$ | $a+b$, $-a$, $0$, $na$ |
| $(ab)^{-1}=b^{-1}a^{-1}$ | $-(a+b)=(-b)+(-a)$ |
| $(a^{-1})^{-1}=a$ | $-(-a)=a$ |
| $ac=bc\Rightarrow a=b$ | $a+c=b+c\Rightarrow a=b$ |
| $o(a)$: least $n$ with $a^n=e$ | least $n$ with $na=0$ |
