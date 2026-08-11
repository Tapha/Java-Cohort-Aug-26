Restaurant TABLE

PRIMARY KEY - INT - ID(INDEX)

VARCHAR(255) - Restaurant Name

VARCHAR(50) - longitude

VARCHAR(50) - latitude

Restaurant Cuisine TABLE

PRIMARY KEY - INT - ID(INDEX)

FOREIGN KEY - Restaurant ID [Link to Restaurant table]

ENUM(50) - Cuisines

Recipe TABLE \/ (pointer for restaurant & recipes)

PRIMARY KEY - INT - ID(INDEX)

FOREIGN KEY - Restaurant ID  [Link to Restaurant table]

VARCHAR(255) - Name of recipe

LONGTEXT() - Description of recipe (covers metadata)

VARCHAR(255) - Originator of recipe

- originator

- links to restaurant

- links to food items

- checklist of steps

- [COL]price of recipe at xyz restaurant (average) - could be an API call

- [COL]Time it takes to cook

- [COL]servings

RECIPE METADATA TABLE

PRIMARY KEY - INT - ID(INDEX)

FOREIGN KEY - Recipe ID [Link to Recipe table]

ENUM(40) - recipe metadata (servings)

Recipe item TABLE

PRIMARY KEY -

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

PRIMARY KEY - INT - User ID

- cooking progress of recipe(s)

- saved recipes (

- current location

Dietary Preferences TABLE 

PRIMARY KEY - INT - ID [ONE ID to MANY Choices]

FOREIGN KEY - User ID [Link to User table]

ENUM(10) - Dietary choice

Images Table 

- link to CDN

- link to Recipes

Video Table /

- link to CDN

- link to Recipes
