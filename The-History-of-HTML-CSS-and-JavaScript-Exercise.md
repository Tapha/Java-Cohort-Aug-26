# 🌐 Exercise — The History of HTML, CSS, and JavaScript
## How the Web Learned Structure, Style, and Behaviour

## Goal

By the end of this exercise, you should be able to explain how the web evolved from connected documents into interactive applications.

The core story is:

```text
HTML = structure
CSS = presentation
JavaScript = behaviour
```

But the deeper story is:

```text
Connected documents
        ↓
Structured pages
        ↓
Styled pages
        ↓
Interactive pages
        ↓
Dynamic web applications
        ↓
Component-based frontends
        ↓
Mobile and full-stack applications
```

You are not just learning three technologies.

You are learning how the frontend became an application runtime.

---

# 🧠 Part 1 — The Original Web Problem

Complete the sentence:

```text
The web did not begin as an app platform.

It began as an __________________ system.
#test
```

## Questions

1. What problem was the early web originally trying to solve?
2. Why were links important?
3. What did URLs provide?
4. What role did HTTP play?
5. What role did the browser play?

Complete:

```text
URL = __________________
HTTP = __________________
HTML = __________________
Browser = __________________
```

---

# 🕸️ Part 2 — The First Web Loop

Complete the flow:

```text
User enters URL
        ↓
Browser sends __________________
        ↓
Server returns __________________
        ↓
Browser __________________ the document
```

## Questions

1. Which side is the client?
2. Which side is the server?
3. What is the response body in the earliest web model?
4. How does this connect to the backend request/response model you already know?

---

# 🧾 Part 3 — HTML: Structure

Look at this HTML:

```html
<h1>Fridge2Meal</h1>
<p>Take a picture of your fridge and get meal suggestions.</p>
<a href="/meals">View meals</a>
```

For each element, explain what structural meaning it gives:

```text
<h1> = ?
<p> = ?
<a> = ?
```

## Questions

1. What does HTML mainly describe?
2. Does HTML mainly control colour and spacing?
3. Does HTML mainly control application behaviour?
4. What does “markup” mean in HTML?
5. Why was HTML important to the growth of the web?

Complete:

```text
HTML = meaning and __________________ of the page
```

---

# 🎨 Part 4 — Why CSS Appeared

Look at this old style of HTML:

```html
<font color="red">
  <center>
    <h1>Welcome</h1>
  </center>
</font>
```

## Questions

1. What two responsibilities are mixed here?
2. Why does mixing structure and appearance become difficult to maintain?
3. What pressure led to CSS?

Complete:

```text
HTML should describe __________________.
CSS should describe __________________.
```

---

# 🎨 Part 5 — CSS: Presentation

HTML:

```html
<h1 class="title">Fridge2Meal</h1>
```

CSS:

```css
.title {
  color: green;
  font-size: 32px;
  margin-bottom: 16px;
}
```

## Questions

1. What does the HTML decide?
2. What does the CSS decide?
3. Which file would you change if you wanted the heading to become blue?
4. Which file would you change if the heading should become a paragraph instead?

Complete:

```text
HTML = __________________
CSS = __________________
```

---

# 🧱 Part 6 — Separation of Responsibilities

Match each responsibility to its home.

| Responsibility | Home |
|---|---|
| page structure | ? |
| page appearance | ? |
| page behaviour | ? |
| HTTP boundary | ? |
| business logic | ? |
| persistence | ? |
| API data shape | ? |

Use:

```text
HTML
CSS
JavaScript
Controller
Service
Repository
DTO
```

Explain:

```text
Different concerns should have different homes.
```

How does this connect frontend architecture to backend architecture?

---

# ⚡ Part 7 — Why JavaScript Appeared

HTML gave the web structure.

CSS gave the web appearance.

But what was still missing?

Complete:

```text
The browser needed __________________.
```

List four things users wanted web pages to do dynamically.

---

# ⚡ Part 8 — JavaScript: Behaviour

Look at this:

```html
<button id="suggestMealButton">Suggest Meal</button>
<p id="result"></p>
```

```javascript
const button = document.getElementById("suggestMealButton");
const result = document.getElementById("result");

button.addEventListener("click", function () {
  result.textContent = "Tomato Pasta";
});
```

## Questions

1. Which technology creates the button?
2. Which technology could style the button?
3. Which technology makes something happen when the button is clicked?
4. What event is being listened for?
5. What changes after the event?

Complete:

```text
HTML = what __________________
CSS = how it __________________
JavaScript = what __________________
```

---

# 🧠 Part 9 — The Frontend Triangle

Complete:

```text
HTML
        ↓
__________________

CSS
        ↓
__________________

JavaScript
        ↓
__________________
```

Then create your own analogy for all three.

---

# 🌳 Part 10 — The DOM

DOM means:

```text
__________________ Object Model
```

Given:

```html
<body>
  <h1>Fridge2Meal</h1>
  <button>Suggest Meal</button>
</body>
```

Draw the DOM tree beginning with:

```text
document
└── ?
```

## Questions

1. Why does the browser create a DOM?
2. What does JavaScript gain by having access to it?
3. What happens here?

```javascript
document.querySelector("h1").textContent = "Meal Ready";
```

Complete:

```text
JavaScript can __________________ the page after it has loaded.
```

---

# 🔄 Part 11 — Static vs Dynamic Pages

Complete the old web flow:

```text
Click link
        ↓
Browser requests __________________
        ↓
Server returns __________________
        ↓
Browser __________________
```

Complete the dynamic web flow:

```text
Click button
        ↓
JavaScript sends __________________
        ↓
Server returns __________________
        ↓
Page updates without __________________
```

Why was this shift important?

---

# 🌐 Part 12 — AJAX

AJAX means:

```text
A__________________ JavaScript and XML
```

Complete:

```text
JavaScript can request data from the server
without __________________ the whole page.
```

Modern examples:

```javascript
const response = await fetch("/api/meals");
const meals = await response.json();
```

```javascript
const response = await axios.get("/api/meals");
```

## Questions

1. What does `fetch()` do?
2. What does Axios do?
3. Why does this matter for Fridge2Meal?
4. Does AJAX require XML today?

---

# 📦 Part 13 — JSON

JSON means:

```text
__________________ Object Notation
```

Look at:

```json
{
  "title": "Tomato Pasta",
  "usedIngredients": ["tomato", "pasta", "cheese"],
  "steps": ["Boil pasta", "Cook tomatoes", "Mix together"],
  "timeEstimateMinutes": 20
}
```

## Questions

1. Is JSON page structure?
2. Is JSON mainly used to carry data?
3. What could a frontend do with the JSON above?
4. Why is JSON useful between frontend and backend?

Complete:

```text
HTML = __________________ structure
JSON = __________________ structure
```

---

# 🧠 Part 14 — Browser as Runtime

Originally:

```text
Browser = document reader
```

Over time:

```text
Browser = __________________ runtime
```

List five capabilities modern browsers gained.

## Reflection

Why did frontend development become a serious engineering discipline?

---

# ⚙️ Part 15 — Node.js

Complete:

```text
Originally JavaScript mainly ran in __________________.
Node.js allowed JavaScript to run __________________.
```

List four things JavaScript can do through Node.js.

## Questions

1. Why was Node.js a major change?
2. What is npm?
3. Why do tools such as Vite and Expo depend on the wider JavaScript ecosystem?

---

# 📱 Part 16 — React Native / Expo

Complete:

```text
Components = __________________
Styles = __________________
JavaScript / TypeScript = __________________
```

For Fridge2Meal:

```text
Camera screen = __________________
Styling = __________________
Image capture + Axios POST = __________________
```

Why is the HTML/CSS/JavaScript triangle still useful even when building a mobile app?

---

# 📸 Part 17 — Fridge2Meal Full Loop

Complete the frontend flow:

```text
UI structure
        ↓
__________________ capture
        ↓
FormData
        ↓
Axios __________________ POST
```

Complete the backend flow:

```text
Controller receives image
        ↓
__________________ coordinates meal suggestion
        ↓
Vision port defines __________________
        ↓
Adapter handles __________________
        ↓
Response DTO returns __________________
```

Then:

```text
JavaScript sends request
        ↓
Backend returns __________________
        ↓
Frontend __________________ response
```

---

# 🎛️ Part 18 — Events

List six frontend events.

Look at:

```javascript
button.addEventListener("click", handleClick);
```

Explain:

```text
When __________________ happens,
run __________________.
```

Map the Fridge2Meal camera flow:

```text
User taps camera button
        ↓
__________________ event
        ↓
image is created
        ↓
__________________ request is sent
        ↓
response updates __________________
```

---

# 🔁 Part 19 — State

Complete:

```text
State = what the interface currently __________________.
```

Classify:

| Item | State? |
|---|---|
| selected image | ? |
| loading true/false | ? |
| meal response | ? |
| error message | ? |
| current screen | ? |
| Java source code | ? |
| database schema file | ? |

Complete:

```text
Before request:
loading = ?
meal = ?

During request:
loading = ?

After success:
loading = ?
meal = ?

After failure:
loading = ?
error = ?
```

---

# 🧪 Part 20 — Testing Frontend Behaviour

| Behaviour | How would you test it? |
|---|---|
| button triggers request | ? |
| request contains correct image | ? |
| loading state appears | ? |
| response displays correctly | ? |
| error state appears | ? |

Complete:

```text
Behaviour should be __________________.
```

How is this the same idea as backend testing?

---

# ⚠️ Part 21 — Common Confusions

Mark TRUE or FALSE.

1. HTML is mainly a programming language for business logic.
2. CSS controls layout and visual communication.
3. JavaScript only exists for button click code.
4. JSON and HTML are the same thing.
5. Frontend and backend meet through requests and responses.
6. JavaScript can manage state.
7. Node.js lets JavaScript run outside the browser.
8. React Native keeps the same basic structure/style/behaviour separation.

Correct every false statement.

---

# 🧬 Part 22 — Historical Stages

Put these in order:

```text
Component-Based Frontends
Interactive Pages
Connected Documents
Full-Stack and Mobile Expansion
Dynamic Web Applications
Styled Documents
```

Then add the main technology or idea beside each stage.

---

# 🗺️ Part 23 — Full-Stack Responsibility Map

Complete:

```text
Frontend asks
        ↓
Backend __________________
        ↓
Backend __________________
        ↓
Frontend __________________
```

Sort these into frontend or backend:

```text
state
controller
events
service
repository
DOM
DTO
database
styles
Axios
entities
components
```

---

# 🧠 Part 24 — Architecture Comparison

Complete the table:

| Frontend | Backend | Shared Principle |
|---|---|---|
| HTML | ? | responsibility has a home |
| CSS | ? | separation |
| JavaScript | ? | behaviour / coordination |
| JSON | DTO | boundary shape |
| browser event | HTTP request | external trigger |
| frontend state | ? | remembered runtime information |

There may be more than one reasonable answer.

Explain your choices.

---

# 🚀 Part 25 — Build Challenge

Create a tiny Fridge2Meal web interface using plain HTML, CSS, and JavaScript.

## HTML

Create:

```text
Page title
Ingredient input
Suggest Meal button
Loading area
Result area
Error area
```

## CSS

Style:

```text
Page layout
Button
Input
Result card
Error state
```

## JavaScript

Implement:

```text
Read ingredient input
Listen for button click
Set loading state
Send or simulate API request
Receive result
Display meal result
Handle failure
```

If the backend endpoint exists, connect to it.

If it does not, simulate:

```javascript
{
  title: "Tomato Pasta",
  timeEstimateMinutes: 20
}
```

---

# 🌟 Stretch Challenge — Explain the Entire Evolution

Without looking back at the guide, explain:

```text
Connected documents
        ↓
HTML
        ↓
CSS
        ↓
JavaScript
        ↓
DOM
        ↓
Events
        ↓
AJAX
        ↓
JSON APIs
        ↓
Stateful applications
        ↓
React / React Native
        ↓
Fridge2Meal
```

For every arrow, answer:

```text
What problem appeared?
What technology or idea solved it?
```

---

# 🚀 Final Compression

Complete from memory:

```text
HTML = __________________
CSS = __________________
JavaScript = __________________
DOM = __________________
Events = __________________
State = __________________
AJAX/fetch/axios = __________________
JSON = __________________
Browser = __________________
Node.js = __________________
React Native / Expo = __________________
```

---

# 🌌 Final Reflection

1. Why did the web begin with HTML rather than JavaScript?
2. Why did CSS need to become separate from HTML?
3. Why did JavaScript become necessary?
4. Why was the DOM important?
5. What changed when pages could request data without full reloads?
6. Why did JSON become central to modern applications?
7. Why is state important in frontend applications?
8. How does React Native preserve the structure/style/behaviour pattern?
9. How does Fridge2Meal connect frontend and backend?
10. Explain:

```text
The web began as connected documents
and evolved into an application runtime.
```
