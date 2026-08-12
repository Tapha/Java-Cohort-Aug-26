Founding Spec
-------------

### 1\. Product Definition

The product is a mobile application that connects what users currently have in their fridges with restaurant-style dishes they love, transforming raw household ingredients and dietary preferences into realistic, guided home-cooking experiences paired with restaurant inspiration.

### 2\. Problem / Wedge

People frequently look into their fridges, see a collection of random ingredients, and feel uninspired or order expensive takeaway. The wedge is bridging the gap between leftover kitchen ingredients and craveable restaurant-quality meals by turning a simple photo of a fridge into a tailored cooking plan.

### 3\. Target User

The initial user is a busy urban home-cook or young professional who wants to eat better, save money, and reduce food waste, but lacks the inspiration or knowledge to combine everyday fridge items into exciting, restaurant-style meals.

### 4\. Core Product Invariant

The system must always translate available user ingredients and preferences into feasible, structured cooking instructions and restaurant-style recipe matches without breaking data integrity or generating unachievable culinary requirements.

### 5\. Primary User Loop

1.  Open the application and specify desired cuisine style or dietary preferences.

2.  Capture a photo of the fridge interior.

3.  Review and refine the AI-extracted ingredient list.

4.  Receive candidate restaurant-style recipes matched against available ingredients.

5.  Select a recipe to view ingredient breakdowns, instructions, and missing item requirements.

6.  Generate a targeted shopping cart for any missing grocery items.

### 6\. Product Inputs

-   User profile settings, location, and dietary preferences.

-   Visual imagery (photographs of fridge contents).

-   Manual ingredient additions, corrections, and quantity tweaks.

-   Cuisine style selections.

### 7\. Product Outputs

-   Extracted and structured inventory of available fridge items.

-   Candidate restaurant-style recipes with matched origin restaurants.

-   Step-by-step cooking preparation flows.

-   Missing ingredient shopping lists with store inventory or pricing references.

### 8\. Core Domain Concepts

-   **User & Fridge:** Represents the user profile, location, and their physical inventory space containing individual food items.

-   **FoodItem:** Individual ingredients tracked inside the user's fridge or mapped to store inventories, linked to availability states.

-   **Recipe:** A culinary blueprint tied to a restaurant origin, containing descriptive metadata, costs, and preparation steps.

-   **Restaurant:** The physical establishment or culinary brand that inspires the dish style and provides a point of comparison for the user.

-   **DietaryPreferences & CuisineType:** Constraints and categorization models used to filter and guide recipe generation or discovery.

-   **GroceryStoreInventory:** External or store-specific mapping of food items to local availability and pricing for shopping list creation.

### 9\. AI Responsibilities

-   **Image-to-Text Extraction:** Input: Fridge photo $\rightarrow$ Output: Structured list of recognized food items and confidence scores.

-   **Recipe Candidate Matching:** Input: Ingredient list + Cuisine style + Dietary choice $\rightarrow$ Output: Structured list of compatible restaurant-style recipes.

-   **Ingredient Compatibility Scoring:** Input: Available ingredients vs. Recipe requirements $\rightarrow$ Output: Feasibility score and missing item identification.

-   **Recipe Adaptation/Generation:** Input: Base constraints $\rightarrow$ Output: Coherently structured cooking steps and ingredient quantities matching system architecture constraints.

### 10\. Non-AI Responsibilities

-   Deterministic user account management and preference saving.

-   Storage and retrieval of structured relational entities (recipes, steps, inventory tables).

-   Cart management, location tracking, and distance calculations.

-   Progress tracking through preparation steps.

### 11\. External Capabilities

-   **Location Services:** Capability to determine user coordinates for local store proximity.

-   **Restaurant & Menu Discovery:** Access to external restaurant metadata, regional cuisines, and menu styles.

-   **Grocery/Store Inventory & Pricing:** Capability to retrieve store data and item costs for shopping cart generation.

-   **Media / CDN Services:** Capability to host and serve fridge photographs and recipe imagery efficiently.

### 12\. First-Version Scope

-   Fridge photo capture and manual ingredient list editing.

-   Basic cuisine/dietary preference filtering.

-   AI-driven recipe matching linked to restaurant inspirations.

-   Step-by-step recipe viewing and shopping cart creation for missing ingredients.

### 13\. Explicit Non-Goals

-   Social sharing, reviews, or community recipe feeds.

-   In-app payment processing or direct grocery delivery checkout.

-   Sponsorships, advertisements, or restaurant reservation booking.

-   Advanced personalized machine learning recommendation engines beyond basic prompt-matching.

### 14\. Assumptions Requiring Validation

-   Accuracy and reliability of visual fridge ingredient recognition across varying lighting and clutter levels.

-   Feasibility of mapping home cooking recipes accurately to real-world restaurant menu items.

-   User willingness to manually adjust and correct AI-extracted ingredient lists.

-   Consistency and accuracy of local grocery store inventory and pricing data.

### 15\. Success Condition

"The founding product works" when a user can snap a photo of a messy fridge, verify the extracted ingredients, receive realistic restaurant-style recipe matches, and successfully view a complete cooking guide with a shopping list for missing items in under two minutes.

### 16\. Open Product Questions

-   How should the system handle missing quantities or ambiguous ingredient volumes extracted from fridge photos?

-   Should restaurant matching be purely conceptual, or tightly bound to verified local restaurant menus?
