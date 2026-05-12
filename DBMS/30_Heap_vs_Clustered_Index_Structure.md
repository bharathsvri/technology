# Heaps, Clustered Indexes, and Table Structure

In **SQL Server**, **heap** = table **without** a clustered index; **clustered index** = table rows are **sorted/stored** in **B-tree** key order (typically the **primary key**).

---

## Heap characteristics

- **Inserts** can be fast at end-of-file patterns; **selective lookups** without supporting NC indexes are **scans**.
- **Forwarded records** can occur after wide updates (heap maintenance concern).

---

## Clustered index

- **Range scans** on clustering key are efficient.
- **Choose clustering key** wisely: **narrow**, **stable**, **unique**, **ever-increasing** vs random GUID trade-offs (fragmentation vs insert locality).

---

## Interview soundbites

- **“Clustered index is the table”** for most designs.
- **Nonclustered index** leaf points to **RID** (heap) or **clustering key** (clustered table).

---

## Related

- [Indexes](07_Indexes.md)
- [Execution Plans and Query Store](17_Execution_Plans_and_Query_Store.md)
