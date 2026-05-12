# Slots and Scoped Slots

**Slots** are **composition outlets**: the parent supplies **markup** where the child declares **`<slot>`** placeholders. **Scoped slots** let the **child pass data up** into the parent’s slot template.

---

## Default slot

**Child:**

```vue
<template>
  <div class="card">
    <slot />
  </div>
</template>
```

**Parent:**

```html
<Card>Any content here projects into default slot.</Card>
```

---

## Named slots

**Child:**

```vue
<slot name="header" />
<slot /> <!-- default -->
<slot name="footer" />
```

**Parent** (shorthand **`#`** = **`v-slot:`**):

```html
<Card>
  <template #header><h1>Title</h1></template>
  Main body
  <template #footer><button>OK</button></template>
</Card>
```

---

## Scoped slots (slot props)

**Child** exposes **`item`** to parent:

```vue
<slot name="row" :item="row" :index="i" />
```

**Parent:**

```html
<DataTable>
  <template #row="{ item, index }">
    <td>{{ index }}</td><td>{{ item.name }}</td>
  </template>
</DataTable>
```

This pattern powers **tables**, **lists**, and **renderless components** (logic in child, presentation in parent).

---

## Fallback content

```vue
<slot>Default when parent provides nothing</slot>
```

---

## `v-slot` on component vs on `<template>`**

Use **`<template #name>`** when placing multiple slots; `v-slot` must bind to **`template`** or the **component root** per Vue grammar rules.

---

## Interview angles

- Compare to **React `children` render props** — scoped slot is the **render prop** pattern.
- **Performance**: heavy slot content re-renders with parent unless **`v-memo`** / stable **`key`** strategies apply.

---

## Related

Next: [Provide / inject](09-Provide-and-Inject.md).
