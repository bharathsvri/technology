# Performance Optimization

## OnPush + immutable data

Reduce change detection cost.

## `trackBy` in `*ngFor`

Stable identity for list items.

## Bundle size

Lazy routes, analyze with `ng build --stats-json` + webpack-bundle-analyzer.

## Images and defer

`NgOptimizedImage`, native `loading="lazy"` where appropriate.

## Avoid heavy work in templates

No function calls that allocate or compute large structures per CD tick.
