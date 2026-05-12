# Amortized Analysis and the Master Theorem

Interviewers use **amortized analysis** to justify that a *sequence* of operations is cheap *on average*, even if one operation is occasionally expensive. The **Master Theorem** gives closed forms for common **divide-and-conquer** recurrences.

---

## 1. Amortized analysis — why “average” is not “expected value”

- **Worst-case per operation**: bound on a *single* call.
- **Amortized per operation**: total cost of **n** operations ÷ **n**, for a *specific algorithm and operation mix*.

### Classic example: dynamic array doubling

Append is usually **O(1)**. When capacity is full, allocate **2×** array and copy → **O(n)** for that append.

Over **n** appends starting from empty:

- Copies form a geometric series: **n + n/2 + n/4 + … < 2n** element moves.
- Total work **O(n)** ⇒ **amortized O(1)** per append.

### Three standard proof styles

| Method | Idea |
|--------|------|
| **Aggregate** | Prove total work ≤ **c·n**, divide by **n** |
| **Accounting (banker’s)** | Each cheap op pays **credits**; expensive op spends saved credits |
| **Potential** | Define **Φ(state)**; show `real_cost + ΔΦ` is small each step |

### Interview talking points

- **Union–Find** with path compression + union by rank: **inverse Ackermann** amortized (say “almost constant” in practice).
- **Splay tree**: amortized **O(log n)**; single operations can still be **O(n)** worst case.

---

## 2. Master Theorem (divide & conquer)

For recurrences (constants **a ≥ 1**, **b > 1**):

\[
T(n) = a\,T(n/b) + f(n)
\]

Compare **f(n)** with **n^{log_b a}**:

1. If **f(n) = O(n^{log_b a − ε})** for some **ε > 0** ⇒ **T(n) = Θ(n^{log_b a})** (leaves dominate).
2. If **f(n) = Θ(n^{log_b a} log^k n)** ⇒ **T(n) = Θ(n^{log_b a} log^{k+1} n)**.
3. If **f(n) = Ω(n^{log_b a + ε})** and **regularity** holds ⇒ **T(n) = Θ(f(n))** (root work dominates).

### Examples you should recite

- **Merge sort**: \(T(n)=2T(n/2)+O(n)\) ⇒ case 2 ⇒ **Θ(n log n)**.
- **Binary search** (one subproblem): \(T(n)=T(n/2)+O(1)\) ⇒ **Θ(log n)**.
- **Strassen-style** (if you mention it): three subproblems change **log_b a**.

### Master theorem limitations

- Unequal splits like \(T(n)=T(n/3)+T(2n/3)+n\) → use **recursion tree** or **Akra–Bazzi**.

---

## 3. What to say in an interview

1. Define whether the bound is **worst**, **amortized**, or **average** (hash tables).
2. For recurrences: try Master theorem; if it fails, sketch a **recursion tree** and sum per level.

### Practice prompts

- Why is `ArrayList` append amortized **O(1)**?
- Why does **binary heap** `build_heap` run in **O(n)** even though `insert` is **O(log n)** each?

---

## Related

- `18_Divide_and_Conquer.md`, `13_Sorting_Algorithms.md`, `24_Union_Find_Disjoint_Set.md`
