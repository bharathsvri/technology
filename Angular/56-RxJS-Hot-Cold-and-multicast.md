# RxJS: Hot vs Cold and Multicast

Interviewers use **cold vs hot** observables to probe whether you understand **when side effects run** and how **`share` / `shareReplay`** affect **subscriptions** and **Angular HTTP**.

---

## Cold observable

Each **subscriber** triggers a **new producer** path (e.g. **HttpClient** issues a **new request** per `subscribe` unless shared).

**Implication**: two `async` pipes on the same cold source without sharing → **duplicate network calls**.

---

## Hot observable

Producer **independent** of subscribers (Subjects, DOM events, WebSocket after connection). Late subscribers **miss** earlier emissions unless buffered (`ReplaySubject`, `shareReplay`).

---

## `share()` / `shareReplay(1)`

Multicast a **cold** source so the **upstream** runs once; downstream subscribers share emissions.

**`shareReplay({ bufferSize: 1, refCount: true })`** — common for **cached HTTP**; **`refCount`** tears down when last subscriber leaves (watch for **re-subscribe** churn).

---

## Angular `HttpClient`

Default calls are **cold**. Pair **`shareReplay(1)`** with route resolvers or store streams when multiple consumers need the **same** payload.

---

## Related

- [RxJS Basics for Angular](15-RxJS-Basics-for-Angular.md)
- [HttpClient and Interceptors](12-HttpClient-and-Interceptors.md)
