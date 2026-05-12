# Vue Router

## Create router

```ts
const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/', component: Home },
    { path: '/about', component: About },
  ],
});
```

## Navigation

`<router-link>`, `useRouter().push`, `useRoute()` for params/query/meta.

## Navigation guards

`beforeEach` for auth; per-route `beforeEnter`.

Next: [Transitions](16-Transitions-and-Animation.md).
