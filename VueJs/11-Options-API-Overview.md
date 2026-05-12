# Options API Overview

The **Options API** organizes component logic by **option type**: **`data`**, **`computed`**, **`methods`**, **lifecycle hooks**. Vue 3 still supports it fully; many codebases **mix** Options and Composition during migration.

---

## Shape

```ts
export default {
  data() {
    return { count: 0 };
  },
  computed: {
    double() {
      return this.count * 2;
    },
  },
  methods: {
    inc() {
      this.count++;
    },
  },
  mounted() {
    console.log(this.count);
  },
};
```

**`this`** is the component **proxy**; typing **`this`** in TypeScript needs **`ComponentCustomProperties`** augmentation for globals.

---

## When Options API is still reasonable

- Small presentational components.
- Team familiarity / incremental migration.
- Some **Vue 2** patterns map 1:1.

---

## Mixins vs composables

**Mixins**:

- Implicit merge order; **naming collisions** are painful.
- Harder to trace **where** a method came from.

**Composables** (functions using Vue APIs):

- Explicit imports: **`const { x } = useThing()`**.
- Preferred for **Vue 3** greenfield.

---

## `extends` and higher-order components

Know they exist; prefer **composition** for reuse in new code.

---

## Related

Next: [Lifecycle](12-Lifecycle-Hooks.md).
