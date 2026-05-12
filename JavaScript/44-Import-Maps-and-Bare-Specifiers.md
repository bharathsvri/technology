# Import Maps and Bare Specifiers Resolution (Browser)

**Import maps** let browsers resolve **bare specifiers** (`import "vue"`) to URLs without a bundler—useful for **prototypes** and **esm.sh**-style CDNs; most production apps still bundle.

---

## Example

```html
<script type="importmap">
{
  "imports": {
    "vue": "https://cdn.example.com/vue.esm-browser.js"
  }
}
</script>
<script type="module">import { createApp } from 'vue';</script>
```

---

## Cascading maps

**`scopes`** prefix remapping for dependency subgraphs—mirrors **package exports** concepts loosely.

---

## Pitfalls

- **Version skew** across transitive imports without lockfile discipline.
- **CORS** and **integrity** (`importmap` + **`integrity` attribute** patterns evolving)—audit for supply chain.

---

## Related

- [ES Modules import and export](11-ES-Modules-import-and-export.md)
- [Node.js and npm Overview](30-Node-js-and-npm-Overview.md)
