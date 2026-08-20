# 📚 The Story of Technical Documentation

## How to Write Documentation That Actually Transfers Understanding

---

# 🎯 1. What Documentation Is Really For

Most weak documentation treats documentation as **notes about code**.

That is too small.

Documentation exists because the person reading the system does **not** begin with the same internal model as the person who built it.

The author knows:

- why the feature exists;
- how the pieces connect;
- which files matter;
- which decisions were deliberate;
- what assumptions were made;
- what must be configured;
- what will break;
- what “working” looks like.

The reader does not.

So documentation has one primary job:

> **Move the reader from insufficient context to sufficient context for correct action.**

That gives us a useful model:

```text
UNKNOWN SYSTEM
      ↓
CONTEXT
      ↓
STRUCTURE
      ↓
MECHANISM
      ↓
ACTION
      ↓
VERIFICATION
      ↓
UNDERSTANDING
```

Good documentation is therefore not simply information.

It is **controlled context transfer**.

---

# 🧠 2. The Documentation Invariant

Across setup guides, architecture docs, feature docs, troubleshooting guides and PR descriptions, one thing remains true:

> **The reader should finish the document able to do or understand something they could not reliably do or understand before.**

That is the invariant.

If the reader finishes with more words but not more capability, the document has failed.

---

# 🌫️ 3. Documentation as Ambiguity Reduction

Before documentation, the reader may have questions like:

```text
Where does this feature live?
What starts first?
Which command do I run?
Which environment variables are required?
Which endpoint is being called?
Who owns this behaviour?
What is expected to happen?
How do I know it worked?
What do I do if it fails?
```

A strong document progressively collapses those uncertainties.

```text
ambiguity
   ↓
orientation
   ↓
structure
   ↓
constraints
   ↓
steps
   ↓
observable result
```

This is why good documentation feels **clear before it feels detailed**.

---

# 👤 4. Start With the Reader, Not the Author

The author naturally thinks in terms of:

```text
what I built
```

The reader needs:

```text
what I need to know next
```

Those are not the same thing.

Before writing, identify the reader.

Examples:

- 👨‍💻 another developer;
- 🌱 a new team member;
- 🔍 a PR reviewer;
- 🧪 a tester;
- 🛠️ a maintainer;
- 📋 a product owner;
- 🧠 your future self.

Then ask:

> **What is the minimum model this person needs in order to act correctly?**

That question should shape the document.

---

# 🧭 5. Every Document Needs a Destination

A document should answer one central question.

Examples:

| Document | Core Question |
|---|---|
| 🛠️ Setup Guide | How do I get this running? |
| 🏗️ Architecture Guide | How is this system organised? |
| ✨ Feature Guide | How does this feature work end to end? |
| 🔌 API Guide | How do I interact with this boundary? |
| 🚨 Troubleshooting Guide | How do I move from failure back to working? |
| ⚖️ Decision Record | Why did we choose this approach? |
| 🔀 PR Description | What changed, why, how do I run it, and what should I review? |

If you cannot state the core question in one sentence, the document probably does not yet have a clear shape.

---

# 🧱 6. The General Shape of a Strong Technical Document

Most useful technical documentation follows some version of this descent:

```text
1. Context
2. Outcome
3. Mental model
4. Structure
5. Steps
6. Verification
7. Failure states
8. Summary
```

Or visually:

```text
WHY
 ↓
WHAT
 ↓
WHERE
 ↓
HOW
 ↓
PROVE
 ↓
RECOVER
```

This order matters.

A reader should understand the **shape of the problem** before being asked to manipulate the implementation.

---

# 🎬 7. Begin With Context

Weak documentation often starts like this:

```bash
npm install
npm run dev
```

The commands may be correct, but the reader has no orientation.

A better opening is:

```md
# 📱 React Native Setup

This guide adds the mobile client to the existing application.

The backend remains the Java Spring Boot application.

React Native is responsible for the mobile UI and communicates with Spring Boot through REST/JSON and multipart image upload.

By the end of this guide you should be able to:

- run the mobile client;
- connect it to the backend;
- open the first screen;
- submit a test request successfully.
```

Now the reader knows:

```text
what is being changed
why it exists
where it sits
what success looks like
```

---

# 🗺️ 8. Give the Reader a Mental Model Before the Details

Humans understand steps better when they can place them inside a larger structure.

For example:

```text
React Native
    ↓
HTTP / JSON / multipart
    ↓
Spring Boot Controller
    ↓
Service
    ↓
Repository
    ↓
PostgreSQL
```

Now a later command such as:

```bash
npx expo start
```

has somewhere to live mentally.

Without the model, it is merely a command.

With the model, it is part of a system.

---

# 🧩 9. Document Relationships, Not Just Objects

Weak:

```text
There is a controller.
There is a service.
There is a repository.
```

Stronger:

```text
The controller receives the HTTP request.

It passes application work to the service.

The service coordinates the use case.

The repository persists or retrieves the required state.
```

Strongest:

```text
HTTP Request
   ↓
Controller      ← HTTP boundary
   ↓
Service         ← application decision boundary
   ↓
Repository      ← persistence boundary
   ↓
Database
```

The system is not a list of files.

It is a network of responsibilities.

Documentation should expose that network.

---

# 🏁 10. Define Success Before Giving Instructions

Before the reader starts, show them the destination.

Example:

```text
JDK installed
→ Spring Boot starts
→ PostgreSQL connects
→ Flyway runs
→ tables appear
→ health request succeeds
```

This creates an implicit diagnostic tree.

If the reader reaches:

```text
Spring Boot starts
```

but not:

```text
PostgreSQL connects
```

they immediately know where the failure sits.

---

# 👣 11. Write Instructions as Observable Actions

Instructions should be executable.

❌ Weak:

```text
Configure the database.
```

✅ Better:

```properties
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
```

✅ Better still:

```text
Add the following properties to `src/main/resources/application.properties`:
```

```properties
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
```

Then verify:

```bash
echo $DB_URL
```

The reader now has:

```text
action
+
location
+
expected evidence
```

---

# 💻 12. Separate Commands From Meaning

Commands should be visually distinct.

```bash
git checkout main
git pull origin main
git checkout -b feature/image-upload
```

Then explain the meaning:

> This creates the new feature branch from the latest canonical version of `main`.

Do not force the reader to extract commands from prose.

---

# 🔍 13. Explain Decisions, Not Obvious Syntax

Documentation should spend words where uncertainty is highest.

Useful explanation:

```text
We use Flyway to own schema evolution.

Hibernate validates the schema rather than creating it so database changes remain explicit and versioned.
```

Low-value explanation:

```text
The `git pull` command pulls code.
```

The rule:

> **Explain what is surprising, consequential, structural or easy to misuse.**

---

# 🧪 14. Verification Is Part of the Documentation

A setup step without verification is incomplete.

For every important action, ask:

> **How can the reader prove this worked?**

Examples:

### Git branch

```bash
git branch
```

Expected:

```text
* feature/image-upload
  main
```

### Backend

```bash
curl http://localhost:8080/actuator/health
```

Expected shape:

```json
{
  "status": "UP"
}
```

### Database

```sql
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public';
```

### Frontend

```text
The application opens without a red error screen and the first API request completes successfully.
```

Verification converts instruction into evidence.

---

# 🚨 15. Failure States Belong in Good Documentation

A good document does not pretend the happy path is the only path.

Use this pattern:

```text
SYMPTOM
  ↓
LIKELY LAYER
  ↓
CHECK
  ↓
FIX
```

Example:

```md
## 🚨 Backend returns connection refused

This usually means the frontend can reach the configured host, but nothing is listening on the expected backend port.

Check:

```bash
curl http://localhost:8080
```

Then confirm:

- Spring Boot is running;
- the port is correct;
- the mobile client is not using `localhost` incorrectly when running on a physical device.
```

The goal is not to document every possible error.

Document the failures that are:

- common;
- expensive;
- confusing;
- specific to this system.

---

# 🧪 16. Examples Are Compression

Examples are not decoration.

A good example can replace paragraphs of abstraction.

Instead of:

> The API should return structured ingredient recognition data.

Show:

```json
{
  "ingredients": [
    {
      "name": "chicken breast",
      "confidence": 0.94
    }
  ]
}
```

Now the reader immediately understands the expected shape.

---

# 🏷️ 17. Terminology Must Stay Stable

If one concept is called `Recipe`, keep calling it `Recipe`.

Do not casually rotate through:

```text
Recipe
Dish
Meal Object
Food Result
Recipe Item
```

unless those are genuinely distinct domain concepts.

Every unnecessary synonym creates additional interpretation work.

This applies to:

- classes;
- endpoints;
- database tables;
- services;
- domain concepts;
- branch names;
- feature names.

---

# 🗂️ 18. The Main Types of Technical Documentation

## 🛠️ Setup Guide

Purpose:

> **Take a machine from “not ready” to “working.”**

A strong setup guide normally contains:

```text
Prerequisites
→ installation
→ configuration
→ environment variables
→ startup order
→ commands
→ verification
→ common failures
```

---

## 🏗️ Architecture Guide

Purpose:

> **Give the reader a correct model of the system.**

It should explain:

- major components;
- boundaries;
- responsibilities;
- dependencies;
- data flow;
- persistence;
- external systems;
- important constraints.

Example:

```text
Mobile Client
    ↓
REST API
    ↓
Application Layer
    ↓
Domain
    ↓
Persistence
```

---

## ✨ Feature Guide

Purpose:

> **Explain one capability from user action to system result.**

A strong feature guide usually follows:

```text
User intent
→ frontend interaction
→ API request
→ application behaviour
→ persistence / external dependency
→ response
→ UI state
```

---

## 🔌 API Guide

Purpose:

> **Make an external boundary usable without reading the implementation.**

Include:

- endpoint;
- HTTP method;
- request body;
- response body;
- validation;
- status codes;
- authentication if relevant;
- failure responses;
- example request.

---

## 🚨 Troubleshooting Guide

Purpose:

> **Turn symptoms into diagnostic movement.**

Structure:

```text
symptom
→ likely cause
→ diagnostic check
→ fix
→ verification
```

---

## ⚖️ Decision Record

Purpose:

> **Preserve why a decision was made.**

Template:

```md
# ⚖️ Decision: <Title>

## Context

What problem required a decision?

## Options Considered

What credible alternatives existed?

## Decision

What did we choose?

## Why

Why was this the best choice under the current constraints?

## Consequences

What becomes easier, harder or newly constrained because of this?
```

---

# 🔀 19. Pull Request Descriptions Are Documentation

A PR description is not administrative text.

It is a **review interface**.

The code diff answers:

```text
What lines changed?
```

The PR description should answer:

```text
Why do these changes exist?
What behaviour changed?
How does this fit the system?
How can I run it?
How can I prove it works?
What should I inspect carefully?
What is deliberately not included?
```

That is a completely different job.

---

# 🧠 20. The PR Description Invariant

A reviewer should be able to move from:

```text
I have never seen this branch
```

to:

```text
I understand the purpose
I can run it locally
I can exercise the change
I know what evidence exists
I know what to review
```

without messaging the author for basic context.

That is the standard.

---

# 🧱 21. The Shape of a Strong PR Description

A strong PR description normally contains:

```text
Summary
↓
Why
↓
What Changed
↓
Architecture / Important Decisions
↓
Local Setup
↓
How to Run
↓
How to Test
↓
Evidence
↓
Out of Scope
↓
Review Focus
```

Not every PR needs every section.

But **Local Setup / How to Run** should be present whenever the reviewer needs anything beyond the normal project baseline.

---

# 📋 22. A Strong General PR Template

```md
# 🔀 PR: <Short Descriptive Title>

## 🎯 Summary

What does this PR add, remove or change?

Keep this behavioural rather than file-by-file.

## 💡 Why

Why is this change needed?

Link it back to the ticket, bug, user story or architectural requirement.

## 🔧 What Changed

- Change 1
- Change 2
- Change 3

Focus on meaningful system changes.

## 🏗️ Architecture / Decisions

Describe any important boundaries, trade-offs or implementation decisions.

Include this section only when it adds useful review context.

## 🖥️ Local Setup

Explain everything another developer needs to reproduce this change locally.

### Prerequisites

- required runtime / SDK versions;
- required database;
- required external services;
- required package manager;
- required tooling.

### Environment Variables

```env
EXAMPLE_API_KEY=
DB_URL=
DB_USERNAME=
DB_PASSWORD=
```

Do not include real secrets.

### Install / Update Dependencies

```bash
<dependency installation commands>
```

### Database / Migrations

```bash
<commands required to create / migrate / seed the database>
```

### Additional Setup

Document anything unusual such as:

- emulator configuration;
- local storage;
- seeded users;
- fixtures;
- feature flags;
- external service mocks;
- local certificates.

## ▶️ How to Run

Backend:

```bash
<backend command>
```

Frontend:

```bash
<frontend command>
```

Any required startup order should be explicit.

## 🧪 How to Test

Give the reviewer an exact path through the change.

Example:

1. Start the backend.
2. Start the frontend.
3. Open the relevant screen.
4. Perform the changed action.
5. Observe the expected result.

Automated tests:

```bash
<test command>
```

## ✅ Expected Result

Describe what success looks like.

Example:

```text
Submitting a valid image returns HTTP 200 and displays the detected ingredients.
```

## 📸 Evidence

Add where useful:

- screenshots;
- API responses;
- test output;
- logs;
- before / after behaviour.

## 🚧 Out of Scope

List behaviour intentionally not included in this PR.

## 👀 Review Focus

Tell the reviewer where careful attention is most valuable.
```

---

# 🖥️ 23. Why “Local Setup” Belongs in a PR

This section is often missing, and that creates friction.

A PR may be logically correct but still be difficult to review because the reviewer cannot reproduce it.

For example, the author may already have:

```text
database created
environment variables configured
migration applied
dependencies installed
test account seeded
backend running
```

The reviewer may have none of those things.

Without setup instructions, the reviewer experiences:

```text
code
→ attempt to run
→ failure
→ guess
→ message author
→ wait
→ recover context
```

With setup instructions:

```text
PR
→ setup
→ run
→ test
→ review
```

That is a dramatic reduction in coordination cost.

---

# 🧰 24. What “Set It Up Locally” Actually Means

A useful local setup section answers these questions:

```text
What version do I need?
What do I need installed?
What environment variables exist?
What database must exist?
What migrations must run?
What seed data do I need?
What services must be running?
Which command starts each component?
Which URL should I open?
Which test account / input should I use?
```

Do not write:

```text
Run locally as normal.
```

unless the PR genuinely introduces no new local requirements.

---

# 🧪 25. Example: PR With Proper Local Setup

```md
# 🔀 PR: Add Fridge Image Upload

## 🎯 Summary

Adds the first end-to-end image-upload path.

The mobile client can select an image and send it to the Spring Boot backend as multipart form data.

## 💡 Why

Image upload is the first technical boundary required for the fridge-recognition flow.

The recognition feature cannot exist until the application can reliably receive an image.

## 🔧 What Changed

- Added the mobile image-selection flow.
- Added multipart request handling.
- Added the backend upload controller.
- Added validation for missing image data.
- Added a response DTO.
- Added tests for the upload boundary.

## 🏗️ Architecture / Decisions

The controller owns the HTTP/multipart concern.

The service owns application orchestration.

Image storage remains behind a service boundary so storage implementation can change without changing the controller.

## 🖥️ Local Setup

### Prerequisites

- Java 25
- Maven
- Node.js
- Expo / React Native tooling
- PostgreSQL

### Environment Variables

Backend:

```env
DB_URL=jdbc:postgresql://localhost:5432/restaurant_recipes
DB_USERNAME=postgres
DB_PASSWORD=<your-local-password>
```

Mobile client:

```env
EXPO_PUBLIC_API_URL=http://<your-machine-ip>:8080
```

Use your machine's LAN IP when testing on a physical phone.

### Database

Create the local database:

```sql
CREATE DATABASE restaurant_recipes;
```

Start the backend once so Flyway can apply migrations.

### Install Dependencies

Backend:

```bash
./mvnw clean install
```

Mobile:

```bash
npm install
```

## ▶️ How to Run

Start the backend:

```bash
./mvnw spring-boot:run
```

Then start the mobile client:

```bash
npx expo start
```

## 🧪 How to Test

1. Open the mobile application.
2. Navigate to the fridge-image screen.
3. Select a test image.
4. Submit the image.
5. Confirm the request reaches the backend.
6. Confirm the UI displays the successful response.

Automated backend tests:

```bash
./mvnw test
```

## ✅ Expected Result

A valid image is accepted and returns a successful structured response.

A missing image returns the expected validation error.

## 🚧 Out of Scope

- AI ingredient recognition;
- cloud image storage;
- recipe matching.

## 👀 Review Focus

Please check:

- multipart request handling;
- controller/service separation;
- validation behaviour;
- whether local mobile-to-backend connectivity is documented clearly.
```

Notice what the reviewer now has:

```text
purpose
+
architecture
+
setup
+
commands
+
test path
+
expected result
```

That PR can actually be reviewed independently.

---

# 🔍 26. Describe Behaviour, Not the Diff

Weak:

```text
Added ImageController.java.
Added ImageService.java.
Changed package.json.
Added test.
```

The reviewer can already see that.

Better:

```text
Introduces the image-upload boundary between the mobile client and Spring Boot.

The controller handles multipart transport while the service owns the upload workflow.
```

The PR description should provide **meaning the diff cannot provide by itself**.

---

# 🎫 27. Connect the PR to the Ticket

A useful model is:

```text
TICKET
= promise

CODE
= implementation

TESTS
= evidence

PR
= explanation of implementation + evidence
```

So the PR should make it easy to answer:

```text
Did this implementation actually fulfil the promise?
```

---

# 📦 28. Keep the PR Coherent

A PR should normally have one understandable purpose.

✅ Good:

```text
Add fridge image upload
```

❌ Weak:

```text
Add image upload
Refactor authentication
Rename recipes
Change Docker
Update database
Fix navbar
```

A coherent PR is easier to:

- understand;
- test;
- review;
- merge;
- revert;
- reason about later.

---

# 🚧 29. State What Is Deliberately Not Included

A reviewer cannot reliably distinguish:

```text
missing by mistake
```

from:

```text
intentionally deferred
```

unless you tell them.

Use:

```md
## 🚧 Out of Scope

- AI ingredient recognition
- cloud storage
- recipe matching
```

This protects the boundary of the PR.

---

# 👀 30. Tell the Reviewer Where to Look

A PR review is a limited attention process.

Direct that attention.

Example:

```md
## 👀 Review Focus

Please pay particular attention to:

- whether the service boundary is appropriate;
- request validation;
- error handling;
- whether the migration is reversible;
- whether the local setup steps are complete.
```

This improves the quality of review.

---

# 📸 31. Evidence Makes Review Faster

Where useful, include evidence.

Examples:

### UI

```text
Before
→ screenshot

After
→ screenshot
```

### API

```json
{
  "status": "success",
  "imageId": 42
}
```

### Tests

```text
Tests run: 18
Failures: 0
Errors: 0
```

### Database

```text
Flyway migration V3 applied successfully.
```

Evidence lowers the amount of trust the reviewer must supply.

---

# ❌ 32. Weak PR Description Patterns

Avoid:

```text
Done
```

```text
Feature added
```

```text
Please review
```

```text
Ticket complete
```

```text
Works on my machine
```

```text
Run it normally
```

Each one pushes context reconstruction onto the reviewer.

---

# 🧠 33. Documentation Should Match the Level of the Change

Not every change needs a 500-line document.

The principle is not:

> write more.

The principle is:

> **remove enough ambiguity for the reader to act correctly.**

A typo fix may need:

```text
Summary
Testing
```

A new service boundary may need:

```text
Context
Architecture
Setup
Run
Test
Failure States
Review Focus
```

The amount of documentation should track the **context burden** of the change.

---

# 🔄 34. Documentation Has a Lifecycle

Documentation can be correct when written and wrong later.

So documentation should be treated as part of the system.

When the system changes, ask:

```text
Did a command change?
Did a file move?
Did an endpoint change?
Did configuration change?
Did ownership move?
Did an assumption stop being true?
```

If yes, the documentation may need to change too.

> **Outdated documentation is not neutral. It actively teaches the wrong system.**

---

# 🧹 35. Delete Dead Documentation

Teams often keep obsolete documentation because deleting it feels destructive.

That is backwards.

If a document confidently describes a system that no longer exists, it is producing noise.

Prefer:

```text
one current source of truth
```

over:

```text
five partially correct guides
```

---

# 🧭 36. Documentation and Source of Truth

Different truths should live in appropriate places.

Example:

```text
Code
→ executable behaviour

Migration files
→ database history

API schema
→ API contract

README / setup guide
→ developer entry point

Architecture docs
→ system model

PR
→ explanation of one change

Ticket
→ desired outcome / promise
```

Do not make one document carry every kind of truth.

---

# 🧪 37. A Documentation Review Exercise

When reviewing a document, do not ask:

> Does this look detailed?

Ask:

### Orientation

Can I tell what problem this document solves?

### Structure

Can I see how the parts relate?

### Action

Can I follow the instructions without guessing?

### Verification

Can I prove each important stage worked?

### Recovery

If something fails, do I know where to look?

### Currency

Does this describe the system that exists now?

### Independence

Could another developer use this without asking the author basic questions?

If the answer to the final question is no, the document is probably incomplete.

---

# ✅ 38. Documentation Definition of Done

A technical document is ready when:

- 🎯 its purpose is obvious;
- 👤 its reader is clear;
- 🧠 it gives the necessary mental model;
- 🗺️ it shows where the subject fits in the system;
- 👣 its instructions are executable;
- 💻 commands are copyable;
- ✅ success is observable;
- 🚨 important failure states are covered;
- 🏷️ terminology is consistent;
- 🔗 paths, endpoints and names are real;
- 🧪 examples are concrete;
- 🔄 it reflects the current system;
- ⚡ unnecessary explanation has been removed.

For a PR description specifically:

- 🎫 it connects back to the intended change;
- 💡 it explains why the change exists;
- 🔧 it describes meaningful behaviour;
- 🖥️ it explains how to set the change up locally;
- ▶️ it explains how to run it;
- 🧪 it explains how to test it;
- ✅ it states the expected result;
- 🚧 it states what is out of scope;
- 👀 it directs reviewer attention where useful.

---

# 🌳 39. The Deeper Model

The author begins with a dense internal representation of the system.

The reader begins with a sparse one.

Documentation performs a transformation:

```text
AUTHOR'S INTERNAL MODEL
        ↓
     COMPRESS
        ↓
   DOCUMENTATION
        ↓
     RECONSTRUCT
        ↓
READER'S WORKING MODEL
```

Poor documentation loses the important structure during compression.

Good documentation preserves:

```text
causality
relationships
constraints
sequence
verification
```

while discarding irrelevant detail.

That is why documentation is a technical skill.

It is not typing.

It is **model transfer**.

---

# 🧬 40. Final Compression

Good documentation moves the reader through this chain:

```text
CONTEXT
   ↓
STRUCTURE
   ↓
MECHANISM
   ↓
ACTION
   ↓
EVIDENCE
   ↓
UNDERSTANDING
```

And good PR documentation moves the reviewer through this chain:

```text
WHY
 ↓
WHAT CHANGED
 ↓
HOW IT FITS
 ↓
HOW TO SET IT UP
 ↓
HOW TO RUN IT
 ↓
HOW TO TEST IT
 ↓
WHAT TO REVIEW
```

The goal is not maximum detail.

The goal is:

> **Minimum ambiguity, maximum usable understanding.**
