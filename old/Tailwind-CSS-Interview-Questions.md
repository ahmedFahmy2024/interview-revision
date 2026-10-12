# modern Tailwind CSS interview Questions & Answers

Click ⭐ if you like the project. Follow for more updates.

---

# Tailwind CSS Interview Questions 🌬️

| #   | Question                                                                           |
| --- | ---------------------------------------------------------------------------------- |
| 1   | What is Tailwind CSS and how does it differ from traditional CSS frameworks?       |
| 2   | What is the utility-first approach in Tailwind CSS?                                |
| 3   | What is Preflight in Tailwind CSS?                                                 |
| 4   | How does Tailwind CSS handle responsive design?                                    |
| 5   | How does dark mode work in Tailwind CSS?                                           |
| 6   | What are state variants in Tailwind CSS?                                           |
| 7   | What is the `@apply` directive and when should you use it?                         |
| 8   | What are arbitrary values in Tailwind CSS?                                         |
| 9   | How do you customize the Tailwind theme?                                           |
| 10  | What is the difference between Tailwind CSS v3 and v4?                             |
| 11  | What is the `@theme` directive in Tailwind CSS v4?                                 |
| 12  | How does Tailwind CSS scan and purge unused styles?                                |
| 13  | What are the `group` and `group-*` variants?                                       |
| 14  | What are the `peer` and `peer-*` variants?                                         |
| 15  | What is the `@layer` directive in Tailwind CSS?                                    |
| 16  | How do you add custom utilities in Tailwind CSS?                                   |
| 17  | What are Tailwind CSS plugins?                                                     |
| 18  | What is the `space-x` and `space-y` utility?                                       |
| 19  | How does Tailwind handle typography?                                               |
| 20  | What is the `divide` utility in Tailwind CSS?                                      |
| 21  | What is the `ring` utility in Tailwind CSS?                                        |
| 22  | How do you use Tailwind CSS with CSS Grid?                                         |
| 23  | How do you use Tailwind CSS with Flexbox?                                          |
| 24  | What are the `container` and `mx-auto` utilities?                                  |
| 25  | What is the `aspect-ratio` utility in Tailwind?                                    |
| 26  | What are `min-*`, `max-*`, and `clamp` utilities in Tailwind?                      |
| 27  | What is the `transition` and `animate` utility in Tailwind?                        |
| 28  | What are `before:` and `after:` pseudo-element variants?                           |
| 29  | What are arbitrary variants in Tailwind CSS?                                       |
| 30  | What is the `@custom-variant` directive?                                           |
| 31  | How do you handle forms with Tailwind CSS?                                         |
| 32  | What is the `@tailwindcss/forms` plugin?                                           |
| 33  | What is the `@tailwindcss/typography` plugin?                                      |
| 34  | How do you use Tailwind CSS with React or Next.js?                                 |
| 35  | What is the `cn()` / `clsx` pattern used with Tailwind?                            |
| 36  | What are `data-*` attribute variants in Tailwind?                                  |
| 37  | What is the `not-*` variant in Tailwind CSS?                                       |
| 38  | How do you create a design system with Tailwind CSS?                               |
| 39  | What are the performance best practices for Tailwind CSS?                          |
| 40  | What is the difference between `hidden`, `invisible`, and `opacity-0` in Tailwind? |

---

# Tailwind CSS Questions & Answers

---

### 1. What is Tailwind CSS and how does it differ from traditional CSS frameworks?

Tailwind CSS is a **utility-first CSS framework** that provides low-level utility classes you compose directly in your HTML to build any design. Unlike component-based frameworks (Bootstrap, Bulma) that give you pre-built components like `.btn` or `.card`, Tailwind gives you atomic building blocks like `flex`, `pt-4`, `text-center`, and `bg-blue-500`.

| Feature         | Tailwind CSS      | Bootstrap / Bulma   |
| --------------- | ----------------- | ------------------- |
| Approach        | Utility-first     | Component-based     |
| Customization   | Fully custom      | Override defaults   |
| Bundle size     | Only used classes | Full framework      |
| Design opinions | None — you decide | Opinionated styles  |
| Learning curve  | Learn utilities   | Learn component API |

Example:

```html
<!-- Tailwind: compose utilities directly -->
<button
  class="bg-blue-500 hover:bg-blue-600 text-white font-semibold py-2 px-4 rounded-lg transition"
>
  Save Changes
</button>

<!-- Bootstrap: use pre-built component -->
<button class="btn btn-primary">Save Changes</button>
```

---

### 2. What is the utility-first approach in Tailwind CSS?

The utility-first approach means styling elements by applying small, single-purpose classes directly in your markup instead of writing custom CSS. Each class does exactly one thing — `text-lg` sets font size, `font-bold` sets font weight, `mt-4` sets margin-top.

Benefits:

- No context switching between HTML and CSS files
- No naming things (no `.card-header-title-wrapper`)
- Styles are co-located with markup — easy to read and delete
- No specificity conflicts — all utilities have the same weight

Example:

```html
<!-- Utility-first card component -->
<div class="bg-white rounded-xl shadow-md overflow-hidden max-w-sm">
  <img class="w-full h-48 object-cover" src="photo.jpg" alt="Card image" />
  <div class="p-6">
    <span class="text-xs font-semibold text-blue-500 uppercase tracking-wide">
      Category
    </span>
    <h2 class="mt-1 text-xl font-bold text-gray-900">Card Title</h2>
    <p class="mt-2 text-gray-500 text-sm leading-relaxed">
      A short description of the card content goes here.
    </p>
    <button
      class="mt-4 bg-blue-500 hover:bg-blue-600 text-white text-sm font-medium py-2 px-4 rounded-lg transition"
    >
      Read More
    </button>
  </div>
</div>
```

---

### 3. What is Preflight in Tailwind CSS?

Preflight is Tailwind's base stylesheet — a set of opinionated CSS resets built on top of `modern-normalize`. It removes all browser default styles to give you a consistent, predictable baseline across browsers.

Key things Preflight does:

- Removes default margins from headings, paragraphs, lists
- Makes headings unstyled (same size as body text)
- Removes list bullets and numbering
- Makes images `display: block` and `max-width: 100%`
- Removes default button and input styles
- Sets `box-sizing: border-box` globally

Example:

```css
/* Preflight is injected automatically when you import Tailwind */
@import "tailwindcss";

/* Override Preflight defaults with @layer base */
@layer base {
  h1 {
    font-size: var(--text-2xl);
    font-weight: 700;
  }
  h2 {
    font-size: var(--text-xl);
    font-weight: 600;
  }
  a {
    color: var(--color-blue-600);
    text-decoration: underline;
  }
}
```

```html
<!-- Without Preflight: h1 would be large and bold by default -->
<!-- With Preflight: h1 looks like body text until you add utilities -->
<h1 class="text-3xl font-bold text-gray-900">Page Title</h1>
```

---

### 4. How does Tailwind CSS handle responsive design?

Tailwind uses a **mobile-first** breakpoint system. Every utility can be prefixed with a breakpoint to apply it at that screen size and above. No prefix = applies to all screen sizes.

Default breakpoints:

| Prefix | Min-width |
| ------ | --------- |
| `sm:`  | 640px     |
| `md:`  | 768px     |
| `lg:`  | 1024px    |
| `xl:`  | 1280px    |
| `2xl:` | 1536px    |

Example:

```html
<!-- Mobile: 1 col → Tablet: 2 cols → Desktop: 4 cols -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
  <div class="bg-white rounded-lg p-4 shadow">Card 1</div>
  <div class="bg-white rounded-lg p-4 shadow">Card 2</div>
  <div class="bg-white rounded-lg p-4 shadow">Card 3</div>
  <div class="bg-white rounded-lg p-4 shadow">Card 4</div>
</div>

<!-- Text size scales with viewport -->
<h1 class="text-2xl md:text-4xl lg:text-6xl font-bold">Responsive Heading</h1>

<!-- Arbitrary one-off breakpoint -->
<div class="min-[900px]:flex max-[600px]:bg-sky-300">Custom breakpoint</div>
```

---

### 5. How does dark mode work in Tailwind CSS?

Tailwind supports dark mode via the `dark:` variant. It can work in two ways:

- **Media strategy** (default): Respects the OS `prefers-color-scheme` setting automatically.
- **Selector strategy**: Toggles dark mode by adding a `.dark` class to the `<html>` element — useful for a manual toggle button.

Example:

```html
<!-- Automatic OS-level dark mode -->
<div
  class="bg-white dark:bg-gray-900 text-gray-900 dark:text-white p-8 rounded-xl shadow"
>
  <h2 class="text-2xl font-bold">Settings</h2>
  <p class="mt-2 text-gray-500 dark:text-gray-400">
    Manage your account preferences.
  </p>
</div>

<!-- Combine dark mode with responsive breakpoints -->
<div class="lg:bg-white lg:dark:bg-black">...</div>
```

```css
/* v4: opt into selector-based dark mode */
@import "tailwindcss";
@variant dark (&:where(.dark, .dark *));
```

```js
// Toggle dark mode via JavaScript
document.documentElement.classList.toggle("dark");
```

---

### 6. What are state variants in Tailwind CSS?

State variants are prefixes that apply utilities only when a specific condition is true — like hover, focus, active, disabled, or checked states. They map directly to CSS pseudo-classes and pseudo-elements.

Example:

```html
<!-- Hover, focus, active on a button -->
<button
  class="
  bg-violet-500
  hover:bg-violet-600
  focus:outline-2 focus:outline-offset-2 focus:outline-violet-500
  active:bg-violet-700
  disabled:opacity-50 disabled:cursor-not-allowed
  transition
"
>
  Save Changes
</button>

<!-- Focus on input -->
<input
  class="border border-gray-300 focus:border-blue-400 focus:ring-2 focus:ring-blue-200 rounded-md px-3 py-2 outline-none transition"
  type="text"
  placeholder="Enter email"
/>

<!-- Checked state on checkbox -->
<input
  type="checkbox"
  class="accent-blue-500 checked:ring-2 checked:ring-blue-300"
/>

<!-- First, last, odd, even children -->
<li class="odd:bg-gray-50 even:bg-white px-4 py-2">Item</li>
```

---

### 7. What is the `@apply` directive and when should you use it?

`@apply` lets you extract repeated utility combinations into a single CSS class. It's useful when you have a pattern you repeat many times and want to keep your HTML clean.

Use it sparingly — overusing `@apply` defeats the purpose of utility-first and makes your CSS harder to maintain.

Example:

```css
/* Extract a reusable button style */
@layer components {
  .btn-primary {
    @apply bg-blue-500 hover:bg-blue-600 text-white font-semibold
           py-2 px-4 rounded-lg transition focus:outline-none
           focus:ring-2 focus:ring-blue-300 disabled:opacity-50;
  }

  .card {
    @apply bg-white rounded-xl shadow-md overflow-hidden p-6;
  }

  .input-field {
    @apply w-full border border-gray-300 rounded-md px-3 py-2
           focus:border-blue-400 focus:ring-2 focus:ring-blue-200
           outline-none transition;
  }
}
```

```html
<!-- Now use the extracted class -->
<button class="btn-primary">Save Changes</button>
<div class="card">...</div>
<input class="input-field" type="text" />
```

---

### 8. What are arbitrary values in Tailwind CSS?

Arbitrary values let you use any CSS value that isn't in the default theme by wrapping it in square brackets. This gives you the full power of CSS without leaving Tailwind or adding custom config.

Example:

```html
<!-- Arbitrary color -->
<div class="bg-[#1da1f2] text-[#fff]">Twitter Blue</div>

<!-- Arbitrary size -->
<div class="w-[350px] h-[200px] mt-[13px]">Custom size</div>

<!-- Arbitrary font size -->
<p class="text-[17px] leading-[1.6]">Custom typography</p>

<!-- Arbitrary grid columns -->
<div class="grid grid-cols-[1fr_2fr_1fr] gap-4">
  <div>Left</div>
  <div>Center</div>
  <div>Right</div>
</div>

<!-- Arbitrary CSS property with square bracket syntax -->
<div class="[mask-image:linear-gradient(to_bottom,black,transparent)]">
  Fade out
</div>

<!-- Arbitrary breakpoint -->
<div class="min-[320px]:text-center max-[600px]:bg-sky-300">
  Custom breakpoint
</div>
```

---

### 9. How do you customize the Tailwind theme?

In **Tailwind v4**, customization is done directly in CSS using the `@theme` directive — no JavaScript config file needed. In **v3**, you extend the theme in `tailwind.config.js`.

Example (v4 — CSS-first):

```css
@import "tailwindcss";

@theme {
  /* Custom fonts */
  --font-display: "Satoshi", "sans-serif";
  --font-body: "Inter", system-ui, sans-serif;

  /* Custom colors */
  --color-brand: oklch(71.7% 0.25 360);
  --color-brand-dark: oklch(55% 0.22 360);

  /* Custom breakpoints */
  --breakpoint-3xl: 1920px;

  /* Custom spacing */
  --spacing-18: 4.5rem;
  --spacing-128: 32rem;

  /* Custom easing */
  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
}
```

Example (v3 — JS config):

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: { DEFAULT: "#667eea", dark: "#5a67d8" },
      },
      fontFamily: {
        display: ["Satoshi", "sans-serif"],
      },
      spacing: {
        18: "4.5rem",
        128: "32rem",
      },
    },
  },
};
```

---

### 10. What is the difference between Tailwind CSS v3 and v4?

Tailwind v4 is a ground-up rewrite with a new engine and a CSS-first philosophy. Key differences:

| Feature              | v3                                | v4                                     |
| -------------------- | --------------------------------- | -------------------------------------- |
| Configuration        | `tailwind.config.js` (JavaScript) | `@theme` in CSS (CSS-first)            |
| Engine               | PostCSS plugin                    | Oxide engine (Rust-based, ~10x faster) |
| Installation         | `tailwindcss` + PostCSS config    | `@import "tailwindcss"`                |
| Dark mode            | `darkMode: 'class'` in config     | `@variant dark` in CSS                 |
| Custom variants      | `addVariant()` in plugin          | `@custom-variant` in CSS               |
| Arbitrary properties | `[property:value]`                | Same, improved                         |
| CSS variables        | Generated separately              | All utilities exposed as CSS vars      |
| Bundle size          | Scans and purges                  | Even smaller, zero-runtime             |

Example (v4 setup):

```css
/* app.css — that's it, no config file needed */
@import "tailwindcss";

@theme {
  --color-primary: #667eea;
  --font-sans: "Inter", system-ui, sans-serif;
}
```

```html
<!-- index.html -->
<link rel="stylesheet" href="app.css" />
<h1 class="text-3xl font-bold text-primary">Hello v4</h1>
```

---

### 11. What is the `@theme` directive in Tailwind CSS v4?

`@theme` is the CSS-first replacement for `tailwind.config.js` in v4. You define design tokens as CSS custom properties inside `@theme`, and Tailwind automatically generates utility classes and variants from them.

Example:

```css
@import "tailwindcss";

@theme {
  /* Colors → generates bg-*, text-*, border-* utilities */
  --color-neon-pink: oklch(71.7% 0.25 360);
  --color-neon-lime: oklch(91.5% 0.258 129);
  --color-neon-cyan: oklch(91.3% 0.139 195.8);

  /* Fonts → generates font-* utilities */
  --font-display: "Satoshi", "sans-serif";

  /* Breakpoints → generates sm:, md:, 3xl: variants */
  --breakpoint-3xl: 1920px;

  /* Spacing → generates p-*, m-*, w-*, h-* utilities */
  --spacing-18: 4.5rem;

  /* Easing → generates ease-* utilities */
  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
}
```

```html
<!-- Use the generated utilities directly -->
<h1 class="font-display text-neon-pink 3xl:text-6xl">Hello Tailwind v4</h1>
```

---

### 12. How does Tailwind CSS scan and purge unused styles?

Tailwind scans your source files for class names and only generates CSS for the classes it finds. In v3 this was called "purging" (via the `content` config). In v4, automatic content detection is built-in — Tailwind finds your templates automatically.

Example (v3 — explicit content config):

```js
// tailwind.config.js
module.exports = {
  content: [
    "./src/**/*.{html,js,jsx,ts,tsx,vue}",
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
  // ...
};
```

```css
/* v4 — auto-detection, no config needed */
@import "tailwindcss";
/* Tailwind scans all files in your project automatically */
```

Important: never construct class names dynamically — Tailwind won't find them:

```js
// ❌ BAD — Tailwind can't detect this
const color = "blue";
<div class={`bg-${color}-500`}>...</div>;

// ✅ GOOD — full class names are detectable
const classes = isActive ? "bg-blue-500" : "bg-gray-200";
<div class={classes}>...</div>;
```

---

### 13. What are the `group` and `group-*` variants?

The `group` utility lets you style child elements based on the state of a parent element. Add `group` to the parent, then use `group-hover:`, `group-focus:`, `group-active:` etc. on children.

Example:

```html
<!-- Style children when parent is hovered -->
<a
  href="#"
  class="group flex items-center gap-3 p-4 rounded-lg hover:bg-blue-50 transition"
>
  <div
    class="w-10 h-10 rounded-full bg-gray-200 group-hover:bg-blue-200 transition"
  >
    <!-- avatar -->
  </div>
  <div>
    <p class="font-semibold text-gray-900 group-hover:text-blue-600 transition">
      Ahmed Hassan
    </p>
    <p class="text-sm text-gray-500 group-hover:text-blue-400 transition">
      View profile →
    </p>
  </div>
</a>

<!-- Named groups for nested scenarios -->
<div class="group/card hover:shadow-xl transition">
  <div class="group/btn">
    <button class="group-hover/card:bg-blue-500 group-hover/btn:scale-105">
      Click me
    </button>
  </div>
</div>
```

---

### 14. What are the `peer` and `peer-*` variants?

The `peer` utility lets you style an element based on the state of a **sibling** element. Add `peer` to the sibling, then use `peer-checked:`, `peer-focus:`, `peer-invalid:` etc. on the element you want to style.

Example:

```html
<!-- Show label hint when input is focused -->
<div class="relative">
  <input
    id="email"
    type="email"
    class="peer w-full border border-gray-300 rounded-md px-3 py-2 outline-none focus:border-blue-500"
    placeholder=" "
  />
  <label
    for="email"
    class="absolute left-3 top-2 text-gray-400 text-sm transition-all
           peer-focus:-top-5 peer-focus:text-xs peer-focus:text-blue-500
           peer-[:not(:placeholder-shown)]:-top-5 peer-[:not(:placeholder-shown)]:text-xs"
  >
    Email address
  </label>
</div>

<!-- Show error message when input is invalid -->
<input type="email" class="peer border rounded px-3 py-2" />
<p class="hidden peer-invalid:block text-red-500 text-sm mt-1">
  Please enter a valid email address.
</p>
```

---

### 15. What is the `@layer` directive in Tailwind CSS?

`@layer` in Tailwind lets you add custom styles into one of Tailwind's three layers: `base`, `components`, or `utilities`. This ensures your custom styles respect the correct cascade order and can be overridden predictably.

- `base` — global resets and element defaults
- `components` — reusable component classes (extracted with `@apply`)
- `utilities` — single-purpose utility overrides

Example:

```css
@import "tailwindcss";

/* Custom base styles */
@layer base {
  h1 {
    @apply text-3xl font-bold text-gray-900;
  }
  h2 {
    @apply text-2xl font-semibold text-gray-800;
  }
  a {
    @apply text-blue-600 hover:underline;
  }
}

/* Reusable component classes */
@layer components {
  .btn {
    @apply inline-flex items-center justify-center px-4 py-2
           rounded-lg font-medium transition focus:outline-none
           focus:ring-2 focus:ring-offset-2 disabled:opacity-50;
  }
  .btn-primary {
    @apply btn bg-blue-500 hover:bg-blue-600 text-white focus:ring-blue-400;
  }
  .btn-secondary {
    @apply btn bg-gray-100 hover:bg-gray-200 text-gray-800 focus:ring-gray-300;
  }
}

/* Custom utility overrides */
@layer utilities {
  .text-balance {
    text-wrap: balance;
  }
  .scrollbar-hide {
    scrollbar-width: none;
  }
}
```

---

### 16. How do you add custom utilities in Tailwind CSS?

You can add custom utilities using `@layer utilities` in CSS (both v3 and v4), or via the `addUtilities()` function in a v3 plugin.

Example (CSS approach — works in v3 and v4):

```css
@layer utilities {
  /* Custom scrollbar hiding */
  .scrollbar-hide {
    scrollbar-width: none;
    -ms-overflow-style: none;
  }
  .scrollbar-hide::-webkit-scrollbar {
    display: none;
  }

  /* Custom text gradient */
  .text-gradient {
    background: linear-gradient(135deg, #667eea, #764ba2);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  /* Custom truncation */
  .line-clamp-2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}
```

```html
<h1 class="text-gradient text-4xl font-bold">Gradient Title</h1>
<p class="line-clamp-2 text-gray-600">Long text that gets truncated...</p>
```

---

### 17. What are Tailwind CSS plugins?

Plugins extend Tailwind with additional utilities, components, variants, or base styles. Official plugins cover common needs; you can also write custom plugins.

Official plugins:

- `@tailwindcss/typography` — prose styles for rich text
- `@tailwindcss/forms` — consistent form element styling
- `@tailwindcss/aspect-ratio` — aspect ratio utilities (now built-in)
- `@tailwindcss/container-queries` — container query variants

Example (v3 plugin setup):

```js
// tailwind.config.js
module.exports = {
  plugins: [require("@tailwindcss/typography"), require("@tailwindcss/forms")],
};
```

Example (custom plugin in v3):

```js
const plugin = require("tailwindcss/plugin");

module.exports = {
  plugins: [
    plugin(function ({ addUtilities, addComponents, theme }) {
      addUtilities({
        ".text-shadow": {
          textShadow: "2px 2px 4px rgba(0,0,0,0.1)",
        },
        ".text-shadow-lg": {
          textShadow: "4px 4px 8px rgba(0,0,0,0.2)",
        },
      });
    }),
  ],
};
```

---

### 18. What is the `space-x` and `space-y` utility?

`space-x-*` and `space-y-*` add horizontal or vertical spacing **between** sibling elements using a CSS selector trick (`> * + *`). It's a quick alternative to adding `gap` on a flex/grid container or `margin` on individual items.

Example:

```html
<!-- space-x: horizontal gap between children -->
<nav class="flex space-x-4">
  <a href="#" class="text-gray-600 hover:text-blue-500">Home</a>
  <a href="#" class="text-gray-600 hover:text-blue-500">About</a>
  <a href="#" class="text-gray-600 hover:text-blue-500">Contact</a>
</nav>

<!-- space-y: vertical gap between children -->
<form class="space-y-4">
  <input
    class="w-full border rounded px-3 py-2"
    type="text"
    placeholder="Name"
  />
  <input
    class="w-full border rounded px-3 py-2"
    type="email"
    placeholder="Email"
  />
  <textarea
    class="w-full border rounded px-3 py-2"
    placeholder="Message"
  ></textarea>
  <button class="w-full bg-blue-500 text-white py-2 rounded">Send</button>
</form>

<!-- Note: prefer gap-* on flex/grid containers when possible -->
<div class="flex gap-4">...</div>
```

---

### 19. How does Tailwind handle typography?

Tailwind provides typography utilities for font size, weight, line height, letter spacing, text alignment, color, and decoration. For rich text content (blog posts, markdown), the `@tailwindcss/typography` plugin adds a `prose` class.

Example (built-in utilities):

```html
<article>
  <h1
    class="text-4xl font-extrabold tracking-tight text-gray-900 leading-tight"
  >
    Article Title
  </h1>
  <p class="mt-2 text-lg text-gray-500 font-normal leading-relaxed">
    Subtitle or description text here.
  </p>
  <p class="mt-6 text-base text-gray-700 leading-7">
    Body paragraph with comfortable reading line height.
  </p>
  <a
    href="#"
    class="text-blue-600 underline underline-offset-2 hover:text-blue-800"
  >
    Read more
  </a>
</article>
```

Example (`prose` from `@tailwindcss/typography`):

```html
<!-- Automatically styles all child HTML elements for reading -->
<article class="prose prose-lg prose-blue max-w-none dark:prose-invert">
  <h1>Automatically styled heading</h1>
  <p>Paragraph with proper spacing and line height.</p>
  <ul>
    <li>List item with bullets</li>
    <li>Another item</li>
  </ul>
  <pre><code>// Code block with syntax styling</code></pre>
</article>
```

---

### 20. What is the `divide` utility in Tailwind CSS?

The `divide-*` utilities add borders **between** sibling elements — similar to `space-*` but for borders instead of margins. It uses the `> * + *` selector pattern.

Example:

```html
<!-- Vertical dividers between list items -->
<ul
  class="divide-y divide-gray-200 border border-gray-200 rounded-lg overflow-hidden"
>
  <li class="flex items-center justify-between px-4 py-3 hover:bg-gray-50">
    <span class="font-medium">Ahmed Hassan</span>
    <span class="text-sm text-gray-500">Admin</span>
  </li>
  <li class="flex items-center justify-between px-4 py-3 hover:bg-gray-50">
    <span class="font-medium">Sara Ali</span>
    <span class="text-sm text-gray-500">Editor</span>
  </li>
  <li class="flex items-center justify-between px-4 py-3 hover:bg-gray-50">
    <span class="font-medium">Omar Khaled</span>
    <span class="text-sm text-gray-500">Viewer</span>
  </li>
</ul>

<!-- Horizontal dividers in a flex row -->
<div class="flex divide-x divide-gray-300">
  <span class="px-4">Home</span>
  <span class="px-4">About</span>
  <span class="px-4">Contact</span>
</div>
```

---

### 21. What is the `ring` utility in Tailwind CSS?

The `ring-*` utilities create focus rings using `box-shadow` (not `outline`). They're commonly used for accessible focus indicators on interactive elements. You can control the ring width, color, offset, and opacity.

Example:

```html
<!-- Default focus ring on a button -->
<button
  class="
  bg-blue-500 text-white px-4 py-2 rounded-lg
  focus:outline-none
  focus:ring-2 focus:ring-blue-400 focus:ring-offset-2
  transition
"
>
  Submit
</button>

<!-- Custom ring color and width -->
<input
  type="text"
  class="border border-gray-300 rounded-md px-3 py-2 outline-none
         focus:ring-4 focus:ring-purple-300 focus:border-purple-500 transition"
/>

<!-- Ring on a card for selection state -->
<div
  class="rounded-xl p-4 border-2 border-transparent
            ring-2 ring-blue-500 ring-offset-2 cursor-pointer"
>
  Selected card
</div>

<!-- Inset ring (inside the element) -->
<button class="ring-2 ring-inset ring-blue-500 px-4 py-2 rounded">
  Inset ring
</button>
```

---

### 22. How do you use Tailwind CSS with CSS Grid?

Tailwind provides a full set of grid utilities covering `grid-template-columns`, `grid-template-rows`, `grid-column`, `grid-row`, `gap`, and placement.

Example:

```html
<!-- Basic responsive grid -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
  <div class="bg-white rounded-lg shadow p-4">Card 1</div>
  <div class="bg-white rounded-lg shadow p-4">Card 2</div>
  <div class="bg-white rounded-lg shadow p-4">Card 3</div>
</div>

<!-- Named template areas with arbitrary values -->
<div
  class="grid [grid-template-areas:'header_header''sidebar_main''footer_footer']
            grid-cols-[250px_1fr] grid-rows-[auto_1fr_auto] min-h-screen gap-4"
>
  <header class="[grid-area:header] bg-gray-800 text-white p-4">Header</header>
  <aside class="[grid-area:sidebar] bg-gray-100 p-4">Sidebar</aside>
  <main class="[grid-area:main] p-4">Main Content</main>
  <footer class="[grid-area:footer] bg-gray-800 text-white p-4">Footer</footer>
</div>

<!-- Spanning columns and rows -->
<div class="grid grid-cols-3 gap-4">
  <div class="col-span-2 bg-blue-100 p-4">Spans 2 columns</div>
  <div class="row-span-2 bg-green-100 p-4">Spans 2 rows</div>
  <div class="col-start-1 col-end-3 bg-yellow-100 p-4">Explicit placement</div>
</div>
```

---

### 23. How do you use Tailwind CSS with Flexbox?

Tailwind covers all Flexbox properties: `flex-direction`, `justify-content`, `align-items`, `flex-wrap`, `flex-grow`, `flex-shrink`, `flex-basis`, `order`, and `gap`.

Example:

```html
<!-- Horizontal navbar -->
<nav class="flex items-center justify-between px-6 py-4 bg-white shadow">
  <a href="#" class="text-xl font-bold text-gray-900">Logo</a>
  <ul class="flex items-center gap-6">
    <li><a href="#" class="text-gray-600 hover:text-blue-500">Home</a></li>
    <li><a href="#" class="text-gray-600 hover:text-blue-500">About</a></li>
    <li><a href="#" class="text-gray-600 hover:text-blue-500">Contact</a></li>
  </ul>
  <button class="bg-blue-500 text-white px-4 py-2 rounded-lg">Sign In</button>
</nav>

<!-- Centered hero section -->
<section
  class="flex flex-col items-center justify-center min-h-screen text-center px-4"
>
  <h1 class="text-5xl font-bold text-gray-900">Welcome</h1>
  <p class="mt-4 text-xl text-gray-500 max-w-xl">A short description here.</p>
  <button class="mt-8 bg-blue-500 text-white px-8 py-3 rounded-full text-lg">
    Get Started
  </button>
</section>

<!-- Flex item growth control -->
<div class="flex gap-4">
  <div class="flex-none w-48 bg-gray-100 p-4">Fixed width</div>
  <div class="flex-1 bg-blue-50 p-4">Grows to fill space</div>
  <div class="flex-none w-32 bg-gray-100 p-4">Fixed width</div>
</div>
```

---

### 24. What are the `container` and `mx-auto` utilities?

`container` sets a max-width that matches the current breakpoint. `mx-auto` centers a block element horizontally by setting `margin-left: auto` and `margin-right: auto`.

Example:

```html
<!-- Container centers and constrains content width -->
<div class="container mx-auto px-4">
  <h1 class="text-3xl font-bold">Page Title</h1>
  <p class="mt-4 text-gray-600">Content is constrained and centered.</p>
</div>

<!-- Custom max-width centering (more common pattern) -->
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
  <!-- Content -->
</div>

<!-- Center a card -->
<div class="max-w-md mx-auto bg-white rounded-xl shadow-md p-6 mt-10">
  <h2 class="text-xl font-semibold">Centered Card</h2>
  <p class="mt-2 text-gray-500">This card is centered on the page.</p>
</div>
```

```css
/* v3: configure container to be centered by default */
/* tailwind.config.js */
/* theme: { container: { center: true, padding: '1rem' } } */
```

---

### 25. What is the `aspect-ratio` utility in Tailwind?

The `aspect-ratio` utilities set a fixed width-to-height ratio on an element. Tailwind includes common presets and supports arbitrary values.

Example:

```html
<!-- 16:9 video embed -->
<div class="aspect-video w-full bg-black rounded-xl overflow-hidden">
  <iframe
    class="w-full h-full"
    src="https://www.youtube.com/embed/dQw4w9WgXcQ"
    allowfullscreen
  ></iframe>
</div>

<!-- Square image -->
<img
  class="aspect-square w-32 object-cover rounded-full"
  src="avatar.jpg"
  alt="Avatar"
/>

<!-- 4:3 ratio -->
<div class="aspect-[4/3] bg-gray-100 rounded-lg overflow-hidden">
  <img class="w-full h-full object-cover" src="photo.jpg" alt="" />
</div>

<!-- Arbitrary ratio -->
<div
  class="aspect-[3/1] bg-gradient-to-r from-blue-500 to-purple-600 rounded-xl"
>
  Banner
</div>
```

---

### 26. What are `min-*`, `max-*`, and `clamp` utilities in Tailwind?

Tailwind provides `min-w-*`, `max-w-*`, `min-h-*`, `max-h-*` for constraining dimensions. For fluid sizing with `clamp()`, you use arbitrary values.

Example:

```html
<!-- Constrain width -->
<div class="w-full max-w-lg mx-auto">Constrained to 32rem max</div>
<div class="w-full min-w-0">Can shrink to zero</div>

<!-- Constrain height -->
<div class="h-auto max-h-64 overflow-y-auto">
  Scrollable when content exceeds 16rem
</div>

<!-- Fluid typography with clamp via arbitrary value -->
<h1 class="text-[clamp(1.5rem,4vw,3rem)] font-bold">Fluid heading</h1>

<!-- Fluid spacing -->
<section class="py-[clamp(2rem,5vw,6rem)]">Fluid vertical padding</section>

<!-- min() and max() functions -->
<div class="w-[min(100%,600px)] mx-auto">Never wider than 600px</div>
```

---

### 27. What is the `transition` and `animate` utility in Tailwind?

`transition-*` utilities enable smooth CSS transitions on property changes. `animate-*` utilities apply pre-built CSS animations. Both can be customized or extended.

Example:

```html
<!-- Transition on hover -->
<button
  class="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded
               transition-colors duration-300 ease-in-out"
>
  Hover me
</button>

<!-- Transition multiple properties -->
<div
  class="opacity-0 translate-y-4 transition-all duration-500 ease-out
            hover:opacity-100 hover:translate-y-0"
>
  Fade and slide in
</div>

<!-- Built-in animations -->
<div
  class="animate-spin w-8 h-8 border-4 border-blue-500 border-t-transparent rounded-full"
></div>
<div class="animate-ping w-4 h-4 bg-blue-400 rounded-full"></div>
<div class="animate-pulse bg-gray-200 rounded h-4 w-48"></div>
<div class="animate-bounce text-2xl">👋</div>

<!-- Custom animation via arbitrary value -->
<div class="animate-[fadeIn_0.5s_ease_forwards]">Custom animation</div>
```

---

### 28. What are `before:` and `after:` pseudo-element variants?

Tailwind's `before:` and `after:` variants let you style `::before` and `::after` pseudo-elements directly in HTML. You must include `content-['']` (or a value) for them to render.

Example:

```html
<!-- Decorative arrow after a link -->
<a
  href="#"
  class="text-blue-600 after:content-['_→'] after:ml-1 hover:after:translate-x-1 after:inline-block after:transition"
>
  Read more
</a>

<!-- External link indicator -->
<a
  href="https://example.com"
  class="text-blue-600 after:content-['_↗'] after:text-xs after:align-super"
>
  Pro Display XDR
</a>

<!-- Decorative line before a heading -->
<h2
  class="before:content-[''] before:block before:w-12 before:h-1
           before:bg-blue-500 before:mb-3 text-2xl font-bold"
>
  Section Title
</h2>

<!-- Badge with before pseudo-element -->
<span
  class="relative before:content-[''] before:absolute before:-top-1 before:-right-1
             before:w-2 before:h-2 before:bg-red-500 before:rounded-full"
>
  Notifications
</span>
```

---

### 29. What are arbitrary variants in Tailwind CSS?

Arbitrary variants let you write any CSS selector directly in your class name using square bracket notation. This handles complex selectors without writing custom CSS.

Example:

```html
<!-- Style nth-child elements -->
<ul>
  <li class="lg:[&:nth-child(-n+3)]:hover:underline">Item</li>
</ul>

<!-- Style based on a custom class -->
<li class="[&.is-dragging]:cursor-grabbing [&.is-active]:bg-blue-50">
  Draggable item
</li>

<!-- Style a child element -->
<div class="[&>span]:text-blue-600 [&>p]:mt-2">
  <span>Blue text</span>
  <p>Spaced paragraph</p>
</div>

<!-- Style based on data attribute -->
<div class="[&>[data-active]+span]:text-blue-600">
  <span data-active></span>
  <span>This will be blue</span>
</div>

<!-- Combine with responsive and state variants -->
<div class="hover:[&>*]:opacity-100 md:[&:nth-child(2)]:col-span-2">
  Complex selector
</div>
```

---

### 30. What is the `@custom-variant` directive?

`@custom-variant` (Tailwind v4) lets you define reusable custom variants in CSS, replacing the `addVariant()` plugin API from v3. You can create variants for any CSS selector pattern.

Example:

```css
@import "tailwindcss";

/* Custom ARIA variants */
@custom-variant aria-asc (&[aria-sort="ascending"]);
@custom-variant aria-desc (&[aria-sort="descending"]);

/* Custom state variant */
@custom-variant selected (&[data-selected="true"]);
@custom-variant loading (&[data-loading]);

/* Selector-based dark mode */
@custom-variant dark (&:where(.dark, .dark *));
```

```html
<!-- Use the custom variants in HTML -->
<th
  class="aria-asc:after:content-['↑'] aria-desc:after:content-['↓']"
  aria-sort="ascending"
>
  Name
</th>

<div
  class="selected:bg-blue-50 selected:border-blue-500 border rounded p-4"
  data-selected="true"
>
  Selected item
</div>

<button class="loading:opacity-50 loading:cursor-wait" data-loading>
  Submit
</button>
```

---

### 31. How do you handle forms with Tailwind CSS?

Tailwind resets all form element styles via Preflight, so you style them from scratch with utilities. For a consistent cross-browser baseline, use the `@tailwindcss/forms` plugin.

Example (manual styling):

```html
<form class="space-y-5 max-w-md mx-auto">
  <!-- Text input -->
  <div>
    <label class="block text-sm font-medium text-gray-700 mb-1" for="name">
      Full Name
    </label>
    <input
      id="name"
      type="text"
      class="w-full border border-gray-300 rounded-lg px-3 py-2 text-sm
             focus:outline-none focus:ring-2 focus:ring-blue-400 focus:border-blue-400
             transition placeholder-gray-400"
      placeholder="Ahmed Hassan"
    />
  </div>

  <!-- Select -->
  <div>
    <label class="block text-sm font-medium text-gray-700 mb-1" for="role"
      >Role</label
    >
    <select
      id="role"
      class="w-full border border-gray-300 rounded-lg px-3 py-2 text-sm
             focus:outline-none focus:ring-2 focus:ring-blue-400 bg-white"
    >
      <option>Developer</option>
      <option>Designer</option>
      <option>Manager</option>
    </select>
  </div>

  <!-- Checkbox -->
  <label class="flex items-center gap-2 cursor-pointer">
    <input type="checkbox" class="w-4 h-4 accent-blue-500 rounded" />
    <span class="text-sm text-gray-700">I agree to the terms</span>
  </label>

  <button
    type="submit"
    class="w-full bg-blue-500 hover:bg-blue-600 text-white font-semibold
           py-2 rounded-lg transition focus:outline-none focus:ring-2 focus:ring-blue-300"
  >
    Submit
  </button>
</form>
```

---

### 32. What is the `@tailwindcss/forms` plugin?

The `@tailwindcss/forms` plugin provides a sensible cross-browser reset for form elements, giving inputs, selects, checkboxes, and radios a consistent base style that's easy to override with utilities.

Example:

```css
/* v3: install and add to config */
/* npm install @tailwindcss/forms */
```

```js
// tailwind.config.js (v3)
module.exports = {
  plugins: [require("@tailwindcss/forms")],
};
```

```css
/* v4: import directly */
@import "tailwindcss";
@plugin "@tailwindcss/forms";
```

```html
<!-- After the plugin, form elements have a clean base style -->
<input
  type="email"
  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm
         focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50"
  placeholder="you@example.com"
/>

<select
  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm
               focus:border-blue-300 focus:ring focus:ring-blue-200"
>
  <option>Option 1</option>
</select>
```

---

### 33. What is the `@tailwindcss/typography` plugin?

The `@tailwindcss/typography` plugin adds a `prose` class that applies beautiful typographic defaults to any block of HTML content — perfect for blog posts, documentation, and markdown-rendered content.

Example:

```css
/* v3 */
/* npm install @tailwindcss/typography */
```

```js
// tailwind.config.js (v3)
module.exports = {
  plugins: [require("@tailwindcss/typography")],
};
```

```html
<!-- Apply prose to any rich text container -->
<article class="prose prose-lg prose-blue max-w-none dark:prose-invert mx-auto">
  <h1>Getting Started with Tailwind CSS</h1>
  <p>Tailwind CSS is a utility-first framework...</p>
  <h2>Installation</h2>
  <pre><code>npm install tailwindcss</code></pre>
  <ul>
    <li>Fast build times</li>
    <li>Fully customizable</li>
  </ul>
  <blockquote>
    <p>Tailwind makes styling fun again.</p>
  </blockquote>
</article>

<!-- Prose size modifiers: prose-sm, prose-base, prose-lg, prose-xl, prose-2xl -->
<!-- Color modifiers: prose-gray, prose-blue, prose-indigo, etc. -->
```

---

### 34. How do you use Tailwind CSS with React or Next.js?

Tailwind integrates seamlessly with React and Next.js. In v4, setup is minimal — just install and import.

Example (Next.js v4 setup):

```bash
npm install tailwindcss @tailwindcss/vite
```

```css
/* app/globals.css */
@import "tailwindcss";
```

```tsx
// app/layout.tsx
import "./globals.css";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

```tsx
// app/page.tsx — use utilities directly in JSX
export default function HomePage() {
  return (
    <main className="min-h-screen bg-gray-50 flex items-center justify-center">
      <div className="bg-white rounded-2xl shadow-lg p-8 max-w-md w-full">
        <h1 className="text-3xl font-bold text-gray-900">Hello Tailwind</h1>
        <p className="mt-3 text-gray-500">
          Utility-first CSS in React is a joy.
        </p>
        <button
          className="mt-6 w-full bg-blue-500 hover:bg-blue-600 text-white
                           font-semibold py-3 rounded-xl transition"
        >
          Get Started
        </button>
      </div>
    </main>
  );
}
```

---

### 35. What is the `cn()` / `clsx` pattern used with Tailwind?

`cn()` is a utility function that combines `clsx` (conditional class merging) with `tailwind-merge` (resolves conflicting Tailwind classes). It's the standard pattern in component libraries like shadcn/ui.

Example:

```bash
npm install clsx tailwind-merge
```

```ts
// lib/utils.ts
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

```tsx
// Button component with conditional classes
import { cn } from "@/lib/utils";

type ButtonProps = {
  variant?: "primary" | "secondary" | "danger";
  size?: "sm" | "md" | "lg";
  disabled?: boolean;
  className?: string;
  children: React.ReactNode;
};

export function Button({
  variant = "primary",
  size = "md",
  disabled,
  className,
  children,
}: ButtonProps) {
  return (
    <button
      disabled={disabled}
      className={cn(
        "inline-flex items-center justify-center rounded-lg font-medium transition",
        {
          "bg-blue-500 hover:bg-blue-600 text-white": variant === "primary",
          "bg-gray-100 hover:bg-gray-200 text-gray-800":
            variant === "secondary",
          "bg-red-500 hover:bg-red-600 text-white": variant === "danger",
          "px-3 py-1.5 text-sm": size === "sm",
          "px-4 py-2 text-base": size === "md",
          "px-6 py-3 text-lg": size === "lg",
          "opacity-50 cursor-not-allowed": disabled,
        },
        className, // allows overrides from parent
      )}
    >
      {children}
    </button>
  );
}
```

---

### 36. What are `data-*` attribute variants in Tailwind?

Tailwind supports styling elements based on their `data-*` attributes using `data-[value]:` variants. This is useful for state-driven UI without adding extra CSS classes.

Example:

```html
<!-- Style based on data attribute value -->
<div
  data-state="open"
  class="data-[state=open]:block data-[state=closed]:hidden"
>
  Visible when open
</div>

<!-- Accordion item -->
<div
  data-state="open"
  class="border rounded-lg overflow-hidden
         data-[state=open]:shadow-md data-[state=closed]:shadow-none transition"
>
  <button
    class="w-full text-left px-4 py-3 font-medium
                 data-[state=open]:text-blue-600"
  >
    Question text
  </button>
  <div class="px-4 pb-4 data-[state=closed]:hidden">Answer content here.</div>
</div>

<!-- Loading state -->
<button
  data-loading="true"
  class="bg-blue-500 text-white px-4 py-2 rounded
         data-[loading=true]:opacity-60 data-[loading=true]:cursor-wait"
>
  Submit
</button>
```

---

### 37. What is the `not-*` variant in Tailwind CSS?

The `not-*` variant applies styles when a condition is **not** true. It maps to the CSS `:not()` pseudo-class and can be combined with other variants.

Example:

```html
<!-- Style all items except the last -->
<li class="not-last:border-b border-gray-200 px-4 py-3">Item</li>

<!-- Style all links that are not visited -->
<a class="not-visited:text-blue-600 visited:text-purple-600" href="#">Link</a>

<!-- Style inputs that are not disabled -->
<input
  class="not-disabled:hover:border-blue-400 disabled:opacity-50 disabled:cursor-not-allowed
         border rounded px-3 py-2 transition"
  type="text"
/>

<!-- Style elements that are not focused -->
<div class="not-focus-within:opacity-70 focus-within:opacity-100 transition">
  <input
    type="text"
    class="border rounded px-3 py-2 outline-none focus:ring-2"
  />
</div>
```

---

### 38. How do you create a design system with Tailwind CSS?

A Tailwind design system centralizes all design tokens in `@theme` (v4) or `tailwind.config.js` (v3), then builds reusable components on top using `@layer components` and the `cn()` pattern.

Example:

```css
/* design-system/tokens.css */
@import "tailwindcss";

@theme {
  /* Color palette */
  --color-primary-50: oklch(0.97 0.02 250);
  --color-primary-500: oklch(0.6 0.2 250);
  --color-primary-900: oklch(0.25 0.1 250);

  /* Typography scale */
  --font-sans: "Inter", system-ui, sans-serif;
  --font-display: "Satoshi", "sans-serif";

  /* Spacing scale */
  --spacing-18: 4.5rem;
  --spacing-128: 32rem;

  /* Border radius */
  --radius-xl: 1rem;
  --radius-2xl: 1.5rem;

  /* Shadows */
  --shadow-card: 0 4px 24px -4px rgba(0, 0, 0, 0.12);
}

@layer components {
  .btn {
    @apply inline-flex items-center justify-center rounded-lg font-medium transition focus:outline-none focus:ring-2 focus:ring-offset-2;
  }
  .btn-sm {
    @apply btn px-3 py-1.5 text-sm;
  }
  .btn-md {
    @apply btn px-4 py-2 text-base;
  }
  .btn-primary {
    @apply btn-md bg-primary-500 hover:bg-primary-600 text-white focus:ring-primary-400;
  }
  .card {
    @apply bg-white rounded-2xl shadow-card p-6;
  }
  .input {
    @apply w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-primary-400 focus:border-primary-400 transition;
  }
}
```

---

### 39. What are the performance best practices for Tailwind CSS?

Tailwind is already highly optimized — it only ships CSS for classes you actually use. But there are additional practices to keep your build lean and fast.

Example:

```js
// ✅ v3: be specific with content paths — don't scan node_modules
// tailwind.config.js
module.exports = {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
    // ❌ Avoid: './**/*.{js,ts}' — too broad
  ],
};
```

```js
// ✅ Always use complete class names — never dynamic string construction
// ❌ BAD — Tailwind can't detect this at build time
const size = "lg";
<div class={`text-${size}`} />;

// ✅ GOOD — full class names are statically detectable
const classes = isLarge ? "text-lg" : "text-sm";
<div class={classes} />;
```

```html
<!-- ✅ Use @apply only for truly repeated patterns, not one-offs -->
<!-- ✅ Prefer gap over space-x/space-y for flex/grid layouts -->
<!-- ✅ Use prose for long-form content instead of styling every element -->
<!-- ✅ Enable JIT (default in v3+) for on-demand generation -->
<!-- ✅ Use PurgeCSS-compatible patterns in JS frameworks -->

<!-- ✅ Lazy load heavy components to reduce initial CSS scope -->
<div class="hidden lg:block">Desktop-only content</div>
```

---

### 40. What is the difference between `hidden`, `invisible`, and `opacity-0` in Tailwind?

All three hide an element visually, but they behave very differently in terms of layout and interactivity.

| Class       | CSS equivalent       | Takes up space? | Interactive? | Transition-able? |
| ----------- | -------------------- | --------------- | ------------ | ---------------- |
| `hidden`    | `display: none`      | ❌ No           | ❌ No        | ❌ No            |
| `invisible` | `visibility: hidden` | ✅ Yes          | ❌ No        | ❌ No            |
| `opacity-0` | `opacity: 0`         | ✅ Yes          | ✅ Yes       | ✅ Yes           |

Example:

```html
<!-- hidden: removed from layout entirely -->
<div class="hidden md:block">Only visible on md and above</div>

<!-- invisible: hidden but holds its space -->
<div class="invisible group-hover:visible transition">
  Appears on parent hover, space always reserved
</div>

<!-- opacity-0: invisible but still interactive, great for transitions -->
<div class="opacity-0 hover:opacity-100 transition-opacity duration-300">
  Fades in on hover
</div>

<!-- Common pattern: fade-in animation -->
<div
  class="opacity-0 translate-y-4 transition-all duration-500
            data-[visible=true]:opacity-100 data-[visible=true]:translate-y-0"
  data-visible="true"
>
  Animated entrance
</div>
```

---

> Made with ❤️ — Click ⭐ if this helped you prepare for your next interview!
