# 🧪 The Story of Test-Driven Development

## Why We Test, What TDD Changes, and How Tests Become Part of the Design of Software

---

# 🎯 1. Why Testing Exists

Software is unusual because it can look correct long before it is correct.

A method can compile.

An endpoint can return `200`.

A screen can render.

A database row can appear.

And yet the system can still be wrong.

The core problem is simple:

> **Implementation is an attempt. Testing is evidence.**

```text
Requirement
   ↓
Implementation
   ↓
Claim
   ↓
Test
   ↓
Evidence
```

Without tests, we are often saying:

```text
"I think this works."
```

With tests, we can say:

```text
"I have evidence that this behaviour works under these conditions."
```

That difference is the entire reason testing exists.

---

# 🧠 2. Software Creates an Evidence Problem

When we write code, we are making claims.

For example:

```text
This service rejects invalid users.
This repository stores recipes correctly.
This endpoint returns 404 when the record does not exist.
This matcher calculates compatibility correctly.
```

Each statement is a claim about behaviour.

A test asks:

> **Can we prove it?**

```text
CODE
= proposed behaviour

TEST
= challenge to the proposal

PASS
= evidence the proposal survived that challenge
```

A test does not prove the system is universally correct.

It proves something narrower:

> **Under the conditions we specified, the observed behaviour matched the expected behaviour.**

---

# 🎫 3. Ticket → Implementation → Test

A useful engineering model is:

```text
Ticket
= promise

Implementation
= attempt

Test
= proof
```

If the ticket says:

> A user should not be able to save a recipe that does not exist.

Then the implementation is only one possible attempt to satisfy that promise.

The test turns the promise into something executable.

```text
Given
A recipe does not exist

When
The user tries to save it

Then
The system rejects the operation
```

The test is the bridge between human intention and machine-observable behaviour.

---

# 🧬 4. The Testing Invariant

Different systems use different frameworks. Different teams use different naming conventions. Different applications need different test strategies.

But one invariant remains:

> **A useful test creates a controlled condition, performs an action, and checks an observable result.**

This usually becomes:

```text
ARRANGE
   ↓
ACT
   ↓
ASSERT
```

---

# 🧱 5. Arrange → Act → Assert

## 🧩 Arrange

Create the conditions required for the test.

```java
Recipe recipe = new Recipe("Butter Chicken");
```

## ⚙️ Act

Perform the behaviour being tested.

```java
recipe.addIngredient("Chicken");
```

## ✅ Assert

Check the observable result.

```java
assertTrue(recipe.getIngredients().contains("Chicken"));
```

The structure is:

```text
STATE
 ↓
ACTION
 ↓
OBSERVATION
```

A clear test should make those three stages obvious.

---

# 🔴🟢🔵 6. What Test-Driven Development Changes

Traditional development often looks like this:

```text
Idea
↓
Write code
↓
Finish feature
↓
Maybe write tests
```

TDD changes the order.

```text
Desired behaviour
↓
Write failing test
↓
Write minimum code
↓
Pass test
↓
Improve design
```

This is the famous:

> **Red → Green → Refactor**

The deeper idea is not merely “write tests first.”

> **Define observable behaviour before allowing implementation freedom to expand.**

---

# 🔴 7. Red — Make the Missing Behaviour Visible

The first step is to write a test for behaviour that does not yet exist.

```java
@Test
void calculatesMissingIngredients() {
    // desired behaviour
}
```

The test should fail.

Why?

Because if the test passes before the behaviour exists, the test may not actually be testing the thing you think it is.

Red establishes:

```text
This behaviour is currently absent.
```

The failing test creates a boundary around the next piece of work.

---

# 🟢 8. Green — Write the Minimum Code That Works

Now implement only enough behaviour to make the test pass.

The goal is not elegance yet.

The goal is:

```text
make the required behaviour true
```

This limits unnecessary invention.

Without this constraint, developers often start building:

```text
future abstractions
extra configuration
unused extension points
generalised frameworks
premature architecture
```

TDD pushes back against that.

> **What is the smallest implementation that satisfies the behaviour we actually need?**

---

# 🔵 9. Refactor — Improve the Structure Without Changing Behaviour

Once the test is green, improve the code.

Examples:

- rename unclear variables;
- extract methods;
- remove duplication;
- simplify conditionals;
- introduce a useful abstraction;
- improve object boundaries.

The crucial part is:

> **The tests stay green.**

So refactoring becomes:

```text
change structure
without changing behaviour
```

This is one of the greatest powers of a good test suite.

---

# 🔁 10. The TDD Loop

```text
        ┌──────────────┐
        │ Write a test │
        └──────┬───────┘
               ↓
             🔴 RED
               ↓
     ┌────────────────────┐
     │ Write minimum code │
     └─────────┬──────────┘
               ↓
            🟢 GREEN
               ↓
       ┌──────────────┐
       │   Refactor   │
       └──────┬───────┘
               ↓
            🔵 CLEAN
               ↓
             repeat
```

Each loop should be small.

TDD works best when the next behaviour is narrow and clear.

---

# 🧠 11. TDD Is Really About Controlling Design

If you write a test before the implementation, you are forced to ask:

```text
What should this object do?
What input should it need?
What should it return?
Which behaviour is externally visible?
Which dependency should be replaceable?
```

These are design questions.

So TDD is not just a testing technique.

It is also a design pressure.

Good tests tend to encourage:

- smaller units;
- explicit dependencies;
- clearer interfaces;
- lower coupling;
- stronger cohesion;
- observable behaviour.

---

# 🧲 12. Testability Is a Design Signal

Imagine a service that:

- creates its own database connection;
- reads environment variables directly;
- calls an external API directly;
- generates the current time internally;
- writes files directly;
- contains all business logic in one method.

That code may work.

But it is difficult to test because its dependencies are hidden inside it.

```text
Service
 ├─ creates DB
 ├─ calls AI
 ├─ reads clock
 ├─ writes file
 └─ decides business logic
```

A more testable design might be:

```text
Service
 ├─ RecipeRepository
 ├─ ImageAnalysisPort
 ├─ Clock
 └─ business logic
```

Now the dependencies are explicit.

> **If a unit is extremely difficult to test, ask whether the boundary itself is wrong.**

---

# 🏗️ 13. Dependency Injection and Testing

Dependency injection separates:

```text
what the service does
```

from:

```text
which concrete dependency performs the work
```

```java
public class RecipeService {

    private final RecipeRepository repository;

    public RecipeService(RecipeRepository repository) {
        this.repository = repository;
    }
}
```

Production can provide a PostgreSQL-backed repository.

A unit test can provide a mock repository.

The service does not need to know the difference.

That is why dependency injection, interfaces and testing often reinforce each other.

---

# 🧪 14. Not All Tests Test the Same Thing

Testing exists at different boundaries.

```text
Unit
↓
Integration
↓
API / Component
↓
End-to-End
```

Each level answers a different question.

---

# ⚡ 15. Unit Tests

A unit test asks:

> **Does this small piece of behaviour work in isolation?**

Examples:

- compatibility percentage;
- shopping-list calculation;
- dietary filtering;
- validation logic;
- service decision logic.

A unit test should usually be:

```text
fast
focused
deterministic
isolated
```

```java
@Test
void calculatesCompatibilityPercentage() {
    int result = matcher.calculate(3, 5);

    assertEquals(60, result);
}
```

Unit tests are excellent for business rules.

---

# 🔗 16. Integration Tests

An integration test asks:

> **Do these real components work together correctly?**

Examples:

```text
Repository ↔ PostgreSQL
Spring Boot ↔ Flyway
Service ↔ Repository
JSON ↔ DTO mapping
```

A repository unit test using mocks cannot prove SQL actually works.

An integration test can.

```text
Unit test:
Does my repository-using logic behave?

Integration test:
Does the real repository actually talk to the real database correctly?
```

---

# 🌐 17. API / Controller Tests

An API test checks the HTTP boundary.

It asks questions such as:

```text
Does POST /recipes accept valid JSON?
Does invalid input return 400?
Does missing data return 404?
Is the response shape correct?
Are validation errors structured correctly?
```

In Spring Boot, this may involve:

```text
MockMvc
WebTestClient
@SpringBootTest
```

The API test proves the contract between client and backend.

---

# 🌍 18. End-to-End Tests

An end-to-end test checks the system through a user-visible flow.

Example:

```text
User selects fridge image
↓
Mobile client uploads
↓
Backend receives image
↓
Ingredient list returned
↓
User confirms
↓
Recipe suggestions appear
```

E2E tests give powerful confidence.

But they are usually:

- slower;
- more fragile;
- harder to debug;
- more expensive to maintain.

So they should be used deliberately.

---

# 🔺 19. The Test Pyramid

```text
             /\
            /  \
           / E2E\
          /------\
         / API /  \
        /Component \
       /------------\
      / Integration  \
     /----------------\
    /    Unit Tests    \
   /____________________\
```

The idea is:

```text
many fast tests
+
fewer expensive tests
```

Not because unit tests are inherently better.

Because different test types have different costs.

---

# ⚖️ 20. Confidence vs Cost

Every test provides some amount of confidence at some cost.

A unit test is:

```text
cheap
fast
precise
```

but has limited system coverage.

An E2E test is:

```text
broad
realistic
high-confidence
```

but more expensive.

So test strategy is an optimisation problem:

```text
maximum useful confidence
÷
minimum unnecessary cost
```

---

# 🎭 21. What a Mock Is

A mock is a controlled replacement for a dependency.

Suppose:

```text
RecipeService
      ↓
RecipeRepository
      ↓
PostgreSQL
```

A unit test may not want PostgreSQL involved.

So the test replaces the repository:

```text
RecipeService
      ↓
Mock RecipeRepository
```

Example with Mockito:

```java
when(repository.findById(1L))
    .thenReturn(Optional.of(recipe));
```

This allows the service behaviour to be tested independently.

---

# ⚠️ 22. What Mocks Are Not

Mocks are not proof that the real dependency works.

If this passes:

```java
when(repository.findById(1L))
    .thenReturn(Optional.of(recipe));
```

we have proven:

```text
the service reacts correctly
when the repository returns a recipe
```

We have not proven:

```text
the real repository can retrieve that recipe from PostgreSQL
```

That requires an integration test.

---

# 🪞 23. Over-Mocking

A dangerous test can become:

```text
mock A
mock B
mock C
mock D

when A does X, return Y
when B receives Y, return Z
verify C called D
```

At some point, the test may simply reproduce the implementation.

Then it is proving the mocks behaved exactly as configured rather than the system behaviour being correct.

> **Mock boundaries, not reality.**

Good candidates for mocking include:

- external APIs;
- repositories in service unit tests;
- clocks;
- message queues;
- file systems;
- expensive external dependencies.

---

# 🧠 24. State-Based vs Interaction-Based Tests

There are two broad things tests can observe.

## ✅ State

```text
After this action, what result exists?
```

```java
assertEquals(60, result.getCompatibilityPercentage());
```

## 🔄 Interaction

```text
Did this dependency receive the expected call?
```

```java
verify(repository).save(recipe);
```

Prefer observable outcomes when possible.

Interaction verification is useful when the interaction itself is meaningful.

---

# 🧹 25. A Good Test Should Read Like Behaviour

Compare:

```java
@Test
void testMethod1() {
}
```

with:

```java
@Test
void returnsMissingIngredientsWhenRecipeRequiresItemsNotInFridge() {
}
```

The second name explains:

```text
condition
+
behaviour
+
expected outcome
```

A test suite should act as executable documentation.

---

# 📝 26. Given → When → Then

Another useful testing structure is:

```text
GIVEN
a starting condition

WHEN
an action occurs

THEN
an observable outcome follows
```

Example:

```text
GIVEN
the fridge contains chicken and garlic

WHEN
the user requests compatibility for a recipe requiring
chicken, garlic and yoghurt

THEN
the system reports yoghurt as missing
```

This structure is closely related to BDD.

---

# 🥒 27. Behaviour-Driven Development

BDD extends the TDD idea toward business-readable behaviour.

Instead of beginning from a method, it begins from behaviour.

Typical structure:

```text
Given
When
Then
```

BDD is useful when:

- business rules matter;
- behaviour needs to be discussed with non-developers;
- acceptance criteria need to become executable;
- system behaviour is more important than implementation detail.

TDD and BDD are not enemies.

> **BDD can be thought of as TDD viewed through the language of behaviour and outcomes.**

---

# 📜 28. Acceptance Tests

Acceptance tests ask:

> **Has the promised feature actually been delivered?**

If the ticket says:

```text
User can remove a detected ingredient before confirming the fridge.
```

An acceptance test should prove that behaviour.

Acceptance tests sit close to requirements rather than implementation details.

---

# 🤝 29. Contract Tests

In distributed systems, two services may agree on a contract.

Example:

```text
Frontend expects:

POST /images

Response:
{
  "imageId": 42,
  "status": "PROCESSING"
}
```

A contract test checks that the producer and consumer remain compatible.

```text
Consumer expectation
↔
Provider behaviour
```

Contract tests become especially valuable as systems split into services.

---

# 🧯 30. Regression Testing

A regression is:

> **Something that used to work stops working after a change.**

```text
Bug:
dietary filter incorrectly allowed peanuts

↓ fix bug

↓ add test

Future change:
bug cannot silently return without failing the test
```

This turns historical failures into permanent knowledge.

> **Every important bug is a candidate for a new test.**

---

# 🐛 31. The Best Bug Fix Often Begins With a Failing Test

When a bug is found:

```text
1. Reproduce the bug in a test.
2. Watch the test fail.
3. Fix the bug.
4. Watch the test pass.
```

Now you know the test represents the bug and the fix actually changes that behaviour.

This is TDD applied to debugging.

---

# 🔒 32. Tests Create a Behavioural Safety Net

Imagine refactoring a service with no tests.

Every change creates uncertainty:

```text
Did I break validation?
Did I change the response?
Did I break filtering?
Did I break saving?
```

With useful tests:

```text
Refactor
↓
Run suite
↓
Known behaviours remain green
```

This does not eliminate risk.

It reduces the space in which unknown breakage can hide.

---

# 🛠️ 33. Tests Improve Refactoring

Refactoring means:

> **Change the internal structure without changing externally observable behaviour.**

Tests are what make that claim checkable.

Without tests:

```text
"Should still work."
```

With tests:

```text
"Known behaviours still pass."
```

So:

```text
testing
→ confidence
→ easier refactoring
→ better design
```

---

# 🧬 34. Tests and SOLID

Testing often reveals SOLID principles naturally.

## S — Single Responsibility

A class doing too many things becomes hard to test.

## O — Open/Closed

Well-defined abstractions allow behaviour to change behind stable boundaries.

## L — Liskov Substitution

Test doubles depend on interchangeable implementations respecting the same contract.

## I — Interface Segregation

Small interfaces are easier to replace and test.

## D — Dependency Inversion

High-level behaviour can depend on abstractions instead of hard-coded infrastructure.

This is why testing and architecture often improve together.

---

# 🚧 35. What Makes Code Hard to Test?

Common causes:

```text
hidden dependencies
global state
static mutable state
large methods
too many responsibilities
direct external calls
time-dependent logic
randomness
hard-coded configuration
tight coupling
```

Testing difficulty is often telling you something.

Do not always ask:

> How can I make the test more complicated?

Sometimes ask:

> Why is the production design forcing the test to be complicated?

---

# ⏰ 36. Time and Randomness

Code using:

```java
LocalDateTime.now()
```

directly may be difficult to test because the result changes.

A better design may inject a clock.

Likewise, random behaviour may require a controlled random source.

> **Nondeterministic dependencies should become controllable at the test boundary.**

A good test should normally produce the same result every time.

---

# 🧊 37. Deterministic Tests

A deterministic test means:

```text
same inputs
+
same controlled environment
=
same result
```

Flaky tests violate this.

A test that sometimes passes and sometimes fails creates mistrust.

Once developers stop trusting the test suite, they start ignoring it.

A flaky suite can be worse than a smaller reliable suite.

---

# 🚫 38. Tests Should Not Depend on Execution Order

Bad:

```text
test 1 creates data
test 2 assumes test 1 already ran
test 3 deletes it
```

Good:

```text
each test establishes its own required state
```

Tests should ideally be isolated.

This makes them repeatable, parallelisable and easier to debug.

---

# 🧼 39. Test Data Should Be Intentional

Avoid massive test fixtures when the test only needs three values.

If the behaviour is compatibility percentage, this may be enough:

```text
fridge = [chicken, garlic]
recipe = [chicken, garlic, yoghurt]
```

The simpler the test data, the easier it is to understand why the test failed.

---

# 🎯 40. Test Behaviour, Not Private Implementation

Suppose a service contains:

```java
private int calculateScore(...)
```

A common temptation is to test the private method directly.

Usually the better question is:

> **Which public behaviour depends on this calculation?**

Test that.

Private structure should remain free to change during refactoring.

Tests that know too much about internals become brittle.

---

# 🧲 41. Brittle Tests

A brittle test fails when implementation changes even though behaviour remains correct.

Example:

```text
test verifies exactly five internal method calls
```

Then you refactor from five calls to three while preserving the result.

The test fails even though the user-visible behaviour remains correct.

That means the test is coupled to implementation rather than behaviour.

---

# 📏 42. Code Coverage

Coverage answers questions such as:

```text
Which lines executed during tests?
Which branches executed?
Which methods executed?
```

Example:

```text
82% line coverage
```

Coverage is useful.

But coverage is not correctness.

A test can execute a line without meaningfully checking its behaviour.

---

# ⚠️ 43. 100% Coverage Can Still Be Bad Testing

Imagine:

```java
calculator.divide(10, 2);
```

The line executed.

Coverage increases.

But there is no assertion.

We did not prove the result was `5`.

So:

```text
coverage
≠
quality
```

Coverage tells us where tests travelled, not whether they asked good questions.

---

# 🧠 44. Mutation Testing

Mutation testing pushes this idea further.

It deliberately changes production code.

Example:

```java
return a + b;
```

becomes:

```java
return a - b;
```

Then the tests run.

If the tests still pass, they were not strong enough to detect the behavioural change.

Mutation testing asks:

> **Would the test suite notice if the code were wrong?**

That is often a deeper quality signal than raw coverage.

---

# 🏭 45. Tests in CI

Tests become far more valuable when they run automatically.

A CI pipeline may look like:

```text
Checkout
↓
Build
↓
Unit Tests
↓
Integration Tests
↓
Package
↓
Deploy
```

The key rule:

> **A failed test should stop the pipeline before broken behaviour is promoted.**

Now tests are no longer just a developer tool.

They become an automated delivery gate.

---

# 🚦 46. Tests as a Quality Gate

Without CI:

```text
developer forgets to run tests
↓
broken code pushed
↓
problem discovered later
```

With CI:

```text
code pushed
↓
tests run automatically
↓
failure blocks progression
```

This makes quality less dependent on memory and discipline.

The process itself enforces the rule.

---

# ⚡ 47. Fast Feedback Matters

Testing is most useful when feedback arrives quickly.

Compare:

```text
Write code
↓
wait 40 minutes
↓
test fails
```

with:

```text
Write code
↓
wait 500 ms
↓
test fails
```

The shorter loop encourages experimentation.

Fast tests are not merely convenient.

They change developer behaviour.

---

# 🧭 48. Test Scope Should Match the Question

If the question is:

> Does compatibility math work?

Use a unit test.

If the question is:

> Does JPA persist the entity?

Use an integration test.

If the question is:

> Does invalid JSON return HTTP 400?

Use an API test.

If the question is:

> Can the user complete the full fridge-to-recipe flow?

Use an end-to-end test.

The mistake is asking a question at the wrong boundary.

---

# 🏗️ 49. Example Testing Strategy for This Project

For the restaurant/fridge application:

```text
UNIT
├─ recipe compatibility
├─ shopping-list calculation
├─ dietary filtering
├─ service decisions
└─ validation rules

INTEGRATION
├─ JPA entity mappings
├─ repositories
├─ PostgreSQL
└─ Flyway migrations

API
├─ controllers
├─ JSON contracts
├─ validation
├─ error responses
└─ status codes

END-TO-END
└─ fridge image → ingredients → recipe → cooking flow
```

This creates layered confidence.

---

# ☕ 50. JUnit 5

JUnit is the primary Java testing framework used to define and run tests.

```java
@Test
void returnsSixtyPercentCompatibility() {
    int result = matcher.calculate(3, 5);

    assertEquals(60, result);
}
```

JUnit provides:

- test lifecycle;
- assertions;
- parameterised tests;
- grouping;
- exception testing;
- repeated tests;
- test discovery.

JUnit answers:

> **How do we express and execute Java tests?**

---

# 🎭 51. Mockito

Mockito helps create controlled replacements for dependencies.

```java
RecipeRepository repository = mock(RecipeRepository.class);

when(repository.findById(1L))
    .thenReturn(Optional.of(recipe));
```

Then:

```java
Recipe result = service.getRecipe(1L);
```

And verify:

```java
assertEquals(recipe, result);
```

Mockito is especially useful when testing service logic without involving databases, networks or external systems.

---

# 🌱 52. Spring Boot Testing

Spring Boot adds tools for testing different layers.

```text
plain JUnit
→ pure unit logic

Mockito
→ isolated dependencies

@DataJpaTest
→ JPA/repository layer

@WebMvcTest
→ controller boundary

@SpringBootTest
→ larger application integration
```

The principle remains:

> **Load only as much of the system as the question requires.**

Do not start the entire application when a pure Java unit test is enough.

---

# 🧪 53. Parameterised Tests

Sometimes one behaviour must hold across many inputs.

Instead of four separate nearly identical tests, use parameterised tests.

Example concept:

```text
0/5 → 0%
1/5 → 20%
3/5 → 60%
5/5 → 100%
```

This is especially useful for validation, calculations, edge cases and mappings.

---

# 🧱 54. Boundary Cases

Bugs often hide at boundaries.

Examples:

```text
0
1
maximum
minimum
empty
null
duplicate
missing
already exists
```

For compatibility:

```text
0 required ingredients
all ingredients present
no ingredients present
duplicate ingredients
```

A strong test suite deliberately probes boundaries.

---

# 💥 55. Exception Testing

Failure behaviour is still behaviour.

```java
assertThrows(
    RecipeNotFoundException.class,
    () -> service.getRecipe(999L)
);
```

This proves the system rejects an invalid condition in the expected way.

Testing only successful behaviour leaves half the system unexplored.

---

# 🚨 56. Negative Tests

Positive test:

```text
valid user saves recipe
→ success
```

Negative test:

```text
missing recipe
→ rejected
```

Other negative cases:

- invalid input;
- unauthorized user;
- duplicate record;
- malformed JSON;
- missing image;
- unsupported file type;
- database unavailable.

Good systems define failure deliberately.

Tests should too.

---

# 🔐 57. Security Testing

Security behaviour can also be tested.

Examples:

```text
unauthenticated request → 401
authenticated but forbidden → 403
invalid token → rejected
user A cannot access user B's private resource
```

Security requirements should become executable where possible.

---

# 🚀 58. Performance Tests

Correctness is not always enough.

A system may return the right answer but take 30 seconds.

Performance tests ask:

```text
How fast?
How many?
How much load?
What happens under pressure?
```

Examples include:

- load tests;
- stress tests;
- latency tests;
- throughput tests.

These belong to a different quality dimension than ordinary unit testing.

---

# 🧱 59. Testing Is One Part of Quality

Tests do not replace:

- code review;
- static analysis;
- type systems;
- monitoring;
- logging;
- observability;
- security review;
- production metrics.

A broader model is:

```text
Static checks
+
Tests
+
Review
+
CI
+
Observability
=
Layered confidence
```

No single mechanism is enough.

---

# 🪵 60. Logging and Testing

Tests answer:

```text
Did known behaviour work?
```

Logs help answer:

```text
What happened when something failed?
```

```text
Tests
→ pre-deployment evidence

Logs / metrics / traces
→ runtime evidence
```

Testability and observability complement each other.

---

# 🌐 61. Testing External APIs

External APIs are dangerous to rely on directly in every test because they may be:

- slow;
- unavailable;
- rate-limited;
- expensive;
- nondeterministic.

A good pattern is:

```text
business logic
      ↓
interface / port
      ↓
external adapter
```

Unit tests mock the port.

Integration tests may test the adapter against a controlled environment.

This keeps external uncertainty outside the core domain logic.

---

# 🗄️ 62. Testing Databases

Database tests should prove things mocks cannot.

Examples:

```text
mapping is correct
foreign key works
query returns expected rows
transaction behaves correctly
migration applies
constraint rejects invalid state
```

For integration tests, use a realistic database environment.

The closer the test database is to production behaviour, the more meaningful the evidence.

---

# 🌱 63. Testcontainers

Testcontainers can start real infrastructure such as PostgreSQL inside disposable containers for tests.

```text
test starts
↓
temporary PostgreSQL starts
↓
migration runs
↓
test executes
↓
container disappears
```

This gives stronger integration confidence without requiring every developer to manually prepare a permanent test database.

---

# 🛡️ 64. Tests as Invariants

Some tests encode rules that should remain true regardless of implementation changes.

Example:

```text
compatibility percentage must always remain between 0 and 100
```

Another:

```text
a saved recipe must always reference an existing user
```

These tests are especially valuable because they protect deep system truths rather than incidental examples.

---

# 🧠 65. TDD and Invariants

TDD becomes especially powerful when the test is not merely:

```text
example input → example output
```

but:

```text
this rule must remain true
```

Examples:

```text
missing ingredients can never be negative
a confirmed fridge belongs to exactly one user
invalid dietary state cannot be persisted
```

The closer tests move toward stable rules, the more durable they become.

---

# ❌ 66. Common Testing Anti-Patterns

## Testing implementation instead of behaviour

```text
verify every internal call
```

## Giant tests

```text
one test proves fifteen behaviours
```

## No assertions

```text
code executes
but nothing is checked
```

## Flaky tests

```text
sometimes green
sometimes red
```

## Shared mutable state

```text
tests depend on each other
```

## Mocking everything

```text
test proves mock choreography
```

## Only testing the happy path

```text
failures remain undefined
```

## Chasing coverage

```text
more executed lines
without better evidence
```

---

# 🧹 67. One Test Should Have One Clear Reason to Fail

A test may contain multiple assertions if they prove one coherent behaviour.

But if a test can fail for ten unrelated reasons, diagnosis becomes slow.

Prefer:

```text
one behaviour
→ one understandable failure
```

This keeps feedback precise.

---

# 🏷️ 68. Naming Tests

Good names describe behaviour.

Useful pattern:

```text
<behaviour>_when_<condition>
```

Example:

```java
returnsNotFound_whenRecipeDoesNotExist()
```

Or natural sentence style:

```java
void returnsMissingIngredientsWhenFridgeIsIncomplete()
```

When a test fails in CI, its name should immediately tell you what behaviour broke.

---

# 🧠 69. Tests Are Executable Documentation

Normal documentation can drift away from reality.

A test has an advantage:

```text
if the documented behaviour is false
the test fails
```

For example:

```java
@Test
void unauthenticatedUserCannotSaveRecipe() {
}
```

This communicates a domain rule while also enforcing it.

---

# 🔄 70. TDD Is a Feedback Loop

TDD is best understood as:

```text
Hypothesis
↓
Executable expectation
↓
Implementation
↓
Feedback
↓
Design adjustment
```

This is why small loops matter.

The faster the loop, the less uncertainty accumulates between decisions.

---

# 🧭 71. When TDD Is Most Valuable

TDD tends to be especially useful when:

- business rules are clear;
- edge cases matter;
- behaviour can be expressed precisely;
- refactoring is likely;
- regressions are expensive;
- domain logic is important.

Examples:

```text
pricing
matching
validation
permissions
calculations
state transitions
```

---

# 🌫️ 72. When TDD Is Harder

TDD can be less natural when:

- rapidly exploring an unknown UI;
- experimenting with visual design;
- prototyping uncertain architecture;
- interacting with poorly understood external systems;
- requirements are still extremely fluid.

That does not mean “do not test.”

It means the feedback loop may begin with exploration before being stabilised into tests.

```text
explore
↓
discover behaviour
↓
stabilise
↓
capture with tests
```

---

# 🧠 73. TDD Is Not a Religion

The purpose is not:

```text
test-first at all costs
```

The purpose is:

```text
use executable feedback to control complexity
```

Sometimes strict TDD is ideal.

Sometimes tests are written immediately after a spike.

Sometimes integration tests matter more than unit tests.

The invariant remains:

> **Important behaviour should acquire reliable evidence.**

---

# 📐 74. Testing Strategy Should Follow Risk

Do not test everything equally.

Ask:

```text
What would be expensive if wrong?
What changes frequently?
What contains complex rules?
What sits at a critical boundary?
What has failed before?
```

Higher-risk areas deserve stronger evidence.

This is risk-based testing.

---

# 🧬 75. A Useful Confidence Stack

```text
Requirement
   ↓
Unit Tests
   ↓
Integration Tests
   ↓
API Tests
   ↓
End-to-End Tests
   ↓
CI
   ↓
Production Observability
```

Each layer catches a different class of failure.

Confidence emerges from the stack.

---

# ✅ 76. Definition of Done

A feature should not be considered complete merely because the code compiles.

A stronger definition is:

```text
requirement understood
↓
implementation complete
↓
important behaviours tested
↓
failure states tested
↓
integration verified
↓
PR reviewed
↓
CI passes
```

Testing is therefore part of delivery, not an optional activity afterwards.

---

# 🧪 77. Practical Test Checklist

### 🎯 Behaviour
- What behaviour am I proving?
- Does the test name explain it?

### 🧩 Boundaries
- Is this a unit, integration, API or E2E question?
- Am I testing at the correct layer?

### ✅ Evidence
- Is there a meaningful assertion?
- Would the test fail if the behaviour were wrong?

### 🚨 Failure
- Have important negative cases been tested?
- Are boundary cases covered?

### 🎭 Mocks
- Am I mocking a dependency boundary?
- Or am I mocking so much that the test is meaningless?

### ⚡ Reliability
- Is the test deterministic?
- Is it fast enough for its layer?
- Can it run independently?

### 🧹 Maintainability
- Is the setup minimal?
- Is the behaviour easy to understand?
- Is the test coupled to internals unnecessarily?

---

# 🌳 78. The Deeper Story

At first, programming can feel like:

```text
write code
→ run it
→ see what happens
```

As systems grow, this stops scaling.

The number of possible interactions expands.

A change in one place can affect another place you did not manually check.

Testing introduces structured memory into the engineering process.

```text
Human remembers requirement
        ↓
Test stores requirement as behaviour
        ↓
Future code is challenged against it
```

The test suite becomes a persistent memory of what the system has promised to keep true.

That is why mature codebases rely so heavily on automated tests.

---

# 🧠 79. Tests as System Memory

A test suite records:

```text
known rules
known failures
known edge cases
known contracts
known expectations
```

When a developer changes the system months later, they may not know the full history.

But the tests still remember.

```text
Developer forgets
↓
Test suite remembers
```

This is one of the deepest reasons automated testing matters.

---

# 🧬 80. Final Compression

Testing exists because:

> **Code makes claims. Tests provide evidence.**

TDD changes the order:

```text
desired behaviour
↓
failing test
↓
minimum implementation
↓
passing test
↓
refactor
```

Different tests answer different questions:

```text
Unit
→ does the rule work?

Integration
→ do real components work together?

API
→ does the boundary behave correctly?

E2E
→ does the whole user flow work?
```

And the deeper engineering loop is:

```text
PROMISE
   ↓
IMPLEMENTATION
   ↓
EVIDENCE
   ↓
CONFIDENCE
   ↓
CHANGE
   ↓
RE-VERIFICATION
```

The goal is not to have the most tests.

The goal is:

> **Enough reliable evidence that we can change the system without losing control of what it is supposed to do.**
