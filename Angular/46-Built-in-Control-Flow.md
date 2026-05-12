# Built-in Control Flow (`@if`, `@for`, `@switch`)

## Motivation

New syntax replaces many `*ngIf`, `*ngFor`, `*ngSwitch` microsyntax cases with **better typing** and **performance** characteristics in the compiler.

## `@if` / `@else`

```html
@if (user(); as u) {
  <p>{{ u.name }}</p>
} @else {
  <p>Guest</p>
}
```

## `@for` with `track`

**Always** specify **`track`** for identity-stable lists—critical for performance and animation.

```html
@for (item of items(); track item.id) {
  <li>{{ item.name }}</li>
}
```

## `@switch`

Exhaustive-style branching in templates.

## Coexistence

Structural directives remain supported; migrate incrementally per component.

Next: [`input()` and `output()` functions](47-input-output-model-functions.md).
