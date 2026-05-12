# Map, Set, WeakMap, WeakSet

## `Map`

Any keys (including objects); preserves insertion order; `size`, `get`, `set`, `has`, `delete`.

## `Set`

Unique values; useful for deduplication.

## `WeakMap` / `WeakSet`

Keys are **objects only**; weakly held—good for private metadata without preventing GC.

## vs plain objects

Maps avoid prototype key collisions and behave well with non-string keys.

Next: [Symbol and BigInt](25-Symbol-and-BigInt.md).
