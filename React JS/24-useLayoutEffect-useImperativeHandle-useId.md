# useLayoutEffect, useImperativeHandle, and useId

Three hooks that solve **specific** problems—overusing them is a smell; knowing **when** they apply separates mid from senior answers.

---

## `useLayoutEffect`

Runs **after DOM mutations** but **before the browser paints**—synchronous after React commits.

**Use for**: measuring DOM (tooltips, animations that need correct size), preventing **visual flicker** when layout depends on measurements.

**Avoid for**: data fetching, heavy work (blocks paint → hurts INP).

**SSR**: `useLayoutEffect` warns on server—prefer **`useEffect`** unless you have a client-only branch.

---

## `useImperativeHandle`

Customizes the instance value exposed when parent uses **`ref`** on your **forwardRef** component.

**Use for**: focusing inputs, imperative animation APIs, integrating **non-React** libraries.

**Avoid for**: normal data flow—prefer **props** and **declarative** APIs.

```tsx
// Conceptual pattern
useImperativeHandle(ref, () => ({ focus: () => inner.current?.focus() }), []);
```

---

## `useId`

Stable **unique ID** across SSR/client for **`aria-*`**, `htmlFor`, SVG defs—avoids ID collisions when multiple instances mount.

**Not** for: list keys or database primary keys.

---

## Related

- [Hooks useMemo useCallback and useRef](07-Hooks-useMemo-useCallback-and-useRef.md)
- [Portals and Refs](15-Portals-and-Refs.md)
- [Performance Optimization](16-Performance-Optimization.md)
