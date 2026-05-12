# Provide and Inject

**Provide / inject** avoids **prop drilling** by letting an ancestor **offer** a value and a descendant **request** it by key — useful for **theme**, **auth context**, **table column APIs**, and **micro-frontend** boundaries.

---

## Basic usage

**Ancestor:**

```ts
import { provide, ref } from 'vue';

const theme = ref<'light' | 'dark'>('light');
provide('theme', theme);
```

**Descendant:**

```ts
import { inject, type Ref } from 'vue';

const theme = inject<Ref<'light' | 'dark'>>('theme');
if (!theme) throw new Error('theme not provided');
```

---

## Symbol keys (library pattern)

```ts
export const ThemeKey: InjectionKey<Ref<Theme>> = Symbol('Theme');
provide(ThemeKey, theme);
const theme = inject(ThemeKey)!;
```

Avoids **string collisions** between unrelated packages.

---

## Reactivity through `provide`

Provide **`ref` / `computed` / `reactive`**, not bare primitives, if children must **track updates**.

**Anti-pattern**: provide a **plain object** then replace the whole object reference upstream without also updating provided reference — consumers may still hold stale refs unless design is clear.

---

## Default values

```ts
const locale = inject('locale', () => 'en');
```

Use factory for **non-primitive** defaults so each inject gets a fresh object if needed.

---

## SSR and testing

Inject is **runtime** wiring — ensure tests **`provide`** the same keys your components **`inject`**.

---

## vs Pinia

- **Pinia**: global **stores**, devtools, persistence plugins.
- **provide/inject**: **tree-local** services (per subtree), not necessarily singleton.

---

## Related

Next: [Composition API script setup](10-Composition-API-script-setup.md).
