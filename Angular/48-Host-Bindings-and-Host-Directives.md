# Host Bindings and Host Directives

## `@HostBinding` / `@HostListener` (decorators)

Attach properties and events to the **host element** of a directive or component.

## Host directives (composition)

Apply one directive’s behavior to another directive or component’s host—useful for cross-cutting UI concerns without inheritance.

## `host` object in `@Component` / `@Directive` (modern)

```ts
@Component({
  host: {
    '[class.active]': 'isActive()',
    '(click)': 'toggle()',
  },
})
```

Prefer **`host`** metadata or signal-based host APIs per style guide of your Angular version.

## Style encapsulation interaction

Host bindings respect **emulated** encapsulation rules.

Next: [Structural vs attribute directives deep dive](49-Directives-Deep-Dive.md).
