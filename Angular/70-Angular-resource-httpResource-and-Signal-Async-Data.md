# Angular: `resource`, `httpResource`, and signal-backed async data

## Why this exists
Older patterns lean on **`HttpClient` + RxJS** (`12-HttpClient-and-Interceptors.md`) or **`AsyncPipe`** (`61-AsyncPipe-and-Change-Detection.md`). Modern Angular adds **signal-native** primitives so loading/error/data states are **first-class** without manually mirroring observables into fields.

## `resource` (conceptual API)
Use **`resource`** when async work is driven by a **computed signal** (for example a route param or filter) and you want a single object exposing:

- **`value`**: resolved data (or `undefined` while loading)
- **`status`**: idle / loading / error / resolved semantics (follow current Angular docs for exact discriminated union)
- **`error`**: failure information when the loader rejects

Typical mental model: **loader function** runs when dependencies change; Angular cancels or ignores stale results according to the documented behavior for your version.

## `httpResource`
A specialized variant wired to **`HttpClient`**: you supply URL template + options and get the same **signal-shaped** read model. Prefer it for **GET-style** reads that should stay aligned with reactive inputs.

## When to stay on RxJS instead
- **Streaming** responses, **progress** events, or long-lived subscriptions.
- Interceptors + operators you already standardized on (`retry`, `shareReplay`, etc.) where migration cost outweighs benefit.

## Practices
- Keep **loader side effects** minimal; push business rules into services.
- Align with **SSR**: ensure loaders do not assume browser-only globals without guards (`33-SSR-Angular-Universal.md`, `65-TransferState-and-SSR-State-Hydration.md`).
- Pair with **`httpResource` / `resource` error paths** and your global **`ProblemDetails` or API error shape** conventions.

## Related docs
- `20-Signals.md`, `45-Signals-Advanced-Patterns.md`
- `12-HttpClient-and-Interceptors.md`
- `21-State-Management-Patterns.md` — caching and store boundaries when `resource` is not enough
