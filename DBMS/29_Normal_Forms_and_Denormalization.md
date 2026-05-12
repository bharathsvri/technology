# Normal Forms (1NF–3NF) and Denormalization Trade-offs

**Normalization** reduces **redundancy** and **anomalies**; **denormalization** trades duplicate data for **read performance**—OLTP vs reporting shapes the answer.

---

## First Normal Form (1NF)

- **Atomic** columns (no repeating groups / comma lists in one column).
- Each row is **unique** with a **primary key**.

---

## Second Normal Form (2NF)

- **1NF** plus every **non-key** column depends on the **whole** composite key (not a part).

---

## Third Normal Form (3NF)

- **2NF** plus no **transitive** dependency: non-key columns depend **only** on the key, not on each other.

**Example violation**: `EmployeeId → DeptId → DeptName`—**DeptName** should live in **`Departments`**.

---

## When to denormalize

- **Read-heavy** dashboards with stable dimensions.
- **Controlled duplication** with **triggers** or **ETL** to keep consistency.

---

## Related

- [Constraints](08_Constraints.md)
- [Views](05_Views.md)
