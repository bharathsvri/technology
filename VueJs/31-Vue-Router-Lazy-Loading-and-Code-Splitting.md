# Vue Router Lazy Loading and Code Splitting

**Dynamic `import()`** on route components shrinks the **initial** bundle; pair with **`webpackChunkName`** comments (Webpack) or Vite’s automatic **chunk naming** for observability.

---

## `defineAsyncComponent` vs route `component: () => import(...)`

Both create **async boundaries**; route-level lazy loading is the **primary** navigation-based split point.

---

## Loading and error UI

Use **`Suspense`** (where applicable) or route **`meta`** + global loading bar in **`router.beforeEach`**—consistent UX matters more than mechanism.

---

## Prefetching

**`router.prefetch`** patterns or **`<link rel="modulepreload">`** for critical next routes—trade **bandwidth** for **snappier** navigations.

---

## Related

- [Vue Router](15-Vue-Router.md)
- [Teleport and Suspense](17-Teleport-and-Suspense.md)
