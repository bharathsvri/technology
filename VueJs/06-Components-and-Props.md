# Components and Props

Vue components are **reusable pieces** with **inputs (props)**, **outputs (emits)**, and **slots** for composition. Props enforce **one-way data flow** from parent to child.

---

## Defining props (`<script setup>` + TypeScript)

```vue
<script setup lang="ts">
const props = defineProps<{
  title: string;
  count?: number;
}>();
</script>
```

**Defaults** with TypeScript:

```ts
const props = withDefaults(
  defineProps<{ count?: number }>(),
  { count: 0 }
);
```

Runtime declaration (no TS):

```ts
defineProps({
  title: { type: String, required: true },
  count: { type: Number, default: 0 },
});
```

---

## One-way flow

**Do not** mutate a prop object’s fields in the child unless the parent intends **shared mutable state** (rare, hard to reason about). Prefer:

- **`emit`** an event so parent updates source of truth, or
- **Local copy** with `watch` on prop → `local = { ...prop }`, or
- **`v-model`** pattern for two-way binding with explicit events.

---

## Prop types and validation

Runtime checks run in **dev** only. **`validator`** functions are possible for custom rules.

---

## Boolean and multiple attributes coercion

- Boolean prop absent vs `prop=""` behavior follows Vue’s **boolean casting** rules — read docs when writing toggles.

---

## Performance note

**Large objects** as props cause child re-renders when parent re-renders unless you **`v-memo`** / split state / use **`shallowRef`** patterns. Mention **“pass ids, fetch in child”** for huge payloads.

---

## Related

Next: [Emits and v-model](07-Emits-and-v-model.md).
