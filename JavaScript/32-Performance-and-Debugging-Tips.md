# Performance and Debugging Tips

## Measure first

DevTools **Performance** tab, **Lighthouse**, `console.time` / `timeEnd`.

## Hot paths

Avoid **layout thrashing** (read/write DOM interleave); batch DOM updates; use `DocumentFragment`.

## Memory

Detach listeners when disposing components; avoid accidental **global** caches of large graphs.

## Debugging

`debugger` statement, breakpoints, **conditional breakpoints**, `console.trace`, source maps for transpiled code.

## Bundling

Tree-shaking removes dead ESM exports when bundlers can prove unused code paths.
