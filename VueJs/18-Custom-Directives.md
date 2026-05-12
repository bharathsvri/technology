# Custom Directives

**Directives** encapsulate **DOM-centric** behavior that is awkward in declarative templates alone: **focus**, **click-outside**, **intersection observer**, **tooltip positioning**.

---

## Function shorthand (mount + update)

```ts
const vHighlight: ObjectDirective<HTMLElement, string> = {
  mounted(el, binding) {
    el.style.backgroundColor = binding.value;
  },
  updated(el, binding) {
    el.style.backgroundColor = binding.value;
  },
};
```

Register locally: **`v-highlight`** in `<script setup>`.

---

## Hook object

Common hooks: **`created`**, **`beforeMount`**, **`mounted`**, **`beforeUpdate`**, **`updated`**, **`beforeUnmount`**, **`unmounted`**.

---

## `binding` object

- **`binding.value`** — JS expression passed to directive.
- **`binding.arg`** — `v-my:foo` → **`"foo"`**.
- **`binding.modifiers`** — `v-my.click.stop` style metadata.

---

## When *not* to use directives

Prefer **components** for structured UI. Use directives for **cross-cutting DOM** tweaks shared across many element types.

---

## SSR caution

Directives run against **real DOM** — guard browser APIs (`window`, `ResizeObserver`) or run only **`onMounted`**-equivalent phases.

---

## Related

Next: [Plugins](19-Plugins.md).
