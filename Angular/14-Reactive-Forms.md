# Reactive Forms

## Building blocks

`FormControl`, `FormGroup`, `FormArray`, `FormBuilder`.

```ts
form = new FormGroup({
  email: new FormControl('', { validators: [Validators.required, Validators.email] }),
});
```

## Template

`formControlName`, `formGroup`, `formArrayName`.

## Value changes

`this.form.valueChanges.pipe(debounceTime(300))` for reactive UX.

## Dynamic forms

Add/remove controls at runtime with `FormArray`.

Next: [RxJS](15-RxJS-Basics-for-Angular.md).
