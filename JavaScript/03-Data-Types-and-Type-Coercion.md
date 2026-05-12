# Data Types and Type Coercion

JavaScript has **dynamic types**: the same variable can hold different types over time (discouraged in practice). Understanding **primitives vs objects** and **coercion** prevents subtle bugs.

---

## Primitives (7 kinds)

1. **`undefined`** — variable declared but not assigned; missing function return.
2. **`null`** — intentional absence (often API sentinel).
3. **`boolean`**
4. **`number`** — IEEE-754 double; **safe integers** up to `Number.MAX_SAFE_INTEGER`.
5. **`bigint`** — arbitrary precision integers (`123n`).
6. **`string`**
7. **`symbol`** — unique keys (metadata, protocols).

Everything else—including **arrays**, **functions**, **dates**—is an **object**.

---

## `typeof` quirks

```js
typeof null === 'object'   // historical bug — use value === null
typeof [] === 'object'
typeof function(){} === 'function' // functions are callable objects
```

Use **`Array.isArray`** for arrays.

---

## Truthy and falsy

**Falsy** values: `undefined`, `null`, `false`, `0`, `-0`, `NaN`, `""`.

**Everything else is truthy** — including `'0'`, `[]`, `{}`.

```js
if (userInput) { ... } // empty string skips block — often correct for forms
```

---

## Coercion with `==` vs `===`

**`===`** checks **type and value** without coercion — **default choice**.

**`==`** applies coercion rules (e.g. `0 == false` is true). Rarely needed; if you use it, comment **why**.

---

## Number ↔ string

```js
Number('42')   // 42
Number('')     // 0 (surprise)
parseInt('08', 10) // always pass radix 10 for decimal
String(123)
```

---

## Practical advice

- Validate **API boundaries** with schemas (zod, JSON Schema) instead of hoping coercion is safe.
- Prefer **`===`**, **`Number()`**, explicit **`Boolean()`** at system edges.

---

## Next

- [Operators and equality](04-Operators-and-Equality.md)
