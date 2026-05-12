# `AsyncPipe` and Change Detection

The **`async` pipe** subscribes to **`Observable`** or **`Promise`** in templates and **unsubscribes** on destroy—reducing **memory leaks** and **manual** subscription boilerplate.

---

## Why it triggers change detection

In **default** change detection, **`async` pipe** marks the host view **dirty** when a new value arrives—convenient but can cause **extra** checks if overused on **hot** streams.

---

## Zoneless / signals context

As apps move toward **zoneless** or **signal-heavy** patterns, **`async` pipe** behavior still ties to **zone** in classic setups—know your **Angular version** defaults.

---

## Prefer `toSignal` sometimes

For **Signal**-first components, **`toSignal(obs$)`** can replace **`async` pipe** with explicit **computed** chains—clearer dependency graph in **`script setup`**.

---

## Pitfalls

- **`async` pipe twice** on the **same cold observable** → **two subscriptions** unless shared (use **`shareReplay`** or **`toSignal`**).
- **Nested `async` pipes** hurt readability—**flatten** in component with **`combineLatest`** / **`switchMap`**.

---

## Related

- [RxJS Basics for Angular](15-RxJS-Basics-for-Angular.md)
- [Change Detection](16-Change-Detection.md)
- [RxJS to Signals Interop](53-RxJS-to-Signals-Interop.md)
