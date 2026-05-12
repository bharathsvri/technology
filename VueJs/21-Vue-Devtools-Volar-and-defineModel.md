# Vue Devtools, Volar, and `defineModel` Patterns

This note bundles **tooling** and a **common API pattern** that often appears in Vue 3.4+ codebases and interviews.

---

## Vue / Vite DevTools

- **Component tree** with props/state inspection.
- **Pinia** store timeline.
- **Router** navigation history.

**Interview tip**: mention **devtools hook timing** does not change production behavior—strip in prod builds by default.

---

## Volar (Vue - Official)

- **SFC typecheck** for `<template>` expressions.
- **Takeover mode** (historical) vs built-in TS server—follow current extension docs.

---

## `defineModel` recap

```vue
<script setup lang="ts">
const title = defineModel<string>({ required: true });
</script>
```

Parent: **`v-model="docTitle"`** ↔ child **`title`** without hand-written **`update:modelValue`**.

**Modifiers**: `defineModel` can receive **`trim`** / **`number`** depending on setup—confirm project Vue version.

---

## `defineSlots` / generic components (awareness)

Vue 3.3+ improved **generic** components and **typed slots**—worth naming in senior interviews even if you do not memorize syntax.

---

## Related

- [Composition API script setup](10-Composition-API-script-setup.md)
- [Emits and v-model](07-Emits-and-v-model.md)
- [Vite and Tooling](20-Vite-and-Tooling.md)
