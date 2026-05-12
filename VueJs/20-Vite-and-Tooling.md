# Vite and Tooling

**Vite** is the **default** dev/build tool for Vue 3: **native ESM** dev server, **instant HMR**, and **Rollup**-based production builds.

---

## Why Vite feels fast

- Dev server **does not bundle** the whole app up front; transforms **on demand**.
- **esbuild** pre-bundles dependencies for compatibility and speed.

---

## `vite.config.ts` essentials

```ts
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import path from 'node:path';

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: { '@': path.resolve(__dirname, 'src') },
  },
});
```

---

## Environment variables

**`import.meta.env.MODE`**, **`import.meta.env.PROD`**, **`import.meta.env.DEV`**, **`import.meta.env.VITE_*`** — only **`VITE_`** prefixed vars are exposed to client bundle.

---

## Testing stack

- **Vitest** — Vite-native Jest-like runner, fast watch mode.
- **`@vue/test-utils`** — mount components, stub child components, trigger events.

---

## Lint / format

**ESLint** + **`eslint-plugin-vue`** (rules for template correctness). **Prettier** for formatting; **Volar** (Vue - Official) extension in editor.

---

## Production concerns

- **Code splitting** via dynamic `import()`.
- **Asset handling** (`import img from './x.png'` vs `public/` folder rules).
- **Preview** server: **`vite preview`** after **`vite build`**.

---

## Related

- [JavaScript fundamentals](../JavaScript/README.md)
- [Vue README](README.md)
