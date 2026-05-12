# `NgTemplateOutlet` and Template Context Typing

**`NgTemplateOutlet`** renders an **`ng-template`** with an optional **context** object—powerful for **table columns**, **reusable** shells, and **typed** slot-like APIs.

```html
<ng-container *ngTemplateOutlet="cellTpl; context: { $implicit: row, col: column }"></ng-container>
```

---

## `$implicit`

Default **template variable** when consumer writes **`let-row`** without **`let-row="..."`**.

---

## Type safety

Use **typed template context** with **`@Input`**/`TemplateRef` generics patterns (Angular version dependent) or **narrow** in wrapper components.

---

## vs `ng-content`

**`ng-content`** projects **static** DOM from parent; **`NgTemplateOutlet`** passes **data** + **lazy** instantiation.

---

## Related

- [Content Projection](18-Content-Projection.md)
- [Built-in Control Flow @if @for @switch](46-Built-in-Control-Flow.md)
