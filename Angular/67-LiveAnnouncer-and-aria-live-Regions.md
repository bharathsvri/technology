# `LiveAnnouncer` and `aria-live` Regions

The **Angular CDK a11y** package includes **`LiveAnnouncer`** to programmatically push messages to **screen readers** via **`aria-live="polite"`** or **`assertive"`** regions—essential for **async** success/error toasts that are not focused.

---

## When to use

- **Form submit** completed without moving focus.
- **Infinite scroll** loaded next page (polite announcement).

---

## `aria-live` pitfalls

- **Too chatty** polite regions reduce comprehension—**debounce** announcements.
- **Duplicate** messages may not re-announce—vary **message** string or clear region per UX pattern.

---

## Related

- [Angular CDK](32-Angular-CDK.md)
- [Accessibility a11y in React](../React%20JS/25-Accessibility-a11y-in-React.md) (cross-stack comparison)
