# `input()`, `output()`, and `model()` (Signal-based APIs)

## `input` / `input.required`

```ts
userId = input.required<string>();
optionalTitle = input<string>();
```

Replaces many `@Input()` decorators with **readonly signals** in class fields.

## `output`

```ts
saved = output<void>();
```

EventEmitter-like API without manual instantiation in many cases.

## `model()` for two-way binding

```ts
title = model<string>('');
```

Parent uses **`[(title)]`** on the child selector when exposed as model.

## Migration

Coexists with decorator `@Input`/`@Output` during incremental refactors.

Next: [Host bindings and host directives](48-Host-Bindings-and-Host-Directives.md).
