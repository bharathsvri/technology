# Template Syntax and Directives

Vue templates are **HTML-like** and compile to **render functions**. **Directives** are attributes with a **`v-`** prefix that attach reactive behavior to the DOM.

---

## Text interpolation

```html
<span>{{ message }}</span>
```

**`{{ }}`** is JavaScript **expression** (not statements). It **HTML-escapes** by default for safety.

**`v-html`**: raw HTML — **XSS risk** unless you fully trust the string.

---

## Common directives

| Directive | Role |
|-----------|------|
| **`v-bind` / `:`** | Bind a JS expression to an attribute or prop. |
| **`v-on` / `@`** | Attach event listeners. |
| **`v-if` / `v-else-if` / `v-else`** | Conditional render (**in/out** of DOM). |
| **`v-show`** | Toggle **`display`** via CSS; element stays in DOM. |
| **`v-for`** | List rendering — always pair with **`:key`**. |
| **`v-model`** | Two-way binding (syntax sugar over value + events). |

---

## `v-if` vs `v-show`

- **`v-if`**: cheaper if false **initially** (no DOM); toggling cost is higher (re-create).
- **`v-show`**: cheaper to toggle visibility; pays DOM cost up front.

---

## `v-for` rules

```html
<li v-for="item in items" :key="item.id">{{ item.name }}</li>
```

- **`:key`** should be **stable** (prefer **ids**, not **array index** if list mutates).
- **Never** put **`v-if` on the same element as `v-for`** (Vue 3 warns; use wrapper or computed list).

---

## Event & form modifiers

- **`@click.stop`** — `event.stopPropagation()`
- **`@submit.prevent`** — `event.preventDefault()`
- **`v-model.lazy`** — sync on `change` instead of every input
- **`.number`**, **`.trim`** — coerce / trim

---

## Dynamic arguments

```html
<a :[attrName]="url">...</a>
<a @[eventName]="handler">...</a>
```

Useful for generic components; **attrName** must be lowercase in DOM for attribute names.

---

## Interview talking points

- Templates are **compiled** → many errors caught at **build** time with tooling.
- **Reactivity** in templates unwraps **`ref`** automatically; in script you use **`.value`**.

---

## Related

Next: [Reactivity](04-Reactivity-ref-reactive-computed.md).
