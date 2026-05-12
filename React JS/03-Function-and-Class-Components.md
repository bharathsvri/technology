# Function and Class Components

## Function components (preferred)

```tsx
export function Welcome({ name }: { name: string }) {
  return <h1>Hello, {name}</h1>;
}
```

## Class components (legacy)

`extends React.Component` with `render()` and lifecycle methods—avoid for new code unless maintaining old codebases.

## Hooks requirement

Hooks only in **function** components or **custom** hooks—not classes.

Next: [Props and state](04-Props-and-State.md).
