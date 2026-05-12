# Props and State

## Props

Read-only inputs from parent; can be **children** prop for composition.

## State

`useState` returns `[value, setValue]`; updates may be **batched**; functional updates `setCount(c => c + 1)` when next state depends on previous.

## Lifting state up

Share state by moving it to the **lowest common ancestor** and passing callbacks down.

Next: [useState and useEffect](05-Hooks-useState-and-useEffect.md).
