# Pattern Matching for `switch` — Exhaustiveness (Java 21+)

**Pattern matching for `switch`** lets you **`switch` on types** and **destructured** shapes with **`case` labels**. Compilers can warn on **non-exhaustive** switches over **sealed** hierarchies—useful for **domain events** and **`Result`**-like types.

---

## Sealed + switch

When **`switch`** covers all permitted subtypes, **`default`** may be **unreachable** or unnecessary—verify with your **JDK** release notes and **lint** settings.

---

## `null` handling

Decide explicitly: **`case null`** vs guarding **before** `switch`—avoid **surprise** NPE paths.

---

## vs visitor pattern

Pattern matching can **replace** verbose **visitor** hierarchies for **localized** dispatch—still weigh **open** vs **closed** extensibility.

---

## Related

- [Pattern Matching](41_Pattern_Matching.md)
- [Sealed Classes](40_Sealed_Classes.md)
