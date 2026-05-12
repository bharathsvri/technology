# CDK Overlay and `OverlayContainer`

The **Angular CDK Overlay** package positions **floating UI** (menus, tooltips, typeaheads) in a **flexible connected** layer above the app—solving **z-index**, **scroll clipping**, and **positioning** that raw `position: fixed` struggles with.

---

## `Overlay` service

Creates an **overlay pane** with a **portal** (component or template) attached. Supports **positions** (`FlexibleConnectedPositionStrategy`), **scroll strategies** (reposition, block, close), and **backdrop**.

---

## `OverlayContainer`

The **DOM node** where overlays append—customize for **embedded** Angular apps or **shadow DOM** boundaries so **CDK** matches your stacking context.

---

## Interview talking points

- Prefer CDK **Overlay** over ad-hoc **body** appends for **accessibility** (focus trap often paired with **A11yModule** patterns).
- **SSR**: ensure overlay container exists only in **browser** or guard **document** access.

---

## Related

- [Angular CDK](32-Angular-CDK.md)
- [Angular Material](31-Angular-Material.md)
- [SSR and Angular Universal](33-SSR-Angular-Universal.md)
