# Introduction to Vue

**Vue** is a **progressive** JavaScript framework: you can enhance a static page with a small widget, or scale to a full **SPA** with routing, global state, and a modern build pipeline (**Vite**).

---

## What “progressive” means in interviews

- **Low ceremony** to start: a script tag or a single mounted root.
- **Escape hatches**: render functions, custom directives, and direct DOM when needed.
- **Official ecosystem** grows with the app: **Vue Router**, **Pinia**, **Vite** — not forced on day one.

---

## Core pillars

| Pillar | Interview soundbite |
|--------|----------------------|
| **Reactivity** | Dependency-tracked state drives the DOM; updates are batched and efficient. |
| **Single-file components (SFC)** | `.vue` bundles template, logic, and scoped style. |
| **Composition API** | Colocate **feature** logic in `setup` / `<script setup>` instead of splitting by option type. |

---

## Vue 2 vs Vue 3 (still asked)

- **Vue 3**: **Composition API** first-class, **Proxy**-based reactivity, **Fragments** / **Teleport** / **Suspense**, better tree-shaking, **`<script setup>`** ergonomics.
- **Vue 2** EOL for open-source line; legacy jobs may mention **Options API** and **Vuex**.

---

## Mental model vs React (common compare question)

- Vue leans on a **template compiler** + **reactivity system** (fine-grained subscriptions) rather than a default “whole subtree reconcile” story (though both can use virtual DOM under the hood).
- **Pinia** ≈ lightweight stores; **provide/inject** for cross-cutting context.

---

## Official ecosystem (names to drop)

- **Vite** — dev server and build.
- **Vue Router** — client-side routing.
- **Pinia** — recommended state management for Vue 3.
- **Vitest** + **@vue/test-utils** — testing.

---

## Related

Next: [Creating an Application](02-Creating-an-Application.md).
