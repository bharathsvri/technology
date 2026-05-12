# Testing with Jest or Vitest (instead of Karma)

## Why switch

Faster CI, simpler **Node** test runner, better snapshot ergonomics for some teams.

## `jest-preset-angular` or Vitest + `@analogjs/vite-plugin-angular`**

Community recipes evolve—follow the official Angular guide for your major version.

## `TestBed` still applies

Configure standalone component same as Karma tests.

## Mocking HTTP

`HttpClientTestingModule` pattern remains valid.

## E2E

**Playwright** or **Cypress** complement unit tests; do not replace component tests entirely.

Next: [ESLint and `angular-eslint`](43-ESLint-and-angular-eslint.md).
