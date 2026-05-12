# Keys and Lists

## `key` prop

Stable identity for list items—**do not use array index** if order changes or items insert/delete.

## Reconciliation

Keys help React match previous tree to minimize DOM operations.

## Anti-pattern

Random `key` on each render forces full remount.

Next: [Composition](22-Composition-Patterns.md).
