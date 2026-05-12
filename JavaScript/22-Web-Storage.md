# Web Storage (`localStorage` / `sessionStorage`)

## API

`setItem`, `getItem`, `removeItem`, `clear` — keys and values are **strings**.

## Quota

Typically ~5MB per origin; `QuotaExceededError` possible.

## Security

Do not store **secrets** or JWTs if XSS exists—any script on origin can read them.

## `sessionStorage`

Tab-scoped; cleared when tab closes.

Next: [Regex](23-Regular-Expressions.md).
