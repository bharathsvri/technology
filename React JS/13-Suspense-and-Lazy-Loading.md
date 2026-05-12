# Suspense and Lazy Loading

## `React.lazy` + `Suspense`

```tsx
const Admin = lazy(() => import('./Admin'));
<Suspense fallback={<Spinner />}>
  <Admin />
</Suspense>
```

## Data Suspense

Experimental patterns with concurrent features and libraries that throw promises to Suspense boundaries.

Next: [Error boundaries](14-Error-Boundaries.md).
