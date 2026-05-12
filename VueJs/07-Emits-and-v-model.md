# Emits and `v-model` on Components

## `defineEmits`

```ts
const emit = defineEmits<{ save: [id: string]; cancel: [] }>();
emit('save', id);
```

## `v-model` customization

`modelValue` prop + `update:modelValue` event; named models `v-model:title`.

Next: [Slots](08-Slots-and-Scoped-Slots.md).
