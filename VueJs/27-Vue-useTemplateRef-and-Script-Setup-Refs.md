# `useTemplateRef` and Script-Setup Ref Patterns

Vue 3.5+ introduces **`useTemplateRef('name')`** for **typed**, **named** template refs in **`<script setup>`**, reducing **`ref(null)` + string name** duplication.

---

## Classic pattern

```vue
<script setup lang="ts">
import { ref, onMounted } from 'vue';
const input = ref<HTMLInputElement | null>(null);
onMounted(() => input.value?.focus());
</script>
<template><input ref="input" /></template>
```

---

## `useTemplateRef` direction

```ts
const input = useTemplateRef<HTMLInputElement>('input');
```

Aligns **template `ref="input"`** with a **single** composable-style handle.

---

## Component refs

Use **`InstanceType<typeof Child>`** or **`expose`d** public type for **child** component refs.

---

## Related

- [Composition API script setup](10-Composition-API-script-setup.md)
- [Lifecycle Hooks](12-Lifecycle-Hooks.md)
