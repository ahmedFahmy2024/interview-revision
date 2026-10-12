# React 19.3 interview questions and answers

This is a React question bank for revision before interviews. Answers come from the React documentation fetched with `opensrc path reactjs/react.dev` and checked against the React 19.3.0 source fetched with `opensrc path react` using a lockfile that pinned `react@19.3.0`. The documentation repository is a live `main` snapshot, so the [React 19.3.0 exports](https://github.com/facebook/react/blob/v19.3.0/packages/react/index.js) are the version boundary. The source links under each section point to the documentation pages used for its answers.

The [older question bank](https://github.com/alfa155518/modern-JavaScript-React-interview-questions-) mixes JavaScript and React, repeats some React questions, and gives class components more space than current React practice. This file covers React only. Class APIs appear where they still matter.

## Components, JSX, and rendering

Sources: [Your first component](https://react.dev/learn/your-first-component), [Writing markup with JSX](https://react.dev/learn/writing-markup-with-jsx), [Keeping components pure](https://react.dev/learn/keeping-components-pure), [Render and commit](https://react.dev/learn/render-and-commit), [Rendering lists](https://react.dev/learn/rendering-lists), [Conditional rendering](https://react.dev/learn/conditional-rendering), [Fragment](https://react.dev/reference/react/Fragment).

### 1. What is a React component?

A component is a JavaScript function that returns the UI React should render. Component names start with a capital letter so JSX can distinguish them from built-in HTML tags.

### 2. What is JSX?

JSX is syntax for writing markup inside JavaScript. A build tool transforms it into JavaScript; browsers do not interpret JSX directly. Use `className`, close tags, and return one root node or a Fragment.

### 3. Are JSX elements DOM nodes?

No. A JSX element describes what should appear. React uses that description during rendering and then commits the necessary DOM changes.

### 4. What are props?

Props are inputs a parent passes to a component. The child reads them during rendering and must not mutate them.

### 5. What does `children` mean?

`children` is the prop that contains JSX nested between a component's opening and closing tags. It lets a component wrap arbitrary content.

### 6. What makes a component pure?

It returns the same JSX for the same props, state, and context, and does not change values that existed before rendering. Put side effects in event handlers or Effects, as appropriate.

### 7. What triggers a render, and what happens next?

The initial root render and later state updates trigger rendering. React calls components to calculate the next UI, then commits changes to the DOM. A render does not imply a DOM change.

### 8. How does conditional rendering work?

Use ordinary JavaScript branches, `? :`, or `&&` to choose JSX. Returning `null` renders nothing from that component.

### 9. Why do list items need keys?

Keys identify siblings across renders so React can match items when they move, appear, or disappear. Use a stable ID from the data. An array index is risky when order or membership changes.

### 10. Does a key become a component prop?

No. React uses `key` for reconciliation. Pass the ID separately if the component needs it.

### 11. What is a Fragment?

`<>...</>` groups elements without adding a DOM wrapper. Use the explicit `<Fragment key={id}>` form when a Fragment in a list needs a key.

### 12. How should you describe the "virtual DOM" in an interview?

Describe React's render and commit process instead of claiming that React always rebuilds the whole DOM. Rendering calculates the requested UI; committing applies the needed DOM operations.

## State and identity

Sources: [State as a snapshot](https://react.dev/learn/state-as-a-snapshot), [Queueing state updates](https://react.dev/learn/queueing-a-series-of-state-updates), [Choosing state structure](https://react.dev/learn/choosing-the-state-structure), [Updating objects in state](https://react.dev/learn/updating-objects-in-state), [Updating arrays in state](https://react.dev/learn/updating-arrays-in-state), [Preserving and resetting state](https://react.dev/learn/preserving-and-resetting-state), [`useState`](https://react.dev/reference/react/useState), [`useReducer`](https://react.dev/reference/react/useReducer).

### 13. What is state?

State is component memory held by React. Calling a setter requests a new render with a new state snapshot; it does not change the variable captured by the current render.

### 14. When should you use the functional form of a state setter?

Use `setCount(c => c + 1)` when the next value depends on the previous one, especially when queueing several updates. The updater receives the latest queued value.

### 15. Does React batch state updates?

Yes. React groups updates before rendering so several setters in one event do not require one render per call. A setter does not immediately update the state variable in the running handler.

### 16. Why must state objects and arrays be replaced rather than mutated?

React treats state as a read-only snapshot. Create a new object or array with the change so the next render gets a new value; avoid mutating the old value.

### 17. What state should you avoid storing?

Avoid duplicate state and values that can be calculated from current props or state during rendering. Fewer independent values reduce synchronization bugs.

### 18. What is the difference between props and state?

Props arrive from a parent. State belongs to a component's position in the tree and changes through React's update functions. Both are read-only snapshots during a render.

### 19. When is `useReducer` useful?

Use it when several event handlers update related state through named actions. The reducer takes the current state and action and returns the next state; it must be pure.

### 20. How does React decide whether to preserve state?

React associates state with a component's position in the render tree. Keeping the same component at the same position preserves state; changing its type or key resets it.

### 21. How can a key intentionally reset a component?

Change the key when the identity changes, such as switching from one user profile form to another. React treats the keyed subtree as a new one and initializes its state again.

### 22. What does "lift state up" mean?

Move shared state to the closest common parent. That parent passes the value and update handlers to children so they stay coordinated.

### 23. How can a child send information to its parent?

The parent passes a callback prop, and the child calls it from an event handler. Data still flows down through props on the next render.

## Events, Effects, and Hook rules

Sources: [Responding to events](https://react.dev/learn/responding-to-events), [Synchronizing with Effects](https://react.dev/learn/synchronizing-with-effects), [You might not need an Effect](https://react.dev/learn/you-might-not-need-an-effect), [Lifecycle of reactive Effects](https://react.dev/learn/lifecycle-of-reactive-effects), [Separating events from Effects](https://react.dev/learn/separating-events-from-effects), [Rules of Hooks](https://react.dev/reference/rules/rules-of-hooks), [`useEffect`](https://react.dev/reference/react/useEffect), [`useEffectEvent`](https://react.dev/reference/react/useEffectEvent), [`useLayoutEffect`](https://react.dev/reference/react/useLayoutEffect), [`useInsertionEffect`](https://react.dev/reference/react/useInsertionEffect).

### 24. How are event handlers different from Effects?

An event handler runs because a specific interaction occurred. An Effect runs after a render to synchronize with an external system, such as a connection, browser API, or non-React widget.

### 25. When do you not need an Effect?

If a value can be calculated from props and state during rendering, calculate it there. Handle user-triggered work in the event handler. An Effect is for synchronization with something outside React.

### 26. What does an Effect's dependency array mean?

List every reactive value read by the Effect. React compares dependencies between renders and reruns the Effect when they change. The linter helps find missing values.

### 27. When does an Effect cleanup run?

React runs cleanup before rerunning the Effect with changed dependencies and when the component unmounts. Cleanup should undo the setup, such as disconnecting a subscription.

### 28. Why can an Effect appear to run twice on mount in development?

Under `<StrictMode>`, React performs an extra setup and cleanup cycle to reveal missing cleanup. This development check does not happen in production.

### 29. Does `useEffect` replace every class lifecycle method one for one?

No. An Effect describes synchronization and its cleanup, which can repeat as dependencies change. First decide whether the work belongs in rendering, an event handler, or an Effect.

### 30. When would you use `useLayoutEffect`?

Use it for work that must run before the browser paints, such as measuring layout before positioning a tooltip. Prefer `useEffect` for ordinary synchronization because layout Effects block paint.

### 31. Who is `useInsertionEffect` for?

Primarily CSS-in-JS library authors who need to insert styles before layout Effects run. It is not a general replacement for `useEffect`.

### 32. What does `useEffectEvent` solve?

It lets Effect logic read the latest props and state without making that non-reactive callback a dependency that restarts the Effect. Call an Effect Event only from an Effect or another Effect Event in the same component.

### 33. What are the Rules of Hooks?

Call Hooks at the top level of React components or custom Hooks, never in loops, conditions, nested functions, or event handlers. React must see Hook calls in a consistent order.

### 34. Is `use` subject to the same conditional-call rule?

No. `use(resource)` may appear in conditions and loops. It still must run while React renders a component or Hook, and it cannot be wrapped in `try/catch`.

### 35. What is a custom Hook?

It is a function named with a `use` prefix that composes Hooks to reuse stateful logic. Each call has its own state; a custom Hook does not create shared state by itself.

## Context, refs, and escape hatches

Sources: [Passing data deeply with context](https://react.dev/learn/passing-data-deeply-with-context), [`createContext`](https://react.dev/reference/react/createContext), [`useContext`](https://react.dev/reference/react/useContext), [`useRef`](https://react.dev/reference/react/useRef), [Manipulating the DOM with refs](https://react.dev/learn/manipulating-the-dom-with-refs), [`useImperativeHandle`](https://react.dev/reference/react/useImperativeHandle), [`forwardRef`](https://react.dev/reference/react/forwardRef), [`useId`](https://react.dev/reference/react/useId), [`createPortal`](https://react.dev/reference/react-dom/createPortal).

### 36. When should you use context?

Use context when many nested components need the same value and passing it through each level would be cumbersome. Context does not replace local state; a provider supplies a value that consumers read.

### 37. What changed about context providers in React 19?

You can render `<ThemeContext value={theme}>` directly. `<ThemeContext.Provider value={theme}>` remains the older form.

### 38. What does `useContext` read?

It reads the value from the closest matching provider above the component. Without one, it returns the static default passed to `createContext`.

### 39. When should you use a ref instead of state?

Use `useRef` for a mutable value that must survive renders but should not trigger one, such as a timer ID or DOM node. Changing `ref.current` does not update the screen.

### 40. Can a function component receive `ref` as a prop in React 19?

Yes. A function component can accept `ref` in its props and pass it to a DOM element. `forwardRef` is still available, but the docs say it is no longer necessary for new React 19 code and will be deprecated in a future release.

### 41. What does `useImperativeHandle` do?

It customizes the value exposed through a ref. Use it when a parent needs a narrow imperative handle, such as `focus()`, instead of direct access to the child's DOM node.

### 42. What is `useId` for?

It creates stable IDs for accessibility relationships, such as connecting a label and input. Do not use it for list keys; keys must come from the data.

### 43. What does a portal change?

`createPortal` places DOM nodes elsewhere in the document while keeping their React parent relationship. Context still flows through that relationship, and events bubble through the React tree.

## Performance and scheduling

Sources: [`memo`](https://react.dev/reference/react/memo), [`useMemo`](https://react.dev/reference/react/useMemo), [`useCallback`](https://react.dev/reference/react/useCallback), [`useDeferredValue`](https://react.dev/reference/react/useDeferredValue), [`useTransition`](https://react.dev/reference/react/useTransition), [`startTransition`](https://react.dev/reference/react/startTransition), [`useSyncExternalStore`](https://react.dev/reference/react/useSyncExternalStore), [`<Profiler>`](https://react.dev/reference/react/Profiler), [React Compiler](https://react.dev/learn/react-compiler).

### 44. What does `memo` do?

It lets React skip a component render when its props are unchanged. It is a performance optimization, not a guarantee that the component will never render.

### 45. What is the difference between `useMemo` and `useCallback`?

`useMemo` caches a calculated value between renders. `useCallback` caches a function identity. Both depend on a dependency list and are useful when avoiding measured work or preserving identity matters.

### 46. Does `memo` stop renders caused by state or context?

No. A memoized component still renders when its own state changes or when context it reads changes.

### 47. What does `useTransition` return?

It returns `isPending` and `startTransition`. Updates marked as Transitions can render in the background so urgent interactions remain responsive.

### 48. What is an Action in a Transition?

The function passed to `startTransition` is an Action. It can perform async work, and `isPending` can cover that work. State updates after an `await` currently need another `startTransition` call to be marked as Transition updates.

### 49. Can a Transition control a text input?

No. Keep the controlled input's state update urgent, then use a Transition for slower results or another non-urgent part of the UI.

### 50. What does `useDeferredValue` do?

It lets a slower part of the UI lag behind a rapidly changing value. The latest input can render immediately while React updates the deferred view in the background.

### 51. What is `useSyncExternalStore` for?

It subscribes a component to a store outside React with a consistent snapshot. It is mainly useful for library integrations and external browser or application stores.

### 52. How do you measure a React rendering problem?

Use React DevTools Profiler or `<Profiler>` to inspect render timing, then optimize the expensive path. Adding memoization everywhere without a measured problem makes code harder to maintain.

### 53. What does React Compiler change about manual memoization?

React Compiler can apply memoization automatically to supported code. Existing `memo`, `useMemo`, and `useCallback` remain valid; follow the compiler's rules and keep manual memoization where it is needed.

## Suspense, data, and visible UI

Sources: [`<Suspense>`](https://react.dev/reference/react/Suspense), [`lazy`](https://react.dev/reference/react/lazy), [`use`](https://react.dev/reference/react/use), [`<Activity>`](https://react.dev/reference/react/Activity), [`<ViewTransition>`](https://react.dev/reference/react/ViewTransition), [`addTransitionType`](https://react.dev/reference/react/addTransitionType), [`cache`](https://react.dev/reference/react/cache).

### 54. What does Suspense do?

It displays a fallback while a child suspends, then shows the child when it is ready. A Suspense boundary does not fetch data by itself.

### 55. How does `lazy` work with Suspense?

`lazy(() => import('./Page.js'))` defers loading the component code. The component suspends while its module loads, so a nearby Suspense boundary can show a fallback.

### 56. What can `use(resource)` read?

In React 19.3, it can read a Promise or context during rendering. A pending Promise suspends, a rejected Promise reaches an Error Boundary, and `use(context)` reads the nearest provider value.

### 57. Why should a Promise passed to `use` be stable?

Recreating a Promise on every client render can suspend repeatedly. Pass a cached Promise, often created by a Server Component or a Suspense-aware data source.

### 58. What happens when an already visible Suspense boundary suspends again?

React may show its fallback again. If the update is a Transition or uses a deferred value, React can keep the previous content visible while preparing the next result.

### 59. What does `<Activity>` do?

With `mode="hidden"`, it hides its children, preserves their state, and cleans up their Effects. When visible again, React restores the UI state and recreates the Effects.

### 60. What is `<ViewTransition>` for?

It lets React animate a component tree during Transitions and Suspense reveals using the browser View Transitions API. `addTransitionType` can label the cause of a Transition for choosing animations.

### 61. Is `cache(fn)` a client-side replacement for `useMemo`?

No. The `cache` API in these docs is for React Server Components. It memoizes a function's result for repeated calls in a server render cache.

## Forms and Actions

Sources: [`<form>`](https://react.dev/reference/react-dom/components/form), [`<input>`](https://react.dev/reference/react-dom/components/input), [`<select>`](https://react.dev/reference/react-dom/components/select), [`<textarea>`](https://react.dev/reference/react-dom/components/textarea), [`useActionState`](https://react.dev/reference/react/useActionState), [`useOptimistic`](https://react.dev/reference/react/useOptimistic), [`useFormStatus`](https://react.dev/reference/react-dom/hooks/useFormStatus).

### 62. What makes an input controlled?

React controls it when you pass `value` or `checked` and update that value in `onChange`. `defaultValue` and `defaultChecked` set initial values for uncontrolled inputs.

### 63. What happens if a controlled input has no `onChange`?

It becomes effectively read-only unless you explicitly mark it `readOnly`. The handler must synchronously update the backing state for normal typing.

### 64. What can React 19 pass to `<form action>`?

It can pass a function that receives `FormData` on submission. React runs it in a Transition and resets uncontrolled fields after a successful Action.

### 65. How is a function `action` different from `onSubmit`?

The function `action` receives `FormData` directly and React handles the submission as an Action. `onSubmit` receives a submit event and is the event-handler route for custom submission behavior.

### 66. What does `useActionState` return?

It returns the current result state, a dispatch function, and an `isPending` flag. Its reducer Action receives previous state and a payload and may be async or perform side effects.

### 67. How does `useActionState` differ from `useReducer`?

`useReducer` takes a pure synchronous reducer for state transitions. The reducer Action passed to `useActionState` may run async work and side effects, and multiple dispatches run sequentially.

### 68. What does `useFormStatus` read?

It reads the nearest parent form's submission status, including `pending`, submitted `data`, `method`, and `action`. Call it in a component rendered inside the form, such as its submit button.

### 69. What does `useOptimistic` do?

It temporarily shows an expected result while an Action is pending. It returns the optimistic value and a setter; the displayed value returns to the source value when the Action settles.

### 70. What should happen after an optimistic mutation fails?

Keep the source state authoritative and show an error or retry path. The optimistic value is temporary, so the UI must handle the failed Action rather than pretending the server accepted it.

## DOM, accessibility, and escape hatches

Sources: [Common DOM components](https://react.dev/reference/react-dom/components/common), [`<input>`](https://react.dev/reference/react-dom/components/input), [`<img>`](https://react.dev/reference/react-dom/components/img), [`createPortal`](https://react.dev/reference/react-dom/createPortal), [`flushSync`](https://react.dev/reference/react-dom/flushSync), [`useId`](https://react.dev/reference/react/useId).

### 71. How do React event props work?

Pass a function, such as `onClick={handleClick}`, rather than calling it during rendering. React event handlers receive a React event object with familiar methods such as `preventDefault` and `stopPropagation`.

### 72. What is event propagation in a portal?

Portal events propagate through the React component tree, even though the DOM nodes are elsewhere. Place a handler or call `stopPropagation` with that React tree in mind.

### 73. When should you use `dangerouslySetInnerHTML`?

Only when rendering trusted or sanitized HTML is necessary. It bypasses normal JSX text escaping, so untrusted content can create cross-site scripting vulnerabilities.

### 74. How should a custom form control keep its label accessible?

Give its label and control matching `htmlFor` and `id` values. `useId` can generate an ID that remains stable across server and client rendering.

### 75. When would `flushSync` be justified?

Use it sparingly when a browser or third-party API requires React's DOM update to finish synchronously before the next statement. It can hurt performance and can reveal Suspense fallbacks.

## Client rendering, server rendering, and boundaries

Sources: [`createRoot`](https://react.dev/reference/react-dom/client/createRoot), [`hydrateRoot`](https://react.dev/reference/react-dom/client/hydrateRoot), [`renderToPipeableStream`](https://react.dev/reference/react-dom/server/renderToPipeableStream), [`renderToReadableStream`](https://react.dev/reference/react-dom/server/renderToReadableStream), [Server Components](https://react.dev/reference/rsc/server-components), [`'use client'`](https://react.dev/reference/rsc/use-client), [`'use server'`](https://react.dev/reference/rsc/use-server), [`cache`](https://react.dev/reference/react/cache).

### 76. What does `createRoot` do?

It creates a React root inside a browser DOM node. Call `root.render(<App />)` to render into it. A framework may call this for you.

### 77. What is hydration?

Hydration attaches React to HTML that the server already rendered. `hydrateRoot` expects the initial client output to match the server HTML; treat mismatches as bugs.

### 78. What is the difference between client rendering and server rendering?

Client rendering creates UI in the browser. Server rendering produces initial HTML on a server and can then hydrate it in the browser for interaction.

### 79. What is streaming server rendering?

React can send HTML in chunks as content becomes ready. The Node API is `renderToPipeableStream`; the Web Streams API is `renderToReadableStream`.

### 80. What is a React Server Component?

It renders in an environment separate from the client app, either at build time or per request. Its code does not need to ship to the browser, and it can read server-side resources directly.

### 81. Does a Server Component mean the same thing as server-rendered HTML?

No. Server Components describe where component code runs and what crosses the server-client boundary. Server rendering produces HTML; an app can use both mechanisms.

### 82. What does `'use client'` mark?

It marks a module as a client boundary when a compatible Server Components framework imports it from server code. The directive belongs at the top of the file, before imports.

### 83. Does every interactive component need its own `'use client'` line?

No. A client module brings its imported module subtree into the client bundle. Put the directive at the boundary between server and client code.

### 84. What does `'use server'` mark?

It marks async Server Functions that client code can invoke through a framework. Calls cross the network, so validate arguments and authorize mutations on the server.

### 85. Should a Server Function be the default way to fetch data?

No. The React docs describe Server Functions mainly for mutations. Server Components can read data during rendering, and frameworks may provide their own data APIs.

### 86. What data may cross a server-client boundary?

Props and Server Function arguments or results must use supported serializable values. Functions, browser-only objects, and secrets cannot be passed as ordinary props to Client Components.

## Errors, debugging, and older APIs

Sources: [`Component`](https://react.dev/reference/react/Component), [`PureComponent`](https://react.dev/reference/react/PureComponent), [`<StrictMode>`](https://react.dev/reference/react/StrictMode), [`act`](https://react.dev/reference/react/act), [`useDebugValue`](https://react.dev/reference/react/useDebugValue), [React Developer Tools](https://react.dev/learn/react-developer-tools), [`forwardRef`](https://react.dev/reference/react/forwardRef).

### 87. What does an Error Boundary catch?

It can show fallback UI when a child throws during rendering. The documented boundary API uses a class component with `getDerivedStateFromError` or `componentDidCatch`; event-handler errors need separate handling.

### 88. Are class components removed in React 19.3?

No. `Component` and `PureComponent` remain exported. Function components with Hooks are the usual choice for new code, while class APIs still matter when maintaining older code or implementing an Error Boundary.

### 89. What does `PureComponent` compare?

It shallowly compares props and state to skip some class renders. Mutating nested objects can defeat that comparison, so immutable updates still matter.

### 90. What does `<StrictMode>` check?

It enables development checks that help reveal impure rendering, missing Effect cleanup, missing ref cleanup, and deprecated API use. It does not change production behavior.

### 91. What is `act` for?

In tests, `act` groups renders and interactions so React processes related updates before assertions. Use the async form when the tested work is async.

### 92. When is `useDebugValue` useful?

It adds a label for a custom Hook in React DevTools. It helps inspect complex reusable Hooks but is unnecessary for most Hooks.

### 93. What is outdated in a lifecycle-only explanation of React?

Modern React explains behavior through rendering, event handlers, state identity, and Effect synchronization. Class lifecycle methods still exist, but they are not a one-to-one map for designing Hook code.

### 94. Is `forwardRef` already removed or deprecated in React 19.3?

No. It remains exported and usable. React 19 lets function components receive `ref` as a prop, and the docs say `forwardRef` will be deprecated in a future release.

## Fast interview scenarios

Sources: [You might not need an Effect](https://react.dev/learn/you-might-not-need-an-effect), [Preserving and resetting state](https://react.dev/learn/preserving-and-resetting-state), [`useTransition`](https://react.dev/reference/react/useTransition), [`useActionState`](https://react.dev/reference/react/useActionState), [`useOptimistic`](https://react.dev/reference/react/useOptimistic), [`hydrateRoot`](https://react.dev/reference/react-dom/client/hydrateRoot).

### 95. A component stores `fullName` and updates it in an Effect when `firstName` changes. What would you change?

Calculate `fullName` from `firstName` and `lastName` during rendering. The Effect and extra state create a needless second update and can get out of sync.

### 96. A counter calls `setCount(count + 1)` three times in one click but increases once. Why?

Each call reads the same `count` snapshot. Use `setCount(c => c + 1)` three times so each queued update receives the previous result.

### 97. A modal's form keeps the previous user's input. How can you reset it?

Give the form subtree a key based on the user ID. When the ID changes, React creates a new subtree with fresh state.

### 98. Search input typing stalls while a large result list renders. What should stay urgent?

Update the controlled input immediately. Defer the result list with `useDeferredValue` or mark its state update as a Transition.

### 99. A submit button must show pending status for its parent form. Which API fits?

Render a child button component inside the form and call `useFormStatus()` there. Read its `pending` value to disable the button or show progress.

### 100. The UI should show a new comment before the server confirms it. Which API fits?

Use `useOptimistic` during the submission Action, then let the authoritative state reflect the server result. Show a failure path if the mutation fails.

### 101. Hydration warns that server and client markup differ. What should you inspect?

Find values that differ during the first render, such as browser-only data, time, randomness, or inconsistent HTML. Make the initial client render match the server output.

## Less common APIs worth recognizing

Sources: [Common DOM components](https://react.dev/reference/react-dom/components/common), [`Children`](https://react.dev/reference/react/Children), [`cloneElement`](https://react.dev/reference/react/cloneElement), [`createElement`](https://react.dev/reference/react/createElement), [`isValidElement`](https://react.dev/reference/react/isValidElement), [`<title>`](https://react.dev/reference/react-dom/components/title), [`<meta>`](https://react.dev/reference/react-dom/components/meta), [Resource preloading APIs](https://react.dev/reference/react-dom), [`cacheSignal`](https://react.dev/reference/react/cacheSignal), [Passing data deeply with context](https://react.dev/learn/passing-data-deeply-with-context).

### 102. What is a React synthetic event?

It is the React event object passed to an event handler. It follows the standard event interface, with React-specific behavior where documented. Use its methods such as `preventDefault()` rather than relying on browser-specific details.

### 103. What is prop drilling?

It is passing a prop through intermediate components that only relay it to a deeper child. Component composition or context can reduce the relay when it becomes cumbersome.

### 104. What does `Children` do?

It provides legacy utilities for inspecting or transforming a component's `children` prop. The docs call it uncommon and often prefer explicit props, arrays, or component composition.

### 105. Why is `cloneElement` uncommon?

It creates a new element with overridden props or children, but can make data flow hard to trace. Prefer passing props directly or exposing a render prop when that fits the API.

### 106. When might you use `createElement`?

It creates a React element without JSX. JSX is more common, but `createElement` is useful when JSX syntax is unavailable or elements are assembled dynamically.

### 107. What does `isValidElement` check?

It checks whether a value is a React element object. It does not tell you whether a value is every kind of renderable React node; strings and numbers can render but are not elements.

### 108. Can a component render document metadata in React 19?

Yes. React can hoist supported `<title>`, `<meta>`, and `<link>` elements into the document head. Check the individual element docs for placement and precedence rules.

### 109. What do `preload`, `preconnect`, and `prefetchDNS` do?

They give the browser resource hints. `preload` starts fetching a known resource, `preconnect` opens a connection to an origin, and `prefetchDNS` resolves an origin's DNS name early.

### 110. What does `cacheSignal` indicate?

In React Server Components, it gives an `AbortSignal` that aborts when the `cache()` lifetime ends. Use it to cancel work that is no longer needed.

## Source and update notes

- React source was fetched by opensrc from the `v19.3.0` tag. Package exports checked: `packages/react/index.js` and `packages/react-dom/index.js`.
- React documentation was fetched by opensrc from `reactjs/react.dev` `main`. The linked pages above correspond to Markdown files under `src/content/learn` and `src/content/reference` in that cache.
- The documentation repository can change independently of the React 19.3.0 tag. When revising this guide, fetch it again with `opensrc path reactjs/react.dev`, check any changed API against the pinned React source, and update the answer and its source link together.
