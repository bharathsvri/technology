# Symbol and BigInt

## `Symbol`

Unique primitive identifiers; `Symbol.iterator`, well-known symbols for meta-programming.

## `Symbol.for`

Global symbol registry keyed by string.

## `BigInt`

Arbitrary-precision integers; suffix `n` literal; cannot mix with `Number` without explicit conversion.

## JSON

`JSON.stringify` throws on `BigInt` unless you add a replacer.

Next: [Strict mode and TDZ](26-Strict-Mode-and-Temporal-Dead-Zone.md).
