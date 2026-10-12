# Modern React Design Patterns — Interview Guide (React 19.2) <img src="https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg" alt="React Icon" width="90" height="90">

> A revision-focused reference of **all the important React patterns** a Frontend / React Engineer is expected to know in interviews — updated for **React 19.2** (released Oct 1, 2025).
> Each pattern includes: **What it is → Why/When → Code → Interview soundbite → 🔥 tricky follow-up Q&As** interviewers actually ask.

> 💡 **How to use this file:** Skim the table, then read top-to-bottom. Patterns are ordered from *foundational* → *component-level* → *state & data* → *performance* → *React 19 modern patterns*. Every code sample uses function components + hooks (no class components except where the pattern requires them).

---

## Table of Contents

| No. | Pattern |
| --- | ------- |
| **Foundational patterns** | |
| 1 | [Composition over Inheritance](#1-composition-over-inheritance) |
| 2 | [Container / Presentational (Smart & Dumb)](#2-container--presentational-smart--dumb) |
| 3 | [Controlled vs Uncontrolled Components](#3-controlled-vs-uncontrolled-components) |
| 4 | [Conditional Rendering Patterns](#4-conditional-rendering-patterns) |
| 5 | [Lists & Keys Pattern](#5-lists--keys-pattern) |
| **Reuse patterns** | |
| 6 | [Custom Hooks (the #1 pattern)](#6-custom-hooks-the-1-pattern) |
| 7 | [Higher-Order Components (HOC)](#7-higher-order-components-hoc) |
| 8 | [Render Props](#8-render-props) |
| 9 | [Props Getters](#9-props-getters) |
| **Component API patterns** | |
| 10 | [Compound Components](#10-compound-components) |
| 11 | [Control Props (Controlled/Uncontrolled hybrid)](#11-control-props-controlleduncontrolled-hybrid) |
| 12 | [State Reducer Pattern](#12-state-reducer-pattern) |
| 13 | [Slots / Children as an API](#13-slots--children-as-an-api) |
| **State & data patterns** | |
| 14 | [Lifting State Up](#14-lifting-state-up) |
| 15 | [Context / Provider Pattern](#15-context--provider-pattern) |
| 16 | [Reducer Pattern (useReducer)](#16-reducer-pattern-usereducer) |
| 17 | [Data Fetching with Suspense + `use`](#17-data-fetching-with-suspense--use) |
| 18 | [Error Boundaries](#18-error-boundaries) |
| **Performance patterns** | |
| 19 | [Memoization (memo / useMemo / useCallback)](#19-memoization-memo--usememo--usecallback) |
| 20 | [Lazy Loading & Code Splitting](#20-lazy-loading--code-splitting) |
| 21 | [List Virtualization](#21-list-virtualization) |
| 22 | [The React Compiler (auto-memoization)](#22-the-react-compiler-auto-memoization) |
| **React 19 / 19.2 modern patterns** | |
| 23 | [Actions & `useActionState`](#23-actions--useactionstate) |
| 24 | [Optimistic UI with `useOptimistic`](#24-optimistic-ui-with-useoptimistic) |
| 25 | [Form Status with `useFormStatus`](#25-form-status-with-useformstatus) |
| 26 | [`useTransition` & Concurrent Rendering](#26-usetransition--concurrent-rendering) |
| 27 | [`useDeferredValue`](#27-usedeferredvalue) |
| 28 | [`useEffectEvent` (19.2 stable)](#28-useeffectevent-192-stable) |
| 29 | [`<Activity>` Component (19.2)](#29-activity-component-192) |
| 30 | [`ref` as a Prop & Cleanup Refs](#30-ref-as-a-prop--cleanup-refs) |
| 31 | [Server Components & Server Functions](#31-server-components--server-functions) |
| 32 | [Provider without Context.Provider (19)](#32-provider-without-contextprovider-19) |
| 33 | [Anti-patterns to avoid](#33-anti-patterns-to-avoid) |
| 34 | [Quick Interview Cheat Sheet](#34-quick-interview-cheat-sheet) |

---

# Foundational Patterns

## 1. Composition over Inheritance

**What:** React has *no* component inheritance. You build complex UIs by **composing** small components — passing them as `children` or as props — instead of extending a base class.

**Why/When:** It's the core mental model of React. Any time you're tempted to "extend" a component, compose instead. Enables reuse, keeps components decoupled, and avoids fragile class hierarchies.

```jsx
// A generic shell that doesn't know or care what's inside it.
function Card({ title, children, footer }) {
  return (
    <section className="card">
      <header>{title}</header>
      <div className="card-body">{children}</div>
      {footer && <footer>{footer}</footer>}
    </section>
  );
}

// Specialization by composition, not inheritance.
function UserCard({ user }) {
  return (
    <Card title={user.name} footer={<button>Follow</button>}>
      <p>{user.bio}</p>
    </Card>
  );
}
```

> 🎯 **Say in interview:** "React favors composition over inheritance. Instead of subclassing, I pass children or components as props — the `children` prop is the primary composition tool."

**🔥 Gotcha follow-ups**
- **Q: How do you share code between components without inheritance?** → Custom hooks (logic), composition/`children` (UI), and utility functions (pure logic).
- **Q: What if I need "specialized" versions of a component?** → Configure via props, or wrap it — don't extend it. React has no `extends Component` for your own components (only `React.Component` as a class base).
- **Q: `children` vs a render prop?** → `children` inserts static JSX; a render prop (`children` as a *function*) lets the parent pass data back into what you render.

**[⬆ Back to Top](#table-of-contents)**

---

## 2. Container / Presentational (Smart & Dumb)

**What:** Split components into **containers** (know *how* things work: data, state, side effects) and **presentational** components (know *how* things look: pure props → UI).

**Why/When:** Separates concerns so UI is testable in isolation and easy to reuse. In modern React the "container" logic usually lives in a **custom hook** rather than a wrapper component, but the mental model still matters in interviews.

```jsx
// Presentational — pure, no data logic, trivial to test/storybook.
function UserListView({ users, isLoading }) {
  if (isLoading) return <Spinner />;
  return (
    <ul>
      {users.map((u) => <li key={u.id}>{u.name}</li>)}
    </ul>
  );
}

// Container — owns data fetching, passes props down.
function UserListContainer() {
  const { users, isLoading } = useUsers(); // custom hook = modern "container"
  return <UserListView users={users} isLoading={isLoading} />;
}
```

> 🎯 **Say in interview:** "Presentational components are pure functions of props. Containers handle data/side effects. Today I often replace the container component with a custom hook — same separation, less boilerplate."

**🔥 Gotcha follow-ups**
- **Q: Is this pattern outdated?** → The *strict* container-component split is largely replaced by custom hooks, but the *principle* (separate data logic from view) is timeless. Say that explicitly.
- **Q: Where do Server Components fit?** → An RSC is essentially the "container" (fetches data), passing props to Client Components (the "presentational" leaves).
- **Q: How does this help testing?** → Presentational components take props only, so you test them with plain props — no mocking of fetch/stores.

**[⬆ Back to Top](#table-of-contents)**

---

## 3. Controlled vs Uncontrolled Components

**What:**
- **Controlled** — React state is the single source of truth (`value` + `onChange`).
- **Uncontrolled** — the DOM holds the value; you read it via a `ref` (or, in React 19, via form data).

**Why/When:** Controlled = you need to validate/transform/react to every keystroke. Uncontrolled = simple forms where you only need the value on submit (less re-rendering).

```jsx
// Controlled
function ControlledInput() {
  const [value, setValue] = useState("");
  return <input value={value} onChange={(e) => setValue(e.target.value)} />;
}

// Uncontrolled (ref) — React 19: ref is a normal prop, no forwardRef needed
function UncontrolledInput() {
  const inputRef = useRef(null);
  const onSubmit = () => console.log(inputRef.current.value);
  return (
    <>
      <input ref={inputRef} defaultValue="hi" />
      <button onClick={onSubmit}>Submit</button>
    </>
  );
}
```

> 🎯 **Say in interview:** "Controlled components store the value in state and are the React default. Uncontrolled ones let the DOM own the value and you grab it with a ref or FormData — good for simple, performance-sensitive forms."

**🔥 Gotcha follow-ups**
- **Q: Why does React warn "changing an uncontrolled input to controlled"?** → You passed `value={undefined}` first, then a defined value. Initialize with `""` (or use `defaultValue` for uncontrolled) — never flip between the two.
- **Q: `value` vs `defaultValue`?** → `value` = controlled (React owns it). `defaultValue` = uncontrolled initial value (DOM owns it after).
- **Q: React 19 way to read an uncontrolled form?** → Pass an `action` to `<form>` and read `FormData` — no refs, and it works before hydration.

**[⬆ Back to Top](#table-of-contents)**

---

## 4. Conditional Rendering Patterns

**What:** Idiomatic ways to render UI based on state.

```jsx
// 1. Ternary — either/or
{isLoggedIn ? <Dashboard /> : <Login />}

// 2. Logical AND — render or nothing (beware: 0 && ... renders "0")
{cartCount > 0 && <Badge count={cartCount} />}

// 3. Early return — guard clauses keep JSX flat
function Profile({ user }) {
  if (!user) return <Skeleton />;
  return <div>{user.name}</div>;
}

// 4. Lookup map / enum — cleaner than long if/else chains
const STATUS_UI = {
  loading: <Spinner />,
  error: <ErrorMsg />,
  success: <Data />,
};
return STATUS_UI[status];
```

> 🎯 **Gotcha:** `count && <X/>` renders `0` when count is `0`. Use `count > 0 && ...` or `!!count && ...`.

**🔥 Gotcha follow-ups**
- **Q: Why did `{items.length && <List/>}` render "0"?** → `0` is falsy but still a valid React child, so it prints. Coerce to boolean: `items.length > 0 && ...`.
- **Q: Can you `return null`?** → Yes — returning `null`/`false`/`undefined` renders nothing (but the component still mounts and runs hooks).
- **Q: Ternary vs `&&`?** → Use `&&` for "render or nothing"; use a ternary when you have a genuine either/or. Nested ternaries in JSX are a code smell — extract to a variable or lookup map.

**[⬆ Back to Top](#table-of-contents)**

---

## 5. Lists & Keys Pattern

**What:** When rendering arrays, give each item a **stable, unique `key`** so React can track identity across re-renders.

**Why/When:** Keys let React's reconciler match old and new elements. Wrong keys → lost state, wrong DOM reuse, subtle bugs.

```jsx
// ✅ stable id
{todos.map((t) => <Todo key={t.id} todo={t} />)}

// ❌ index as key when the list can reorder/insert/delete → bugs
{todos.map((t, i) => <Todo key={i} todo={t} />)}
```

> 🎯 **Say in interview:** "Keys must be stable and unique among siblings. Using the array index is fine only for static, append-only lists; for anything that reorders it causes state to attach to the wrong item."

**🔥 Gotcha follow-ups**
- **Q: What actually breaks with index keys?** → On reorder/insert/delete React reuses the wrong DOM/state — e.g. an input's typed text stays with the position, not the item.
- **Q: Do keys need to be globally unique?** → No — only unique **among siblings** in the same list.
- **Q: Can I use a key to force a remount?** → Yes. Changing a component's `key` unmounts and remounts it — a handy way to reset internal state (e.g. reset a form when the selected record changes).
- **Q: Index keys ever OK?** → Only if the list is static, never reordered, and items have no state/id.

**[⬆ Back to Top](#table-of-contents)**

---

# Reuse Patterns

## 6. Custom Hooks (the #1 pattern)

**What:** A function starting with `use` that extracts **stateful, reusable logic** and can call other hooks.

**Why/When:** *The* modern reuse pattern — it replaced ~90% of HOC and render-prop use cases. Composes naturally, adds no DOM wrappers, trivial to test. Whenever you duplicate `useState`/`useEffect` logic across components, extract a hook.

```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  const [isLoading, setLoading] = useState(true);

  useEffect(() => {
    let active = true;
    setLoading(true);
    fetch(url)
      .then((r) => r.json())
      .then((json) => active && (setData(json), setLoading(false)))
      .catch((e) => active && (setError(e), setLoading(false)));
    return () => { active = false; }; // avoid setting state after unmount
  }, [url]);

  return { data, error, isLoading };
}

// Usage
function Users() {
  const { data, isLoading } = useFetch("/api/users");
  if (isLoading) return <Spinner />;
  return <UserList users={data} />;
}
```

> 🎯 **Rules of Hooks:** call hooks only at the top level (not in loops/conditions) and only from React functions/other hooks. Custom hooks *share logic, not state* — each call gets its own isolated state.

**🔥 Gotcha follow-ups**
- **Q: If two components use the same custom hook, do they share state?** → No. Each call is independent — the hook shares *logic*, not a state instance. To share state, use Context or a store.
- **Q: Why must hooks start with `use`?** → It's how the linter/compiler know to enforce the Rules of Hooks. Not just convention — tooling depends on it.
- **Q: Why can't hooks be conditional?** → React tracks hook state by *call order*. A skipped hook shifts every subsequent hook's slot → corrupted state. Put the condition *inside* the hook.
- **Q: The `useFetch` bug interviewers probe?** → Race conditions: fast-changing `url` resolves out of order. The `active` cleanup flag (or an `AbortController`) fixes it. Even better: use React Query / SWR / `use`+Suspense in real apps.

**[⬆ Back to Top](#table-of-contents)**

---

## 7. Higher-Order Components (HOC)

**What:** A function that takes a component and returns an **enhanced** component. `const Enhanced = withX(Component)`.

**Why/When:** Mostly **legacy** now — custom hooks are preferred. Still relevant for cross-cutting concerns applied to *many* components at once (e.g., `connect` in older Redux, error/analytics wrappers) and shows up in interviews as a "do you know the older patterns?" question.

```jsx
function withAuth(Component) {
  return function AuthGuarded(props) {
    const { user } = useAuth();
    if (!user) return <Redirect to="/login" />;
    return <Component {...props} user={user} />;
  };
}

const ProtectedDashboard = withAuth(Dashboard);
```

> 🎯 **Downsides:** wrapper hell, prop-name collisions, indirection in the tree. **Say:** "HOCs solved reuse before hooks; today I'd write a `useAuth` hook or a guard component instead."

**🔥 Gotcha follow-ups**
- **Q: Why did hooks replace HOCs?** → HOCs add wrapper components (deep trees), can collide on prop names, and make the data flow implicit. Hooks compose without any of that.
- **Q: Do you need to forward refs / hoist statics in an HOC?** → Traditionally yes (`forwardRef` + `hoist-non-react-statics`). In React 19 `ref` is a normal prop, so you just pass it through — one less footgun.
- **Q: Any HOC still worth using?** → Framework-level ones you don't write yourself (e.g. `React.memo` is technically a HOC). For app code, prefer hooks.

**[⬆ Back to Top](#table-of-contents)**

---

## 8. Render Props

**What:** A component takes a **function as a prop (often `children`)** and calls it with internal state to decide what to render.

**Why/When:** Still valuable when behavior is **tightly coupled to the JSX tree** — mouse trackers, virtualization, animation libraries (React Spring, Downshift). For plain logic reuse, prefer a hook.

```jsx
function MouseTracker({ children }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  return (
    <div onMouseMove={(e) => setPos({ x: e.clientX, y: e.clientY })}>
      {children(pos)}
    </div>
  );
}

// Usage — consumer controls rendering
<MouseTracker>
  {({ x, y }) => <p>Cursor at {x}, {y}</p>}
</MouseTracker>
```

> 🎯 **Say in interview:** "Render props share behavior by passing a render function. Hooks replaced most uses, but render props still win when the shared logic needs to inject *into the JSX structure itself*."

**🔥 Gotcha follow-ups**
- **Q: Do render props hurt performance?** → An inline arrow re-creates the function each render, so a memoized child underneath re-renders. Usually negligible; hoist the function or use a hook if it matters.
- **Q: Render prop vs custom hook — which?** → Hook for logic reuse (default). Render prop when the shared thing must control *what JSX is produced* (e.g. Downshift, virtualization, animation).
- **Q: "Render prop" — must it be the `children` prop?** → No, any prop that's a function returning JSX counts; `children`-as-function is just the most common style.

**[⬆ Back to Top](#table-of-contents)**

---

## 9. Props Getters

**What:** Evolution of render props: instead of a render function, the hook/component returns **functions that produce prop objects** you spread onto elements.

**Why/When:** Keeps consumers flexible while guaranteeing required props (a11y attributes, event handlers) are wired correctly. Popularized by **Downshift** and **React Hook Form**.

```jsx
function useToggle() {
  const [on, setOn] = useState(false);
  const getTogglerProps = (props = {}) => ({
    "aria-pressed": on,
    ...props,
    onClick: (e) => { props.onClick?.(e); setOn((o) => !o); },
  });
  return { on, getTogglerProps };
}

// Usage — user's onClick is composed, not overwritten
const { on, getTogglerProps } = useToggle();
<button {...getTogglerProps({ onClick: () => console.log("clicked") })}>
  {on ? "ON" : "OFF"}
</button>
```

> 🎯 **Key idea:** the getter *merges* the consumer's handlers with the internal ones instead of clobbering them.

**🔥 Gotcha follow-ups**
- **Q: Why call the internal handler *after* the consumer's?** → So the consumer can `preventDefault`/short-circuit first. Order matters: `props.onClick?.(e)` then your logic.
- **Q: Getter vs just spreading a props object?** → A getter lets each call pass overrides and still guarantees the required a11y/behavior props are merged, not overwritten.
- **Q: Real-world example?** → React Hook Form's `register(...)` returns `name/onChange/onBlur/ref` — a props getter.

**[⬆ Back to Top](#table-of-contents)**

---

# Component API Patterns

## 10. Compound Components

**What:** A set of components that work together as one unit, sharing **implicit state via Context** — like `<select>` / `<option>`. Consumers compose sub-components (`<Tabs.List>`, `<Tabs.Tab>`, `<Tabs.Panel>`).

**Why/When:** The gold standard for flexible, expressive UI primitives (Radix, Headless UI, Reach). Avoids "prop explosion" — consumers arrange layout freely while the parent coordinates state.

```jsx
const TabsContext = createContext(null);

function Tabs({ children, defaultIndex = 0 }) {
  const [active, setActive] = useState(defaultIndex);
  return (
    <TabsContext value={{ active, setActive }}>{children}</TabsContext>
  );
}

function TabList({ children }) {
  return <div role="tablist">{children}</div>;
}

function Tab({ index, children }) {
  const { active, setActive } = useContext(TabsContext);
  return (
    <button role="tab" aria-selected={active === index} onClick={() => setActive(index)}>
      {children}
    </button>
  );
}

function TabPanel({ index, children }) {
  const { active } = useContext(TabsContext);
  return active === index ? <div role="tabpanel">{children}</div> : null;
}

// Attach for the nice dotted API
Tabs.List = TabList;
Tabs.Tab = Tab;
Tabs.Panel = TabPanel;

// Usage
<Tabs defaultIndex={0}>
  <Tabs.List>
    <Tabs.Tab index={0}>Profile</Tabs.Tab>
    <Tabs.Tab index={1}>Settings</Tabs.Tab>
  </Tabs.List>
  <Tabs.Panel index={0}>Profile content</Tabs.Panel>
  <Tabs.Panel index={1}>Settings content</Tabs.Panel>
</Tabs>
```

> 🎯 **Say in interview:** "Compound components share state implicitly through Context so children stay decoupled but coordinated. It gives a clean, flexible API without dozens of configuration props." (Note: in React 19 you can render `<Context>` directly as the provider.)

**🔥 Gotcha follow-ups**
- **Q: Context vs `React.cloneElement` for compound components?** → Context is preferred: `cloneElement` only reaches *direct* children, so it breaks if a user wraps a child in a `<div>`. Context works at any depth.
- **Q: What if someone uses `<Tabs.Tab>` outside `<Tabs>`?** → `useContext` returns the default (`null`) → throw a clear error from a custom hook so misuse fails loudly.
- **Q: How do consumers get flexibility here?** → They arrange/omit/wrap sub-components freely; the parent still coordinates selection state. That's the whole selling point vs a monolithic `<Tabs items={[...]}/>` API.

**[⬆ Back to Top](#table-of-contents)**

---

## 11. Control Props (Controlled/Uncontrolled hybrid)

**What:** Let a reusable component work **either** as uncontrolled (manages its own state) **or** controlled (parent owns `value` + `onChange`) — the same way `<input>` does.

**Why/When:** Building library-grade components that must fit any consumer.

```jsx
function Toggle({ on: controlledOn, onChange, defaultOn = false }) {
  const [uncontrolledOn, setUncontrolledOn] = useState(defaultOn);
  const isControlled = controlledOn !== undefined;
  const on = isControlled ? controlledOn : uncontrolledOn;

  const toggle = () => {
    if (!isControlled) setUncontrolledOn((o) => !o);
    onChange?.(!on);
  };
  return <button aria-pressed={on} onClick={toggle}>{on ? "ON" : "OFF"}</button>;
}

// Uncontrolled:  <Toggle defaultOn />
// Controlled:    <Toggle on={state} onChange={setState} />
```

> 🎯 **Key idea:** "controlled" is detected by whether the `value`-style prop is `undefined`.

**🔥 Gotcha follow-ups**
- **Q: What if a component flips between controlled and uncontrolled?** → Bad — React warns and state gets confused. Lock the mode on first render (`useRef(isControlled)`) and warn in dev if it changes.
- **Q: Why call `onChange` even in uncontrolled mode?** → So a parent can *observe* changes without *owning* the state — matches native `<input>` behavior.
- **Q: How is this different from the State Reducer pattern?** → Control props hand over the *value*; the state reducer hands over the *transition logic*. They compose.

**[⬆ Back to Top](#table-of-contents)**

---

## 12. State Reducer Pattern

**What:** Give consumers a chance to **override or veto internal state transitions** by passing a `reducer` the component runs on every action. (Kent C. Dodds' pattern — "inversion of control".)

**Why/When:** Maximum flexibility for library components — consumers can, e.g., prevent a dropdown from closing on select without you exposing a boolean for every behavior.

```jsx
function useToggle({ reducer = (s, a) => a.changes } = {}) {
  const [state, setState] = useState({ on: false });
  const dispatch = (action) => {
    const changes = { on: action.type === "toggle" ? !state.on : state.on };
    setState(reducer(state, { ...action, changes })); // consumer can rewrite `changes`
  };
  return { on: state.on, toggle: () => dispatch({ type: "toggle" }) };
}

// Consumer overrides behavior — e.g. cap at N toggles
const { on, toggle } = useToggle({
  reducer(state, action) {
    if (action.type === "toggle" && tooManyToggles) return state; // veto
    return action.changes;
  },
});
```

> 🎯 **Say in interview:** "The state reducer pattern inverts control of state logic to the consumer — they get the proposed changes and return the final state, so they can customize behavior without me anticipating every case."

**🔥 Gotcha follow-ups**
- **Q: How is this different from just passing `useReducer` a reducer?** → Here the *library* computes the proposed `changes` and lets the *consumer* rewrite them — inversion of control, not just externalizing the reducer.
- **Q: Why pass an `action.type` to the consumer's reducer?** → So they can target one specific transition (e.g. "don't close on outside click") while letting all others pass through.
- **Q: When is this overkill?** → For app components. It's a *library* pattern (Downshift, Reakit) where you can't predict every consumer requirement.

**[⬆ Back to Top](#table-of-contents)**

---

## 13. Slots / Children as an API

**What:** Expose named "slots" by accepting elements as props (`header`, `sidebar`, `footer`) or by inspecting `children`.

**Why/When:** Layout components (page shells, dialogs) where you want labeled insertion points rather than a single `children`.

```jsx
function Dialog({ title, body, actions }) {
  return (
    <div role="dialog" aria-label={title}>
      <h2>{title}</h2>
      <div className="content">{body}</div>
      <div className="actions">{actions}</div>
    </div>
  );
}

<Dialog
  title="Delete file?"
  body={<p>This can't be undone.</p>}
  actions={<><button>Cancel</button><button>Delete</button></>}
/>
```

> 🎯 **Tip:** slot props are just `ReactNode` props — simpler than Context when you only need placement, not shared state.

**🔥 Gotcha follow-ups**
- **Q: Slots vs compound components?** → Slots = named prop insertion points (no shared state). Compound components = child components sharing state via Context. Pick slots when you only need *placement*.
- **Q: Is prop-drilling elements like this bad?** → No — passing JSX as a prop is composition, and it actually *avoids* re-render coupling (the passed element doesn't re-render when the parent's own state changes).
- **Q: How to detect specific children (named slots from `children`)?** → Inspect `child.type` with `React.Children`, but prefer explicit element props — it's more robust than matching component types.

**[⬆ Back to Top](#table-of-contents)**

---

# State & Data Patterns

## 14. Lifting State Up

**What:** When two siblings need the same state, move it to their **closest common parent** and pass it down as props.

**Why/When:** The default answer to "how do sibling components share state?" Do this *before* reaching for Context or a store.

```jsx
function TemperatureApp() {
  const [temp, setTemp] = useState("");
  return (
    <>
      <CelsiusInput value={temp} onChange={setTemp} />
      <FahrenheitDisplay celsius={temp} />
    </>
  );
}
```

> 🎯 **Say in interview:** "I lift state to the lowest common ancestor. If that ancestor is far away and causes prop drilling, *then* I reach for Context."

**🔥 Gotcha follow-ups**
- **Q: Downside of lifting state too high?** → The whole subtree re-renders on every change, and unrelated children get coupled. Keep state as *low* as possible — "colocate state."
- **Q: Lifting state up vs Context vs a store — decision order?** → Lift up first → Context when drilling gets painful → external store (Zustand/Redux) for high-frequency or truly global state.
- **Q: Can you lift state *down*?** → Yes, "colocation" — if only one child needs it, move it *into* that child so the parent doesn't re-render.

**[⬆ Back to Top](#table-of-contents)**

---

## 15. Context / Provider Pattern

**What:** Share values (theme, auth, locale) across a subtree **without prop drilling**. In **React 19**, `<Context>` can be rendered directly as the provider (no `.Provider`).

**Why/When:** Low-frequency global-ish data. **Not** a substitute for a state manager for high-frequency updates (every consumer re-renders when the value changes).

```jsx
const ThemeContext = createContext("light");

// Provider pattern: co-locate state + provider in one component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");
  const value = useMemo(() => ({ theme, setTheme }), [theme]); // stabilize
  // React 19: render <ThemeContext> directly (no .Provider)
  return <ThemeContext value={value}>{children}</ThemeContext>;
}

function useTheme() {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error("useTheme must be used within ThemeProvider");
  return ctx;
}
```

> 🎯 **Gotchas:** (1) memoize the value object or every consumer re-renders each render; (2) split contexts (state vs dispatch) to limit re-renders; (3) always wrap access in a custom hook that throws if the provider is missing.

**🔥 Gotcha follow-ups**
- **Q: Why does passing `value={{theme, setTheme}}` cause re-renders?** → A fresh object every render ≠ referentially equal, so *all* consumers re-render. `useMemo` it.
- **Q: Does Context re-render only the consumers?** → Yes — only components that call `useContext` for *that* context (and their subtrees), not every child of the provider.
- **Q: Context vs Redux/Zustand?** → Context is a *transport* mechanism, not a state manager. For frequent updates or fine-grained subscriptions, use a store (which lets components subscribe to slices, avoiding the all-consumers-re-render problem).
- **Q: How to avoid re-rendering dispatch-only consumers?** → Split into two contexts: one for state, one for the stable `dispatch`/setters.

**[⬆ Back to Top](#table-of-contents)**

---

## 16. Reducer Pattern (useReducer)

**What:** Manage complex/related state with a **pure reducer** `(state, action) => newState` and dispatched actions.

**Why/When:** When state transitions are complex, interdependent, or the next state depends on the previous — clearer and more testable than juggling many `useState`s. Pairs perfectly with Context for a lightweight "Redux".

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "increment": return { count: state.count + 1 };
    case "decrement": return { count: state.count - 1 };
    case "reset":     return { count: 0 };
    default:          throw new Error(`Unknown action: ${action.type}`);
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  return (
    <>
      <span>{state.count}</span>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "reset" })}>reset</button>
    </>
  );
}
```

> 🎯 **useState vs useReducer:** "Reach for `useReducer` when transitions are complex, when many actions update the same state, or when the next state depends on the previous — the reducer is pure, so it's easy to test."

**🔥 Gotcha follow-ups**
- **Q: Must a reducer be pure?** → Yes — no fetch, no mutation, no side effects. Same inputs → same output. Side effects go in event handlers or effects.
- **Q: Can `dispatch` be a `useEffect` dependency?** → It's stable (React guarantees a constant identity), so it's safe to omit or include.
- **Q: `useReducer` + Context = Redux?** → It's a lightweight equivalent for local/medium apps. Redux Toolkit adds middleware, devtools, and slice-level subscriptions you don't get for free here.
- **Q: How do you do async with a reducer?** → The reducer stays sync; do the async work in the handler/effect, then `dispatch` the result.

**[⬆ Back to Top](#table-of-contents)**

---

## 17. Data Fetching with Suspense + `use`

**What:** The **`use`** API (React 19) reads a Promise (or Context) *during render*; combined with `<Suspense>` it declaratively shows a fallback while the promise resolves — no `isLoading` flag needed.

**Why/When:** Modern read-a-resource-in-render pattern. Unlike hooks, `use` **can be called conditionally / in loops**. Typically the promise is created outside render (RSC, a cache, or a framework loader) to avoid re-creating it every render.

```jsx
import { use, Suspense } from "react";

function Message({ messagePromise }) {
  const message = use(messagePromise); // suspends until resolved
  return <p>{message}</p>;
}

function Page({ messagePromise }) {
  return (
    <Suspense fallback={<p>Loading…</p>}>
      <Message messagePromise={messagePromise} />
    </Suspense>
  );
}

// `use` can also read context conditionally:
function Heading({ children }) {
  if (children == null) return null;
  const theme = use(ThemeContext); // ✅ allowed after an early return
  return <h1 style={{ color: theme.color }}>{children}</h1>;
}
```

> 🎯 **Say in interview:** "`use` unwraps a promise or context during render and integrates with Suspense/Error Boundaries. It's not a hook, so it can be called conditionally. I don't create the promise inside render — I get it from a cache or the server."

**🔥 Gotcha follow-ups**
- **Q: Why not create the promise inside the component with `use`?** → A new promise every render → infinite suspense loop. Create it in a Server Component, a cache, or a framework loader and pass it down.
- **Q: `use` vs `useEffect` for fetching?** → `use` reads during render and suspends (declarative loading/error via Suspense + Error Boundary). `useEffect` fetches *after* paint (waterfalls, manual loading flags).
- **Q: Is `use` a hook?** → No — that's why it can sit after an early `return` or inside a condition. It just can't be in a `try/catch` around suspense.
- **Q: What renders the error if the promise rejects?** → The nearest Error Boundary. Suspense handles pending; the boundary handles rejection.

**[⬆ Back to Top](#table-of-contents)**

---

## 18. Error Boundaries

**What:** Components that **catch render/lifecycle errors** in their subtree and show a fallback instead of crashing the whole app. Still require a **class component** (or `react-error-boundary`).

**Why/When:** Wrap risky subtrees (routes, widgets, lazy chunks). Pairs with Suspense: Suspense handles *loading*, Error Boundaries handle *failures*. Note: they do **not** catch errors in event handlers or async code — use try/catch there.

```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError() { return { hasError: true }; }
  componentDidCatch(error, info) { logToService(error, info); }
  render() {
    return this.state.hasError ? <this.props.Fallback /> : this.props.children;
  }
}

// Usage — often nested with Suspense
<ErrorBoundary Fallback={ErrorScreen}>
  <Suspense fallback={<Spinner />}>
    <Profile />
  </Suspense>
</ErrorBoundary>
```

> 🎯 **Say in interview:** "Error boundaries catch errors during rendering, in lifecycles, and in children — but not in event handlers, async code, or SSR. In practice I use the `react-error-boundary` package for a hook-friendly API with reset."

**🔥 Gotcha follow-ups**
- **Q: What do error boundaries NOT catch?** → Event handlers, `setTimeout`/async code, server-side rendering errors, and errors thrown in the boundary itself. Use `try/catch` for those.
- **Q: Why still a class component?** → `getDerivedStateFromError` / `componentDidCatch` have no hook equivalent yet. The `react-error-boundary` lib wraps this in a nicer API.
- **Q: `getDerivedStateFromError` vs `componentDidCatch`?** → The first (static) returns new state to render the fallback; the second is for side effects like logging.
- **Q: How do you recover/retry?** → Give the boundary a `resetKeys` or a reset function that clears `hasError` (and often re-mounts children via a changing `key`).

**[⬆ Back to Top](#table-of-contents)**

---

# Performance Patterns

## 19. Memoization (memo / useMemo / useCallback)

**What:**
- `React.memo(Component)` — skip re-render if props are shallow-equal.
- `useMemo(fn, deps)` — cache an expensive computed **value**.
- `useCallback(fn, deps)` — cache a **function identity** (so memoized children don't re-render).

**Why/When:** Only when you have a *measured* performance problem — premature memoization adds complexity and can hurt. (See #22: the React Compiler is making manual memoization largely obsolete.)

```jsx
const ExpensiveList = React.memo(function ExpensiveList({ items, onSelect }) {
  return items.map((i) => <Row key={i.id} item={i} onSelect={onSelect} />);
});

function Parent({ items }) {
  const sorted = useMemo(() => [...items].sort(byName), [items]); // value
  const onSelect = useCallback((id) => console.log(id), []);       // stable fn
  return <ExpensiveList items={sorted} onSelect={onSelect} />;
}
```

> 🎯 **memo vs useMemo:** "`memo` memoizes a *component*, `useMemo` memoizes a *value*, `useCallback` memoizes a *function*. `useCallback(fn, d)` is just `useMemo(() => fn, d)`."

**🔥 Gotcha follow-ups**
- **Q: Why did `React.memo` "not work"?** → You passed a new object/array/function prop each render (e.g. `style={{}}` or an inline handler). Shallow-equal fails → it re-renders anyway. Stabilize the props.
- **Q: Does `useMemo` guarantee the value is cached?** → No — React may discard the cache (dev, memory pressure). Treat it as a *performance hint*, never for correctness.
- **Q: Is memoizing everything good?** → No. Each `useMemo`/`useCallback` has its own cost and deps to maintain. Memoize when there's a measured win — and with the React Compiler, usually don't do it by hand.
- **Q: `useMemo` vs `useEffect`?** → `useMemo` computes a value *during* render; `useEffect` runs a side effect *after* render. Don't use effects to derive state.

**[⬆ Back to Top](#table-of-contents)**

---

## 20. Lazy Loading & Code Splitting

**What:** Defer loading a component's code until it's needed with `React.lazy` + `<Suspense>` (or a route-level split).

**Why/When:** Shrink the initial bundle; load heavy/rarely-used views (charts, editors, modals, routes) on demand.

```jsx
import { lazy, Suspense } from "react";
const Analytics = lazy(() => import("./Analytics"));

function App() {
  return (
    <Suspense fallback={<Spinner />}>
      <Analytics />
    </Suspense>
  );
}
```

> 🎯 **Tip:** combine with `<Activity>` (see #29) to *prefetch* the code/data of a likely-next view without showing it.

**🔥 Gotcha follow-ups**
- **Q: Where should the `<Suspense>` boundary go?** → Around the lazy component, at a level where a fallback makes UX sense (route level is common). Too high = whole page flashes; too low = layout jump.
- **Q: `React.lazy` gotcha?** → It only works with a **default export**. Re-map named exports: `lazy(() => import('./x').then(m => ({ default: m.X })))`.
- **Q: How to avoid the loading flash on fast connections?** → Wrap the navigation in `startTransition` so React keeps the old UI until the new one is ready, instead of showing the fallback.

**[⬆ Back to Top](#table-of-contents)**

---

## 21. List Virtualization

**What:** Render only the rows currently in the viewport (+ a small buffer) instead of thousands of DOM nodes. Libraries: `@tanstack/react-virtual`, `react-window`.

**Why/When:** Long lists/tables/feeds where mounting every row kills performance.

```jsx
import { useVirtualizer } from "@tanstack/react-virtual";

function VirtualList({ rows }) {
  const parentRef = useRef(null);
  const v = useVirtualizer({
    count: rows.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 40,
  });
  return (
    <div ref={parentRef} style={{ height: 400, overflow: "auto" }}>
      <div style={{ height: v.getTotalSize(), position: "relative" }}>
        {v.getVirtualItems().map((item) => (
          <div key={item.key} style={{ position: "absolute", top: item.start }}>
            {rows[item.index].label}
          </div>
        ))}
      </div>
    </div>
  );
}
```

> 🎯 **Say in interview:** "Virtualization renders only visible rows, keeping DOM node count constant regardless of dataset size."

**🔥 Gotcha follow-ups**
- **Q: Downsides of virtualization?** → Ctrl+F/in-page find breaks, SEO/accessibility need care, variable row heights are tricky, and jump-to-item requires measuring. Only use it when the list is genuinely large.
- **Q: Virtualization vs pagination vs infinite scroll?** → Virtualization keeps *all* data in memory but renders a slice; pagination/infinite scroll limit the *data* fetched. Often combined.
- **Q: `content-visibility: auto` — does CSS solve this?** → It helps skip *rendering* off-screen content, but the DOM nodes still exist. True virtualization avoids creating them at all.

**[⬆ Back to Top](#table-of-contents)**

---

## 22. The React Compiler (auto-memoization)

**What:** The **React Compiler** (stable in 2025) automatically memoizes components and values at build time — reducing the need for manual `useMemo`/`useCallback`/`memo`.

**Why/When:** Turn it on and delete most manual memoization. It relies on you following the Rules of React (pure components, no mutation of props/state).

```jsx
// With the compiler, this "just works" — no useMemo/useCallback needed:
function ProductList({ products, query }) {
  const filtered = products.filter((p) => p.name.includes(query));
  const onBuy = (id) => addToCart(id); // stable automatically
  return filtered.map((p) => <Product key={p.id} product={p} onBuy={onBuy} />);
}
```

> 🎯 **Say in interview:** "The React Compiler auto-memoizes based on my code, so manual memoization is increasingly a code smell. It works because it assumes I follow the Rules of React — pure renders and no mutation."

**🔥 Gotcha follow-ups**
- **Q: Does the compiler replace `useMemo`/`useCallback` entirely?** → For most cases yes, but keep manual memoization where an external constraint requires a *stable identity* the compiler can't infer, or in code the compiler bails on.
- **Q: What makes the compiler "bail out" on a component?** → Breaking the Rules of React — mutating props/state, side effects during render, or dynamic conditional hooks. It skips those safely rather than miscompiling.
- **Q: Is it runtime magic?** → No — it's a build-time Babel/SWC plugin that rewrites your components to add memoization. Zero runtime library needed (beyond a tiny runtime).

**[⬆ Back to Top](#table-of-contents)**

---

# React 19 / 19.2 Modern Patterns

## 23. Actions & `useActionState`

**What:** **Actions** are async functions passed to a form's `action` (or triggered in a transition). **`useActionState(action, initialState)`** wires an action to a form and returns `[state, formAction, isPending]` — handling pending, errors, and the result automatically.

**Why/When:** The modern way to handle form submission + mutation state without manual `isLoading`/`error`/`useState` bookkeeping.

```jsx
import { useActionState } from "react";

function UpdateName() {
  const [error, submitAction, isPending] = useActionState(
    async (previousState, formData) => {
      const err = await updateName(formData.get("name"));
      if (err) return err;       // becomes the new `state`
      redirect("/profile");
      return null;
    },
    null // initial state
  );

  return (
    <form action={submitAction}>
      <input name="name" />
      <button disabled={isPending}>Update</button>
      {error && <p className="error">{error}</p>}
    </form>
  );
}
```

> 🎯 **Say in interview:** "`useActionState` runs an async action inside a transition and gives me the result/error plus a pending flag — no manual loading state. Passing the action to `<form action>` also progressively enhances the form."

**🔥 Gotcha follow-ups**
- **Q: What's the first arg the action receives?** → The *previous state* (not the event). The second is the `FormData`. Return value becomes the new state.
- **Q: `useActionState` vs `useFormStatus`?** → `useActionState` *owns* the action/state/pending in the component that renders the form. `useFormStatus` *reads* the pending status from a child inside the form.
- **Q: Does it work without JS (progressive enhancement)?** → Yes with Server Functions — the form submits and the server action runs even before hydration.
- **Q: Where does the pending state come from?** → Actions run inside a transition, so React tracks pending automatically — no `useState(false)` needed.

**[⬆ Back to Top](#table-of-contents)**

---

## 24. Optimistic UI with `useOptimistic`

**What:** Show the **expected final state immediately** while the async action runs; React automatically reverts to the real state if it fails or when it resolves.

**Why/When:** Snappy UX for likes, todos, sends — anywhere the mutation almost always succeeds.

```jsx
import { useOptimistic, startTransition } from "react";

function TodoItem({ todo }) {
  const [optimisticDone, setOptimisticDone] = useOptimistic(todo.isComplete);

  return (
    <input
      type="checkbox"
      checked={optimisticDone}
      onChange={(e) => {
        startTransition(async () => {
          setOptimisticDone(e.target.checked); // instant UI
          await setTodoComplete(todo.id, e.target.checked); // real update
        });
      }}
    />
  );
}
```

> 🎯 **Say in interview:** "`useOptimistic` renders the anticipated value immediately during a transition and automatically rolls back to the source-of-truth when the async action settles."

**🔥 Gotcha follow-ups**
- **Q: What happens on error/failure?** → The optimistic value is *discarded* and React shows the real state again — you don't manually roll back. Show your own error UI separately.
- **Q: Must it run in a transition?** → Yes — the optimistic update only lives for the duration of the surrounding action/transition; outside one it resets immediately.
- **Q: Is the optimistic state the source of truth?** → No — it's a temporary override. The real state (prop/server) always wins once the action settles.

**[⬆ Back to Top](#table-of-contents)**

---

## 25. Form Status with `useFormStatus`

**What:** Read the pending state of the **nearest parent `<form>`** from a child component — no prop drilling.

**Why/When:** Reusable submit buttons / spinners that live *inside* a form and need to know if it's submitting.

```jsx
import { useFormStatus } from "react-dom";

function SubmitButton() {
  const { pending } = useFormStatus(); // reads parent <form>
  return <button disabled={pending}>{pending ? "Saving…" : "Save"}</button>;
}

// Reusable anywhere inside a <form action={...}>
<form action={submitAction}>
  <input name="title" />
  <SubmitButton />
</form>
```

> 🎯 **Gotcha:** `useFormStatus` reads the form *above* it — it must be a **child** of the form, not called in the same component that renders `<form>`.

**🔥 Gotcha follow-ups**
- **Q: Why does `useFormStatus` return `pending: false` when I expected true?** → You called it in the same component that renders `<form>`. It reads the *nearest ancestor* form, so it must live in a child component.
- **Q: Which package is it in?** → `react-dom`, not `react` (it's DOM-specific).
- **Q: What else does it return?** → `pending`, plus `data` (the FormData), `method`, and `action` of the in-flight submission.

**[⬆ Back to Top](#table-of-contents)**

---

## 26. `useTransition` & Concurrent Rendering

**What:** Mark state updates as **non-urgent transitions** so React can interrupt/deprioritize them, keeping the UI responsive. Returns `[isPending, startTransition]`. React 19 supports **async** functions in transitions.

**Why/When:** Expensive updates (filtering a big list, tab switches) that shouldn't block typing/clicking.

```jsx
function SearchResults() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();

  const onChange = (e) => {
    setQuery(e.target.value);          // urgent: input stays responsive
    startTransition(() => {
      setResults(expensiveSearch(e.target.value)); // non-urgent
    });
  };
  return (
    <>
      <input value={query} onChange={onChange} />
      {isPending ? <Spinner /> : <List items={results} />}
    </>
  );
}
```

> 🎯 **Say in interview:** "`startTransition` tells React an update is low-priority and interruptible, so urgent updates like keystrokes aren't blocked. `isPending` lets me show subtle loading UI."

**🔥 Gotcha follow-ups**
- **Q: What can't go inside `startTransition`?** → The input's *own* value update — that must stay urgent (synchronous) or the field feels laggy. Only the expensive downstream update is the transition.
- **Q: Transitions vs debouncing?** → Debounce delays *running* the work; a transition runs it but keeps it *interruptible* and non-blocking. They can be combined.
- **Q: Does `startTransition` make things faster?** → No — it makes them *non-blocking*. Total work is the same; the UI just stays responsive and can bail on stale work.
- **Q: React 19 change?** → `startTransition` now supports **async** functions, so you can `await` inside a transition (used by Actions).

**[⬆ Back to Top](#table-of-contents)**

---

## 27. `useDeferredValue`

**What:** Returns a **deferred copy** of a value that lags behind during heavy renders, letting the urgent part update first.

**Why/When:** Like `useTransition` but for a *value* you don't control the update site of (e.g., a prop). Great for search/filter UIs.

```jsx
function List({ query }) {
  const deferredQuery = useDeferredValue(query); // lags behind `query`
  const items = useMemo(() => filter(deferredQuery), [deferredQuery]);
  const isStale = query !== deferredQuery;
  return <ul style={{ opacity: isStale ? 0.5 : 1 }}>{items.map(/*…*/)}</ul>;
}
```

> 🎯 **Transition vs DeferredValue:** "`useTransition` wraps the *update*; `useDeferredValue` wraps the *value*. Use deferred value when you receive a value (e.g., from props) and can't wrap its setter."

**🔥 Gotcha follow-ups**
- **Q: When do you pick `useDeferredValue` over `useTransition`?** → When you *don't own* the update (it's a prop, or state set elsewhere). You can only wrap the value, not the setter.
- **Q: How do you show "stale" UI?** → Compare `value !== deferredValue`; if different, the deferred render is behind — dim it or show a spinner.
- **Q: Does it debounce?** → No fixed delay — it re-renders as soon as React has spare time, then keeps up. It's adaptive, unlike a fixed debounce timeout.

**[⬆ Back to Top](#table-of-contents)**

---

## 28. `useEffectEvent` (19.2 stable)

**What:** Extracts non-reactive logic out of an Effect into an **Effect Event** — a stable callback that always sees the latest props/state but does **not** trigger the Effect to re-run. Shipped stable in **React 19.2**.

**Why/When:** When an Effect reads a value only inside a callback (logging, notifications) but shouldn't re-run when that value changes. Solves the "I had to add X to deps but it re-runs too often" problem — without lying to the linter.

```jsx
import { useEffect, useEffectEvent } from "react";

function ChatRoom({ roomId, theme }) {
  // reads latest `theme` but does NOT cause reconnect when theme changes
  const onConnected = useEffectEvent(() => {
    showNotification("Connected!", theme);
  });

  useEffect(() => {
    const conn = createConnection(roomId);
    conn.on("connected", () => onConnected());
    conn.connect();
    return () => conn.disconnect();
  }, [roomId]); // ✅ only roomId — onConnected is NOT a dependency
}
```

> 🎯 **Rules:** Effect Events must **not** go in the dependency array, must be **called only from Effects**, and must be declared in the same component/hook as their Effect. Don't overuse it just to silence the linter.

**🔥 Gotcha follow-ups**
- **Q: `useEffectEvent` vs `useCallback`?** → `useCallback` returns a *reactive* stable function (identity changes with deps). `useEffectEvent` is *non-reactive* — always sees the latest values and never triggers the Effect. Different purposes.
- **Q: Why not just remove the value from the deps array manually?** → That *lies* to the linter and risks stale closures elsewhere. `useEffectEvent` is the sanctioned way to mark logic as non-reactive.
- **Q: Can I call an Effect Event from an event handler / render?** → No — only from inside Effects. Calling it during render throws.
- **Q: When did it stabilize?** → React 19.2 (Oct 2025); it was `experimental_useEffectEvent` before. Requires the latest `eslint-plugin-react-hooks`.

**[⬆ Back to Top](#table-of-contents)**

---

## 29. `<Activity>` Component (19.2)

**What:** Keeps part of the UI **mounted but hidden**, *preserving state* while pausing its Effects (subscriptions, timers) so they don't waste resources. Modes: `"visible"` / `"hidden"`.

**Why/When:** Tabs, wizards, modals, and **prefetching** — hide the previous tab without losing its state, or pre-render the *next* view in the background so switching is instant. Better than `display:none` (which keeps effects running) and than unmounting (which loses state).

```jsx
import { Activity } from "react";

function Tabs({ active }) {
  return (
    <>
      <Activity mode={active === "home" ? "visible" : "hidden"}>
        <Home />   {/* state + scroll position preserved while hidden */}
      </Activity>
      <Activity mode={active === "settings" ? "visible" : "hidden"}>
        <Settings />
      </Activity>
    </>
  );
}
```

> 🎯 **Say in interview:** "`<Activity>` unmounts *effects* but keeps *state* when hidden — unlike CSS hiding (effects keep running) or conditional rendering (state is lost). It's ideal for tab UIs and prefetching likely-next screens."

**🔥 Gotcha follow-ups**
- **Q: `<Activity mode="hidden">` vs `display:none`?** → Both keep the DOM/state, but Activity *pauses Effects* (cleanup runs, subscriptions/timers stop) so hidden content doesn't burn resources. CSS-hidden keeps effects running.
- **Q: vs `{show && <X/>}`?** → Conditional rendering *unmounts* → loses state and DOM. Activity keeps state so switching back is instant.
- **Q: Does hidden content still render?** → It renders at a *lower priority* in the background (great for prefetching), but its effects stay unmounted until it becomes visible.
- **Q: Which modes exist in 19.2?** → Only `"visible"` and `"hidden"`; more modes are planned. It's stable in 19.2.

**[⬆ Back to Top](#table-of-contents)**

---

## 30. `ref` as a Prop & Cleanup Refs

**What:** In **React 19**, `ref` is a **normal prop** — `forwardRef` is no longer needed. Ref **callbacks** may now return a **cleanup function**.

**Why/When:** Simpler component APIs; deterministic setup/teardown for DOM refs.

```jsx
// React 19 — no forwardRef
function MyInput({ ref, ...props }) {
  return <input ref={ref} {...props} />;
}

// Ref callback with cleanup
<div
  ref={(node) => {
    const observer = new ResizeObserver(() => {});
    if (node) observer.observe(node);
    return () => observer.disconnect(); // 🆕 cleanup on unmount/ref change
  }}
/>
```

> 🎯 **Say in interview:** "React 19 passes `ref` as a regular prop, so I destructure it like any other prop and skip `forwardRef`. Ref callbacks can also return a cleanup, mirroring `useEffect`."

**🔥 Gotcha follow-ups**
- **Q: Is `forwardRef` removed?** → Not removed, but *deprecated* — new code takes `ref` as a normal prop. A codemod migrates existing usage.
- **Q: When does a ref callback run?** → With the node on mount/attach, and (pre-19) with `null` on unmount. In React 19 you can instead **return a cleanup** from the callback and skip the `null` case.
- **Q: `useRef` vs `useState` for a value?** → `useRef` is mutable and does **not** trigger a re-render when changed — use it for DOM nodes, timers, and "instance variables," not for UI-driving state.
- **Q: `useImperativeHandle` still needed?** → Yes, to expose a *custom* imperative API (not just the DOM node) to a parent's ref.

**[⬆ Back to Top](#table-of-contents)**

---

## 31. Server Components & Server Functions

**What:**
- **Server Components (RSC):** render on the server, ship **zero JS** to the client, can `await` data directly. Default in frameworks like Next.js App Router.
- **`"use client"`:** marks the boundary where interactivity (state, effects, events) begins.
- **Server Functions (`"use server"`):** async functions defined on the server but callable from the client (form actions / mutations).

**Why/When:** Smaller bundles, data-fetching next to the component, secrets stay server-side. Interactivity lives in Client Components at the leaves.

```jsx
// Server Component — runs on server, no "use client"
async function ProductPage({ id }) {
  const product = await db.product.find(id); // direct data access
  return (
    <article>
      <h1>{product.name}</h1>
      <AddToCart id={product.id} /> {/* Client Component */}
    </article>
  );
}

// AddToCart.jsx
"use client";
function AddToCart({ id }) {
  return <button onClick={() => addToCart(id)}>Add to cart</button>;
}
```

> 🎯 **Say in interview:** "Server Components run only on the server and send no JS, so I fetch data right in the component. I mark interactive leaves with `use client`. Mutations go through Server Functions with `use server`, callable straight from a form's `action`."
>
> ⚠️ **Security note:** RSC in React 19.0–19.2.2 was affected by a critical RCE (CVE-2025-55182); patched in **19.2.3**. Good "do you follow releases?" trivia.

**🔥 Gotcha follow-ups**
- **Q: Can a Server Component use `useState`/`useEffect`?** → No — RSCs don't run on the client, so no state, effects, or event handlers. Push interactivity into a `"use client"` component.
- **Q: Can a Client Component import a Server Component?** → Not directly. Pass the Server Component as `children`/props from a server parent (composition across the boundary).
- **Q: What can cross the RSC → Client boundary as props?** → Only serializable data (and other elements) — **not** functions (except Server Functions) or class instances.
- **Q: RSC vs SSR?** → SSR renders your *whole* app to HTML then hydrates it (JS still shipped). RSC components render on the server and ship **no JS** for themselves at all.

**[⬆ Back to Top](#table-of-contents)**

---

## 32. Provider without `Context.Provider` (19)

**What:** React 19 lets you render the context object **directly** as the provider: `<MyContext value={...}>` instead of `<MyContext.Provider value={...}>`.

**Why/When:** Cleaner Provider components; less boilerplate in the Context/Compound patterns above.

```jsx
const ThemeContext = createContext("light");

// React 19 ✅
<ThemeContext value="dark">
  <App />
</ThemeContext>

// Pre-19 (still works)
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>
```

> 🎯 **Note:** `<Context.Provider>` is deprecated in favor of `<Context>` and may be removed in a future version.

**🔥 Gotcha follow-ups**
- **Q: Does `<Context>` replace the *consumer* too?** → No — you still read with `useContext(Context)` (or `use(Context)`). Only the `.Provider` wrapper is being simplified.
- **Q: Is `<Context.Provider>` broken in 19?** → No, still works — just deprecated. A codemod rewrites it to `<Context>`.
- **Q: Can I read context conditionally now?** → With `use(Context)` yes (it's not a hook); with `useContext` no.

**[⬆ Back to Top](#table-of-contents)**

---

## 33. Anti-patterns to avoid

| Anti-pattern | Why it's bad | Do instead |
| --- | --- | --- |
| **Index as `key`** in dynamic lists | State attaches to wrong item on reorder | Stable unique id |
| **Derived state in `useState` + `useEffect`** | Extra renders, stale bugs | Compute during render (or `useMemo`) |
| **Overusing Context for high-frequency state** | Every consumer re-renders | Split contexts / use a store (Zustand, Redux Toolkit) |
| **Premature `useMemo`/`useCallback` everywhere** | Complexity > benefit | Measure first; rely on React Compiler |
| **Effects for things that aren't side effects** | Fetch-on-render waterfalls, sync bugs | Event handlers, or `use`/Suspense for data |
| **Mutating state directly** (`state.push`) | React won't re-render; breaks compiler | Return new objects/arrays |
| **Huge god components** | Unreusable, untestable | Extract custom hooks + composition |
| **Prop drilling many levels** | Fragile, noisy | Context or component composition |

> 🎯 **Say in interview:** "A common one I watch for is putting *derived* data in state and syncing it with an Effect — I compute it during render instead. And I don't wrap everything in `useMemo`; with the React Compiler that's usually unnecessary."

**[⬆ Back to Top](#table-of-contents)**

---

## 34. Quick Interview Cheat Sheet

**Reuse logic** → Custom Hook (default) · Render Props / Props Getters (JSX-coupled) · HOC (legacy).

**Flexible component API** → Compound Components · Control Props · State Reducer · Slots.

**Share state** → Lift state up → Context/Provider → external store (only for high-frequency/global).

**Complex state** → `useReducer` (+ Context = mini-Redux).

**Data fetching** → `use` + `<Suspense>` + Error Boundary; Server Components for server data.

**Forms/mutations (React 19)** → `<form action>` + `useActionState` + `useFormStatus` + `useOptimistic`.

**Responsiveness** → `useTransition` (wrap update) · `useDeferredValue` (wrap value) · `<Activity>` (keep-alive/prefetch).

**Effect correctness (19.2)** → `useEffectEvent` for non-reactive logic inside Effects.

**Performance** → Let the **React Compiler** memoize; manual `memo`/`useMemo`/`useCallback` only when measured; lazy-load + virtualize big lists.

**React 19 API wins** → `ref` as a prop (no `forwardRef`) · `<Context>` as provider (no `.Provider`) · ref-callback cleanup · async transitions · document metadata & stylesheet hoisting.

---

### Sources & further reading
- [React 19.2 release notes (react.dev)](https://react.dev/blog/2025/10/01/react-19-2)
- [React 19 release notes (react.dev)](https://react.dev/blog/2024/12/05/react-19)
- [`useEffectEvent` — react.dev reference](https://react.dev/reference/react/useEffectEvent)
- [`<Activity>` — react.dev reference](https://react.dev/reference/react/Activity)
- [`use`, `useActionState`, `useOptimistic`, `useFormStatus` — react.dev](https://react.dev/reference/react)
- [Kent C. Dodds — Advanced React Component Patterns](https://kentcdodds.com/blog/compound-components-with-react-hooks)
- [21 Fantastic React Design Patterns — Persson Dennis](https://www.perssondennis.com/articles/21-fantastic-react-design-patterns-and-when-to-use-them)

> Built for interview revision — React **19.2** · Last updated 2026-07-10.
