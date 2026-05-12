# Big-O, Big-Ω, and Big-Θ

Interviewers expect you to classify algorithms with **asymptotic notation** and to distinguish **worst**, **average**, and **amortized** bounds.

---

## Big-O (upper bound)

**f(n) = O(g(n))**: beyond some **n₀**, **f(n) ≤ c·g(n)** for constant **c > 0**.

Meaning: **g** is an **upper bound** on growth rate (not tight necessarily).

---

## Big-Ω (lower bound)

**f(n) = Ω(g(n))**: **f** grows **at least** as fast as **g** (definitions vary slightly between textbooks for “for all n” vs “infinitely often”; know your course’s version).

**Example**: Any comparison sort needs **Ω(n log n)** comparisons in the **worst case** over general totally ordered inputs.

---

## Big-Θ (tight bound)

**f(n) = Θ(g(n))** when **f** is both **O(g)** and **Ω(g)** — same growth rate up to constants.

Merge sort worst case: **Θ(n log n)**.

---

## Little-o / ω (strict)

- **o(g)**: strictly **smaller** growth than **g**.
- **ω(g)**: strictly **larger**.

---

## Common growth hierarchy

**O(1) ⊂ O(log n) ⊂ O(n) ⊂ O(n log n) ⊂ O(n²) ⊂ O(2^n)**

Drop lower-order terms and constants: **3n² + 100n → O(n²)**.

---

## Pitfalls for interviews

1. **Asymptotic ≠ fast in practice** — constants and cache matter at real **n**.
2. **Hash table “O(1)”** is **average**; worst can be **O(n)** with collisions.
3. **State worst vs amortized** for **Union–Find**, **dynamic array**, **hash map resize**.

---

## Related

- `31_Amortized_Analysis_and_Master_Theorem.md`, `13_Sorting_Algorithms.md`
