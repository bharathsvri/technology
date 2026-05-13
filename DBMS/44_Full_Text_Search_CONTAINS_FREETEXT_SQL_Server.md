# SQL Server: Full-Text Search with `CONTAINS` and `FREETEXT`

## What full-text search adds
**LIKE '%word%'** cannot use normal B-tree indexes effectively and scans large text. **Full-text indexes** tokenize words (with language-specific word breakers) and support ranked search.

## Prerequisites (overview)
1. Create a **full-text catalog**.
2. Define a **full-text index** on the columns to search (often `VARBINARY(MAX)` / `FILESTREAM` or large `NVARCHAR` documents).
3. Maintain the index population schedule appropriate to your RPO/RTO.

(Exact DDL varies by edition and operational standards; treat the above as a checklist, not a copy-paste runbook.)

## `CONTAINS` vs `FREETEXT`
- **`CONTAINS`**: precise **term** or **phrase** queries, proximity (`NEAR`), inflectional forms (`FORMSOF`), weights—best when users supply **keywords** you control.
- **`FREETEXT`**: natural-language style ranking—more “Google-like,” but less predictable for security-sensitive filtering.

### Example shape

```sql
SELECT Id, Title
FROM dbo.Articles
WHERE CONTAINS((Title, Body), N'"payment gateway" AND fraud');
```

## Operational notes
- Full-text populations can lag writes; do not treat the index as strongly consistent unless you verify settings.
- Combine with **`NOEXPAND`** / indexed views only when applicable; avoid mixing conflicting hints without measuring plans (`17_Execution_Plans_and_Query_Store.md`).

## Related docs
- `07_Indexes.md` — index types and seekability
- `35_SARGable_Predicates_and_Index_Seekability.md` — why `LIKE` prefix patterns differ
- `26_Window_Functions_SQL_Server.md` — ranking results after search
