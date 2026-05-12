# Angular Architecture

## Building blocks

- **Components** — UI + behavior.
- **Templates** — HTML with Angular syntax.
- **Services** — shared logic, DI singletons by default.
- **Directives** — extend DOM behavior/appearance.
- **Pipes** — transform display values.

## Modules vs standalone

**NgModule** grouped declarations/imports/exports; **standalone: true`** on components/directives/pipes reduces module boilerplate for new apps.

## Zone.js (classic)

**Zone.js** monkey-patches async APIs to trigger **change detection**; optional **zoneless** mode in newer Angular with signals/manual CD.

Next: [TypeScript](03-TypeScript-Essentials-for-Angular.md).
