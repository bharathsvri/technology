# `@testing-library/user-event` vs `fireEvent`

**`user-event`** simulates **realistic** user interactions (pointer moves, focus, keyboard layout) vs **`fireEvent`** which dispatches **single DOM events**—prefer **`user-event`** in RTL for **behavioral** tests.

---

## Example intent

```ts
await user.click(screen.getByRole('button', { name: /save/i }));
await user.keyboard('{Enter}');
```

---

## When `fireEvent` remains OK

- **Low-level** component testing **edge cases** (explicit `change` events).
- **Very fast** unit tests where **`user-event`** async overhead is unnecessary (use sparingly).

---

## Pitfalls

- **`await`** all **`user-event`** calls—API is **async**.
- **Pointer** targets must be **visible** / **enabled**—closer to real users, stricter tests.

---

## Related

- [Testing with React Testing Library](17-Testing-React-Testing-Library.md)
- [Vitest and Jest Mocking Patterns](36-Vitest-and-Jest-Mocking-Patterns.md)
