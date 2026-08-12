# 🧭 From DBML to a Working Java ORM-Backed Database
## Turning the Restaurant Recipe Suggestion Data Model into PostgreSQL + JPA/Hibernate

## Project Context

We are building a new application.

The core user journey is:

```text
User takes a photo
        ↓
Image is analysed
        ↓
Food items / ingredients are detected
        ↓
Detected food is compared against recipes
        ↓
Recipes correspond to real restaurant menu items
        ↓
User receives restaurant-style recipe suggestions
```

This is similar in shape to our earlier image-to-meal work, but the product is different.

We are **not** building Fridge2Meal.

Our recipe catalogue is grounded in real restaurant dishes and menu items.

The database therefore needs to model:

```text
restaurants
restaurant cuisines
recipes
restaurant menu items
recipe metadata
food items
recipe ingredients
users
fridges
images
saved recipes
dietary preferences
preparation steps
preparation progress
grocery stores
store inventory
videos
```

The DBML gives us the conceptual relational model.

Our job is to descend that model into:

```text
DBML
        ↓
PostgreSQL schema
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
        ↓
Tests
```

That is the bridge from **database design** to a **working Java backend**.

---

# 1️⃣ What the DBML Already Gives Us

The current DBML defines these tables:

```text
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
users
preparation_step_progress
saved_recipes
dietary_preferences
images
videos
recipe_videos
```

The DBML already tells us:

```text
what things exist
which columns they contain
which records have IDs
which relationships exist
where foreign keys point
```

Example:

```dbml
Table recipes {
  id int [pk, increment]
  restaurant_id int [not null, ref: > restaurants.id]
  recipe_name varchar(255)
}
```

This tells us:

```text
A recipe belongs to a restaurant.
```

In relational language:

```text
restaurants 1 ─────── * recipes
```

In Java/JPA language:

```java
@ManyToOne
private Restaurant restaurant;
```

That translation is the central task.

---

# 2️⃣ Two Required Changes Before Implementation

Before turning the DBML into SQL, we are adding two cross-cutting requirements.

## A. Every record gets timestamps

Every table should have:

```text
created_at
updated_at
```

Recommended PostgreSQL shape:

```sql
created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
```

For this learning project, that gives every entity a lifecycle.

Conceptually:

```text
created_at = when this record first entered persistent storage

updated_at = when this record was last changed
```

Later, JPA lifecycle hooks or auditing can keep `updated_at` current.

---

## B. Every fridge belongs to a user

Current DBML:

```dbml
Table fridges {
  id int [pk, increment]
  fridge_name varchar(255)
}
```

Required change:

```dbml
Table fridges {
  id int [pk, increment]
  user_id int [not null, ref: > users.id]
  fridge_name varchar(255)
}
```

Conceptually:

```text
User 1 ─────── * Fridges
```

A user can own multiple fridges.

A fridge belongs to one user.

Java:

```java
@ManyToOne
@JoinColumn(name = "user_id", nullable = false)
private User user;
```

---

# 3️⃣ First Step: Normalise the Database Types

DBML is a modelling language.

PostgreSQL has its own concrete types.

Some DBML types map directly:

| DBML | PostgreSQL | Java |
|---|---|---|
| `int` | `BIGINT` or `INTEGER` | `Long` / `Integer` |
| `varchar(255)` | `VARCHAR(255)` | `String` |
| `boolean` | `BOOLEAN` | `Boolean` / `boolean` |
| `decimal(10,2)` | `NUMERIC(10,2)` | `BigDecimal` |
| `longtext` | `TEXT` | `String` |
| timestamp | `TIMESTAMP` | `LocalDateTime` |

For this project, using:

```text
BIGSERIAL / BIGINT IDs
```

is a good Java/Spring convention.

So DBML:

```dbml
id int [pk, increment]
```

can become:

```sql
id BIGSERIAL PRIMARY KEY
```

And Java:

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

---

# 4️⃣ Treat Money as BigDecimal

The schema contains:

```text
avg_restaurant_price
cost
price
measurement_value
```

For money, avoid:

```java
double
float
```

Use:

```java
BigDecimal
```

Example:

```java
@Column(name = "avg_restaurant_price", precision = 10, scale = 2)
private BigDecimal averageRestaurantPrice;
```

Why?

Because financial values should not rely on floating-point approximation.

---

# 5️⃣ Think in Relationships Before Writing Entities

The most important move is to read each foreign key as a relationship.

From the DBML:

```text
restaurant_cuisines.restaurant_id → restaurants.id
recipes.restaurant_id → restaurants.id
recipe_metadata.recipe_id → recipes.id
recipe_items.recipe_id → recipes.id
recipe_food_items.recipe_id → recipes.id
recipe_food_items.food_item_id → food_items.id
food_items.image_id → images.id
food_items.fridge_id → fridges.id
grocery_store_inventory.grocery_store_id → grocery_stores.id
grocery_store_inventory.food_item_id → food_items.id
fridges.user_id → users.id
preparation_steps.recipe_id → recipes.id
preparation_step_progress.user_id → users.id
preparation_step_progress.recipe_id → recipes.id
preparation_step_progress.preparation_step_id → preparation_steps.id
saved_recipes.recipe_id → recipes.id
saved_recipes.user_id → users.id
dietary_preferences.user_id → users.id
recipe_videos.recipe_id → recipes.id
recipe_videos.video_id → videos.id
```

These relationships become the graph of the backend.

---

# 6️⃣ Relationship Categories

## Many-to-One

Example:

```text
Many recipes belong to one restaurant.
```

JPA:

```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "restaurant_id", nullable = false)
private Restaurant restaurant;
```

---

## One-to-Many

The reverse side:

```text
One restaurant has many recipes.
```

JPA:

```java
@OneToMany(mappedBy = "restaurant")
private List<Recipe> recipes = new ArrayList<>();
```

Important:

You do **not** always need both sides immediately.

For beginners, start with the side you need.

---

# 7️⃣ Join Tables With Their Own Data Are Entities

This is especially important in this DBML.

Look at:

```text
recipe_food_items
```

It joins:

```text
recipe
+
food_item
```

But it also contains:

```text
measurement_unit
measurement_value
measurement_text
```

Therefore it is **not just a hidden many-to-many table**.

It has behaviourally meaningful data.

So model it as a proper entity:

```java
@Entity
@Table(name = "recipe_food_items")
public class RecipeFoodItem {
}
```

with:

```java
@ManyToOne
private Recipe recipe;

@ManyToOne
private FoodItem foodItem;

private String measurementUnit;

private BigDecimal measurementValue;

private String measurementText;
```

This is better than:

```java
@ManyToMany
```

because the relationship itself contains data.

---

# 8️⃣ The Same Rule Applies to Other Join Entities

These should be treated as real entities:

```text
recipe_food_items
grocery_store_inventory
preparation_step_progress
saved_recipes
recipe_videos
```

Why?

Because either:

```text
the relationship has its own fields
```

or:

```text
we want the relationship to have its own identity/timestamps
```

This matters especially now because every record receives:

```text
created_at
updated_at
```

So even a conceptually simple join becomes a first-class persistent record.

---

# 9️⃣ Suggested Entity Map

Your Java `entity` package should eventually contain:

```text
Restaurant
RestaurantCuisine
Recipe
RecipeMetadata
RecipeItem
FoodItem
RecipeFoodItem
GroceryStore
GroceryStoreInventory
Fridge
PreparationStep
User
PreparationStepProgress
SavedRecipe
DietaryPreference
Image
Video
RecipeVideo
```

Each class maps to one table.

That is the first implementation milestone:

```text
one table
↔
one entity
```

---

# 🔟 Create a Shared Timestamp Base Class

Because every record now needs timestamps, we should avoid repeating the same fields manually 18 times.

A clean Java approach:

```java
@MappedSuperclass
public abstract class BaseEntity {

    @Column(name = "created_at", nullable = false)
    private LocalDateTime createdAt;

    @Column(name = "updated_at", nullable = false)
    private LocalDateTime updatedAt;

    @PrePersist
    protected void onCreate() {
        LocalDateTime now = LocalDateTime.now();
        createdAt = now;
        updatedAt = now;
    }

    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }
}
```

Then:

```java
@Entity
@Table(name = "restaurants")
public class Restaurant extends BaseEntity {
}
```

This is an example of inheritance being used for a truthful shared persistence concern.

Later, Spring Data JPA auditing could replace the lifecycle methods.

For now, this is easy to see and understand.

---

# 1️⃣1️⃣ Example: Restaurant Entity

SQL concept:

```sql
CREATE TABLE restaurants (
    id BIGSERIAL PRIMARY KEY,
    restaurant_name VARCHAR(255),
    longitude VARCHAR(50),
    latitude VARCHAR(50),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

Java:

```java
@Entity
@Table(name = "restaurants")
public class Restaurant extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "restaurant_name")
    private String restaurantName;

    private String longitude;

    private String latitude;
}
```

---

# 1️⃣2️⃣ Example: Recipe Entity

The DBML says:

```text
Recipe belongs to Restaurant.
```

Java:

```java
@Entity
@Table(name = "recipes")
public class Recipe extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "restaurant_id", nullable = false)
    private Restaurant restaurant;

    @Column(name = "recipe_name")
    private String recipeName;

    @Column(columnDefinition = "TEXT")
    private String description;

    private String originator;

    @Column(name = "avg_restaurant_price", precision = 10, scale = 2)
    private BigDecimal averageRestaurantPrice;

    private Integer servings;
}
```

Notice:

```text
restaurant_id in SQL
```

becomes:

```text
Restaurant restaurant in Java
```

That is ORM in action.

---

# 1️⃣3️⃣ Example: Fridge Entity

With the new requirement:

```java
@Entity
@Table(name = "fridges")
public class Fridge extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;

    @Column(name = "fridge_name")
    private String fridgeName;
}
```

Conceptually:

```text
foreign key in database
        ↓
object reference in Java
```

---

# 1️⃣4️⃣ Example: FoodItem Entity

A detected food item may be linked to:

```text
the image it came from
the fridge it belongs to
```

Example:

```java
@Entity
@Table(name = "food_items")
public class FoodItem extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "image_id")
    private Image image;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "fridge_id")
    private Fridge fridge;

    @Column(name = "ingredient_name")
    private String ingredientName;

    private Boolean available;
}
```

This is important for our product flow:

```text
Image
        ↓
FoodItems detected
        ↓
FoodItems used to match recipes
```

---

# 1️⃣5️⃣ Example: RecipeFoodItem

This entity tells us:

```text
Which food items are required for a recipe?
And in what quantity?
```

```java
@Entity
@Table(name = "recipe_food_items")
public class RecipeFoodItem extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "recipe_id", nullable = false)
    private Recipe recipe;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "food_item_id", nullable = false)
    private FoodItem foodItem;

    @Column(name = "measurement_unit")
    private String measurementUnit;

    @Column(name = "measurement_value", precision = 10, scale = 2)
    private BigDecimal measurementValue;

    @Column(name = "measurement_text")
    private String measurementText;
}
```

This entity is central to recipe matching.

---

# 1️⃣6️⃣ A Modelling Question to Watch Carefully

The DBML currently uses:

```text
food_items
```

for items detected from images/fridges **and** for items referenced by recipes/store inventory.

That may be intentional.

But think carefully about the domain.

There are potentially two different concepts:

```text
Ingredient definition:
"tomato"

User-owned/detected food instance:
"this tomato in this fridge"
```

Those are not always the same thing.

A more mature model might eventually split:

```text
Ingredient
        ↓
FoodInventoryItem
```

Example:

```text
Ingredient:
id = 1
name = tomato

FoodInventoryItem:
ingredient_id = 1
fridge_id = 5
available = true
```

For now, we can implement the supplied DBML.

But this is an excellent architecture discussion during PR review.

Do not silently redesign the schema before the team agrees.

---

# 1️⃣7️⃣ Flyway Is the Source of Database Structure

The curriculum expects persistent relational data through:

```text
JPA/Hibernate
+
relational database
```

But Hibernate should not secretly invent our schema.

Use Flyway.

Recommended:

```properties
spring.jpa.hibernate.ddl-auto=validate

spring.flyway.enabled=true
spring.flyway.locations=classpath:db/migration
```

Meaning:

```text
Flyway creates the schema.

Hibernate checks that Java agrees with it.
```

That gives us a clean separation.

---

# 1️⃣8️⃣ Migration Strategy

Start with:

```text
src/main/resources/db/migration/
```

Example:

```text
V1__create_initial_schema.sql
```

The migration should create all tables in dependency-safe order.

---

# 1️⃣9️⃣ Dependency-Aware Table Order

Foreign keys mean tables cannot always be created randomly.

A sensible order is:

```text
1. users
2. restaurants
3. grocery_stores
4. images
5. videos
6. fridges
7. restaurant_cuisines
8. recipes
9. food_items
10. recipe_metadata
11. recipe_items
12. preparation_steps
13. dietary_preferences
14. saved_recipes
15. recipe_food_items
16. grocery_store_inventory
17. preparation_step_progress
18. recipe_videos
```

Why?

Because:

```text
parent tables must exist
before child tables can reference them
```

This is relational dependency thinking.

---

# 2️⃣0️⃣ Add Constraints, Not Just Columns

A workable database is more than columns.

Use constraints to protect truth.

Examples:

```sql
restaurant_id BIGINT NOT NULL REFERENCES restaurants(id)
```

```sql
user_id BIGINT NOT NULL REFERENCES users(id)
```

Potential unique constraints:

```text
saved_recipes(user_id, recipe_id)

recipe_videos(recipe_id, video_id)

restaurant_cuisines(restaurant_id, cuisine)
```

These prevent accidental duplicate relationships.

The exact constraints should be agreed by the team.

---

# 2️⃣1️⃣ Create the Repositories

The curriculum expects repositories for persistence and sensible query methods.

Each main entity can have a repository.

Example:

```java
public interface RecipeRepository
        extends JpaRepository<Recipe, Long> {
}
```

Example:

```java
public interface RestaurantRepository
        extends JpaRepository<Restaurant, Long> {
}
```

Example custom query:

```java
List<Recipe> findByRestaurantId(Long restaurantId);
```

Or:

```java
List<Recipe> findByRestaurant_RestaurantName(String restaurantName);
```

Spring Data JPA can derive queries from method names.

---

# 2️⃣2️⃣ Repositories Should Answer Storage Questions

Examples:

```text
Find recipes for restaurant X.

Find food items detected from image Y.

Find saved recipes for user Z.

Find dietary preferences for user Z.

Find recipe ingredients for recipe X.

Find grocery inventory records for ingredient Y.
```

Repository layer:

```text
asks storage questions
```

Service layer:

```text
decides what those answers mean
```

This preserves the layered architecture required by the curriculum.

---

# 2️⃣3️⃣ Do Not Put Matching Logic in the Repository

Our key business feature is:

```text
image
→ detected food
→ restaurant recipe suggestions
```

The repository should not decide which recipe is best.

Bad:

```java
recipeRepository.magicBestRecipeForUser(...)
```

Better:

```text
repositories retrieve data
        ↓
service compares / ranks / applies rules
```

Example:

```java
@Service
public class RecipeSuggestionService {

    private final RecipeRepository recipeRepository;
    private final FoodItemRepository foodItemRepository;
    private final RecipeFoodItemRepository recipeFoodItemRepository;

    public List<RecipeSuggestionResponse> suggestRecipes(Long imageId) {
        // load detected foods
        // load candidate recipes
        // compare requirements
        // calculate match
        // return ranked suggestions
    }
}
```

Business logic belongs in services.

---

# 2️⃣4️⃣ DTOs Must Be Separate From Entities

The curriculum expects REST controllers using DTOs and predictable response shapes.

Do not return JPA entities directly.

Clean rule:

```text
Entity = storage shape
DTO = API boundary shape
```

Example response:

```java
public record RecipeSuggestionResponse(
        Long recipeId,
        String recipeName,
        String restaurantName,
        BigDecimal restaurantPrice,
        List<String> matchedIngredients,
        List<String> missingIngredients
) {}
```

This lets the API expose the product view without leaking internal persistence structure.

---

# 2️⃣5️⃣ Suggested First Useful API Flow

For this project, a strong early vertical slice is:

```http
POST /api/images/{imageId}/recipe-suggestions
```

Conceptually:

```text
Image exists
        ↓
Food items are associated with image
        ↓
RecipeSuggestionService loads food items
        ↓
Recipe requirements are loaded
        ↓
Recipes are compared
        ↓
Restaurant-backed suggestions returned
```

The exact endpoint can evolve.

The important thing is the layer flow:

```text
Controller
        ↓
DTO
        ↓
Service
        ↓
Repositories
        ↓
JPA/Hibernate
        ↓
PostgreSQL
```

---

# 2️⃣6️⃣ JPA Fetching and Lazy Loading

For relationships such as:

```java
@ManyToOne
```

prefer:

```java
fetch = FetchType.LAZY
```

unless you have a clear reason not to.

Why?

Because ORM relationships can hide database I/O.

This:

```java
recipe.getRestaurant()
```

may cause another database query.

That connects directly to the N+1 problem.

The curriculum expects learners to reason about real database behaviour, not just object syntax.

---

# 2️⃣7️⃣ Transactions

Some service operations may change several records.

Example:

```text
save image
        ↓
save detected food items
        ↓
save suggestion metadata
```

If these operations form one consistency boundary, use:

```java
@Transactional
```

at the service layer.

Conceptually:

```text
all succeed
or
all roll back
```

Transactions are not a controller concern.

They belong around business operations.

---

# 2️⃣8️⃣ Validation

The curriculum requires validation and predictable errors at the API boundary.

Example request DTO:

```java
public record CreateRestaurantRequest(

    @NotBlank
    String restaurantName,

    String longitude,

    String latitude
) {}
```

Controller:

```java
@PostMapping
public ResponseEntity<RestaurantResponse> create(
        @Valid @RequestBody CreateRestaurantRequest request
) {
    ...
}
```

Validation protects the system before bad data reaches persistence.

---

# 2️⃣9️⃣ Error Handling

Typical failures:

```text
restaurant not found
recipe not found
user not found
fridge does not belong to user
image not found
duplicate saved recipe
invalid measurement
database constraint violation
```

Use named exceptions.

Example:

```java
public class RecipeNotFoundException
        extends RuntimeException {
}
```

Then use:

```java
@RestControllerAdvice
```

to translate failures into structured API responses.

This keeps controllers thin.

---

# 3️⃣0️⃣ Testing Requirements

The curriculum expects meaningful tests, especially around high-risk behaviour.

For this ORM layer, use three levels.

## Entity/schema validation

Start app with:

```properties
spring.jpa.hibernate.ddl-auto=validate
```

If Java mappings do not match PostgreSQL, startup should fail.

---

## Repository tests

Examples:

```text
Can a recipe be saved?
Can recipes be found by restaurant?
Can saved recipes be found by user?
Can food items be found by image?
```

Potential tool:

```java
@DataJpaTest
```

---

## Service tests

The highest-value logic is recipe matching.

Test things like:

```text
exact ingredient match
partial ingredient match
missing ingredients
dietary preference filtering
multiple restaurants with similar dishes
no detected food
no matching recipe
```

Remember:

```text
Ticket = promise
Implementation = attempt
Test = proof
```

---

# 3️⃣1️⃣ Git and PR Delivery

The curriculum also expects:

```text
branches
pull requests
code reviews
incremental delivery
```

Do not have one person build the entire ORM layer.

Break it into dependency-aware tickets.

Example:

```text
Ticket 1:
BaseEntity + User + Fridge

Ticket 2:
Restaurant + RestaurantCuisine

Ticket 3:
Recipe + RecipeMetadata + RecipeItem

Ticket 4:
Image + FoodItem

Ticket 5:
RecipeFoodItem

Ticket 6:
SavedRecipe + DietaryPreference

Ticket 7:
PreparationStep + PreparationStepProgress

Ticket 8:
GroceryStore + GroceryStoreInventory

Ticket 9:
Video + RecipeVideo
```

Each learner can implement individually.

Then:

```text
open PR
        ↓
review mappings
        ↓
compare design
        ↓
merge strongest base
        ↓
integrate useful ideas
```

This is exactly the kind of engineering reasoning the capstone should exercise.

---

# 3️⃣2️⃣ What to Review in ORM Pull Requests

Review:

```text
Does entity map to table?

Are column names correct?

Are nullable constraints accurate?

Are relationships correct?

Is ManyToOne used where appropriate?

Is a join entity being incorrectly modelled as ManyToMany?

Are money values BigDecimal?

Are timestamps inherited?

Are fetch types reasonable?

Are repositories clean?

Does Hibernate validate successfully?

Does Flyway migrate successfully?

Are tests meaningful?
```

---

# 3️⃣3️⃣ Suggested Package Structure

```text
com.restaurantrecipes
├── controller
├── dto
├── entity
├── repository
├── service
├── exception
└── config
```

Do not over-engineer package structure yet.

Keep responsibilities legible.

---

# 3️⃣4️⃣ Environment Variables Still Matter

Database credentials should not be committed.

Shared configuration:

```properties
spring.datasource.url=${DB_URL:jdbc:postgresql://localhost:5432/restaurant_recipes}
spring.datasource.username=${DB_USERNAME:app_user}
spring.datasource.password=${DB_PASSWORD:password}

spring.jpa.hibernate.ddl-auto=validate

spring.flyway.enabled=true
spring.flyway.locations=classpath:db/migration
```

Each learner supplies their local credentials.

Same code.

Different environment.

---

# 3️⃣5️⃣ The Complete ORM Flow

When the system reads a recipe:

```text
RecipeService
        ↓
RecipeRepository
        ↓
Spring Data JPA
        ↓
Hibernate
        ↓
SQL SELECT
        ↓
PostgreSQL
        ↓
database rows
        ↓
Hibernate
        ↓
Recipe entity
        ↓
Service
        ↓
Response DTO
```

When the system saves a fridge:

```text
CreateFridgeRequest
        ↓
Controller
        ↓
FridgeService
        ↓
find User
        ↓
create Fridge entity
        ↓
FridgeRepository.save()
        ↓
Hibernate INSERT
        ↓
PostgreSQL
```

This is ORM-backed development in motion.

---

# 3️⃣6️⃣ The Product Data Flow

The database should ultimately support this:

```text
User
        ↓ owns
Fridge
        ↓ linked to
Image
        ↓ produces
FoodItems
        ↓ compared with
RecipeFoodItems
        ↓ identify
Recipes
        ↓ belong to
Restaurants
        ↓ expose
Real restaurant menu dishes
```

Then additional context can influence the result:

```text
DietaryPreferences
RecipeMetadata
RestaurantCuisine
GroceryStoreInventory
SavedRecipes
PreparationProgress
```

That is the actual system we are building.

---

# 3️⃣7️⃣ Recommended Build Sequence

## Phase 1 — Schema truth

```text
Update DBML
Add timestamps
Add user_id to fridges
Resolve naming/types
Agree constraints
```

Acceptance gate:

```text
Team agrees the relational model represents the intended domain.
```

---

## Phase 2 — Flyway

```text
Create V1 migration
Create tables in dependency order
Add PKs/FKs
Add constraints
Run PostgreSQL migration
```

Acceptance gate:

```text
Flyway completes successfully.
All expected tables exist.
```

---

## Phase 3 — Entities

```text
Create BaseEntity
Create core entities
Map columns
Map foreign keys
```

Acceptance gate:

```text
Backend starts with Hibernate validate.
```

---

## Phase 4 — Repositories

```text
Create repositories
Add minimal useful query methods
```

Acceptance gate:

```text
Repository tests can persist and retrieve important relationships.
```

---

## Phase 5 — Services

```text
Create business services
Keep logic out of controllers/repositories
```

Acceptance gate:

```text
Core use cases work through Java services.
```

---

## Phase 6 — REST

```text
Request DTOs
Response DTOs
Controllers
Validation
Status codes
Structured errors
```

Acceptance gate:

```text
API can drive a coherent user journey.
```

---

## Phase 7 — Image-to-Restaurant-Recipe Loop

```text
Image
→ detected food items
→ persistence
→ recipe matching
→ restaurant menu recipe response
```

Acceptance gate:

```text
A real image can result in a restaurant-backed recipe suggestion.
```

---

## Phase 8 — Tests + PR Review

```text
Repository tests
Service tests
Controller/API tests
Review architecture
Refactor
```

Acceptance gate:

```text
Expected behaviour is provable.
```

---

# 3️⃣8️⃣ Curriculum Alignment

This work directly exercises the expected course outcomes:

```text
Real-world domain
        ↓
entities + relationships
        ↓
JPA/Hibernate persistence
        ↓
repositories
        ↓
services
        ↓
DTOs + controllers
        ↓
validation + errors
        ↓
tests
        ↓
Git / PR review
```

You should be able to explain not only:

```text
what code you wrote
```

but:

```text
why this relationship exists
why this layer owns this responsibility
why the schema takes this shape
how the Java object maps to relational storage
how the system proves it works
```

That is the real curriculum outcome.

---

# 🚀 Final Compression

```text
DBML = conceptual relational truth

Flyway = executable schema history

PostgreSQL = persistent relational storage

Entity = Java representation of a table record

@ManyToOne = Java representation of a foreign-key relationship

Join entity = relationship that has its own data/identity

Repository = persistence boundary

Service = business use-case owner

DTO = API boundary shape

Controller = HTTP boundary

Hibernate = ORM engine

JPA = Java persistence contract

Test = proof that the model and behaviour work
```

---

# 🌌 Ultimate Compression

```text
DBML describes relationships.

SQL makes those relationships physically real.

JPA gives those relationships Java shape.

Repositories let Java reach them.

Services give them business meaning.

APIs expose that meaning to the outside world.
```

The database is not just storage.

It is the persistent relationship graph underneath the application.
