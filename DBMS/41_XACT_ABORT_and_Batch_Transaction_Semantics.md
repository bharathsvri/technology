# `XACT_ABORT` and Batch Transaction Semantics

**`SET XACT_ABORT ON`** makes **runtime errors** automatically **rollback** an open transaction in SQL Server—often **recommended** for predictable **T-SQL** batches and **ORM**-generated SQL.

---

## Behavior sketch

- **`ON`**: most errors **abort** the batch and **rollback** the transaction.
- **`OFF`**: some errors only **stop** the current statement; transaction may **continue** unless you **`CATCH`**.

---

## `TRY...CATCH` interaction

Combine with **`THROW`** / **`RAISERROR`** patterns in **`11_Error_Handling.md`**—still validate **`XACT_STATE()`** before **`COMMIT`**.

---

## Interview caution

Client drivers may **implicitly** set options—**verify** session defaults in **pooled** connections.

---

## Related

- [Transactions](09_Transactions.md)
- [Error Handling](11_Error_Handling.md)
