# Pinia: Setup Stores vs Options Stores

**Pinia** supports **setup** stores (Composition API style) and **options** stores (Vuex-like **`state/getters/actions`**). New codebases usually prefer **setup** for **TypeScript inference**.

---

## Setup store

```ts
export const useCartStore = defineStore('cart', () => {
  const items = ref<Item[]>([]);
  const total = computed(() => items.value.reduce((s, i) => s + i.price, 0));
  function add(item: Item) { items.value.push(item); }
  return { items, total, add };
});
```

**Return** exposes the public API—**private** helpers stay unreturned.

---

## Options store

Familiar to **Vuex** migrants; slightly more boilerplate for **typed** getters.

---

## Store composition

Call **`useOtherStore()`** inside actions or setup stores for **coordinated** domains—avoid **circular** imports between stores.

---

## Related

- [Pinia State Management](14-Pinia-State-Management.md)
- [Composition API script setup](10-Composition-API-script-setup.md)
