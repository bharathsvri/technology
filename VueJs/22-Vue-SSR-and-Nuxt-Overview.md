# Vue SSR, Nuxt, and Hydration Basics

**SSR** renders HTML on the server for faster **FCP** and **SEO**. **Nuxt** is the common **meta-framework** for Vue SSR, file-based routing, and data fetching conventions.

---

## Why SSR changes your mental model

- **No `window`/`document`** in server-only code paths.
- **State snapshot**: serialize **Pinia** or **initialState** so client **hydrates** without mismatch.
- **Hydration mismatch** → broken UI or warnings—**random IDs**, **locale time**, and **`Date.now()`** in render are classic bugs.

---

## Nuxt concepts (interview bullets)

- **`useFetch` / `useAsyncData`**: fetch tied to route with **keyed** caching.
- **`server/api`**: colocated server routes (Nitro engine).
- **Auto-imports** for composables—team conventions still matter.

---

## Client-only components

Wrap browser-only widgets with **`<ClientOnly>`** (Nuxt) or dynamic import + **`onMounted`** guards.

---

## Related

- [Lifecycle Hooks](12-Lifecycle-Hooks.md)
- [Teleport and Suspense](17-Teleport-and-Suspense.md)
- [Pinia State Management](14-Pinia-State-Management.md)
