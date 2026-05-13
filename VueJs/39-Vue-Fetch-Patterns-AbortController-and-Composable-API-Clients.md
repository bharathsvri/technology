# Vue: Fetch patterns, `AbortController`, and composable API clients

## Goal
Centralize **HTTP concerns**—base URL, auth headers, JSON parsing, **cancellation**, and **error normalization**—so components only consume **`ref` / `computed` / `shallowRef`** data shaped for templates.

## `fetch` + `AbortController`
- Create a controller per logical request; call **`abort()`** in **`onBeforeUnmount`** or when inputs change.
- Pass **`signal: controller.signal`** into `fetch`.
- Remember **`fetch` only rejects on network failure**; check **`response.ok`** and map HTTP errors to a typed `AppError`.

## Composable sketch (pattern)

```ts
import { ref, shallowRef, watch, onScopeDispose } from 'vue';

export function useJson<T>(url: () => string) {
  const data = shallowRef<T | null>(null);
  const error = ref<unknown>(null);
  const pending = ref(false);

  async function load() {
    const controller = new AbortController();
    pending.value = true;
    error.value = null;
    try {
      const res = await fetch(url(), { signal: controller.signal });
      if (!res.ok) throw new Error(`HTTP ${res.status}`);
      data.value = (await res.json()) as T;
    } catch (e) {
      if ((e as DOMException)?.name !== 'AbortError') error.value = e;
    } finally {
      pending.value = false;
    }
    onScopeDispose(() => controller.abort());
  }

  watch(url, load, { immediate: true });
  return { data, error, pending, reload: load };
}
```

(Adapt to your HTTP client—**axios** interceptors, **ky**, **ofetch**—the cancellation and state shape stay the same.)

## When to reach for a library
- **Automatic retries**, **deduplication**, and **cache staleness** → consider TanStack Query patterns ported to Vue or **Pinia** stores with explicit invalidation (`14-Pinia-State-Management.md`).

## Related docs
- `24-Vue-Composables-and-VueUse-Ecosystem.md`
- `22-Vue-SSR-and-Nuxt-Overview.md` (Nuxt `useFetch` semantics differ)
- `36-Vite-import-meta-env-and-Modes.md` for base URLs per mode
