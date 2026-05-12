# Plugins

A **Vue plugin** is an **`install(app, options)`** function (or object with **`install`**) that registers **global components**, **directives**, **`provide`**, **`config.globalProperties`**, or **mixin-like** behavior once per app.

---

## Shape

```ts
import type { App } from 'vue';

export const MyPlugin = {
  install(app: App, options: { apiBase: string }) {
    app.provide('apiBase', options.apiBase);
    app.config.globalProperties.$http = createClient(options.apiBase);
  },
};
```

**Usage:**

```ts
createApp(App).use(MyPlugin, { apiBase: '/api' }).mount('#app');
```

---

## Typing `globalProperties` (TypeScript)

```ts
declare module 'vue' {
  interface ComponentCustomProperties {
    $http: HttpClient;
  }
}
```

---

## Idempotent installs

Good plugins guard **double-install** in tests/hot reload.

---

## Prefer smaller surface area

Expose **`provide` + `inject`** or **composables** instead of many **`$`** globals — easier to test and tree-shake.

---

## Related

Next: [Vite](20-Vite-and-Tooling.md).
