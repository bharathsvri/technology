# ESLint and `angular-eslint`

## `@angular-eslint`

Replaces legacy TSLint with **ESLint** rules aware of templates and Angular metadata.

## Template linting

`@angular-eslint/template/*` rules catch accessibility issues, `*ngIf` misuse, etc.

## Strictness

Enable **`strictTemplates`** in `tsconfig` + ESLint recommended sets for maximum safety.

## CI

Run `ng lint` (or `eslint .`) in pipeline with **zero warnings** policy for new code.

## Formatter

Pair with **Prettier**; use `eslint-config-prettier` to avoid rule conflicts.

Next: [Angular DevTools](44-Angular-DevTools.md).
