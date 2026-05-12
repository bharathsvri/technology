# RxJS to Signals Interop

Modern Angular code often **coexists** with **Observables** (HTTP, router events, WebSockets) and **Signals** (fine-grained local UI state). Interviews probe how you **bridge** them without double subscriptions or memory leaks.

---

## `toSignal` (RxJS → Signal)

Converts a **cold or hot** observable into a **read-only `Signal`**:

- Handles **subscription lifecycle** tied to **injection context** / explicit options (`manualCleanup` when needed).
- Initial **`undefined`** or a **`initialValue`** option for strict templates.

**Use when**: HTTP result, `ActivatedRoute.paramMap`, websocket stream should drive the template **declaratively**.

---

## `computed` + `toSignal`

Derive UI-only values from the signalified stream without re-subscribing in `subscribe`.

---

## Signal → Observable (less common)

For legacy pipes or operators only available on streams, **`outputFromObservable`** patterns or small **`Observable`** wrappers reading **`signal()`** in `defer` exist—prefer **`toObservable`** from `@angular/core/rxjs-interop` when available in your version for **`effect`**/signal-driven streams.

---

## Pitfalls

1. **Cold HTTP re-executed** if you recreate the observable each CD cycle—**shareReplay(1)** or **`inject(HttpClient)`** in a stable field.
2. **`async` pipe + `toSignal`** duplication — pick **one** reactive path per source.
3. **Manual `subscribe`** in components without teardown → leaks; **`takeUntilDestroyed`** or **`toSignal`** reduce foot-guns.

---

## Related

- [RxJS Basics for Angular](15-RxJS-Basics-for-Angular.md)
- [Signals](20-Signals.md)
- [Signals — Advanced Patterns](45-Signals-Advanced-Patterns.md)
