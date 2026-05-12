# React Router Data APIs (Overview)

**React Router v6.4+** introduces **data routers** (`createBrowserRouter`, `RouterProvider`) with **`loader`**, **`action`**, and **`errorElement`** colocated on route definitions—closer to **Remix-style** data coupling.

---

## `loader`

Runs **before** route render to **fetch** data; can return **plain objects** or **`defer`** for streaming promises.

**Benefit**: parallel data loading per route segment; **type-safe** params via **`LoaderFunctionArgs`**.

---

## `action`

Handles **mutations** from **`<Form method="post">`** without manual `preventDefault` glue in many cases.

---

## Error boundaries per route

**`errorElement`** maps thrown errors / responses to UI—**localized** failure surfaces.

---

## Migration note

Classic **`BrowserRouter` + `useEffect` fetch`** remains valid; data APIs are **opt-in** complexity for **better UX** on navigations.

---

## Related

- [React Router](11-React-Router.md)
- [Data Fetching Patterns](12-Data-Fetching-Patterns.md)
