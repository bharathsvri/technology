# Data Fetching Patterns

## `useEffect` + `fetch`

Classic; handle abort with `AbortController`, loading/error states.

## TanStack Query (React Query)

Caching, deduping, background refetch, pagination—recommended for apps hitting REST/GraphQL.

## Server Components (Next.js App Router)

Fetch on server by default in RSC—no client bundle for static data paths.

## Suspense + data libraries

Integrate with concurrent rendering where supported.

Next: [Suspense](13-Suspense-and-Lazy-Loading.md).
