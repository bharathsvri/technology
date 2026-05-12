# Dependency Injection — Advanced

## `InjectionToken`

Typed token for non-class dependencies:

```ts
export const API_BASE = new InjectionToken<string>('api.base');
{ provide: API_BASE, useValue: 'https://api.example.com' }
```

## `useFactory` and `deps`

```ts
{
  provide: AuthConfig,
  useFactory: (env: Environment) => ({ issuer: env.issuer }),
  deps: [Environment],
}
```

## `multi: true`

Multiple providers for one token — e.g. **`HTTP_INTERCEPTORS`**, **`APP_INITIALIZER`**.

## `providedIn` vs module `providers`

`providedIn: 'root'` for tree-shakable singletons; component-level `providers` for **scoped** service instances per component subtree.

## `inject()` function

Use inside `constructor`, field initializers, `runInInjectionContext`, or `afterNextRender` for functional patterns without constructor boilerplate.

Next: [`ControlValueAccessor`](29-ControlValueAccessor.md).
