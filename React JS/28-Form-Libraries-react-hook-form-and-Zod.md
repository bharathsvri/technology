# Form Libraries: `react-hook-form` + Schema Validation

Large React apps rarely use **only** controlled inputs for every field—**react-hook-form** (RHF) minimizes re-renders by **uncontrolled** registration + **refs**, while **Zod** (or Yup) supplies **schema** validation for interviews and production.

---

## RHF basics

- **`register('name')`** spreads props onto inputs.
- **`handleSubmit(onValid, onInvalid)`** gates submission.
- **`watch` / `useWatch`** for dependent fields (use sparingly—re-renders).

---

## Zod resolver

```tsx
const schema = z.object({ email: z.string().email(), age: z.coerce.number().min(18) });
const { register, handleSubmit, formState: { errors } } = useForm({
  resolver: zodResolver(schema),
});
```

**Interview point**: schema is **single source of truth** for client validation; mirror rules on server.

---

## When not to use RHF

Very small forms or **fully controlled** design systems that already batch updates—**trade-off**, not religion.

---

## Related

- [Forms in React](10-Forms-in-React.md)
