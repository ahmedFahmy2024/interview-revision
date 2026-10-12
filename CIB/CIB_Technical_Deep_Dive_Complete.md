# CIB Bank — Complete Technical Deep Dive
**Senior Frontend Developer (React JS) — 21 July 2026, 6:00 PM, Microsoft Teams**

Exhaustive coverage across all seven fields. Skim the table of contents, drill into what feels shaky, skip what you already know cold.

## Contents
1. [JavaScript Fundamentals](#1-javascript-fundamentals)
2. [React + TypeScript](#2-react--typescript)
3. [Styling / CSS](#3-styling--css)
4. [Testing](#4-testing)
5. [REST API Integration](#5-rest-api-integration)
6. [System Design](#6-system-design)
7. [Git](#7-git)

---

## 1. JavaScript Fundamentals

### 1.1 Closures
A closure is a function retaining access to its enclosing scope's variables after that scope has returned.
```js
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}
```
`timer` persists across calls purely because the inner function closes over it — the mechanism behind a debounced autosave.

### 1.2 `var` vs `let`/`const`, hoisting, TDZ
`var` is function-scoped, hoisted and initialized to `undefined`. `let`/`const` are block-scoped, hoisted but not initialized — the gap before their declaration line is the **temporal dead zone**, where access throws `ReferenceError`. `const` freezes the binding, not the value — `const arr = []; arr.push(1)` is legal.

### 1.3 The event loop
```js
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// 1, 4, 3, 2
```
Call stack runs sync code first. The **microtask queue** (promise callbacks) fully drains after each sync block, before any macrotask. The **macrotask queue** (`setTimeout`, I/O) runs one task per tick, with microtasks drained again in between.

### 1.4 Promises & async/await
- `Promise.all` — fail-fast, rejects if any promise rejects.
- `Promise.allSettled` — always resolves once everything settles, giving per-promise status — use when one failure shouldn't sink the whole batch.
- `Promise.race` — settles as soon as the first promise settles (useful for timeouts: race a fetch against a `setTimeout` rejection).
- Sequential `await` in a loop creates an accidental waterfall; kick off all promises first, then `Promise.all` them for true parallelism.
- try/catch around `await` for error handling; an unhandled rejection in an `async` function becomes a rejected promise, not a thrown synchronous error — callers still need to catch it.

### 1.5 `this`, `call`/`apply`/`bind`, arrow vs regular functions
Regular functions get `this` from *how* they're called (implicit binding via the calling object, or `undefined`/global in strict mode if called bare). Arrow functions don't have their own `this` — they inherit it lexically from the enclosing scope, which is exactly why they're the default choice for callbacks inside class-like or object methods. `call`/`apply` invoke a function immediately with an explicit `this`; `bind` returns a new function permanently bound to a given `this`.

### 1.6 `==` vs `===`, type coercion
`===` compares value and type with no conversion. `==` coerces operands to a common type first, which produces surprising results (`'' == 0` is `true`, `null == undefined` is `true` but `null === undefined` is `false`). Default to `===` always; the only common intentional use of `==` is checking `value == null` to catch both `null` and `undefined` in one comparison.

### 1.7 Debounce vs throttle
Debounce delays execution until a pause in events (fires once, after the burst stops — right for autosave, search-as-you-type). Throttle guarantees execution at most once per interval regardless of event frequency (right for scroll/resize handlers, where you want steady updates during continuous activity, not just at the end).
```js
function throttle(fn, limit) {
  let inThrottle = false;
  return (...args) => {
    if (inThrottle) return;
    fn(...args);
    inThrottle = true;
    setTimeout(() => (inThrottle = false), limit);
  };
}
```

### 1.8 Higher-order functions & array methods
`map` transforms each element (same length out), `filter` selects a subset (predicate-based), `reduce` folds into a single accumulated value — the most general of the three, technically able to implement `map`/`filter` itself. Know the difference between mutating (`push`, `sort`, `splice`) and non-mutating (`map`, `filter`, `slice`, spread) array operations — mutating the original array unexpectedly is a classic React bug source (state mutated in place doesn't trigger a re-render).

### 1.9 Destructuring, spread/rest
```js
const { name, ...rest } = user;   // rest = everything except name
const merged = { ...defaults, ...overrides }; // shallow merge, overrides win
```
Spread does a **shallow** copy/merge — nested objects are still shared references, a common source of "I updated a copy but the original changed too" bugs.

### 1.10 Shallow vs deep copy
Shallow copy (`{...obj}`, `Object.assign`, `Array.prototype.slice`) copies top-level properties only; nested objects/arrays remain shared references. Deep copy duplicates everything recursively (`structuredClone(obj)` natively, or a library like lodash's `cloneDeep` for older environments). Relevant directly to React state: mutating a nested field of state in place and calling `setState` with the same top-level reference won't trigger a re-render, because React does a shallow reference comparison.

### 1.11 Prototypal inheritance
Every object has an internal link (`[[Prototype]]`, accessible via `Object.getPrototypeOf`/`__proto__`) to another object it delegates property lookups to when a property isn't found on itself — the **prototype chain**. `class` syntax in modern JS is sugar over this same mechanism, not a separate inheritance model.

### 1.12 Generators & iterators
A generator function (`function*`) can pause and resume execution, yielding a sequence of values lazily via `yield` — useful for iterating large or infinite sequences without computing them all up front. Any object implementing `Symbol.iterator` (arrays, strings, Maps, Sets, and generator return values) works with `for...of` and spread syntax.

### 1.13 ES Modules vs CommonJS
ESM (`import`/`export`) is statically analyzable — bindings are resolved at parse time, enabling tree-shaking (unused exports get dropped from the bundle) and top-level `await`. CommonJS (`require`/`module.exports`) resolves dynamically at runtime, which is why it can't be tree-shaken the same way. Next.js and modern bundlers default to ESM; you'll still see CommonJS in older Node tooling/config files.

### 1.14 Memory & closures
Closures keep their captured variables alive as long as the closure itself is reachable — which is normally fine, but can become a leak if a long-lived closure (e.g., an event listener never removed) holds a reference to something large (a big DOM tree, a large dataset) that should have been garbage collected. Always pair `addEventListener` with `removeEventListener` in a cleanup function (React: the `useEffect` return function).

### 1.15 Optional chaining & nullish coalescing
```js
const city = user?.address?.city ?? 'Unknown';
```
`?.` short-circuits to `undefined` instead of throwing if any link in the chain is `null`/`undefined`. `??` falls back only on `null`/`undefined` — unlike `||`, it doesn't incorrectly fall back on legitimate falsy values like `0` or `''`.

### 1.16 Symbols, WeakMap/WeakSet
`Symbol` creates a guaranteed-unique value, commonly used as a non-colliding object property key. `WeakMap`/`WeakSet` hold their keys weakly — if nothing else references a key, it can be garbage collected even while still "in" the WeakMap, making them useful for attaching metadata to objects (like DOM nodes) without preventing cleanup.

### 1.17 Time complexity of common operations
| Operation | Complexity |
|---|---|
| Array push/pop | O(1) amortized |
| Array shift/unshift | O(n) |
| Array index access | O(1) |
| Array `.includes()`/`.indexOf()` | O(n) |
| Object/Map get/set | O(1) average |
| Array `.sort()` | O(n log n) |
| Set/Map `.has()` | O(1) |

### 1.18 Recursion & memoization
Recursion trades iteration for a call-stack-based solution — natural for tree/graph traversal, risky for very deep inputs (stack overflow; JS has no guaranteed tail-call optimization in most engines). Memoization caches a pure function's output keyed by its input, trading memory for avoiding recomputation — the general-purpose version of what `useMemo` does for a single component render.

### 1.19 Event delegation
Instead of attaching a listener to every child element, attach one listener to a common ancestor and inspect `event.target` to determine which child triggered it — fewer listeners, and it automatically covers dynamically added children. This is the mechanism behind how a lot of table/list interaction handlers work without re-binding on every row.

### 1.20 Storage: localStorage vs sessionStorage vs cookies
`localStorage` persists until explicitly cleared, `sessionStorage` clears when the tab closes — both are synchronous, ~5-10MB, and JS-only (never sent to the server automatically). Cookies are small (~4KB), sent automatically with every same-origin request (relevant to auth token design), and can be marked `httpOnly` (inaccessible to JS, protecting against XSS token theft) and `secure`/`sameSite` for additional protection.

---

## 2. React + TypeScript

### 2.1 Virtual DOM & reconciliation
React keeps a lightweight in-memory tree representation of the UI. On state change, it builds a new tree, diffs it against the previous one (reconciliation), and computes the minimal set of real DOM mutations needed — cheaper than re-rendering the actual DOM wholesale, since DOM operations are comparatively expensive.

### 2.2 Keys in lists
Keys let React match array items between renders to decide what to update, reorder, or remove, instead of re-creating the whole list. Using array index as a key breaks this when items are reordered/inserted/removed — React can misattribute state to the wrong item. Always use a stable, unique identifier (an ID from the data) instead.

### 2.3 Rules of hooks
Hooks must be called at the top level (never inside conditions, loops, or nested functions) and only from React function components or custom hooks. The reason: React tracks hooks by call *order*, not by name — a conditional hook call shifts every subsequent hook's position between renders and silently corrupts state.

### 2.4 Custom hooks
A function prefixed `use` that composes other hooks to extract and reuse stateful logic — e.g., a `useDebouncedValue(value, delay)` hook wrapping `useState` + `useEffect`. The value is sharing *behavior*, not shared state (each component using the hook gets its own independent instance of that state).

### 2.5 `useReducer` vs `useState`
`useState` is best for simple, independent pieces of state. `useReducer` centralizes more complex state transitions into a single reducer function — better when several fields update together in response to the same action, or when the next state depends heavily on the previous one (a form wizard, a multi-step quiz builder's local draft state).

### 2.6 Context API vs Zustand vs Redux
Context is built-in, good for low-frequency-update global values (theme, current user) — but every consumer re-renders on any context value change, so it's a poor fit for frequently-changing state. Zustand is a small external store with fine-grained subscriptions (a component only re-renders if the specific slice it reads changes) — a common real-world choice for UI-local global state (sidebar, theme) kept deliberately separate from server state. Redux adds more structure/boilerplate (actions, reducers, middleware) and shines in larger apps needing strict, traceable state transitions and devtools time-travel debugging — often overkill when TanStack Query already owns server state and Zustand owns UI state.

### 2.7 Error boundaries
A class component (no hook equivalent yet) implementing `componentDidCatch`/`static getDerivedStateFromError` that catches render-time errors in its child tree and renders a fallback UI instead of crashing the whole app. It does **not** catch errors in event handlers, async code, or SSR — those need their own try/catch.

### 2.8 Suspense & code splitting
`React.lazy(() => import('./Component'))` + `<Suspense fallback={...}>` splits a component into its own bundle chunk, loaded on demand, showing a fallback while it loads. In the App Router, this pattern also gates streaming — a slow data-fetching component can be wrapped in `Suspense` so the rest of the page renders immediately and that piece streams in when ready.

### 2.9 React 19 features
The `use` hook reads a promise or context directly during render (including conditionally, unlike other hooks), letting Suspense handle the pending state. Actions and `useActionState`/`useOptimistic` give built-in patterns for form submissions with pending/error state and optimistic UI updates without hand-rolling that logic. The React Compiler (opt-in) auto-memoizes components, reducing the need for manual `useMemo`/`useCallback` in many cases — worth mentioning as a reason manual memoization is becoming less critical to hand-apply everywhere.

### 2.10 `forwardRef` & portals
`forwardRef` lets a parent attach a ref through a component to an underlying DOM node (needed for a custom `<Input>` wrapper that still needs `.focus()` to work from outside). Portals (`createPortal`) render a child into a different DOM node than its parent in the tree — the standard mechanism for modals/tooltips that need to escape a parent's `overflow: hidden` or `z-index` stacking context while staying part of the React tree logically.

### 2.11 Prop drilling
Passing a prop through several layers of components that don't use it themselves, just to reach a deep child. Fixes: Context for infrequently-changing values, component composition (passing JSX as `children`/props instead of drilling data down through structure), or a global store (Zustand) once the value is genuinely cross-cutting rather than parent-child specific.

### 2.12 HOC vs render props vs hooks
All three solve "share logic across components" at different eras of React. HOCs (`withAuth(Component)`) wrap a component, but stack awkwardly and obscure props via wrapper hell. Render props (`<DataFetcher render={data => ...}>`) avoid the wrapping problem but nest visually. Hooks solved this cleanly by letting logic be reused without wrapping the component tree at all — the reason hooks are now the default answer, with HOCs/render props mostly seen in older codebases.

### 2.13 Hydration mismatches
When server-rendered HTML doesn't match what the client would render on first paint (e.g., using `Date.now()` or `Math.random()` directly in render, or browser-only APIs like `window` accessed during SSR), React logs a hydration warning and may re-render, causing a flash. Fix by deferring browser-only/non-deterministic values to a `useEffect` (so they only apply after mount) or gating them behind a client-only check.

### 2.14 TypeScript: interfaces vs types
Both define object shapes; interfaces can be reopened/extended (`declare module` augmentation, multiple `interface` declarations merge), types can't be redeclared but support unions, intersections, and mapped/conditional types that interfaces can't express directly. Common convention: `interface` for public object/prop shapes meant to be extended, `type` for unions, primitives, and anything using type-level logic.

### 2.15 Generics & constraints
```ts
function getProp<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```
`K extends keyof T` constrains the generic so only valid keys of `T` are accepted, and the return type is correctly inferred as `T[K]` rather than `any`.

### 2.16 Discriminated unions & narrowing
```ts
type QuizResultNested = { kind: 'nested'; data: { score: number } };
type QuizResultFlat = { kind: 'flat'; score: string };
type QuizResult = QuizResultNested | QuizResultFlat;

function getScore(result: QuizResult): number {
  switch (result.kind) {
    case 'nested': return result.data.score;
    case 'flat': return Number(result.score);
  }
}
```
A shared literal `kind` field lets TypeScript narrow the union in each branch with no manual casting — directly applicable to the real dual-shape quiz-result problem from the Zad Dashboard project.

### 2.17 Utility types
`Pick<T, K>`/`Omit<T, K>` derive a narrower type from a larger one (a form-input type from a full entity). `Partial<T>` fits PATCH-style update payloads. `Record<K, V>` fits string-keyed registries — like the `TABLE_COLUMNS_MAP` pattern used across the admin dashboard projects. `Required<T>` and `Readonly<T>` flip optionality/mutability.

### 2.18 `as const`
Narrows a literal to its most specific type instead of widening it — `const status = 'active' as const` types as the literal `'active'`, not `string`. Common for defining a fixed set of options that TypeScript should treat as a union of literals rather than a generic string.

### 2.19 Next.js App Router conventions
`page.tsx` defines a route's UI, `layout.tsx` wraps it and persists across navigations within it, `loading.tsx` auto-wraps the segment in Suspense with that fallback, `error.tsx` is an error boundary for the segment. Route groups `(name)` organize files without affecting the URL — used across several real projects to bucket ~30 feature folders by business domain without cluttering the route structure.

### 2.20 Rendering strategies: SSR / SSG / ISR / CSR
SSG renders at build time (fastest, but content is frozen until the next build). SSR renders per-request on the server (always fresh, higher server cost). ISR revalidates a statically generated page after a time window or on-demand (`revalidateTag`), giving SSG-like speed with periodic freshness. CSR renders entirely in the browser after JS loads (most flexible, weakest for SEO/first paint). Real apps mix all four per route based on how personalized and how fresh that specific page's data needs to be.

### 2.21 Data fetching & caching in Next.js
`fetch` in a Server Component is cache-aware by default in the App Router; `"use cache"`/`unstable_cache` opt specific functions into caching explicitly, with `revalidateTag`/`revalidatePath` for on-demand invalidation. The core discipline (seen directly in the Inox project): cache what's identical for every viewer (public catalog data), keep per-user data (cart, wishlist, auth) fully dynamic, and reconcile any personalization gap on the client.

### 2.22 Middleware
Runs at the edge before a request completes, commonly used for auth/session checks, i18n locale detection and redirecting, and request rewriting — before a page component ever renders. Note: Next.js 16 renamed `middleware.ts` to `proxy.ts`; know that naming may vary by the app's Next.js version.

### 2.23 Server Actions
Functions marked `"use server"` that can be called directly from a Client Component (e.g., a form `action` prop) without manually wiring an API route — the framework handles the network call. Useful for mutations that need to run server-side only (setting an httpOnly cookie, hitting a backend with server-only credentials).

### 2.24 Image/font optimization
`next/image` auto-generates responsive sizes, lazy-loads offscreen images, and serves modern formats (WebP/AVIF) where supported. `next/font` self-hosts and inlines font files at build time, eliminating a render-blocking external font request and the associated layout shift.

### 2.25 Form handling: React Hook Form + Zod
React Hook Form keeps form state largely uncontrolled internally (better performance on large forms — fewer re-renders per keystroke) while still exposing a controlled-feeling API. Zod defines the validation schema once and can be shared between client-side form validation and server-side payload validation, keeping the two in sync instead of duplicating validation logic.

### 2.26 Accessibility basics
Semantic HTML first (`<button>` not a styled `<div onClick>`), visible focus states preserved (never blanket `outline: none`), form inputs paired with real `<label>`s, `aria-*` attributes only where semantic HTML genuinely can't express the relationship, and color contrast meeting WCAG AA as a baseline. Worth naming as a genuine practice, not just a checkbox — an interviewer at a bank cares about this more than at a typical consumer startup, since accessibility often has compliance weight.

---

## 3. Styling / CSS

### 3.1 Box model
`content` → `padding` → `border` → `margin`, outward. `box-sizing: border-box` (near-universal default choice) makes `width`/`height` include padding and border, avoiding the classic "my 100% width element is now wider than its container" bug from `content-box`.

### 3.2 Position types
`static` (default, normal flow), `relative` (offset from its own normal position, still occupies its original space), `absolute` (removed from flow, positioned relative to the nearest positioned ancestor), `fixed` (positioned relative to the viewport, ignores scrolling), `sticky` (toggles between relative and fixed based on scroll position within its container).

### 3.3 CSS specificity & the cascade
Specificity order (low to high): type selectors → class/attribute/pseudo-class selectors → ID selectors → inline styles → `!important`. When specificity ties, the later rule in source order wins. Utility-first frameworks like Tailwind sidestep most specificity fights by keeping specificity flat (all classes are roughly equal weight) and relying on source order/composition instead.

### 3.4 Flexbox vs Grid
Flexbox is one-dimensional — ideal for content that flows in a single row/column (nav bars, button groups). Grid is two-dimensional — explicit row *and* column control, the right tool for structured page/section layout (the CSS form exercise: mixed 3-column and 2-column rows sharing one 6-track grid, no `calc()` needed, no risk of items unexpectedly wrapping).

### 3.5 Responsive design & mobile-first
Mobile-first means writing the base (no media query) styles for the smallest viewport, then layering `min-width` media queries to progressively enhance for larger screens — generally produces leaner CSS than a desktop-first `max-width` approach, since most content naturally needs less styling at the smallest size. Tailwind's default breakpoint system (`sm:`, `md:`, `lg:`) follows this same mobile-first logic.

### 3.6 CSS custom properties (variables)
```css
:root { --brand-color: #0caa41; }
.button { background: var(--brand-color); }
```
Unlike Sass variables (compile-time only), CSS custom properties are live at runtime — they can be reassigned via JS or media queries, which is exactly the mechanism behind runtime theme switching (light/dark mode) without a full CSS rebuild.

### 3.7 Tailwind vs Styled Components
Tailwind: utility-first, zero runtime cost (pure CSS, purged at build time), styling stays in JSX, plays cleanly with Server Components. Downside: JSX verbosity, needs a shared token config for team consistency. Styled Components: real component-scoped CSS, natural prop-driven dynamic styling. Downside: runtime style injection cost, extra configuration needed to work well with Server Components since it predates that model. For a larger, longer-lived enterprise codebase, Tailwind's zero-runtime-cost and token-enforced consistency tend to hold up better at scale.

### 3.8 RTL / bidirectional styling
Real, hands-on experience here across nearly every project (Arabic-first products). Key practices: use logical properties (`margin-inline-start` instead of `margin-left`) so spacing automatically flips with direction, set `dir="rtl"`/`dir="ltr"` per-locale at the root, and remember direction sometimes needs to be per-*field* rather than per-page in bilingual `{ar, en}` forms — a real pattern from the Zad Dashboard project.

### 3.9 Animations & transitions
`transition` animates between two states smoothly (hover, focus, state toggles) and is cheap for properties like `opacity`/`transform` since they can run on the compositor thread without triggering layout/paint. `@keyframes` + `animation` gives more control over multi-step sequences. `prefers-reduced-motion` should gate non-essential animation for users who've opted out at the OS level — an accessibility consideration worth naming proactively.

### 3.10 z-index & stacking contexts
`z-index` only compares elements within the same stacking context — a new stacking context is created by properties like `position` + `z-index` together, `opacity < 1`, `transform`, or `filter`. A common real bug: a modal with a high `z-index` still renders behind something else because it's trapped inside an ancestor's stacking context with a lower one — the fix is usually a portal (see §2.10) to escape that ancestor entirely.

---

## 4. Testing

### 4.1 Unit vs integration vs e2e
Unit tests isolate a single function/component, usually with dependencies mocked — fast, precise about *what* broke. Integration tests exercise several units together (a form + its validation + its submit handler) — closer to real usage, slower, more valuable per test written. E2E tests drive the real app in a real browser end-to-end (Cypress/Playwright) — highest confidence, slowest, most brittle to unrelated UI changes. A healthy suite is mostly unit/integration with a thin layer of E2E on critical paths, not the other way around.

### 4.2 Jest basics
`describe`/`it` structure suites; `expect(...).toBe()`/`.toEqual()`/`.toHaveBeenCalledWith()` etc. as matchers; `beforeEach`/`afterEach` for setup/teardown (resetting mocks between tests is easy to forget and causes test pollution); `jest.fn()` creates a mock function, `jest.mock('./api-client')` mocks an entire module so tests never hit real network calls.

### 4.3 React Testing Library query patterns
Query priority: `getByRole` > `getByLabelText` > `getByPlaceholderText` > `getByText` > `getByTestId` (last resort) — mirrors how a real user or screen reader finds the element, keeping tests resilient to markup changes that don't affect behavior. `user-event` over `fireEvent` — simulates a real interaction sequence rather than one synthetic event, catching more real bugs (e.g., typing fires multiple key events, not just a single value change).

### 4.4 Mocking API calls
Two common approaches: `jest.mock()` on the API client module directly (simple, tightly coupled to implementation), or Mock Service Worker (MSW), which intercepts actual network requests at the network layer so the component code under test doesn't need to know it's being tested at all — generally the more realistic and less brittle approach for anything using `fetch`/TanStack Query.

### 4.5 Testing async components/hooks
Wrap the component in a fresh `QueryClientProvider` per test (state shouldn't leak between tests). Use `findBy*` or `waitFor` to assert on the eventual state after a fetch resolves, rather than asserting synchronously. For a custom hook, `renderHook` from RTL lets you call the hook in isolation and assert on its return value across renders.

### 4.6 Snapshot testing — pros and cons
Pros: catches unintended markup changes with almost no test-writing effort. Cons: snapshots get rubber-stamp-updated without real review once they're large or numerous, providing false confidence — best used sparingly, for small, stable pieces of UI rather than whole pages.

### 4.7 TDD
Write a failing test first, write the minimum code to pass it, then refactor with the test as a safety net. Genuinely useful for well-understood logic (a pricing calculation, a validation rule) where the expected behavior is clear upfront; less natural for UI exploration where the shape of the solution is still being discovered.

### 4.8 What to prioritize for coverage
Business-critical and error-prone logic first — validation rules, permission checks, data transformations, anything touching money/auth — over exhaustively testing simple presentational components. A high coverage percentage on trivial code is a worse use of time than deep coverage on the few pieces of logic that actually cause incidents when wrong.

### 4.9 E2E tools
Cypress and Playwright both drive a real browser through real user flows. Playwright supports multiple browser engines natively and tends to be faster/more reliable for parallel runs; Cypress has a longer-established ecosystem and a well-loved interactive test runner. Either is a reasonable answer — know the trade-off exists rather than having a strong opinion you can't back up.

---

## 5. REST API Integration

### 5.1 HTTP methods & status codes
`GET` (read, idempotent, cacheable), `POST` (create, not idempotent), `PUT` (full replace, idempotent), `PATCH` (partial update, not necessarily idempotent), `DELETE` (idempotent). Status codes: `2xx` success, `3xx` redirect, `4xx` client error (`400` bad request, `401` unauthenticated, `403` forbidden/authenticated-but-not-authorized, `404` not found, `429` rate limited), `5xx` server error.

### 5.2 REST vs GraphQL
REST exposes fixed endpoints returning fixed shapes — simple, cacheable at the HTTP layer, but prone to over-fetching (getting fields you don't need) or under-fetching (needing multiple round-trips). GraphQL lets the client specify exactly the shape it needs in one request, reducing round-trips for complex nested data, at the cost of more backend complexity and harder HTTP-level caching. Every project in the portfolio is REST — a reasonable, honest answer if asked about GraphQL experience is naming the trade-off without claiming hands-on GraphQL depth.

### 5.3 CORS
A browser security mechanism blocking a page from one origin from making requests to a different origin unless the server explicitly allows it via response headers (`Access-Control-Allow-Origin`, etc.). It's enforced by the browser, not the server — a `curl` request to the same endpoint works fine, which is a common point of confusion when debugging a "CORS error" that only shows up in the browser.

### 5.4 Rate limiting / retries
Real example: the Inox storefront build hit the backend's rate limiter because `next build` evaluated every cached function across ~86 pages concurrently. Fixed with two layers — throttling concurrency at the source (`staticGenerationMaxConcurrency` in `next.config.ts`) rather than only reacting after a 429, plus exponential backoff with jitter in the API client, scoped only to idempotent server-side `GET`s (never blindly retry a non-idempotent `POST`/`PATCH`).

### 5.5 Cache invalidation
TanStack Query: `staleTime` (freshness window before background refetch), `invalidateQueries` after a mutation, optimistic updates via `onMutate` + rollback in `onError`. Next.js: `revalidateTag`/`revalidatePath` for on-demand server cache invalidation. Core principle: cache what's identical for every viewer, keep per-user data dynamic, reconcile any gap on the client.

### 5.6 Auth token handling
A recurring real pattern: two token stores in parallel — a session-layer token (NextAuth JWT, used for "am I logged in" UI state) and a separate raw bearer-token cookie that's what actually gets attached to API requests. Worth naming honestly as a design quirk rather than a best practice: it's a real source of subtle bugs (a logout flow needing to clear multiple stores), and a JWT session cookie can silently exceed the browser's 4KB limit if it's carrying more than the minimum (a real bug from the UPL project — see the behavioral answers file).

### 5.7 Pagination strategies
Offset-based (`?page=2&limit=20`) is simple but can skip/duplicate items if data changes between page loads. Cursor-based (`?after=<id>`) is more consistent under concurrent writes and scales better on large datasets since it avoids a costly `OFFSET` scan on the backend, at the cost of not supporting "jump to page 7" directly.

### 5.8 Real-time: WebSockets vs polling vs SSE
WebSockets give full-duplex, low-latency bidirectional communication — the right choice for something like live bidding (a real project used WebSocket-powered real-time bid displays for exactly this reason). Polling (repeated `GET` requests on an interval) is simplest to implement and debug but wastes requests and adds latency proportional to the interval. Server-Sent Events (SSE) give one-way server-to-client streaming over plain HTTP — a lighter-weight middle ground when the client never needs to push data back over the same connection.

### 5.9 Error handling patterns
A centralized API client with a response interceptor is preferable to scattering try/catch at every call site — handle cross-cutting concerns once (401 → redirect to login, 5xx → generic retry/toast) rather than reimplementing that logic at every fetch call. Distinguish expected errors (validation failure, 404) that the UI should present gracefully from unexpected errors (network failure, 500) that might warrant a retry or fallback state.

### 5.10 Race conditions in fetch (e.g., search-as-you-type)
If request A (typed first) resolves *after* request B (typed second), the UI can end up showing stale results from A. Fixes: an `AbortController` to cancel the previous in-flight request when a new one starts, or a request-sequence-number check that ignores any response older than the latest one sent. TanStack Query handles this automatically via query-key-based cancellation — one more concrete reason to prefer it over hand-rolled fetching.

### 5.11 File uploads
`multipart/form-data` via `FormData` for the request body (not JSON) when uploading binary files alongside metadata. For large files or slow connections, chunked/resumable upload strategies and a progress indicator (via `XMLHttpRequest`'s `progress` event, since `fetch` doesn't expose upload progress natively) meaningfully improve UX.

---

## 6. System Design

### 6.1 Reusable data table for 20+ CRUD screens
Real, built experience — three times independently. Three parallel string-keyed registries (`TABLE_COLUMNS_MAP`, `TABLE_FORM_SCHEMA_MAP`, `TABLE_QUERY_MAP`) feeding one generic `<DataTablePage tableFor="branches" />` component — adding an entity means filling in config, not writing a new page. Honest trade-off: string-keyed lookups have weaker compile-time guarantees than a fully-typed approach; a stronger version types all three maps against the same `TableName` union so a missing entry is a compile error.

### 6.2 RBAC design
Model permissions as `resource:action` pairs bundled into roles, not scattered `if (role === 'admin')` checks. Enforce at multiple layers — route/middleware, UI (hide/disable, for UX only), and always re-checked server-side, since client-side gating is never a security boundary. A real footgun worth naming candidly: a permission-guard helper where omitting `requiredRoles` silently grants access because an empty roles array defaults to "satisfied" — a good example of why authorization logic needs to fail closed by default, not open.

### 6.3 State management architecture — deciding what goes where
A clean split used consistently across several real projects: **server state** (anything from the API) lives in TanStack Query, never duplicated into a separate store; **UI-local state** (sidebar open/closed, theme, modal visibility) lives in Zustand; **URL state** (filters, pagination, selected tab) lives in the URL itself (via `nuqs` or `useSearchParams`) so it's shareable/bookmarkable and survives a refresh; **component-local state** (an uncontrolled input's transient value) stays in `useState`. The decision rule: state should live at the *lowest* level that still lets everything that needs it reach it — reaching for a global store by default is usually premature.

### 6.4 Large list performance / virtualization
Rendering thousands of DOM nodes at once tanks performance regardless of how efficient each individual component is. Virtualization (e.g., `react-window`/`react-virtual`) renders only the rows currently in (or near) the viewport, recycling DOM nodes as the user scrolls — the standard fix once a table/list is expected to regularly exceed a few hundred rows.

### 6.5 Offline-first / PWA architecture
Real, shipped experience: Zad Al-Akhera, a production offline-first PWA. Key pieces: a service worker (Workbox) with a multi-tier caching strategy (cache-first for static assets, network-first with a cache fallback for API data that should stay fresh when online but still work offline), a web app manifest for installability, and a sync strategy for reconciling any writes made while offline once connectivity returns. The hard part isn't the caching itself — it's deciding, tier by tier, what's safe to serve stale versus what must fail loudly offline.

### 6.6 i18n / RTL architecture at scale
Real experience across nearly every project (Arabic-first, bilingual). Key decisions: locale as a required URL segment (so every route is unambiguous and shareable), logical CSS properties instead of directional ones so spacing auto-flips with `dir`, and — the less obvious lesson — direction sometimes needs to be evaluated *per field* rather than per page in bilingual `{ar, en}` forms, since a single form can mix RTL and LTR content depending on which language field is active.

### 6.7 Error logging / monitoring strategy
At minimum: a global error boundary catching render errors, a centralized API client logging failed requests with enough context to reproduce (endpoint, status, payload shape — never sensitive data), and client-side error reporting (Sentry or similar) capturing unhandled exceptions with source maps for real stack traces in production. For a banking-adjacent product specifically, logging discipline around *never* capturing sensitive fields (tokens, PII, account numbers) in error payloads is worth naming proactively.

### 6.8 Design system / component library approach
Start from design tokens (color, spacing, typography scale) as the single source of truth, build primitive components on top of an accessible headless base (e.g., Radix, which shadcn/ui wraps) rather than reinventing accessibility behavior like focus trapping or keyboard navigation from scratch, and layer composed/business components on top of those primitives. The payoff compounds directly with the config-driven table/form pattern — the more consistent the primitive layer, the less custom styling every new CRUD screen needs.

### 6.9 Scaling a Next.js app
As an app grows past a handful of routes: route groups to organize by business domain without affecting URLs (a pattern used across the UPL and Smart Wash dashboards to keep ~30 feature folders navigable), a generic table/form platform to avoid duplicating CRUD boilerplate, explicit cache/revalidation strategy per data type rather than ad hoc per-page decisions, and — once a single Next.js app genuinely outgrows a single team — considering a monorepo (Turborepo/Nx) to share a design system and types across multiple apps without a slower, more error-prone copy-paste-between-repos workflow.

---

## 7. Git

### 7.1 Workflow & branching
Feature branches off `main`/`develop`, PR-based review, CI gating the merge. Trunk-based development with short-lived branches suits fast-moving teams; a stricter branching model with required reviewers and protected branches is more likely — and worth being comfortable adapting to — in a regulated environment like banking.

### 7.2 Rebase vs merge
Merge preserves full history with a merge commit. Rebase rewrites commits onto a new base for linear history. Rule: rebase your own local feature branch to clean up history before opening a PR; never rebase a shared/public branch, since it rewrites commit hashes and breaks anyone who's based work on it.

### 7.3 Resolving conflicts
`git status` to see conflicted files, open each, understand *why* each side changed that code (not just pick "ours"/"theirs" blindly), resolve, `git add`, continue the merge/rebase.

### 7.4 Stash, cherry-pick, bisect
`git stash` shelves uncommitted changes to switch branches cleanly, then `git stash pop` restores them. `git cherry-pick <commit>` applies a specific commit from another branch without merging the whole branch — useful for backporting a fix. `git bisect` binary-searches commit history between a known-good and known-bad commit to find exactly which commit introduced a regression — genuinely useful for tracking down when a subtle bug was introduced in a large history.

### 7.5 Reset vs revert
`git reset` moves the branch pointer (and optionally working directory/staging) backward, rewriting history — fine for local, unpushed commits, dangerous on shared branches. `git revert` creates a *new* commit that undoes a previous one, preserving history — the safe choice for undoing something already pushed/shared, since it doesn't rewrite anything others may have built on.

### 7.6 Commit hygiene
Small, focused commits with descriptive messages (conventional commits — `fix:`, `feat:`, `refactor:` — are common enough to recognize even if a team doesn't enforce them) make `git bisect`, code review, and `git blame` meaningfully more useful later. A commit that bundles an unrelated formatting pass with a real logic change makes the actual change harder to review and harder to revert independently if needed.

---

## Final Prep Checklist
- [ ] Say the event loop example out loud a few times — order-of-execution trips people up live even when they know it cold on paper
- [ ] Rehearse the config-driven table pattern (§6.1) in under 90 seconds — your strongest system-design card
- [ ] Have the Inox 429/backoff story and the RBAC OR-logic footgun ready as concrete, evidenced answers
- [ ] Be ready to say "I know the fundamentals, but I don't have deep production test-suite experience" plainly if testing goes deep — confidence in what you do know beats overclaiming
- [ ] Skim §2.9 (React 19) and §2.19–2.23 (App Router) once more the morning of — these are the areas most likely to have shifted since you last studied them formally
