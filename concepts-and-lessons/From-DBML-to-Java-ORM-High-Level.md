# 🧭 From DBML to a Java ORM-Backed Database
## Turning a Data Model Into a Working Persistence Layer

## Project Context

We are building a new application where a user can:

```text
take a photo
        ↓
have the image analysed
        ↓
identify food / ingredients
        ↓
receive recipe suggestions
        ↓
see recipes that correspond to real restaurant menu items
```

The DBML is the starting point for the persistent data model.

It tells us:

```text
what things exist
how they relate
which records depend on which others
```

The job now is to turn that model into a working Java-backed database.

The broad journey is:

```text
DBML
        ↓
Relational schema
        ↓
Flyway migrations
        ↓
JPA entities
        ↓
JPA relationships
        ↓
Repositories
        ↓
Services
        ↓
REST API
```

The important thing is to understand what each step adds.

---

# 🧠 1️⃣ What the DBML Represents

DBML is a design language.

It helps us describe:

```text
tables
columns
primary keys
foreign keys
relationships
```

At this stage, the database exists as a model.

It is not yet the live database used by the Java application.

The DBML is our statement of:

```text
what the persistent world should look like
```

Before implementation, we also add two project-wide rules:

```text
every table gets timestamps

every fridge belongs to a user
```

That means the model now includes:

```text
created_at
updated_at
```

for every record, plus:

```text
fridge → user
```

as an ownership relationship.

---

# 🧱 2️⃣ From DBML to Relational Schema

The first descent is:

```text
DBML
        ↓
PostgreSQL schema
```

The ideas remain the same.

But the abstract model becomes concrete database structure.

For example:

```text
DBML table
        ↓
PostgreSQL table

DBML column
        ↓
PostgreSQL column

DBML primary key
        ↓
PRIMARY KEY

DBML reference
        ↓
FOREIGN KEY
```

The database now has enforceable structure.

This matters because relationships stop being drawings.

They become rules the database protects.

---

# 🔗 3️⃣ Relationships Become Constraints

A relationship such as:

```text
Recipe belongs to Restaurant
```

becomes a foreign key.

Conceptually:

```text
restaurants
      ↑
      |
recipes
```

This means the database can protect the relationship.

A recipe cannot simply point to a restaurant that does not exist.

The same pattern appears throughout the model.

Examples include:

```text
fridge → user

recipe → restaurant

food item → image

food item → fridge

saved recipe → user + recipe

preparation progress → user + recipe + preparation step
```

This is one of the most important transitions:

```text
relationship in design
        ↓
constraint in storage
```

---

# 🧬 4️⃣ Dependency Order Matters

Relationships create dependency.

A table that refers to another table depends on it.

So the schema cannot always be created in random order.

Example:

```text
User
        ↓
Fridge
```

The user table has to exist before the fridge can reference it.

Likewise:

```text
Restaurant
        ↓
Recipe
        ↓
Recipe-related records
```

This introduces a systems-thinking principle:

```text
relationships create build order
```

The schema is not just a collection of tables.

It is a dependency graph.

---

# 🛠️ 5️⃣ Flyway Makes the Schema Executable

The DBML is the model.

Flyway turns schema decisions into repeatable database history.

Conceptually:

```text
DBML says:
this is what the database should be

Flyway says:
this is how we create and evolve it
```

A migration might represent:

```text
create initial tables
        ↓
add relationship
        ↓
add constraint
        ↓
change a column
```

Instead of every developer manually editing their database, the project carries the database changes with it.

That gives us:

```text
shared schema history
repeatable setup
controlled change
```

For this project, the database structure should come from migrations, while Hibernate checks that Java agrees with the schema.

---

# ☕ 6️⃣ From Tables to Java Entities

The next descent is:

```text
database table
        ↓
Java entity
```

An entity is a Java representation of persistent data.

Broadly:

```text
table
        ↔
entity class

column
        ↔
field

primary key
        ↔
@Id
```

Example idea:

```text
restaurants
        ↓
Restaurant

recipes
        ↓
Recipe

users
        ↓
User

fridges
        ↓
Fridge
```

At this point, Java begins to understand the shape of the database.

---

# 🔗 7️⃣ Foreign Keys Become Object Relationships

This is where ORM becomes especially useful.

The database thinks in:

```text
IDs
foreign keys
rows
```

Java thinks in:

```text
objects
references
collections
```

ORM bridges the two.

Database:

```text
recipe.restaurant_id
```

Java:

```text
Recipe has a Restaurant
```

Database:

```text
fridge.user_id
```

Java:

```text
Fridge has a User
```

So ORM translates:

```text
relational relationships
        ↓
object relationships
```

That is the heart of JPA/Hibernate.

---

# 🧠 8️⃣ Not Every Relationship Is the Same

As we translate the DBML, we ask:

```text
Does one record belong to one other record?

Can one record have many others?

Does the relationship itself contain data?
```

That gives us relationship shapes such as:

```text
many-to-one

one-to-many

relationship entity / join entity
```

This is important because some relationships are more than simple connections.

For example, if a record connects:

```text
recipe
+
food item
```

and also stores:

```text
quantity
measurement
```

then the relationship itself has meaning.

That relationship should usually become a first-class entity.

This is relational modelling meeting object modelling.

---

# ⏱️ 9️⃣ Timestamps as a Shared Persistence Concern

Every table will contain:

```text
created_at
updated_at
```

The meaning is simple:

```text
created_at = when the record first existed

updated_at = when the record last changed
```

Because every entity shares this concern, Java can model it once and reuse it.

Conceptually:

```text
shared persistence behaviour
        ↓
shared base entity
```

This connects back to OOP:

```text
identify genuinely shared behaviour
        ↓
model it once
```

---

# 🗄️ 1️⃣0️⃣ What Hibernate Actually Does

Hibernate sits between Java and PostgreSQL.

Java works with:

```text
objects
```

PostgreSQL stores:

```text
rows
```

Hibernate maps between them.

Conceptually:

```text
Java Entity
        ↓
Hibernate
        ↓
SQL
        ↓
PostgreSQL
```

And on the way back:

```text
PostgreSQL rows
        ↓
Hibernate
        ↓
Java Entities
```

So Java code can work with:

```text
Recipe
Restaurant
User
Fridge
```

without manually writing SQL for every basic operation.

---

# 📚 1️⃣1️⃣ Repositories Become the Persistence Boundary

Once the entities exist, repositories give the application a place to ask storage questions.

Examples:

```text
find this recipe

find recipes for this restaurant

find food items detected from this image

find recipes saved by this user
```

The repository does not decide what the product should do.

It answers questions about persisted data.

A clean boundary is:

```text
Repository = access stored data

Service = decide what that data means
```

This separation is part of the layered architecture expected by the curriculum.

---

# ⚙️ 1️⃣2️⃣ Services Turn Stored Data Into Behaviour

The database stores facts.

The service layer turns those facts into application behaviour.

For our project:

```text
Image
        ↓
detected food items
        ↓
recipe requirements
        ↓
restaurant-backed recipe candidates
        ↓
suggestion decision
```

Repositories help retrieve the information.

Services coordinate the use case.

This keeps responsibilities clear:

```text
database = memory

repository = access

service = decision
```

---

# 🌐 1️⃣3️⃣ DTOs and Controllers Sit Above the ORM

The ORM model is internal.

The API should not simply expose database entities directly.

Instead:

```text
Entity = storage shape

DTO = API shape
```

Then the full flow becomes:

```text
HTTP request
        ↓
Controller
        ↓
DTO
        ↓
Service
        ↓
Repository
        ↓
JPA / Hibernate
        ↓
PostgreSQL
```

And back:

```text
PostgreSQL
        ↓
Entity
        ↓
Service
        ↓
Response DTO
        ↓
JSON
```

This is the complete backend data journey.

---

# 🧪 1️⃣4️⃣ How We Know the ORM Layer Works

The persistence layer is not finished because the classes compile.

We need proof.

At a high level, we verify:

```text
Can Flyway create the schema?

Does Hibernate agree with the schema?

Can entities be persisted?

Can relationships be loaded?

Can repositories retrieve expected data?

Can services use the persisted data correctly?
```

This connects directly to the curriculum emphasis on testing.

The important principle is:

```text
model = intention

implementation = attempt

test = proof
```

---

# 🔐 1️⃣5️⃣ Configuration Still Lives Outside the Code

The database connection itself is environment-specific.

Different developers may have:

```text
different usernames
different passwords
different local database settings
```

So the codebase should not depend on one person’s local credentials.

Instead:

```text
shared code
+
external configuration
```

This allows the same Java application to run:

```text
on different laptops

inside Docker

in test environments

in the cloud
```

The ORM model stays the same.

The environment changes.

---

# 🌍 1️⃣6️⃣ How This Supports the Product

The persistence model ultimately supports this relationship chain:

```text
User
        ↓
Fridge
        ↓
Image
        ↓
Detected food
        ↓
Recipe requirements
        ↓
Recipe
        ↓
Restaurant
```

The application can then answer:

```text
Given what we saw in this image,
which real restaurant-menu recipes
are relevant to this user?
```

Additional data can enrich that decision:

```text
dietary preferences

saved recipes

cuisine

preparation steps

grocery availability

videos
```

The database is therefore not just storage.

It is the persistent relationship graph underneath the product.

---

# 🎓 1️⃣7️⃣ Curriculum Alignment

This stage deliberately brings together several course requirements.

Students are practising:

```text
relational data modelling

primary keys and foreign keys

JPA/Hibernate

entity relationships

repositories

layered architecture

OOP and SOLID boundaries

services

DTOs

REST-ready design

database configuration

migrations

testing

Git / PR-based delivery
```

The important learning outcome is not merely:

```text
I can create an @Entity.
```

It is:

```text
I can take a relational model,
turn it into a real database,
map that database into Java,
and use it safely inside an application architecture.
```

---

# 🧭 1️⃣8️⃣ Recommended High-Level Build Sequence

## Step 1 — Agree the model

```text
DBML
timestamps
fridge ownership
relationships
naming
```

---

## Step 2 — Create the database schema

```text
DBML
        ↓
Flyway
        ↓
PostgreSQL
```

---

## Step 3 — Map the schema into Java

```text
tables
        ↓
entities

foreign keys
        ↓
object relationships
```

---

## Step 4 — Add persistence access

```text
entities
        ↓
repositories
```

---

## Step 5 — Add application behaviour

```text
repositories
        ↓
services
```

---

## Step 6 — Expose behaviour

```text
services
        ↓
DTOs
        ↓
controllers
        ↓
JSON API
```

---

## Step 7 — Prove the system

```text
migration checks

ORM validation

repository tests

service tests

API tests
```

---

# 🚀 Final Compression

```text
DBML = model the persistent world

PostgreSQL = store that world

Flyway = create and evolve that world

JPA = describe persistence in Java

Hibernate = translate Java objects ↔ database rows

Entity = Java persistence shape

Relationship annotation = Java view of a foreign key

Repository = persistence boundary

Service = business behaviour

DTO = API boundary

Controller = HTTP boundary

Test = proof
```

---

# 🌌 Ultimate Compression

```text
DBML describes the relationships.

SQL makes the relationships real.

ORM gives those relationships Java shape.

Repositories let Java reach them.

Services give them meaning.

APIs expose that meaning.
```

That is how a database diagram becomes a working backend.
