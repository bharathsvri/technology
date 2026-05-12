# `TransferState` and SSR State Hydration

**`TransferState`** is a **key-value** store that **serializes server state** into the HTML payload so the **browser** bootstraps without **re-fetching** the same data—critical for **Angular Universal** performance and **SEO**.

---

## Flow

1. **Server**: `TransferState.set(key, data)` during SSR.
2. **HTML** includes **serialized** state (inline script).
3. **Browser**: read with **`TransferState.get`** before firing duplicate HTTP calls.

---

## Pair with HTTP

Interceptors can **short-circuit** `HttpClient` when a URL’s response already exists in **`TransferState`**.

---

## Pitfalls

- **Stale** data if TTL semantics ignored—only transfer **idempotent** read payloads.
- **Sensitive** payloads must **not** leak secrets into HTML—**filter** keys.

---

## Related

- [SSR and Angular Universal](33-SSR-Angular-Universal.md)
- [HttpClient and Interceptors](12-HttpClient-and-Interceptors.md)
