# `@defer` — Lazy Template Blocks

## Purpose

**Defer** loading of heavy template subtrees until the browser is idle, the user scrolls into view, or a **custom trigger** fires—smaller initial bundles and faster first paint.

## Core blocks

- **`@defer (on viewport; on idle; on timer(2s); when expr)`** — when to start loading.
- **`@placeholder`** — UI before deferred chunk loads.
- **`@loading`** — optional minimum loading UI.
- **`@error`** — if lazy dependency fails.

## Prefetching

`prefetch when` loads resources earlier than display when appropriate.

## Interaction with routing

Different from route-level lazy loading—**component-level** deferral of template + dependencies.

## Pitfalls

Avoid deferring **above-the-fold** critical content without placeholders that preserve layout (**CLS**).

## Version notes

`@defer` shipped in Angular **17+**; verify exact trigger syntax in docs for your version.
