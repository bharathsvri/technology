# `APP_INITIALIZER` and Bootstrap Hooks

## `APP_INITIALIZER`

Multi-provider token: factories run **before** app bootstrap completes—use for loading **config.json**, feature flags, or OAuth metadata.

```ts
{
  provide: APP_INITIALIZER,
  useFactory: (cfg: ConfigService) => () => cfg.load(),
  deps: [ConfigService],
  multi: true,
}
```

Return **`Promise` or `Observable`** that completes—Angular waits (with timeout discipline you enforce).

## Pitfalls

Blocking too long hurts **TTI**; show shell spinner or split critical vs lazy config.

## `PLATFORM_INITIALIZER`

Lower-level platform hooks—rare in app code.

## `bootstrapApplication` providers

Register initializers alongside `provideRouter`, `provideHttpClient`, etc.

Next: [Animations](39-Angular-Animations.md).
