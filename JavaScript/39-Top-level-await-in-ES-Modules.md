# Top-Level `await` in ES Modules

**Top-level `await`** pauses **module evaluation** until a **Promise** settles—only valid in **ES modules** (not classic scripts). It changes **graph loading** semantics vs **`async` IIFE** at module scope.

---

## Behavior

```js
// a.mjs
const config = await fetch('/config.json').then((r) => r.json());
export { config };
```

**Consumers** of this module **wait** implicitly for **`config`** before their own top-level runs (dependency order preserved by the loader).

---

## Interview implications

- **Startup waterfalls**: chains of modules with top-level `await` can **serialize** network.
- **Cyclic imports** + TLA → **complex** ordering; still discouraged.

---

## Node.js

**ESM** `.mjs` / `"type": "module"` required; interop with **CJS** has sharp edges.

---

## Related

- [ES Modules import and export](11-ES-Modules-import-and-export.md)
- [async and await](16-async-and-await.md)
