# modern HTML & HTML5 interview Questions & Answers

Click ⭐ if you like the project. Follow for more updates.

---

# HTML & HTML5 Interview Questions 🌐

| #   | Question                                                                           |
| --- | ---------------------------------------------------------------------------------- |
| 1   | What is HTML?                                                                      |
| 2   | What is the difference between HTML and HTML5?                                     |
| 3   | What is a DOCTYPE and why is it important?                                         |
| 4   | What are semantic elements in HTML5?                                               |
| 5   | What is the difference between `<div>` and `<span>`?                               |
| 6   | What are void elements in HTML?                                                    |
| 7   | What is the difference between `id` and `class` attributes?                        |
| 8   | What are data attributes in HTML5?                                                 |
| 9   | What is the difference between `localStorage` and `sessionStorage`?                |
| 10  | What is the `<canvas>` element?                                                    |
| 11  | What is the `<svg>` element and how does it differ from `<canvas>`?                |
| 12  | What are Web Workers?                                                              |
| 13  | What is the `<picture>` element used for?                                          |
| 14  | What are the new form input types introduced in HTML5?                             |
| 15  | What is the `required` attribute in HTML5 forms?                                   |
| 16  | What is the difference between `<script>`, `<script async>`, and `<script defer>`? |
| 17  | What is the `<meta>` tag used for?                                                 |
| 18  | What is the difference between `<strong>` and `<b>`, and `<em>` and `<i>`?         |
| 19  | What is the `<template>` element in HTML5?                                         |
| 20  | What is the `contenteditable` attribute?                                           |
| 21  | What is the difference between `<section>`, `<article>`, and `<div>`?              |
| 22  | What is the `<details>` and `<summary>` element?                                   |
| 23  | What is the `<dialog>` element?                                                    |
| 24  | What is the `srcset` attribute in `<img>`?                                         |
| 25  | What is the difference between `GET` and `POST` in HTML forms?                     |
| 26  | What is the `<link>` tag used for?                                                 |
| 27  | What is the `<base>` tag?                                                          |
| 28  | What is the `<noscript>` tag?                                                      |
| 29  | What is the `<iframe>` element and what are its security concerns?                 |
| 30  | What is the `<progress>` and `<meter>` element?                                    |
| 31  | What is the `<output>` element?                                                    |
| 32  | What is the `<datalist>` element?                                                  |
| 33  | What is the `<figure>` and `<figcaption>` element?                                 |
| 34  | What is the `<time>` element?                                                      |
| 35  | What is the `<mark>` element?                                                      |
| 36  | What is the `<wbr>` element?                                                       |
| 37  | What is the `<abbr>` element?                                                      |
| 38  | What is the `<blockquote>` vs `<q>` element?                                       |
| 39  | What is the `<map>` and `<area>` element?                                          |
| 40  | What is the `<track>` element used for in HTML5?                                   |

---

# HTML & HTML5 Questions & Answers

---

### 1. What is HTML?

HTML (HyperText Markup Language) is the standard markup language used to create and structure content on the web. It defines the structure of web pages using a system of elements represented by tags.

Example:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>My Page</title>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
```

---

### 2. What is the difference between HTML and HTML5?

HTML5 is the latest version of HTML and introduced many new features over older versions:

- **New semantic elements**: `<header>`, `<footer>`, `<article>`, `<section>`, `<nav>`, `<aside>`
- **Multimedia support**: Native `<audio>` and `<video>` elements without plugins
- **Graphics**: `<canvas>` and inline `<svg>` support
- **APIs**: Web Storage, Web Workers, Geolocation, Drag & Drop
- **New form input types**: `email`, `date`, `range`, `color`, `search`, etc.
- **Removed deprecated elements**: `<font>`, `<center>`, `<frame>` are no longer valid

Example:

```html
<!-- HTML5 semantic structure -->
<header>
  <nav>...</nav>
</header>
<main>
  <article>...</article>
  <aside>...</aside>
</main>
<footer>...</footer>
```

---

### 3. What is a DOCTYPE and why is it important?

The `<!DOCTYPE>` declaration tells the browser which version of HTML the document is written in. It must be the very first line of an HTML document. In HTML5, the doctype is simplified to just `<!DOCTYPE html>`.

Without a proper DOCTYPE, browsers may render the page in "quirks mode", which can cause inconsistent rendering across browsers.

Example:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>
  <body>
    <p>Rendered in standards mode.</p>
  </body>
</html>
```

---

### 4. What are semantic elements in HTML5?

Semantic elements clearly describe their meaning to both the browser and the developer. They improve accessibility, SEO, and code readability.

Common semantic elements:

- `<header>` — introductory content or navigation
- `<nav>` — navigation links
- `<main>` — main content of the document
- `<article>` — self-contained content
- `<section>` — thematic grouping of content
- `<aside>` — content tangentially related to the main content
- `<footer>` — footer of a section or page

Example:

```html
<article>
  <header>
    <h2>Article Title</h2>
  </header>
  <section>
    <p>Article content goes here.</p>
  </section>
  <footer>
    <p>Published on <time datetime="2024-01-01">January 1, 2024</time></p>
  </footer>
</article>
```

---

### 5. What is the difference between `<div>` and `<span>`?

- `<div>` is a **block-level** element. It takes up the full width available and starts on a new line. Used for grouping larger sections of content.
- `<span>` is an **inline** element. It only takes up as much width as its content. Used for styling small pieces of text or inline content.

Example:

```html
<div style="background: lightblue;">
  This is a block-level div.
  <span style="color: red;">This is an inline span.</span>
</div>
```

---

### 6. What are void elements in HTML?

Void elements are HTML elements that do not have closing tags and cannot have child content. They are self-closing by nature.

Common void elements: `<br>`, `<hr>`, `<img>`, `<input>`, `<link>`, `<meta>`, `<area>`, `<base>`, `<col>`, `<embed>`, `<param>`, `<source>`, `<track>`, `<wbr>`

Example:

```html
<img src="photo.jpg" alt="A photo" />
<input type="text" placeholder="Enter name" />
<br />
<hr />
```

---

### 7. What is the difference between `id` and `class` attributes?

- `id`: Uniquely identifies a **single** element on the page. Must be unique within the document. Used with `#` in CSS and JavaScript.
- `class`: Can be applied to **multiple** elements. Used to group elements for shared styling or behavior. Used with `.` in CSS.

Example:

```html
<div id="main-header">Unique Header</div>

<p class="highlight">First highlighted paragraph.</p>
<p class="highlight">Second highlighted paragraph.</p>

<style>
  #main-header {
    font-size: 2rem;
  }
  .highlight {
    background-color: yellow;
  }
</style>
```

---

### 8. What are data attributes in HTML5?

Data attributes allow you to store custom data directly on HTML elements using the `data-*` syntax. They are accessible via JavaScript using `dataset`.

Example:

```html
<button data-user-id="42" data-role="admin">Click Me</button>

<script>
  const btn = document.querySelector("button");
  console.log(btn.dataset.userId); // "42"
  console.log(btn.dataset.role); // "admin"
</script>
```

---

### 9. What is the difference between `localStorage` and `sessionStorage`?

Both are part of the Web Storage API introduced in HTML5 and store key-value pairs in the browser.

- `localStorage`: Data persists even after the browser is closed. Shared across all tabs of the same origin.
- `sessionStorage`: Data is cleared when the browser tab is closed. Scoped to the current tab only.

Example:

```html
<script>
  // localStorage - persists across sessions
  localStorage.setItem("username", "Ahmed");
  console.log(localStorage.getItem("username")); // "Ahmed"

  // sessionStorage - cleared on tab close
  sessionStorage.setItem("token", "abc123");
  console.log(sessionStorage.getItem("token")); // "abc123"
</script>
```

---

### 10. What is the `<canvas>` element?

The `<canvas>` element provides a drawable region in HTML where you can render 2D graphics, animations, and game visuals using JavaScript's Canvas API.

Example:

```html
<canvas id="myCanvas" width="300" height="150"></canvas>

<script>
  const canvas = document.getElementById("myCanvas");
  const ctx = canvas.getContext("2d");

  ctx.fillStyle = "steelblue";
  ctx.fillRect(20, 20, 150, 100);

  ctx.fillStyle = "white";
  ctx.font = "20px Arial";
  ctx.fillText("Hello Canvas!", 30, 80);
</script>
```

---

### 11. What is the `<svg>` element and how does it differ from `<canvas>`?

SVG (Scalable Vector Graphics) is an XML-based format for vector graphics. Unlike `<canvas>`, SVG elements are part of the DOM and can be styled with CSS and manipulated with JavaScript.

| Feature     | `<canvas>`              | `<svg>`                        |
| ----------- | ----------------------- | ------------------------------ |
| Type        | Raster (pixel-based)    | Vector (shape-based)           |
| DOM access  | No (drawn pixels)       | Yes (each shape is a DOM node) |
| Scalability | Loses quality on zoom   | Scales without quality loss    |
| Best for    | Games, image processing | Icons, charts, illustrations   |

Example:

```html
<svg width="200" height="100">
  <rect x="10" y="10" width="180" height="80" fill="steelblue" rx="10" />
  <text x="50" y="60" fill="white" font-size="20">Hello SVG</text>
</svg>
```

---

### 12. What are Web Workers?

Web Workers allow JavaScript to run in background threads, separate from the main UI thread. This prevents heavy computations from blocking the user interface.

Example:

```html
<!-- main.js -->
<script>
  const worker = new Worker("worker.js");

  worker.postMessage(1000000);

  worker.onmessage = function (e) {
    console.log("Result from worker:", e.data);
  };
</script>
```

```js
// worker.js
self.onmessage = function (e) {
  let sum = 0;
  for (let i = 0; i < e.data; i++) sum += i;
  self.postMessage(sum);
};
```

---

### 13. What is the `<picture>` element used for?

The `<picture>` element provides multiple image sources for different screen sizes or formats, giving the browser flexibility to choose the most appropriate one. It's used for responsive images and modern format support (like WebP).

Example:

```html
<picture>
  <source media="(min-width: 800px)" srcset="large.webp" type="image/webp" />
  <source media="(min-width: 400px)" srcset="medium.jpg" />
  <img src="small.jpg" alt="A responsive image" />
</picture>
```

---

### 14. What are the new form input types introduced in HTML5?

HTML5 introduced many new `<input>` types that provide built-in validation and better UX on mobile devices.

New types: `email`, `url`, `tel`, `number`, `range`, `date`, `time`, `datetime-local`, `month`, `week`, `color`, `search`

Example:

```html
<form>
  <input type="email" placeholder="Enter email" required />
  <input type="date" />
  <input type="range" min="0" max="100" value="50" />
  <input type="color" value="#ff0000" />
  <input type="number" min="1" max="10" />
  <button type="submit">Submit</button>
</form>
```

---

### 15. What is the `required` attribute in HTML5 forms?

The `required` attribute is a built-in HTML5 form validation attribute that prevents form submission if the field is empty. No JavaScript is needed.

Example:

```html
<form>
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" required />

  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required />

  <button type="submit">Submit</button>
</form>
```

---

### 16. What is the difference between `<script>`, `<script async>`, and `<script defer>`?

All three load external JavaScript files, but they differ in when the script is executed relative to HTML parsing.

- `<script>`: Blocks HTML parsing while the script is fetched and executed.
- `<script async>`: Downloads the script in parallel, executes it as soon as it's ready (may interrupt parsing).
- `<script defer>`: Downloads the script in parallel, executes it only after the HTML is fully parsed.

Example:

```html
<!-- Blocks rendering -->
<script src="app.js"></script>

<!-- Executes as soon as downloaded, order not guaranteed -->
<script async src="analytics.js"></script>

<!-- Executes after DOM is ready, order preserved -->
<script defer src="main.js"></script>
```

---

### 17. What is the `<meta>` tag used for?

The `<meta>` tag provides metadata about the HTML document — information not displayed on the page but used by browsers, search engines, and social media platforms.

Example:

```html
<head>
  <!-- Character encoding -->
  <meta charset="UTF-8" />

  <!-- Responsive viewport -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- SEO description -->
  <meta name="description" content="HTML5 interview questions and answers." />

  <!-- Open Graph for social sharing -->
  <meta property="og:title" content="HTML5 Interview Questions" />
  <meta property="og:image" content="thumbnail.png" />
</head>
```

---

### 18. What is the difference between `<strong>` and `<b>`, and `<em>` and `<i>`?

- `<strong>`: Indicates **important** content. Has semantic meaning — screen readers emphasize it.
- `<b>`: Makes text **bold** visually only. No semantic meaning.
- `<em>`: Indicates _emphasized_ content. Screen readers stress it.
- `<i>`: Makes text _italic_ visually only. No semantic meaning.

Example:

```html
<p><strong>Warning:</strong> This action cannot be undone.</p>
<p>The term <em>semantic HTML</em> refers to meaningful markup.</p>

<p><b>Bold text</b> with no special meaning.</p>
<p><i>Italic text</i> with no special meaning.</p>
```

---

### 19. What is the `<template>` element in HTML5?

The `<template>` element holds HTML content that is not rendered when the page loads. It can be cloned and inserted into the DOM dynamically using JavaScript.

Example:

```html
<template id="card-template">
  <div class="card">
    <h3 class="card-title"></h3>
    <p class="card-body"></p>
  </div>
</template>

<div id="container"></div>

<script>
  const template = document.getElementById("card-template");
  const clone = template.content.cloneNode(true);

  clone.querySelector(".card-title").textContent = "Hello";
  clone.querySelector(".card-body").textContent = "This is a card.";

  document.getElementById("container").appendChild(clone);
</script>
```

---

### 20. What is the `contenteditable` attribute?

The `contenteditable` attribute makes any HTML element editable directly in the browser, turning it into an inline editor without needing an `<input>` or `<textarea>`.

Example:

```html
<div contenteditable="true" style="border: 1px solid #ccc; padding: 10px;">
  Click here and start typing to edit this content...
</div>
```

---

### 21. What is the difference between `<section>`, `<article>`, and `<div>`?

- `<section>`: A thematic grouping of content, typically with a heading. Part of the document outline.
- `<article>`: Self-contained, independently distributable content (e.g., a blog post, news article, comment).
- `<div>`: A generic container with no semantic meaning. Used purely for layout and styling.

Example:

```html
<section>
  <h2>Latest News</h2>

  <article>
    <h3>HTML5 Released</h3>
    <p>HTML5 brings many new features to the web...</p>
  </article>

  <article>
    <h3>CSS Grid is Awesome</h3>
    <p>CSS Grid makes layout much easier...</p>
  </article>
</section>

<div class="wrapper">
  <!-- Generic layout container -->
</div>
```

---

### 22. What is the `<details>` and `<summary>` element?

`<details>` creates a disclosure widget that the user can open and close. `<summary>` provides the visible heading for the widget. No JavaScript required.

Example:

```html
<details>
  <summary>What is HTML5?</summary>
  <p>
    HTML5 is the latest version of the HyperText Markup Language, introducing
    new semantic elements, APIs, and multimedia support.
  </p>
</details>
```

---

### 23. What is the `<dialog>` element?

The `<dialog>` element represents a dialog box or modal window. It can be shown and hidden using the `open` attribute or JavaScript's `.showModal()` and `.close()` methods.

Example:

```html
<dialog id="myDialog">
  <p>This is a dialog box!</p>
  <button id="closeBtn">Close</button>
</dialog>

<button id="openBtn">Open Dialog</button>

<script>
  const dialog = document.getElementById("myDialog");
  document.getElementById("openBtn").addEventListener("click", () => {
    dialog.showModal();
  });
  document.getElementById("closeBtn").addEventListener("click", () => {
    dialog.close();
  });
</script>
```

---

### 24. What is the `srcset` attribute in `<img>`?

The `srcset` attribute allows you to provide multiple image sources at different resolutions or sizes. The browser picks the most appropriate one based on the device's screen density or viewport size.

Example:

```html
<img
  src="image-400.jpg"
  srcset="image-400.jpg 400w, image-800.jpg 800w, image-1200.jpg 1200w"
  sizes="(max-width: 600px) 400px, (max-width: 1000px) 800px, 1200px"
  alt="Responsive image example"
/>
```

---

### 25. What is the difference between `GET` and `POST` in HTML forms?

- `GET`: Appends form data to the URL as query parameters. Visible in the browser address bar. Used for non-sensitive, idempotent requests (e.g., search).
- `POST`: Sends form data in the request body. Not visible in the URL. Used for sensitive data or actions that change server state (e.g., login, file upload).

Example:

```html
<!-- GET - data visible in URL: /search?query=html5 -->
<form action="/search" method="GET">
  <input type="text" name="query" />
  <button type="submit">Search</button>
</form>

<!-- POST - data sent in request body -->
<form action="/login" method="POST">
  <input type="email" name="email" />
  <input type="password" name="password" />
  <button type="submit">Login</button>
</form>
```

---

### 26. What is the `<link>` tag used for?

The `<link>` tag defines relationships between the current document and external resources. Most commonly used to link CSS stylesheets, but also used for favicons, preloading, and canonical URLs.

Example:

```html
<head>
  <!-- Stylesheet -->
  <link rel="stylesheet" href="styles.css" />

  <!-- Favicon -->
  <link rel="icon" href="favicon.ico" type="image/x-icon" />

  <!-- Preload a font -->
  <link
    rel="preload"
    href="font.woff2"
    as="font"
    type="font/woff2"
    crossorigin
  />

  <!-- Canonical URL for SEO -->
  <link rel="canonical" href="https://example.com/page" />
</head>
```

---

### 27. What is the `<base>` tag?

The `<base>` tag specifies a base URL for all relative URLs in the document. It also sets the default target for all links. There can only be one `<base>` element per document.

Example:

```html
<head>
  <base href="https://example.com/" target="_blank" />
</head>
<body>
  <!-- This resolves to https://example.com/about -->
  <a href="about">About Us</a>

  <!-- This resolves to https://example.com/images/logo.png -->
  <img src="images/logo.png" alt="Logo" />
</body>
```

---

### 28. What is the `<noscript>` tag?

The `<noscript>` tag defines content to be displayed when JavaScript is disabled in the browser or not supported. It provides a fallback experience for users without JavaScript.

Example:

```html
<noscript>
  <p style="color: red; text-align: center;">
    ⚠️ JavaScript is disabled. Please enable JavaScript for the best experience.
  </p>
</noscript>

<script>
  document.write("<p>JavaScript is enabled!</p>");
</script>
```

---

### 29. What is the `<iframe>` element and what are its security concerns?

The `<iframe>` element embeds another HTML document within the current page. While useful for embedding maps, videos, or third-party widgets, it comes with security risks.

Security concerns:

- **Clickjacking**: Malicious sites can embed your page in an iframe to trick users.
- **XSS**: Embedded content can execute scripts in the parent context.
- **Mitigation**: Use the `sandbox` attribute and `X-Frame-Options` / `Content-Security-Policy` headers.

Example:

```html
<!-- Basic iframe -->
<iframe src="https://www.example.com" width="600" height="400"></iframe>

<!-- Sandboxed iframe - restricts capabilities -->
<iframe
  src="https://www.example.com"
  sandbox="allow-scripts allow-same-origin"
  width="600"
  height="400"
></iframe>
```

---

### 30. What is the `<progress>` and `<meter>` element?

- `<progress>`: Represents the completion progress of a task (e.g., file upload, loading).
- `<meter>`: Represents a scalar measurement within a known range (e.g., disk usage, score).

Example:

```html
<!-- Progress bar: 70% complete -->
<label>Upload Progress:</label>
<progress value="70" max="100">70%</progress>

<!-- Meter: disk usage -->
<label>Disk Usage:</label>
<meter value="6" min="0" max="10" low="3" high="8" optimum="2">
  6 out of 10
</meter>
```

---

### 31. What is the `<output>` element?

The `<output>` element represents the result of a calculation or user action. It is typically used with forms to display computed values.

Example:

```html
<form oninput="result.value = parseInt(a.value) + parseInt(b.value)">
  <input type="number" id="a" value="0" /> +
  <input type="number" id="b" value="0" /> =
  <output name="result" for="a b">0</output>
</form>
```

---

### 32. What is the `<datalist>` element?

The `<datalist>` element provides a list of predefined options for an `<input>` element, offering autocomplete suggestions while still allowing free-text input.

Example:

```html
<label for="browser">Choose a browser:</label>
<input list="browsers" id="browser" name="browser" />

<datalist id="browsers">
  <option value="Chrome" />
  <option value="Firefox" />
  <option value="Safari" />
  <option value="Edge" />
  <option value="Opera" />
</datalist>
```

---

### 33. What is the `<figure>` and `<figcaption>` element?

`<figure>` represents self-contained content like images, diagrams, or code snippets. `<figcaption>` provides a caption or description for the figure. Together they semantically associate an image with its caption.

Example:

```html
<figure>
  <img src="html5-logo.png" alt="HTML5 Logo" width="200" />
  <figcaption>Figure 1: The official HTML5 logo.</figcaption>
</figure>
```

---

### 34. What is the `<time>` element?

The `<time>` element represents a specific time or date in a machine-readable format using the `datetime` attribute. It helps search engines and browsers understand dates and times.

Example:

```html
<p>
  The event starts on
  <time datetime="2024-06-15T09:00">June 15, 2024 at 9:00 AM</time>.
</p>

<p>Published <time datetime="2024-01-01">January 1st</time>.</p>
```

---

### 35. What is the `<mark>` element?

The `<mark>` element highlights text that is relevant or of special interest in the current context, like search results. It renders with a yellow background by default.

Example:

```html
<p>Search results for "HTML5":</p>
<p>
  <mark>HTML5</mark> is the latest version of the HyperText Markup Language. It
  introduces many new features over previous versions of <mark>HTML</mark>.
</p>
```

---

### 36. What is the `<wbr>` element?

The `<wbr>` (Word Break Opportunity) element suggests a position within text where the browser may optionally break a line. Useful for long URLs or strings that shouldn't break at random points.

Example:

```html
<p>
  Visit us at: https://www.example<wbr />.com/very<wbr />/long<wbr />/url<wbr />/path
</p>
```

---

### 37. What is the `<abbr>` element?

The `<abbr>` element represents an abbreviation or acronym. The `title` attribute provides the full expansion, which appears as a tooltip on hover and is read by screen readers.

Example:

```html
<p>
  <abbr title="HyperText Markup Language">HTML</abbr> is the backbone of the
  web.
</p>
<p><abbr title="Cascading Style Sheets">CSS</abbr> handles the styling.</p>
```

---

### 38. What is the difference between `<blockquote>` and `<q>`?

- `<blockquote>`: Used for **long quotations** that are displayed as a block, typically indented. Use the `cite` attribute to reference the source URL.
- `<q>`: Used for **short inline quotations**. Browsers automatically add quotation marks around the content.

Example:

```html
<!-- Block quotation -->
<blockquote cite="https://developer.mozilla.org">
  <p>
    HTML5 is a markup language used for structuring and presenting content on
    the World Wide Web.
  </p>
</blockquote>

<!-- Inline quotation -->
<p>
  As MDN states,
  <q cite="https://developer.mozilla.org"
    >HTML5 is the latest evolution of the standard that defines HTML.</q
  >
</p>
```

---

### 39. What is the `<map>` and `<area>` element?

The `<map>` element defines an image map — an image with clickable regions. Each clickable region is defined by an `<area>` element with coordinates and a link.

Example:

```html
<img
  src="world-map.jpg"
  alt="World Map"
  usemap="#worldmap"
  width="600"
  height="400"
/>

<map name="worldmap">
  <area shape="rect" coords="50,50,200,150" href="/europe" alt="Europe" />
  <area shape="circle" coords="400,200,80" href="/asia" alt="Asia" />
  <area
    shape="poly"
    coords="100,300,150,350,80,380"
    href="/africa"
    alt="Africa"
  />
</map>
```

---

### 40. What is the `<track>` element used for in HTML5?

The `<track>` element is used with `<video>` and `<audio>` elements to provide timed text tracks such as subtitles, captions, descriptions, chapters, or metadata.

Example:

```html
<video controls width="640">
  <source src="movie.mp4" type="video/mp4" />

  <!-- Subtitles in English -->
  <track
    kind="subtitles"
    src="subtitles-en.vtt"
    srclang="en"
    label="English"
    default
  />

  <!-- Subtitles in Arabic -->
  <track kind="subtitles" src="subtitles-ar.vtt" srclang="ar" label="العربية" />

  Your browser does not support the video element.
</video>
```

---

> Made with ❤️ — Click ⭐ if this helped you prepare for your next interview!
