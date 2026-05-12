# Vue Router `scrollBehavior`

**`scrollBehavior`** controls **scroll position** on navigation (top, saved position, **anchor** element)—important for **UX** in SPAs with long lists.

---

## Return shapes

- **`{ left: 0, top: 0 }`** — scroll to top.
- **`savedPosition`** from **`popstate`** navigation (back/forward).
- **`el: '#hash'`** — element scroll.

---

## Async `scrollBehavior`

Return a **Promise** when waiting for **layout** (fonts, images) before scrolling—avoid **jumpy** restores.

---

## Related

- [Vue Router](15-Vue-Router.md)
- [Vue Router Lazy Loading and Code Splitting](31-Vue-Router-Lazy-Loading-and-Code-Splitting.md)
