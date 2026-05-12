# Render Functions and JSX in Vue

Vue templates compile to **render functions**. You can write **render functions** or **JSX** directly when you need **dynamic** tag/component choice, **higher-order** rendering, or **library** integration.

---

## `h()` (hyperscript)

```ts
import { h } from 'vue';

export default {
  props: ['tag'],
  setup(props) {
    return () => h(props.tag, { class: 'box' }, () => 'content');
  },
};
```

**Third argument**: children (string, array of VNodes, or slot function depending on signature).

---

## JSX (`@vue/babel-plugin-jsx`)

```tsx
const vnode = <MyComponent count={n.value} v-model={title} />;
```

Know project **TS** config for **`jsxImportSource`**.

---

## When templates win

Most app UI—**compiler optimizations**, **`v-model`**, and **directives** are ergonomic in SFC templates.

---

## Related

- [Components and Props](06-Components-and-Props.md)
- [Slots and Scoped Slots](08-Slots-and-Scoped-Slots.md)
