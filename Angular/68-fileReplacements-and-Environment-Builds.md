# `fileReplacements` and Environment-Specific Builds

Angular CLI **`configurations`** (e.g. **`production`**, **`staging`**) swap **source files** at build time via **`fileReplacements`**—commonly `environment.ts` → `environment.prod.ts`.

---

## Pattern

```json
"configurations": {
  "production": {
    "fileReplacements": [
      { "replace": "src/environments/environment.ts", "with": "src/environments/environment.prod.ts" }
    ]
  }
}
```

---

## Alternatives

- **Runtime config** JSON fetched at bootstrap—required when **same artifact** must target **multiple** backends.
- **`import.meta.env`** style patterns in **Vite**-first stacks (compare Vue note).

---

## Pitfalls

- **Secrets** must **never** land in `environment.*.ts` committed to git—use **CI/CD** injection or **server-side** config.

---

## Related

- [Angular CLI](22-Angular-CLI.md)
- [APP_INITIALIZER and Bootstrap](38-APP-Initializer-and-Bootstrap.md)
