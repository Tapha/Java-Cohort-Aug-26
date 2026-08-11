Database Design

User Journey 

Unique fields 

Relationships between entities(fields)

Dependencies

Restaurant TABLE

- tags - cuisine - type of restaurant (vegan, vegetarian) - could be an API call

Recipe TABLE \/ (pointer for restaurant & recipes)

- originator

- links to restaurant

- links to food items

- checklist of steps

- [COL]price of recipe at xyz restaurant (average) - could be an API call

- [COL]Time it takes to cook

- [COL]servings

Recipe item TABLE

- cost of menu items or use API(Menus API) or

- Recipe items -> Food items [list comparison of ingredients] pivot table with unique ID

--- Example

Restaurant - Nandos

Recipes -  1/4 Chicken, Butterfly Burger, 

Restaurant -> Recipe -> 

Food items + (Food Items)Food Item Cost + Preparation Items +  + Nutrition

---

AI Output

[Name of recipe] + [Ingredients] + [Instructions]

---

Food Items TABLE (populates over time)

 - Items from the fridge (Fridge Table)

 - Items from the recipe generated

 -

grocery store TABLE

 - items you need to buy from store

 - location of store

 -

PIVOT TABLE grocery store, Food Items 

- many to many relationship

Fridge TABLE

- User ID

- Fridge ID

- Food Items

Food Item Cost TABLE

- cost of individual ingredients

[Food Items]Nutrition TABLE /

 - kcal, and all that stuff

 - macros

(Could be an API - don't need to maintain that data)

Preparation Steps TABLE

- ID

- recipe step content

- recipe ID

User TABLE

- cooking progress of recipe(s)

- saved recipes (

- current location

Images Table 

- link to CDN

- link to Recipes

Video Table /

- link to CDN

- link to Recipes

###Future

Social TABLE

Ratings TABLE

payments TABLE

###

----

User Stories:

Primary Loop (User journeys) - (not having to go to the shop - save time)

1\. Pickup phone -> 

2\. uber eats like -> (search restaurants or directory or featured restaurants) 

(loads images, user data)

3\. pick cuisine

3\. Take a picture of fridge (load/update current Ingredients data) ->

---

command center - API to get recipe from ingredients data,

then ai filters available recipes

---

(current user) location data + google maps api(map view) -> restaurants in radius -> google places api to find businesses / restaurants finders -> get menu[hardest part] (filter restaurants that don't have a direct menu) hopefully there is an API

---

MenusApi to get food name + description (for AI to create recipe)

---

4\. search for restaurants & recipes based on current ingredients (filters down results) ->

####(future functionality) x. choose menu items they want to cook & 'checkout' -> 

5\. list of items to buy from grocery store ->

6\. Recipe instructions shown on app -> (loads specific recipe)

Alt primary loop (browsable mode / if you don't mind buying ingredients)

1\. Pickup phone -> 

2\. uber eats like -> (search restaurants or directory or featured restaurants) 

(loads images, user data) 

3\. picks a restaurant they want to cook -> (loads recipes / food items (ingredients, prices, 

4\. choose items they want to cook & 'checkout' -> 

5\. Shopping list gets generated -> (loads fridge items vs need to buy)

7\. Recipe instructions -> (loads specific recipe)

1\. get phone location data -> google places api (radius) to get list name restaurants in a 5-10++++ mile radius (to limit number of restaurants to show you)

2\. extract a list of all the restaurants from google places api to send to MenusAPI

3\. MenusAPI returns all restaurants with menu's we can use, and items they sell

4\. send that to AI to get a recipe

5.

get fridge ingredients, get ai to get approximate dishes, then do menu search based on location

get fridge ingredients, allow for substitutions (ingredients)

icon for cuisines - user input after picture is taken

###### Confirmed user journey

User opens app

pick cuisine you want to have / dietary requirements(in settings instead?)

take picture of fridge - extract ingredients as an list

[backend] using the list of ingredients, Ask AI which recipes fit based on selected cuisine

AI - Input (cuisine + current ingredients list ) + Output must be the same, it needs to be structured, so that it is coherent for out system architecture

AI returns recipes and (popular)restaurant it fits with

(store data, so in the future you can use a cache for future searches)

(redundancy - AI + cache)

(store data for custom AI model)

recipe score based on compatibility with current ingredients)

Recipe View & how to cook, timers, preheat

-- create a shopping list from AI results (recipes based on what you have in pantry, 

-checkbox for ingredients (what you have vs need to buy), already ticked off based on picture you took 

-which appliances to use (oven, air fryer, grill, pan)

######

Secondary Loop

1.

Tertiary Loop

a. Video instructions -> (loads video of recipe or cooking techniques)

b. voting/rating of recipes - including images of what it looks like when made

c. featured recipes / sponsored recipes ($$$)

d. recommendation engine

e. social sharing buttons

f. review - comments
