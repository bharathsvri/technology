# TanStack Query (React Query) — Core Patterns

**TanStack Query** manages **server state**: caching, deduplication, **background refetch**, pagination, and **optimistic updates**—complementary to **client state** (forms, UI toggles).

---

## Mental model

- **`useQuery`**: key + async **queryFn** → **data, status, error, refetch**.
- **Keys** are arrays: `['user', id]` — hierarchical **invalidation** (`queryClient.invalidateQueries({ queryKey: ['user'] })`).

---

## Stale vs cache time

- **`staleTime`**: how long data is **fresh** (no automatic refetch on mount/focus).
- **`gcTime`** (formerly `cacheTime`): how long **unused** data stays in memory.

Tune per **volatility** of the resource.

---

## Mutations

**`useMutation`** + **`onSuccess`** invalidate related queries or **setQueryData** for instant cache patches.

---

## Pitfalls

- **Over-fetching** every mount with **`staleTime: 0`** defaults—adjust for dashboards.
- **Keys** must include **all** variables that change the payload (including **filters**).

---

## Related

- [Data Fetching Patterns](12-Data-Fetching-Patterns.md)
- [Suspense and Lazy Loading](13-Suspense-and-Lazy-Loading.md)
