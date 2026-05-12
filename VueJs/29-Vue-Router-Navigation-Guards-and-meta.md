# Vue Router: Navigation Guards and `meta`

**Navigation guards** control **enter/leave** and **updates** on routes. **`route.meta`** carries **static** configuration (roles, layout, analytics) readable in components and guards.

---

## Global vs per-route vs in-component

- **Global**: `router.beforeEach` — auth, progress bars.
- **Per-route**: `beforeEnter` on route record — feature-specific gating.
- **In-component**: `onBeforeRouteLeave` — unsaved form warnings.

---

## `meta` typing (TypeScript)

Augment **`RouteMeta`** interface (module augmentation) so **`to.meta.requiresAuth`** is typed project-wide.

---

## Async guards

Return **`Promise<boolean>`** or call **`next`**-style patterns per Vue Router version—**prefer** consistent async style in new code.

---

## Pitfalls

- **Infinite redirect loops** when guard always redirects to a route that **also** fails guard.
- **SSR**: guards run on server for **initial** navigation in Nuxt—no `window`.

---

## Related

- [Vue Router](15-Vue-Router.md)
- [Vue SSR and Nuxt Overview](22-Vue-SSR-and-Nuxt-Overview.md)
