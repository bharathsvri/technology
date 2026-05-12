# Fetch API and HTTP

The **`fetch`** API is the modern **Promise-based** way to perform HTTP requests in browsers and in **Node 18+** (undici). It replaces much `XMLHttpRequest` usage for JSON APIs.

---

## GET JSON

```js
async function getJson(url) {
  const res = await fetch(url, {
    headers: { Accept: 'application/json' },
  });
  if (!res.ok) {
    throw new Error(`${res.status} ${res.statusText}`);
  }
  return res.json();
}
```

**Important**: `fetch` **only rejects** on **network failure**. HTTP **404/500** still **resolve** — always check **`res.ok`** or `res.status`.

---

## POST JSON

```js
const res = await fetch('/api/orders', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    Authorization: `Bearer ${token}`,
  },
  body: JSON.stringify({ sku: 'A1', qty: 2 }),
});
```

---

## Aborting requests

```js
const ac = new AbortController();
fetch('/slow', { signal: ac.signal });
ac.abort(); // rejects with DOMException AbortError
```

Useful for **timeouts**, **navigation away**, or **user cancel**.

---

## Credentials and CORS

```js
fetch('/api/me', { credentials: 'include' }); // send cookies — server must allow CORS credentials
```

---

## Streaming responses

`res.body` is a **ReadableStream** — process large downloads without buffering entire body in memory.

---

## Next

- [Web Storage](22-Web-Storage.md)
