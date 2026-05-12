# `useSlots` and `useAttrs` in `<script setup>`

**`useSlots()`** exposes the parent’s **slot functions**; **`useAttrs()`** exposes **non-prop attributes** (e.g. **`class`**, **`aria-*`**) for forwarding to a **root element** or **child** component.

---

## `inheritAttrs: false`

When wrapping a **native input**, set **`inheritAttrs: false`** and manually **`v-bind="attrs"`** on the inner element to avoid **duplicate** attributes on the wrapper.

---

## `useSlots` patterns

Detect **`slots.default`** presence to render **optional** chrome—useful for **polymorphic** components.

---

## Pitfalls

- **`attrs`** are **not reactive** the same way as props in all cases—prefer **explicit props** for critical API.
- **Large `attrs`** objects encourage **unknown prop** warnings—document forwarded props.

---

## Related

- [Composition API script setup](10-Composition-API-script-setup.md)
- [Slots and Scoped Slots](08-Slots-and-Scoped-Slots.md)
