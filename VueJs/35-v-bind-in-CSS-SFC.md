# `v-bind` in CSS (SFC)

Vue SFCs support **`v-bind()` inside `<style>`** to reference **reactive** values as CSS custom properties—no manual **`document.documentElement.style`** sync.

```vue
<script setup lang="ts">
const accent = ref('#3eaf7c');
</script>
<style scoped>
button { background: v-bind(accent); }
</style>
```

---

## Use cases

- **Theming** toggles (dark mode) driven by **JS state**.
- **Layout** numbers from **props** (sidebar width).

---

## Pitfalls

- **SSR**: ensure initial values are **serializable** and **hydration-safe**.
- **Performance**: avoid **per-frame** updates to bound CSS vars without need.

---

## Related

- [Single-File Components](13-Single-File-Components.md)
- [Reactivity ref reactive computed](04-Reactivity-ref-reactive-computed.md)
