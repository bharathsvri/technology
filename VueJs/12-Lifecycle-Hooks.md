# Lifecycle Hooks

## Composition API

`onMounted`, `onUpdated`, `onUnmounted`, `onBeforeMount`, etc.

## Options API

`mounted`, `beforeUnmount`, ...

## SSR note

`onMounted` does not run on server—put universal side effects in `created`/`setup` carefully or use `onServerPrefetch`.

Next: [SFC](13-Single-File-Components.md).
