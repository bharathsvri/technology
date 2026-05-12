# Route Guards and Resolvers

## Functional guards (modern)

`canActivateFn`, `canMatchFn`, `canDeactivateFn` returning `boolean`, `UrlTree`, or `Observable`/`Promise`.

## Use cases

- Auth checks before entering routes.
- Feature flags / role-based access.
- **Dirty form** confirmation on leave (`canDeactivate`).

## Resolvers / `resolve`

Pre-fetch data before activation (prefer `fetch` in router or `input()` binding in v19+ patterns where applicable).

Next: [HttpClient](12-HttpClient-and-Interceptors.md).
