# JavaScript Data Types & Type Coercion - Interview Questions

## Fundamentals → Advanced

---

1️⃣ **typeof null**

❓ **Question**

```javascript
typeof null;
```

✅ **Answer**
`"object"`

⚠️ **Trap**
This is a historical bug in JavaScript that cannot be fixed due to backward compatibility.

✔️ **Correct way to check null**

```javascript
value === null;
```

---

2️⃣ **Primitive Data Types**

❓ **Question**
List all 7 primitive data types in JavaScript.

✅ **Answer**

1. `string`
2. `number`
3. `boolean`
4. `undefined`
5. `null`
6. `symbol`
7. `bigint`

⚠️ **Trap**
Candidates often forget `symbol` and `bigint` (ES6+).

---

3️⃣ **typeof undefined vs undeclared**

❓ **Question**

```javascript
let a;
console.log(typeof a);
console.log(typeof b);
```

✅ **Answer**

```javascript
"undefined";
"undefined";
```

⚠️ **Trap**
`typeof` returns `"undefined"` for both declared-but-uninitialized and undeclared variables. It doesn't throw a ReferenceError.

✔️ **Correct way to check if variable exists**

```javascript
if (typeof b !== "undefined") {
  // safe to use b
}
```

---

4️⃣ **NaN type and equality**

❓ **Question**

```javascript
typeof NaN;
NaN === NaN;
```

✅ **Answer**

```javascript
"number";
false;
```

⚠️ **Trap**
`NaN` is the only value in JavaScript that is not equal to itself.

✔️ **Correct way to check NaN**

```javascript
Number.isNaN(value);
// or
Object.is(value, NaN);
```

---

5️⃣ **String to Number coercion**

❓ **Question**

```javascript
"5" - 3;
"5" + 3;
```

✅ **Answer**

```javascript
2;
("53");
```

⚠️ **Trap**
The `-` operator coerces to number, but `+` is overloaded for both addition and concatenation. String concatenation takes precedence.

✔️ **Correct way to convert**

```javascript
Number("5") +
  3 + // 8
  "5" +
  3; // 8
```

---

6️⃣ **Truthy and Falsy values**

❓ **Question**
List all falsy values in JavaScript.

✅ **Answer**

```javascript
false
0
-0
0n (BigInt zero)
"" (empty string)
null
undefined
NaN
```

⚠️ **Trap**
Everything else is truthy, including `"0"`, `"false"`, `[]`, `{}`, and `function(){}`.

---

7️⃣ **Double vs Triple equals**

❓ **Question**

```javascript
"5" == 5;
"5" === 5;
```

✅ **Answer**

```javascript
true;
false;
```

⚠️ **Trap**
`==` performs type coercion before comparison. `===` checks both type and value.

✔️ **Correct way**
Always use `===` unless you specifically need type coercion.

---

8️⃣ **Object to Primitive coercion**

❓ **Question**

```javascript
const obj = { value: 10 };
console.log(obj + 5);
```

✅ **Answer**

```javascript
"[object Object]5";
```

⚠️ **Trap**
Objects are converted to primitives via `toString()` then `valueOf()`. Default `toString()` returns `"[object Object]"`.

✔️ **Correct way to customize**

```javascript
const obj = {
  value: 10,
  valueOf() {
    return this.value;
  },
};
console.log(obj + 5); // 15
```

---

9️⃣ **Array coercion**

❓ **Question**

```javascript
[] + []
[] + {}
{} + []
```

✅ **Answer**

```javascript
"";
"[object Object]";
0; // or "[object Object]" depending on context
```

⚠️ **Trap**

- `[]` converts to `""` (empty string)
- `{}` at statement start is treated as a code block, not an object
- `{} + []` becomes `+[]` which is `0`

✔️ **Correct way**

```javascript
({}) + []; // "[object Object]"
```

---

🔟 **Implicit vs Explicit coercion**

❓ **Question**

```javascript
Boolean("false");
!!"false";
```

✅ **Answer**

```javascript
true;
true;
```

⚠️ **Trap**
The string `"false"` is truthy because it's a non-empty string.

✔️ **Correct way to parse boolean strings**

```javascript
const str = "false";
str === "true"; // false
```

---

1️⃣1️⃣ **parseInt gotcha**

❓ **Question**

```javascript
parseInt("08");
parseInt("08", 10)[("1", "2", "3")].map(parseInt);
```

✅ **Answer**

```javascript
8;
(8)[(1, NaN, NaN)];
```

⚠️ **Trap**
`parseInt` takes two arguments: `(string, radix)`. When used with `map`, it receives `(element, index, array)`. So it becomes:

- `parseInt("1", 0)` → 1 (radix 0 defaults to 10)
- `parseInt("2", 1)` → NaN (invalid radix)
- `parseInt("3", 2)` → NaN (3 is invalid in base 2)

✔️ **Correct way**

```javascript
["1", "2", "3"]
  .map(Number) // [1, 2, 3]
  [("1", "2", "3")].map((str) => parseInt(str, 10));
```

---

1️⃣2️⃣ **Loose equality edge cases**

❓ **Question**

```javascript
null == undefined;
null == 0;
undefined == 0;
```

✅ **Answer**

```javascript
true;
false;
false;
```

⚠️ **Trap**
`null` and `undefined` are only loosely equal to each other, not to any other value.

✔️ **Correct way**
Use `===` to avoid confusion.

---

1️⃣3️⃣ **Symbol uniqueness**

❓ **Question**

```javascript
Symbol("foo") === Symbol("foo");
Symbol.for("foo") === Symbol.for("foo");
```

✅ **Answer**

```javascript
false;
true;
```

⚠️ **Trap**
`Symbol()` always creates a unique symbol. `Symbol.for()` creates/retrieves from a global registry.

✔️ **Correct way**
Use `Symbol.for()` when you need to share symbols across different parts of code.

---

1️⃣4️⃣ **BigInt operations**

❓ **Question**

```javascript
10n + 5;
10n + 5n;
10n == 10;
10n === 10;
```

✅ **Answer**

```javascript
TypeError: Cannot mix BigInt and other types
15n
true
false
```

⚠️ **Trap**
BigInt cannot be mixed with regular numbers in arithmetic operations. Loose equality works, strict doesn't.

✔️ **Correct way**

```javascript
10n + BigInt(5); // 15n
Number(10n) + 5; // 15
```

---

1️⃣5️⃣ **String to Number edge cases**

❓ **Question**

```javascript
Number("");
Number(" ");
Number("123abc");
Number("   123   ");
```

✅ **Answer**

```javascript
0;
0;
NaN;
123;
```

⚠️ **Trap**
Empty strings and whitespace-only strings convert to `0`. Whitespace is trimmed before conversion.

✔️ **Correct way to validate**

```javascript
const num = Number(str);
if (!isNaN(num) && str.trim() !== "") {
  // valid number
}
```

---

1️⃣6️⃣ **Object.is() vs ===**

❓ **Question**

```javascript
Object.is(NaN, NaN);
NaN === NaN;

Object.is(+0, -0) + 0 === -0;
```

✅ **Answer**

```javascript
true;
false;

false;
true;
```

⚠️ **Trap**
`Object.is()` handles two special cases differently than `===`:

- `NaN` is equal to `NaN`
- `+0` is not equal to `-0`

✔️ **Correct way**
Use `Object.is()` when you need to distinguish these edge cases.

---

1️⃣7️⃣ **Unary plus operator**

❓ **Question**

```javascript
+"42" + true + false + null + undefined + [] + [1] + [1, 2];
```

✅ **Answer**

```javascript
42;
1;
0;
0;
NaN;
0;
1;
NaN;
```

⚠️ **Trap**
Unary `+` is the fastest way to convert to number, but behavior varies:

- Empty array `[]` → `""` → `0`
- Single-element array `[1]` → `"1"` → `1`
- Multi-element array → `"1,2"` → `NaN`

---

1️⃣8️⃣ **Template literal coercion**

❓ **Question**

```javascript
`${null}``${undefined}``${[1, 2, 3]}``${{ a: 1 }}`;
```

✅ **Answer**

```javascript
"null";
"undefined";
"1,2,3";
"[object Object]";
```

⚠️ **Trap**
Template literals always convert to string using `toString()` method.

✔️ **Correct way for objects**

```javascript
`${JSON.stringify({ a: 1 })}`; // "{"a":1}"
```

---

1️⃣9️⃣ **Comparison coercion**

❓ **Question**

```javascript
"10" > 9;
"10" > "9"[10] > 9;
```

✅ **Answer**

```javascript
true;
false;
true;
```

⚠️ **Trap**

- `"10" > 9`: String is coerced to number → `10 > 9`
- `"10" > "9"`: Both strings, lexicographic comparison → `"1" < "9"`
- `[10] > 9`: Array converted to primitive → `"10"` → `10 > 9`

✔️ **Correct way**
Always compare same types:

```javascript
Number("10") > 9; // true
"10" > "9"; // false (lexicographic)
```

---

2️⃣0️⃣ **valueOf vs toString priority**

❓ **Question**

```javascript
const obj = {
  valueOf() {
    return 10;
  },
  toString() {
    return "20";
  },
};

console.log(obj + 5);
console.log(String(obj));
```

✅ **Answer**

```javascript
15;
("20");
```

⚠️ **Trap**

- For numeric context: `valueOf()` is called first
- For string context: `toString()` is called first
- `+` operator prefers numeric conversion

✔️ **Correct way to understand**

```javascript
// Numeric context
Number(obj); // calls valueOf() → 10

// String context
String(obj); // calls toString() → "20"
```

---

## Summary

These questions cover:

- Primitive types and their quirks
- Type coercion rules (implicit & explicit)
- Equality comparisons (`==` vs `===` vs `Object.is()`)
- Edge cases with `NaN`, `null`, `undefined`
- Array and object coercion
- Modern types (`Symbol`, `BigInt`)
- Common interview traps

**Key Takeaway**: Always prefer explicit type conversion and strict equality (`===`) unless you have a specific reason to use coercion.
