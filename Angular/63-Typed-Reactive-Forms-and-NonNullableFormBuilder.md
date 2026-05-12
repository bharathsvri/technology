# Typed Reactive Forms and Non-Nullable Controls

Angular reactive forms support **strong typing** with **`FormControl<T>`**, **`FormGroup<T>`**, and **`NonNullableFormBuilder`**—reduces **`any`** drift in large forms.

---

## `NonNullableFormBuilder`

```ts
const fb = inject(NonNullableFormBuilder);
const form = fb.group({ email: [''] }); // FormControl<string>, not string | null
```

---

## `valueChanges` typing

Typed controls propagate to **`valueChanges`** as **`Observable<T>`**—pair with **`takeUntilDestroyed`** for subscriptions in **`inject()`** contexts.

---

## Dynamic controls

**`Record<string, FormControl>`** vs **`FormArray`**—choose based on **schema** stability; dynamic keys complicate typing but **`Partial`** helpers exist.

---

## Related

- [Reactive Forms](14-Reactive-Forms.md)
- [TypeScript Essentials for Angular](03-TypeScript-Essentials-for-Angular.md)
