# View Encapsulation: `Emulated`, `None`, and `ShadowDom`

**View encapsulation** controls how component **styles** are scoped to the template—default **`Emulated`** emulates shadow DOM with **host attributes** without native **Shadow DOM** everywhere.

---

## Modes

| Mode | Behavior |
|------|-----------|
| **`Emulated`** (default) | Adds unique attributes to template nodes and **scoping** selectors |
| **`None`** | Global styles—use sparingly for **design system** leaf widgets or **third-party** overrides |
| **`ShadowDom`** | Native **shadow root**—strong isolation; **older** browser / polyfill considerations |

---

## `::ng-deep` (deprecated pattern)

Historically pierced encapsulation—**avoid** in new code; use **`ViewEncapsulation.None`** with BEM, **`:host-context`**, or **CSS variables** instead.

---

## Interview note

**ShadowDom** can affect **CDK overlay** styling unless **parts** / **tokens** are planned—**test** global overlay themes.

---

## Related

- [Components and Templates](04-Components-and-Templates.md)
- [Directives — Deep Dive](49-Directives-Deep-Dive.md)
