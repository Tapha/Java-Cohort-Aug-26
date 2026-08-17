TICKET:


Cooked Homepage — Initial Vertical Slice
User Story: As a Cooked user, I want a simple homepage with useful lists so I can quickly find something to cook.

Scope: Search bar at top. Horizontal modules: Featured, Recent, Recommended, Popular Restaurants. Bottom navigation: Home, Search, Fridge, Saved, Profile. Backend endpoint GET /api/homepage returning modules (type, title, items[]). Items include id, title, imageUrl (+ optional fields).

Out of Scope: Recommendation engine, restaurant association, fridge image processing, recipe generation, grocery list, cooking mode.

Tech Notes: Create HomepageController + HomepageService. DTOs: HomepageModule, HomepageItem. Use placeholder data. Modules returned as a list, not fixed fields.

Acceptance Criteria:  
AC1 — GET /api/homepage returns modules[].
AC2 — Modules contain type, title, items[].
AC3 — Items contain id, title, imageUrl.
AC4 — Empty modules return items: [].
AC5 — Adding/removing modules does not break frontend.
AC6 — Homepage shows search bar.
AC7 — Modules render horizontally.
AC8 — Bottom nav works.
AC9 — Module order does not matter.
AC10 — Empty modules render gracefully.
AC11 — New module types auto‑render.
AC12 — Removing modules does not break layout.
