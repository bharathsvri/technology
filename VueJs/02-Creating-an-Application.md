# Creating an Application

Every Vue 3 SPA starts by **creating an app instance**, **registering plugins**, and **mounting** a root component to a DOM element.

---

## Minimal bootstrap (Vue 3)

```ts
import { createApp } from 'vue';
import App from './App.vue';

createApp(App).mount('#app');
```

**`mount`** returns the **root component proxy** (rarely needed; useful for tests or imperative access).

---

## Plugins: `.use()`

Router, Pinia, i18n, etc. install themselves on the app:

```ts
import { createApp } from 'vue';
import { createPinia } from 'pinia';
import router from './router';
import App from './App.vue';

const app = createApp(App);
app.use(createPinia());
app.use(router);
app.mount('#app');
```

**Order** can matter when plugins depend on each other (e.g. auth plugin after router).

---

## Global registration vs local imports

- **Global**: `app.component('MyButton', MyButton)` — convenient, hurts tree-shaking if abused.
- **Local (Vue 3.3+)**: import in `<script setup>` — **auto-available** in template; preferred for apps.

---

## App-level config (know it exists)

- **`app.config.errorHandler`** — centralize runtime errors.
- **`app.config.performance`** — dev profiling hooks.
- **`app.provide`** — app-wide DI (theme, feature flags).

---

## Vite + SFC compilation

`@vitejs/plugin-vue` compiles **`<template>`** to render functions at build time. **No runtime template compiler** in production if you only use SFCs — smaller bundle.

---

## Interview pitfalls

- Calling **`mount` twice** on the same app instance is wrong; create a **new** `createApp` per mount in tests.
- **`#app` missing** in `index.html` → blank page.

---

## Related

Next: [Template syntax](03-Template-Syntax-and-Directives.md).
