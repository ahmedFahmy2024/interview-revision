# modern CSS & CSS3 interview Questions & Answers

Click ⭐ if you like the project. Follow for more updates.

---

# CSS & CSS3 Interview Questions 🎨

| #   | Question                                                                                |
| --- | --------------------------------------------------------------------------------------- |
| 1   | What is CSS and what is it used for?                                                    |
| 2   | What is the difference between CSS and CSS3?                                            |
| 3   | What is the CSS Box Model?                                                              |
| 4   | What is the difference between `box-sizing: content-box` and `box-sizing: border-box`?  |
| 5   | What is CSS specificity and how is it calculated?                                       |
| 6   | What is the difference between `class` selectors and `id` selectors in CSS?             |
| 7   | What are pseudo-classes and pseudo-elements?                                            |
| 8   | What is the difference between `display: none` and `visibility: hidden`?                |
| 9   | What is the difference between `position: relative`, `absolute`, `fixed`, and `sticky`? |
| 10  | What is a CSS stacking context and how does `z-index` work?                             |
| 11  | What is Flexbox and when should you use it?                                             |
| 12  | What is CSS Grid and how does it differ from Flexbox?                                   |
| 13  | What are CSS custom properties (variables)?                                             |
| 14  | What is the difference between `em`, `rem`, `px`, `vw`, and `vh` units?                 |
| 15  | What are CSS transitions?                                                               |
| 16  | What are CSS animations and `@keyframes`?                                               |
| 17  | What is the difference between `transition` and `animation` in CSS?                     |
| 18  | What are CSS media queries?                                                             |
| 19  | What is the CSS cascade and how does inheritance work?                                  |
| 20  | What are CSS cascade layers (`@layer`)?                                                 |
| 21  | What are container queries in CSS?                                                      |
| 22  | What is the `clamp()` function in CSS?                                                  |
| 23  | What is `calc()` in CSS?                                                                |
| 24  | What are CSS combinators?                                                               |
| 25  | What is the difference between `inline`, `block`, and `inline-block`?                   |
| 26  | What is `float` and how does `clearfix` work?                                           |
| 27  | What is the difference between `margin` and `padding`?                                  |
| 28  | What is `overflow` in CSS?                                                              |
| 29  | What are CSS transforms?                                                                |
| 30  | What is `will-change` in CSS?                                                           |
| 31  | What is the `:root` pseudo-class?                                                       |
| 32  | What are CSS logical properties?                                                        |
| 33  | What is `object-fit` and `object-position`?                                             |
| 34  | What is `aspect-ratio` in CSS?                                                          |
| 35  | What is the `gap` property in CSS?                                                      |
| 36  | What is `backdrop-filter` in CSS?                                                       |
| 37  | What are CSS `@supports` rules?                                                         |
| 38  | What is the `color-mix()` function in CSS?                                              |
| 39  | What is a CSS reset and why is it used?                                                 |
| 40  | What is the difference between `min-width` and `max-width` in media queries?            |

---

# CSS & CSS3 Questions & Answers

---

### 1. What is CSS and what is it used for?

CSS (Cascading Style Sheets) is a stylesheet language used to describe the presentation of HTML documents. It controls layout, colors, fonts, spacing, animations, and responsiveness — separating content (HTML) from design (CSS).

Example:

```css
body {
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
  color: #333;
  margin: 0;
  padding: 0;
}

h1 {
  color: steelblue;
  font-size: 2rem;
}
```

---

### 2. What is the difference between CSS and CSS3?

CSS3 is the latest major version of CSS, split into independent modules for faster development. It introduced many powerful features over older CSS:

- **New selectors**: `:nth-child()`, `:not()`, `:is()`, `:where()`
- **Box model**: `box-sizing`, `border-radius`, `box-shadow`
- **Visual effects**: `text-shadow`, `opacity`, `gradients`, `filters`
- **Layouts**: Flexbox, CSS Grid
- **Animations**: `@keyframes`, `transition`, `transform`
- **Responsive**: media queries, `clamp()`, `min()`, `max()`
- **Variables**: custom properties (`--var`)
- **Modern**: container queries, `@layer`, `color-mix()`

Example:

```css
/* CSS3 features */
.card {
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  background: linear-gradient(135deg, #667eea, #764ba2);
  transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-4px);
}
```

---

### 3. What is the CSS Box Model?

Every HTML element is a rectangular box. The CSS Box Model describes the layers that make up that box from inside out:

- **Content**: The actual text or image
- **Padding**: Space between content and border
- **Border**: The border around the padding
- **Margin**: Space outside the border, separating elements

Example:

```css
.box {
  width: 200px; /* content width */
  padding: 20px; /* inner spacing */
  border: 2px solid steelblue;
  margin: 16px; /* outer spacing */
}

/* Total rendered width = 200 + 20*2 + 2*2 = 244px (content-box default) */
```

---

### 4. What is the difference between `box-sizing: content-box` and `box-sizing: border-box`?

- `content-box` (default): `width` and `height` apply to the **content only**. Padding and border are added on top, making the element larger than declared.
- `border-box`: `width` and `height` include **content + padding + border**. The element stays exactly the declared size.

Example:

```css
/* content-box: total width = 200 + 40 + 4 = 244px */
.content-box {
  box-sizing: content-box;
  width: 200px;
  padding: 20px;
  border: 2px solid black;
}

/* border-box: total width = exactly 200px */
.border-box {
  box-sizing: border-box;
  width: 200px;
  padding: 20px;
  border: 2px solid black;
}

/* Best practice: apply globally */
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

---

### 5. What is CSS specificity and how is it calculated?

Specificity determines which CSS rule wins when multiple rules target the same element. It is calculated as a score with four levels:

| Selector type                  | Score   |
| ------------------------------ | ------- |
| Inline styles                  | 1-0-0-0 |
| ID selectors (`#id`)           | 0-1-0-0 |
| Class, attribute, pseudo-class | 0-0-1-0 |
| Element, pseudo-element        | 0-0-0-1 |

Higher specificity always wins. Equal specificity: the last rule in the stylesheet wins.

Example:

```css
p {
  color: black;
} /* 0-0-0-1 */
.text {
  color: blue;
} /* 0-0-1-0 */
#title {
  color: red;
} /* 0-1-0-0 */
/* inline style="color: green"   1-0-0-0  */

/* Result: inline > #id > .class > element */
```

---

### 6. What is the difference between `class` selectors and `id` selectors in CSS?

- `id` (`#name`): Targets a **single unique** element. Higher specificity. Should be used once per page.
- `class` (`.name`): Targets **multiple elements**. Lower specificity. Reusable across the page.

Example:

```css
/* ID - unique, high specificity */
#main-header {
  font-size: 2.5rem;
  color: #1a1a2e;
}

/* Class - reusable, lower specificity */
.card {
  border-radius: 8px;
  padding: 16px;
  background: white;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
```

---

### 7. What are pseudo-classes and pseudo-elements?

- **Pseudo-classes** (`:`) target elements based on their **state or position** in the DOM. They select existing elements.
- **Pseudo-elements** (`::`) target a **specific part** of an element, creating virtual elements that don't exist in the HTML.

Example:

```css
/* Pseudo-classes */
a:hover {
  color: steelblue;
}
input:focus {
  outline: 2px solid blue;
}
li:nth-child(odd) {
  background: #f5f5f5;
}
p:not(.special) {
  color: gray;
}

/* Pseudo-elements */
p::first-line {
  font-weight: bold;
}
h2::before {
  content: "→ ";
  color: steelblue;
}
.quote::after {
  content: " ❞";
}
::selection {
  background: yellow;
  color: black;
}
```

---

### 8. What is the difference between `display: none` and `visibility: hidden`?

- `display: none`: Removes the element **completely from the layout**. It takes up no space. Other elements reflow to fill the gap.
- `visibility: hidden`: Hides the element visually but it **still occupies its space** in the layout. No reflow occurs.

Example:

```css
.removed {
  display: none;
  /* Element gone from layout — surrounding elements shift */
}

.invisible {
  visibility: hidden;
  /* Element invisible but space is preserved */
}

/* Also useful: opacity: 0 — invisible but interactive, space preserved */
.transparent {
  opacity: 0;
}
```

---

### 9. What is the difference between `position: relative`, `absolute`, `fixed`, and `sticky`?

- `static` (default): Normal document flow. `top`/`left` have no effect.
- `relative`: Positioned relative to its **own normal position**. Still in document flow.
- `absolute`: Removed from flow. Positioned relative to the nearest **positioned ancestor** (non-static).
- `fixed`: Removed from flow. Positioned relative to the **viewport**. Stays on screen when scrolling.
- `sticky`: Stays in flow until a scroll threshold is hit, then acts like `fixed`.

Example:

```css
.parent {
  position: relative; /* establishes positioning context */
}

.tooltip {
  position: absolute;
  top: -30px;
  left: 50%;
  transform: translateX(-50%);
}

.navbar {
  position: sticky;
  top: 0;
  z-index: 100;
  background: white;
}

.cookie-banner {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
}
```

---

### 10. What is a CSS stacking context and how does `z-index` work?

`z-index` controls the **stacking order** of overlapping elements along the Z axis. Higher values appear on top. It only works on **positioned elements** (non-static) or elements with certain CSS properties.

A **stacking context** is a self-contained layer. Elements inside it are stacked relative to each other, not to the rest of the page. New stacking contexts are created by `position + z-index`, `opacity < 1`, `transform`, `filter`, `isolation: isolate`, etc.

Example:

```css
.overlay {
  position: fixed;
  z-index: 1000;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
}

.modal {
  position: fixed;
  z-index: 1001; /* above overlay */
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

/* Create isolated stacking context */
.card {
  isolation: isolate;
}
```

---

### 11. What is Flexbox and when should you use it?

Flexbox (Flexible Box Layout) is a one-dimensional layout model for arranging items in a row or column. It excels at distributing space and aligning items within a container, even when their sizes are unknown.

Use Flexbox for: navigation bars, centering content, card rows, toolbars, and any single-axis layout.

Example:

```css
.container {
  display: flex;
  flex-direction: row; /* main axis: horizontal */
  justify-content: space-between; /* align along main axis */
  align-items: center; /* align along cross axis */
  gap: 16px;
  flex-wrap: wrap; /* allow items to wrap */
}

.item {
  flex: 1 1 200px; /* grow | shrink | basis */
}

/* Perfect centering */
.centered {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
```

---

### 12. What is CSS Grid and how does it differ from Flexbox?

CSS Grid is a two-dimensional layout system for rows **and** columns simultaneously. It's ideal for full-page layouts and complex component structures.

| Feature        | Flexbox             | Grid                    |
| -------------- | ------------------- | ----------------------- |
| Dimensions     | 1D (row or column)  | 2D (rows and columns)   |
| Best for       | Component alignment | Page/section layouts    |
| Item placement | Flow-based          | Explicit cell placement |
| Content-driven | Yes                 | Structure-driven        |

Example:

```css
.layout {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header  header"
    "sidebar main"
    "footer  footer";
  min-height: 100vh;
  gap: 16px;
}

header {
  grid-area: header;
}
.sidebar {
  grid-area: sidebar;
}
main {
  grid-area: main;
}
footer {
  grid-area: footer;
}

/* Responsive grid */
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
}
```

---

### 13. What are CSS custom properties (variables)?

CSS custom properties (also called CSS variables) are reusable values defined with `--` prefix and accessed with `var()`. They cascade and inherit like regular CSS properties, and can be updated with JavaScript.

Example:

```css
/* Define on :root for global scope */
:root {
  --color-primary: #667eea;
  --color-text: #1a1a2e;
  --spacing-md: 16px;
  --border-radius: 8px;
  --font-size-base: 1rem;
}

.button {
  background: var(--color-primary);
  padding: var(--spacing-md);
  border-radius: var(--border-radius);
  font-size: var(--font-size-base);
}

/* Dark mode override */
@media (prefers-color-scheme: dark) {
  :root {
    --color-text: #eee;
    --color-primary: #a86bff;
  }
}

/* Update via JavaScript */
/* document.documentElement.style.setProperty('--color-primary', '#ff6b6b'); */
```

---

### 14. What is the difference between `em`, `rem`, `px`, `vw`, and `vh` units?

| Unit  | Relative to                | Use case                         |
| ----- | -------------------------- | -------------------------------- |
| `px`  | Fixed pixels               | Borders, shadows, precise sizing |
| `em`  | Parent element's font-size | Component-level scaling          |
| `rem` | Root (`<html>`) font-size  | Consistent global scaling        |
| `vw`  | 1% of viewport width       | Full-width layouts, fluid type   |
| `vh`  | 1% of viewport height      | Full-height sections             |
| `%`   | Parent element             | Fluid widths                     |

Example:

```css
:root {
  font-size: 16px; /* 1rem = 16px */
}

.container {
  width: 90vw; /* 90% of viewport width */
  min-height: 100vh; /* full viewport height */
  padding: 1.5rem; /* 24px, scales with root font */
}

.card-title {
  font-size: 1.25rem; /* 20px — scales globally */
}

.icon {
  width: 1em; /* scales with parent font-size */
  height: 1em;
}

/* Fluid typography with clamp */
h1 {
  font-size: clamp(1.5rem, 4vw, 3rem);
}
```

---

### 15. What are CSS transitions?

CSS transitions smoothly animate a property change from one value to another over a specified duration. They are triggered by state changes like `:hover`, `:focus`, or class toggling via JavaScript.

Syntax: `transition: property duration timing-function delay`

Example:

```css
.button {
  background: steelblue;
  color: white;
  padding: 12px 24px;
  border-radius: 6px;
  /* Transition multiple properties */
  transition:
    background 0.3s ease,
    transform 0.2s ease,
    box-shadow 0.3s ease;
}

.button:hover {
  background: #1a5276;
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
}

.button:active {
  transform: translateY(0);
}
```

---

### 16. What are CSS animations and `@keyframes`?

CSS animations allow elements to animate automatically without a trigger. You define the animation steps with `@keyframes` and apply it with the `animation` property.

Example:

```css
/* Define keyframes */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes pulse {
  0%,
  100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
}

/* Apply animation */
.hero-title {
  animation: fadeInUp 0.6s ease forwards;
}

.badge {
  animation: pulse 2s ease-in-out infinite;
}

/* Full shorthand: name duration timing-function delay iteration-count direction fill-mode */
.spinner {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
```

---

### 17. What is the difference between `transition` and `animation` in CSS?

| Feature    | `transition`                                  | `animation`                    |
| ---------- | --------------------------------------------- | ------------------------------ |
| Trigger    | Requires a state change (hover, focus, class) | Runs automatically             |
| Keyframes  | Only start and end state                      | Full control with `@keyframes` |
| Looping    | Cannot loop                                   | Can loop with `infinite`       |
| Direction  | Always A → B                                  | Can alternate, reverse         |
| Complexity | Simple two-state changes                      | Complex multi-step sequences   |

Example:

```css
/* Transition: triggered by hover */
.card {
  transform: scale(1);
  transition: transform 0.3s ease;
}
.card:hover {
  transform: scale(1.03);
}

/* Animation: runs on load, no trigger needed */
.loader {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
```

---

### 18. What are CSS media queries?

Media queries apply CSS rules conditionally based on device characteristics like viewport width, height, orientation, or user preferences. They are the foundation of responsive design.

Example:

```css
/* Mobile first: base styles for small screens */
.container {
  padding: 16px;
  font-size: 1rem;
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    padding: 32px;
    font-size: 1.1rem;
  }
}

/* Desktop and up */
@media (min-width: 1200px) {
  .container {
    max-width: 1140px;
    margin: 0 auto;
  }
}

/* User preference: dark mode */
@media (prefers-color-scheme: dark) {
  body {
    background: #121212;
    color: #eee;
  }
}

/* User preference: reduced motion */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

### 19. What is the CSS cascade and how does inheritance work?

The **cascade** is the algorithm CSS uses to resolve conflicts when multiple rules target the same element. It considers (in order of priority):

1. Origin & importance (`!important`, user agent, author, user)
2. Specificity (inline > ID > class > element)
3. Order of appearance (last rule wins on equal specificity)

**Inheritance**: Some CSS properties (like `color`, `font-family`, `font-size`) are automatically inherited by child elements from their parents. Others (like `margin`, `padding`, `border`) are not.

Example:

```css
/* Inheritance */
body {
  font-family: Arial, sans-serif; /* inherited by all children */
  color: #333; /* inherited by all children */
}

p {
  /* font-family and color are inherited from body */
  margin: 0; /* margin is NOT inherited — must be set explicitly */
}

/* Force inheritance */
.child {
  border: inherit;
}

/* Reset inherited value */
.reset {
  color: initial; /* browser default */
  font-size: unset; /* inherits if inheritable, else initial */
}
```

---

### 20. What are CSS cascade layers (`@layer`)?

Cascade layers (`@layer`) let you organize CSS into named layers with explicit priority order. Later-declared layers win over earlier ones at equal specificity — giving you predictable control without fighting specificity wars or overusing `!important`.

Example:

```css
/* Declare layer order — utilities always win */
@layer base, components, utilities;

@layer base {
  h2 {
    font-size: 1.5rem;
    color: black;
  }
  a {
    color: blue;
  }
}

@layer components {
  .card {
    border-radius: 8px;
    padding: 16px;
  }
  .btn {
    padding: 8px 16px;
    background: steelblue;
  }
}

@layer utilities {
  .mt-4 {
    margin-top: 1rem;
  }
  .text-center {
    text-align: center;
  }
  /* These win over base and components regardless of specificity */
}

/* Unlayered styles always beat layered styles */
.override {
  color: red;
}
```

---

### 21. What are container queries in CSS?

Container queries allow elements to apply styles based on the **size of their parent container** rather than the viewport. This makes components truly self-contained and reusable in any context.

Example:

```css
/* Step 1: Define a containment context */
.card-wrapper {
  container-type: inline-size;
  container-name: card;
}

/* Step 2: Query the container */
@container card (min-width: 400px) {
  .card {
    display: flex;
    flex-direction: row;
    gap: 16px;
  }

  .card__image {
    width: 120px;
    flex-shrink: 0;
  }
}

@container card (max-width: 399px) {
  .card {
    display: flex;
    flex-direction: column;
  }
}
```

---

### 22. What is the `clamp()` function in CSS?

`clamp(min, preferred, max)` restricts a value between a minimum and maximum, with a preferred fluid value in between. It's perfect for fluid typography and spacing that scales smoothly with the viewport.

Example:

```css
/* Font size: never smaller than 1rem, never larger than 3rem */
h1 {
  font-size: clamp(1rem, 4vw, 3rem);
}

/* Padding: fluid between 16px and 48px */
.section {
  padding: clamp(1rem, 5vw, 3rem);
}

/* Width: fluid between 300px and 800px */
.container {
  width: clamp(300px, 80%, 800px);
  margin: 0 auto;
}
```

---

### 23. What is `calc()` in CSS?

`calc()` lets you perform mathematical calculations directly in CSS, mixing different units. It's evaluated at render time by the browser.

Example:

```css
.sidebar {
  width: 250px;
}

.main-content {
  /* Subtract sidebar width and gap from full width */
  width: calc(100% - 250px - 24px);
}

.full-bleed {
  /* Extend beyond parent padding */
  margin-left: calc(-1 * var(--spacing));
  width: calc(100% + 2 * var(--spacing));
}

/* Combine with custom properties */
:root {
  --header-height: 64px;
}

.page-body {
  min-height: calc(100vh - var(--header-height));
}
```

---

### 24. What are CSS combinators?

Combinators define the relationship between selectors. There are four types:

| Combinator       | Syntax  | Selects                        |
| ---------------- | ------- | ------------------------------ |
| Descendant       | `A B`   | All `B` inside `A` (any depth) |
| Child            | `A > B` | Direct children `B` of `A`     |
| Adjacent sibling | `A + B` | `B` immediately after `A`      |
| General sibling  | `A ~ B` | All `B` siblings after `A`     |

Example:

```css
/* Descendant: all <p> inside .article */
.article p {
  line-height: 1.7;
}

/* Child: only direct <li> of <ul> */
ul > li {
  list-style: disc;
}

/* Adjacent sibling: <p> right after <h2> */
h2 + p {
  font-size: 1.1rem;
  color: #555;
}

/* General sibling: all <p> after .intro */
.intro ~ p {
  margin-top: 1rem;
}
```

---

### 25. What is the difference between `inline`, `block`, and `inline-block`?

- `block`: Takes up the **full width** available. Starts on a new line. Respects `width`, `height`, `margin`, `padding` on all sides.
- `inline`: Takes up only as much **width as its content**. Does not start on a new line. Ignores `width`/`height` and top/bottom `margin`.
- `inline-block`: Flows inline like text but **respects width, height, and all margins** like a block element.

Example:

```css
/* Block */
div,
p,
h1,
section {
  display: block;
}

/* Inline */
span,
a,
strong,
em {
  display: inline;
}

/* Inline-block: useful for buttons, badges */
.badge {
  display: inline-block;
  width: 80px;
  height: 24px;
  padding: 2px 8px;
  background: steelblue;
  color: white;
  border-radius: 12px;
  text-align: center;
}
```

---

### 26. What is `float` and how does `clearfix` work?

`float` was originally designed for wrapping text around images. It removes an element from normal flow and pushes it to the left or right. Parent containers collapse when all children are floated.

`clearfix` is a technique to make a parent container contain its floated children.

Example:

```css
/* Float an image */
.article img {
  float: left;
  margin: 0 16px 16px 0;
}

/* Modern clearfix using ::after pseudo-element */
.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

/* Modern alternative: use overflow */
.parent {
  overflow: hidden; /* also contains floats */
}

/* Best practice today: use Flexbox or Grid instead of float for layout */
```

---

### 27. What is the difference between `margin` and `padding`?

- `padding`: Space **inside** the element, between the content and the border. Has the element's background color.
- `margin`: Space **outside** the element, between the border and neighboring elements. Always transparent.

Key difference: vertical margins between adjacent block elements **collapse** (the larger margin wins). Padding never collapses.

Example:

```css
.card {
  padding: 24px; /* inner breathing room — shows background */
  margin: 16px auto; /* outer spacing — transparent */
  background: white;
  border: 1px solid #ddd;
}

/* Margin collapse example */
.box-a {
  margin-bottom: 32px;
}
.box-b {
  margin-top: 16px;
}
/* Actual gap between them = 32px (not 48px) — margins collapse */

/* Padding does NOT collapse */
.box-a {
  padding-bottom: 32px;
}
.box-b {
  padding-top: 16px;
}
/* Total space = 48px */
```

---

### 28. What is `overflow` in CSS?

`overflow` controls what happens when content is too large to fit in its container.

| Value               | Behavior                                      |
| ------------------- | --------------------------------------------- |
| `visible` (default) | Content overflows outside the box             |
| `hidden`            | Overflow is clipped and invisible             |
| `scroll`            | Always shows scrollbars                       |
| `auto`              | Shows scrollbars only when needed             |
| `clip`              | Clips without establishing a scroll container |

Example:

```css
/* Scrollable container */
.chat-messages {
  height: 400px;
  overflow-y: auto;
  overflow-x: hidden;
}

/* Truncate text with ellipsis */
.card-title {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 200px;
}

/* Hide overflow for rounded corners */
.avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  overflow: hidden;
}
```

---

### 29. What are CSS transforms?

CSS transforms let you visually move, rotate, scale, or skew elements without affecting document flow. They are GPU-accelerated and do not trigger layout reflow.

Example:

```css
/* 2D transforms */
.card:hover {
  transform: translateY(-8px) scale(1.02);
}

.icon {
  transform: rotate(45deg);
}

.skewed-banner {
  transform: skewY(-3deg);
}

/* 3D transforms */
.flip-card {
  transform-style: preserve-3d;
  transition: transform 0.6s;
}

.flip-card:hover {
  transform: rotateY(180deg);
}

/* Transform origin */
.tooltip {
  transform-origin: bottom center;
  transform: scale(0);
  transition: transform 0.2s ease;
}

.tooltip.visible {
  transform: scale(1);
}
```

---

### 30. What is `will-change` in CSS?

`will-change` hints to the browser that an element will be animated or transformed, allowing it to optimize rendering in advance by promoting the element to its own compositor layer.

Use it sparingly — overuse wastes memory and can hurt performance.

Example:

```css
/* Tell browser to prepare for transform changes */
.animated-card {
  will-change: transform;
  transition: transform 0.3s ease;
}

.animated-card:hover {
  transform: translateY(-8px);
}

/* Remove after animation completes (via JS) to free resources */
/* element.style.willChange = 'auto'; */

/* Avoid: applying to too many elements */
/* BAD: * { will-change: transform; } */
```

---

### 31. What is the `:root` pseudo-class?

`:root` targets the highest-level element in the document — the `<html>` element. It has higher specificity than `html {}`. It's the standard place to define global CSS custom properties (variables).

Example:

```css
:root {
  /* Design tokens */
  --color-primary: #667eea;
  --color-secondary: #764ba2;
  --color-text: #1a1a2e;
  --color-bg: #ffffff;

  --font-sans: "Inter", system-ui, sans-serif;
  --font-mono: "Fira Code", monospace;

  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 32px;
  --spacing-xl: 64px;

  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;
}

body {
  font-family: var(--font-sans);
  color: var(--color-text);
  background: var(--color-bg);
}
```

---

### 32. What are CSS logical properties?

CSS logical properties use writing-mode-relative directions (`inline`, `block`, `start`, `end`) instead of physical directions (`top`, `right`, `bottom`, `left`). They make layouts work correctly for both left-to-right (LTR) and right-to-left (RTL) languages.

Example:

```css
/* Physical properties (LTR-specific) */
.old {
  margin-left: 16px;
  margin-right: 16px;
  padding-top: 8px;
  border-left: 2px solid blue;
}

/* Logical properties (works for LTR and RTL) */
.new {
  margin-inline: 16px; /* left + right in LTR, right + left in RTL */
  padding-block-start: 8px; /* top in horizontal writing */
  border-inline-start: 2px solid blue; /* left in LTR, right in RTL */
}

/* Shorthand logical properties */
.card {
  margin-block: 24px; /* top and bottom */
  padding-inline: 16px; /* left and right */
  inset-block-start: 0; /* top */
}
```

---

### 33. What is `object-fit` and `object-position`?

`object-fit` controls how a replaced element (`<img>`, `<video>`) fills its container, similar to `background-size`. `object-position` sets the focal point of the image within the container.

| Value            | Behavior                        |
| ---------------- | ------------------------------- |
| `fill` (default) | Stretches to fill — may distort |
| `contain`        | Scales to fit — may letterbox   |
| `cover`          | Scales to cover — may crop      |
| `none`           | Original size — may overflow    |
| `scale-down`     | Smaller of `none` or `contain`  |

Example:

```css
/* Consistent image cards without distortion */
.card__image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  object-position: center top; /* focus on top of image */
  border-radius: 8px 8px 0 0;
}

/* Avatar */
.avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  object-fit: cover;
  object-position: center;
}
```

---

### 34. What is `aspect-ratio` in CSS?

`aspect-ratio` sets a preferred width-to-height ratio for an element. The browser automatically calculates the missing dimension. It eliminates the old "padding-top hack" for responsive embeds.

Example:

```css
/* 16:9 video embed */
.video-wrapper {
  width: 100%;
  aspect-ratio: 16 / 9;
}

/* Square card */
.square-card {
  width: 200px;
  aspect-ratio: 1; /* shorthand for 1/1 */
}

/* Responsive image with fixed ratio */
.hero-image {
  width: 100%;
  aspect-ratio: 3 / 1;
  object-fit: cover;
}

/* Old hack (no longer needed) */
/* .old-embed { padding-top: 56.25%; position: relative; } */
```

---

### 35. What is the `gap` property in CSS?

`gap` (formerly `grid-gap`) sets the spacing between rows and columns in Grid and Flexbox layouts. It's cleaner than using margins on individual items because it only adds space **between** items, not on the outer edges.

Example:

```css
/* Flexbox gap */
.nav {
  display: flex;
  gap: 8px; /* equal row and column gap */
}

.form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

/* Grid gap */
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px; /* row-gap and column-gap */
}

/* Different row and column gaps */
.layout {
  display: grid;
  gap: 32px 16px; /* row-gap column-gap */
}
```

---

### 36. What is `backdrop-filter` in CSS?

`backdrop-filter` applies graphical effects (like blur, brightness, or saturation) to the area **behind** an element. It's used to create frosted glass effects. The element itself must have some transparency for the effect to show.

Example:

```css
/* Frosted glass navbar */
.navbar {
  position: sticky;
  top: 0;
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px); /* Safari */
  border-bottom: 1px solid rgba(255, 255, 255, 0.3);
}

/* Glass card */
.glass-card {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px) saturate(180%);
  -webkit-backdrop-filter: blur(10px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  padding: 24px;
}
```

---

### 37. What are CSS `@supports` rules?

`@supports` (feature queries) let you apply CSS only if the browser supports a specific property or value. It's the CSS equivalent of feature detection, allowing progressive enhancement.

Example:

```css
/* Base styles for all browsers */
.layout {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}

/* Enhanced layout for browsers that support Grid */
@supports (display: grid) {
  .layout {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  }
}

/* Check for backdrop-filter support */
@supports (backdrop-filter: blur(1px)) {
  .navbar {
    background: rgba(255, 255, 255, 0.7);
    backdrop-filter: blur(12px);
  }
}

/* Negate: apply when NOT supported */
@supports not (display: grid) {
  .layout {
    display: flex;
  }
}
```

---

### 38. What is the `color-mix()` function in CSS?

`color-mix()` blends two colors together in a specified color space and proportion. It's a native CSS way to create tints, shades, and color variations without a preprocessor.

Example:

```css
:root {
  --color-brand: #667eea;

  /* Create tints and shades natively */
  --color-brand-light: color-mix(in srgb, var(--color-brand) 30%, white);
  --color-brand-dark: color-mix(in srgb, var(--color-brand) 80%, black);
  --color-brand-muted: color-mix(in srgb, var(--color-brand) 50%, gray);
}

.button-primary {
  background: var(--color-brand);
}

.button-primary:hover {
  background: var(--color-brand-dark);
}

.tag {
  background: var(--color-brand-light);
  color: var(--color-brand-dark);
}

/* Mix two specific colors */
.sunset {
  background: color-mix(in oklch, #ff6b6b 60%, #ffd93d);
}
```

---

### 39. What is a CSS reset and why is it used?

A CSS reset removes or normalizes the inconsistent default styles that browsers apply to HTML elements. This gives you a clean, predictable baseline to build from.

There are two approaches:

- **Hard reset**: Zero out all margins, paddings, and styles (e.g., Eric Meyer's reset)
- **Normalize**: Preserve useful defaults while fixing cross-browser inconsistencies (e.g., `normalize.css`)

Example:

```css
/* Modern minimal CSS reset */
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  -webkit-text-size-adjust: 100%;
}

body {
  min-height: 100vh;
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}

img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
}

input,
button,
textarea,
select {
  font: inherit;
}

p,
h1,
h2,
h3,
h4,
h5,
h6 {
  overflow-wrap: break-word;
}
```

---

### 40. What is the difference between `min-width` and `max-width` in media queries?

- `min-width`: Applies styles when the viewport is **at least** the specified width. Used in **mobile-first** design — start with mobile styles, add complexity for larger screens.
- `max-width`: Applies styles when the viewport is **at most** the specified width. Used in **desktop-first** design — start with desktop styles, simplify for smaller screens.

Example:

```css
/* ✅ Mobile-first (recommended) — use min-width */
.grid {
  display: block; /* mobile: single column */
}

@media (min-width: 768px) {
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr; /* tablet: 2 columns */
  }
}

@media (min-width: 1200px) {
  .grid {
    grid-template-columns: repeat(4, 1fr); /* desktop: 4 columns */
  }
}

/* Desktop-first — use max-width */
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

@media (max-width: 1199px) {
  .grid {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 767px) {
  .grid {
    display: block;
  }
}
```

---

> Made with ❤️ — Click ⭐ if this helped you prepare for your next interview!
