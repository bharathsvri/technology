# React Server Components (RSC) — Overview

**React Server Components** let components run **only on the server**, stream UI, and reduce client JavaScript for **data-heavy** pages. In practice they ship with **Next.js App Router**, **Waku**, and similar frameworks—not as a standalone “import RSC from react” in every project.

---

## Mental model for interviews

- **Server Components**: can **`async`**, read DB/files, use **secrets**; **no** `useState` / `useEffect` / browser APIs.
- **Client Components** (`'use client'` in Next.js): interactive; hydrates; bundle cost applies.
- **Composition**: Server components **can import** client components as children, passing **serializable props** (JSON-like).

---

## Benefits (talking points)

- **Zero or less JS** shipped for static/read-heavy subtrees.
- **Closer data fetching** without exposing internal APIs to the browser.
- **Streaming HTML** for faster first paint when paired with Suspense boundaries.

---

## Constraints

- **No passing functions** from server to client as props (not serializable).
- **Caching and invalidation** become **framework concerns** (Next: `fetch` cache, `revalidatePath`, tags).
- **Mental overhead** vs classic SPA—team needs conventions for **where** state lives.

---

## Compare to SSR-only

Classic **SSR** renders HTML on the server then hydrates a **full client tree**. **RSC** aims at a **split graph** where much of the tree never hydrates.

---

## Related

- [Data Fetching Patterns](12-Data-Fetching-Patterns.md)
- [Suspense and Lazy Loading](13-Suspense-and-Lazy-Loading.md)
- [React 18 Concurrent Features](19-React-18-Concurrent-Features.md)
