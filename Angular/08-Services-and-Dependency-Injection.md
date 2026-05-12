# Services and Dependency Injection

## `@Injectable({ providedIn: 'root' })`

Singleton service tree-shakable when unused.

## Constructor injection

```ts
constructor(private http: HttpClient) {}
```

## Injection tokens

`InjectionToken<T>` for typed non-class dependencies.

## Hierarchical injectors

`providers` on component → subtree scope; useful for feature-local state.

Next: [Standalone components](09-Standalone-Components.md).
