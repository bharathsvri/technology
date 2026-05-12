# Composables and the VueUse Ecosystem

**Composables** are functions using Vue’s reactivity APIs (`ref`, `computed`, `onMounted`) that can be **shared** across components—Vue’s answer to **React custom hooks**.

---

## Authoring pattern

```ts
import { ref, onMounted, onUnmounted } from 'vue';

export function useWindowWidth() {
  const width = ref(window.innerWidth);
  function onResize() { width.value = window.innerWidth; }
  onMounted(() => window.addEventListener('resize', onResize));
  onUnmounted(() => window.removeEventListener('resize', onResize));
  return { width };
}
```

**Naming**: **`useX`** convention signals composable.

---

## VueUse (awareness)

**VueUse** ships **ready-made composables** (`useMouse`, `useLocalStorage`, `useDark`)—in interviews, mention **“don’t reinvent** debounced event listeners**”** and **SSR guards** (`useEventListener` handles client-only).

---

## Pitfalls

- **Side effects** without **`onUnmounted`** cleanup.
- **Composable calling order**: don’t call composables **conditionally** (same rule as React hooks).

---

## Related

- [Composition API script setup](10-Composition-API-script-setup.md)
- [Watchers](05-Watchers.md)
