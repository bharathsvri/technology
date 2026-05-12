# Accessibility (a11y) in React

Accessibility is a **core** frontend interview theme: keyboard flows, semantics, and ARIA—React does not “fix” a11y automatically.

---

## Foundations

- Prefer **semantic HTML** (`button`, `nav`, `main`, `label`) over `div` + `onClick`.
- **`label`** + **`htmlFor`** tied to **`useId`** for inputs.
- **Keyboard**: `onKeyDown` for Escape to close dialogs; **focus trap** in modals (libraries: **FocusTrap**, **Radix**, **Headless UI**).

---

## Common patterns

| Pattern | Notes |
|---------|--------|
| **Modal** | `role="dialog"`, `aria-modal="true"`, restore focus on close |
| **Disclosure** | `aria-expanded` toggled with state |
| **Live regions** | `aria-live="polite"` for async status updates |

---

## React-specific pitfalls

- **`div` with `onClick`** for navigation → not keyboard reachable; use **`a href`** or **`button type="button"`**.
- **Strict Mode** double-mounting in dev can confuse focus measurements—test in production build too.

---

## Tooling

- **eslint-plugin-jsx-a11y** in CI.
- **axe-core** in **RTL**/**Playwright** tests.

---

## Related

- [Forms in React](10-Forms-in-React.md)
- [Testing with React Testing Library](17-Testing-React-Testing-Library.md)
- [Composition Patterns](22-Composition-Patterns.md)
