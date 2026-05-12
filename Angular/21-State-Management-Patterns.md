# State Management Patterns

## Service + signals / BehaviorSubject

Shared singleton service holds app state; components consume via signals or `async` pipe.

## NgRx / Akita / Elf

Flux-style **store** for large apps: actions, reducers, selectors, devtools.

## When to adopt a store

Many unrelated components need same data, undo/time-travel, strict audit—otherwise services + router data often suffice.

Next: [Angular CLI](22-Angular-CLI.md).
