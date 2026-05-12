# Teleport and Suspense

**`<Teleport>`** renders UI **elsewhere in the DOM** while preserving component context. **`<Suspense>`** coordinates **async dependencies** with a **fallback** slot.

---

## `<Teleport>`

```html
<Teleport to="body">
  <Modal v-if="open" />
</Teleport>
```

**Why**: modals/tooltips escape **overflow: hidden** and **z-index** stacking issues from nested parents.

**`disabled` prop** — keep subtree in place (e.g. during animation).

**Multiple teleports** to same target **append in order** — document ordering for overlays.

---

## `<Suspense>`

```html
<Suspense>
  <template #default>
    <AsyncPanel />
  </template>
  <template #fallback>
    <Spinner />
  </template>
</Suspense>
```

**`AsyncPanel`** may have **`async setup`** or be an **async component**.

---

## Error handling

Pair with **`onErrorCaptured`** or router-level error hooks; Suspense alone does not replace **error boundaries** from other frameworks (community patterns exist).

---

## SSR notes

Teleport target must exist on server HTML; **Suspense** + SSR streaming semantics depend on your meta-framework (**Nuxt** handles many details).

---

## Related

Next: [Custom directives](18-Custom-Directives.md).
