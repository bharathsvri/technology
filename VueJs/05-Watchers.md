# Watchers

## `watch`

```ts
watch(source, (newVal, oldVal) => { ... }, { deep: true, flush: 'post' });
```

## `watchEffect`

Auto-track dependencies on first run; good for sync side effects.

## Cleanup

Return teardown function from `watchEffect` callback.

Next: [Components and props](06-Components-and-Props.md).
