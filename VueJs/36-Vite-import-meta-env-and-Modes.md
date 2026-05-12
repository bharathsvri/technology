# `import.meta.env` and Vite Modes

**Vite** exposes **`import.meta.env`** for **build-time** and **mode** variables: **`MODE`**, **`DEV`**, **`PROD`**, **`BASE_URL`**, and **`VITE_*`** prefixed custom keys.

---

## `.env` files

- **`.env`**: all modes
- **`.env.development`**, **`.env.production`**: per **`--mode`**
- **Only `VITE_` keys** are exposed to client code—server secrets stay out of the bundle.

---

## SSR / Nuxt

Nuxt uses **`runtimeConfig`** for **server** vs **public** split—do not confuse with raw Vite env alone.

---

## Pitfalls

- **`import.meta.env` values are strings**—coerce **booleans** explicitly.
- **Changing `.env`** requires **dev server restart** in many setups.

---

## Related

- [Vite and Tooling](20-Vite-and-Tooling.md)
- [Vue SSR and Nuxt Overview](22-Vue-SSR-and-Nuxt-Overview.md)
