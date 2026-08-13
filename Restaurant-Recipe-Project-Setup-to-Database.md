# 🍽️ Restaurant Recipe Suggestions — Beginner Project Setup Guide
## From Empty Folder to a Working PostgreSQL Database

## Goal

By the end of this guide, you should have:

```text
Java working
        ↓
Spring Boot project running
        ↓
PostgreSQL running
        ↓
Application connected to PostgreSQL
        ↓
Flyway migration running
        ↓
Database tables created
```

We stop at the **database level**.

We are **not yet** building:

```text
JPA entities
repositories
services
controllers
image analysis
Azure deployment
```

Those come next.

For now, our goal is simply:

```text
Can every student run the same project
and create the same database structure?
```

---

# 🧠 1. What Are We Building?

The application will eventually allow a user to:

```text
take a photo
        ↓
identify food / ingredients
        ↓
match them against recipes
        ↓
receive recipe suggestions
        ↓
see recipes connected to real restaurant menu items
```

Our DBML already describes the main data model.

It includes concepts such as:

```text
users
restaurants
restaurant cuisines
recipes
restaurant menu items
food items
fridges
images
saved recipes
dietary preferences
grocery stores
preparation steps
videos
```

Before creating the database, we are also adding:

```text
created_at and updated_at to every table
```

and:

```text
user_id to fridges
```

so that every fridge belongs to a user.

---

# 🛠️ 2. Install the Required Tools

Make sure these are installed before continuing.

## Required

```text
JDK 17 or 21
IntelliJ IDEA
Git
PostgreSQL
pgAdmin or DBeaver
```

Useful later:

```text
Postman / Bruno / Insomnia
Node.js
VS Code
Docker
Azure tools
```

Azure is part of the later curriculum.

For this stage, everything runs locally.

---

# ☕ 3. Check Java

Open PowerShell.

Run:

```powershell
java -version
```

You should see Java 17 or Java 21.

Also run:

```powershell
javac -version
```

If both commands return a version, Java is ready.

---

# 📁 4. Create the Project Folder

Choose a simple location.

Example:

```text
C:\dev\restaurant-recipe-app
```

Create it:

```powershell
mkdir C:\dev\restaurant-recipe-app
cd C:\dev\restaurant-recipe-app
```

---

# 🌱 5. Create the Spring Boot Project

Use Spring Initializr or the starter project provided by the trainer.

Recommended settings:

```text
Project: Maven
Language: Java
Spring Boot: trainer/current version
Packaging: Jar
Java: 17 or 21
```

Add these dependencies:

```text
Spring Web
Spring Data JPA
PostgreSQL Driver
```

Then open the generated project in IntelliJ.

Your project should contain something like:

```text
restaurant-recipe-app/
├── pom.xml
├── mvnw
├── mvnw.cmd
└── src/
    ├── main/
    │   ├── java/
    │   └── resources/
    └── test/
```

---

# ▶️ 6. Run Spring Boot Once

Before adding the database, prove the Java project itself works.

In PowerShell:

```powershell
cd C:\dev\restaurant-recipe-app
.\mvnw.cmd spring-boot:run
```

You should eventually see a message showing that Spring Boot has started.

If the project starts, stop it with:

```text
Ctrl + C
```

This proves:

```text
Java works
Maven works
Spring Boot works
```

---

# 🗄️ 7. Create the PostgreSQL Database

Open pgAdmin or DBeaver.

Create a database:

```sql
CREATE DATABASE restaurant_recipes;
```

You may use your existing PostgreSQL user.

Every student may have a different:

```text
username
password
```

That is okay.

We will keep those values outside the shared code.

---

# 🔐 8. Set Your Local Database Environment Variables

Do **not** put your personal PostgreSQL password directly into the shared project.

In PowerShell:

```powershell
$env:DB_URL="jdbc:postgresql://localhost:5432/restaurant_recipes"
$env:DB_USERNAME="your_postgres_username"
$env:DB_PASSWORD="your_postgres_password"
```

Replace:

```text
your_postgres_username
your_postgres_password
```

with your own values.

These variables exist for the current PowerShell session.

This lets everyone use the same project even if their local credentials are different.

---

# 📦 9. Add Flyway

Open:

```text
pom.xml
```

Inside `<dependencies>`, make sure these database dependencies exist:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>

<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-core</artifactId>
</dependency>

<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-database-postgresql</artifactId>
</dependency>
```

Then reload Maven in IntelliJ.

Flyway will create and update our database structure.

---

# ⚙️ 10. Configure Spring Boot

Open:

```text
src/main/resources/application.properties
```

Add:

```properties
spring.application.name=restaurant-recipe-app

spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}

spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=true

spring.flyway.enabled=true
spring.flyway.locations=classpath:db/migration
```

The important idea is:

```text
Flyway creates the database structure.

Hibernate checks it later.
```

We are not asking Hibernate to invent the database for us.

---

# 📂 11. Create the Migration Folder

Inside:

```text
src/main/resources
```

create:

```text
db/migration
```

The result should be:

```text
src/
└── main/
    └── resources/
        ├── application.properties
        └── db/
            └── migration/
```

Now create:

```text
V1__create_initial_schema.sql
```

Final path:

```text
src/main/resources/db/migration/V1__create_initial_schema.sql
```

---

# 🧱 12. Add the Initial Database Schema

Paste this into:

```text
V1__create_initial_schema.sql
```

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(255),
    saved_longitude VARCHAR(50),
    saved_latitude VARCHAR(50),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE restaurants (
    id BIGSERIAL PRIMARY KEY,
    restaurant_name VARCHAR(255),
    longitude VARCHAR(50),
    latitude VARCHAR(50),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE grocery_stores (
    id BIGSERIAL PRIMARY KEY,
    store_name VARCHAR(255),
    longitude VARCHAR(50),
    latitude VARCHAR(50),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE images (
    id BIGSERIAL PRIMARY KEY,
    url VARCHAR(255),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE videos (
    id BIGSERIAL PRIMARY KEY,
    url VARCHAR(255),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE fridges (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL REFERENCES users(id),
    fridge_name VARCHAR(255),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE restaurant_cuisines (
    id BIGSERIAL PRIMARY KEY,
    restaurant_id BIGINT NOT NULL REFERENCES restaurants(id),
    cuisine VARCHAR(50),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE recipes (
    id BIGSERIAL PRIMARY KEY,
    restaurant_id BIGINT NOT NULL REFERENCES restaurants(id),
    recipe_name VARCHAR(255),
    description TEXT,
    originator VARCHAR(255),
    avg_restaurant_price NUMERIC(10,2),
    servings INTEGER,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE food_items (
    id BIGSERIAL PRIMARY KEY,
    image_id BIGINT REFERENCES images(id),
    fridge_id BIGINT REFERENCES fridges(id),
    ingredient_name VARCHAR(255),
    available BOOLEAN,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE recipe_metadata (
    id BIGSERIAL PRIMARY KEY,
    recipe_id BIGINT NOT NULL REFERENCES recipes(id),
    metadata_type VARCHAR(40),
    metadata_value VARCHAR(255),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE recipe_items (
    id BIGSERIAL PRIMARY KEY,
    recipe_id BIGINT NOT NULL REFERENCES recipes(id),
    menu_item_name VARCHAR(255),
    cost NUMERIC(10,2),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE preparation_steps (
    id BIGSERIAL PRIMARY KEY,
    recipe_id BIGINT NOT NULL REFERENCES recipes(id),
    step_number INTEGER,
    recipe_step VARCHAR(255),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE dietary_preferences (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL REFERENCES users(id),
    dietary_choice VARCHAR(50),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE saved_recipes (
    id BIGSERIAL PRIMARY KEY,
    recipe_id BIGINT NOT NULL REFERENCES recipes(id),
    user_id BIGINT NOT NULL REFERENCES users(id),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE recipe_food_items (
    id BIGSERIAL PRIMARY KEY,
    recipe_id BIGINT NOT NULL REFERENCES recipes(id),
    food_item_id BIGINT NOT NULL REFERENCES food_items(id),
    measurement_unit VARCHAR(50),
    measurement_value NUMERIC(10,2),
    measurement_text VARCHAR(255),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE grocery_store_inventory (
    id BIGSERIAL PRIMARY KEY,
    grocery_store_id BIGINT NOT NULL REFERENCES grocery_stores(id),
    food_item_id BIGINT NOT NULL REFERENCES food_items(id),
    price NUMERIC(10,2),
    measurement_unit VARCHAR(50),
    measurement_value NUMERIC(10,2),
    measurement_text VARCHAR(255),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE preparation_step_progress (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT REFERENCES users(id),
    recipe_id BIGINT REFERENCES recipes(id),
    preparation_step_id BIGINT REFERENCES preparation_steps(id),
    total_steps INTEGER,
    current_step INTEGER,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE recipe_videos (
    id BIGSERIAL PRIMARY KEY,
    recipe_id BIGINT NOT NULL REFERENCES recipes(id),
    video_id BIGINT NOT NULL REFERENCES videos(id),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

Do not worry about memorising this SQL.

At this stage, understand the shape:

```text
DBML
        ↓
SQL tables
        ↓
primary keys
        ↓
foreign keys
        ↓
real relational database
```

---

# 🚀 13. Run the Migration

Make sure PostgreSQL is running.

In the **same PowerShell window** where you set the environment variables, run:

```powershell
.\mvnw.cmd spring-boot:run
```

Spring Boot should:

```text
connect to PostgreSQL
        ↓
start Flyway
        ↓
find V1__create_initial_schema.sql
        ↓
create the tables
```

---

# 🔎 14. Check the Database

Open pgAdmin or DBeaver.

Refresh:

```text
restaurant_recipes
```

Look under:

```text
Schemas
→ public
→ Tables
```

You should see tables including:

```text
users
restaurants
restaurant_cuisines
recipes
recipe_metadata
recipe_items
food_items
recipe_food_items
grocery_stores
grocery_store_inventory
fridges
preparation_steps
preparation_step_progress
saved_recipes
dietary_preferences
images
videos
recipe_videos
flyway_schema_history
```

`flyway_schema_history` is created by Flyway.

It remembers which migrations have already run.

---

# 🧪 15. Quick SQL Check

Run:

```sql
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public'
ORDER BY table_name;
```

If your tables appear, the migration worked.

You can also try:

```sql
SELECT * FROM users;
```

It should return an empty result rather than an error.

That means the table exists.

---

# 🧠 16. What We Have Achieved

At this point:

```text
DBML
        ↓
Flyway SQL migration
        ↓
PostgreSQL database
```

The database now physically understands relationships such as:

```text
User
        ↓ owns
Fridge

Restaurant
        ↓ has
Recipe

Image
        ↓ can relate to
Food Item

Recipe
        ↓ uses
Food Item
```

These relationships will later become Java object relationships through JPA/Hibernate.

---

# ☕ 17. What Comes Next

The next stage will be:

```text
PostgreSQL table
        ↓
JPA Entity
        ↓
Repository
```

For example:

```text
users table
        ↓
User.java
        ↓
UserRepository
```

Then:

```text
recipes table
        ↓
Recipe.java
        ↓
RecipeRepository
```

Eventually the full backend becomes:

```text
Controller
        ↓
Service
        ↓
Repository
        ↓
JPA / Hibernate
        ↓
PostgreSQL
```

But **do not move ahead until the database setup works correctly**.

---

# ⚠️ 18. Common Problems

## `java` command not found

Java is not installed correctly or is not on your PATH.

---

## PostgreSQL connection refused

Check that PostgreSQL is running.

---

## Password authentication failed

Check:

```text
DB_USERNAME
DB_PASSWORD
```

Remember: each student may have different local credentials.

---

## `DB_URL` cannot be resolved

Make sure you set the environment variables in the same PowerShell window before starting Spring Boot.

---

## Flyway migration failed

Read the first Flyway error carefully.

Common causes:

```text
SQL typo
database already contains conflicting tables
foreign key points to a table that was not created first
```

---

## Migration changed after it already ran

Do not casually edit an old migration after the team has started using it.

Later database changes should normally become:

```text
V2__...
V3__...
V4__...
```

This is how the database gains a controlled history.

---

# ✅ 19. Completion Checklist

You are finished when:

- [ ] Java 17 or 21 works
- [ ] IntelliJ opens the project
- [ ] Maven runs
- [ ] Spring Boot starts
- [ ] PostgreSQL is installed and running
- [ ] `restaurant_recipes` database exists
- [ ] database environment variables are set
- [ ] JPA dependency is installed
- [ ] PostgreSQL driver is installed
- [ ] Flyway is installed
- [ ] `application.properties` uses environment variables
- [ ] `V1__create_initial_schema.sql` exists
- [ ] Spring Boot runs the migration successfully
- [ ] all expected tables exist
- [ ] `flyway_schema_history` exists
- [ ] no personal database password is committed to Git

---

# 🎓 Curriculum Connection

This setup prepares us for the Java persistence section of the curriculum.

The progression is:

```text
Database design
        ↓
PostgreSQL
        ↓
JPA / Hibernate
        ↓
Repositories
        ↓
Services
        ↓
REST APIs
        ↓
Testing
        ↓
Azure
```

Azure comes later.

The important thing now is that everyone has the **same project and the same database shape**.

---

# 🚀 Final Compression

```text
DBML = design

PostgreSQL = real database

Flyway = repeatable database setup

Environment variables = local configuration

Foreign key = database relationship

JPA/Hibernate = next bridge from database to Java
```

For this stage:

```text
If Spring Boot starts,
Flyway succeeds,
and the tables exist,
you are done.
```
