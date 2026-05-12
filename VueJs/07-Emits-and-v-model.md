# Emits and `v-model` on Components

**Emits** are the child’s **output contract**: type-safe events upward. **`v-model`** on a component is **syntactic sugar** over a **prop + update event** pair (Vue 3 supports **multiple** models).

---

## `defineEmits` (typed)

```vue
<script setup lang="ts">
const emit = defineEmits<{
  save: [id: string];
  cancel: [];
}>();

function onOk() {
  emit('save', crypto.randomUUID());
}
</script>
```

**Runtime** array form still works: `defineEmits(['save', 'cancel'])`.

---

## Default `v-model` (`modelValue`)

Parent:

```html
<Child v-model="title" />
```

Child:

```vue
<script setup lang="ts">
const props = defineProps<{ modelValue: string }>();
const emit = defineEmits<{ 'update:modelValue': [v: string] }>();

function update(v: string) {
  emit('update:modelValue', v);
}
</script>
```

Template sugar: **`v-model="title"`** ≡ **`:modelValue="title"`** + **`@update:modelValue="title = $event"`**.

---

## Named `v-model`s

```html
<UserForm v-model:name="name" v-model:email="email" />
```

Child props: **`name`**, **`email`**; events: **`update:name`**, **`update:email`**.

---

## Modifiers on `v-model`

**`.trim`**, **`.number`**, **`.lazy`** on component `v-model` forward to the child if you implement **`emitsOptions`** / handle in child — know that **modifiers require `defineModel` macro or manual plumbing** in advanced cases.

---

## `defineModel` (Vue 3.4+)

Shorthand for two-way binding boilerplate:

```ts
const title = defineModel<string>();
// title.value in script; v-model:title in parent
```

---

## Interview pitfalls

- **`v-model` on native elements** vs **components** — different underlying events (`input` vs `update:modelValue`).
- **Mutating `modelValue` prop** directly is wrong — always **`emit`**.

---

## Related

Next: [Slots](08-Slots-and-Scoped-Slots.md).
