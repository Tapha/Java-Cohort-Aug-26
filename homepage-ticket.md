# Create Frontend Homepage

## 1. Motivation & Business Impact

The homepage is the main entry point into Cooked. It should make it easy for users to discover relevant food, return to recipes they have interacted with, and navigate the app.

We believe a simple, content-led homepage will reduce the effort required to decide what to cook.

Success means users can quickly find a relevant recipe or dish without unnecessary navigation.

---

## 2. User Story

**As a Cooked user,**  
I want to see relevant recipes and food content when I open the app,  
**so that** I can quickly decide what to cook or explore.

### High-Level Acceptance Criteria

- Users can search from the homepage.
- Users can discover recipes and restaurants.
- Users can access previously viewed or saved recipes.
- Users can see personalised recommendations.
- Users can navigate to other areas of the app.

---

## 3. User Interaction / Design

The homepage should include:

- **Search** — at the top of the page.
- **Featured Recipes** — curated recipe selection.
- **Recently Viewed / Saved** — relevant user content.
- **For You** — personalised recipe recommendations.
- **Popular Restaurants** — popular restaurant/dish content.
- **Bottom Navigation** — persistent navigation with the current section highlighted.

The experience should be simple, easy to scan, and require minimal decision-making.

### Dietary Requirements

During initial sign-up/setup, users should have a one-time opportunity to select dietary requirements. These preferences should be saved and made available for future personalisation.

---

## 4. Functional Requirements

- Build the homepage in React.
- Use reusable components for recipe and restaurant collections.
- The same components should support different datasets, e.g. Featured, Saved, Recently Viewed and Personalised.
- Use mock/seeded data where backend functionality is unavailable.
- Keep presentation components separate from recommendation/ranking logic.
- Ensure the homepage is responsive.

Example:

```text
HomePage
├── Search
├── ContentSection
│   └── RecipeList
│       └── RecipeCard
├── ContentSection
│   └── RestaurantList
│       └── RestaurantCard
└── BottomNavigation

---

## Definition of Done

- Homepage is implemented and accessible as the primary landing page.
- All required homepage sections are displayed using reusable components.
- Mock data is used where backend functionality is not yet available.
- Core interactions and navigation work as expected.
- The homepage is responsive across supported screen sizes.
- The implementation is ready for integration with future backend functionality.
