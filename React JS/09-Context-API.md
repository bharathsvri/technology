# Context API

## `createContext` + `Provider`

```tsx
const ThemeContext = createContext<'light' | 'dark'>('light');
<ThemeContext.Provider value={theme}>...</ThemeContext.Provider>
```

## Default value

Used only when no matching Provider above—document carefully.

## Performance

Split contexts or pass **memoized** values to avoid broad re-renders; consider **Zustand/Jotai/Redux** for very large trees.

Next: [Forms](10-Forms-in-React.md).
