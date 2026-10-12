# JavaScript interview questions and answers

This guide uses the MDN JavaScript documentation fetched with `opensrc path mdn/content` and the [ECMAScript language specification](https://github.com/tc39/ecma262/blob/main/spec.html) fetched with `opensrc path tc39/ecma262`. Section links point to the public pages corresponding to files read from the opensrc cache. MDN's `main` branch changes over time, and JavaScript features reach runtimes at different times, so check compatibility for a target browser or Node.js version.

The [older question bank](https://github.com/alfa155518/modern-JavaScript-React-interview-questions-) repeats the event-loop and async questions and mixes language features with browser APIs. This file keeps those categories distinct and adds modern standard APIs. The answers come from the fetched docs, with short code exercises at the end.

## Language and values

Sources: [JavaScript overview](https://developer.mozilla.org/en-US/docs/Web/JavaScript), [data types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Data_structures), [equality comparisons](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Equality_comparisons_and_sameness), [`typeof`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof), [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt), [Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol).

### 1. What is JavaScript, and what is ECMAScript?

JavaScript is a dynamic, garbage-collected language with first-class functions. ECMAScript specifies its core language behavior. Browsers and other hosts add APIs such as the DOM, timers, and network access.

### 2. What are the primitive types?

`undefined`, `null`, `boolean`, `number`, `bigint`, `string`, and `symbol`. Objects are the other broad category of values. Arrays and functions are objects.

### 3. What is the difference between `null` and `undefined`?

`undefined` commonly means a value was not supplied or a property is absent. `null` is an explicit absence value. They compare loosely equal, but not strictly equal.

### 4. Why does `typeof null` return `"object"`?

It is a historical JavaScript behavior. `null` is still a primitive value, not an object.

### 5. What is `NaN`, and how should you check for it?

`NaN` is a number value representing an invalid numeric result. `NaN === NaN` is false; `Number.isNaN(value)` checks whether a value is actually `NaN` without first coercing it.

### 6. How do `==` and `===` differ?

`===` compares without converting operand types. `==` may convert operands according to defined coercion rules. Prefer `===` unless a specific loose comparison is intentional, such as `value == null` to match both `null` and `undefined`.

### 7. How is `Object.is()` different from `===`?

`Object.is(NaN, NaN)` is true, while `NaN === NaN` is false. `Object.is(0, -0)` is false, while `0 === -0` is true.

### 8. What does truthy or falsy mean?

Boolean contexts convert values to `true` or `false`. Falsy values include `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, and `NaN`; objects and arrays are truthy, even when empty.

### 9. When should you use `??` instead of `||`?

`??` uses its right side only when the left side is `null` or `undefined`. `||` also replaces valid falsy values such as `0`, `false`, and `""`.

### 10. What does optional chaining do?

`obj?.prop`, `obj?.[key]`, and `fn?.()` stop and return `undefined` when the value on the left is `null` or `undefined`. They do not hide errors from other invalid operations.

### 11. When is `BigInt` useful?

It represents integers beyond the safe integer range of `Number`. Most arithmetic cannot mix `bigint` and `number` without explicit conversion, and `JSON.stringify` does not serialize a BigInt by default.

### 12. What is a `Symbol`?

It is a unique primitive often used as an object property key or to customize built-in protocols such as iteration. Two calls to `Symbol('x')` produce distinct values.

### 13. Are objects compared by their contents with `===`?

No. Objects compare by identity. Two separate objects with equal properties are not strictly equal; two references to the same object are.

## Declarations, scope, and control flow

Sources: [grammar and types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const), [`var`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var), [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode), [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring), [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals).

### 14. How do `let`, `const`, and `var` differ?

`let` and `const` are block-scoped; `var` is function-scoped or global-scoped. `const` prevents reassignment of the binding, while `let` allows it. `var` can be redeclared in the same scope.

### 15. Does `const` make an object immutable?

No. It prevents assigning a different value to the variable. The object's properties can still change unless you take separate steps such as `Object.freeze()`.

### 16. What is the temporal dead zone?

From the start of a block until a `let` or `const` declaration is initialized, accessing that binding throws a `ReferenceError`. The binding exists but is not ready to read.

### 17. What does "hoisting" explain?

It describes how declarations affect a scope before execution reaches their source line. Function declarations can generally be called earlier; `var` reads as `undefined` before assignment; `let` and `const` remain in the temporal dead zone.

### 18. What is lexical scope?

A function resolves variable names through the scopes where it was defined, not the scopes from which it is later called.

### 19. What does destructuring do?

It extracts values from an array or properties from an object into bindings. You can rename object bindings and provide defaults for values that are `undefined`.

### 20. What are rest and spread syntax?

Rest collects remaining arguments or properties into a new binding. Spread expands an iterable into elements or an object's own enumerable properties into another object.

### 21. Is object spread a deep copy?

No. It copies only one level. Nested objects retain their references, so changing a nested object can affect both structures.

### 22. What are template literals for?

Backticks allow interpolation with `${expression}` and multiline strings. A tagged template passes the literal parts and substitutions to a function for custom processing.

### 23. What does strict mode change?

It catches some silent mistakes as errors and changes some semantics, including the `this` value for a plain function call. Modules and classes run in strict mode automatically.

### 24. How do `for...in` and `for...of` differ?

`for...in` enumerates enumerable property keys, including inherited ones. `for...of` consumes values from an iterable such as an array, string, Map, or Set.

## Functions, closures, and iteration

Sources: [functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions), [closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures), [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [`this`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this), [`call`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call), [`apply`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply), [`bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind), [iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols), [iterators and generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_generators).

### 25. What is a first-class function?

A function is a value. You can assign it to a variable, pass it as an argument, return it from another function, or store it in an object.

### 26. What is a closure?

A closure is a function together with references to its lexical environment. It can read outer variables even after the outer function has returned.

### 27. What is the difference between a function declaration and expression?

A declaration defines a named function binding and can usually be called before its source line. An expression creates a function value where the expression runs; access before assignment follows the variable declaration's rules.

### 28. How do arrow functions handle `this`?

They capture `this` from the surrounding scope instead of creating their own. An arrow function is a poor choice for an object method that needs `this` from its call site.

### 29. How is a regular function's `this` chosen?

It usually depends on how the function is called: `obj.method()` uses `obj`, while a plain call gets `undefined` in strict mode. `call`, `apply`, and `bind` can set the value explicitly.

### 30. How do `call`, `apply`, and `bind` differ?

`call(thisArg, a, b)` invokes immediately with separate arguments. `apply(thisArg, [a, b])` invokes immediately with an array-like argument list. `bind(thisArg, a)` returns a new function with the binding and optional leading arguments fixed.

### 31. What is the difference between rest parameters and `arguments`?

Rest parameters collect remaining arguments into a real array. `arguments` is an array-like object available in ordinary functions, but not as an arrow function's own binding.

### 32. What happens when a default parameter receives `null`?

The default is used only for `undefined` or a missing argument. Passing `null` keeps `null`.

### 33. What is a higher-order function?

It accepts a function, returns a function, or both. Array `map`, `filter`, and `reduce` are common examples that accept callbacks.

### 34. What is a generator function?

A `function*` call returns a generator object. Each `next()` resumes execution until the next `yield` or completion; generators also implement the iterator protocol.

### 35. What makes an object iterable?

Its `Symbol.iterator` method returns an iterator whose `next()` returns `{ value, done }`. `for...of` and spread consume this protocol.

### 36. How is an async iterator consumed?

It exposes `Symbol.asyncIterator` and yields asynchronously. Use `for await...of` to await each step.

## Objects, prototypes, and classes

Sources: [objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_objects), [prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain), [`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [private elements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_elements), [`Object.create`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create), [`Object.assign`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign), [`Object.freeze`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze), [`Proxy`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy).

### 37. What is the prototype chain?

An object has an internal `[[Prototype]]` link. If a property is not found on the object, lookup continues through its prototypes until it finds the property or reaches `null`.

### 38. How do `prototype` and an object's prototype differ?

`prototype` is a property on a constructor function used by `new` for instances. An instance's `[[Prototype]]` is its internal inheritance link; inspect it with `Object.getPrototypeOf()`.

### 39. What does `new` do?

It creates an object linked to the constructor's `prototype`, calls the constructor with that object as `this`, and normally returns that object unless the constructor explicitly returns another object.

### 40. Are JavaScript classes a separate inheritance system?

No. Class syntax is built on prototypes. It provides constructors, methods, inheritance with `extends`, and private elements, with class-specific semantics.

### 41. What are private class fields?

Names beginning with `#` are accessible only inside the class that declares them. They are not ordinary string-keyed properties and cannot be read with bracket notation from outside.

### 42. What does `Object.create(proto)` do?

It creates a new object whose `[[Prototype]]` is `proto`. Passing `null` creates an object with no prototype methods inherited from `Object.prototype`.

### 43. What is an own property?

It belongs directly to an object rather than being inherited. `Object.hasOwn(obj, key)` checks this without depending on the object's prototype.

### 44. What does `Object.assign()` copy?

It assigns enumerable own properties from source objects onto a target object. The copy is shallow, and setters on the target can run during assignment.

### 45. Does `Object.freeze()` deeply freeze an object?

No. It freezes the object's own properties and extensibility at one level. Nested objects remain mutable unless frozen separately.

### 46. What does a `Proxy` do?

It wraps a target object and uses traps to customize operations such as reading, writing, or checking properties. The traps must obey language invariants for the target.

### 47. What is a property descriptor?

It records attributes such as `value`, `writable`, `enumerable`, and `configurable`, or getter and setter functions. `Object.getOwnPropertyDescriptor()` reads it.

## Arrays and collections

Sources: [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array), [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map), [`filter`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter), [`reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce), [`sort`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort), [`toSorted`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted), [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set), [WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap), [WeakSet](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet).

### 48. What do `map`, `filter`, and `reduce` return?

`map` returns an array of transformed elements. `filter` returns an array of elements that pass a test. `reduce` combines elements into one accumulator value, whose type you choose.

### 49. Does `sort()` mutate its array?

Yes. It sorts in place and returns the same array. Without a comparator, it sorts elements as strings, so numeric sorting needs a comparator such as `(a, b) => a - b`.

### 50. What is `toSorted()`?

It is the copying counterpart of `sort()`. It returns a sorted array while leaving the original array unchanged.

### 51. What is the difference between `slice()` and `splice()`?

`slice()` returns a selected portion without modifying the source array. `splice()` changes the source by removing, replacing, or inserting elements.

### 52. What does `flat()` do?

It returns a new array with nested arrays flattened to a chosen depth. The default depth is one; `flat(Infinity)` recursively flattens all nested arrays.

### 53. When would you use `Map` instead of a plain object?

Use `Map` for a collection of key-value entries with keys of any type, predictable insertion-order iteration, and a `size` property. Use an object when you are modeling a record with known property names.

### 54. What does a `Set` store?

It stores unique values and iterates them in insertion order. It uses SameValueZero equality, so `NaN` is considered the same value as another `NaN`.

### 55. What do `union()` and `intersection()` add to `Set`?

They create new sets containing values in either set or in both sets, respectively. They do not mutate the original set.

### 56. How does `WeakMap` differ from `Map`?

WeakMap keys are weakly held so the collection does not keep an otherwise unreachable key alive. It is not iterable and has no `size`, because its contents depend on garbage collection.

### 57. What can a `WeakSet` contain?

It holds garbage-collectable values such as objects and nonregistered symbols. Like `WeakMap`, it is not iterable.

### 58. What does `Object.groupBy()` return?

It groups iterable items by a callback's string or symbol key and returns an object whose properties contain arrays of matching items. Use `Map.groupBy()` when group keys should retain arbitrary types.

### 59. Is `JSON.stringify()` a general deep-clone tool?

No. JSON serialization drops or changes unsupported values and cannot represent cycles. Use a cloning approach suited to the data, such as the host's `structuredClone()` when its supported types fit.

## Promises, async functions, and the execution model

Sources: [execution model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model), [promises guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises), [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), [async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function), [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await), [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all), [`Promise.allSettled`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled), [`Promise.any`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any), [`Promise.race`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race), [microtask guide](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide).

### 60. What are the states of a Promise?

A Promise starts pending and eventually becomes fulfilled with a value or rejected with a reason. Fulfilled and rejected are both settled states.

### 61. What does `.then()` return?

It returns a new Promise. The handler's returned value fulfills that Promise; a thrown error rejects it; returning another Promise makes the chain follow that Promise's outcome.

### 62. How does `.catch()` work?

It adds a rejection handler and returns a new Promise. It handles rejections from earlier steps in the chain, unless a later step creates a new rejection.

### 63. What does `.finally()` receive and return?

Its callback receives no fulfillment value or rejection reason. It runs on either outcome and normally passes the prior result through, unless the callback throws or returns a rejected Promise.

### 64. What does an `async` function return?

It always returns a Promise. A returned value fulfills it; a thrown error rejects it.

### 65. Does `await` block the whole JavaScript runtime?

No. It pauses the current async function and lets other work proceed. Execution resumes in a later microtask when the awaited value settles.

### 66. When should you use `Promise.all()`?

Use it to await independent operations together. It fulfills with results in input order after all fulfill, and rejects when one rejects.

### 67. How do `allSettled`, `any`, and `race` differ?

`allSettled` waits for every outcome. `any` fulfills with the first fulfillment or rejects with an `AggregateError` if all reject. `race` settles with the first settled input, whether fulfilled or rejected.

### 68. What is the event loop?

The host runs jobs while coordinating tasks and microtasks. JavaScript runs one job to completion before starting another in the same agent; Promise reactions and `queueMicrotask` schedule microtasks.

### 69. Why can a Promise callback run before a zero-delay timer?

After a task finishes, the browser drains its microtask queue before starting another task. A resolved Promise's `.then()` callback is a microtask; a timer callback is a later task.

### 70. What is a common async error-handling mistake?

Starting a Promise without returning or awaiting it can detach its rejection from the caller's chain. Return or await the operation so errors reach the intended handler.

## Modules and errors

Sources: [modules guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules), [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import), [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export), [dynamic `import()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import), [`await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await), [`try...catch`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch), [Error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error).

### 71. What are ECMAScript modules?

They are files with explicit `import` and `export` declarations. Each module has its own scope, and imported bindings stay linked to the exporting module.

### 72. Are imported bindings snapshots?

No. They are live bindings: when the exporter updates its binding, importers observe the new value. An importer cannot reassign the imported binding.

### 73. How do named and default exports differ?

A module can have several named exports and one default export. Import named exports with braces; import the default under a local name without braces.

### 74. What does dynamic `import()` return?

It returns a Promise that fulfills with a module namespace object. Use it when a module should load conditionally or later in the program.

### 75. Where can top-level `await` be used?

Inside an ECMAScript module, top-level `await` can delay evaluation of modules that depend on it. Ordinary scripts cannot use it at top level.

### 76. What does `try...catch...finally` do?

`catch` handles a thrown exception from `try`. `finally` runs after `try` or `catch` before control leaves the construct, including when a return or throw is pending.

### 77. Why throw an `Error` object rather than a string?

An `Error` carries a message and debugging information such as a stack in common runtimes. Subclasses can represent distinct failure types; a `cause` can preserve the underlying error.

### 78. What is the difference between an exception and a rejected Promise?

A synchronous `throw` exits the current call unless caught. A throw in an async function rejects its returned Promise, and callers handle it with `await` plus `try...catch` or with `.catch()`.

## Browser APIs, events, and scheduling

Sources: [JavaScript execution model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model), [event bubbling](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Event_bubbling), [`addEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener), [`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout), [`setInterval`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setInterval), [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch), [AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController), [`structuredClone`](https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone).

### 79. Are the DOM, `fetch`, and timers part of ECMAScript?

No. They are host APIs. Browsers provide them to JavaScript, while other runtimes may offer different APIs or compatible implementations.

### 80. What are capture and bubble phases?

During capture, an event travels down the ancestor chain toward its target. During bubbling, a bubbling event travels back up. `addEventListener` listens in the bubbling phase by default; `{ capture: true }` selects capture.

### 81. What is event delegation?

Attach one listener to an ancestor and inspect the bubbling event's target or its closest matching ancestor. It can handle matching descendants, including ones added later, without one listener per child.

### 82. What is the difference between `target` and `currentTarget`?

`target` is the node where the event originated. `currentTarget` is the node whose listener is currently running.

### 83. What do `preventDefault()` and `stopPropagation()` change?

`preventDefault()` cancels a cancelable default browser action. `stopPropagation()` prevents further travel through the event path; it does not cancel the default action.

### 84. Does `setTimeout(fn, 0)` run `fn` immediately?

No. It schedules a timer task after the current work and applicable microtasks. Actual delay depends on the host's event loop and timer rules.

### 85. When might `setInterval` be the wrong tool for repeated async work?

An interval schedules by time rather than by completion of the previous async operation. Use a self-scheduling timeout if the next run must wait until the prior one finishes.

### 86. Does `fetch()` reject for an HTTP 404 or 500?

Normally no. It fulfills with a `Response`; check `response.ok` or `response.status`. It rejects for failures such as network errors or abortion.

### 87. How do you cancel a fetch request?

Create an `AbortController`, pass its `signal` to `fetch`, and call `abort()` when the request is no longer needed. Handle the resulting abort rejection.

### 88. What does `structuredClone()` do?

It uses the structured clone algorithm to deep-clone supported values and can transfer some objects. It is a browser API here, not a general ECMAScript operator, and it cannot clone every value, such as functions.

## Newer documented APIs

Sources: [`toSorted`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted), [`Object.groupBy`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/groupBy), [`Set.union`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/union), [`Promise.withResolvers`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/withResolvers), [iterator helpers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Iterator/map), [`using`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/using), [`await using`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/await_using), [TC39 decorators proposal](https://github.com/tc39/proposal-decorators/blob/master/README.md).

### 89. Which array methods preserve the original array while changing order or contents?

`toSorted()`, `toReversed()`, `toSpliced()`, and `with()` return changed copies instead of mutating the source. Their results are still shallow copies.

### 90. What is `Promise.withResolvers()`?

It returns an object with a new Promise plus separate `resolve` and `reject` functions. It is useful when completion is triggered outside the Promise executor.

### 91. What do iterator helpers such as `.map()` change?

They transform values lazily as an iterator is consumed, instead of building an intermediate array at each step. A helper returns another iterator.

### 92. What does a `using` declaration do?

It declares a block-scoped resource that calls its `Symbol.dispose` method when the scope exits. `await using` supports asynchronous disposal through `Symbol.asyncDispose`; check runtime support before using either.

### 93. Are decorators built into the ECMAScript language covered by this guide?

No. Decorator syntax may be available through tools or proposals, but do not assume that a framework's decorator transform is a standard JavaScript feature. Check the target runtime and toolchain.

## Short coding exercises

These solutions combine the documented language and browser APIs linked below. State assumptions before coding in an interview, especially for Unicode text, mutation, and invalid inputs.

Sources: [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set), [array iteration methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array), [`flat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat), [`toSorted`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted), [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), [`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout), [`clearTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/Window/clearTimeout).

### 94. How do you remove duplicate primitive values while preserving order?

```js
const unique = (values) => [...new Set(values)];
```

Set uses value equality for primitives. Separate object references remain separate entries, even if their properties match.

### 95. How do you check whether a string is a palindrome?

For an exact, case-sensitive comparison of Unicode code points:

```js
function isPalindrome(text) {
  const chars = [...text];
  return chars.every((ch, i) => ch === chars[chars.length - 1 - i]);
}
```

This does not normalize case, punctuation, combining marks, or grapheme clusters.

### 96. How do you count occurrences of each character?

```js
function countCharacters(text) {
  const counts = new Map();
  for (const ch of text) counts.set(ch, (counts.get(ch) ?? 0) + 1);
  return counts;
}
```

`for...of` iterates string code points; user-perceived characters can contain multiple code points.

### 97. How do you flatten nested arrays?

```js
const flattened = nested.flat(Infinity);
```

This returns a new array. It removes empty slots and keeps references to non-array object elements.

### 98. How do you sort numbers without mutating the input?

```js
const ascending = numbers.toSorted((a, b) => a - b);
const descending = numbers.toSorted((a, b) => b - a);
```

The comparator prevents default string-based sorting.

### 99. How do you merge two already sorted numeric arrays in linear time?

```js
function mergeSorted(a, b) {
  const result = [];
  let i = 0;
  let j = 0;
  while (i < a.length && j < b.length) {
    result.push(a[i] <= b[j] ? a[i++] : b[j++]);
  }
  return result.concat(a.slice(i), b.slice(j));
}
```

This runs in `O(a.length + b.length)` time and does not change either input.

### 100. How do you test whether an integer is prime?

```js
function isPrime(n) {
  if (!Number.isInteger(n) || n < 2) return false;
  for (let divisor = 2; divisor * divisor <= n; divisor++) {
    if (n % divisor === 0) return false;
  }
  return true;
}
```

Testing divisors only through the square root gives `O(sqrt(n))` time.

### 101. How do you produce the first `n` Fibonacci numbers?

```js
function fibonacci(n) {
  if (!Number.isInteger(n) || n < 0) throw new RangeError('n must be a nonnegative integer');
  const values = [];
  let a = 0;
  let b = 1;
  for (let i = 0; i < n; i++) {
    values.push(a);
    [a, b] = [b, a + b];
  }
  return values;
}
```

Large terms exceed `Number.MAX_SAFE_INTEGER`; use BigInt if exact large values are required.

### 102. How do you debounce a browser event handler?

```js
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

This calls `fn` after calls stop for `delay` milliseconds. It does not preserve a dynamic `this` value; use a regular returned function if that matters.

## Quick reasoning checks

Sources: [closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures), [equality comparisons](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Equality_comparisons_and_sameness), [Promise jobs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model), [`Map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), [`Array.sort`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort).

### 103. Why do callbacks created in a `var` loop often read the final index?

`var` creates one function-scoped binding shared by the closures. `let` creates a new binding for each loop iteration.

### 104. What is the output of `console.log([] === [])`?

`false`. Each array literal creates a distinct object, and strict equality compares object identity.

### 105. What does `console.log(0 || 5, 0 ?? 5)` print?

`5 0`. The `||` operator treats `0` as falsy; `??` treats only `null` and `undefined` as missing.

### 106. What is the output order of a synchronous log, a resolved Promise callback, and a zero-delay timer?

The synchronous log runs first, the Promise callback runs in a microtask, and the timer runs in a later task. A host may impose additional timer delay.

### 107. Why is `[10, 2].sort()` surprising?

Without a comparator, sorting compares string forms, so `10` comes before `2`. Use `(a, b) => a - b` for numeric order.

### 108. Why does `map.get(key) || defaultValue` sometimes lose a stored value?

Stored `0`, `false`, or `""` values are falsy. Use `map.get(key) ?? defaultValue` if `null` and `undefined` mean missing, or `map.has(key)` when `undefined` itself is a valid stored value.

### 109. Why is a shallow copy not enough to isolate nested state?

The copy shares nested object references with the original. Copy each changed level or use a data structure and update method that produces the required independent values.

### 110. Why does `await Promise.all(tasks.map(asyncTask))` run work differently from a `for` loop with `await`?

`map` calls `asyncTask` for every item before the combined await, so independent operations can overlap. A loop that awaits inside each iteration starts the next operation only after the previous one settles.

## Memory and built-in data tools

Sources: [memory management](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Memory_management), [prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain), [typed arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Typed_arrays), [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp), [`RegExp.test`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test), [`String.match`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match), [Intl](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl), [WeakRef](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef).

### 111. How does JavaScript reclaim memory?

Engines use garbage collection to reclaim objects that are no longer reachable from roots. Collection timing is not under normal application control, so release references to data you no longer need.

### 112. Can a closure cause a memory leak?

A closure keeps access to its captured environment while the closure remains reachable. A long-lived callback or listener can retain data unintentionally; remove listeners and references when their work ends.

### 113. Is `obj.__proto__` the same as `Constructor.prototype`?

No. `obj.__proto__` is a legacy accessor for the object's internal `[[Prototype]]`; `Constructor.prototype` is a property used to set that link for instances made with `new`. Prefer `Object.getPrototypeOf(obj)` for inspection.

### 114. What are typed arrays for?

They are views over binary buffers for reading and writing fixed numeric representations. They are useful for files, media, network protocols, and other binary data.

### 115. What is the difference between `RegExp.test()` and `String.match()`?

`test()` returns a boolean for whether a pattern matches. `match()` returns match details or `null`, with behavior affected by the regular expression's flags.

### 116. What does `Intl` provide?

Locale-sensitive formatting and comparison APIs, including numbers, dates, and text collation. Use it instead of hard-coding one locale's presentation rules.

### 117. When is `WeakRef` appropriate?

Rarely. It holds a weak reference without keeping the target alive, but garbage collection timing is unpredictable. Prefer normal ownership and cleanup rules unless a weak cache truly needs this behavior.

## Source and update notes

- opensrc cached `mdn/content` and `tc39/ecma262` from their `main` branches. The language answers use MDN files under `files/en-us/web/javascript/` and the ECMAScript specification in `spec.html`.
- Browser questions use MDN files under `files/en-us/web/api/` and `files/en-us/learn_web_development/`. They describe browser behavior, not the ECMAScript language itself.
- The decorators answer also uses the `tc39/proposal-decorators` repository fetched through opensrc. At the fetched snapshot, its README labels decorators a proposal.
- For a new target runtime, refresh the opensrc sources and check MDN compatibility data before treating a newer API as available.
