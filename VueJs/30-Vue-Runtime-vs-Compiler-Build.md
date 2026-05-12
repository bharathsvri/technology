# Vue Build Targets: Runtime-Only vs Full Compiler

Vue ships **runtime + compiler** and **runtime-only** builds. **`vue/dist/vue.esm-bundler.js`** expects **compile-time** template transforms from **Vite/webpack**; **`vue.runtime.esm-browser.js`** includes the **template compiler** for in-browser compilation.

---

## Production apps

Use **bundler + runtime-only** to **shrink** bundle—templates live in **`.vue`** files compiled ahead of time.

---

## In-browser demos

**Full build** allows **`template: '<div>...</div>'`** strings on `defineComponent`—educational, not typical prod.

---

## `__VUE_OPTIONS_API__` / `__VUE_PROD_DEVTOOLS__` flags

Feature flags for **tree-shaking** unused APIs—configure via **`define`** in Vite.

---

## Related

- [Vite and Tooling](20-Vite-and-Tooling.md)
- [Single-File Components](13-Single-File-Components.md)
