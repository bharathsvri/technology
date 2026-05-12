# Pinia State Management

## Store definition

```ts
export const useUserStore = defineStore('user', () => {
  const name = ref('Ada');
  function setName(n: string) { name.value = n; }
  return { name, setName };
});
```

## Usage

`const user = useUserStore()` in `setup`.

## Devtools

Time-travel and action logging.

## vs Vuex

Pinia is the **recommended** store for Vue 3 (simpler API, full TS inference).

Next: [Vue Router](15-Vue-Router.md).
