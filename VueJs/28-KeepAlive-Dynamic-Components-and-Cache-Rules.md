# `KeepAlive`, Dynamic Components, and Cache Rules

**`<KeepAlive>`** caches **dynamic component** subtrees (via **`<component :is>`**) so **state** and **DOM** survive tab switches—common in **wizards** and **multi-tab admin** shells.

---

## `include` / `exclude`

Pass **name** strings (component **`name`** option or SFC inferred name) to limit cache membership—**memory** control.

---

## Lifecycle hooks for cached components

**`onActivated` / `onDeactivated`** fire when a cached instance is **shown/hidden**—refresh **stale** data here, pause timers on deactivate.

---

## Pitfalls

- **Max** cache depth—**unbounded** caches leak memory with many unique dynamic types.
- **Keys** on **`<component :is>`** control identity; changing key **recreates** instance (often desired).

---

## Related

- [Lifecycle Hooks](12-Lifecycle-Hooks.md)
- [Components and Props](06-Components-and-Props.md)
