# ⚛️🧪 The Story of Frontend Testing

## How React Turns System State Into Human Experience — and How We Prove the Translation Is Correct

---

# 1. 🌍 The Frontend Lives Between Two Worlds

The backend lives primarily in the world of:

```text
domain concepts
rules
persistence
services
APIs
```

The user does not experience any of those things directly.

A user experiences:

```text
buttons
forms
images
text
loading
errors
navigation
feedback
choices
```

So the frontend sits between:

```text
SYSTEM
```

and:

```text
HUMAN
```

Its job is to translate one world into the other.

```text
Domain / Application State
          ↓
        API
          ↓
     Frontend State
          ↓
       React
          ↓
    Visible Interface
          ↓
        Human
```

And in the other direction:

```text
Human Intent
     ↓
Interaction
     ↓
React Event
     ↓
State / Request
     ↓
API
     ↓
Application Behaviour
```

> **Frontend testing exists to prove that human intent is translated into the correct system behaviour, and that system state is translated back into the correct human experience.**

---

# 2. 🧠 The Backend Protects Domain Truth; the Frontend Protects Interaction Truth

In Domain-Driven Design, we asked:

> What must remain true in the domain?

Examples:

```text
Compatibility must remain between 0 and 100.

Dietary exclusions override recipe compatibility.

Confirmed fridge ingredients become authoritative state.
```

The frontend has a different but related responsibility.

It must preserve truths such as:

```text
A loading request should look like loading.

A failed request should not look like success.

A disabled action should not appear available.

The user should see the result of the action they actually performed.

Invalid input should receive meaningful feedback.

A successful state change should be reflected in the interface.
```

These are **interaction invariants**.

DDD protects meaning inside the system.

Frontend design protects meaning at the human boundary.

Testing connects them.

---

# 3. 🪞 The UI Is a Projection

A useful way to understand a frontend is:

> **The UI is a projection of state.**

Suppose the application contains:

```text
status = "loading"
```

The user may see:

```text
Loading recipes...
```

If:

```text
status = "error"
```

the projection may become:

```text
We couldn't load recipes.
Try again.
```

If:

```text
status = "success"
recipes = [...]
```

the projection becomes:

```text
Recipe cards
```

So:

```text
STATE
  ↓
RENDER
  ↓
VISIBLE EXPERIENCE
```

React makes this relationship unusually explicit.

A React component is, conceptually:

```text
state + props
      ↓
render
      ↓
interface
```

That gives us the deepest reason React is testable.

---

# 4. ⚛️ React Is a State-to-Interface Machine

At the simplest level:

```jsx
function Greeting({ name }) {
  return <h1>Hello {name}</h1>;
}
```

Conceptually:

```text
Input:
name = "Simi"

↓

Projection:

<h1>Hello Simi</h1>
```

Add state:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increase
      </button>
    </>
  );
}
```

Now the model becomes:

```text
State
  ↓
Render
  ↓
User Action
  ↓
State Transition
  ↓
Re-render
```

This loop is the heart of React.

And therefore it is the heart of React testing.

---

# 5. 🧬 The Frontend Testing Invariant

Across components, pages, forms, hooks and full applications, one principle remains stable:

> **Given a particular visible state, when the user performs an action, the interface should move to the correct observable next state.**

That gives us:

```text
GIVEN
visible starting state

WHEN
user interaction occurs

THEN
observable result appears
```

This is the frontend equivalent of:

```text
Arrange
Act
Assert
```

but expressed from the user's perspective.

---

# 6. 👤 Test What the User Experiences

A user does not know your component tree.

They do not know whether you used:

```text
useState
useReducer
Context
Redux
custom hooks
three components
ten components
```

They experience:

```text
Butter Chicken
82% match
2 ingredients missing
[Save Recipe]
```

So a strong frontend test should ask:

```text
Can the user see the expected information?

Can they perform the expected action?

Does the interface respond correctly?
```

not:

```text
Did this private helper run?

Did this internal component render?

Was this state setter called?
```

The closer the test is to the user's experience, the more resilient it becomes to refactoring.

---

# 7. 🔍 Semantic Queries

Suppose the interface contains:

```jsx
<button>Save Recipe</button>
```

Weak:

```javascript
container.querySelector('.save-button')
```

Stronger:

```javascript
screen.getByRole('button', { name: /save recipe/i })
```

Why?

Because a user interacts with:

```text
a button called Save Recipe
```

not:

```text
a DOM node with a particular CSS class
```

This also connects testing to accessibility.

If an element has:

```text
clear role
clear label
clear accessible name
```

it is easier for both:

```text
users
and
tests
```

to understand.

---

# 8. ♿ Accessibility and Testability Converge

A form like:

```jsx
<label htmlFor="cuisine">Cuisine</label>
<select id="cuisine">...</select>
```

has clear semantics.

A test can find it naturally.

But:

```jsx
<div onClick={...}>Submit</div>
```

has weaker semantics.

That should make us ask:

> Is the test hard because the test is bad, or because the interface itself is unclear?

Frontend tests are therefore also design feedback.

---

# 9. 🧭 The Frontend Is a State Machine

Take recipe discovery.

The interface may move through:

```text
IDLE
↓
LOADING
↓
SUCCESS
```

or:

```text
IDLE
↓
LOADING
↓
EMPTY
```

or:

```text
IDLE
↓
LOADING
↓
ERROR
```

So the real structure is:

```text
          ┌──────── SUCCESS
IDLE → LOADING
          ├──────── EMPTY
          └──────── ERROR
```

Even if the code does not explicitly use a state machine, the user experiences one.

This means frontend testing should focus heavily on:

```text
states
+
transitions
```

---

# 10. 🌫️ Bugs Often Live Between States

Common frontend bugs are transitional:

```text
loading never finishes

old data stays visible after an error

submit can be clicked twice

empty results look like an error

error remains after successful retry

a stale response overwrites a newer one
```

These are not simply rendering bugs.

They are **state-transition bugs**.

So frontend testing is not just:

```text
does component X exist?
```

It is:

> **Does the interface move through the correct sequence of meanings?**

---

# 11. 🔄 The Fundamental React Test Loop

The core loop is:

```text
RENDER
  ↓
OBSERVE
  ↓
INTERACT
  ↓
STATE CHANGES
  ↓
RENDER AGAIN
  ↓
OBSERVE AGAIN
```

Example:

```javascript
render(<RecipeSearch />);

await user.click(
  screen.getByRole('button', { name: /find recipes/i })
);

expect(
  screen.getByText(/loading recipes/i)
).toBeInTheDocument();

expect(
  await screen.findByText(/butter chicken/i)
).toBeInTheDocument();
```

The test follows the same causal path as the user.

---

# 12. 🎭 Simulate Intent, Not Internal Callbacks

A user does not call:

```text
onClick()
```

They:

```text
click
type
select
tab
submit
```

So testing should enter through those same interaction boundaries.

Conceptually:

```text
USER INTENT
↓
INTERACTION
↓
REACT EVENT
↓
STATE TRANSITION
↓
VISIBLE RESULT
```

This preserves the real causal chain.

---

# 13. 🧩 Props Are Inputs

A component such as:

```jsx
<RecipeCard
  name="Butter Chicken"
  compatibility={82}
/>
```

can be understood as:

```text
INPUT
↓
PROJECTION
```

A useful test asks:

```text
Given name = Butter Chicken
and compatibility = 82

does the user see:
Butter Chicken
82% match
```

Simple.

Observable.

Stable.

---

# 14. 🧠 Local State Is Interaction Memory

React state often exists because the interface needs to remember something about the current interaction.

Examples:

```text
selectedCuisine
isSubmitting
currentStep
searchText
selectedIngredients
errorMessage
```

This is different from domain truth.

A useful distinction:

```text
DOMAIN STATE
= truth about the product world

UI STATE
= truth about the current interaction
```

Example:

```text
Saved Recipe
```

may be domain state.

But:

```text
Save button is currently showing a spinner
```

is UI state.

Frontend testing should know which one it is proving.

---

# 15. 📡 Server State Is Different Again

Server state is data that:

- originates somewhere else;
- may be stale;
- may be loading;
- may fail;
- may change independently.

Example:

```text
recipe recommendations from Spring Boot
```

The frontend does not own the authoritative truth.

It owns a temporary representation of it.

```text
Server Truth
↓
Frontend Representation
↓
Rendered Projection
```

---

# 16. 🧠 The Frontend Should Not Invent Domain Truth

Suppose the backend returns:

```json
{
  "compatibility": 82
}
```

The frontend should normally project:

```text
82% match
```

It should not independently reimplement the business rule unless the architecture explicitly requires that.

Otherwise we risk:

```text
Backend says 82
Frontend calculates 79
```

Now two truths exist.

DDD helps decide where authority belongs.

Frontend tests make sure the authoritative result is represented correctly.

---

# 17. 🧪 Different Boundaries, Different Tests

Backend test:

```text
Does compatibility calculate correctly?
```

Frontend test:

```text
Given compatibility = 82,
does the user see 82% match?
```

End-to-end test:

```text
Given a known fridge and recipe,
does the full application eventually show the correct compatibility?
```

Same product behaviour.

Different boundaries.

---

# 18. ⚛️ Component Tests

A component test asks:

> **Does this coherent piece of interface behave correctly at its boundary?**

For a `RecipeCard`:

```text
recipe name appears

compatibility appears

missing ingredient count appears

save action is available

save behaviour is triggered correctly
```

A component test does not need to prove:

```text
PostgreSQL actually persisted the recipe
```

That belongs to another layer.

---

# 19. 🧬 A Component Is Not Automatically a Test Unit

Do not assume:

```text
one file
=
one test
```

A tiny visual component may not deserve an isolated test.

A larger component may be best tested together with several children.

The useful boundary is:

> **The smallest coherent piece of behaviour worth proving independently.**

This mirrors the same cohesion logic we used in DDD.

---

# 20. ⚛️ Components as Interaction Boundaries

Consider:

```text
IngredientEditor

Inputs:
detected ingredients

Visible State:
editable ingredient list

Interactions:
add
remove
rename
confirm

Output:
confirmed ingredient set
```

Once we model it this way, the tests begin to derive themselves.

---

# 21. 🧠 Derive Tests From Behaviour, Not Files

Ask:

```text
What can the user see?

What can the user do?

What states exist?

What transitions exist?

What can fail?
```

For `IngredientEditor`:

```text
DETECTED
↓ edit
EDITING
↓ confirm
CONFIRMING
↓ success
CONFIRMED
```

Failure:

```text
CONFIRMING
↓ request fails
ERROR
```

Those are the meaningful test cases.

---

# 22. 🔴🟢🔵 Frontend TDD

Suppose the requirement says:

> The confirm button is disabled when there are no ingredients.

### 🔴 Red

```javascript
render(<IngredientEditor ingredients={[]} />);

expect(
  screen.getByRole('button', { name: /confirm/i })
).toBeDisabled();
```

### 🟢 Green

Implement the minimum behaviour.

### 🔵 Refactor

Improve component structure without changing the user-visible rule.

Again, TDD begins with:

```text
observable behaviour
```

not:

```text
implementation detail
```

---

# 23. 🧠 TDD Can Discover Frontend State

Suppose we write:

```text
When the user submits,
the button becomes disabled
until the request completes.
```

That reveals a missing state:

```text
SUBMITTING
```

The test helps discover the interaction model.

So the loop becomes:

```text
User Rule
↓
Test
↓
Required State
↓
Implementation
↓
Refactor
```

This is the frontend equivalent of DDD/TDD discovering a domain concept.

---

# 24. 🧬 Interaction Invariants

Frontend applications also have invariants.

Examples:

```text
A form cannot submit twice while already submitting.

An error must not be shown as success.

Detected ingredients remain editable until confirmed.

The user cannot move beyond the final cooking step.

A disabled action cannot be triggered.
```

These are stable interaction truths.

They are excellent candidates for tests.

---

# 25. 📋 Forms Are Small State Machines

A form may move through:

```text
PRISTINE
↓
DIRTY
↓
VALID / INVALID
↓
SUBMITTING
↓
SUCCESS / ERROR
```

Testing:

```text
input exists
button exists
```

is not enough.

The real behaviour is in the transitions.

---

# 26. 🚨 Frontend Validation vs Backend Validation

Frontend validation exists to improve interaction.

Backend validation exists to protect truth.

Example:

Frontend:

```text
missing cuisine
→ show immediate message
→ prevent pointless request
```

Backend:

```text
missing cuisine
→ reject request
```

The frontend is not the security boundary.

A user can bypass it.

So both layers need tests for their own responsibility.

---

# 27. ⏳ Async Behaviour Is Central

Most useful React interfaces are asynchronous.

```text
click
↓
request begins
↓
time passes
↓
response returns
↓
state updates
↓
UI changes
```

Tests must therefore handle time.

Example:

```javascript
await user.click(button);

expect(
  await screen.findByText(/butter chicken/i)
).toBeInTheDocument();
```

The test waits for the meaningful result rather than assuming it already exists.

---

# 28. ⌛ Loading Is Behaviour

Loading communicates:

```text
your action was received
the system is working
the result is not ready
```

That matters.

Useful tests:

```text
request begins
→ loading appears

request resolves
→ loading disappears

request fails
→ loading disappears
→ error appears
```

---

# 29. 🚨 Error Is a First-Class State

Possible failures:

```text
network unavailable
backend 500
validation 400
unauthorized 401
not found 404
upload failure
AI failure
timeout
```

The frontend must decide:

```text
What does the user see?

What can they do next?
```

Testing makes failure behaviour intentional.

---

# 30. 🌵 Empty Is Not Error

A response of:

```json
[]
```

may mean:

```text
success
+
no matching recipes
```

That is not necessarily an error.

So distinguish:

```text
LOADING
ERROR
EMPTY
SUCCESS
```

A good test suite preserves those meanings.

---

# 31. 🗺️ Navigation Is State

Routes represent user location inside the application.

```text
/fridge
/recipes
/recipes/42
/cooking/42
```

Useful tests:

```text
select recipe
→ recipe page appears

invalid recipe route
→ not-found experience appears
```

Navigation is part of the interaction model.

---

# 32. 🌐 The API Boundary

React eventually reaches outside itself:

```text
React
↓
HTTP
↓
Spring Boot
```

Now we inherit:

```text
latency
network errors
response shapes
backend failure
```

If every frontend test uses the real backend, the test suite becomes expensive and fragile.

So we need a controlled boundary.

---

# 33. 🎭 Mock the Network Boundary

A useful frontend strategy is:

```text
React Application
      ↓
      HTTP
      ↓
Controlled Test Response
```

The application still performs its real frontend behaviour.

The test controls the world outside it.

This allows us to simulate:

```text
success
empty
400
404
500
timeout
```

without rewriting component internals.

---

# 34. 🧠 Why Boundary Mocking Is Stronger

Weak approach:

```text
mock hook
mock child component
mock fetch helper
mock state setter
```

At some point the test may no longer resemble the real application.

Stronger:

```text
let React behave normally
control the HTTP boundary
observe the result
```

The test preserves more of the real causal chain.

---

# 35. 📜 The API Contract Connects Frontend and Backend

Suppose Spring Boot promises:

```json
{
  "recipeId": 42,
  "name": "Butter Chicken",
  "compatibility": 82
}
```

The frontend consumes that contract.

So:

```text
BACKEND
   ↓ produces
API CONTRACT
   ↑ consumes
FRONTEND
```

Backend API tests prove the producer side.

Frontend integration tests prove the consumer side.

Contract testing can protect the seam itself.

---

# 36. 📦 DTOs Become Frontend Inputs

On the backend:

```text
DTO
= boundary shape
```

On the frontend:

```text
DTO / JSON
= input to the interface
```

So:

```text
Backend Domain
↓
Response DTO
↓
JSON
↓
Frontend State
↓
React Projection
```

Frontend tests prove that the projection is correct.

---

# 37. 🧠 API Shape Is Not Automatically UI Shape

The API may return:

```json
{
  "available": 6,
  "required": 8
}
```

The UI may show:

```text
6 of 8 ingredients available
```

That is presentation logic.

But if:

```text
compatibility percentage
```

is a true domain rule, the backend may remain authoritative.

DDD tells us where meaning belongs.

Frontend testing enforces the presentation consequences.

---

# 38. 🧮 Pure Presentation Logic

Some frontend behaviour is pure.

Example:

```javascript
formatCookingTime(90)
```

Expected:

```text
1 hr 30 min
```

This can be tested without React.

Use the smallest useful boundary.

Not every frontend test needs rendering.

---

# 39. 🪝 Hooks

Hooks may contain:

```text
state
effects
async logic
shared interaction behaviour
```

Do not test a hook merely because it exists.

Ask:

> Does this hook represent meaningful reusable behaviour that deserves its own test?

If not, a component/page test may be stronger.

---

# 40. 🧩 Reducers Make Transitions Explicit

A reducer:

```text
(state, action) → nextState
```

is effectively an explicit state machine.

Example:

```text
IDLE + SEARCH
→ LOADING

LOADING + SUCCESS
→ SUCCESS

LOADING + FAILURE
→ ERROR
```

These can be tested as pure functions.

This gives frontend state the same structural clarity we seek in domain models.

---

# 41. 🧠 State Management Is Memory With Rules

A useful compression:

> **State management is memory with rules.**

Frontend tests ask:

```text
Did we remember the right thing?

Did the correct event change it?

Did the new state render correctly?
```

So:

```text
Event
↓
State Transition
↓
Memory
↓
Render
```

That is the state-management testing story.

---

# 42. 📸 Snapshot Tests

A snapshot:

```text
render
↓
serialize
↓
save
↓
compare later
```

can detect change.

But it does not automatically tell us whether the change is wrong.

A huge snapshot often creates:

```text
noise
```

rather than:

```text
meaningful evidence
```

Use snapshots selectively.

Do not let them replace behavioural assertions.

---

# 43. 🧪 Frontend Integration Tests

A frontend integration test may combine:

```text
Search Form
+
Network Request
+
Loading State
+
Recipe Results
```

Example:

```text
choose cuisine
↓
click Find Recipes
↓
loading appears
↓
controlled API returns recipes
↓
cards appear
```

This often gives strong confidence at moderate cost.

---

# 44. 🧱 Page Tests Are Often a Useful Boundary

A page naturally combines:

```text
routing
state
components
network
interaction
```

while avoiding the cost of:

```text
real browser
real backend
full deployment
```

That makes page-level integration tests a useful middle layer.

---

# 45. 🌍 End-to-End Testing

E2E asks:

> **Can a real user complete the assembled journey?**

For this project:

```text
Open app
↓
choose cuisine
↓
upload fridge image
↓
confirm ingredients
↓
receive recommendations
↓
open recipe
↓
view missing ingredients
↓
start cooking
```

This tests the full product path.

---

# 46. 🔺 The Frontend Testing Pyramid

```text
             /            /             / E2E          /------         / Page /         /Integration       /------------      / Component         /----------------    / Pure Logic          /____________________```

As we move upward:

```text
realism ↑
cost ↑
execution time ↑
failure ambiguity ↑
```

As we move downward:

```text
speed ↑
precision ↑
isolation ↑
```

The goal is layered confidence.

---

# 47. 🧠 Use the Cheapest Test That Proves the Behaviour

Question:

```text
Does formatTime(90) work?
```

Use:

```text
pure unit test
```

Question:

```text
Does RecipeCard display compatibility?
```

Use:

```text
component test
```

Question:

```text
Does search show results after API success?
```

Use:

```text
integration/page test
```

Question:

```text
Can the full user journey complete?
```

Use:

```text
E2E
```

The right test boundary comes from the question.

---

# 48. 🧬 Frontend and Backend Testing Mirror Each Other

Backend:

```text
Domain Unit Test
→ Service Test
→ Repository Integration
→ API Test
→ E2E
```

Frontend:

```text
Pure Logic Test
→ Component Test
→ Page Integration
→ API Boundary
→ E2E
```

They are two halves of one product.

---

# 49. 🔗 The API Is the Seam

```text
BACKEND
Domain
↓
Application
↓
Controller
↓
JSON
════════════ API CONTRACT ════════════
JSON
↓
Frontend State
↓
React
↓
Human
FRONTEND
```

Backend testing moves upward toward the seam.

Frontend testing moves downward from the seam toward the user.

---

# 50. 🧠 The Full Causal Chain

```text
USER INTENT
    ↓
FRONTEND INTERACTION
    ↓
FRONTEND STATE
    ↓
HTTP REQUEST
    ↓
APPLICATION USE CASE
    ↓
DOMAIN RULE
    ↓
PERSISTENCE
    ↓
HTTP RESPONSE
    ↓
FRONTEND STATE
    ↓
RENDER
    ↓
USER OBSERVATION
```

Testing can enter at any point.

The question is:

> **Which part of the causal chain are we trying to prove?**

---

# 51. 🧪 Example: Save Recipe

Requirement:

> The user can save a recipe.

Backend domain rule:

```text
SavedRecipe must reference a valid User and Recipe.
```

Frontend interaction rule:

```text
The save action shows progress and eventually communicates success or failure.
```

---

# 52. ⚛️ Component-Level Evidence

```text
Given:
an unsaved recipe

When:
the user clicks Save Recipe

Then:
pending state becomes visible
```

---

# 53. 🌐 Frontend Integration Evidence

```text
Given:
API returns success

When:
user clicks Save Recipe

Then:
request is sent
and
"Recipe saved" appears
```

Failure:

```text
Given:
API returns 500

Then:
error feedback appears
and
retry remains possible
```

---

# 54. ☕ Backend API Evidence

```text
POST /users/1/saved-recipes

Given:
user and recipe exist

Then:
success status
and
expected response
```

---

# 55. 🧠 Backend Domain Evidence

```text
Given:
recipe does not exist

When:
save is requested

Then:
operation is rejected
```

---

# 56. 🌍 End-to-End Evidence

```text
Open real application
↓
open recipe
↓
click Save Recipe
↓
real backend persists it
↓
return later
↓
recipe remains saved
```

Now one feature has layered proof.

---

# 57. 🎯 What Each Layer Proves

```text
Domain Test
→ business truth

API Test
→ transport contract

Component Test
→ local interaction

Frontend Integration
→ UI + network behaviour

E2E
→ assembled user journey
```

---

# 58. 🧪 Minimum Server-Backed UI States

For most server-backed features, consider:

```text
IDLE
LOADING
SUCCESS
EMPTY
ERROR
```

Ask:

```text
What does the user see in each?

What transitions are possible?

What actions are available?
```

That usually gives you a strong minimum test set.

---

# 59. 🧠 Optimistic UI Adds More Obligations

Optimistic behaviour:

```text
click Save
↓
UI immediately shows Saved
↓
server request
```

If the request fails:

```text
rollback
```

Now tests must cover:

```text
optimistic state
successful confirmation
failure rollback
```

More sophisticated interaction means more states to preserve.

---

# 60. 🚫 Do Not Test `useState`

Weak:

```text
verify setIsOpen(true)
```

Stronger:

```text
click Edit Ingredients
↓
ingredient editor becomes visible
```

The setter is implementation.

The visible editor is behaviour.

---

# 61. 🚫 Do Not Test Private Component Structure

Weak:

```text
RecipePage renders three RecipeCard components
```

Stronger:

```text
Given three recipes,
the user can see all three recipe names
```

The second survives refactoring.

---

# 62. ⚠️ `data-testid` Is a Fallback

Sometimes a semantic query is impossible or inappropriate.

Then a test ID can help.

But if every test depends on test IDs, ask whether:

```text
the interface semantics are weak
```

or:

```text
the tests are too tightly coupled to implementation
```

Prefer meaningful user-facing queries where possible.

---

# 63. ♿ Accessibility Is Behaviour

Important behaviours include:

```text
inputs have labels
buttons have meaningful names
keyboard navigation works
errors are associated with fields
dialogs can be understood
focus moves sensibly
```

An interface that works only for a mouse user is not fully correct.

Frontend testing can protect accessibility expectations too.

---

# 64. 🧠 Testing Is Design Feedback

Backend tests may reveal:

```text
too many dependencies
```

Frontend tests may reveal:

```text
too many component responsibilities

unclear semantics

ambiguous state

poor API shape

buried side effects
```

Difficulty testing is information.

Do not always solve it with more mocks.

Sometimes improve the design.

---

# 65. 🧲 Giant Components Create Giant Tests

If one component owns:

```text
routing
fetching
validation
modal state
saving
formatting
analytics
error handling
```

the test will likely become difficult.

That may indicate weak cohesion.

The same architectural rule appears again:

> **Keep strongly related responsibilities together. Separate things that change for different reasons.**

---

# 66. 🧱 But Do Not Split Everything

The opposite failure is atomising every tiny piece.

```text
ButtonText
ButtonWrapper
ButtonIcon
ButtonBehaviour
```

with a test for each.

That creates artificial boundaries.

Test meaningful units of behaviour.

---

# 67. 🧠 The Component Tree Is Not the Domain Tree

A domain `Recipe` may appear through:

```text
RecipeCard
RecipeDetailPage
SavedRecipeList
CookingScreen
```

The domain model describes meaning.

The component tree describes interaction and presentation.

They are related.

They are not the same model.

---

# 68. 🎨 Visual State Can Still Be Behaviour

Not every style needs a test.

But styling that communicates:

```text
disabled
selected
hidden
expanded
invalid
loading
```

can be behavioural.

Test the meaning the user receives, not arbitrary CSS implementation details.

---

# 69. 📱 React Native Changes the Renderer, Not the Story

React Native replaces:

```text
DOM
```

with:

```text
native UI components
```

But the deeper loop remains:

```text
state
↓
render
↓
interaction
↓
state transition
↓
observable result
```

So the testing philosophy carries across React Web and React Native.

---

# 70. 📷 The Fridge Image Flow

For this project:

```text
User chooses / captures image
↓
preview appears
↓
user submits
↓
upload begins
↓
processing state appears
↓
detected ingredients return
↓
ingredient editor appears
```

Tests should derive directly from this flow.

---

# 71. 🧪 Image Flow — Component Level

```text
Given:
no image selected

Then:
submit is unavailable

When:
image is selected

Then:
selected state / preview appears
and
submit becomes available
```

---

# 72. 🌐 Image Flow — Integration Level

```text
Given:
upload API returns detected ingredients

When:
user submits image

Then:
processing state appears

And later:
detected ingredients appear for correction
```

Failure:

```text
Given:
upload fails

Then:
retry feedback appears

And:
ingredients are not falsely marked as confirmed
```

---

# 73. 🧠 The DDD Connection

Backend invariant:

> AI recognition does not automatically become authoritative fridge state.

Frontend consequence:

```text
detected ingredients
↓
must remain editable
↓
user confirms
↓
only then represent confirmed state
```

Frontend test:

```text
Given detected ingredients

Then:
the user can correct them

And:
the interface does not present them as already confirmed
```

This is domain meaning reaching the UI boundary.

---

# 74. 🧬 The Frontend Protects the Interaction Consequence of Domain Rules

Backend:

```text
Confirmed fridge state is authoritative.
```

Frontend:

```text
Detection result remains editable until confirmation.
```

Same product truth.

Different responsibility.

This is how frontend testing connects to DDD without duplicating backend logic.

---

# 75. 🍽️ Recipe Results

Given:

```json
{
  "name": "Butter Chicken",
  "compatibility": 82,
  "availableIngredients": 9,
  "missingIngredients": 2,
  "cookingTime": 45
}
```

The frontend should project:

```text
Butter Chicken
82% match
9 available
2 missing
45 min
```

The test should prove those meanings are visible.

Not that five specific nested elements exist.

---

# 76. 🛒 Shopping List

Backend rule:

```text
Shopping List
=
Required Ingredients
−
Confirmed Fridge Ingredients
```

Backend tests prove the calculation.

Frontend tests prove:

```text
missing ingredients appear

items can be checked

checking one does not change another

empty list has sensible feedback
```

Again:

```text
rule
vs
interaction
```

---

# 77. 👨‍🍳 Cooking Mode

Cooking Mode is naturally stateful:

```text
Step 1 of 7
↓ next
Step 2 of 7
↓ next
...
Step 7 of 7
↓ complete
```

Tests:

```text
Previous unavailable at first step

Next advances one step

Previous moves back one step

final step exposes completion

current instruction matches current step
```

A state machine naturally generates test cases.

---

# 78. 🎯 Tests Should Come From Behavioural Space

Weak strategy:

```text
These lines are uncovered.
Write tests until coverage rises.
```

Stronger:

```text
What states exist?

What transitions exist?

What failures exist?

What boundaries exist?

What promises matter?
```

Then use coverage as feedback.

Coverage tells you where tests travelled.

It does not tell you whether they asked meaningful questions.

---

# 79. 🏷️ Test Names Should Describe Behaviour

Weak:

```javascript
it('works')
```

Better:

```javascript
it('shows detected ingredients after image processing completes')
```

Or:

```javascript
it('allows detected ingredients to be corrected before confirmation')
```

When a test fails in CI, the name should reveal which user promise broke.

---

# 80. 🧹 Tests Should Survive Refactoring

If:

```text
useState
```

becomes:

```text
useReducer
```

but behaviour is unchanged, most tests should remain green.

If `RecipePage` is split into more components but the user experience is unchanged, page tests should remain green.

This is a sign that tests are coupled to behaviour instead of structure.

---

# 81. 🐛 Regression Tests

Bug:

```text
Double-clicking Save sends two requests.
```

Fix process:

```text
write failing test
↓
reproduce bug
↓
fix behaviour
↓
test passes
```

Now the bug becomes permanent system memory.

---

# 82. 🧠 Frontend Tests Are Interaction Memory

Backend tests remember:

```text
domain rules
contracts
edge cases
```

Frontend tests remember:

```text
user journeys
interaction rules
loading behaviour
error recovery
accessibility expectations
UI regressions
```

So:

```text
Backend Tests
= rule memory

Frontend Tests
= interaction memory
```

Together they preserve product behaviour.

---

# 83. 🏭 Frontend Testing in CI

A pipeline may become:

```text
Checkout
↓
Install
↓
Lint
↓
Type Check
↓
Frontend Tests
↓
Backend Tests
↓
Build
↓
E2E
↓
Package
↓
Deploy
```

Every change re-proves the important promises before progression.

---

# 84. ⚡ Fast Feedback Still Matters

Inner loop:

```text
change component
↓
run fast test
↓
feedback
```

Outer loop:

```text
merge / CI
↓
integration
↓
E2E
```

Different test layers serve different feedback speeds.

---

# 85. 🧠 Tools Are Not the Philosophy

A runner such as:

```text
Vitest
Jest
```

executes tests.

React Testing Library helps interact with rendered UI.

A network mocking layer controls external HTTP behaviour.

An E2E tool drives the assembled application.

The tools are replaceable.

The model is more stable.

---

# 86. 🧬 Architecture of Confidence

```text
Pure Logic
      ↓
Component Behaviour
      ↓
Page Integration
      ↓
API Contract
      ↓
Real User Journey
```

Connected to the backend:

```text
                   HUMAN
                     ↓
               FRONTEND
                     ↓
             API CONTRACT
                     ↓
              CONTROLLER
                     ↓
              APPLICATION
                     ↓
                DOMAIN
                     ↓
               DATABASE
```

Testing creates evidence across the whole path.

---

# 87. 🧠 The Deeper DDD Connection

DDD began with:

```text
Reality is too rich.
↓
Select what matters.
↓
Create a domain model.
```

Frontend design begins with another compression:

```text
The domain is too rich to show all at once.
↓
Select what matters to this user,
for this task,
at this moment.
↓
Create an interface.
```

So:

> **The domain model is a compression of reality.  
> The interface is a projection of that model into human attention.**

That is the deep connection.

---

# 88. 🎨 The UI Is a Task-Specific Projection of the Domain

A `Recipe` may contain:

```text
id
name
ingredients
steps
restaurant
metadata
timestamps
status
```

A search card may only show:

```text
name
compatibility
missing ingredients
cooking time
```

The Recipe page shows more.

Cooking Mode shows only what matters to the current step.

Same domain.

Different projection.

---

# 89. 🧠 Frontend Architecture Is Attention Architecture

The backend often asks:

> What is true?

The frontend often asks:

> What does the user need to know or do **right now**?

So:

```text
domain state
↓
task context
↓
selected information
↓
interface
```

Testing verifies that the right information and action appear at the right moment.

---

# 90. 🧬 React Decomposes the Projection

A large interface is split into coherent interaction responsibilities.

```text
RecipePage
├─ RecipeHeader
├─ IngredientAvailability
├─ ShoppingList
└─ PreparationSteps
```

This is not the same as a DDD aggregate.

But the same abstract principle appears:

> **Find coherent boundaries so complexity can be reasoned about locally.**

---

# 91. 🔄 The Full Product Loop

```text
REAL WORLD
↓
DOMAIN MODEL
↓
BACKEND STATE
↓
API REPRESENTATION
↓
FRONTEND STATE
↓
REACT PROJECTION
↓
HUMAN PERCEPTION
↓
HUMAN ACTION
↓
REACT EVENT
↓
APPLICATION REQUEST
↓
DOMAIN TRANSITION
↓
NEW SYSTEM STATE
↓
NEW UI
```

Software is a loop.

Not a collection of unrelated technologies.

---

# 92. 🧪 Testing Samples the Loop at Different Points

Backend unit test:

```text
Does the domain transition preserve its invariant?
```

API test:

```text
Is the transition exposed correctly?
```

Frontend component test:

```text
Does the UI represent the correct state?
```

Frontend integration test:

```text
Does interaction cross the API boundary correctly?
```

E2E:

```text
Does the whole human → system → human loop work?
```

That is the complete testing story.

---

# 93. 🧭 The Dot-Connecting Curriculum

```text
PRODUCT INTENT
      ↓
DDD
      ↓
DOMAIN MODEL
      ↓
BACKEND TDD
      ↓
APPLICATION + API
      ↓
FRONTEND STATE MODEL
      ↓
REACT
      ↓
FRONTEND TESTING
      ↓
E2E
      ↓
CI/CD
```

Or:

```text
DDD
→ what does the product mean?

Backend
→ how is that meaning enforced?

API
→ how does that meaning cross boundaries?

React
→ how is that meaning projected to a human?

Frontend Testing
→ can the human perceive and manipulate it correctly?

E2E
→ does the whole causal loop survive assembly?
```

---

# 94. ✅ Frontend Testing Checklist

### 🎯 User Behaviour

- What should the user be able to perceive?
- What should they be able to do?
- What should happen afterwards?

### 🧠 State

- What states exist?
- What transitions exist?
- Are impossible states representable?

### ⚛️ Rendering

- Does each meaningful state create the correct visible interface?
- Are tests focused on behaviour instead of internals?

### 👤 Interaction

- Are tests interacting the way a user would?
- Can controls be found by meaningful roles or labels?

### ⏳ Async

- Is loading tested?
- Is success tested?
- Is failure tested?
- Is empty distinct from failure?

### 🌐 API Boundary

- What response shapes does the UI depend on?
- Are success and failure responses controlled in integration tests?
- Is domain logic being duplicated unnecessarily?

### 🚨 Failure

- Does the user get useful feedback?
- Can they recover where appropriate?

### ♿ Accessibility

- Are labels and roles meaningful?
- Can important interactions work beyond mouse-only use?

### 🧪 Test Boundary

- Is this best proven with pure logic, component, integration or E2E testing?
- Are we using the cheapest layer that provides enough confidence?

### 🔄 Refactoring

- Would the test survive an internal refactor if user behaviour remained unchanged?

---

# 95. 🌳 The Deeper Model

Frontend testing is not fundamentally about React.

The deeper problem is:

```text
A system contains state.

A human cannot see state directly.

We construct a representation.

The human acts on that representation.

The action changes the system.

The representation changes again.
```

So:

```text
STATE
  ↓
REPRESENTATION
  ↓
PERCEPTION
  ↓
ACTION
  ↓
TRANSITION
  ↓
STATE
```

Frontend testing asks:

> **Does this loop preserve meaning?**

---

# 96. 🧬 Final Compression

The backend story was:

```text
Reality
↓
Domain
↓
Rules
↓
Code
↓
Tests
```

The frontend story continues:

```text
Domain / Server State
↓
Frontend State
↓
React
↓
Visible Interface
↓
Human Interaction
↓
State Transition
↓
New Interface
```

Testing wraps around the loop:

```text
GIVEN
the user can perceive state X

WHEN
the user performs action Y

THEN
the system reaches state Z

AND
the interface communicates Z correctly
```

So the full connection is:

```text
DDD
= preserve meaning inside the software

BACKEND TDD
= prove the rules that protect that meaning

REST / DTOs
= carry that meaning across the system boundary

REACT
= project that meaning into human experience

FRONTEND TESTING
= prove that the projection and interaction remain correct

E2E
= prove the complete human ↔ system loop
```

> **A frontend test is not really testing JSX.  
> It is testing whether the software still makes the correct promise to the human using it.**
