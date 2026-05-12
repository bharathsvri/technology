# Pinia State Management

**Pinia** is the **official** store for Vue 3: lightweight, **devtools**-friendly, and **TypeScript-first**. It replaces **Vuex** for new projects.

---

## Store styles

**Setup store** (Composition-like):

```ts
import { defineStore } from 'pinia';
import { ref, computed } from 'vue';

export const useUserStore = defineStore('user', () => {
  const name = ref('Ada');
  const greeting = computed(() => `Hello, ${name.value}`);
  function setName(n: string) {
    name.value = n;
  }
  return { name, greeting, setName };
});
```

**Option store** mirrors Vuex’s **`state/getters/actions`** shape — still supported.

---

## Using stores in components

```ts
const user = useUserStore();
user.setName('Grace');
```

**Destructuring** loses reactivity → use **`storeToRefs(user)`** for state/getters.

---

## Actions vs plain functions

**Actions** are trackable in devtools and good for **async** workflows (`await api` then commit-like updates).

---

## Persistence & plugins

Official ecosystem: **pinia-plugin-persistedstate**, router guards reading **`useAuthStore()`**, etc.

---

## Pinia vs provide/inject

| | Pinia | provide/inject |
|--|--------|----------------|
| Scope | App-level **singleton** stores | **Subtree** DI |
| Devtools | Rich | Limited |
| SSR | Needs **per-request** pinia instance on server | Same care |

---

## Testing

Call **`setActivePinia(createPinia())`** in test setup before **`useXStore()`**.

---

## Related

Next: [Vue Router](15-Vue-Router.md).
