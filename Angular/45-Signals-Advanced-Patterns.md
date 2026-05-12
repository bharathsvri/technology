# Signals — Advanced Patterns

## `linkedSignal`

Derive writable signal from another with explicit **reduction** when source changes (Angular version dependent—verify API in your release notes).

## `resource` (experimental / newer APIs)

Model async data loading with status (`idle`, `loading`, `error`, `loaded`) in a composable way—check current Angular docs for stability.

## Interop

- **`toSignal(observable)`** — bridge RxJS → signal; **`initialValue`** avoids undefined flash.
- **`toObservable(signal)`** — feed operators when legacy code expects streams.

## `untracked`

Read signals **without** creating reactive dependency—avoid feedback loops in `computed`/`effect`.

## `equal` comparator

Custom equality for `signal` updates to reduce noisy downstream recomputation.

Next: [Built-in control flow `@if` `@for` `@switch`](46-Built-in-Control-Flow.md).
