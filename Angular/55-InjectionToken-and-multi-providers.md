# `InjectionToken` and Multi-Providers

Angular DI uses **tokens** to look up dependencies. Beyond **class tokens**, **`InjectionToken<T>`** gives **type-safe**, **opaque** keys for configuration, feature flags, and **multi** collections (e.g. many `HTTP_INTERCEPTORS`).

---

## Why `InjectionToken`

- **Interfaces** erase at runtime—cannot inject an interface type directly.
- **Strings** collide across libraries—**`InjectionToken`** is referentially unique.

```ts
export const API_BASE_URL = new InjectionToken<string>('API_BASE_URL');
```

Provide in root or route:

```ts
{ provide: API_BASE_URL, useValue: 'https://api.example.com' },
```

Inject with **`inject(API_BASE_URL)`** or constructor injection.

---

## `multi: true`

Register **many** providers under the **same token**; consumers receive **`T[]`** (or inject one-by-one via framework APIs).

**Built-in**: `HTTP_INTERCEPTORS`, `APP_INITIALIZER`.

**Pitfall**: **order** matters for interceptors—document ordering in your library.

---

## `providedIn: 'root'` vs module `providers`

Prefer **`providedIn`** for **tree-shakable** services when the implementation is known at compile time.

---

## Related

- [Services and Dependency Injection](08-Services-and-Dependency-Injection.md)
- [Dependency Injection — Advanced](28-Dependency-Injection-Advanced.md)
