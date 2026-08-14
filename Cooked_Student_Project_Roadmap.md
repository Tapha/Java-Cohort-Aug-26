# Cooked — Student Project Roadmap 🍳

> **Build the product vertically. Add technology only when the product creates a reason for it.**

## Target Loop

**Fridge → Ingredients → Preferences → Dish → Missing Ingredients → Recipe → Cooking**

Each phase should leave the application more complete than before. Do not build isolated technology demos. Build the next piece of the product, test it, review it, merge it, then continue.

---

## Phase 1 — Project Setup

- Clone the shared repository.
- Run the Java / Spring Boot backend.
- Run the React frontend.
- Connect to local PostgreSQL.
- Run database migrations.
- Review the core database relationships.
- Confirm the frontend can call the backend.

**Milestone:** React can call Java and retrieve data.

**Focus:** Git, Maven, Spring Boot, PostgreSQL, REST basics.

---

## Phase 2 — Core Backend

Implement:

- Users
- Fridges
- Food Items
- Recipes
- Recipe Ingredients
- Preparation Steps
- Dietary Preferences

For each feature, follow:

**Entity → Repository → Service → Controller → DTO**

Also:

- add request validation;
- add useful exception handling;
- write JUnit tests;
- use Mockito where appropriate.

**Milestone:** manually entered ingredients can be stored and recipes can be retrieved.

**Focus:** Java OOP, Spring layers, JPA, DTOs, validation, exceptions, JUnit and Mockito.

---

## Phase 3 — React Core UI

Build:

- cuisine selection;
- dietary preference selection;
- ingredient-list screen;
- recipe-results cards;
- recipe-detail screen;
- REST/JSON communication with the backend.

Learn and use:

- components;
- props;
- state;
- forms;
- API calls;
- conditional rendering.

**Milestone:** a user can move through the basic journey using seeded data.

---

## Phase 4 — Fridge Image Flow

Build:

- fridge image capture/upload in React;
- backend multipart upload endpoint;
- uploading and processing states;
- AI ingredient-recognition flow;
- structured ingredient JSON;
- detected ingredient display;
- add/remove/edit ingredient controls;
- persistence of confirmed fridge state.

The AI output is only a suggestion. The user's corrected list becomes authoritative.

**Milestone:** a fridge image becomes an editable ingredient list.

---

## Phase 5 — Recipe Matching

Input:

- confirmed ingredients;
- cuisine;
- dietary preferences.

Build:

- candidate recipe retrieval;
- dietary filtering;
- fridge ↔ recipe ingredient comparison;
- missing-ingredient calculation;
- candidate ranking;
- React results display.

### Scala Component

Add a small Scala compatibility module.

Scala receives:

- available ingredients;
- required recipe ingredients.

Scala returns:

- match count;
- missing ingredients;
- required count;
- compatibility percentage.

Example:

```text
Available:
chicken, garlic, onion, rice

Recipe:
chicken, garlic, rice, yoghurt, paprika

Result:
3 / 5 ingredients available
60% match
2 ingredients missing
```

Java remains responsible for orchestration.

**Milestone:** the user receives ranked recipes based on what they actually have.

---

## Phase 6 — Complete Product Loop

Build:

- full recipe view;
- `You have` vs `You need`;
- shopping-list generation;
- ordered preparation steps;
- Cooking Mode;
- basic cooking-progress persistence;
- saved recipes;
- simple timers if the core flow is already stable.

**Milestone:**

**Fridge → Ingredients → Recipe → Shopping List → Cooking**

works end-to-end.

This is the first true MVP.

---

## Phase 7 — Restaurant Context

Add:

- restaurants;
- restaurant cuisines;
- recipe ↔ restaurant relationships;
- small seeded restaurant/menu dataset;
- provenance;
- `Verified` vs `Restaurant-inspired` labels.

Optional later:

- location-aware filtering.

Restaurant-data failure must not break the core recipe journey.

**Milestone:** restaurant context improves discovery without controlling the product.

---

## Phase 8 — AWS Integration

### S3

Store fridge images.

### Lambda

Use Lambda for the image / AI-processing seam.

Example:

```text
Image uploaded
    ↓
S3
    ↓
Lambda
    ↓
AI ingredient recognition
    ↓
Structured ingredient result
```

### Aurora

Move deployed PostgreSQL persistence to Aurora PostgreSQL.

### IAM

Use least-privilege permissions.

### CloudWatch

Capture:

- application logs;
- Lambda logs;
- failures;
- useful operational information.

**Milestone:** the working product now uses real AWS services.

---

## Phase 9 — Quality + CI/CD

Expand testing:

- JUnit unit tests;
- Mockito service tests;
- controller/API tests;
- repository/integration tests;
- Scala compatibility tests;
- code coverage.

Create a Jenkins pipeline:

```text
Checkout
   ↓
Build
   ↓
Test
   ↓
Package
   ↓
Docker Build
```

Tests must pass before deployment continues.

**Milestone:** every push can produce a tested, deployable artifact.

---

## Phase 10 — Docker + Kubernetes

### Docker

Containerise:

- Java backend;
- React frontend.

PostgreSQL may also run through Docker locally.

### Kubernetes

Create:

- Deployments;
- Services;
- ConfigMaps;
- Secrets;
- health/readiness checks;
- resource configuration.

Connect deployment into the CI/CD flow.

**Milestone:** Cooked can be built, tested and deployed through the full pipeline.

---

# Student Working Cycle

Every ticket follows the same loop:

1. Pick one clearly scoped feature.
2. Branch from `main`.
3. Implement the feature.
4. Write or update its tests.
5. Run the application locally.
6. Commit.
7. Push.
8. Open a pull request.
9. Review the PR as a group.
10. Merge once the feature is understood and working.
11. Pull the latest `main`.
12. Take the next ticket.

---

# Project Rules

- Prefer a complete narrow feature over a broad half-built feature.
- Do not add technology just to say it was used.
- Keep **Java** as the main backend.
- Keep **React** as the main frontend.
- Keep **Scala** narrow and meaningful.
- Treat AI output as proposed structured data, not unquestioned truth.
- Never present AI-generated restaurant information as verified fact.
- Tests belong with the feature, not at the end.
- AWS comes after the application works locally.
- Docker and Kubernetes come after the core application works.
- `main` should always represent the best currently integrated version of Cooked.

---

# Final Definition of Done

> **Cooked is complete when a user can submit a fridge image, confirm detected ingredients, choose cuisine and dietary preferences, receive ranked recipe candidates, see what they have versus what they need, generate a shopping list, and follow the recipe through Cooking Mode — with the application tested, containerised and deployable.**
