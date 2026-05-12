# Transitions and Animation

Vue’s **`<Transition>`** and **`<TransitionGroup>`** components apply **CSS classes** or **JS hooks** around **enter/leave** of elements — good for **modals**, **lists**, and **route transitions**.

---

## `<Transition>` basics

```html
<Transition name="fade">
  <p v-if="show">hello</p>
</Transition>
```

**CSS** (name prefix):

```css
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
```

Vue 3 uses **`*-enter-from` / `*-leave-to`** naming (older `*-enter` aliases exist for compat).

---

## `mode="out-in"`

Animate **old** out before **new** in — avoids overlapping layout jumps.

---

## `<TransitionGroup>` for lists

- Children need **stable `:key`**.
- **FLIP** move animations for reordering.

---

## JavaScript hooks

```html
<Transition
  @before-enter="onBeforeEnter"
  @enter="onEnter"
  @after-enter="onAfterEnter"
/>
```

Integrate **GSAP** / **motion** libraries when CSS is not enough.

---

## Performance

Prefer **`transform` / `opacity`** for smooth GPU-friendly transitions.

---

## Related

Next: [Teleport and Suspense](17-Teleport-and-Suspense.md).
