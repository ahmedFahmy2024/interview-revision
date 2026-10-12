# modern TypeScript interview Questions & Answers

Click ⭐ if you like the project. Follow for more updates.

---

# TypeScript Interview Questions 🔷

| #   | Question                                                              |
| --- | --------------------------------------------------------------------- |
| 1   | What is TypeScript and why use it over JavaScript?                    |
| 2   | What is the difference between `type` and `interface` in TypeScript?  |
| 3   | What are union and intersection types?                                |
| 4   | What are generics in TypeScript?                                      |
| 5   | What is type narrowing and how does it work?                          |
| 6   | What are type guards in TypeScript?                                   |
| 7   | What are enums in TypeScript?                                         |
| 8   | What is the difference between `any`, `unknown`, and `never`?         |
| 9   | What are utility types in TypeScript?                                 |
| 10  | What is the difference between `Partial`, `Required`, and `Readonly`? |
| 11  | What are `Pick`, `Omit`, `Record`, `Exclude`, and `Extract`?          |
| 12  | What are mapped types in TypeScript?                                  |
| 13  | What are conditional types in TypeScript?                             |
| 14  | What is the `infer` keyword in TypeScript?                            |
| 15  | What are template literal types?                                      |
| 16  | What is the `keyof` operator?                                         |
| 17  | What is the `typeof` operator in TypeScript?                          |
| 18  | What is the `satisfies` operator?                                     |
| 19  | What are decorators in TypeScript?                                    |
| 20  | What are access modifiers in TypeScript classes?                      |
| 21  | What is the difference between `interface` and `abstract class`?      |
| 22  | What is declaration merging in TypeScript?                            |
| 23  | What are index signatures in TypeScript?                              |
| 24  | What is a discriminated union?                                        |
| 25  | What is the `readonly` modifier and `ReadonlyArray`?                  |
| 26  | What are function overloads in TypeScript?                            |
| 27  | What is the `as const` assertion?                                     |
| 28  | What is the `NonNullable` utility type?                               |
| 29  | What are `ReturnType` and `Parameters` utility types?                 |
| 30  | What is module augmentation in TypeScript?                            |
| 31  | What is the difference between `strict` mode flags?                   |
| 32  | What is structural typing in TypeScript?                              |
| 33  | What are namespace and module in TypeScript?                          |
| 34  | What is the `Awaited` utility type?                                   |
| 35  | What are tuple types in TypeScript?                                   |
| 36  | What is the `noUncheckedIndexedAccess` compiler option?               |
| 37  | What is the difference between `extends` and `implements`?            |
| 38  | What are optional and default parameters in TypeScript?               |
| 39  | What is the `using` keyword in TypeScript 5?                          |
| 40  | What are some TypeScript best practices for large codebases?          |

---

# TypeScript Questions & Answers

---

### 1. What is TypeScript and why use it over JavaScript?

TypeScript is a **strongly typed superset of JavaScript** developed by Microsoft. It compiles down to plain JavaScript and adds optional static types, interfaces, generics, and modern tooling support.

Why use TypeScript:

- Catches type errors at **compile time**, not runtime
- Better IDE support — autocomplete, refactoring, go-to-definition
- Self-documenting code — types serve as inline documentation
- Safer refactoring in large codebases
- Works with all existing JavaScript libraries

Example:

```typescript
// JavaScript — error only discovered at runtime
function add(a, b) {
  return a + b;
}
console.log(add("5", 10)); // "510" — silent bug

// TypeScript — error caught at compile time
function add(a: number, b: number): number {
  return a + b;
}
console.log(add("5", 10));
// Error: Argument of type 'string' is not assignable to parameter of type 'number'
```

---

### 2. What is the difference between `type` and `interface` in TypeScript?

Both define the shape of an object, but they have key differences:

| Feature             | `interface`            | `type`                        |
| ------------------- | ---------------------- | ----------------------------- |
| Extension           | `extends` keyword      | Intersection `&`              |
| Declaration merging | ✅ Yes                 | ❌ No                         |
| Primitives / unions | ❌ No                  | ✅ Yes                        |
| Computed properties | Limited                | ✅ Yes                        |
| Preferred for       | Object shapes, classes | Unions, tuples, complex types |

Example:

```typescript
// Interface — extendable, mergeable
interface Animal {
  name: string;
}
interface Bear extends Animal {
  honey: boolean;
}

// Declaration merging (only interfaces)
interface Window {
  myCustomProp: string; // merged into existing Window type
}

// Type alias — supports unions, tuples, primitives
type ID = string | number;
type Point = [number, number];
type BearType = Animal & { honey: boolean };

// Type with computed keys
type EventMap = {
  [K in "click" | "focus" | "blur"]: (e: Event) => void;
};
```

---

### 3. What are union and intersection types?

- **Union type** (`|`): A value can be **one of several types**. TypeScript narrows the type based on checks.
- **Intersection type** (`&`): A value must satisfy **all combined types** simultaneously.

Example:

```typescript
// Union — one OR the other
type ID = string | number;

function printId(id: ID) {
  if (typeof id === "string") {
    console.log(id.toUpperCase()); // string methods available
  } else {
    console.log(id.toFixed(2)); // number methods available
  }
}

printId("abc123"); // ABC123
printId(42); // 42.00

// Intersection — all combined
interface Serializable {
  serialize(): string;
}
interface Loggable {
  log(): void;
}

type SerializableLogger = Serializable & Loggable;

// Generic union utility
type OrNull<T> = T | null;
type OneOrMany<T> = T | T[];
type OneOrManyOrNull<T> = OrNull<OneOrMany<T>>;

let val: OneOrManyOrNull<string> = "hello";
let val2: OneOrManyOrNull<string> = ["hello", "world"];
let val3: OneOrManyOrNull<string> = null;
```

---

### 4. What are generics in TypeScript?

Generics let you write **reusable, type-safe code** that works with any type while preserving type information. They act as type parameters — placeholders filled in when the function or class is used.

Example:

```typescript
// Generic function
function identity<T>(value: T): T {
  return value;
}
identity<string>("hello"); // T = string
identity<number>(42); // T = number

// Generic with constraint
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
const user = { name: "Ahmed", age: 30 };
getProperty(user, "name"); // string ✅
getProperty(user, "email"); // Error: not a key of user ❌

// Generic interface
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

type UserResponse = ApiResponse<{ id: number; name: string }>;

// Generic class
class Stack<T> {
  private items: T[] = [];
  push(item: T): void {
    this.items.push(item);
  }
  pop(): T | undefined {
    return this.items.pop();
  }
  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }
}

const stack = new Stack<number>();
stack.push(1);
stack.push(2);
console.log(stack.pop()); // 2
```

---

### 5. What is type narrowing and how does it work?

Type narrowing is TypeScript's ability to **refine a broader type to a more specific one** inside a conditional block. TypeScript analyzes control flow to track what type a variable can be at each point.

Example:

```typescript
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    // TypeScript knows padding is number here
    return " ".repeat(padding) + input;
  }
  // TypeScript knows padding is string here
  return padding + input;
}

// Narrowing with truthiness
function printLength(value: string | null | undefined) {
  if (value) {
    // value is string here — null and undefined are falsy
    console.log(value.length);
  }
}

// Narrowing with in operator
interface Cat {
  meow(): void;
}
interface Dog {
  woof(): void;
}

function makeSound(animal: Cat | Dog) {
  if ("meow" in animal) {
    animal.meow(); // Cat
  } else {
    animal.woof(); // Dog
  }
}

// Narrowing with instanceof
function formatDate(value: Date | string) {
  if (value instanceof Date) {
    return value.toISOString();
  }
  return new Date(value).toISOString();
}
```

---

### 6. What are type guards in TypeScript?

Type guards are expressions that perform runtime checks and tell TypeScript which type a value is within a scope. They can be built-in (`typeof`, `instanceof`, `in`) or custom user-defined functions using the `is` keyword.

Example:

```typescript
// typeof guard
function formatCommandline(c: string | string[]) {
  if (typeof c === "string") {
    return c.trim();
  }
  return c.join(" ");
}

// instanceof guard
class Dog {
  woof() {
    console.log("Woof!");
  }
}
class Cat {
  meow() {
    console.log("Meow!");
  }
}

function makeSound(pet: Dog | Cat) {
  if (pet instanceof Dog) {
    pet.woof();
  } else {
    pet.meow();
  }
}

// Custom type guard with 'is'
interface Fish {
  swim(): void;
}
interface Bird {
  fly(): void;
}

function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

function move(pet: Fish | Bird) {
  if (isFish(pet)) {
    pet.swim(); // TypeScript knows it's Fish
  } else {
    pet.fly(); // TypeScript knows it's Bird
  }
}

// Maybe type with type guard
type Maybe<T> = T | void;

function isDefined<T>(x: Maybe<T>): x is T {
  return x !== undefined && x !== null;
}
```

---

### 7. What are enums in TypeScript?

Enums define a set of named constants. TypeScript supports numeric enums, string enums, and const enums.

Example:

```typescript
// Numeric enum (auto-increments from 0)
enum Direction {
  Up, // 0
  Down, // 1
  Left, // 2
  Right, // 3
}
console.log(Direction.Up); // 0
console.log(Direction[0]); // "Up" (reverse mapping)

// String enum (no reverse mapping)
enum Status {
  Active = "ACTIVE",
  Inactive = "INACTIVE",
  Pending = "PENDING",
}

function updateUser(status: Status) {
  console.log(`Status: ${status}`);
}
updateUser(Status.Active); // "Status: ACTIVE"

// Const enum — inlined at compile time, no runtime object
const enum HttpMethod {
  GET = "GET",
  POST = "POST",
  PUT = "PUT",
  DELETE = "DELETE",
}

// Alternative: union of string literals (often preferred)
type Direction2 = "Up" | "Down" | "Left" | "Right";
```

---

### 8. What is the difference between `any`, `unknown`, and `never`?

| Type      | Description                                   | Type-safe? | Use case                            |
| --------- | --------------------------------------------- | ---------- | ----------------------------------- |
| `any`     | Opts out of type checking entirely            | ❌ No      | Legacy code, escape hatch           |
| `unknown` | Like `any` but forces you to check before use | ✅ Yes     | External data, API responses        |
| `never`   | Represents values that never occur            | ✅ Yes     | Exhaustive checks, unreachable code |

Example:

```typescript
// any — unsafe, no checks required
let a: any = "hello";
a.toFixed(2); // No error, but crashes at runtime

// unknown — safe, must narrow before use
let u: unknown = "hello";
// u.toUpperCase(); // Error: Object is of type 'unknown'
if (typeof u === "string") {
  u.toUpperCase(); // ✅ safe after narrowing
}

// never — exhaustive union check
type Shape = "circle" | "square" | "triangle";

function getArea(shape: Shape): number {
  switch (shape) {
    case "circle":
      return Math.PI * 5 ** 2;
    case "square":
      return 5 * 5;
    case "triangle":
      return 0.5 * 5 * 5;
    default:
      // If a new shape is added but not handled, TypeScript errors here
      const _exhaustive: never = shape;
      throw new Error(`Unhandled shape: ${_exhaustive}`);
  }
}
```

---

### 9. What are utility types in TypeScript?

Utility types are built-in generic types that transform existing types into new ones. They cover the most common type manipulation patterns so you don't have to write them from scratch.

Common utility types:

| Utility          | Description                     |
| ---------------- | ------------------------------- |
| `Partial<T>`     | All properties optional         |
| `Required<T>`    | All properties required         |
| `Readonly<T>`    | All properties readonly         |
| `Pick<T, K>`     | Keep only keys K                |
| `Omit<T, K>`     | Remove keys K                   |
| `Record<K, V>`   | Object with keys K and values V |
| `Exclude<T, U>`  | Remove U from union T           |
| `Extract<T, U>`  | Keep only U from union T        |
| `NonNullable<T>` | Remove null and undefined       |
| `ReturnType<T>`  | Return type of a function       |
| `Parameters<T>`  | Parameter types of a function   |
| `Awaited<T>`     | Unwrap Promise type             |

Example:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  role: "admin" | "user";
}

type PartialUser = Partial<User>; // all optional
type RequiredUser = Required<User>; // all required
type ReadonlyUser = Readonly<User>; // all readonly
type UserPreview = Pick<User, "id" | "name">;
type UserWithoutRole = Omit<User, "role">;
type UserMap = Record<string, User>;
```

---

### 10. What is the difference between `Partial`, `Required`, and `Readonly`?

- `Partial<T>`: Makes **all properties optional** (`?`). Useful for update/patch payloads.
- `Required<T>`: Makes **all properties required** (removes `?`). Opposite of Partial.
- `Readonly<T>`: Makes **all properties readonly**. Prevents reassignment after creation.

Example:

```typescript
interface Config {
  host: string;
  port?: number;
  debug?: boolean;
}

// Partial — all optional (useful for PATCH requests)
type PatchConfig = Partial<Config>;
const patch: PatchConfig = { host: "localhost" }; // port and debug omitted ✅

// Required — all required (no optional properties)
type FullConfig = Required<Config>;
const full: FullConfig = { host: "localhost", port: 3000, debug: false }; // all required ✅

// Readonly — no reassignment allowed
const config: Readonly<Config> = { host: "localhost", port: 3000 };
// config.host = "other"; // Error: Cannot assign to 'host' because it is a read-only property

// ReadonlyArray
const nums: ReadonlyArray<number> = [1, 2, 3];
// nums.push(4); // Error: Property 'push' does not exist on type 'readonly number[]'
// nums[0] = 99; // Error: Index signature in type 'readonly number[]' only permits reading
```

---

### 11. What are `Pick`, `Omit`, `Record`, `Exclude`, and `Extract`?

These are the most commonly used structural utility types for reshaping and filtering types.

Example:

```typescript
type Person = {
  name: string;
  age: number;
  location: string;
  email: string;
};

// Pick — keep only specified keys
type PersonPreview = Pick<Person, "name" | "age">;
// { name: string; age: number }

// Omit — remove specified keys
type PersonWithoutLocation = Omit<Person, "location">;
// { name: string; age: number; email: string }

// Record — create object type with given keys and value type
type RoleMap = Record<"admin" | "user" | "guest", Person[]>;
// { admin: Person[]; user: Person[]; guest: Person[] }

// Exclude — remove members from a union
type T1 = Exclude<"a" | "b" | "c" | "d", "a" | "c">; // "b" | "d"
type T2 = Exclude<string | number | (() => void), Function>; // string | number

// Extract — keep only matching members from a union
type T3 = Extract<"a" | "b" | "c" | "d", "a" | "c" | "f">; // "a" | "c"
type T4 = Extract<string | number | (() => void), Function>; // () => void
```

---

### 12. What are mapped types in TypeScript?

Mapped types iterate over the keys of an existing type and transform each property. They use the `in keyof` syntax and are the foundation of most utility types.

Example:

```typescript
// Manual Partial implementation
type MyPartial<T> = {
  [K in keyof T]?: T[K];
};

// Manual Readonly implementation
type MyReadonly<T> = {
  readonly [K in keyof T]: T[K];
};

// Nullable — make all properties nullable
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};

// Getters — transform all properties to getter functions
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

interface User {
  name: string;
  age: number;
}
type UserGetters = Getters<User>;
// { getName: () => string; getAge: () => number }

// Filter keys by value type
type StringKeys<T> = {
  [K in keyof T]: T[K] extends string ? K : never;
}[keyof T];

interface Mixed {
  id: number;
  name: string;
  active: boolean;
  label: string;
}
type OnlyStringKeys = StringKeys<Mixed>; // "name" | "label"
```

---

### 13. What are conditional types in TypeScript?

Conditional types choose between two types based on a condition, using the `T extends U ? X : Y` syntax. They enable powerful type-level logic and are the basis for many built-in utility types.

Example:

```typescript
// Basic conditional type
type IsString<T> = T extends string ? true : false;
type A = IsString<string>; // true
type B = IsString<number>; // false

// Flatten arrays
type Flatten<T> = T extends Array<infer Item> ? Item : T;
type Str = Flatten<string[]>; // string
type Num = Flatten<number>; // number

// Distributive conditional types (distributes over unions)
type ToArray<T> = T extends any ? T[] : never;
type StrOrNumArr = ToArray<string | number>; // string[] | number[]

// Non-distributive (wrap in tuple to prevent distribution)
type ToArrayNonDist<T> = [T] extends [any] ? T[] : never;
type Combined = ToArrayNonDist<string | number>; // (string | number)[]

// Nested conditional types
type Unpacked<T> = T extends (infer U)[]
  ? U
  : T extends (...args: any[]) => infer U
    ? U
    : T extends Promise<infer U>
      ? U
      : T;

type T0 = Unpacked<string>; // string
type T1 = Unpacked<string[]>; // string
type T2 = Unpacked<() => string>; // string
type T3 = Unpacked<Promise<string>>; // string
```

---

### 14. What is the `infer` keyword in TypeScript?

`infer` is used inside conditional types to **declare a type variable** that TypeScript infers from the matched type. It lets you extract parts of a type without knowing them in advance.

Example:

```typescript
// Extract return type of any function
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

type R1 = ReturnType<() => string>; // string
type R2 = ReturnType<(x: number) => boolean>; // boolean

// Extract first argument type
type FirstArg<T> = T extends (first: infer F, ...rest: any[]) => any
  ? F
  : never;
type F1 = FirstArg<(x: string, y: number) => void>; // string

// Extract Promise value
type Awaited<T> = T extends Promise<infer U> ? Awaited<U> : T;
type P1 = Awaited<Promise<string>>; // string
type P2 = Awaited<Promise<Promise<number>>>; // number

// Template string inference
type SomeNum = "100" extends `${infer U extends number}` ? U : never; // 100

// Extract array element type
type ElementType<T> = T extends (infer E)[] ? E : never;
type E1 = ElementType<string[]>; // string
type E2 = ElementType<number[]>; // number
```

---

### 15. What are template literal types?

Template literal types build new string types by combining string literals with type interpolation — the same syntax as JavaScript template literals but at the type level.

Example:

```typescript
// Basic template literal type
type Greeting = `Hello, ${string}`;
const g1: Greeting = "Hello, Ahmed"; // ✅
const g2: Greeting = "Hi, Ahmed"; // ❌ Error

// Combine with unions — generates all combinations
type Direction = "top" | "bottom" | "left" | "right";
type CSSProperty = `margin-${Direction}` | `padding-${Direction}`;
// "margin-top" | "margin-bottom" | ... | "padding-right"

// Event name pattern
type EventName<T extends string> = `on${Capitalize<T>}`;
type ClickEvent = EventName<"click">; // "onClick"
type FocusEvent = EventName<"focus">; // "onFocus"

// Getter/setter generation
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};
type Setters<T> = {
  [K in keyof T as `set${Capitalize<string & K>}`]: (val: T[K]) => void;
};

interface User {
  name: string;
  age: number;
}
type UserGetters = Getters<User>;
// { getName: () => string; getAge: () => number }
```

---

### 16. What is the `keyof` operator?

`keyof` produces a **union of all keys** of a given type as string (or number) literals. It's essential for writing generic functions that work with object properties safely.

Example:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type UserKeys = keyof User; // "id" | "name" | "email"

// Generic property accessor — fully type-safe
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user: User = { id: 1, name: "Ahmed", email: "ahmed@example.com" };
const name = getProperty(user, "name"); // string ✅
const id = getProperty(user, "id"); // number ✅
// getProperty(user, "phone"); // Error: not a key of User ❌

// keyof with typeof
const config = { host: "localhost", port: 3000, debug: true };
type ConfigKey = keyof typeof config; // "host" | "port" | "debug"

// Mapped type using keyof
type Optional<T> = { [K in keyof T]?: T[K] };
```

---

### 17. What is the `typeof` operator in TypeScript?

TypeScript's `typeof` has two uses: the JavaScript runtime check (`typeof x === "string"`) for narrowing, and the **type-level** `typeof` for extracting the type of a variable or expression without declaring it manually.

Example:

```typescript
// Runtime typeof — for narrowing
function format(value: string | number) {
  if (typeof value === "string") {
    return value.toUpperCase();
  }
  return value.toFixed(2);
}

// Type-level typeof — extract type from a value
const defaultConfig = {
  host: "localhost",
  port: 3000,
  debug: false,
};

type Config = typeof defaultConfig;
// { host: string; port: number; debug: boolean }

// Use with functions
function createUser(name: string, age: number) {
  return { id: Math.random(), name, age };
}

type User = ReturnType<typeof createUser>;
// { id: number; name: string; age: number }

// Use with enums
enum Direction {
  Up,
  Down,
  Left,
  Right,
}
type DirectionType = typeof Direction;
// { Up: number; Down: number; Left: number; Right: number }
```

---

### 18. What is the `satisfies` operator?

The `satisfies` operator (TypeScript 4.9+) validates that an expression **matches a type** without widening the inferred type. It gives you both type safety and precise inference — the best of both worlds.

Example:

```typescript
type Color = "red" | "green" | "blue";
type Palette = Record<string, Color | [number, number, number]>;

// Without satisfies — type is widened to Palette
const palette1: Palette = {
  red: [255, 0, 0],
  green: "green",
};
// palette1.red is Color | [number, number, number] — lost precision

// With satisfies — validated against Palette but keeps precise types
const palette2 = {
  red: [255, 0, 0],
  green: "green",
  blue: [0, 0, 255],
} satisfies Palette;

palette2.red[0]; // ✅ TypeScript knows it's [number, number, number]
palette2.green.toUpperCase(); // ✅ TypeScript knows it's string

// Config example
interface CompilerOptions {
  strict?: boolean;
  outDir?: string;
}
interface Config {
  compilerOptions?: CompilerOptions;
  extends?: string | string[];
}

const myConfig = {
  compilerOptions: { strict: true, outDir: "../lib" },
  extends: ["@tsconfig/strictest/tsconfig.json"],
} satisfies Config;

// myConfig.extends is string[] — not string | string[]
myConfig.extends.map((p) => p.toUpperCase()); // ✅
```

---

### 19. What are decorators in TypeScript?

Decorators are special declarations that can be attached to classes, methods, properties, or parameters. They use the `@expression` syntax and are a stage-3 JavaScript proposal, enabled with `experimentalDecorators` in older TS or natively in TS 5+.

Example:

```typescript
// Class decorator — adds metadata or modifies the class
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class BugReport {
  type = "report";
  title: string;
  constructor(t: string) {
    this.title = t;
  }
}

// Method decorator — logs execution time
function log(target: any, key: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${key} with`, args);
    const result = original.apply(this, args);
    console.log(`${key} returned`, result);
    return result;
  };
  return descriptor;
}

class Calculator {
  @log
  add(a: number, b: number): number {
    return a + b;
  }
}

// Property decorator
function readonly(target: any, key: string) {
  Object.defineProperty(target, key, { writable: false });
}

class Config {
  @readonly
  version = "1.0.0";
}
```

---

### 20. What are access modifiers in TypeScript classes?

TypeScript adds `public`, `private`, `protected`, and `readonly` access modifiers to class members, controlling visibility and mutability.

| Modifier           | Accessible in class | Subclass | Outside |
| ------------------ | ------------------- | -------- | ------- |
| `public` (default) | ✅                  | ✅       | ✅      |
| `protected`        | ✅                  | ✅       | ❌      |
| `private`          | ✅                  | ❌       | ❌      |
| `readonly`         | ✅ (read only)      | ✅       | ✅      |

Example:

```typescript
class BankAccount {
  public owner: string;
  protected balance: number;
  private readonly accountNumber: string;

  constructor(owner: string, balance: number) {
    this.owner = owner;
    this.balance = balance;
    this.accountNumber = Math.random().toString(36).slice(2);
  }

  public getBalance(): number {
    return this.balance;
  }

  protected updateBalance(amount: number): void {
    this.balance += amount;
  }
}

class SavingsAccount extends BankAccount {
  deposit(amount: number): void {
    this.updateBalance(amount); // ✅ protected — accessible in subclass
    // this.accountNumber;       // ❌ private — not accessible
  }
}

const account = new BankAccount("Ahmed", 1000);
account.owner; // ✅ public
account.getBalance(); // ✅ public method
// account.balance;  // ❌ protected
```

---

### 21. What is the difference between `interface` and `abstract class`?

- `interface`: A pure type contract — no implementation, no runtime existence. A class can implement multiple interfaces.
- `abstract class`: Can have both abstract (unimplemented) and concrete (implemented) members. Has runtime existence. A class can only extend one abstract class.

Example:

```typescript
// Interface — pure contract, no implementation
interface Serializable {
  serialize(): string;
  deserialize(data: string): void;
}

interface Loggable {
  log(): void;
}

// A class can implement multiple interfaces
class UserService implements Serializable, Loggable {
  serialize(): string {
    return JSON.stringify(this);
  }
  deserialize(data: string): void {
    Object.assign(this, JSON.parse(data));
  }
  log(): void {
    console.log(this);
  }
}

// Abstract class — partial implementation
abstract class Animal {
  abstract makeSound(): void; // must be implemented by subclass

  move(distance: number = 0): void {
    // concrete — shared implementation
    console.log(`Moving ${distance}m`);
  }
}

class Dog extends Animal {
  makeSound(): void {
    console.log("Woof!");
  }
}

// new Animal(); // Error: Cannot create an instance of an abstract class
const dog = new Dog();
dog.makeSound(); // "Woof!"
dog.move(10); // "Moving 10m"
```

---

### 22. What is declaration merging in TypeScript?

Declaration merging allows TypeScript to combine multiple declarations with the same name into a single definition. It works with interfaces, namespaces, and enums — but not type aliases.

Example:

```typescript
// Interface merging — both declarations are combined
interface User {
  name: string;
  age: number;
}

interface User {
  email: string;
  role: "admin" | "user";
}

// Result: User has all four properties
const user: User = {
  name: "Ahmed",
  age: 30,
  email: "ahmed@example.com",
  role: "admin",
};

// Module augmentation — extend third-party types
// Extend Express Request to add a user property
declare module "express" {
  interface Request {
    user?: { id: number; name: string };
  }
}

// Extend Window with a custom property
interface Window {
  analytics: { track: (event: string) => void };
}
```

---

### 23. What are index signatures in TypeScript?

Index signatures define the type of values when an object is accessed with a dynamic key. They allow objects to have any number of properties of a specified type.

Example:

```typescript
// Basic index signature
interface StringMap {
  [key: string]: string;
}

const headers: StringMap = {
  "Content-Type": "application/json",
  Authorization: "Bearer token123",
};

// Mixed: known properties + index signature
// Known properties must match the index signature type
interface Config {
  debug: boolean;
  [key: string]: boolean | string; // must be compatible with debug: boolean
}

// Record<K, V> is often cleaner than index signatures
type UserMap = Record<string, { name: string; age: number }>;

// Numeric index signature
interface NumberArray {
  [index: number]: string;
  length: number;
}

// Readonly index signature
interface ReadonlyStringMap {
  readonly [key: string]: string;
}

const map: ReadonlyStringMap = { key: "value" };
// map.key = "other"; // Error: Index signature only permits reading
```

---

### 24. What is a discriminated union?

A discriminated union (also called a tagged union) is a union of types that each have a **common literal property** (the discriminant). TypeScript uses this property to narrow the type in switch/if statements.

Example:

```typescript
// Each shape has a 'kind' discriminant
interface Circle {
  kind: "circle";
  radius: number;
}
interface Square {
  kind: "square";
  side: number;
}
interface Triangle {
  kind: "triangle";
  base: number;
  height: number;
}

type Shape = Circle | Square | Triangle;

function getArea(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2; // shape is Circle here
    case "square":
      return shape.side ** 2; // shape is Square here
    case "triangle":
      return 0.5 * shape.base * shape.height; // shape is Triangle here
    default:
      const _exhaustive: never = shape; // compile error if case missed
      throw new Error(`Unknown shape: ${_exhaustive}`);
  }
}

// Real-world: API response states
type ApiState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: string };

function render<T>(state: ApiState<T>) {
  switch (state.status) {
    case "idle":
      return "Idle";
    case "loading":
      return "Loading...";
    case "success":
      return state.data; // T available here
    case "error":
      return state.error; // string available here
  }
}
```

---

### 25. What is the `readonly` modifier and `ReadonlyArray`?

`readonly` prevents a property from being reassigned after initialization. `ReadonlyArray<T>` (or `readonly T[]`) prevents mutation methods like `push`, `pop`, and `splice`.

Example:

```typescript
// readonly on interface properties
interface Point {
  readonly x: number;
  readonly y: number;
}

const p: Point = { x: 10, y: 20 };
// p.x = 5; // Error: Cannot assign to 'x' because it is a read-only property

// readonly on class members
class Config {
  readonly version: string = "1.0.0";
  readonly createdAt: Date;

  constructor() {
    this.createdAt = new Date(); // ✅ can assign in constructor
  }
}

// ReadonlyArray — no mutation methods
const nums: ReadonlyArray<number> = [1, 2, 3];
// nums.push(4);   // Error
// nums[0] = 99;   // Error
const newNums = [...nums, 4]; // ✅ create new array instead

// readonly tuple
const pair: readonly [string, number] = ["hello", 42];
// pair[0] = "world"; // Error

// Deep readonly with utility type
type DeepReadonly<T> = {
  readonly [K in keyof T]: T[K] extends object ? DeepReadonly<T[K]> : T[K];
};
```

---

### 26. What are function overloads in TypeScript?

Function overloads let you define multiple call signatures for a function, so TypeScript can infer the correct return type based on the argument types passed.

Example:

```typescript
// Overload signatures (no implementation)
function format(value: string): string;
function format(value: number): string;
function format(value: Date): string;

// Implementation signature (must be compatible with all overloads)
function format(value: string | number | Date): string {
  if (typeof value === "string") return value.trim();
  if (typeof value === "number") return value.toLocaleString();
  return value.toISOString();
}

const s = format("  hello  "); // string ✅
const n = format(1234567); // string ✅
const d = format(new Date()); // string ✅

// Method overloads in a class
class EventEmitter {
  on(event: "click", handler: (e: MouseEvent) => void): void;
  on(event: "keydown", handler: (e: KeyboardEvent) => void): void;
  on(event: string, handler: (e: Event) => void): void {
    document.addEventListener(event, handler as EventListener);
  }
}
```

---

### 27. What is the `as const` assertion?

`as const` tells TypeScript to infer the **narrowest possible type** for a value — treating all values as literal types and making all properties `readonly`. It's essential for creating type-safe constant objects and arrays.

Example:

```typescript
// Without as const — widened types
const config1 = { host: "localhost", port: 3000 };
// type: { host: string; port: number }

// With as const — literal types, all readonly
const config2 = { host: "localhost", port: 3000 } as const;
// type: { readonly host: "localhost"; readonly port: 3000 }

// Array as const — readonly tuple with literal types
const directions = ["up", "down", "left", "right"] as const;
// type: readonly ["up", "down", "left", "right"]

type Direction = (typeof directions)[number]; // "up" | "down" | "left" | "right"

// Enum-like pattern with as const
const HttpStatus = {
  OK: 200,
  NOT_FOUND: 404,
  ERROR: 500,
} as const;

type HttpStatusCode = (typeof HttpStatus)[keyof typeof HttpStatus]; // 200 | 404 | 500

// Route definitions
const ROUTES = {
  HOME: "/",
  ABOUT: "/about",
  PROFILE: "/profile/:id",
} as const;

type Route = (typeof ROUTES)[keyof typeof ROUTES];
```

---

### 28. What is the `NonNullable` utility type?

`NonNullable<T>` removes `null` and `undefined` from a type. It's useful when you've already checked that a value exists and want TypeScript to reflect that.

Example:

```typescript
// Basic usage
type MaybeString = string | null | undefined;
type DefiniteString = NonNullable<MaybeString>; // string

type T1 = NonNullable<string | number | undefined>; // string | number
type T2 = NonNullable<null | undefined>; // never

// Practical: filter nullable values from an array
const values = ["hello", null, "world", undefined, "!"];
const defined = values.filter((v): v is NonNullable<typeof v> => v != null);
// defined: string[]

// With mapped types — remove null from all properties
type RequiredProps<T> = {
  [K in keyof T]: NonNullable<T[K]>;
};

interface FormData {
  name: string | null;
  email: string | undefined;
  age: number | null;
}

type CleanFormData = RequiredProps<FormData>;
// { name: string; email: string; age: number }
```

---

### 29. What are `ReturnType` and `Parameters` utility types?

- `ReturnType<T>`: Extracts the **return type** of a function type.
- `Parameters<T>`: Extracts the **parameter types** of a function as a tuple.

Example:

```typescript
function createUser(name: string, age: number, role: "admin" | "user") {
  return { id: Math.random(), name, age, role, createdAt: new Date() };
}

// ReturnType — extract what the function returns
type User = ReturnType<typeof createUser>;
// { id: number; name: string; age: number; role: "admin" | "user"; createdAt: Date }

// Parameters — extract the argument types as a tuple
type CreateUserArgs = Parameters<typeof createUser>;
// [name: string, age: number, role: "admin" | "user"]

// Access individual parameter types
type NameParam = Parameters<typeof createUser>[0]; // string
type RoleParam = Parameters<typeof createUser>[2]; // "admin" | "user"

// Practical: wrap a function with the same signature
function withLogging<T extends (...args: any[]) => any>(fn: T) {
  return function (...args: Parameters<T>): ReturnType<T> {
    console.log("Calling with:", args);
    const result = fn(...args);
    console.log("Result:", result);
    return result;
  };
}

const loggedCreateUser = withLogging(createUser);
loggedCreateUser("Ahmed", 30, "admin"); // fully typed ✅
```

---

### 30. What is module augmentation in TypeScript?

Module augmentation lets you **add new declarations to an existing module** without modifying its source. It's the standard way to extend third-party library types.

Example:

```typescript
// Extend Express Request type
import "express";

declare module "express" {
  interface Request {
    user?: {
      id: number;
      name: string;
      role: "admin" | "user";
    };
  }
}

// Now req.user is typed everywhere
import { Request, Response } from "express";

function authMiddleware(req: Request, res: Response, next: Function) {
  req.user = { id: 1, name: "Ahmed", role: "admin" }; // ✅ typed
  next();
}

// Extend a third-party library
declare module "some-library" {
  interface SomeInterface {
    newMethod(): void;
  }
}

// Extend global types
declare global {
  interface Window {
    analytics: {
      track(event: string, props?: Record<string, unknown>): void;
    };
  }
  interface Array<T> {
    last(): T | undefined;
  }
}
```

---

### 31. What is the difference between `strict` mode flags?

The `strict` flag in `tsconfig.json` enables a group of strictness checks. You can enable them all at once with `"strict": true` or individually.

| Flag                           | What it checks                                           |
| ------------------------------ | -------------------------------------------------------- |
| `strictNullChecks`             | `null` and `undefined` are not assignable to other types |
| `strictFunctionTypes`          | Stricter checking of function parameter types            |
| `strictBindCallApply`          | Correct types for `bind`, `call`, `apply`                |
| `strictPropertyInitialization` | Class properties must be initialized in constructor      |
| `noImplicitAny`                | Error on implicit `any` type                             |
| `noImplicitThis`               | Error on `this` with implicit `any` type                 |
| `alwaysStrict`                 | Emits `"use strict"` in all files                        |

Example:

```json
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,

    // Additional recommended flags
    "noUncheckedIndexedAccess": true,
    "noImplicitOverride": true,
    "exactOptionalPropertyTypes": true,
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

```typescript
// strictNullChecks
let name: string = null; // Error with strict — null not assignable to string
let name2: string | null = null; // ✅ explicit null

// noImplicitAny
function greet(name) {
  // Error: Parameter 'name' implicitly has an 'any' type
  return `Hello, ${name}`;
}
function greet2(name: string) {
  return `Hello, ${name}`;
} // ✅

// strictPropertyInitialization
class User {
  name: string; // Error: not initialized
  name2: string = ""; // ✅
  name3!: string; // ✅ definite assignment assertion
}
```

---

### 32. What is structural typing in TypeScript?

TypeScript uses **structural typing** (also called duck typing) — a type is compatible with another if it has all the required properties, regardless of what it's called. This is different from nominal typing (Java, C#) where types must explicitly declare compatibility.

Example:

```typescript
interface Point2D {
  x: number;
  y: number;
}
interface Point3D {
  x: number;
  y: number;
  z: number;
}

// Point3D is structurally compatible with Point2D (has all required props)
function printPoint(p: Point2D) {
  console.log(`(${p.x}, ${p.y})`);
}

const p3d: Point3D = { x: 1, y: 2, z: 3 };
printPoint(p3d); // ✅ works — Point3D has x and y

// Classes are also structurally typed
class Dog {
  name: string = "Rex";
  bark() {}
}
class Cat {
  name: string = "Whiskers";
  bark() {}
}

const animals: Dog[] = [];
animals.push(new Cat()); // ✅ Cat is structurally compatible with Dog

// Excess property checks — only on object literals
interface Config {
  host: string;
  port: number;
}

const c1: Config = { host: "localhost", port: 3000, debug: true };
// Error: Object literal may only specify known properties

const obj = { host: "localhost", port: 3000, debug: true };
const c2: Config = obj; // ✅ no error — not a fresh object literal
```

---

### 33. What are namespace and module in TypeScript?

- **Module**: Any file with a top-level `import` or `export`. The modern standard for code organization. Each module has its own scope.
- **Namespace**: TypeScript's older way to organize code using the `namespace` keyword. Useful for grouping related types in declaration files (`.d.ts`).

Example:

```typescript
// Module (modern — preferred)
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}
export function subtract(a: number, b: number): number {
  return a - b;
}

// main.ts
import { add, subtract } from "./math";
console.log(add(2, 3)); // 5

// Namespace (legacy — mainly for .d.ts files)
namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }

  export class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string): boolean {
      return /^[A-Za-z]+$/.test(s);
    }
  }
}

const validator = new Validation.LettersOnlyValidator();
validator.isAcceptable("Hello"); // true

// Namespace in declaration files (common for global libraries)
declare namespace MyLib {
  function create(s: string): MyLib.Widget;
  interface Widget {
    name: string;
  }
}
```

---

### 34. What is the `Awaited` utility type?

`Awaited<T>` recursively unwraps `Promise` types to get the resolved value type. It's the built-in replacement for manually writing `T extends Promise<infer U> ? U : T`.

Example:

```typescript
// Basic usage
type A = Awaited<Promise<string>>; // string
type B = Awaited<Promise<Promise<number>>>; // number (recursive unwrap)
type C = Awaited<string>; // string (non-promise passthrough)

// Practical: type the result of async functions
async function fetchUser(id: number) {
  const res = await fetch(`/api/users/${id}`);
  return res.json() as Promise<{ id: number; name: string; email: string }>;
}

type FetchUserResult = Awaited<ReturnType<typeof fetchUser>>;
// { id: number; name: string; email: string }

// With Promise.all
async function fetchAll() {
  return Promise.all([
    fetch("/api/users").then((r) => r.json() as Promise<User[]>),
    fetch("/api/posts").then((r) => r.json() as Promise<Post[]>),
  ]);
}

type FetchAllResult = Awaited<ReturnType<typeof fetchAll>>;
// [User[], Post[]]
```

---

### 35. What are tuple types in TypeScript?

Tuples are fixed-length arrays where each position has a specific type. They're useful for representing structured data like coordinates, key-value pairs, or function return values.

Example:

```typescript
// Basic tuple
type Point = [number, number];
const p: Point = [10, 20];
const [x, y] = p; // destructuring

// Named tuple elements (TS 4.0+)
type Range = [start: number, end: number];
type RGB = [red: number, green: number, blue: number];

// Optional tuple elements
type Config = [string, number?];
const c1: Config = ["localhost"]; // ✅
const c2: Config = ["localhost", 3000]; // ✅

// Rest elements in tuples
type StringsAndNumber = [...string[], number];
const t1: StringsAndNumber = ["a", "b", 42]; // ✅

// Readonly tuple
const pair: readonly [string, number] = ["hello", 42];

// Practical: useState-like return type
function useState<T>(initial: T): [T, (value: T) => void] {
  let state = initial;
  const setState = (value: T) => {
    state = value;
  };
  return [state, setState];
}

const [count, setCount] = useState(0);
setCount(1); // ✅ typed
```

---

### 36. What is the `noUncheckedIndexedAccess` compiler option?

When enabled, TypeScript adds `undefined` to the type of any value accessed via an index signature or array index. This prevents runtime errors from accessing out-of-bounds array elements or missing object keys.

Example:

```typescript
// tsconfig.json: "noUncheckedIndexedAccess": true

// Array access — now includes undefined
const arr = [1, 2, 3];
const first = arr[0]; // number | undefined (not just number)

if (first !== undefined) {
  console.log(first.toFixed(2)); // ✅ safe
}

// Index signature access — now includes undefined
const map: Record<string, string> = { key: "value" };
const val = map["key"]; // string | undefined

if (val !== undefined) {
  console.log(val.toUpperCase()); // ✅ safe
}

// Without the flag — silent runtime bug
const users = ["Ahmed", "Sara"];
const user = users[99]; // string (but actually undefined at runtime)
console.log(user.toUpperCase()); // 💥 TypeError at runtime

// With the flag — caught at compile time
const user2 = users[99]; // string | undefined
// console.log(user2.toUpperCase()); // Error: Object is possibly 'undefined'
```

---

### 37. What is the difference between `extends` and `implements`?

- `extends`: Used for **inheritance** — a class inherits all properties and methods from a parent class or interface. An interface can also extend another interface.
- `implements`: Used for **type checking** — a class declares it satisfies an interface's contract. It does not inherit any implementation.

Example:

```typescript
// extends — inherits implementation
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  move(distance: number): void {
    console.log(`${this.name} moved ${distance}m`);
  }
}

class Dog extends Animal {
  bark(): void {
    console.log("Woof!");
  }
}

const dog = new Dog("Rex");
dog.move(10); // ✅ inherited from Animal
dog.bark(); // ✅ own method

// implements — satisfies a contract, no inheritance
interface Flyable {
  fly(): void;
}
interface Swimmable {
  swim(): void;
}

class Duck implements Flyable, Swimmable {
  fly(): void {
    console.log("Flying!");
  }
  swim(): void {
    console.log("Swimming!");
  }
}

// Interface extends interface
interface Shape {
  color: string;
}
interface Square extends Shape {
  side: number;
}

const sq: Square = { color: "red", side: 5 }; // must have both
```

---

### 38. What are optional and default parameters in TypeScript?

- **Optional parameters** (`?`): May be omitted when calling the function. Their type is automatically `T | undefined`.
- **Default parameters**: Provide a fallback value when the argument is omitted or `undefined`. TypeScript infers the type from the default.

Example:

```typescript
// Optional parameter
function greet(name: string, greeting?: string): string {
  return `${greeting ?? "Hello"}, ${name}!`;
}

greet("Ahmed"); // "Hello, Ahmed!"
greet("Ahmed", "Hi"); // "Hi, Ahmed!"

// Default parameter
function createUser(
  name: string,
  role: "admin" | "user" = "user",
  active: boolean = true,
) {
  return { name, role, active };
}

createUser("Ahmed"); // { name: "Ahmed", role: "user", active: true }
createUser("Sara", "admin"); // { name: "Sara", role: "admin", active: true }
createUser("Omar", "user", false); // { name: "Omar", role: "user", active: false }

// Optional vs default — key difference
function log(message: string, level?: string) {
  // level is string | undefined — must handle undefined
  console.log(`[${level ?? "INFO"}] ${message}`);
}

function log2(message: string, level: string = "INFO") {
  // level is always string — default handles undefined
  console.log(`[${level}] ${message}`);
}
```

---

### 39. What is the `using` keyword in TypeScript 5?

The `using` keyword (TypeScript 5.2+) implements the **Explicit Resource Management** proposal. It automatically calls `[Symbol.dispose]()` on a resource when it goes out of scope — like a `finally` block but cleaner.

Example:

```typescript
// Define a disposable resource
class DatabaseConnection {
  constructor(private url: string) {
    console.log(`Connected to ${url}`);
  }

  query(sql: string) {
    console.log(`Executing: ${sql}`);
    return [];
  }

  [Symbol.dispose]() {
    console.log(`Connection to ${this.url} closed`);
  }
}

// using — automatically disposed at end of scope
function runQuery() {
  using db = new DatabaseConnection("postgres://localhost/mydb");
  // Connected to postgres://localhost/mydb

  const results = db.query("SELECT * FROM users");
  return results;
  // Connection to postgres://localhost/mydb closed ← automatic
}

// await using — for async disposal
class FileHandle {
  async [Symbol.asyncDispose]() {
    await this.close();
  }
  async close() {
    console.log("File closed");
  }
}

async function readFile() {
  await using file = new FileHandle();
  // ... read file
  // file.close() called automatically
}
```

---

### 40. What are some TypeScript best practices for large codebases?

Example:

```typescript
// ✅ 1. Enable strict mode in tsconfig.json
// { "compilerOptions": { "strict": true, "noUncheckedIndexedAccess": true } }

// ✅ 2. Prefer interfaces for object shapes, types for unions/tuples
interface User {
  id: number;
  name: string;
}
type Status = "active" | "inactive" | "pending";

// ✅ 3. Use unknown instead of any for external data
async function fetchData(url: string): Promise<unknown> {
  const res = await fetch(url);
  return res.json();
}

// ✅ 4. Use discriminated unions for state management
type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: string };

// ✅ 5. Use const assertions for immutable config
const ROUTES = { HOME: "/", ABOUT: "/about" } as const;
type Route = (typeof ROUTES)[keyof typeof ROUTES];

// ✅ 6. Use utility types instead of duplicating types
interface CreateUserDto {
  name: string;
  email: string;
  role: "admin" | "user";
}
type UpdateUserDto = Partial<CreateUserDto>;
type UserPreview = Pick<CreateUserDto, "name" | "email">;

// ✅ 7. Use satisfies for config objects
const config = {
  port: 3000,
  host: "localhost",
} satisfies Record<string, string | number>;

// ✅ 8. Avoid type assertions (as) — use type guards instead
// ❌ const user = data as User;
// ✅ if (isUser(data)) { /* use data as User */ }

// ✅ 9. Use ReturnType and Parameters to stay DRY
function buildQuery(table: string, limit: number) {
  /* ... */
}
type QueryArgs = Parameters<typeof buildQuery>;
type QueryResult = ReturnType<typeof buildQuery>;

// ✅ 10. Use template literal types for string patterns
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";
type ApiRoute = `/api/${string}`;
type Endpoint = `${HttpMethod} ${ApiRoute}`;
```

---

> Made with ❤️ — Click ⭐ if this helped you prepare for your next interview!
