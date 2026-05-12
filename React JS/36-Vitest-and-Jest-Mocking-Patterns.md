# Vitest and Jest Mocking Patterns

**Vitest** is Jest-compatible for **Vite** projects; **`vi`** APIs mirror **`jest`** (`vi.fn`, `vi.mock`, `vi.spyOn`). Interviews expect **module** vs **function** mock distinctions.

---

## `vi.fn` / spies

```ts
const onSave = vi.fn();
render(<Form onSave={onSave} />);
expect(onSave).toHaveBeenCalledWith({ id: '1' });
```

---

## `vi.mock('module', factory)`

Hoisted to top of file—**replaces** ES module bindings. Use **`vi.importActual`** for partial mocks.

**Pitfall**: mocking **implementation** that **`import` side effects** can hide integration bugs—**limit** scope.

---

## Timers and fake timers

**`vi.useFakeTimers()`** pairs with **RTL** `act` for debounced UI—restore in **`afterEach`**.

---

## Related

- [Testing with React Testing Library](17-Testing-React-Testing-Library.md)
- [Timers and Scheduling](../JavaScript/27-Timers-and-Scheduling.md)
