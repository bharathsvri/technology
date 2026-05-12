# Lifecycle Hooks

Lifecycle hooks let you tie into a component’s **mount**, **update**, and **unmount** phases for DOM access, timers, subscriptions, and **SSR-safe** initialization.

---

## Composition API hooks

| Hook | Typical use |
|-------|-------------|
| **`onBeforeMount`** | Last moment before DOM mount |
| **`onMounted`** | DOM ready: charts, `focus()`, third-party widgets |
| **`onBeforeUpdate`** | Inspect DOM before patch (rare) |
| **`onUpdated`** | After reactive data caused re-render |
| **`onBeforeUnmount`** | Teardown about to happen |
| **`onUnmounted`** | Clear intervals, abort fetches, remove listeners |

**`onActivated` / `onDeactivated`** — with **`<KeepAlive>`** cached components.

---

## Options API equivalents

`beforeMount` → `mounted` → `beforeUpdate` → `updated` → `beforeUnmount` → `unmounted`.

---

## SSR considerations

- **`onMounted`** does **not** run on the server — no `window` / `document` there.
- Universal data fetching: **`onServerPrefetch`** (Nuxt has its own **`asyncData`** / **`useFetch`**).

---

## Async setup + Suspense

If `setup` is **async**, parent should wrap with **`<Suspense>`** or handle pending UI; otherwise you get **warnings** / blank subtree depending on version.

---

## Pitfalls

- Registering **`document` listeners** in `mounted` without **`removeEventListener` in `onUnmounted`** → leaks.
- Starting **`setInterval`** without cleanup.
- Assuming **child** mounted before **parent** `mounted` — actually **parent `mounted` runs after all children `mounted`**.

---

## Related

Next: [SFC](13-Single-File-Components.md).
