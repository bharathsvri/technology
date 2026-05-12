# JSON

## `JSON.stringify`

Serializes values; **`undefined`** in objects is omitted; functions omitted.

## `JSON.parse`

Reviver function can transform values while parsing.

## Dates

JSON has no date type—serialize as **ISO strings** and parse back explicitly.

## Security

Never `JSON.parse` **untrusted** data and then `eval`; use schema validation for APIs.

Next: [Promises](15-Promises.md).
