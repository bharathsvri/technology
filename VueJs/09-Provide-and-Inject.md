# Provide and Inject

## `provide` / `inject`

Dependency injection for deeply nested trees without prop drilling.

```ts
provide('theme', theme);
const theme = inject<Theme>('theme')!;
```

## Symbols as keys

Use `Symbol` keys to avoid collisions in libraries.

## Reactivity

Provide `ref`/`computed` so consumers stay reactive.

Next: [Composition API script setup](10-Composition-API-script-setup.md).
