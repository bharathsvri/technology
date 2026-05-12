# Private Fields, Static Blocks, and Class Syntax

Modern **class** syntax adds **true privacy**, **static initialization**, and ergonomic **instance fields**—distinct from old **`#`less** “privacy by convention.”

---

## Private instance fields

```js
class Counter {
  #value = 0;
  inc() { this.#value++; }
}
```

**Not** visible on `Object.keys` / JSON.stringify—**hard private** in modern engines.

---

## Private static

```js
class Cache {
  static #instances = new Map();
}
```

---

## Static initialization blocks

```js
class Config {
  static url;
  static {
    this.url = computeFromEnv();
  }
}
```

Runs **once** when class is evaluated—useful for **non-trivial** static setup beyond literals.

---

## Pitfalls

- **Private fields** are not **reified** for reflection (unlike some other languages’ metadata).
- **Subclassing** + private fields: visibility rules are lexical—design APIs carefully.

---

## Related

- [Prototypes and Classes](10-Prototypes-and-Classes.md)
