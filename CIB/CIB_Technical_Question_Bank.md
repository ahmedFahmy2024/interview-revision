# CIB Bank — Technical Question Bank (Deep Dive)
**Senior Frontend Developer (React JS) — 21 July 2026, 6:00 PM, Microsoft Teams**

*Signal source: a May 2026 CIB Bank Senior Software Engineer interview report weights JS fundamentals — closures, scope, hoisting, event loop, data structures — as heavily as React itself. Prep accordingly.*

---

## 3.1 JavaScript Fundamentals

### "Explain closures. Give an example."

A closure is a function that keeps access to variables from its enclosing scope even after that outer function has finished executing. The inner function "closes over" those variables rather than copying their values.

```js
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}
```

`timer` survives between calls purely because the returned function closes over it — there's no other state being tracked. This is the exact mechanism behind a debounced autosave: each keystroke calls the debounced function, which clears the previous pending save and schedules a new one, so only the last call in a burst actually fires.

### "`var` vs `let`/`const` — and what's hoisting/TDZ?"

- `var` is function-scoped, hoisted and initialized to `undefined` — accessing it before the line it's declared on just gives you `undefined`, no error.
- `let`/`const` are block-scoped and hoisted but *not* initialized. The gap between entering the scope and the declaration line is the **temporal dead zone** — accessing the variable there throws a `ReferenceError`, not `undefined`. This is a deliberate safety net against using a variable before it's meant to exist.
- `const` doesn't mean immutable — it means the *binding* can't be reassigned. `const arr = []; arr.push(1)` is perfectly legal; `arr = []` is not.

### "Walk through the event loop — call stack, microtasks, macrotasks."

```js
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// Output: 1, 4, 3, 2
```

- The **call stack** runs synchronous code first, top to bottom — `1` then `4`.
- The **microtask queue** (promise callbacks, `queueMicrotask`) is fully drained after each synchronous block, *before* anything else runs — so `3` beats `2` even though the `setTimeout` was scheduled first.
- The **macrotask queue** (`setTimeout`, `setInterval`, I/O) runs one task per loop tick, with the microtask queue drained again in between each one.

The practical takeaway: a `Promise.resolve().then()` will always run before a `setTimeout(..., 0)`, no matter how it looks in the code.

### "`Promise.all` vs `Promise.allSettled` — and async/await error handling?"

- `Promise.all` is fail-fast — the moment any promise rejects, the whole thing rejects, even if the others would have succeeded.
- `Promise.allSettled` always resolves once every promise has settled, giving you a `{status, value|reason}` object per promise. Use this when you're hitting several independent endpoints and one failure shouldn't take down the rest of the batch.
- For async/await, wrap in try/catch for sequential error handling. For parallel requests, `await` each one individually in a loop creates an accidental waterfall (each request waits for the previous to resolve) — always `Promise.all`/`allSettled` an array of already-started promises instead.

### "Time complexity of common operations?"

| Operation | Complexity |
|---|---|
| Array push/pop | O(1) amortized |
| Array shift/unshift | O(n) — everything reindexes |
| Array index access | O(1) |
| Array search (`includes`/`indexOf`) | O(n) |
| Object/Map get/set | O(1) average |
| Array `.sort()` | O(n log n) |
| Set/Map `.has()` | O(1) vs an array's O(n) `.includes()` |

Worth mentioning proactively: if you're deduplicating or doing repeated lookups against a large list, swapping an array `.includes()` check for a `Set` is a real, easy performance win.

---

## 3.2 React + TypeScript

### "`useEffect` vs `useLayoutEffect`?"

`useEffect` runs asynchronously *after* the browser paints. `useLayoutEffect` runs synchronously *after* DOM mutations but *before* the browser paints. Default to `useEffect`; reach for `useLayoutEffect` only when you need to measure or mutate the DOM before the user sees a flicker — e.g., measuring an element's size to position a tooltip correctly on first render.

### "When do you use `useMemo`/`useCallback`/`React.memo` — and when *not*?"

- `useMemo` — memoize an expensive derived computation, or keep a stable object/array reference so it doesn't invalidate a dependency array or a memoized child every render.
- `useCallback` — memoize a function reference, mainly so it doesn't break `React.memo` on a child or trigger another hook's dependency array unnecessarily.
- `React.memo` — skip re-rendering a component if its props are shallowly equal to last render.

**When not to:** for cheap computations, the memoization bookkeeping (dependency comparison, memory) can cost more than just recomputing. Wrapping everything in `useMemo`/`useCallback` by default adds noise without evidence of a real re-render problem. The right order is: profile first (React DevTools Profiler), confirm an actual unnecessary re-render is expensive, *then* memoize — not the other way around.

### "Controlled vs uncontrolled components?"

Controlled: the value lives in React state, fed back via `value` + `onChange` — React is the source of truth. Uncontrolled: the DOM holds its own state, accessed via `ref` (optionally seeded with `defaultValue`).

A real example from the HackerBank exercise: the starter code mixed `defaultValue` (uncontrolled) with an `onChange` handler that expected to read the picked date — React warns because those two patterns contradict each other. The fix was switching to a controlled `value={selectedDate}` bound to state, since the filter logic needs to actually read the current date, not just seed an initial one.

### "TypeScript: generics, discriminated unions, utility types?"

**Generics** — writing a function/type once that works across types while preserving type information:
```ts
function firstOf<T>(arr: T[]): T | undefined {
  return arr[0];
}
```

**Discriminated unions** — a real pattern from the Zad Dashboard quiz-results feature, where the same conceptual result came back nested from one endpoint and flattened-with-stringified-numbers from another:
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
The `kind` field lets TypeScript narrow the type in each branch — no manual type assertions needed, and adding a third shape later is a compiler-enforced switch-case addition, not a runtime guess.

**Utility types** — `Pick<T, K>` and `Omit<T, K>` to derive a narrower shape from a larger type (e.g., a form-input type derived from a full entity type), `Partial<T>` for PATCH-style update payloads, `Record<K, V>` for the kind of string-keyed registry pattern (`TABLE_COLUMNS_MAP`, etc.) used across the admin dashboard projects.

### "Why TanStack Query over manual `useEffect` fetching?"

Manual fetching means hand-rolling loading/error states, caching, deduplication of identical in-flight requests, background refetching, and retry logic — and it's easy to introduce a race condition where an older request resolves *after* a newer one and overwrites fresher data. TanStack Query handles all of that by default, keyed off the query key, and gives cache invalidation (`invalidateQueries`) as a first-class primitive for "this mutation just changed data, go refetch what depends on it."

### "Server Components vs Client Components in the Next.js App Router?"

Server Components render on the server and ship zero JS for that component to the browser — good for data-heavy, non-interactive UI, and in a BFF-style setup, the only place that should ever hold real backend URLs/credentials. Client Components (`"use client"`) are required for anything interactive — state, effects, event handlers, browser APIs. The default should be Server Components, with Client boundaries pushed as far down the tree as possible ("leaf" client components) so the interactive island is small and the rest of the page stays server-rendered.

---

## 3.3 Styling

### "Tailwind vs Styled Components — trade-offs?"

**Tailwind**: utility-first, no runtime CSS-in-JS cost (pure CSS, unused classes purged at build time), styling stays co-located in JSX without context-switching to a separate file, works cleanly with Server Components since there's no runtime style injection. Downside: JSX can get visually noisy, and consistency across a team depends on discipline around a shared token/theme config.

**Styled Components**: real component-scoped CSS, dynamic styling via props feels natural, familiar to teams coming from traditional CSS. Downside: runtime style injection adds overhead and bundle size, and in the Server Components era it needs extra configuration since it wasn't originally built for a server-rendering-by-default model.

For a larger, longer-lived codebase — which is the situation at a bank — Tailwind (often paired with a component layer like shadcn/ui for consistency) tends to hold up better because there's no runtime cost and the constraints are enforced by the design tokens rather than developer discipline alone.

### "Flexbox vs Grid — when does each win?"

Flexbox is one-dimensional — it excels at content that should flow naturally in a single row or column (nav bars, button groups, form field rows). Grid is two-dimensional — it gives explicit control over both rows and columns simultaneously, which is the right tool when the layout has real structure to it (the CSS form exercise: three rows of mixed column counts sharing one grid, avoiding any `calc()` math or risk of items unexpectedly wrapping onto the same line).

Rule of thumb: reach for Grid when you're laying out a whole section/page with defined rows and columns; reach for Flexbox when you're aligning or distributing items within a single row or column.

---

## 3.4 Testing (your gap — go in extra prepared)

### "Jest basics?"

- `describe`/`it` (or `test`) for structuring test suites and cases.
- `expect(...).toBe(...)`, `.toEqual(...)`, `.toHaveBeenCalledWith(...)` etc. as matchers.
- `beforeEach`/`afterEach` for setup and teardown (e.g., resetting mocks between tests).
- `jest.fn()` to create a mock function, `jest.mock('./module')` to mock an entire module (like an API client) so tests don't hit real network calls.

### "How would you test a component with React Testing Library?"

Query priority, in order of preference: `getByRole` > `getByLabelText` > `getByPlaceholderText` > `getByText` > `getByTestId` (last resort). The reasoning: querying by role/label mirrors how a real user or a screen reader finds the element, which keeps the test resilient to markup changes that don't affect actual behavior.

`user-event` over `fireEvent` — `user-event` simulates a real interaction sequence (typing fires multiple key/input events, hovering triggers the full pointer sequence) rather than dispatching one synthetic event, so it catches more real bugs.

For a component that fetches via TanStack Query — wrap it in a fresh `QueryClientProvider` per test (so cached state doesn't leak between tests) and use `findBy*`/`waitFor` to assert on the eventual UI state, rather than asserting synchronously before the fetch resolves.

**Honest note for the interview:** be upfront that this is fundamentals-level knowledge rather than deep production experience — see the behavioral answers file for how to frame that.

---

## 3.5 REST API Integration

### "How do you handle rate limiting / retries?"

Real example: the Inox storefront build was hitting the backend's rate limiter because `next build` evaluated every cached function across ~86 pages concurrently. The fix was two layers: throttling concurrency at the source (`staticGenerationMaxConcurrency` in `next.config.ts`, capping how many pages build in parallel) rather than only reacting after a 429, *plus* an exponential-backoff-with-jitter retry loop in the API client — scoped only to idempotent server-side GETs, since blindly retrying a POST/PATCH risks double-submitting a non-idempotent request.

### "Cache invalidation strategies?"

- **TanStack Query**: `staleTime` controls how long data is considered fresh before a background refetch; `invalidateQueries` after a mutation tells the cache "this data may be stale, refetch it"; optimistic updates (`onMutate` + rollback in `onError`) update the UI immediately while the real request is in flight.
- **Next.js**: `revalidateTag`/`revalidatePath` invalidate server-side cached data on demand; `unstable_cache`/`"use cache"` opt a function into caching in the first place. The general principle across both: cache what's safe to share across every viewer, keep per-user data dynamic, and reconcile the gap (e.g., a wishlist heart icon on an otherwise-cached product card) on the client.

### "How do you handle auth tokens?"

A pattern that shows up repeatedly across real projects: two token stores running in parallel — a session-layer token (e.g., NextAuth's JWT, used for "am I logged in" UI state and route gating) and a separate raw bearer-token cookie that's what the API client actually attaches to requests. The reasoning is usually that the session library's token isn't trivially readable in the shape the backend expects, so a second cookie carries the literal bearer token. The trade-off worth naming honestly: two parallel token lifecycles is a real source of subtle bugs (e.g., a logout flow needing to clear three separate stores), and a cleaner design would consolidate to one token source if rebuilt today — a JWT session cookie can also silently exceed the browser's 4KB limit if it's carrying more than it should (see the UPL story in the behavioral answers file).

---

## 3.6 System Design / Senior Judgment

### "Design a reusable data table used across 20+ CRUD screens."

This is real, built experience — three separate times (Saldwish, Smart Wash, UPL). The shape of the answer:

- Three parallel, string-keyed registries: `TABLE_COLUMNS_MAP`, `TABLE_FORM_SCHEMA_MAP`, `TABLE_QUERY_MAP`, each keyed by entity name (e.g., `"branches"`, `"drivers"`, `"offers"`).
- One generic `<DataTablePage tableFor="branches" />` component that looks up its columns, form schema, and query function from those maps — adding a new entity means filling in three config entries, not writing a new page.
- Shared building blocks underneath: a table library for sorting/pagination/filtering, a form library + schema validation for the create/edit dialog, and a query library tied to the query map for data fetching.
- **Honest trade-off to name:** string-keyed lookups (`Record<string, ...>`) have weaker compile-time guarantees than a fully-typed per-entity approach — nothing stops a typo'd key from silently missing in one map but not another. A stronger version would type all three maps against the same `TableName` union so a missing entry is a compile error, not a runtime surprise.

### "How would you design RBAC in a frontend?"

- Model permissions as `resource:action` pairs assigned to roles, rather than scattering `if (role === 'admin')` checks through the codebase — a role is just a named bundle of permissions.
- Enforce at multiple layers, not one: route/middleware level (block navigation before a page even renders), UI level (hide or disable actions the user can't perform, for UX not security), and — critically — the backend must re-check on every request, since client-side gating is convenience, not a security boundary.
- A real footgun worth naming if asked about weaknesses in a past RBAC implementation: a permission-guard helper where passing only `requiredPermissions` and omitting `requiredRoles` silently grants access, because an empty roles array defaults to "role check satisfied" rather than failing closed. No live call site hit it, but it's a one-bad-copy-paste-away bug — a good example of why authorization logic specifically needs default-deny behavior and test coverage, not just code review.

---

## 3.7 Git

### "Describe your typical Git workflow."

Feature branches cut from `main`/`develop`, PR-based review before merge, CI checks (lint/build/tests) gating the merge. For a fast-moving team, trunk-based development with short-lived branches reduces merge pain; for a more regulated environment like banking, a stricter branching model with required reviewers and protected branches is more likely, and worth being comfortable adapting to.

### "Rebase vs merge — when do you use each?"

Merge preserves full history including a merge commit, showing exactly when and how branches combined. Rebase rewrites your branch's commits onto a new base, producing linear history with no merge commit. Practical rule: rebase your own local feature branch to clean up history before opening a PR; never rebase a shared/public branch, since it rewrites commit hashes and breaks anyone else who's based work on it.

### "How do you resolve a merge conflict?"

`git status` to see which files conflict, open each one and look at the conflict markers — but the real skill isn't picking "ours" or "theirs" blindly, it's understanding *why* each side changed that code before deciding how to combine them. Then `git add` the resolved files and continue the merge/rebase.

---

## Quick Prep Checklist
- [ ] Re-read the event loop example out loud — order-of-execution questions are easy to fumble live
- [ ] Have the Inox 429/backoff story and the RBAC OR-logic footgun ready as concrete, evidenced answers
- [ ] Practice explaining the config-driven table pattern in under 90 seconds — it's your strongest system-design card
- [ ] Be ready to say "I haven't built extensive test suites, but here's what I know" plainly if a testing question goes deep
