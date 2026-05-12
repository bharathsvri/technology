# Reactivity: `ref`, `reactive`, `computed`

Vue 3’s reactivity is built on **Proxies** (`reactive`) and **getter/setter wrappers** (`ref`). Interviewers often probe **when to use which**, **unwrapping**, and **losing reactivity** pitfalls.

---

## `ref`

Holds any value in a **`.value` cell**; internally can wrap primitives or object references.

```ts
import { ref } from 'vue';

const count = ref(0);
count.value++; // in <script>
```

**In templates**, `ref` is **auto-unwrapped** — write `{{ count }}`, not `count.value`.

**Why `ref` for primitives?** Proxies cannot wrap **string/number/boolean** as standalone reactive values; `ref` boxes them.

---

## `reactive`

Deep **Proxy** on plain objects/arrays/maps/sets.

```ts
import { reactive } from 'vue';

const state = reactive({ count: 0 });
state.count++;
```

No **`.value`** on the root object. **Destructuring loses reactivity** unless you use **`toRefs`** / **`storeToRefs`** (Pinia).

---

## `computed`

**Cached** derived value; tracks dependencies like a **getter**.

```ts
const doubled = computed(() => count.value * 2);

// Writable (rarer)
const fullName = computed({
  get: () => `${first.value} ${last.value}`,
  set: (v) => { /* parse v */ },
});
```

Re-runs only when dependencies change; good for **expensive** pure derivations.

---

## `readonly`

```ts
const copy = readonly(state);
```

Consumers can read but not mutate through the proxy — useful for **exposing store state** without allowing direct writes.

---

## Common pitfalls (high signal for interviews)

1. **Destructuring `reactive`** → plain values, not reactive. Fix: **`toRefs(reactiveObj)`**.
2. **`ref` in objects** — nesting `ref` inside `reactive` **auto-unwraps** one level (know the rules when mixing).
3. **Replacing entire `reactive` object** — assign to root fields, don’t replace the proxy reference from outside without care.

---

## Related

Next: [Watchers](05-Watchers.md).
