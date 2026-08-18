# The Information Quality Hierarchy

*From noise to invariant knowledge*

> **Core idea:** Not all information has the same quality. Better information does more than describe what happened: it reveals structure, improves prediction, explains causes, and eventually identifies what remains true even when the context changes.

A useful rule: **moving upward means moving from surface description toward deeper constraint on reality.** The higher levels should survive more changes in context.

## The hierarchy

```text
NOISE
  ↓
OBSERVATION / DATA
  ↓
SIGNAL
  ↓
INFORMATION
  ↓
STRUCTURE
  ↓
PREDICTIVE KNOWLEDGE
  ↓
CAUSAL KNOWLEDGE
  ↓
INVARIANT KNOWLEDGE
```

| Level | Type | Meaning |
|---|---|---|
| 1 | **Noise** | Variation with little useful meaning |
| 2 | **Observation / Data** | A recorded fact or measurement |
| 3 | **Signal** | A meaningful difference from the background |
| 4 | **Information** | A signal that reduces uncertainty |
| 5 | **Structure** | A stable relationship across observations |
| 6 | **Predictive Knowledge** | A structure that constrains what is likely next |
| 7 | **Causal Knowledge** | An explanation of why the pattern occurs |
| 8 | **Invariant Knowledge** | What remains true across contexts and transformations |

---

## Why this matters to engineers

Software engineers are constantly flooded with logs, metrics, tickets, exceptions, user reports, test failures, dashboards and AI-generated explanations. The skill is not collecting more of them. It is identifying which observations reveal the deepest reusable structure.

- **Debugging:** move from “this request failed” to the mechanism that makes this class of requests fail.
- **System design:** identify relationships that remain true whether the system is a monolith, microservice, queue, database or distributed workflow.
- **Testing:** test invariants and contracts, not only individual examples.
- **AI-assisted work:** verify whether an answer is merely plausible, predictive in this case, causally correct, or robust across cases.
- **Learning:** facts are easier to retain when they are attached to a structure that explains many facts at once.

---

## One problem, climbed through the hierarchy

Imagine a Java service that occasionally gives two users the same available seat.

| Level | Example | What improved? |
|---|---|---|
| **Noise** | A large production log contains thousands of messages. | Most lines are irrelevant to the seat bug. |
| **Observation / Data** | Two successful booking responses were created for seat A14 within 40 ms. | A concrete event has been recorded. |
| **Signal** | The duplicate bookings occur only when requests overlap in time. | Concurrency is now distinguished from background activity. |
| **Information** | The bug becomes much more likely under concurrent load. | Uncertainty about the failure condition has been reduced. |
| **Structure** | Both requests read “available” before either write becomes visible to the other. | A repeatable read-check-write relationship is exposed. |
| **Predictive Knowledge** | Any sufficiently simultaneous requests can reproduce the bug. | We can predict future failures, including unseen cases. |
| **Causal Knowledge** | The system has a race condition because the state transition is not atomic. | We understand why the failure occurs. |
| **Invariant Knowledge** | When multiple actors can mutate shared state, consistency requires a coordination mechanism or an atomic invariant-preserving operation. | This survives Java threads, SQL transactions, distributed locks and other implementations. |

---

## The eight levels

### 1. Noise

Variation that does not reliably help you distinguish states of the system.

**Key question:** What can I safely ignore?

### 2. Observation / Data

A recorded event, fact or measurement. Data can be accurate and still be low-value.

**Key question:** What actually happened?

### 3. Signal

A pattern or deviation that appears meaningful relative to the surrounding noise.

**Key question:** What stands out?

### 4. Information

Signal that meaningfully reduces uncertainty about the system.

**Key question:** What do I now know that I did not know before?

### 5. Structure

A stable relationship connecting multiple observations. Structure compresses many facts into one model.

**Key question:** What relationship keeps repeating?

### 6. Predictive Knowledge

Structure strong enough to constrain what is likely to happen next.

**Key question:** What should I expect in a new case?

### 7. Causal Knowledge

A model of the mechanism that produces the observed pattern.

**Key question:** Why does this happen?

### 8. Invariant Knowledge

A relationship that remains true across meaningful transformations of implementation, scale or context.

**Key question:** What stays true even when the surface changes?

> **Important distinction:** Predictive knowledge is powerful, but it is not automatically the highest-quality knowledge. A correlation may predict for one regime and fail in another. Causal and invariant knowledge are stronger when they continue to explain the system after the environment changes.

---

## Low-quality vs high-quality engineering statements

| Lower-quality statement | Higher-quality statement | Upgrade |
|---|---|---|
| “The API is slow.” | “Latency rises when synchronous work grows on the request path.” | Descriptive → structural |
| “Adding threads made it faster.” | “Parallelism improves throughput only while work can execute independently and the bottleneck is parallelisable.” | Observation → conditional invariant |
| “The test passes with this input.” | “For every valid input, the output must preserve the domain invariant.” | Example → invariant |
| “Redis fixed the issue.” | “Moving repeated reads closer to the consumer reduces access latency when staleness is acceptable.” | Implementation fact → transferable mechanism |

---

## How to judge information quality

When you encounter a claim, metric, explanation or AI answer, test it against five questions:

1. **Uncertainty reduction:** Does this actually narrow the space of possible explanations or outcomes?
2. **Compression:** Does one idea explain many observations, or is it merely another isolated fact?
3. **Predictive power:** Does it help us anticipate an unseen case?
4. **Causal proximity:** Is it close to the mechanism, or only correlated with the result?
5. **Robustness under transformation:** Does it remain useful when the implementation, scale, environment or example changes?

---

## Exercises

### Exercise 1 — Classify the evidence

A service begins returning HTTP 500 errors after a deployment. Place each statement on the hierarchy, then explain why.

- “There were 213 errors between 14:00 and 15:00.”
- “Every failing request contains an empty `customerId`.”
- “The new mapper converts missing `customerId` values into `null`.”
- “Any path that dereferences an optional external value without validating it can fail when that value is absent.”
- “The logs contain 48,000 lines.”
- “Requests from the old client are three times more likely to fail.”

### Exercise 2 — Upgrade the statement

Take each low-level statement and push it upward by at least two levels.

- “The database is slow.”
- “Users abandon the checkout page.”
- “This class is difficult to test.”
- “Our merge conflicts keep happening.”

### Exercise 3 — Find an invariant

Choose one topic you already know — REST APIs, Git, databases, React state, Java memory, queues, authentication, testing, or another system. Write:

- three observations about it;
- one structure connecting those observations;
- one prediction that follows from the structure;
- one causal explanation;
- one candidate invariant that should remain true even if the technology changes.

---

## Worked mini-example: Git merge conflicts

**Data:** Two branches changed the same lines.

**Signal:** Conflicts cluster around files edited by several developers.

**Structure:** Independent histories are modifying overlapping representations of the same state.

**Prediction:** Long-lived branches with high overlap will produce more conflicts.

**Causality:** Git cannot automatically determine which competing change represents the intended final state.

**Invariant:** When independent actors modify the same state without coordination, reconciliation cost increases.

---

## What to carry forward

- Facts matter, but isolated facts are the beginning of understanding, not the end.
- Better information compresses more observations into fewer, stronger relationships.
- Prediction tests whether your structure generalises beyond what you already saw.
- Causality tells you why the structure exists.
- Invariant knowledge is the strongest target: the relationship that survives meaningful change.
- As an engineer, repeatedly ask: **“What is the deepest thing here that would still be true if the implementation changed?”**

> **Canonical maxim:** The highest-quality information is not merely true; it tells you what else must be true.

**One-line summary:** Move from seeing events to seeing the structure that makes the events inevitable.
