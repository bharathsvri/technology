# Angular Elements and Micro-frontends

**Angular Elements** packages Angular components as **Custom Elements** (`<my-widget>`) so other teams (React, Vue, vanilla) or a **shell app** can embed your UI without bootstrapping a full Angular zone inside theirs (with caveats).

---

## Why it comes up in interviews

- **Incremental migration** from Angular to another stack (or the reverse).
- **Micro-frontends**: one team ships a **self-contained bundle** registered as a web component.
- **Design systems**: publish a versioned **component library** consumable outside Angular templates.

---

## High-level flow

1. Use **`@angular/elements`** to bootstrap a **CustomElementConstructor** from a component.
2. Bundle with **single bundle** output (often **ngx-build-plus** or custom **Webpack** config historically; check current Angular docs for your version).
3. Host page calls **`customElements.define('app-root', constructor)`** after loading the script.

---

## Trade-offs

| Pros | Cons |
|------|------|
| Framework-agnostic consumption | **Bundle size** (Zone.js, polyfills) unless **zoneless** + careful build |
| Encapsulation of team ownership | **Change detection** and **DI** boundaries need clear contracts |
| Versioned deployments | **Style isolation** (Shadow DOM vs global CSS strategy) |

---

## Micro-frontend coordination patterns

- **Module Federation** (Webpack) or **native import maps** — who loads which remote.
- **Routing**: shell owns top-level routes; remote exposes **path prefix** or **web component route outlet**.
- **Shared dependencies**: align **Angular major** versions or accept duplication.

---

## Interview talking points

- Prefer **clear props/attributes API** on the custom element; avoid leaking Angular services to the host.
- Discuss **SSR** implications — custom elements in Universal need **DOM availability** checks.

---

## Related

- [Standalone Components](09-Standalone-Components.md)
- [Workspace and Libraries](40-Workspace-and-Libraries.md)
- [SSR and Angular Universal](33-SSR-Angular-Universal.md)
