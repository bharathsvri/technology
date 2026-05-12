# `forwardRef` and Ref Forwarding Patterns

**`forwardRef`** lets **function components** accept a **`ref`** and pass it to a **child DOM node** or **imperative API**—used by libraries (inputs, focus management) and **composition** patterns.

---

## Basic pattern

```tsx
const Input = forwardRef<HTMLInputElement, Props>(function Input(props, ref) {
  return <input ref={ref} {...props} />;
});
```

---

## With `useImperativeHandle`

Expose a **narrow** imperative surface instead of the whole DOM node when wrapping third-party widgets.

---

## Pitfalls

- **Display name** for DevTools: `Input.displayName = 'Input'`.
- **Ref** may be **`null`** on unmount—guard calls.

---

## Related

- [useLayoutEffect useImperativeHandle useId](24-useLayoutEffect-useImperativeHandle-useId.md)
- [Portals and Refs](15-Portals-and-Refs.md)
