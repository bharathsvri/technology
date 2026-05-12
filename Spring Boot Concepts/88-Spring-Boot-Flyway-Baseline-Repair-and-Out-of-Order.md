# Flyway: Baseline, Repair, and Out-of-Order Migrations

**Flyway** versions **schema history** in **`flyway_schema_history`**. Production teams must understand **baseline** for **brownfield** DBs, **repair** after failed migrations, and **out-of-order** handling in **multi-branch** flows.

---

## `baselineOnMigrate`

Marks an **existing** database at a **baselineVersion** without executing **older** scripts—use when introducing Flyway to a legacy DB.

---

## `repair`

Fixes **failed** migration checksum rows—use **carefully** after verifying the **cause** (hotfix script edit vs corruption).

---

## Out-of-order

`outOfOrder=true` allows **lower** version scripts to appear later—helps **parallel** feature branches but risks **nondeterministic** final schema if abused.

---

## Interview takeaway

“**Migrations are code**—review, test on **copy**, and **never** manually edit applied scripts in prod without **repair** discipline.”

---

## Related

- [Spring Boot Database Migrations](28-Spring-Boot-Database-Migrations.md)
- [Spring Boot Data JPA](09-Spring-Boot-Data-JPA.md)
