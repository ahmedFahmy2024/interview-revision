# Interview Debrief — Actual Questions Asked & Answers
**CIB Bank, Senior Frontend Developer (React JS) — 21 July 2026**

Organized exactly as the rounds you were asked, for your records and for next time.

---

## Round 1 – Introduction & Core Concepts

### Professional background and project experience
See the drafted "introduce yourself" answer: SahlSolution (Zad Al-Akhera, 3,000+ daily users, RBAC across 5 roles) → Catchy Tech (Konouz Makkah real-time bidding, HIRAJ geolocation marketplace) → Civil Engineering → frontend pivot → core stack → bridge to the role.

### Why React.js?
Component-based architecture makes UI composable and testable in isolation. The virtual DOM plus a reconciliation algorithm means updates are batched and diffed efficiently rather than touching the real DOM on every change. Hooks unified state and side-effect logic in a way that's easy to extract into reusable custom hooks. And practically: the ecosystem around it — Next.js for rendering strategy, TanStack Query for server state, strong TypeScript support — means most of the hard infrastructure problems (routing, caching, code splitting) already have a mature, well-supported answer rather than needing to be built from scratch.

### Roles and responsibilities in previous projects
Primary or sole frontend developer on most projects — meaning not just implementing features, but owning architecture decisions (state management strategy, folder/module structure, the config-driven table/form pattern reused across three separate dashboards), and being accountable for what happens when those decisions turn out wrong in production.

### Authentication implementation in React applications
Typical flow: user submits credentials, backend returns a token (JWT or session identifier), the token is stored — ideally in an `httpOnly` cookie so client-side JS can't read it (protects against XSS-based token theft) rather than in `localStorage`, which is readable by any script running on the page. Protected routes are gated at the middleware/route level (redirect unauthenticated users before a page even renders) and re-validated server-side on every request, since client-side gating alone is UX, not security. Refresh tokens handle silently extending a session without forcing re-login. In practice, this has come up as a real design tension: some projects use a NextAuth session JWT for UI auth state and a separate raw bearer-token cookie for the actual API calls, because the session library's token isn't in the shape the backend expects — a workable but not ideal pattern worth naming honestly if asked to critique it.

### Session Storage vs Local Storage
Both are synchronous, browser-only (never sent to the server automatically, unlike cookies), roughly 5-10MB capacity. `localStorage` persists until explicitly cleared — survives closing the tab or browser. `sessionStorage` clears the moment that specific tab closes, and isn't shared across tabs even for the same site. Neither should hold sensitive tokens if it can be avoided, since both are readable by any JS running on the page — an `httpOnly` cookie is the safer choice for actual auth tokens.

### Context API and its use cases
Built into React, no extra dependency, good for values that change rarely and are needed broadly — current theme, current authenticated user, current locale. The catch: every component consuming a context re-renders when that context's value changes, with no built-in way to subscribe to just a slice of it — which makes Context a poor fit for frequently-changing state (form values, live data). For that case, an external store with selector-based subscriptions (Zustand) avoids the broad re-render problem entirely.

---

## Round 2 – Technical Discussion

### React authentication strategies
Cookie-based sessions (server sets an `httpOnly` cookie, browser sends it automatically) vs token-based/JWT auth (client explicitly attaches a bearer token to each request, often from memory or a cookie) vs OAuth/social login (delegating identity to a third party, receiving a token back via a redirect flow). Libraries like NextAuth.js wrap most of this — session handling, provider integration, refresh logic — so the common real-world approach is configuring one of these rather than hand-rolling the full flow.

### Latest React features and updates
React 19 introduced the `use` hook, which can read a promise or context directly during render — including conditionally, unlike other hooks — letting Suspense manage the pending state naturally. Actions and `useActionState`/`useOptimistic` give built-in patterns for form submissions with pending/error state and optimistic UI updates, without hand-rolling that logic. The React Compiler (opt-in) auto-memoizes components, reducing how often `useMemo`/`useCallback` need to be applied manually. `ref` can now be passed as a regular prop in many cases, reducing the need for `forwardRef`.

### `addEventListener` and event handling
`addEventListener(type, handler, options)` attaches a handler without overwriting any other handler already on that element (unlike `onclick = handler`). Event handling follows a capture phase (root to target) then a bubble phase (target back up to root) — most listeners run on bubble by default, `{ capture: true }` opts into the capture phase. Always pair with `removeEventListener` in a cleanup step (in React, the `useEffect` cleanup function) to avoid leaking listeners that hold references to unmounted components. React wraps native events in a `SyntheticEvent` for cross-browser consistency; since React 17, events attach at the root container rather than `document`, which matters for how event delegation interacts with multiple React roots on one page.

### `useLayoutEffect` vs `useEffect`
`useEffect` runs asynchronously after the browser paints. `useLayoutEffect` runs synchronously after DOM mutations but before paint. Default to `useEffect`; use `useLayoutEffect` only when you need to measure or mutate the DOM before the user sees a flicker — e.g., measuring an element to position a tooltip correctly on first render.

### Debouncing and Throttling
Debounce delays execution until a pause in events — fires once, after the burst stops (autosave, search-as-you-type). Throttle guarantees execution at most once per interval regardless of event frequency (scroll/resize handlers, where steady updates during continuous activity matter more than just the final state).
```js
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

### JavaScript Event Loop
```js
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// 1, 4, 3, 2
```
Synchronous code on the call stack runs first. The microtask queue (promise callbacks) fully drains after each synchronous block, before any macrotask. The macrotask queue (`setTimeout`, I/O) runs one task per tick, with microtasks drained again in between.

### `useState` and `useEffect` hooks
`useState` holds a piece of state and triggers a re-render when updated; use the functional updater form (`setCount(c => c + 1)`) when the next value depends on the previous one, to avoid stale-closure bugs in rapid updates. `useEffect` runs a side effect after render, re-running whenever anything in its dependency array changes; an empty array means "once, on mount"; omitting the array means "every render" (almost always a mistake); returning a function from the effect defines cleanup, run before the next effect execution and on unmount.

### Redux and performance optimization
Common levers: memoized selectors (`reselect` or RTK's `createSelector`) so a component only re-renders when the specific derived value it reads actually changes, not on every store update; normalizing state shape (flat, ID-keyed entities rather than deeply nested trees) to make targeted updates cheap; splitting `useSelector` calls narrowly rather than selecting a large object (which breaks reference-equality checks on every render); and RTK Query as a built-in alternative to hand-written thunks for server-state caching, retries, and invalidation.

### Lazy Loading and Code Splitting
`React.lazy(() => import('./Component'))` + `<Suspense fallback={...}>` splits a component into its own bundle chunk, loaded on demand instead of upfront. Next.js does route-based code splitting automatically — each route only ships the JS it needs. `next/image` lazy-loads offscreen images by default. The payoff is a smaller initial bundle and faster time-to-interactive, at the cost of a brief loading state the first time that chunk is needed.

---

## Advanced React & JavaScript

### React Reconciliation Algorithm
React's diffing is a heuristic O(n) algorithm, not a naive full-tree comparison (which would be O(n³)). Two key rules: elements of a different type at the same position cause React to tear down the old subtree and build a new one from scratch (no attempt to diff their children); elements of the same type get their props diffed and their children recursively reconciled. Keys let React match list items across renders by identity rather than position, so reordering, inserting, or removing items doesn't require rebuilding the whole list — which is exactly why an array index as a key breaks this when items reorder.

### React Fiber & Concurrent Rendering
Fiber is React's reconciler implementation (since React 16), representing each unit of rendering work as a "fiber" node in a tree that mirrors the component tree. Because work is broken into units rather than done as one uninterruptible pass, React can pause, abort, or resume rendering — enabling **concurrent rendering**: React can prepare a new version of the UI in the background without blocking the main thread, and prioritize urgent updates (like a keystroke) over less urgent ones (like re-rendering a large list), via APIs like `useTransition`/`startTransition`. This is the underlying mechanism that made Suspense-based data fetching and streaming SSR possible.

### CSR vs SSR vs SSG
CSR renders entirely in the browser after JS loads — most flexible, weakest for SEO and first paint. SSR renders per-request on the server — always fresh, higher server cost per request. SSG renders at build time — fastest to serve, but frozen until the next build (or revalidated via ISR on a timer/on-demand). Real apps mix all three per route, based on how personalized and how fresh that specific page's data needs to be.

### `useRef` vs `useState`
`useState` is part of the render output — updating it schedules a re-render. `useRef` holds a mutable value that persists across renders **without** triggering one when it changes, and is also the mechanism for holding a direct reference to a DOM node (`inputRef.current.focus()`). Use `useRef` for things that shouldn't cause a re-render on their own — a `setTimeout` ID to clear later, tracking a previous value for comparison inside an effect, or a DOM node reference. Use `useState` for anything that should visibly affect what's rendered.

### React.memo and performance optimization
`React.memo(Component)` skips re-rendering a component if its props are shallowly equal to the previous render's props. It only helps if the component actually re-renders often with unchanged props *and* re-rendering it is non-trivially expensive — wrapping cheap components in `memo` by default adds a comparison cost without a real payoff. Pair with `useCallback`/`useMemo` on any function/object props passed down, since a new inline function or object reference every render defeats the shallow comparison entirely.

### Custom Hooks
A function prefixed `use` that composes other hooks to extract and reuse *behavior*, not shared state — each component using a custom hook gets its own independent instance of whatever state it wraps. E.g., a `useDebouncedValue(value, delay)` hook wrapping `useState` + `useEffect` to expose a debounced version of a fast-changing value.

### Preventing unnecessary re-renders
Layered approach: keep state as local as possible rather than lifting it higher than necessary (a component's own re-render is cheap; forcing its whole subtree to re-render isn't); split large components so only the piece that actually needs new data re-renders; use `React.memo` plus stable `useCallback`/`useMemo` references for props passed to memoized children; and, when using an external store (Zustand/Redux), subscribe with a selector to just the slice a component reads, not the whole store object.

### Role-Based Access Control (RBAC)
Model permissions as `resource:action` pairs bundled into roles, rather than scattering `if (role === 'admin')` checks through the codebase. Enforce at multiple layers — route/middleware (block navigation before render), UI (hide/disable actions, for UX only), and always re-checked server-side, since client-side gating is never a security boundary on its own. Worth naming honestly if pushed on a real weakness: a permission-guard helper where omitting `requiredRoles` silently grants access because an empty roles array defaults to "satisfied" — a good illustration of why authorization logic needs to fail closed by default, not open.

### Redux Toolkit vs Context API vs TanStack Query
These solve different problems and the honest answer is that they're not really interchangeable:
- **Redux Toolkit** — the modern, opinionated way to write Redux: `createSlice` reduces boilerplate, Immer lets you write "mutating" reducer logic safely, RTK Query adds server-state caching similar in spirit to TanStack Query. Worth reaching for when client-side state is genuinely complex and interconnected across many features, and devtools time-travel debugging or middleware (logging, analytics) add real value.
- **Context API** — built-in, zero dependency, best for rarely-changing global values (theme, current user). No selector-based subscription model, so it re-renders every consumer on any value change — a poor fit for frequently-changing state.
- **TanStack Query** — not a general state manager at all; it's specifically for **server state** — caching, deduplication, background refetching, retries, invalidation. Using it to hold pure client UI state (like "is the sidebar open") is a mismatch; the right tool for that is Context or Zustand.

In practice, the cleanest architecture used repeatedly: TanStack Query owns server state, Zustand (or Context, for rarely-changing values) owns UI-local state, and Redux/RTK only enters if the app's client-state complexity genuinely outgrows that split.

### TypeScript Generics
```ts
function firstOf<T>(arr: T[]): T | undefined {
  return arr[0];
}
function getProp<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```
Generics let a function or type stay reusable across types while still preserving full type information — the second example's `K extends keyof T` constrains the key to valid properties of `T`, so the return type is correctly inferred rather than falling back to `any`.

### Utility Types: `Partial`, `Pick`, `Omit`, `Record`
- `Partial<T>` — makes every property optional; fits PATCH-style update payloads where only some fields are sent.
- `Pick<T, K>` — derives a narrower type containing only the listed keys; useful for a form-input type derived from a larger entity type.
- `Omit<T, K>` — the inverse: every property except the listed keys.
- `Record<K, V>` — a type for a string(or union)-keyed object with uniform value type `V`; the shape behind registry patterns like `TABLE_COLUMNS_MAP` used across several config-driven dashboard projects.

### Microtasks vs Macrotasks
Microtasks (promise callbacks, `queueMicrotask`) fully drain after each synchronous block of code, before the event loop moves to the next macrotask. Macrotasks (`setTimeout`, `setInterval`, I/O callbacks) run one at a time per loop tick. This is why `Promise.resolve().then()` always executes before a `setTimeout(..., 0)`, regardless of code order.

### API Caching, Retries & Background Sync
- **Caching**: TanStack Query's `staleTime` (how long data is fresh before a background refetch) and `gcTime` (how long unused data stays cached); Next.js `revalidateTag`/`revalidatePath` for server-side cache invalidation.
- **Retries**: exponential backoff with jitter, scoped only to idempotent `GET`s — a real project (Inox storefront) hit the backend's rate limiter during `next build` and needed both throttled concurrency at the source *and* a backoff retry layer.
- **Background sync**: `refetchOnWindowFocus`/`refetchInterval` in TanStack Query for keeping data fresh passively; for true offline-first apps, a service worker's Background Sync API can queue writes made while offline and replay them once connectivity returns — directly relevant to the offline-first architecture built for Zad Al-Akhera.

### Scalable Notification System Design
- **Delivery**: a persistent connection (WebSocket or SSE) pushes new notifications to connected clients in real time; falls back to polling if the connection can't be established, so the feature degrades rather than breaking entirely.
- **Backend fan-out**: at scale, a message queue / pub-sub layer (rather than the API server directly tracking every open connection) decouples notification producers from delivery, so a spike in notifications doesn't overload the same process serving normal API traffic.
- **Frontend state**: a dedicated notification store (Zustand) holding the list and unread count, updated by incoming socket messages and reconciled with an initial REST-fetched page of history via TanStack Query, so a page refresh and a live push both land in the same source of truth.
- **Cross-tab consistency**: unread counts should sync across multiple open tabs — either via `BroadcastChannel` or by having TanStack Query's cache invalidation ripple through a shared query key.
- **UX safeguards**: batching/deduping near-simultaneous notifications so the UI doesn't flood, optimistic "mark as read" with rollback on failure, and an `aria-live` region so screen readers announce new notifications without needing focus to move.

---

## System Design Discussion

### "How would you build a real-time dashboard displaying live data updates?"

**WebSockets vs Polling** — WebSockets give a persistent, low-latency, full-duplex connection, the right choice when updates are frequent and low-latency actually matters (this is the same reasoning behind the WebSocket-powered live bidding built for Konouz Makkah). Polling is simpler and more resilient to network interruption but wastes requests and adds latency proportional to the poll interval — a reasonable fallback, not a first choice, for a genuinely real-time dashboard.

**State Management Strategy** — keep the initial/historical data (whatever loads on page open) in TanStack Query as normal server state. Layer live updates on top by writing incoming socket messages directly into that same query cache (`queryClient.setQueryData`) rather than maintaining a second, separate store — this keeps one source of truth instead of two data paths that can drift out of sync.

**Data Caching** — the initial page load still benefits from normal caching (`staleTime`, prefetching); once the socket connection is live, that cached data is treated as a live-updating baseline rather than something that goes stale on its own timer.

**Component Optimization** — for high-frequency updates, don't re-render on every single incoming message: batch updates over a short window (e.g., merge everything that arrived in the last 200-250ms into one state update) to avoid pegging the main thread. For large, constantly-updating lists, virtualization (rendering only visible rows) keeps DOM size bounded regardless of how much data is streaming in.

**Error Handling & Reconnection Logic** — detect a dead connection via a heartbeat/ping-pong rather than waiting for an ambiguous timeout; on disconnect, attempt reconnection with exponential backoff rather than hammering the server immediately; show an explicit "reconnecting…" state in the UI rather than silently showing stale data as if it were live; and on successful reconnect, re-fetch a fresh snapshot (rather than assuming no messages were missed during the gap) to avoid the dashboard silently drifting out of sync with reality.
