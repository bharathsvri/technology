# Custom Directives

## Hook functions

`mounted`, `updated`, `unmounted` on directive definition object.

```ts
const vFocus = {
  mounted(el: HTMLElement) { el.focus(); },
};
```

## Use cases

Low-level DOM behavior: tooltips, click-outside, intersection observers.

Next: [Plugins](19-Plugins.md).
