# Frontend Interview Questions 1–25

Combined revision guide with answers, explanations, and examples.

---

## Q1 · TypeScript
**In a large TypeScript codebase, a team plans to enforce stricter type-checking to catch more errors at compile time. Which compiler option should they enable?**

- `noImplicitAny`
- **`strict`** ✅
- `strictNullChecks`
- Both `noImplicitAny` and `strictNullChecks`

**Why:** `strict` enables the whole strict family at once: `noImplicitAny`, `strictNullChecks`, `strictFunctionTypes`, `strictBindCallApply`, `strictPropertyInitialization`, `noImplicitThis`, `alwaysStrict`, and `useUnknownInCatchVariables`.

---

## Q2 · CSS
**The table is a `display: block` element (not flexbox or grid). What property would you set on the cells (1, 2, 3, etc.) so that they come at the bottom left?**

- `align-items`
- `text-align`
- **`vertical-align`** ✅
- `vertical-justify`

**Why:** Table cells respect `vertical-align: bottom`. `align-items` only works in flex or grid layouts, and `vertical-justify` doesn't exist.

---

## Q3 · JavaScript
**What is the output of `eval(1 + + '2' + 3)`?**

- 15
- 123
- **6** ✅
- 24

**Why:** The second `+` is a unary plus, so `+'2'` becomes `2`. The expression is then `1 + 2 + 3 = 6`, and `eval(6)` returns `6`.

---

## Q4 · CSS
**What happens when content overflows `.container`?**

```css
.container {
  width: 300px;
  height: 300px;
  overflow-x: hidden;
  overflow-y: scroll;
  overflow: auto;
}
```

- **Both horizontal and vertical scroll bars appear when needed** ✅
- Horizontal scroll bar appears, vertical scroll bar is hidden
- Vertical scroll bar appears, horizontal scroll bar is hidden
- No scroll bars appear at all

**Why:** The `overflow` shorthand comes last, so it overrides both longhands and sets each axis to `auto`.

---

## Q5 · ReactJS
**How would you implement a debounce effect for an input field using a custom `useDebounce(value, delay)` hook?**

- **Pass the input value and a delay to `useDebounce` and use the returned value** ✅
- Call `useDebounce` directly inside the render method
- Use `useDebounce` as an event handler for the input's `onDebouncedChange` event
- Use the `useDebounce` hook inside the input's onChange handler

**Why:** Hooks must be called at the top level of a component, never inside handlers. `onDebouncedChange` isn't a real event.

---

## Q6 · CSS
**Which CSS rule can be used to load a custom font located on an external server?**

- `font-family`
- **`font-face`** ✅
- `font-src`
- `custom-font`

**Why:** `@font-face { src: url(...) }` loads the font. `font-src` is a Content-Security-Policy directive, not a CSS rule.

---

## Q7 · NodeJS
**What is a recommended practice for managing packages to ensure you are using secure and trusted versions?**

- Always use the latest version of packages for maximum security
- Randomize package versions to avoid targeted version exploits
- Only use packages that are at least one year old for stability
- **Utilize a package-lock.json file to track exact versions and their dependencies** ✅

---

## Q8 · TypeScript
**Which utility type extracts the type of the last element of a tuple?**

- `type Last<T extends any[]> = T[T['length'] - 1]`
- **`type Last<T extends any[]> = T extends [...infer Rest, infer Last] ? Last : never`** ✅
- `type Last<T extends any[]> = T extends [infer First, ...infer Rest] ? typeof Rest[Rest['length'] - 1] : never`
- `type Last<T extends any[]> = T extends [infer First, ...(infer Rest)] ? Rest : never`

**Why:** TypeScript has no type-level arithmetic, so `- 1` doesn't work. The last option returns the remaining elements, not the last one.

---

## Q9 · CSS
**A block element has `width: 400px`. With `box-sizing: border-box; padding: 20px; border: 5px solid black;`, what is the computed width?**

- 370px
- **400px** ✅
- It becomes a flex container, and the width is undefined
- 450px
- 350px

**Why:** With `border-box`, padding and border sit inside the 400px. The content area works out to 350px. *(Possible ambiguity: if "computed width" is read as the content width, 350px could be argued.)*

---

## Q10 · ReactJS
**In which scenario is `useLayoutEffect` preferred over `useEffect`?**

- **When you need to make DOM measurements and update the layout synchronously** ✅
- When the effect does not interact with the DOM
- When your effect is computationally expensive and not related to DOM updates
- When you need to fetch data asynchronously on component mount

---

## Q11 · ReactJS
**An HOC `withSubscription` creates a new subscription for every wrapped component. How do you mitigate the redundant subscriptions without sacrificing the reusability and composition benefits of the HOC?**

- Replace the HOC with a global event bus
- Modify `withSubscription` to use a static property holding a single shared subscription
- Implement `shouldComponentUpdate` in `withSubscription`
- **Utilize a caching mechanism within `withSubscription` to serve the same data to all components subscribing to the same data source** ✅

---

## Q12 · JavaScript
**How many WebSocket events are there?**

- 3
- 5
- **4** ✅
- 2

**Why:** The four events are `open`, `message`, `error`, and `close`.

---

## Q13 · JavaScript *(multiple correct)*
**Which of the following is true about the JavaScript language?**

- **It is an object-oriented language** ✅
- **It is an interpreted language** ✅
- It is a compiled language
- It is a low-level language

**Why:** Modern engines JIT-compile JavaScript, but quizzes like this conventionally expect "interpreted."

---

## Q14 · TypeScript
**In a full-stack TypeScript project, which pattern is commonly used for sharing type definitions between frontend and backend?**

- **Isomorphic Code** ✅
- Monolithic Architecture
- Microservices Architecture
- Server-Side Rendering

---

## Q15 · NodeJS
**When using `url.parse()`, what is the default behavior regarding the query string?**

- The query string is always returned as a null value
- The query string is parsed into an object using the QueryString module by default
- **The query string is left as a string and not parsed into an object unless the second parameter is set to true** ✅
- `url.parse()` automatically encodes URI components during parsing

---

## Q16 · ReactJS
**When using `useRef`, what is the correct way to update the current value of the ref?**

- **`ref.current = newValue`** ✅
- `ref.setValue(newValue)`
- `setRef(newValue)`
- `ref(newValue)`

---

## Q17 · TypeScript
**A library needs to change a public function's signature. How can it maintain backward compatibility?**

- **Both using function overloading and applying default parameters** ✅
- Using function overloading
- Marking old parameters as deprecated
- Applying default parameters

---

## Q18 · ReactJS *(multiple correct)*
**How can you ensure a class component re-renders only when necessary?**

- **By using `PureComponent`** ✅
- By using HOC components
- By converting class component to functional component
- **We can define `shouldComponentUpdate` method to determine if it should re-render or not** ✅

---

## Q19 · JavaScript
**You want to run code only after all 3 Promises have resolved. Which function do you use?**

- **`Promise.all()`** ✅
- `Promise.race()`
- `Promise.any()`
- `Promise.each()`

---

## Q20 · ReactJS
**How can you prevent unnecessary re-renders in a function-based component?**

- By using `React.pure()`
- By wrapping the component in `React.lazy()`
- By using an HOC
- **By wrapping the component in `React.memo()`** ✅

---

## Q21 · CSS
**What is the output of this CSS on a square div?**

```css
div {
  width: 100px;
  height: 100px;
  background-color: red;
  border-radius: 50%;
  clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
}
```

- A red triangle with a semi-circle on the bottom half
- A red triangle pointing downwards
- A red circle
- **A red triangle pointing upwards** ✅

**Why:** The intended answer is an upward triangle. *(Ambiguity: the rendered shape is actually the overlap of the circle and the triangle, so the bottom corners come out rounded. Worth rewording if this question is generated for real candidates.)*


---

## Question 22 — CSS: Flex Shrink

**Question:** A and B each have a width of 250px. Their container is 400px wide and uses `display: flex`. Which property ensures A and B shrink to fit the container?

**Options:**
1. `flex-wrap: no-wrap;` on the children
2. **`flex-shrink: 2;` on the children** ✅
3. `flex-wrap: wrap;` on the children
4. `flex-shrink: 2;` on the parent

**Correct answer:** **2. `flex-shrink: 2;` on the children.**

**Explanation:** The two items request 500px in total, exceeding the 400px container by 100px. With equal base widths and equal shrink factors, each shrinks to 200px (assuming no min-width, padding, or other constraints). `flex-shrink` applies to flex items, not the parent.

```css
.container { display: flex; width: 400px; }
.child { width: 250px; flex-shrink: 2; }
```

**Interview takeaway:** The default `flex-shrink: 1` would also make these items shrink. Setting `2` is the best answer among the supplied options; it isn't required for this example.

---

## Questions 23–25 — Frontend Interview Revision

## Question 23 — ReactJS: Bundle Size Optimization

**Question:** Which of the following is a recommended practice for reducing the bundle size in a React application?

**Options:**
1. Including all library dependencies in a single, minified file
2. Utilizing React's Concurrent Mode to optimize rendering
3. Applying aggressive code splitting at the component level
4. Storing all component state in a centralized global store

**Correct answer:** **3. Applying aggressive code splitting at the component level.**

**Explanation:** Code splitting divides JavaScript into chunks loaded when needed, reducing the *initial* bundle downloaded by users. It doesn't necessarily reduce the total amount of JavaScript across all chunks. Prefer splitting at meaningful boundaries (routes, heavy features) rather than splitting every tiny component.

```tsx
import { lazy, Suspense } from 'react';

const Reports = lazy(() => import('./Reports'));

export function App() {
  return (
    <Suspense fallback={<p>Loading reports...</p>}>
      <Reports />
    </Suspense>
  );
}
```

**Interview takeaway:** Use dynamic imports, route-level splitting, tree shaking, and bundle analysis. React rendering concurrency and global state management do not directly shrink bundles.

---

## Question 24 — JavaScript: Missing Expression

**Visible question:** “What does evaluate to?”

**Options:**
1. Type Error
2. `1`
3. Syntax Error
4. Reference Error

**Correct answer:** **Cannot be determined from the provided screenshots.** The expression being evaluated is missing or not rendered. Selecting “Type Error” without seeing the expression would be a guess.

**Review concepts:**
- **TypeError:** An operation is attempted on a value of an inappropriate type, e.g. `null.x`.
- **SyntaxError:** JavaScript cannot parse the source, e.g. `const = 1;`.
- **ReferenceError:** A variable name cannot be resolved, e.g. `console.log(unknownVariable);`.
- **`1`:** A valid expression can evaluate to the number `1`, e.g. `Number(true)`.

**Action:** Recover the missing expression before choosing an answer.

---

## Question 25 — TypeScript: Primitive Values vs Object References

**Question:** Given the following TypeScript code snippet, what will be the output?

```ts
let a: number = 42;
let b: number = a;
let obj1: { value: number } = { value: 42 };
let obj2: { value: number } = obj1;

a = 10;
obj1.value = 10;

console.log(b);
console.log(obj2.value);
```

**Options:**
1. 42 and 10
2. 10 and 42
3. 10 and 10
4. 42 and 42

**Correct answer:** **1. 42 and 10.**

**Explanation:** `b = a` copies the primitive number value, so changing `a` does not change `b`. `obj2 = obj1` copies a reference to the same object, so modifying `obj1.value` is visible through `obj2`.

**Output:**

```text
42
10
```

**Interview takeaway:** Primitive assignments copy values; object assignments share a reference to the same object unless a new object is created (for example with `{ ...obj1 }`, which is a shallow copy).
