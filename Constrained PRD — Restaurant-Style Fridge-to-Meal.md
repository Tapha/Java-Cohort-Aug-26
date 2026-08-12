# Constrained Product Requirements Document

## 1. Product Definition

Build a web application that lets a user photograph the food available in their fridge, correct the detected ingredients, select a cuisine and dietary requirements, and receive restaurant-style dishes they can realistically cook.

The system shows:

- what ingredients the user already has;
- what ingredients are missing;
- how compatible each dish is;
- the recipe;
- ordered cooking instructions;
- a basic shopping list.

The objective is not to build every feature envisioned in the full PRD. The objective is to implement the complete:

**Fridge → Ingredients → Preferences → Dish → Missing Ingredients → Recipe → Cooking**

loop while exercising as much of the programme's technology stack as is technically justified.

---

# 2. Product Invariant

> Given a user's available ingredients and relevant preferences, the system identifies a plausible restaurant-style dish they can realistically make, distinguishes what they already have from what they need, and guides them through cooking it.

Technology choices must serve this invariant rather than alter it.

---

# 3. Technical Constraint

The application should deliberately maximise meaningful exposure to the programme technologies.

## Primary technologies

### Frontend
- React
- JavaScript
- HTML
- CSS
- Fetch/Axios
- JSON
- browser APIs for image/file upload

### Core backend
- Java
- Spring Boot
- REST APIs
- JSON/Jackson
- Maven
- JPA/JDBC where appropriate
- PostgreSQL

### Testing
- JUnit 5
- Mockito
- REST/controller tests
- integration tests
- code coverage

### Database
- PostgreSQL locally
- Amazon Aurora PostgreSQL-compatible database for AWS deployment
- migrations using Flyway or Liquibase

### AWS
- IAM
- VPC/network configuration where required
- S3
- Lambda
- CloudWatch
- Aurora
- optional API Gateway where Lambda needs an HTTP boundary

### Delivery
- Git
- Maven
- Jenkins
- Docker
- Kubernetes

### Additional technologies

#### React
React is the primary UI framework.

It builds upon the programme's JavaScript/browser/API material while allowing the application to develop a realistic component/state architecture.

#### Scala
Scala will be included deliberately but minimally.

It must implement a genuine piece of domain behaviour rather than exist as unused demonstration code.

The preferred use is a small **Recipe Compatibility Engine**.

Input:

- user's confirmed ingredients;
- required recipe ingredients.

Output:

- available ingredient count;
- missing ingredient count;
- compatibility percentage;
- compatibility classification.

Example:

`[chicken, garlic, onion, rice]`
+
`[chicken, garlic, rice, yoghurt, paprika]`

→

`3/5 available`
→
`60% match`
→
`2 missing`

The Scala component should expose a small Java-compatible boundary so the Java application can call it without creating a second distributed service.

This gives Scala a real role while avoiding an unnecessary Scala microservice.

---

# 4. Architecture Constraint

Use a **modular monolith first**.

Do not introduce multiple backend microservices merely to demonstrate technologies.

High-level system:

**React Client**
↓ REST/JSON
**Java Spring Boot Application**
↓
**Domain/Application Layer**
↙ ↓ ↘
PostgreSQL | Scala Matcher | External AI
↓
AWS services where appropriate

Infrastructure:

**S3**
→ image storage

**Lambda**
→ asynchronous image-processing/AI orchestration seam

**Aurora PostgreSQL**
→ persistent relational data

**CloudWatch**
→ application/Lambda operational visibility

**Jenkins**
→ build/test/package pipeline

**Docker**
→ application packaging

**Kubernetes**
→ final container orchestration/deployment exercise

---

# 5. Committed User Journey

## Step 1 — Start

User opens the React application.

User selects:

- cuisine;
- dietary requirements.

---

## Step 2 — Photograph Fridge

User uploads or captures an image.

React sends the image through the application upload flow.

The image is persisted to S3.

The UI enters:

`UPLOADING → PROCESSING`

---

## Step 3 — Ingredient Recognition

The image is passed to an external AI capability.

The system requests structured JSON such as:

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

A Lambda function may perform the asynchronous processing/orchestration around this operation.

The application validates the returned structure.

---

## Step 4 — Ingredient Confirmation

React displays the detected ingredients.

The user can:

- add;
- remove;
- rename;
- confirm

ingredients.

The confirmed list becomes authoritative.

AI recognition itself never becomes unquestioned fridge state.

---

# 6. Recipe Discovery

The Java application receives:

- confirmed ingredients;
- cuisine;
- dietary preferences.

It retrieves appropriate recipe candidates from PostgreSQL and/or requests structured candidate recipes from the AI provider.

Recipes must have structured:

- name;
- cuisine;
- ingredients;
- quantities;
- preparation steps;
- time;
- servings;
- origin/provenance.

Restaurant associations may be included only where supported.

The system must not claim an AI-created association is verified restaurant information.

---

# 7. Scala Compatibility Engine

For each candidate recipe, Java sends the relevant ingredient sets to the Scala module.

Scala performs deterministic matching.

Conceptual calculation:

**available recipe ingredients / required recipe ingredients**

The result should contain:

```text
requiredIngredients
availableIngredients
missingIngredients
matchCount
requiredCount
compatibilityPercentage
```

Example:

**Nando's-style chicken**

8 required  
6 available  
2 missing  
75% compatible

The Java application remains responsible for orchestration.

Scala is responsible only for this narrow domain calculation.

This boundary should stay intentionally small.

---

# 8. Results Experience

React displays a list of candidate dishes.

Each card shows:

- recipe name;
- cuisine;
- restaurant association if available;
- compatibility;
- number of ingredients available;
- number missing;
- estimated cooking time.

Example:

**Butter Chicken**

82% match

✓ 9 ingredients available  
\+ 2 ingredients required

The user selects a recipe.

---

# 9. Recipe View

The recipe screen shows:

### Recipe information

- name;
- description;
- cuisine;
- cooking time;
- servings.

### Ingredients

Split into:

**You have**
- chicken
- garlic
- onion

**You need**
- yoghurt
- garam masala

### Preparation

Ordered preparation steps.

---

# 10. Shopping List

The application deterministically computes:

**RecipeFoodItems − confirmed Fridge FoodItems**

The result becomes the shopping list.

React allows each missing item to be checked.

The MVP does NOT require:

- grocery checkout;
- live supermarket stock;
- live grocery pricing.

The existing full PRD already establishes the shopping list as sufficient for MVP rather than full grocery commerce.

---

# 11. Cooking Mode

React provides a dedicated cooking view.

Display:

- current step;
- total steps;
- instruction;
- previous;
- next;
- completion state.

Example:

**Step 3 of 7**

Heat the pan over medium-high heat.

`Previous` `Next`

Progress may be persisted through the backend.

Timers may be implemented where useful but are not required to complete the first vertical slice.

---

# 12. Java Backend Responsibilities

Java is the principal application language.

The backend should demonstrate:

- OOP;
- interfaces;
- collections;
- streams;
- generics where appropriate;
- exceptions;
- logging;
- JSON serialization;
- REST design;
- controller/service/repository separation;
- database access;
- dependency injection;
- asynchronous/concurrent concepts where useful.

The REST layer should expose coherent resources such as:

```text
/users
/fridges
/ingredients
/images
/recipes
/recommendations
/shopping-list
/cooking-progress
```

Exact endpoint design belongs to the subsequent technical specification.

---

# 13. REST + JSON

React communicates with Java exclusively through REST/JSON except for image multipart upload.

The handbook specifically requires REST principles, HTTP verbs/status codes, JSON processing, endpoint design, validation, exception handling, documentation and API testing, so the product should make those visible rather than hiding all behaviour behind one generic endpoint.

Important API behaviours should demonstrate:

- GET;
- POST;
- PUT/PATCH where appropriate;
- DELETE where appropriate;
- validation;
- status codes;
- global error handling;
- structured error responses.

---

# 14. Database

Use the supplied relational model as the starting schema.

Core persisted concepts include:

- Users
- Fridge
- FoodItems
- Restaurant
- RestaurantCuisine
- Recipe
- RecipeFoodItems
- PreparationSteps
- PreparationStepsProgress
- DietaryPreferences
- SavedRecipes
- Images
- AIGeneratedRecipe

PostgreSQL is the development database.

Aurora PostgreSQL becomes the deployed database.

The programme explicitly covers relational modelling, SQL, PostgreSQL, JDBC/integration, schema optimisation and Amazon Aurora, making this project an appropriate place to exercise them.

---

# 15. AWS Usage

## S3 — Required

Store fridge images.

Flow:

**React → backend/upload mechanism → S3**

Persist the resulting object reference rather than binary image data in PostgreSQL.

---

## Lambda — Required but narrow

Use Lambda for the image-processing/AI seam.

Example:

**S3 image created**
→ Lambda invoked
→ request ingredient recognition
→ structured result returned/stored

This makes serverless behaviour meaningful without migrating the whole backend to Lambda.

---

## IAM — Required

Use least-privilege permissions for:

- S3;
- Lambda;
- database/application AWS access.

---

## CloudWatch — Required

Capture:

- Lambda logs;
- processing errors;
- request failures;
- useful operational metrics.

---

## Aurora — Required for deployed environment

Production/demo persistence uses Aurora PostgreSQL-compatible infrastructure.

Local development can use standard PostgreSQL.

---

# 16. Testing Constraint

Testing is part of the product definition of done.

Use JUnit 5 and Mockito.

Tests should cover:

### Unit

- ingredient comparison;
- recipe matching orchestration;
- dietary filtering;
- shopping-list calculation;
- service behaviour.

### API

- controller responses;
- validation;
- status codes;
- malformed requests.

### Integration

- repository/database behaviour;
- principal end-to-end backend flow.

### Scala

Compatibility-engine inputs and outputs must be unit-tested independently.

---

# 17. Jenkins Pipeline

A Jenkins pipeline must demonstrate:

**Checkout**
→ **Build**
→ **Unit Tests**
→ **Integration Tests**
→ **Package**
→ **Docker Build**
→ **Deployment stage**

A failed test prevents deployment.

Code coverage should be surfaced in CI.

---

# 18. Docker

Package the application as containers.

Minimum:

- Java backend container;
- React application/container where deployed independently.

PostgreSQL may be containerised for local development.

External managed AWS services remain external.

---

# 19. Kubernetes

Kubernetes is deliberately a **deployment concern**, not an application-design concern.

The final application should provide manifests/configuration for:

- frontend deployment;
- backend deployment;
- services;
- configuration;
- secrets;
- health checks.

Database and S3 remain managed externally.

Kubernetes should demonstrate deployment/orchestration skills without breaking the application into artificial microservices.

---

# 20. Feature Classification

## COMMIT

- React frontend
- cuisine selection
- dietary preferences
- fridge image upload
- S3 image storage
- AI ingredient recognition
- ingredient confirmation/editing
- Java REST API
- recipe candidates
- Scala compatibility calculation
- available vs missing ingredients
- structured recipe
- shopping list
- cooking steps
- PostgreSQL/Aurora persistence
- JUnit/Mockito
- CloudWatch
- Lambda
- Jenkins
- Docker
- Kubernetes deployment

## SIMPLIFY

### Restaurant discovery

Use seeded/stored restaurant data.

Do not require comprehensive live restaurant discovery.

### Restaurant menus

Use a small known dataset or reliable external data where available.

### Location

Persist basic coordinates if needed but do not make geolocation essential to the primary loop.

### Grocery stores

Retain the domain entities but do not require live inventory integration.

### Cooking progress

Persist basic current-step progress only.

---

## APPROXIMATE

Restaurant-style association may be represented as:

- verified restaurant association where data exists; or
- clearly labelled restaurant-inspired recipe.

Never blur these categories.

---

## DEFER

- live grocery inventory;
- supermarket price comparison;
- payments;
- ordering;
- ratings;
- reviews;
- social features;
- recommendation engine;
- sponsored recipes;
- nutrition platform;
- video instruction platform;
- live restaurant menu crawling;
- comprehensive location search.

---

# 21. Failure States

## Image recognition failure

Allow manual ingredient entry.

## AI unavailable

Use stored recipes where available or return a recoverable error.

## Invalid AI JSON

Reject the response and retry/fallback.

## No compatible recipe

Explain that no strong match exists and allow:

- cuisine change;
- ingredient modification;
- broader matching.

## Restaurant data unavailable

Return restaurant-style/cuisine-based recipes without claiming verified restaurant association.

## S3 upload failure

Image remains unconfirmed and the user can retry.

## Database unavailable

Return explicit service failure rather than silently losing state.

---

# 22. Primary Data Flow

```text
React
  ↓
Cuisine + Dietary Selection
  ↓
Fridge Image
  ↓
S3
  ↓
Lambda / AI
  ↓
Detected Ingredient JSON
  ↓
React Correction
  ↓
Java REST API
  ↓
PostgreSQL/Aurora
  ↓
Candidate Recipes
  ↓
Scala Compatibility Engine
  ↓
Ranked Candidates
  ↓
Recipe Selection
  ↓
Available vs Missing Ingredients
  ↓
Shopping List
  ↓
Preparation Steps
  ↓
Cooking Complete
```

---

# 23. Acceptance Criteria

The constrained application is complete when:

1. React can submit a fridge image.
2. The image can be stored in S3.
3. Ingredient-recognition processing produces structured JSON.
4. The user can correct the detected ingredients.
5. Confirmed ingredients are persisted through the Java REST API.
6. Cuisine and dietary requirements affect recipe selection.
7. At least one structured recipe can be returned where an appropriate match exists.
8. Scala calculates recipe compatibility.
9. React displays available versus missing ingredients.
10. A shopping list can be generated.
11. Preparation steps can be followed in order.
12. PostgreSQL/Aurora persists core state.
13. JUnit/Mockito tests validate important backend behaviour.
14. Jenkins builds and tests the application.
15. Docker images can be produced.
16. The application can be demonstrated using Kubernetes deployment configuration.
17. Important AWS operations can be observed through CloudWatch.
18. Failure of restaurant/menu data does not prevent the primary loop.

---

# 24. Explicit Compromises

We deliberately sacrifice:

### Restaurant breadth
A small reliable restaurant dataset is preferable to pretending to support every nearby restaurant.

### Live menu accuracy
Real-time menu aggregation is outside scope.

### Grocery sophistication
A deterministic missing-ingredient list replaces commerce.

### Location sophistication
Location is supportive rather than foundational.

### Scala breadth
Scala owns one small, meaningful domain calculation rather than creating an artificial second backend.

### Distributed architecture
The system remains primarily a modular Java application rather than being split into unnecessary microservices.

### Kubernetes necessity
Kubernetes exists primarily as a deployment-learning objective. The product itself does not inherently require Kubernetes at this scale.

---

# 25. Definition of Done

> **The constrained product is complete when a user can use the React interface to submit a fridge image, receive and correct AI-detected ingredients, choose cuisine/dietary requirements, receive Java-orchestrated recipe candidates ranked using the Scala compatibility engine, identify missing ingredients, open a structured recipe, generate a shopping list and progress through cooking steps — with data persisted in PostgreSQL/Aurora, images stored in S3, image processing using Lambda, automated tests executed through Jenkins, and the application packaged with Docker and deployable through Kubernetes.**