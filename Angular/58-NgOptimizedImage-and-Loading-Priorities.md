# `NgOptimizedImage` and Loading Priorities

**`NgOptimizedImage`** (built-in directive) wraps image loading with **priority hints**, **lazy loading**, **responsive srcset** generation, and **LCP-friendly** defaults—common in **Core Web Vitals** interview threads.

---

## Why not raw `<img>`

- **Automatic `loading="lazy"`** for non-priority images.
- **`priority`** attribute marks **LCP** candidates for **preload**-style behavior.
- **Built-in warnings** for oversized intrinsic dimensions vs display size.

---

## `fill` mode

For unknown container sizes (hero banners), **`fill`** + positioned parent mimics **`object-fit`** patterns—know **layout** implications.

---

## Interview talking points

- Pair with **CDN** image pipelines (WebP/AVIF) at build or **edge**.
- **SSR**: ensure **width/height** or aspect ratio to reduce **CLS**.

---

## Related

- [Performance Optimization](26-Performance-Optimization.md)
- [SSR and Angular Universal](33-SSR-Angular-Universal.md)
