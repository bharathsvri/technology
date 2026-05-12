# Server-Side Rendering (SSR) with Angular

## Goals

- Faster **first meaningful paint** and better **SEO** for public pages.
- Consistent HTML shell before hydration.

## `@angular/ssr` / Universal patterns

Server bundle runs Angular on **Node** (or other supported environments), serializes HTML, client **hydrates** to attach listeners.

## Data fetching

Use **`TransferState`** to avoid duplicate HTTP on server then client.

## Platform checks

Guard browser-only APIs with `isPlatformBrowser` / `isPlatformServer` from `@angular/common`.

## Trade-offs

More deployment complexity, careful handling of **secrets** and **cookies**, watch for memory limits on Node workers.

## Hydration mismatches

Ensure server and client produce **identical** DOM where required—avoid non-deterministic `Date.now()` in first render without `TransferState`.

Next: [PWA and Service Worker](34-PWA-and-Service-Worker.md).
