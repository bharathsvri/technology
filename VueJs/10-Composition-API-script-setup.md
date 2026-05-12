# Composition API with `<script setup>`

**`<script setup>`** is compile-time sugar over `setup()`: top-level bindings are **auto-exposed** to the template; **macros** generate boilerplate for props, emits, and exposes.

---

## Compiler macros (no import)

- **`defineProps`** — component inputs.
- **`defineEmits`** — event typings / runtime emit fn.
- **`defineExpose`** — choose what parent **`ref`** on child can access.
- **`defineOptions`** — name, `inheritAttrs`, etc. (build tooling / Vue version dependent).
- **`defineModel`** — two-way binding shortcut (Vue 3.4+).

These are **erased** at compile time — they are not real runtime functions.

---

## Top-level `await`

`<script setup>` can be **async** at top level; parent must use **`<Suspense>`** (or handle loading states manually).

---

## Imports and template visibility

Imported components and composables are **directly usable** in template:

```vue
<script setup lang="ts">
import MyButton from './MyButton.vue';
</script>

<template>
  <MyButton />
</template>
```

---

## Co-location benefit (interview narrative)

Group **state + watchers + methods** for a **feature** instead of scattering across **`data` / `computed` / `methods` / `mounted`**. Easier to extract a **`useFeature()`** composable later.

---

## `defineExpose` for imperative handles

```ts
const input = ref<HTMLInputElement | null>(null);
defineExpose({ focus: () => input.value?.focus() });
```

Parent: **`const child = ref(); child.value?.focus()`**.

---

## Related

Next: [Options API](11-Options-API-Overview.md).
