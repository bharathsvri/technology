# `OPTION (RECOMPILE)` and Parameter Sniffing (Sketch)

**Parameter sniffing** means the optimizer **uses literal values** from the **first compilation** of a parameterized plan—great when representative, harmful when **skewed** distributions cause **bad plan reuse**.

---

## Symptoms

- **Fast sometimes**, **slow other times** for same procedure with different parameter values.
- **Plan** in cache optimized for **atypical** parameter.

---

## `OPTION (RECOMPILE)`

Forces **recompile** each execution—can fix sniffing at **cost** of CPU for **high-frequency** tiny queries.

**Scoped** use inside problematic procedures vs global settings.

---

## Alternatives (name-drop)

- **`OPTIMIZE FOR UNKNOWN`** / **`OPTIMIZE FOR (@p = value)`**
- **`Query Store` forced plans** (`17_Execution_Plans_and_Query_Store.md`)
- **Separate procedures** per workload shape (last resort)

---

## Related

- [Execution Plans and Query Store](17_Execution_Plans_and_Query_Store.md)
- [Stored Procedures](01_Stored_Procedures.md)
