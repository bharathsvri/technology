# Storybook for React Components

**Storybook** runs **isolated** component demos with **controls**, **docs**, and **visual regression** hooks—common in **design systems** and **interview** discussions about component quality.

---

## Core ideas

- **Stories** are functions returning component states (`Default`, `Loading`, `Error`).
- **Args** map UI controls to props without rebuilding Storybook for each variant.

---

## Integration

- **Vite** builder (`@storybook/react-vite`) for fast HMR.
- **MSW** addon for network-aware stories (`34-MSW-with-React-Testing-Library.md` patterns align).

---

## Interview framing

- **Visual tests** (Chromatic, Loki) catch **CSS regressions** unit tests miss.
- **A11y addon** runs **axe** checks per story.

---

## Related

- [Testing with React Testing Library](17-Testing-React-Testing-Library.md)
- [Composition Patterns](22-Composition-Patterns.md)
