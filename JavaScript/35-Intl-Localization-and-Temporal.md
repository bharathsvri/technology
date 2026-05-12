# Intl, Localization, and Temporal (roadmap awareness)

**`Intl`** provides **locale-aware** formatting and collation in JavaScript without shipping your own locale tables for common cases.

---

## `Intl.DateTimeFormat`

```js
new Intl.DateTimeFormat('en-IN', { dateStyle: 'medium', timeStyle: 'short' }).format(new Date());
```

Always pass an explicit **locale** (or app-configured) rather than assuming **en-US**.

---

## `Intl.NumberFormat`

Currency, percent, compact notation:

```js
new Intl.NumberFormat('de-DE', { style: 'currency', currency: 'EUR' }).format(1234.5);
```

---

## `Intl.Collator`

Correct **string sort** per locale (accents, phonebooks).

---

## `Intl.Segmenter` (modern engines)

Grapheme-aware segmentation for **Unicode** text UI (careful emoji handling).

---

## Temporal (stage / platform dependent)

**`Temporal`** (proposal implemented in some runtimes behind flags or partial support) addresses **`Date`** pain points (immutable types, time zones, durations). In interviews, acknowledging **`Date` pitfalls** + **`Temporal` direction** is often enough unless the role is i18n-heavy.

---

## Pitfalls

- **`Date`** parses **non-ISO** strings inconsistently—prefer **ISO 8601** or explicit parsing libraries (**date-fns-tz**, **Luxon**) for complex TZ rules.
- Store **UTC** on server; format at edge.

---

## Related

- [JSON](14-JSON.md)
- [Node.js and npm Overview](30-Node-js-and-npm-Overview.md)
