# JSX and Rendering

## JSX rules

One parent element (or **fragment** `<>...</>`); `className` not `class`; camelCase DOM props (`onClick`).

## Expressions

`{value}` embeds JavaScript; booleans/null/undefined render nothing.

## Conditional UI

`cond && <A />`, ternary, early `return null`.

## Root API (React 18)

`createRoot(document.getElementById('root')!).render(<App />)`.

Next: [Components](03-Function-and-Class-Components.md).
