# `ControlValueAccessor` — Custom Form Controls

Bridge Angular **forms** (`FormControl`, `ngModel`) to **non-native** widgets (rich editors, sliders, third-party UI).

## Interface methods

- `writeValue(obj)` — push model → view.
- `registerOnChange(fn)` — view changed → notify Angular.
- `registerOnTouched(fn)` — mark touched on blur/interaction.
- `setDisabledState(isDisabled)` — sync disabled state.

## Provider registration

```ts
{
  provide: NG_VALUE_ACCESSOR,
  useExisting: forwardRef(() => RatingComponent),
  multi: true,
}
```

## `NgControl` injection

Optional: inject `NgControl` and set `valueAccessor = this` for advanced patterns (avoid circular DI pitfalls).

## Accessibility

Wire `aria-*`, keyboard support, and focus management like native inputs.

Next: [NgRx](30-NgRx-Store-Effects.md).
