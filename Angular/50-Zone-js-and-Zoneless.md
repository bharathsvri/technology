# Zone.js and Zoneless Change Detection

## What Zone.js does

Patches **async browser APIs** (timers, XHR, DOM events) so Angular can **tick** change detection after macro tasks complete.

## Costs

Long tasks delay CD; **stack traces** from Zone can complicate debugging; bundle size.

## `NgZone.runOutsideAngular`

Offload high-frequency events (scroll) that should not trigger global CD each tick—**re-enter** zone when updating Angular state.

## Zoneless mode (experimental / evolving)

Angular is moving toward **signal-driven** scheduling where CD runs when **signals** change or **explicit marks** occur—reduces reliance on Zone patches.

## Migration mindset

Prefer **OnPush**, **signals**, **`async` pipe**, and explicit **`ChangeDetectorRef`** only where needed until zoneless maturity matches your risk tolerance.

## Testing

`fakeAsync`, `tick`, `flushMicrotasks` integrate with Zone in tests.
