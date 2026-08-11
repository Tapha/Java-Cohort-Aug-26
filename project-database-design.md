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

- links to food items

- checklist of steps

- [COL]price of recipe at xyz restaurant (average) - could be an API call

- [COL]servings

RECIPE METADATA TABLE

PRIMARY KEY - INT - ID(INDEX)

FOREIGN KEY - Recipe ID [Link to Recipe table]

ENUM(40) - recipe metadata (e.g. servings, time to cook)

Recipe item TABLE

PRIMARY KEY - INT - Recipe Item ID(INDEX)

FOREIGN KEY - Recipe ID [Link to Recipe]

VARCHAR(255) - Menu Item Name

FLOAT() - Cost of menu item

Quesadilla [1 to many]

    \/          \/          \/              \/

1 x Wrap, 100g chicken, 30g pico de gallo, 25g cheese

get ai to use metric system for proportions

Recipe Food Items TABLE

PRIMARY KEY - INT - ID(INDEX)

FOREIGN KEY - INT - Food item ID

ENUM(50) - different units of measurements (grams, liter, tbsp)

INT() - measurement unit (5 x)

VARCHAR(255) - measurement text ("2 of ")

(Picture of fridge populates this table & other things  \/)

Food Items TABLE

PRIMARY KEY - INT - ID(INDEX)

FOREIGN KEY - Image ID

VARCHAR(255) - Name of ingredient

FOREIGN KEY - INT - Fridge ID

BOOLEAN(True/False) - Available (in fridge)

Grocery store TABLE

PRIMARY KEY - INT - ID(INDEX)

VARCHAR(255) - Name of store

VARCHAR(50) - longitude

VARCHAR(50) - latitude

(PIVOT TABLE grocery store, Food Items - BASED on Available in fridge)

(- many to many relationship)

Grocery store inventory TABLE

PRIMARY KEY - INT - ID(INDEX)

FOREIGN KEY - INT - Grocery Store ID

FOREIGN KEY - INT - Food Item ID

FLOAT(2dp) - Price

ENUM(50) - different units of measurements (grams, liter, tbsp)

INT() - measurement unit (5 x)

VARCHAR(255) - measurement text ("2 of ")

Fridge TABLE

PRIMARY KEY - INT - Fridge ID(INDEX)

VARCHAR(255) - Fridge Name

Preparation Steps TABLE

PRIMARY KEY - INT - ID

PRIMARY KEY - INT - Recipe ID(INDEX)

VARCHAR(255) - Recipe Step

User TABLE

PRIMARY KEY - INT - ID

VARCHAR(255) - Name

VARCHAR(50) - Rough/Saved longitude

VARCHAR(50) - Rough/Saved latitude

Preparation Steps Progress TABLE

PRIMARY KEY - INT - ID

FOREIGN KEY - Preparation Steps ID

INT() - how many steps 

INT() - current step (can be used to show a percentage)

Saved Recipes TABLE

PRIMARY KEY - INT - ID

FOREIGN KEY - Recipe ID

FOREIGN KEY - USER ID

Dietary Preferences TABLE 

PRIMARY KEY - INT - ID [ONE ID to MANY Choices]

FOREIGN KEY - User ID [Link to User table]

ENUM(10) - Dietary choice

Images Table

PRIMARY KEY - INT - ID

VARCHAR(255) - URL (CDN)

Video Table

PRIMARY KEY - INT - ID

VARCHAR(255) - URL (CDN)
