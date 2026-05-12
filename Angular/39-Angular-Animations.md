# Angular Animations (`@angular/animations`)

## Module setup

Import **`provideAnimations()`** (or `provideNoopAnimations()` for tests) in standalone bootstrap.

## Trigger / state / transition

```ts
trigger('openClose', [
  state('open', style({ height: '200px' })),
  state('closed', style({ height: '100px' })),
  transition('open <=> closed', animate('300ms ease-in')),
])
```

## DSL in component metadata

`animations: [...]` in `@Component` decorator.

## Template binding

`[@openClose]="isOpen ? 'open' : 'closed'"`

## Performance

Prefer **transform** and **opacity** for GPU-friendly transitions; avoid animating **top/left** on large lists.

## Web Animations API

Angular uses the WAAPI where available.

Next: [Workspace and libraries](40-Workspace-and-Libraries.md).
