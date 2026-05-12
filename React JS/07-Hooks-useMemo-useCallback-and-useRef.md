# `useMemo`, `useCallback`, and `useRef`

## `useMemo`

Memoize expensive pure computations: `useMemo(() => compute(a), [a])`.

## `useCallback`

Stable function identity for child **`memo`** components: `useCallback(() => {...}, [deps])`.

## `useRef`

Mutable box + DOM handle:

```tsx
const inputRef = useRef<HTMLInputElement>(null);
inputRef.current?.focus();
```

## When not to optimize

Measure first; over-memoization adds complexity.

Next: [Custom hooks](08-Custom-Hooks.md).
