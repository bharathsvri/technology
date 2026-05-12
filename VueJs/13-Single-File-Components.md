# Single-File Components (SFC)

A **`.vue` file** bundles **`<template>`**, **`<script>`** (often **`<script setup>`**), and **`<style>`** in one module. Vite/webpack loaders compile SFCs into **JS exports** + **scoped CSS**.

---

## Sections

```vue
<template>...</template>

<script setup lang="ts">
// logic
</script>

<style scoped>
.card { border: 1px solid #ccc; }
</style>
```

You can have **multiple `<script>`** blocks (e.g. one normal for pure exports) — less common.

---

## `scoped` CSS

Compiler adds unique **data attributes** to elements and selectors so styles **don’t leak** to children — but **`:deep()`**, **`:slotted()`**, **`:global()`** escape hatches exist for targeted piercing.

---

## CSS Modules

```vue
<style module lang="scss">
.title { font-weight: 700; }
</style>
```

Template: **`:class="$style.title"`**.

---

## Preprocessors & PostCSS

`lang="scss"` requires **`sass`** installed. **PostCSS** (autoprefixer) runs in Vite pipeline.

---

## Custom blocks

`<route>`, `<i18n>` — supported via **Vite plugins** (unplugin-vue-router, vue-i18n).

---

## Interview trade-offs

- **Pros**: locality of behavior, tooling (Volar), typecheck template expressions.
- **Cons**: very large SFCs → split into **composables** + subcomponents.

---

## Related

Next: [Pinia](14-Pinia-State-Management.md).
