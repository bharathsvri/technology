# Synonyms and Cross-Database References (SQL Server)

## Synonyms
A **synonym** is a database-scoped **alias** for another object, allowing stable names when the underlying object moves between schemas or servers.

```sql
CREATE SYNONYM dbo.CustomerRemote
FOR OtherDb.dbo.Customer;
```

### Uses
- **Decouple** application SQL from physical database/schema names.
- **Simplify** long four-part names in reporting queries.

### Risks
- **Broken synonyms** if the target is renamed/dropped—fail at runtime.
- **Permission** model still flows to the underlying object; synonyms do not bypass security.

## Cross-Database Queries
Three-part name `OtherDb.schema.Table` requires **metadata visibility** and **permissions** in the remote database.

### `contained` databases (brief)
**Partially contained** users can reduce **login mapping** complexity in some consolidation scenarios—evaluate with Microsoft guidance for your edition.

## Linked Servers (Mention)
For true **remote** instances, **linked servers** (`sp_addlinkedserver`) expose four-part names with provider-specific caveats—monitor **security** (credential delegation) and **performance**.

## Related Topics
- `14_Security.md`
- `03_Queries.md`
