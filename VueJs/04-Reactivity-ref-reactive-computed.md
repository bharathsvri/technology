# Reactivity: `ref`, `reactive`, `computed`

## `ref`

Wrapper for primitives/objects; access `.value` in script; auto-unwrap in template.

```ts
const count = ref(0);
count.value++;
```

## `reactive`

Proxy-based deep reactivity for plain objects (no `.value`).

## `computed`

Derived getter (and optional setter) with caching.

## `readonly`

Expose immutable view of state.

Next: [Watchers](05-Watchers.md).
