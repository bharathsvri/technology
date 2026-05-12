# Scoped Values (`java.lang.ScopedValue`)

**Scoped values** let you share **immutable** data along a bounded **execution scope** (and child scopes) without the long-lived footprint of **`ThreadLocal`**. They pair with **virtual threads** and **structured concurrency**.

> **JDK status**: Scoped Values went through several **preview** releases (from JDK 21 onward) and were **standardized in JDK 25** ([JEP 506](https://openjdk.org/jeps/506)). On JDK 21–24, enable preview features if you experiment with them.

## Problems with ThreadLocal (Reminder)
- Values can **linger** after a request if not removed—especially risky with pooled platform threads (and still a hygiene issue with virtual threads).
- Mental model is “per OS thread,” which misaligns with **structured** lifetimes.

## Scoped Value Mental Model
1. Declare a **`ScopedValue<T>`** (often `static final`).
2. In a bounded region, call **`ScopedValue.where(sv, value).run(() -> { ... })`** (or `call`).
3. Inside the runnable/callable—and any code they invoke while still on that scope—**`sv.get()`** reads the bound value.
4. When the `run`/`call` completes, the binding **ends** automatically—no manual `remove`.

## Relationship to Structured Concurrency
Forked structured tasks **inherit** scoped value bindings from the parent scope according to the platform rules for your JDK version—verify behavior when mixing with `StructuredTaskScope`.

## When to Prefer What
- **`ScopedValue`**: immutable contextual data tied to a **clear scope** (request id, tenant id for read-only context).
- **`ThreadLocal`**: legacy code paths, mutable per-thread scratch (still prefer removing in `finally`).
- **Reactor `Context`**: reactive pipelines where thread-bound models do not propagate.

## Caveats
- Values should be **effectively immutable**; mutating shared objects defeats the safety story.
- Preview iterations differed slightly by JDK—when upgrading from JDK 21–24 to **JDK 25+**, re-read the release notes for API tweaks (for example changes around `orElse` null handling in the final JEP).

## Related Topics
- `42_Virtual_Threads.md`
- `117_Structured_Concurrency_Java.md`
- `57_Memory_Leaks.md` (ThreadLocal leaks)
