# Vue Router

**Vue Router** maps **URLs** to **components**, enables **nested routes**, **lazy loading**, and **navigation guards** for auth and data prefetch.

---

## Creating the router

```ts
import { createRouter, createWebHistory } from 'vue-router';

export default createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    { path: '/', name: 'home', component: () => import('./views/Home.vue') },
    { path: '/about', component: () => import('./views/About.vue') },
  ],
});
```

**`createWebHashHistory`** — no server fallback needed; uglier URLs.

---

## Programmatic navigation

```ts
import { useRouter, useRoute } from 'vue-router';

const router = useRouter();
const route = useRoute();

router.push({ name: 'user', params: { id: '42' } });
```

**`route.params` / `route.query` / `route.meta`** — reactive in setup.

---

## Navigation guards

**Global:**

```ts
router.beforeEach((to, from) => {
  if (to.meta.requiresAuth && !isLoggedIn()) return { name: 'login' };
});
```

**Per-route** `beforeEnter`, **in-component** `onBeforeRouteLeave` / `onBeforeRouteUpdate` from **`vue-router`**.

---

## Lazy routes & chunks

**`() => import()`** splits bundles — mention **loading spinners** + **error boundaries** for failed dynamic imports.

---

## Scroll behavior

`createRouter({ scrollBehavior(to, from, saved) { ... } })` — restore position on back navigation.

---

## Interview pitfalls

- **`params` vs `query`** — `params` need route path placeholders **`/user/:id`**.
- **Adding new reactive fields** to **`route.query`** — treat **`route`** object as readonly from router’s perspective.

---

## Related

Next: [Transitions](16-Transitions-and-Animation.md).
