# Vue Testing: VTU, Mounting, and Async Patterns

**@vue/test-utils (VTU)** mounts components in **jsdom** (or happy-dom), drives events, and asserts rendered output—pairs well with **Vitest**.

---

## `mount` vs `shallowMount`

- **`mount`**: full render including child components (integration-ish).
- **`shallowMount`**: stubs children—faster, isolates SUT (unit-ish).

---

## Stubbing async

```ts
import { mount, flushPromises } from '@vue/test-utils';

const w = mount(MyComp, { global: { stubs: { RouterLink: true } } });
await flushPromises();
```

Use **`flushPromises`** after state changes that resolve microtasks (`async setup`, `nextTick`).

---

## Pinia in tests

```ts
import { setActivePinia, createPinia } from 'pinia';

beforeEach(() => {
  setActivePinia(createPinia());
});
```

---

## What to assert

- **Outcomes** users see (text, `aria-*`), not internal `ref` unless testing a **public** imperative API via **`defineExpose`**.

---

## Related

- [Vite and Tooling](20-Vite-and-Tooling.md)
- [Composition API script setup](10-Composition-API-script-setup.md)
