---
course: COMP 403 — Parallel and Distributed Processing
unit: lecture-01
source: lecture-01.pdf
generated: 2026-10-17
---

# Lecture 01 — Summary (Introduction & Basic Concepts)

> Scope: foundation lecture — vocabulary. Establishes the **shared-memory vs. distributed-memory** axis the whole course runs on.

## Programs & Processes
- **Sequential program** $S$: one instruction stream, one processor; correctness = Hoare triple $\{p\}\;S\;\{q\}$.
- **Partial correctness**: *if* $S$ terminates on valid input, output satisfies $q$.
- **Total correctness**: partial correctness **+** termination on all valid inputs.
- **Sequential process**: dynamic execution — ordered ops $o_1,o_2,\dots$; each op completes before the next (no overlap *within* a process).
- **Parallel program**: decomposition into tasks (each internally sequential) run simultaneously.

## Why Parallel?
1. **Speedup** — solve faster by splitting work.
2. **Scale** — solve problems too big for one machine.

## Synchronous vs. Asynchronous
- **Synchronous**: caller waits for each op → forces sequential order.
- **Asynchronous**: ops overlap/interleave; no waiting between processes.
- **Concurrent processes** = sequential processes performed simultaneously and asynchronously → source of race conditions.

## Communication — the course's central axis
| | **Shared memory** | **Distributed memory** |
|---|---|---|
| Address space | one global | private per processor |
| Communication | read/write shared RAM | message passing over network |
| Sync burden | programmer's (critical sections) | explicit messages |
| Failure | single machine | separate failure domains (fault tolerance) |
- Standard failure assumption: **fail-stop** (Byzantine is a stronger model, later).
- Maps to: pthreads/OpenMP (shared) vs. MPI (distributed).

## Correctness of Concurrent Programs
Every task correct **plus** global properties:
- **Safety** ("nothing bad"): **mutual exclusion** (≤1 process in its critical section) + **no deadlock** (no permanent mutual blocking).
- **Liveness** ("something good"): every requesting process eventually gets the resource — **no starvation**.
- **Deadlock:** two+ processes each holding what the other needs → permanent block.

## Critical Section Problem
- Group of processes compete for a resource usable by **one at a time** (shared vars, DB records, files, printers).
- **Critical section** = the fragment using the resource. Requirement: **≤1 task in its CS at a time**.
- Protocol: **acquire → use → release**.

## Semaphores
- **Definition:** $s = (w, q)$ — $w \in \mathbb{N}_0$ (counter), $q$ = set/FIFO queue of blocked tasks.
- **General** ($w$ any $\mathbb{N}_0$) vs. **binary** ($w\in\{0,1\}$; 1 free, 0 used).
- `wait(s)`: if $w>0$ then $w{:=}w-1$; else block task, add to $q$.
- `signal(s)`: if $q\neq\emptyset$ then unblock one task; else $w{:=}w+1$.
- **Both must be ATOMIC** — else two tasks read $w>0$ and both enter the CS.
- **Without it:** race condition on the shared check → mutual exclusion violated.

## Critical Section (2 tasks / $n$ tasks)
```
wait(s);  -- s := (1,∅)
  critical section
signal(s);
```
- **Mutual exclusion proof:** atomicity ⇒ only one task sees $w=1$ and decrements.
- **Deadlock-freedom proof (contradiction):** two tasks both blocked ⇒ $w=0$ and both in $q$, but $w=0$ means one already decremented from 1 ⇒ at most one inside — contradiction.
- Generalises unchanged to $n$ tasks.

## Producer–Consumer (unbounded buffer)
- Data: `buffer(0..∞)`, `in,out:=0`, `nonempty := (0,∅)` (**general** — counts items).
- **Producer:** `Produce; buffer(in):=data; in++; signal(nonempty)`.
- **Consumer:** `wait(nonempty); data:=buffer(out); out++; Consume`.
- **Condition synchronisation**: consumer with $w=0$ blocks until producer signals.
- **Two semaphore roles:** mutual exclusion (binary, guards a CS) vs. condition sync (general, waits for an event).
- **Problem:** unbounded buffer ⇒ **memory exhaustion** if producer faster → use bounded buffer (Lecture 02).

## Decision Rules
- One-at-a-time access to a resource? → **binary semaphore** around the CS.
- Wait for an event/state (data available)? → **general semaphore** (condition sync).
- Two tasks with the same CS? → `wait(s); CS; signal(s)` with `s:=(1,∅)`.
- Sequential-wait vs. overlap? → **synchronous** vs. **asynchronous** communication.

## Key Numbers/Formulas
| Item | Value |
|---|---|
| Hoare triple | $\{p\}\;S\;\{q\}$ |
| Semaphore | $s=(w,q)$, $w\in\mathbb{N}_0$, $q$ FIFO |
| CS semaphore init | `(1, ∅)` (free) |
| Producer–consumer `nonempty` init | `(0, ∅)` (empty buffer) |
