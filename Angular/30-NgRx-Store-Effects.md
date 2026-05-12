# NgRx — Store, Actions, Reducers, Effects

## Core ideas

- **Store** holds immutable **state** tree.
- **Actions** describe events (`props` or `createAction`).
- **Reducers** are pure functions `(state, action) => newState`.
- **Selectors** memoize derived data (`createSelector`).
- **Effects** isolate **side effects** (HTTP, toasts) from reducers.

## When NgRx pays off

Many components read/write the same domain state, time-travel debugging, strict audit trails—**not** for every form field.

## Entity adapter

`@ngrx/entity` normalizes collections by `id` for CRUD UIs.

## Store DevTools

Time-travel and action inspection in browser.

## Alternatives

**Akita**, **Elf**, **signals + services**, **TanStack Query** patterns via adapters.

Next: [Angular Material](31-Angular-Material.md).
