# Lazy Routes and `loadComponent`

Standalone routing favors **`loadComponent`** (and **`loadChildren`** for child route configs) so **chunks** split by **feature**—smaller initial bundle, faster **TTI**.

---

## Pattern

```ts
{
  path: 'reports',
  loadComponent: () => import('./reports/reports.page').then(m => m.ReportsPage),
}
```

---

## Preloading strategies

**`PreloadAllModules`** vs **custom** `PreloadService` implementing **`PreloadingStrategy`**—balance **bandwidth** vs **perceived** navigation speed.

---

## Pitfalls

- **Circular dependency** between route file and lazy chunk.
- **Guards** referencing services only provided in **lazy** injector—verify **provider** scope.

---

## Related

- [Lazy Loading Routes](23-Lazy-Loading-Routes.md)
- [Standalone Components](09-Standalone-Components.md)
