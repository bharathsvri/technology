# Functional Guards, Resolvers, and `Route.data`

Modern Angular routing favors **functions** over classes for **guards** and **resolvers** (depending on version—verify your project’s router API). **`Route.data`** remains the idiomatic place for **static** route metadata (roles, titles, feature flags).

---

## `data` vs `resolve`

- **`data`**: static config on route definition—available synchronously from **`ActivatedRoute`**.
- **`resolve`**: async (or sync) **data fetch before activation**—use sparingly to avoid blocking navigation; prefer **resolver-less** patterns with **`HttpClient` + component** when UX needs spinners.

---

## Functional guard pattern (conceptual)

Return **`UrlTree`**, **`boolean`**, or **`Observable`/Promise** of those—**`CanMatchFn`** can prevent route registration entirely for lazy features.

---

## Pitfalls

- **Circular dependency** if guards inject services that import router-heavy modules.
- **SSR**: guards must not assume **`window`** exists.

---

## Related

- [Routing and Navigation](10-Routing-and-Navigation.md)
- [Route Guards and Resolvers](11-Route-Guards-and-Resolvers.md)
