---
course: COMP 403 — Parallel and Distributed Processing
unit: unit-quiz-synchronization (lectures 01–03)
source: lecture-01.pdf, lecture-02.pdf, lecture-03.pdf
generated: 2026-10-17
---

# Practice Quiz — Concurrency & Synchronization (Lectures 01–03)

**Coverage:** Lecture 01 (models, correctness, semaphores, producer–consumer), Lecture 02 (bounded buffer, Dining Philosophers, fork–join), Lecture 03 (Readers–Writers, monitors, condition variables).
**Format:** 8 multiple-choice, 6 short-answer, 6 problems/traces. Each question is tagged **[Easy] / [Medium] / [Hard]**.
Closed-book: answer first, then check the `## Answer Key` at the end.

---

## Section A — Multiple Choice

**Q1. [Easy]** Which statement best distinguishes a **shared-memory** system from a **distributed-memory** system?
- A. Shared memory communicates by message passing; distributed memory communicates by read/write to a global array.
- B. Shared memory gives all processors a single global address space; distributed memory gives each processor private memory connected by a network.
- C. Shared memory never needs synchronization; distributed memory always does.
- D. Shared memory spans many failure domains; distributed memory is a single failure domain.

**Q2. [Easy]** A semaphore is a compound data structure $s=(w,q)$. Which describes it?
- A. $w$ is a real number; $q$ is a set of resources.
- B. $w$ is a nonnegative integer; $q$ is a FIFO queue of blocked tasks.
- C. $w$ is the queue; $q$ is the counter.
- D. $w$ and $q$ are both nonnegative integers.

**Q3. [Easy]** To protect a critical section shared by $n$ tasks, the **binary semaphore** is initialised to:
- A. $(0,\emptyset)$
- B. $(1,\emptyset)$
- C. $(n,\emptyset)$
- D. $(-1,\emptyset)$

**Q4. [Medium]** A `signal` is executed on a **condition variable** that currently has **no** waiting process. What happens?
- A. A counter is incremented so a future waiter can proceed.
- B. The signal is **lost** (it has no effect).
- C. The signalling process blocks until a waiter appears.
- D. A process waiting on a *different* condition variable is woken.

**Q5. [Medium]** In the Dining Philosophers problem with five forks, the maximum number of philosophers that can eat **simultaneously** is:
- A. 1
- B. 2
- C. 3
- D. 4

**Q6. [Medium]** Which property does a **monitor** guarantee *automatically*, without explicit semaphore operations on the shared data?
- A. Starvation freedom.
- B. Deadlock freedom.
- C. Mutual exclusion of its procedures.
- D. No lost wake-ups.

**Q7. [Medium]** The first (reader-priority) Readers–Writers solution is described as "not entirely correct". Why?
- A. It fails to provide mutual exclusion on the database.
- B. Writers can be starved.
- C. Readers can read inconsistent data.
- D. It deadlocks whenever a reader and a writer arrive together.

**Q8. [Hard]** In the no-starvation Readers–Writers solution, a process that wakes a parked reader/writer (via `signal(reading_allowed)` or `signal(writing_allowed)`) deliberately does **not** also execute `signal(s)`. Why?
- A. Because `s` is used only by writers.
- B. Because the wake-up **hands the `s` lock over** to the woken process, which releases it later (baton passing).
- C. Because `s` was never acquired in that branch.
- D. Because doing so would deadlock the woken process on `s.q`.

---

## Section B — Short Answer

**Q9. [Easy]** Define a **safety** property and a **liveness** property, and give one concrete violation of each drawn from Lectures 01–03.

**Q10. [Medium]** Explain why semaphore `wait` and `signal` must be **atomic**, using a concrete interleaving that breaks a binary-semaphore critical section if they are not.

**Q11. [Medium]** For the **bounded** producer–consumer, state the invariant relating `nonempty.w`, `nonfull.w`, and the buffer size $n$, and explain why the **unbounded** version needs only one semaphore while the bounded version needs two.

**Q12. [Medium]** Why must a bounded buffer be implemented as a **cyclic** array (`in := (in+1) mod n`)? State precisely what goes wrong with linear indices.

**Q13. [Hard]** In the no-starvation Readers–Writers solution, correctness condition 1 says "a new reader is blocked if a **writer is waiting** for access", yet the reader code tests `if nw > 0` (a writer is **inside** the database). Reconcile these two statements.

**Q14. [Medium]** Why is it recommended that `signal` be the **last** operation performed in a monitor procedure?

---

## Section C — Problems & Traces

**Q15. [Medium]** Bounded producer–consumer with buffer size $n=3$, initially empty (`nonempty=(0,∅)`, `nonfull=(3,∅)`, `counter=0`, `in=out=0`). Trace the following operations, giving `counter`, `nonempty.w`, `nonfull.w` after each, and the value of `out`/`in`:
1. Producer puts `x`
2. Producer puts `y`
3. Consumer gets one item
4. Producer puts `z`
5. Consumer gets, Consumer gets
State whether the buffer is empty, partly full, or full at the end, and confirm the invariant. Then: if the producer tried to put a **4th** item before any consumer ran, what would happen and why?

**Q16. [Hard]** Give the exact schedule that produces **deadlock** in the naive Dining Philosophers solution (left fork, then right), and state the resulting wait-for cycle. Then explain in one sentence why adding `up_to_4 := (4,∅)` removes that cycle.

**Q17. [Hard]** Given the semaphores and tasks below, enumerate **every** string the three tasks can print (each task runs once).

```text
s1: semaphore := (1, ∅);
s2: semaphore := (0, ∅);

task body A is        task body B is        task body C is
  wait(s1);             wait(s2);             wait(s1);
  write('A');           write('B');           write('C');
  signal(s1);           signal(s1);           signal(s2);
end A;                end B;                end C;
```

**Q18. [Hard]** Tasks A–E must run according to the dependency graph **A → B → {C, D} → E**: A first, then B, then C and D **in parallel**, then E (a fork–join). Give the semaphore declarations and the body of each task so that this ordering is enforced.

**Q19. [Hard]** Prove that the **first** (reader-priority) Readers–Writers solution can **starve a writer**.

**Q20. [Hard]** In the monitor producer–consumer, `put` contains `if counter = n then wait(nonfull); end if;` and `get` contains `if counter = 0 then wait(nonempty); end if;`. Suppose `wait` on a condition variable did **not** release the monitor lock. Explain what goes wrong.

---

## Answer Key

**Q1. B.** Shared-memory = one global address space (read/write directly); distributed-memory = private memories joined by a network (message passing). A reverses the two. C is false — shared memory still needs synchronization (that's the whole critical-section problem). D reverses the failure-domain property (separate failure domains belong to distributed systems).

**Q2. B.** $s=(w,q)$: $w$ is a nonnegative integer (the counter/weight), $q$ is a set (FIFO queue) of blocked tasks. A, C, D are the common misreadings; in particular $w$ is an integer, never a real, and $q$ is never the counter.

**Q3. B.** A binary semaphore is a lock, so it starts **free** = $1$: `(1, ∅)`. $(0,\emptyset)$ would mean "already held" and would block the very first task; $(n,\emptyset)$ is not binary; $(-1,\emptyset)$ is not even a valid semaphore value.

**Q4. B.** Condition variables have **no stored value** — they are just queues. A `signal` with no waiter is **lost**. A describes a *semaphore* `signal` (which would increment the counter); C and D are nonsense for the event model.

**Q5. B.** Two disjoint fork pairs exist, so $\lfloor 5/2 \rfloor = 2$ philosophers can eat at once. **D is the trap**: `up_to_4` limits *seated* philosophers to 4, but only 2 can hold disjoint fork pairs simultaneously. A is too small; C is impossible with 5 forks.

**Q6. C.** The monitor's single-active-process rule automatically enforces mutual exclusion of its procedures. It does **not** by itself guarantee starvation freedom (A) or deadlock freedom (B) — those depend on the wait/signal protocol used — and it can still lose wake-ups if the code misuses condition variables (D).

**Q7. B.** Mutual exclusion (safety) holds, but liveness fails for writers: if readers keep arriving so `no_of_readers` never returns to 0, `signal(access)` never runs, `access` stays locked, and waiting writers never get in. A and C are false; D is the deadlock (safety) failure mode, which does not occur here.

**Q8. B.** Throughout the no-starvation solution the waker does **not** release `s`; instead it wakes the next process, which "inherits" the lock and later does `signal(s)`. This baton passing both preserves mutual exclusion on the counters and lets one wake-up **cascade** to unblock all parked readers. A and C are false; D is not the reason (the woken process does not re-block on `s.q`).

**Q9.** A **safety** property = "nothing bad happens"; a **liveness** property = "something good eventually happens". **Safety violation:** the naive Dining Philosophers solution **deadlocks** (all five hold their left fork and wait forever) — mutual progress is permanently blocked. **Liveness violation:** the reader-priority Readers–Writers solution **starves writers** — a writer may wait forever while readers flow through. (Either pairing is acceptable if the example genuinely matches the property class.)

**Q10.** If `wait(s)` is implemented as separate "read `s.w`" then "write `s.w−1`" steps, two tasks on a binary semaphore `s=(1,∅)` can both read `s.w = 1`, both decide "free", both decrement, and both enter their critical sections — mutual exclusion is violated. Atomicity makes the check-and-decrement one indivisible operation, so only one task can observe `s.w = 1` and win. (Symmetrically, non-atomic `signal` can corrupt the queue/counter.)

**Q11.** Invariant: `nonempty.w + nonfull.w = n` at all times (items in buffer + free slots = capacity). The **unbounded** buffer can never be full, so the producer never waits — only the consumer can block on an empty buffer, so `nonempty` alone suffices. With a finite buffer, the producer must also be blocked when all $n$ slots are occupied, so `nonfull` (init $n$) is added to count free slots and block the producer when full.

**Q12.** A cyclic index reuses freed slots: after writing slot $n-1$, `in` wraps to $0$. With linear indices, (i) `in`/`out` grow without bound and eventually index **outside the array** (buffer overflow), and (ii) once the producer reaches the end, slots freed by the consumer can never be reused — the buffer reports "full" forever even when empty. `mod n` recycles the slots so a fixed array behaves as a ring buffer.

**Q13.** The two statements describe the same guarantee at different levels. The direct test blocks a reader when a writer is *inside* (`nw > 0`). Writer-starvation prevention comes from the **hand-off**: when the last reader exits (`nr = 0`) and a writer is waiting (`nww > 0`), that reader passes the `s` lock to the writer *without releasing it*. A newly arriving reader then blocks on `wait(s)` before it can even evaluate `nw`, so a **pending writer is guaranteed to run next**. Hence "new readers wait behind a waiting writer" holds — via the lock hand-off rather than via a test on `nww` in the reader's own code.

**Q14.** Because a woken process can only become active *after* the signaller leaves the monitor, and in the meantime the signaller might change the state again. If `signal` is last, the condition the woken process relies on is guaranteed still true at hand-off; if the signaller keeps working after `signal`, the condition may no longer hold when the waiter finally runs.

**Q15.** Using `nonempty.w + nonfull.w = 3` throughout:

| Step | Operation | counter | nonempty.w | nonfull.w | in/out |
|---|---|---|---|---|---|
| 0 | init | 0 | 0 | 3 | in=0, out=0 |
| 1 | P puts `x` | 1 | 1 | 2 | in=1 |
| 2 | P puts `y` | 2 | 2 | 1 | in=2 |
| 3 | C gets (`x`) | 1 | 1 | 2 | out=1 |
| 4 | P puts `z` | 2 | 2 | 1 | in=(2+1)mod3=0 |
| 5 | C gets (`y`) | 1 | 1 | 2 | out=2 |
| 6 | C gets (`z`) | 0 | 0 | 3 | out=(2+1)mod3=0 |

End state: `counter=0`, `nonempty=0`, `nonfull=3`, `in=out=0` → buffer **empty**, and the invariant holds ($0+3=3$). Note step 4 wraps `in` to 0 (slot $n-1$ reused).

*4th put before any consumer:* the producer executes `wait(nonfull)`. With `nonfull.w = 0` (buffer full after 3 inserts), `wait` **blocks** the producer and enqueues it on `nonfull.q`; it stays blocked until a consumer's `signal(nonfull)` increments/awakens it. This is the whole point of the `nonfull` semaphore.

**Q16.** **Deadlock schedule:** all five philosophers execute `wait(fork(i))` (left) successfully before any executes its second `wait`. Each philosopher $i$ then holds fork $i$ and executes `wait(fork((i+1) mod 5))` (right), which blocks because that fork is held by neighbour $i+1$. **Wait-for cycle:** $P_0 \to P_1 \to P_2 \to P_3 \to P_4 \to P_0$. No fork is ever released (no `signal` reached), so all five block permanently. **Why `up_to_4` fixes it:** such a cycle requires **all five** philosophers to be seated mid-meal (the cycle must cover all five forks), so capping the table at 4 seated philosophers makes the cycle impossible — at least one seated philosopher can obtain both forks and eat.

**Q17.** **Possible outputs: `ACB` and `CBA`.** Reason: `B` can only proceed after `signal(s2)`, which only **C** performs, so **`B` always prints after `C`**. Initially `s1 = 1`, so `A` and `C` compete for `s1`:
- **A first:** A takes `s1` (1→0), prints `A`, `signal(s1)` (0→1). Then C takes `s1`, prints `C`, `signal(s2)` → wakes B; B prints `B`, `signal(s1)`. → **`ACB`**.
- **C first:** C takes `s1` (1→0), prints `C`, `signal(s2)` → wakes B. B prints `B`, `signal(s1)` (0→1) → wakes A; A prints `A`. → **`CBA`**.
`ABC`, `BAC`, `BCA`, `CAB` are all impossible because B must follow C.

**Q18.** The fork is at B; the join is at E.

```text
S_B    : semaphore := (0, ∅);   -- A done → start B
S_C    : semaphore := (0, ∅);   -- B done → start C
S_D    : semaphore := (0, ∅);   -- B done → start D
done_C : semaphore := (0, ∅);   -- C done → E
done_D : semaphore := (0, ∅);   -- D done → E

task body A is        task body B is          task body C is
  Operations A;         wait(S_B);              wait(S_C);
  signal(S_B);          Operations B;           Operations C;
end A;                  signal(S_C);            signal(done_C);
                        signal(S_D);          end C;
                      end B;

task body D is        task body E is
  wait(S_D);            wait(done_C);
  Operations D;         wait(done_D);
  signal(done_D);       Operations E;
end D;                end E;
```
A's `signal(S_B)` releases B; B's `signal(S_C)`/`signal(S_D)` release **both** C and D, which then run concurrently (fork); E blocks on **both** `done_C` and `done_D`, so it proceeds only after both finish (join/barrier). (D's order of the two `wait`s is free since it holds no other resource.)

**Q19.** Suppose readers continuously issue requests such that at least one reader is always inside the database. Then `no_of_readers` never returns to 0. `access` is released **only** by a reader executing `signal(access)` when `no_of_readers = 0`, and that condition never occurs. Therefore `access` stays held (value 0) indefinitely, so any writer's `wait(access)` never returns — the writer waits forever even though it could proceed the instant all readers paused. Because a reader never checks whether a writer is *waiting* (it only ever blocks on the first-reader path when `no_of_readers` transitions 0→1), arriving readers keep jumping ahead of waiting writers. Hence a writer can be **starved**. $\blacksquare$

**Q20.** The mutual-exclusion lock would be held by the blocked producer, so **no consumer can enter `get`** to remove an item and execute `signal(nonfull)`. The producer waits on `nonfull`, but the only process that could make the condition true (the consumer) is locked out of the monitor → **permanent deadlock**. `wait` on a condition variable must **release the monitor lock** precisely so that another process can enter and change the state that satisfies the condition.

---

## Scoring guide

- **A (18–20 correct):** solid command of semaphores, monitors, and the three classic problems.
- **B (15–17):** missing nuance on the no-starvation R–W solution or monitor lock semantics — re-read those sections.
- **C (11–14):** revisit invariants (bounded buffer, baton passing) and the safety/liveness distinction.
- **< 11:** re-read Lectures 01–03 notes, focusing on the "what goes wrong" boxes in each.
