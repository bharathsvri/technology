# Change Detection

## Default (Zone)

Async events trigger CD from root → leaves; **`ChangeDetectionStrategy.OnPush`** limits checks to @Input reference changes, events, observables with async pipe, signals.

## OnPush best practice

Prefer immutable `@Input()` updates and `async` pipe or signals.

## Manual CD

`ChangeDetectorRef.markForCheck`, `detectChanges` for edge cases.

## Zoneless

Signals + explicit scheduling reduce reliance on Zone.js in supported setups.

Next: [Lifecycle hooks](17-Component-Lifecycle-Hooks.md).
