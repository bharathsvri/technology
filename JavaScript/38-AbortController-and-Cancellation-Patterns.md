# `AbortController` and Cancellation Patterns

**`AbortController`** provides an **`AbortSignal`** to cancel **`fetch`**, streams, and custom async work—essential for **React/Vue strict mode** double effects and **navigation away** during in-flight requests.

---

## Fetch cancellation

```js
const ac = new AbortController();
fetch('/api', { signal: ac.signal }).catch((e) => {
  if (e.name === 'AbortError') return; // expected
  throw e;
});
// later:
ac.abort();
```

---

## Composing signals

**`AbortSignal.any([a, b])`** (modern platforms): abort when **either** parent signal aborts—useful for **timeout** + **user navigation**.

---

## Pitfalls

- **`AbortError`** is still an error—**do not** treat as bug in telemetry without filtering.
- Libraries must **honor** `signal`; wrapping `fetch` without passing **`signal`** defeats cancellation.

---

## Related

- [Fetch API and HTTP](21-Fetch-API-and-HTTP.md)
- [Web Workers and structuredClone](34-Web-Workers-and-structuredClone.md)
