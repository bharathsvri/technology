# Operators and Equality

## Arithmetic

`+ - * / % **` — `**` is exponentiation.

## Logical

`&&`, `||`, `!` — short-circuit; `??` nullish coalescing (only `null`/`undefined`).

## Bitwise

`& | ^ ~ << >> >>>` — rare in app code; common in performance/crypto tutorials.

## Comparison

`< > <= >=` — type coercion on mixed types; use explicit `Number()` when comparing strings to numbers.

## Spread / rest

`...arr` spread; `function(...args)` rest parameters.

Next: [Control flow](05-Control-Flow.md).
