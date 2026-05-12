# Watchers

**Watchers** run **imperative side effects** when reactive sources change: API calls, DOM sync outside Vue, logging, bridging to non-Vue libraries.

---

## `watch` — explicit sources

```ts
import { ref, watch } from 'vue';

const query = ref('');
watch(query, (newVal, oldVal) => {
  // fetch, debounce, etc.
});
```

**Options**:

- **`deep: true`** — traverse objects/arrays (costly if large).
- **`immediate: true`** — run once on mount.
- **`flush: 'post'`** (default in Vue 3 for watch) — run after DOM update; use **`'sync'`** only when necessary.

**Multiple sources**:

```ts
watch([foo, bar], ([newFoo, newBar], [oldFoo, oldBar]) => { ... });
```

---

## `watchEffect` — auto-tracked

Runs immediately; dependencies are whatever the callback **read** on that run.

```ts
import { watchEffect } from 'vue';

watchEffect(() => {
  console.log(query.value);
});
```

Good for **quick prototypes**; **`watch`** is clearer when the dependency list must be **explicit** (reviews/team standards).

---

## Cleanup / teardown

**`watchEffect`** accepts an `onInvalidate` callback to cancel stale async work:

```ts
watchEffect((onInvalidate) => {
  const ac = new AbortController();
  onInvalidate(() => ac.abort());
  fetch(`/api?id=${id.value}`, { signal: ac.signal }).then(...);
});
```

With **`watch`**, use a **`cancelled`** flag, **debounce** utilities, or versioned **`requestId`** — the exact cleanup hook name depends on Vue version (`onWatcherCleanup` in newer releases).

---

## When *not* to watch

- **Derived UI state** → **`computed`**.
- **Same value written back** in a watcher on the watched field → **infinite loop** risk.

---

## Related

Next: [Components and props](06-Components-and-Props.md).
