# Lazy Loading Routes

## `loadComponent` / `loadChildren`

```ts
{
  path: 'admin',
  loadChildren: () => import('./admin/routes').then(m => m.ADMIN_ROUTES),
}
```

Reduces initial bundle size.

## Preloading strategies

`PreloadAllModules` or custom `PreloadingStrategy` for balance between speed and bandwidth.

Next: [Testing](24-Testing-Jasmine-Karma.md).
