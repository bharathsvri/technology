# Component Lifecycle Hooks

## Key interfaces

`OnInit`, `OnDestroy`, `OnChanges`, `DoCheck`, `AfterContentInit`, `AfterContentChecked`, `AfterViewInit`, `AfterViewChecked`.

## Typical usage

- **`ngOnInit`** — fetch data, subscriptions (or use `inject` + `effect` in constructor with signals).
- **`ngOnDestroy`** — unsubscribe, clear timers (`takeUntilDestroyed` in recent Angular).

## Constructor vs `ngOnInit`

Avoid heavy work in constructor; inputs not stable until `ngOnChanges`/`ngOnInit`.

Next: [Content projection](18-Content-Projection.md).
