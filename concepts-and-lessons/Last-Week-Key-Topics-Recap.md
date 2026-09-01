# 📚 Last Week — Key Topics and the Connecting Story

## From Product Meaning to Invariants, Tests and Frontend Behaviour

---

# 🎯 1. The Main Idea of the Week

Last week was not really a collection of separate topics.

We covered:

```text
Documentation
TDD
DDD
React / Frontend Testing
Invariants
Invariant Traversal
First-Principles Thinking
```

But these ideas connect.

The deeper story was:

> **How do we take a human intention, turn it into software, allow that software to change, and still preserve what was supposed to remain true?**

```text
PRODUCT INTENT
      ↓
DOMAIN MEANING
      ↓
SOFTWARE MODEL
      ↓
ARCHITECTURE
      ↓
IMPLEMENTATION
      ↓
TESTS
      ↓
FRONTEND EXPERIENCE
      ↓
VERIFICATION
      ↓
SHARED UNDERSTANDING
```

The technologies change.

The underlying engineering problem remains.

---

# 📚 2. Documentation — Transferring Context

Documentation is not simply writing information down.

Its real job is:

> **Transfer enough context from one mind to another that the second person can understand or act correctly.**

```text
Context
↓
Structure
↓
Mechanism
↓
Action
↓
Verification
↓
Understanding
```

Good documentation answers:

```text
What is this?
Why does it exist?
Where does it fit?
How does it work?
What do I need to do?
How do I know it worked?
What happens if it fails?
```

### 🧠 Key invariant

```text
The reader should leave with
more usable capability,
not merely more words.
```

---

# 🔀 3. Pull Requests Are Documentation

A PR is not merely:

```text
please review
```

The code diff already tells us which lines changed.

The PR should explain:

```text
Why did they change?
What behaviour changed?
How does this fit the system?
How do I set it up?
How do I run it?
How do I test it?
What should I inspect carefully?
```

A strong PR therefore includes:

```text
Summary
↓
Why
↓
Important Changes
↓
Local Setup
↓
Environment Variables
↓
Run Instructions
↓
Testing
↓
Expected Result
↓
Out of Scope
↓
Review Focus
```

### 🧠 Core model

```text
Ticket
= promise

Implementation
= attempt

Tests
= evidence

PR
= explanation of the attempt and evidence
```

---

# 🧪 4. Testing — Code Makes Claims

Every piece of code makes a claim.

Example:

```text
This service calculates compatibility correctly.

This endpoint rejects invalid input.

This repository persists the recipe.

This React screen shows an error when the request fails.
```

Testing asks:

> **Can we prove the claim?**

```text
CODE
= proposed behaviour

TEST
= challenge

PASS
= evidence
```

> **Implementation is an attempt. Testing is evidence.**

---

# 🔴🟢🔵 5. Test-Driven Development

TDD changes the order of development.

```text
Desired Behaviour
↓
Failing Test
↓
Minimum Implementation
↓
Passing Test
↓
Refactor
```

Or:

```text
🔴 RED
↓
🟢 GREEN
↓
🔵 REFACTOR
```

The deeper reason for TDD is not simply:

```text
tests should come first
```

It is:

> **Define the behaviour before allowing the implementation to expand.**

---

# 🧱 6. Different Tests Answer Different Questions

```text
UNIT
↓
INTEGRATION
↓
API / COMPONENT
↓
END-TO-END
```

### ⚡ Unit

```text
Does this rule work?
```

### 🔗 Integration

```text
Do real components work together?
```

### 🌐 API

```text
Does the HTTP boundary behave correctly?
```

### 🌍 End-to-End

```text
Does the whole user journey work?
```

> **Choose the test boundary based on the question you need answered.**

---

# 🎭 7. Mocks and Boundaries

A mock is a controlled replacement for a dependency.

```text
RecipeService
      ↓
Mock Repository
```

This can prove:

```text
Does the service behave correctly
when the repository returns this result?
```

It cannot prove:

```text
Does the real PostgreSQL repository work?
```

That needs an integration test.

> **Mock boundaries, not reality.**

---

# 🧠 8. Tests Become System Memory

```text
Bug
↓
Failing Test
↓
Fix
↓
Passing Test
```

Months later, the test still remembers the failure.

```text
Human discovers rule
↓
Test stores rule
↓
Future changes are checked against it
```

> **The test suite becomes persistent memory of the promises the system has already made.**

---

# 🌍 9. DDD — Reality Is Too Large to Encode

Reality contains more information than software can represent.

So:

```text
REALITY
↓
PRODUCT INTENT
↓
SELECT WHAT MATTERS
↓
DOMAIN MODEL
```

A model is:

> **A useful compression of reality for a particular purpose.**

---

# 🗣️ 10. Language Creates the Model

We give important distinctions names:

```text
User
Fridge
Ingredient
Recipe
Dietary Preference
Compatibility
Shopping List
```

If we distinguish:

```text
DetectedIngredient
```

from:

```text
ConfirmedIngredient
```

we are saying those states have different meanings.

```text
Reality
↓
Distinctions
↓
Language
↓
Concepts
↓
Model
```

---

# 🪪 11. Entities, Values and Rules

### 🪪 Entity

Something whose identity persists through change.

```text
User
Recipe
Fridge
```

### 💎 Value Object

Something whose meaning is contained in its value.

```text
CompatibilityPercentage
EmailAddress
IngredientQuantity
Cuisine
```

Then come rules:

```text
Compatibility cannot exceed 100%.

A confirmed fridge belongs to a user.

Dietary exclusions override recipe ranking.
```

Some rules must always remain true.

Those are **invariants**.

---

# 🧬 12. Invariants Reveal Architecture

An invariant is closer to:

> **If this becomes false, the model no longer means what we said it means.**

Example:

```text
AI detection
≠
authoritative fridge state
```

So:

```text
Reality
↓
AI Observation
↓
Human Confirmation
↓
Authoritative State
```

That single invariant has consequences across the entire architecture.

---

# 🧭 13. One Invariant Descending Through the System

Start with:

```text
AI observation is not authoritative state.
```

Then descend:

```text
DOMAIN
Detected ingredients ≠ confirmed ingredients

↓
ARCHITECTURE
Detection and confirmation remain separate

↓
API
Detected and confirmed operations are distinct

↓
FRONTEND
Detected ingredients remain editable

↓
TEST
User can correct ingredients before confirmation
```

> **Each lower rule is a local projection of the higher invariant.**

---

# 🧠 14. DDD Connects the Backend Topics

```text
Domain Meaning
↓
Entities / Value Objects
↓
Rules / Invariants
↓
Application Services
↓
Repositories
↓
JPA
↓
PostgreSQL
↓
REST
↓
DTOs
```

### Controller

```text
HTTP boundary
```

### Application Service

```text
coordinates a use case
```

### Domain

```text
owns business meaning and rules
```

### Repository

```text
persistence boundary
```

### DTO

```text
shape of information crossing a boundary
```

These are not arbitrary framework conventions.

They emerge because different responsibilities require different homes.

---

# ⚛️ 15. React — State Becomes Human Experience

```text
SYSTEM STATE
↓
FRONTEND STATE
↓
REACT
↓
VISIBLE INTERFACE
↓
HUMAN
```

Then:

```text
HUMAN INTENT
↓
CLICK / TYPE / SELECT
↓
REACT EVENT
↓
STATE / REQUEST
↓
BACKEND
```

The frontend is the translation boundary between:

```text
system
↔
human
```

---

# 🔁 16. State Is Memory With Rules

A frontend remembers:

```text
selected image
selected cuisine
loading status
error state
current cooking step
API result
```

> **State management is memory with rules.**

```text
Event
↓
State Transition
↓
New Memory
↓
Render
```

Example:

```text
IDLE
↓ click
LOADING
↓ response
SUCCESS
```

or:

```text
LOADING
↓ failure
ERROR
```

---

# ⚛️🧪 17. Frontend Testing

Frontend testing asks:

> **Does system state become the correct human experience, and does human interaction cause the correct state transition?**

```text
GIVEN
visible state X

WHEN
the user performs action Y

THEN
the interface reaches observable state Z
```

So we test:

```text
loading
success
empty
error
disabled
selected
navigation
validation
```

—not merely whether a component exists.

---

# 👤 18. Test the User Boundary

Weak:

```text
Did setIsOpen(true) run?
```

Stronger:

```text
When the user clicks Edit,
does the editor appear?
```

Weak:

```text
Does RecipePage contain
three RecipeCard components?
```

Stronger:

```text
Given three recipes,
can the user see all three recipe names?
```

Good tests usually survive internal refactoring if behaviour stays the same.

---

# 🌐 19. Frontend and Backend Meet at the Contract

```text
BACKEND
Domain
↓
Application
↓
Controller
↓
JSON
════════ API CONTRACT ════════
JSON
↓
Frontend State
↓
React
↓
Human
FRONTEND
```

Backend tests prove:

```text
the contract is produced correctly
```

Frontend tests prove:

```text
the contract is consumed and projected correctly
```

E2E tests prove:

```text
the complete human ↔ system loop works
```

---

# 🧬 20. Shared Invariants Across the Topics

## 1️⃣ Meaning Must Survive Translation

```text
User Need
→ Requirement
→ Domain
→ Code
→ API
→ Frontend
→ Human Experience
```

Representations change.

Meaning should survive.

---

## 2️⃣ Complex Systems Need Boundaries

```text
HTML | CSS | JavaScript

Controller | Service | Repository

Entity | DTO

Port | Adapter

Domain | Infrastructure
```

Same deeper rule:

> **Give each responsibility a coherent home and control how responsibilities interact.**

---

## 3️⃣ State Changes, Some Truths Must Not

```text
STATE
changes

IMPLEMENTATION
changes

TECHNOLOGY
changes

INVARIANTS
constrain the change
```

---

## 4️⃣ Authority Must Be Explicit

```text
AI observation
≠
confirmed truth

Frontend representation
≠
database authority

DTO
≠
domain model
```

Do not confuse a representation of truth with the authoritative source.

---

## 5️⃣ Behaviour Requires Evidence

```text
Requirement
↓
Implementation
↓
Test
```

Important behaviour should be provable.

---

## 6️⃣ Change Must Be Contained

```text
one change
↓
how much unrelated behaviour breaks?
```

SOLID, components, modules, APIs, interfaces, tests and bounded contexts all help reduce uncontrolled change propagation.

---

## 7️⃣ Contracts Enable Cooperation

```text
Function Signature
Interface
API
DTO
Repository Contract
Component Props
Test Expectation
```

> **If the contract is preserved, each side does not need to know all of the other's internal machinery.**

---

# 🧭 21. Invariant Traversal

When implementation becomes confusing:

```text
classes
methods
components
DTOs
state
queries
tests
```

move upward:

```text
Implementation Detail
↑
Component Rule
↑
Architectural Invariant
↑
Domain Invariant
↑
Product Invariant
```

Find the most stable truth that governs the problem.

Then descend:

```text
Product Invariant
↓
Domain Constraint
↓
Architecture
↓
Boundary
↓
Implementation
↓
Test
```

This is **invariant traversal**.

---

# 🔄 22. Traversal Is Bidirectional

Design downward:

```text
What must remain true?
↓
What does that imply?
↓
What architecture preserves it?
↓
What code implements it?
↓
What test proves it?
```

Debug upward:

```text
Something feels wrong
↑
Which local rule is failing?
↑
Which boundary is confused?
↑
Which higher invariant was violated?
```

Then descend again.

```text
BUG
↑
VIOLATED INVARIANT
↓
BETTER DESIGN
```

---

# 🧠 23. First-Principles Thinking

First-principles thinking:

```text
Remove convention
↓
find what is actually necessary
↓
reconstruct the answer
```

Instead of:

```text
Business logic should not go
in controllers because Spring says so.
```

derive:

```text
HTTP is transport.

Dietary filtering is domain behaviour.

Domain truth should not depend
on the transport used to request it.

Therefore:
the controller should not own
the dietary rule.
```

That is first-principles reasoning.

---

# 🧬 24. First Principles vs Invariant Traversal

### First-Principles Thinking

```text
What truths can I derive this answer from?
```

### Invariant Traversal

```text
At which level of the system
is the stable truth I need,
and how does it constrain the levels below?
```

> **First-principles thinking finds the truths.  
> Invariant traversal navigates those truths across levels of a complex system.**

---

# 🤖 25. Why This Matters With AI

AI can increasingly generate:

```text
components
controllers
tests
CRUD
queries
DTOs
configuration
boilerplate
```

The engineering bottleneck moves upward toward:

```text
What should be built?

What must remain true?

Where should responsibility live?

Which output is authoritative?

Which boundary is being violated?

What evidence would prove this solution?

Does locally plausible code
preserve the global system?
```

A useful workflow:

```text
Human defines invariant
↓
Human constrains problem
↓
AI proposes implementation
↓
Human checks against invariants
↓
Tests encode required behaviour
↓
Accept / Reject
```

---

# 🌳 26. The Whole Week in One Model

```text
REALITY
   ↓
PRODUCT INTENT
   ↓
DDD
   ↓
DOMAIN MODEL
   ↓
INVARIANTS
   ↓
ARCHITECTURE
   ↓
IMPLEMENTATION
   ↓
TDD / TESTING
   ↓
REST / DTO CONTRACT
   ↓
REACT STATE
   ↓
USER INTERFACE
   ↓
FRONTEND TESTING
   ↓
E2E
   ↓
CI
   ↓
DOCUMENTATION
   ↓
SHARED SYSTEM UNDERSTANDING
```

These are different parts of one problem:

> **How do we preserve coherent meaning while a complex software system is represented, changed, executed and understood by different actors?**

---

# ⚡ 27. Five Questions to Keep

When facing unfamiliar code, architecture or AI-generated output:

```text
1. What is actually supposed to happen?

2. What must remain true?

3. Where does that truth belong?

4. What implementation preserves it?

5. What evidence would prove it?
```

Those five questions connect almost everything from the week.

---

# 🧬 Final Compression

```text
DDD
→ discover and represent meaning.

SOLID / Architecture
→ give meaning stable boundaries.

TDD
→ turn expected behaviour into evidence.

REST / DTOs
→ move meaning across a system boundary.

React
→ project system state into human experience.

Frontend Testing
→ prove the projection and interaction.

Documentation
→ transfer the model between humans.

Invariant Traversal
→ navigate complexity by moving between stable truths and local implementation.

First-Principles Thinking
→ derive solutions from necessary truths instead of memorised conventions.
```

And underneath all of it:

> **Preserve what must remain true while allowing everything else to change.**
