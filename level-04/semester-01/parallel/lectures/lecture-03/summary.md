---
course: COMP 403 — Parallel and Distributed Processing
unit: lecture-03
source: lecture-03.pdf
generated: 2026-10-17
---

# Lecture 03 — Summary (Readers–Writers · Monitors)

> Scope: **shared-memory parallelism** (one machine, shared address space). No message complexity — concern is starvation, blocking, fairness.

## Readers–Writers Problem
- **Readers** read; many may read at once. **Writers** update; need **exclusive** access.
- **Access rules:** reader ⇔ no writer *inside*; writer ⇔ no reader **and** no writer *inside*.
- Reduces to synchronizing **access conditions**.

### First solution (reader-priority)
- Sems: `s := (1,∅)` (protects `no_of_readers`), `access := (1,∅)` (DB lock); `no_of_readers := 0`.
- **Reader:** `wait(s); nr++; if nr=1 wait(access); signal(s); READ; wait(s); nr--; if nr=0 signal(access); signal(s);`
- **Writer:** `wait(access); WRITE; signal(access);`
- **Advantage:** simple (one semaphore controls writer exclusion).
- **Limitation:** **writer starvation** — readers dominate; `nr` never → 0, `access` never released.
- Liveness failure (starvation), **not** deadlock.

### Correct solution (no starvation)
- Sems: `s:=(1,∅)`, `reading_allowed:=(0,∅)`, `writing_allowed:=(0,∅)`.
- Counters: `nr,nw` (**using**), `nwr,nww` (**waiting**), all 0.
- **Reader:** acquire `s`; if `nw>0` → `nwr++`, release `s`, `wait(reading_allowed)`; then `nr++`; if `nwr>0` wake next reader (cascade); after read: `wait(s); nr--; if nr=0 and nww>0 → signal(writing_allowed)`.
- **Writer:** acquire `s`; if `nr>0 or nw>0` → `nww++`, release `s`, `wait(writing_allowed)`; then `nw++`; after write: wake waiting **reader** first, else waiting writer.
- **Baton passing:** the waker does **not** `signal(s)`; the woken process releases `s`. Wake-one cascades to wake all parked.
- **Correctness rules:** (1) new reader blocked if a writer waits; (2) last reader wakes a writer; (3) writer finishing wakes readers first, else a writer.

## Monitors
- **Definition:** encapsulates **private shared data** + **procedures** + **condition variables**.
- **Vital feature:** only **one process active** in the monitor at a time ⇒ **automatic mutual exclusion** (no explicit semaphore on the data).
- **Purpose:** hides the semaphore bookkeeping.

### Condition variables
- `wait(a)`: block caller, add to FIFO queue of `a`, **release monitor lock**.
- `signal(a)`: unblock one waiter on `a`; `signal` with **no waiter is LOST**.
- Only **one process active** at a time ⇒ a woken process runs only after the signaller **leaves** the monitor.
- **Recommendation:** make `signal` the **last** operation in the procedure (condition must still hold at hand-off).
- **No stored value** — condition variables are *just queues*.

### Condition variables vs. semaphores
| Aspect | Condition Variables | Semaphores |
|---|---|---|
| Value | none | integer component |
| Counting | cannot count | counts wait/signal |
| Nature | no stored state | counter + queue |
| wait | blocks, inserts in FIFO | decrements; blocks if <0 |
| signal | unblocks one waiter, **else lost** | increments; unblocks if waiting |

## Monitor Producer–Consumer
```
monitor Producer_Consumer:
  buffer[0..n-1]; in,out:=0; counter:=0; nonempty,nonfull: condition
  put(d):  if counter=n then wait(nonfull) end if;
           buffer(in):=d; in:=(in+1) mod n; counter++; signal(nonempty)
  get(d):  if counter=0 then wait(nonempty) end if;
           d:=buffer(out); out:=(out+1) mod n; counter--; signal(nonfull)
```
- Producer: `loop Produce data; Producer_Consumer.put(data) end loop`
- Consumer: `loop Producer_Consumer.get(data); Consume data end loop`
- Mutual exclusion of `put`/`get` **guaranteed by the monitor**; callers need no full/empty logic.
- **`counter`** is the state that `nonempty`/`nonfull` (which have no value) cannot represent.
- POSIX mapping ≈ `pthread_mutex_t` + `pthread_cond_t`; `pthread_cond_wait` **releases the mutex** while blocking.

## Decision Rules
- Data mostly read, rarely written? → readers–writers (shared/exclusive) lock; use reader-priority only if writers can tolerate starvation.
- Need writers to never starve? → block new readers when a writer waits (writer-priority branch) + baton passing.
- Need both exclusion **and** condition-waiting? → monitor with condition variables.
- Condition variable forgot an event? → can't; store the state in a monitor variable and test it.
