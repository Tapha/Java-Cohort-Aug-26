# Homepage UI


## Description

### What needs to change?

Build the homepage as the main entry point for discovering restaurant-style meals users can make with their available ingredients.

The homepage should include:
- Search
- Featured recipes
- Recently viewed / saved recipes
- Personalised recommendations
- Popular restaurant dishes
- Bottom navigation

Homepage lists should be modular so they can use different data sources, filters, and ranking rules.

### Why does it matter?

The homepage should quickly answer:

> "What can I make with what I have?"

It should reduce decision-making and surface relevant dishes based on ingredients, cuisine, dietary requirements, and user behaviour.

## UI Requirements

- **Search:** Search icon at the top of the page.
- **Featured:** Small selection of featured recipes.
- **Recently Viewed / Saved:** Show relevant dishes the user has viewed or saved.
- **For You:** Personalised recipe recommendations.
- **Popular:** Popular restaurant/menu-inspired dishes. Only show restaurant information when reliable data exists.
- **Bottom Navigation:** Fixed navigation with the current section highlighted.
- **Modular Lists:** Reusable list component supporting different data sources, filters, ranking, and item limits.

## Interactive States

- Cards and buttons have default, hover, focus, and pressed states.
- Search opens when selected and can be closed.
- Bottom navigation updates the active state when selected.
- Empty sections are hidden or show an appropriate empty state.

## Accessibility

- All interactive elements are keyboard accessible.
- Icon-only controls have ARIA labels.
- Visible focus states are provided.
- Images have appropriate alt text.
- Colour is not the only way to communicate state.

## Acceptance Criteria

- [ ] Homepage displays all required sections.
- [ ] Search is accessible from the top of the homepage.
- [ ] Users can open recipes from homepage cards.
- [ ] Recently viewed and saved content only appears when relevant.
- [ ] Personalised and popular lists can be populated independently.
- [ ] Homepage lists use a reusable/modular component.
- [ ] Lists support different data sources and constraints.
- [ ] Bottom navigation remains visible and shows the active section.
- [ ] Empty or unavailable sections do not break the homepage.
- [ ] Interactive elements meet accessibility requirements.
