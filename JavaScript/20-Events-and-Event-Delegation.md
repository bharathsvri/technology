# Events and Event Delegation

## Adding listeners

```js
btn.addEventListener('click', handler, { once: true, passive: true });
```

## Propagation

**Capture** phase → **target** → **bubble** phase; `stopPropagation`, `preventDefault`.

## Delegation

Attach one listener on a parent; use `event.target.closest('.item')` to handle dynamic children.

## Custom events

`new CustomEvent('my:event', { detail: { id: 1 } })`.

Next: [Fetch](21-Fetch-API-and-HTTP.md).
