# `useContext` and `useReducer`

## `useContext`

Consume nearest `Context.Provider` value—avoid **deep** prop drilling.

```tsx
const theme = useContext(ThemeContext);
```

## `useReducer`

Local Redux-like updates:

```tsx
const [state, dispatch] = useReducer(reducer, initialState);
```

Good for complex form or wizard state.

Next: [useMemo, useCallback, useRef](07-Hooks-useMemo-useCallback-and-useRef.md).
