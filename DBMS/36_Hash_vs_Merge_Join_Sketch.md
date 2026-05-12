# Hash vs Merge Joins (Execution Plan Concepts)

SQL Server’s optimizer picks **physical join operators**—**Nested Loops**, **Hash Match**, **Merge Join**—based on **cardinality estimates**, **sorted inputs**, and **memory** grants.

---

## Hash Match

- Builds a **hash table** on the smaller input (often).
- Good for **large** unsorted sets with **equi-join**.

**Watch**: **memory spills** to tempdb when grant too small.

---

## Merge Join

- Requires **sorted** inputs on join keys.
- Efficient when **both** sides are already ordered (index seek + merge).

---

## Nested Loops

- Great when **outer** is small or **inner** has **index seek per outer row**.

---

## Interview takeaway

“Bad **estimates** → wrong **join** choice → performance cliff; fix **stats** / **parameter sniffing** / **query shape**.”

---

## Related

- [Execution Plans and Query Store](17_Execution_Plans_and_Query_Store.md)
- [Parameter Sniffing and OPTION RECOMPILE](34_Parameter_Sniffing_and_OPTION_RECOMPILE.md)
