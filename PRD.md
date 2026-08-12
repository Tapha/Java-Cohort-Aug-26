Product Requirements Document --- Restaurant-Style Fridge-to-Meal
===============================================================

**Spec level:** Product Requirements Document (one level below Founding Spec)\
**Product principle:** Fridge → ingredients → cuisine/preferences → compatible restaurant-style dishes → recipe → missing ingredients → cooking\
**Primary MVP outcome:** A user can photograph food they already own and be guided to a credible restaurant-style dish they can cook with minimal additional shopping.

* * * * *

1\. Product Summary
-------------------

The product helps users turn ingredients they already have at home into a restaurant-style meal they can realistically cook. The user selects a cuisine and relevant dietary requirements, captures or uploads an image of their available food, reviews the ingredients identified by the system, and receives a small set of compatible dishes.

For each suitable dish, the product explains the ingredient fit, identifies what the user already has and what is missing, and provides a recipe and guided cooking flow. Where reliable restaurant and menu data exists, the dish is associated with a real restaurant or menu item. The primary value is reducing the gap between **"I have these ingredients"** and **"I know what restaurant-style meal I can actually make."**

* * * * *

2\. Problem Statement
=====================

The initial problem is not general recipe discovery or meal planning.

A user has ingredients at home but does not know what appealing meal to make from them. They may have a cuisine in mind, may have dietary restrictions, and may be willing to buy a small number of additional ingredients, but they do not want to start with a recipe that requires an entirely new shopping trip.

Existing recipe discovery generally starts with a recipe, ingredient search, or broad meal category. It does not necessarily begin with the user's actual physical inventory and connect that inventory to a recognisable restaurant-style target.

The product therefore addresses:

> **"I have food at home. What restaurant-style dish can I make with what I already have, and what is the minimum I need to buy to finish it?"**

The product must reduce three types of friction:

1.  **Inventory friction** --- determining what food is actually available.

2.  **Discovery friction** --- translating those ingredients into attractive, cuisine-appropriate dishes.

3.  **Planning friction** --- understanding exactly what is missing and how to cook the selected dish.

Restaurant association provides a useful discovery and aspiration layer. It makes recommendations more concrete and familiar. However, the core product value is the successful transition from **available ingredients → feasible dish → cooked meal**.

* * * * *

3\. Goals
=========

The first-version product should achieve the following outcomes.

### G1 --- Make physical ingredients usable as a digital starting point

A user should be able to photograph available food and receive a useful, editable ingredient list without manually entering everything.

### G2 --- Minimise unnecessary shopping

Recommendations should preferentially use ingredients the user already has and clearly expose unavoidable missing ingredients.

### G3 --- Make restaurant-style discovery actionable

The product should translate cuisine preferences and available ingredients into credible dishes rather than returning generic recipe inspiration.

### G4 --- Make recommendation feasibility transparent

Users should understand why a dish is considered compatible, including what they already own and what they would need to obtain.

### G5 --- Guide the user through cooking

Selecting a dish should lead to a complete, usable recipe and an ordered cooking flow rather than ending at discovery.

### G6 --- Preserve trust in factual information

Restaurant/menu facts must be distinguishable from AI-generated or inferred information. The product must not present an AI inference as a verified restaurant fact.

### G7 --- Prove the complete product loop

The first version should demonstrate that a user can move from a fridge image to a cooked restaurant-style meal with minimal additional planning and shopping.

* * * * *

4\. Non-Goals
=============

The following are outside the first complete product loop:

-   social networking or feeds;

-   comments;

-   user ratings as a core discovery mechanism;

-   payments;

-   restaurant ordering or reservations;

-   sponsored recipes or paid placement;

-   user-generated cooking video;

-   a sophisticated personalised recommendation engine;

-   automated grocery checkout;

-   full grocery commerce;

-   comprehensive nutritional coaching;

-   loyalty/rewards systems;

-   custom-trained AI models;

-   advanced household inventory management;

-   complex restaurant partnership functionality;

-   broad restaurant review functionality.

A grocery list is required for MVP. Full grocery commerce is not.

Video may exist in the underlying domain but is not required to prove the founding loop.

* * * * *

5\. Primary User
================

Situation
---------

The user has ingredients at home but lacks a clear meal idea. They may have a cuisine preference or dietary requirement and want something more interesting than a generic "use up your leftovers" recipe.

Motivation
----------

The user wants to:

-   discover something appealing;

-   make good use of existing ingredients;

-   avoid unnecessary shopping;

-   cook something associated with a restaurant or cuisine they recognise;

-   receive enough guidance to successfully prepare the meal.

Constraints
-----------

The user may:

-   have incomplete or uncertain ingredients;

-   not know exact quantities;

-   have limited cooking experience;

-   have dietary restrictions;

-   be unwilling to buy many additional ingredients;

-   have incomplete access to restaurant/menu information.

Expected behaviour
------------------

The user should be willing to:

-   take or upload a fridge/food image;

-   review and correct detected ingredients;

-   select cuisine and dietary preferences;

-   choose from a small set of recommendations;

-   obtain missing ingredients;

-   follow a guided recipe.

Success for the user
--------------------

The user successfully reaches a credible meal they can cook, while understanding:

-   why the dish was recommended;

-   what they already have;

-   what they are missing;

-   what they need to do next.

* * * * *

6\. Primary User Journey
========================

Step 1 --- Open the application
-----------------------------

The user enters the primary discovery experience.

The application should make the fridge-to-meal journey the obvious starting action rather than requiring the user to understand the underlying recipe or restaurant data model.

* * * * *

Step 2 --- Select cuisine and dietary preferences
-----------------------------------------------

The user chooses:

-   desired cuisine;

-   relevant dietary requirements.

Supported initial cuisine values are aligned with the existing domain:

-   Italian;

-   Indian;

-   Chinese;

-   Japanese;

-   Mexican;

-   American;

-   French;

-   Other.

Supported initial dietary choices are:

-   Vegan;

-   Vegetarian;

-   Pescatarian;

-   Halal;

-   Kosher;

-   Gluten Free;

-   Dairy Free.

Dietary preferences may be saved to the user's profile so they do not need to be re-entered every session.

The product should distinguish between:

-   **saved dietary constraints**, which should persist;

-   **current cuisine intent**, which may change from session to session.

* * * * *

Step 3 --- Capture or upload available food
-----------------------------------------

The user captures a photograph of their fridge or uploads an existing image.

The product sends the image for ingredient recognition.

The system should support the possibility that:

-   several food items are visible;

-   packaging obscures the actual food;

-   quantities are uncertain;

-   an item cannot be confidently identified.

The system must not assume that image recognition is authoritative.

* * * * *

Step 4 --- Extract ingredients
----------------------------

The system returns a structured ingredient list.

Each detected ingredient should have, where practical:

-   normalized ingredient name;

-   availability status;

-   optional quantity;

-   optional unit;

-   recognition confidence or uncertainty.

Example:

| Ingredient | Available | Quantity | Confidence |
| --- | --- | --: | --- |
| Chicken breast | Yes | 2 pieces | High |
| Bell pepper | Yes | 1 | High |
| Garlic | Yes | --- | Medium |
| Soy sauce | Yes | --- | Medium |

Exact quantities are not required where the image cannot support reliable estimation.

* * * * *

Step 5 --- User confirms or corrects ingredients
----------------------------------------------

The user can:

-   remove an incorrectly detected ingredient;

-   add a missed ingredient;

-   change an ingredient name;

-   optionally change quantity;

-   mark an ingredient as unavailable.

The corrected list becomes the authoritative inventory input for the current recommendation request.

This step is mandatory before the system treats image-derived ingredients as the user's confirmed available ingredients.

* * * * *

Step 6 --- Determine candidate dishes
-----------------------------------

The system evaluates the confirmed ingredients against:

-   selected cuisine;

-   dietary requirements;

-   recipe/dish data;

-   restaurant/menu context where available;

-   permitted substitutions.

The system should produce a small, useful set of candidate dishes rather than an unbounded list.

Candidates should be feasible enough that the user can understand the shopping gap.

* * * * *

Step 7 --- Associate dishes with restaurants/menu context
-------------------------------------------------------

Where reliable restaurant/menu information exists, the product associates the candidate with:

-   restaurant;

-   menu item;

-   cuisine;

-   relevant menu information.

The association must indicate the strength of the relationship.

The product must distinguish between:

**Verified fact**

> This restaurant/menu source contains this dish.

and:

**AI inference**

> This recipe is an adaptation inspired by, or similar to, this restaurant-style dish.

AI must not invent a restaurant, menu item, menu price, or restaurant relationship and present it as verified information.

If menu data is unavailable, the product may continue with cuisine/dish matching rather than failing the entire journey.

* * * * *

Step 8 --- Rank results by compatibility
--------------------------------------

Candidate dishes are ranked using a compatibility concept that considers:

-   proportion of required ingredients already available;

-   importance of missing ingredients;

-   valid substitutions;

-   dietary compatibility;

-   cuisine fit;

-   confidence/quality of restaurant or recipe information.

The score exists to help the user understand relative feasibility.

The UI should expose meaningful information rather than relying solely on a numeric score.

For example:

> **High match --- you already have 7 of 9 ingredients.**

rather than only:

> Compatibility: 82%.

* * * * *

Step 9 --- User chooses a dish
----------------------------

The user opens a candidate.

The dish view should show:

-   dish name;

-   description;

-   cuisine;

-   restaurant association where available;

-   restaurant/menu provenance where applicable;

-   compatibility summary;

-   ingredients already available;

-   missing ingredients;

-   substitutions where supported;

-   cooking time;

-   servings where available.

* * * * *

Step 10 --- Generate or retrieve the recipe
-----------------------------------------

The product provides a structured recipe suitable for cooking.

The recipe must contain, where available:

-   name;

-   description;

-   ingredients;

-   quantities;

-   measurement units;

-   ordered preparation steps;

-   cooking time;

-   servings;

-   appliances/tools;

-   substitutions;

-   restaurant association;

-   compatibility information.

The system may retrieve an existing recipe or generate/adapt one.

If generated by AI, that provenance must remain available to the product and should not be represented as an official restaurant recipe unless authoritative evidence exists.

* * * * *

Step 11 --- Generate shopping list
--------------------------------

The system compares recipe requirements with the confirmed available ingredients.

The result is a missing-ingredients list.

Each missing ingredient should be:

-   identifiable;

-   checkable;

-   separated from ingredients already owned.

For MVP, this is a shopping list rather than necessarily a transactional grocery cart.

Where reliable grocery data exists, optional information may include:

-   store;

-   pack size;

-   price;

-   availability.

* * * * *

Step 12 --- Cook the dish
-----------------------

The user enters Cooking Mode.

The product displays:

-   ordered preparation steps;

-   current step;

-   total progress;

-   relevant quantities;

-   preparation/cooking instructions;

-   appliance/preheat guidance where relevant;

-   timers where relevant.

The user can progress through the recipe without having to repeatedly navigate back to the recipe overview.

The primary loop is complete when the user reaches the final preparation step.

* * * * *

7\. Alternative Browsing Journey
================================

The fridge-first journey is primary. A secondary journey should allow discovery to begin from the dish rather than the fridge.

### Flow

**Restaurant/cuisine discovery → dish selection → ingredient comparison → shopping list → recipe → cooking**

The user may:

1.  select a cuisine;

2.  browse relevant dishes/restaurants;

3.  select a dish;

4.  compare the recipe requirements with their known fridge ingredients;

5.  see available versus missing ingredients;

6.  generate a shopping list;

7.  open the recipe;

8.  follow Cooking Mode.

This flow is subordinate to the primary MVP journey.

It should reuse the same recipe, ingredient, restaurant, and compatibility concepts rather than becoming a separate product.

* * * * *

8\. Functional Requirements
===========================

A. User Preferences
-------------------

### A1 --- Cuisine selection

The user must be able to select a cuisine for the current discovery session.

### A2 --- Dietary requirements

The user must be able to select one or more dietary requirements.

### A3 --- Saved dietary preferences

The user should be able to persist dietary preferences to their profile.

### A4 --- Preference enforcement

Dietary requirements must act as eligibility constraints rather than merely recommendation hints where the underlying data is authoritative.

### A5 --- Location

The product may use user location to identify relevant restaurants and stores.

Location must not be required for the core fridge-to-recipe journey if restaurant data is unavailable.

* * * * *

B. Fridge Capture
-----------------

### B1 --- Image capture

The user must be able to capture an image from the application.

### B2 --- Image upload

The user should be able to upload an existing image where platform capabilities permit.

### B3 --- Processing state

The UI must communicate that image processing is in progress.

### B4 --- Ingredient extraction

The system must convert the image into a structured ingredient list.

### B5 --- Uncertainty

The system must be capable of identifying uncertain detections rather than silently asserting them as fact.

### B6 --- User correction

The user must be able to edit the extracted list before recommendation.

### B7 --- Latest known fridge state

The application should retain the user's latest confirmed fridge/ingredient state.

The product should treat this as a working inventory snapshot rather than assuming it is permanently accurate.

* * * * *

C. Ingredient Management
------------------------

### C1 --- Normalized ingredients

Ingredients should map to normalized food/ingredient concepts wherever practical.

### C2 --- Availability

The system must distinguish an ingredient that the user has from one that is required but missing.

### C3 --- Quantities

Quantities should be supported when known.

Exact fridge quantities are optional for MVP where they cannot be reliably determined.

### C4 --- Recipe relationship

The product must be able to compare recipe ingredient requirements against available ingredients.

### C5 --- Substitution

The product may identify acceptable substitutions.

Substitutions must not be treated as equivalent to having the original ingredient unless the product explicitly determines that the substitution is acceptable.

### C6 --- User overrides

User corrections must override uncertain AI detection for the current session.

* * * * *

D. Restaurant Discovery
-----------------------

### D1 --- Location-aware discovery

Where restaurant discovery is enabled, the system should use user location to identify relevant restaurants.

### D2 --- Cuisine filtering

Restaurants should be filterable or discoverable by cuisine.

### D3 --- Useful menu coverage

The system should prefer restaurants for which useful menu information is available.

### D4 --- Incomplete data

A restaurant with incomplete menu information must not be treated as having complete menu coverage.

### D5 --- No restaurant data fallback

If restaurant data is unavailable, the product must still be able to recommend a cuisine-appropriate dish where recipe data supports it.

Restaurant association enhances the experience but must not make the core cooking journey unusable.

* * * * *

E. Menu / Dish Discovery
------------------------

### E1 --- Authoritative menu data

Restaurant/menu facts should originate from an authoritative or identified external source.

### E2 --- Menu item identity

Where possible, a menu item should be represented distinctly from the generated recipe used to recreate it.

### E3 --- Provenance

The system should retain the source/provenance of restaurant and menu information.

### E4 --- AI inference

AI may infer that a recipe resembles a restaurant dish, but this must not be represented as a verified menu fact.

### E5 --- Incomplete menu information

The product may display a restaurant association without asserting menu details that cannot be verified.

* * * * *

F. Recipe Matching
------------------

Candidate recipes must be evaluated against:

1.  available ingredients;

2.  cuisine;

3.  dietary requirements;

4.  restaurant/menu context where available;

5.  permitted substitutions.

Matching should prioritise dishes that can be prepared using a high proportion of the user's available ingredients without requiring disproportionate additional shopping.

Matching should not require exact ingredient quantities when quantity information is unavailable, but quantity-sensitive matching should be used where reliable quantities exist.

* * * * *

G. Compatibility Scoring
------------------------

The product must expose a conceptual compatibility score or equivalent user-facing indicator.

The score should consider:

-   percentage of required ingredients already available;

-   importance of missing ingredients;

-   permitted substitutions;

-   dietary compatibility;

-   cuisine compatibility;

-   quality/confidence of the recipe/menu association where relevant.

The exact mathematical formula is intentionally not fixed at PRD level.

The system should avoid presenting a high score when a missing ingredient is fundamental to the dish.

For example, missing a garnish should not have the same impact as missing the primary protein or defining sauce.

* * * * *

H. Recipe Generation / Adaptation
---------------------------------

The recipe output must be structured.

A recipe should support:

-   name;

-   description;

-   cuisine;

-   ingredients;

-   quantities;

-   measurement units;

-   preparation steps;

-   cooking time;

-   servings;

-   appliances/tools;

-   substitutions;

-   restaurant association;

-   menu item association where applicable;

-   compatibility information;

-   provenance;

-   AI-generated status where applicable.

Generated recipes must be validated before becoming user-visible.

The application must reject or flag malformed outputs rather than attempting to render arbitrary AI text as structured recipe data.

* * * * *

I. Shopping List
----------------

### I1 --- Missing ingredient calculation

The system must compare recipe requirements with the user's confirmed available ingredients.

### I2 --- Missing-only view

The default shopping list should contain ingredients the user needs to obtain.

### I3 --- Checkable items

Each shopping-list item must be individually checkable.

### I4 --- Quantities

Required quantities should be shown where known.

### I5 --- Optional grocery information

Store, availability, price, and package information may be shown where reliable external data exists.

### I6 --- External dependency failure

A missing grocery provider must not prevent the product from generating the basic shopping list.

* * * * *

J. Cooking Mode
---------------

### J1 --- Ordered steps

Recipe preparation steps must be presented in execution order.

### J2 --- Current step

The user must be able to identify the current step.

### J3 --- Progress

The user must be able to progress through the recipe.

### J4 --- Timers

Where a recipe requires timed cooking, the product should support timers.

### J5 --- Appliance guidance

Where relevant, steps should identify appliance or preheating requirements.

### J6 --- Resume

The product should preserve sufficient progress state to allow the user to return to an in-progress recipe.

* * * * *

K. Saved Recipes
----------------

### K1 --- Save

The user should be able to save a recipe.

### K2 --- Retrieve

The user should be able to view previously saved recipes.

### K3 --- Preserve recipe identity

Saving a recipe should preserve the recipe/dish association and not depend on the user's temporary fridge state remaining unchanged.

Saved recipes are secondary to the first successful cooking loop but are supported by the existing domain model.

* * * * *

9\. AI Requirements
===================

AI operations must have explicit boundaries and structured contracts.

AI output must be machine-readable, schema-validatable, and separated from authoritative application data.

AI-1 --- Image → Ingredients
--------------------------

### Input

-   fridge/food image;

-   optionally contextual information supplied by the application.

### Required output

A structured list containing, where possible:

-   ingredient identifier or normalized name;

-   display name;

-   confidence;

-   quantity if inferable;

-   measurement unit if inferable;

-   uncertainty state where appropriate.

### Constraints

-   Do not invent ingredients merely to complete a plausible list.

-   Do not treat uncertain recognition as confirmed inventory.

-   Do not require exact quantity when the image does not support it.

-   Output must conform to the application's expected structure.

-   Invalid output must be rejected or repaired through a controlled process.

The user's correction becomes authoritative for the recommendation session.

* * * * *

AI-2 --- Ingredients → Candidate Dishes
-------------------------------------

### Input

-   confirmed ingredient list;

-   cuisine;

-   dietary preferences;

-   optionally location and restaurant context.

### Required output

Each candidate should contain:

-   dish name;

-   cuisine;

-   required ingredients;

-   available ingredients;

-   missing ingredients;

-   substitution suggestions where valid;

-   dietary compatibility;

-   compatibility rationale;

-   compatibility value;

-   restaurant/menu association if supported;

-   confidence/provenance where relevant.

### Constraints

-   Do not claim a restaurant serves a dish without supporting source data.

-   Do not violate explicit dietary requirements.

-   Do not hide material missing ingredients.

-   Do not treat arbitrary ingredient similarity as proof that a dish is cookable.

-   Output must be structured and validated.

* * * * *

AI-3 --- Menu Context → Recipe Association
----------------------------------------

### Input

-   verified restaurant information;

-   verified menu item information where available;

-   available ingredients;

-   cuisine;

-   dietary requirements.

### Required output

A structured association containing:

-   restaurant;

-   menu item;

-   association type;

-   recipe target;

-   ingredient mapping;

-   confidence;

-   provenance.

### Constraints

The AI may interpret and map information but must not manufacture authoritative restaurant facts.

The system should distinguish at minimum:

1.  **Verified menu association**

2.  **Restaurant-inspired association**

3.  **No reliable association**

* * * * *

AI-4 --- Recipe Generation / Adaptation
-------------------------------------

### Input

-   selected dish;

-   confirmed available ingredients;

-   missing ingredients;

-   dietary requirements;

-   approved substitutions;

-   restaurant/menu context;

-   recipe source, if adapting an existing recipe.

### Required output

A structured recipe containing:

-   name;

-   description;

-   ingredients;

-   quantities;

-   units;

-   ordered steps;

-   cooking time;

-   servings;

-   tools/appliances;

-   substitutions;

-   compatibility information;

-   provenance;

-   generation status.

### Constraints

-   Must comply with dietary requirements.

-   Must use available ingredients where the selected dish expects them.

-   Must explicitly identify missing ingredients.

-   Must not claim to be an official restaurant recipe unless supported by authoritative source data.

-   Must produce executable, ordered steps.

-   Must not omit critical ingredients while describing the dish as complete.

-   Output must be validated against the recipe schema before presentation.

* * * * *

10\. Data Requirements
======================

The existing DBML provides useful domain evidence. The product should be understood through the following conceptual entities rather than treating the current table structure as the final product model.

User
----

Owns preferences, fridge state, saved recipes, and potentially location.

**Product responsibility:** identity and persistent user context.

Fridge
------

Represents a user's current inventory context.

**Product responsibility:** current known food availability.

A user may have one primary fridge for MVP. Multiple fridges are not required to prove the core loop.

Food Item / Ingredient
----------------------

Represents a normalized ingredient or food item.

**Product responsibility:** common language between inventory, recipes, matching, and shopping.

Image
-----

Represents uploaded/captured media, particularly fridge images.

**Product responsibility:** source media for AI recognition and potentially recipe presentation.

Cuisine
-------

Represents culinary classification.

**Product responsibility:** constrain discovery and candidate generation.

Dietary Preference
------------------

Represents user dietary constraints.

**Product responsibility:** constrain candidate eligibility and recipe generation.

Restaurant
----------

Represents a real-world restaurant.

**Product responsibility:** provide local and recognisable restaurant context.

Restaurant Cuisine
------------------

Associates a restaurant with one or more cuisines.

**Product responsibility:** restaurant discovery/filtering.

Menu Item
---------

Represents a dish offered by a restaurant.

This concept is important even though the current DBML partly embeds it within `RecipeItem`.

**Product responsibility:** distinguish a real restaurant offering from the recipe used to recreate or adapt it.

Recipe
------

Represents a cookable dish.

**Product responsibility:** final cooking target.

Recipe Ingredient
-----------------

Associates a recipe with required food items and quantities.

**Product responsibility:** ingredient matching and shopping-list generation.

Preparation Step
----------------

Represents ordered cooking instructions.

**Product responsibility:** Cooking Mode.

Preparation Progress
--------------------

Represents a user's progress through cooking steps.

**Product responsibility:** resume/progress behaviour.

The current schema's progress model appears recipe/step-oriented but does not clearly establish user ownership; this requires later refinement.

Saved Recipe
------------

Associates a user with a recipe they want to retain.

**Product responsibility:** persistent recipe retrieval.

Grocery Store
-------------

Represents a potential place to obtain missing ingredients.

**Product responsibility:** optional grocery discovery.

Grocery Inventory
-----------------

Represents store-level product/price/quantity information.

**Product responsibility:** optional enhancement to the basic shopping list.

AI Generation
-------------

Represents an AI generation event and its input/output.

**Product responsibility:** traceability of generated recipe content.

The current `AIGeneratedRecipe` concept is useful evidence but is narrower than the complete set of AI operations required by the product.

* * * * *

11\. Data Ownership and Provenance
==================================

The product must explicitly separate four information classes.

User-provided data
------------------

Examples:

-   dietary preferences;

-   cuisine choice;

-   fridge images;

-   corrected ingredients;

-   manually entered quantities;

-   saved recipes;

-   cooking progress.

The user's corrections should have higher authority than uncertain AI detection for the current inventory state.

Application-owned data
----------------------

Examples:

-   canonical food items;

-   recipes;

-   preparation steps;

-   cuisine classifications;

-   dietary classifications;

-   compatibility rules;

-   product state.

External authoritative data
---------------------------

Examples:

-   restaurant identities;

-   restaurant locations;

-   verified menu items;

-   grocery-store information;

-   external pricing/inventory.

The product must preserve provenance sufficiently to distinguish external facts from generated content.

AI-generated/inferred data
--------------------------

Examples:

-   image recognition;

-   candidate dishes;

-   ingredient mappings;

-   substitution proposals;

-   recipe adaptations;

-   inferred restaurant similarity.

AI-generated information must not silently become authoritative external fact.

* * * * *

12\. Deterministic Logic vs AI Behaviour
========================================

The boundary should be explicit.

AI is appropriate for
---------------------

-   visual ingredient recognition;

-   semantic ingredient normalization when ambiguous;

-   generating candidate dishes;

-   interpreting menu information;

-   mapping dishes to recipe concepts;

-   proposing substitutions;

-   generating/adapting recipes.

Deterministic application logic is responsible for
--------------------------------------------------

-   storing user state;

-   accepting user corrections;

-   enforcing explicit product constraints;

-   comparing known ingredient identities;

-   determining missing ingredients from structured data;

-   calculating or applying compatibility rules;

-   tracking cooking progress;

-   creating shopping lists;

-   validating AI output structure;

-   enforcing provenance;

-   rendering authoritative restaurant information;

-   handling errors and state transitions.

AI should not be the system of record.

* * * * *

13\. Existing Schema Alignment
==============================

Clearly Supported
-----------------

The current DBML conceptually supports:

-   users;

-   user location;

-   fridges;

-   food items;

-   food availability;

-   images;

-   cuisines;

-   restaurants;

-   restaurant cuisines;

-   recipes;

-   recipe ingredients;

-   ingredient quantities;

-   measurement units;

-   preparation steps;

-   cooking progress;

-   dietary preferences;

-   saved recipes;

-   grocery stores;

-   grocery inventory;

-   grocery pricing;

-   AI-generated recipes;

-   video references.

These provide a credible domain foundation for the founding loop.

* * * * *

Partially Supported
-------------------

### Ingredient recognition

`FoodItems` can represent recognised ingredients, but the model does not clearly distinguish:

-   manually entered ingredients;

-   AI-detected ingredients;

-   confirmed ingredients;

-   uncertain detections.

Confidence and recognition provenance are not explicit.

### Fridge quantities

Recipe ingredients support quantities, but `FoodItems` do not currently provide a quantity for the user's available inventory.

This may be acceptable for the first version if matching primarily operates on ingredient presence.

### Restaurant/menu modelling

`Recipe.restaurant_id` and `RecipeItem.menu_item_name` imply a restaurant relationship, but the semantics are ambiguous.

It is not clear whether:

-   `Recipe` means an official restaurant recipe;

-   `RecipeItem` means a menu item;

-   or the two represent an AI-generated recreation of a menu item.

The product requires this distinction even if the schema is not immediately changed.

### Compatibility score

No explicit compatibility concept is represented.

This is a product requirement and will likely need to become an explicit domain concept later.

### AI generation

`AIGeneratedRecipe` stores prompt and output, but the founding loop requires multiple AI operations, not just recipe generation.

The current model therefore partially supports AI traceability but does not clearly model:

-   ingredient recognition;

-   candidate generation;

-   menu mapping;

-   confidence;

-   structured output;

-   provenance.

### Shopping list

Grocery stores and inventories exist, but there is no clearly represented user-specific shopping list generated from a recipe.

The current schema therefore supports the potential data source but not necessarily the complete product behaviour.

### Cooking progress

`PreparationStepsProgress` supports progress conceptually, but it is not explicitly linked to a user or cooking session.

This is sufficient as evidence of the intended domain but requires later clarification.

### Recipe metadata

`RecipeMetadata` is generic and may support additional metadata, but it is not clear whether it is intended for cuisine, dietary suitability, restaurant provenance, nutrition, or other attributes.

The next schema stage should avoid using generic metadata as a substitute for concepts that have clear product semantics.

* * * * *

Missing or Ambiguous
--------------------

The following are important enough to flag for the next specification stage.

### 1\. Explicit menu item entity

A real restaurant menu item is materially different from a generated recipe.

The current schema contains `RecipeItem.menu_item_name`, but the product needs a clear conceptual distinction.

### 2\. Provenance

Restaurant/menu information needs a source and authority classification.

### 3\. AI operation type

AI generation currently focuses on recipes. The product requires multiple AI operations.

### 4\. AI confidence

Ingredient recognition and AI associations may require confidence or uncertainty.

### 5\. Compatibility result

The product needs to preserve or calculate the compatibility of a candidate against a user's ingredient state.

### 6\. Ingredient quantity in fridge

The product can function without exact quantities, but the domain does not currently model them.

### 7\. Shopping list

A user-specific missing-ingredient list is not clearly represented.

### 8\. Cooking session/user progress

Cooking progress should belong to a user/session rather than only a preparation step.

### 9\. Substitutions

The product requires substitutions conceptually, but the schema does not explicitly represent them.

### 10\. Recipe appliances/tools

The recipe output can require tools and appliances, but these are not clearly represented.

### 11\. Source recipe vs generated adaptation

The domain does not currently distinguish an authoritative recipe from an AI-generated adaptation.

### 12\. Dietary applicability at recipe level

User dietary preferences exist, but explicit recipe/dish dietary metadata is not clearly represented.

These are gaps to resolve during the next specification stage; they are not instructions to redesign the database immediately.

* * * * *

14\. External Dependencies
==========================

| Dependency | MVP Need | Required Capability | Fallback |
| --- | --- | --- | --- |
| AI inference | Essential | Image recognition, candidate generation, recipe generation | Graceful error; allow manual ingredient entry where possible |
| Image/media storage | Essential | Store/process fridge images | Manual ingredient entry if image capture/storage fails |
| Restaurant discovery | Important | Identify relevant local restaurants | Continue with cuisine/dish discovery |
| Menu data | Important but potentially constraining | Verify restaurant/menu associations | Show cuisine/dish association without claiming menu facts |
| Location | Important for local restaurant discovery | Determine relevant geographic context | Allow non-local/cuisine-based discovery |
| Grocery data | Not essential for core MVP | Store/price/availability information | Generate a simple missing-ingredient checklist |
| Nutrition data | Not essential for core MVP | Additional nutritional/dietary information | Use product dietary metadata |
| Recipe/content source | Essential | Base recipes and/or structured dish data | AI-generated/adapted recipe with explicit provenance |

No specific external vendor is required by the PRD. Provider selection belongs to the system/reality specification.

* * * * *

15\. States and Failure Cases
=============================

The application must always leave the user in a defined state.

Image processing
----------------

**State:** Processing

The UI communicates that the image is being analysed.

The user should not be shown a partially constructed ingredient list as if it were final.

* * * * *

Ingredients detected
--------------------

**State:** Review required

The system presents the detected ingredients and allows correction.

The user can continue only after confirming or otherwise accepting the list.

* * * * *

No ingredients detected
-----------------------

**State:** Recovery

The user receives a clear explanation and can:

-   retry the image;

-   take another image;

-   manually add ingredients.

The application should not attempt to fabricate a recommendation from an empty detection.

* * * * *

Partial/uncertain detection
---------------------------

**State:** Review with warnings

The user can see that some items may be uncertain and correct them.

The system should continue where sufficient information exists.

* * * * *

No matching recipe
------------------

**State:** No suitable match

The product should explain that no sufficiently compatible dish was found.

Where possible, it may offer:

-   relaxing the cuisine preference;

-   adding another ingredient;

-   browsing dishes;

-   allowing more substitutions.

It must not manufacture a low-quality match merely to produce a result.

* * * * *

Restaurant data unavailable
---------------------------

**State:** Dish discovery continues

The user can still receive a recipe based on cuisine and ingredient compatibility.

The UI must not imply that a restaurant relationship was verified.

* * * * *

Menu unavailable
----------------

**State:** Restaurant association limited

The product may show the restaurant if the restaurant itself is authoritative, but should not display unverified menu claims.

The dish may be described as restaurant-inspired where appropriate.

* * * * *

AI generation failed
--------------------

**State:** Generation failed

The application should communicate that the requested recipe could not be generated.

Where a stored structured recipe exists, it should be used instead.

Where no fallback exists, the user should be able to return to candidate selection rather than being trapped.

* * * * *

AI output invalid
-----------------

**State:** Validation failure

Malformed output must not be rendered directly.

The application should retry or use a known-safe fallback.

* * * * *

Partial recipe generated
------------------------

**State:** Incomplete

The application should not present a partial recipe as complete.

If sufficient structured data exists, the product may show the incomplete state and allow regeneration; otherwise it should return the user to dish selection.

* * * * *

Location denied
---------------

**State:** Location unavailable

The product should allow the primary recipe loop to continue without location.

Restaurant discovery can fall back to:

-   cuisine-level discovery;

-   user-entered location;

-   previously known location if valid and permitted.

* * * * *

Grocery data unavailable
------------------------

**State:** Basic shopping list

The user can still generate a list of missing ingredients.

Store, price, and availability information are omitted.

* * * * *

Recipe ready
------------

**State:** Ready to cook

The product presents the complete recipe and enables Cooking Mode.

* * * * *

Cooking in progress
-------------------

**State:** Active cooking session

The current step and progress are preserved.

* * * * *

16\. Acceptance Criteria
========================

The first end-to-end version is acceptable when the following are observable.

### AC1 --- Fridge submission

A user can submit a fridge/food image.

### AC2 --- Editable ingredient extraction

The system returns a structured ingredient list that the user can edit.

### AC3 --- Manual correction

The user can add, remove, or correct ingredients before matching.

### AC4 --- Cuisine/preferences

The user can select a cuisine and dietary requirements.

### AC5 --- Candidate generation

Given suitable ingredient and recipe data, the system returns at least one compatible dish.

### AC6 --- Compatibility explanation

Each result communicates ingredient compatibility in a user-understandable way.

### AC7 --- Available vs missing

The selected dish clearly distinguishes ingredients the user already has from ingredients they need.

### AC8 --- Restaurant association

Where verified data exists, the dish can be associated with a restaurant/menu item.

Where it does not exist, the product does not fabricate the association.

### AC9 --- Structured recipe

The user can open a recipe containing structured ingredients and ordered preparation steps.

### AC10 --- Shopping list

The user can generate a checkable shopping list from missing ingredients.

### AC11 --- Cooking Mode

The user can progress through ordered cooking steps.

### AC12 --- Failure recovery

Image, AI, restaurant, location, or grocery failures result in a defined user-facing state and available recovery path.

### AC13 --- Provenance

The product does not present AI-generated restaurant/menu information as verified external fact.

### AC14 --- Complete loop

A user can progress from:

**fridge image → confirmed ingredients → compatible dish → missing ingredients → recipe → cooking steps**

without requiring a separate product or manually reconstructing the workflow.

* * * * *

17\. MVP Boundary
=================

The smallest credible MVP is a **single complete vertical loop**.

It should support:

1.  user profile;

2.  cuisine selection;

3.  dietary preferences;

4.  fridge image capture/upload;

5.  AI ingredient extraction;

6.  editable ingredient confirmation;

7.  structured recipe/dish candidate generation;

8.  compatibility evaluation;

9.  restaurant/menu association where reliable data exists;

10. available-versus-missing ingredient display;

11. missing-ingredient shopping list;

12. structured recipe;

13. basic Cooking Mode;

14. saved recipes only if this does not materially delay the core loop.

The MVP does **not** require:

-   sophisticated grocery commerce;

-   live grocery checkout;

-   comprehensive restaurant coverage;

-   perfect restaurant menu coverage;

-   exact fridge quantity recognition;

-   social features;

-   ratings;

-   payments;

-   personalised recommendation models;

-   video cooking content.

### MVP success statement

> **A user photographs ingredients they already own and is guided to a credible restaurant-style dish that uses those ingredients and requires only a reasonable amount of additional shopping.**

This is the principal product proof.

* * * * *

18\. Future Extensions
======================

The following may be considered after the founding loop is validated:

### Discovery and community

-   social sharing;

-   ratings;

-   comments;

-   user-generated images;

-   user-generated recipes;

-   community collections.

### Cooking

-   video cooking guides;

-   richer timers;

-   hands-free voice cooking;

-   adaptive cooking assistance.

### Personalisation

-   personalised recommendation engine;

-   historical taste preferences;

-   household-specific inventory;

-   learned substitution preferences.

### Commercial

-   sponsored recipes;

-   restaurant partnerships;

-   restaurant ordering;

-   grocery checkout;

-   payments;

-   loyalty programmes;

-   retailer partnerships.

### AI

-   custom-trained AI models;

-   specialised ingredient recognition;

-   personalised recipe generation;

-   richer multimodal cooking assistance.

These should remain subordinate to proving that the core fridge-to-meal loop generates real user value.

* * * * *

19\. Open Questions
===================

Only questions capable of materially affecting the product or subsequent system specification are retained here.

OQ1 --- Where does reliable restaurant/menu data come from?
---------------------------------------------------------

The product depends on distinguishing real restaurant/menu facts from AI-generated associations.

The source, coverage, freshness, licensing, and availability of menu data may materially affect how strongly restaurant association can feature in MVP.

* * * * *

OQ2 --- What does "restaurant-style" mean operationally?
------------------------------------------------------

There are at least three possible interpretations:

1.  an exact restaurant menu dish;

2.  a recreation/approximation of a known restaurant dish;

3.  a cuisine-appropriate dish that resembles restaurant food.

This affects data requirements, AI behaviour, provenance, and user expectations.

* * * * *

OQ3 --- Are recipes recreations, approximations, or official recipes?
-------------------------------------------------------------------

The product must establish whether an AI-generated recipe may be described as the recipe for a restaurant dish or only as an adaptation/inspiration.

This is a material trust and data-provenance decision.

* * * * *

OQ4 --- How much substitution is acceptable?
------------------------------------------

The product needs a definition of when an ingredient substitution still results in a credible recommendation.

This directly affects compatibility scoring and AI generation.

* * * * *

OQ5 --- How much ingredient quantity detection is required?
---------------------------------------------------------

The MVP can operate primarily on ingredient presence, but quantity-aware matching may materially improve feasibility.

The decision affects image recognition requirements and inventory modelling.

* * * * *

OQ6 --- Is restaurant association essential to MVP?
-------------------------------------------------

If reliable menu data cannot be obtained at sufficient coverage, the product needs a decision on whether cuisine/dish matching can temporarily serve as the restaurant-style experience.

The core loop should not be blocked unnecessarily by incomplete restaurant data unless restaurant association is proven to be the primary value driver.

* * * * *

OQ7 --- What constitutes a "minimal additional shopping" recommendation?
----------------------------------------------------------------------

A dish requiring one missing ingredient may be preferable to one requiring five, but the number alone is insufficient because ingredients have different importance.

The product needs a conceptual threshold for recommendation feasibility.

* * * * *

OQ8 --- What data should be cached?
---------------------------------

Restaurant/menu and grocery information may change frequently, while canonical recipes and food definitions may be relatively stable.

Caching policy affects freshness, external dependency load, and correctness.

* * * * *

OQ9 --- What AI-generated information should be persisted?
--------------------------------------------------------

The product needs to distinguish between:

-   transient candidate generation;

-   persisted generated recipes;

-   user-confirmed AI output;

-   externally sourced facts.

This affects data retention, provenance, reproducibility, and future recommendation behaviour.

* * * * *

OQ10 --- How authoritative are dietary classifications?
-----------------------------------------------------

The product needs a sufficiently reliable source of dietary suitability before presenting a recipe as compliant with a user's dietary requirement.

AI-only classification should not be treated as authoritative for constraints where incorrect information could materially affect the user's choice.

* * * * *

20\. Product-to-System Boundary
===============================

The next Spec Descent stage should translate this PRD into system/reality specifications around the following capabilities:

1.  **User and preference management**

2.  **Image capture/storage**

3.  **Ingredient recognition**

4.  **Ingredient normalization and confirmation**

5.  **Recipe and ingredient knowledge**

6.  **Restaurant and menu discovery**

7.  **Dish-to-recipe association**

8.  **Compatibility evaluation**

9.  **AI generation and validation**

10. **Missing-ingredient calculation**

11. **Shopping-list state**

12. **Recipe presentation**

13. **Cooking session/progress**

14. **Provenance and source management**

15. **Failure/retry handling**

The implementation sequence should be derived from the primary dependency chain:

**Confirmed ingredients → recipe/dish knowledge → compatibility → recipe → shopping list → cooking**

Restaurant and grocery integrations should enhance this chain without making the basic cooking loop structurally dependent on them.

* * * * *

21\. Final Product Requirement
==============================

The product must make the following experience possible and trustworthy:

> **"Show me what I have, tell me what restaurant-style dish I can make from it, tell me what little I need to buy, and then help me cook it."**

Every major product decision should be evaluated against whether it improves or obstructs that loop.

The system should optimise for **credible feasibility**, not maximum recipe volume. A small number of well-matched dishes is more valuable than a large set of generic recommendations.

The product's fundamental contract is therefore:

**Given confirmed ingredients, cuisine intent, and dietary constraints, produce a credible, explainable, cookable dish recommendation; distinguish facts from inference; identify the remaining ingredients; and guide the user through preparation.**
