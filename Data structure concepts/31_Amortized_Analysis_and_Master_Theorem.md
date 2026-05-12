# Amortized Analysis and the Master Theorem

## Amortized Analysis
**Amortized complexity** averages the cost of a sequence of operations, even if a single operation is occasionally expensive.

### Classic Example: Dynamic Array Doubling
Appending to a vector that doubles capacity when full:
- Most appends are **O(1)**.
- Occasionally resizing is **O(n)**.
- Over **n** appends, total work is **O(n)**, so **amortized O(1)** per append.

### Techniques
1. **Aggregate method**: bound total cost / number of operations.
2. **Accounting (banker's) method**: assign credits so expensive steps spend stored credits.
3. **Potential method**: define a potential function Φ on state such that real_cost + ΔΦ is small each step.

### Where It Appears
- **Union-Find** with path compression + union by rank: inverse Ackermann amortized.
- **Splay trees**: amortized logarithmic bounds.
- Hash table resizing policies.

## Master Theorem (Divide-and-Conquer Recurrences)
For recurrences of the form:

\[
T(n) = a\,T(n/b) + f(n),\quad a \ge 1,\ b > 1
\]

Compare \(f(n)\) with \(n^{\log_b a}\):

1. If \(f(n) = O(n^{\log_b a - \varepsilon})\) for some \(\varepsilon > 0\):  
   **\(T(n) = \Theta(n^{\log_b a})\)** (leaves dominate).
2. If \(f(n) = \Theta(n^{\log_b a} \log^k n)\):  
   **\(T(n) = \Theta(n^{\log_b a} \log^{k+1} n)\)**.
3. If \(f(n) = \Omega(n^{\log_b a + \varepsilon})\) and regularity holds:  
   **\(T(n) = \Theta(f(n))\)** (work at root dominates).

### Examples
- **Merge Sort**: \(T(n)=2T(n/2)+O(n)\) → case 2 → **\(O(n \log n)\)**.
- **Binary search recursion**: \(T(n)=T(n/2)+O(1)\) → **\(O(\log n)\)**.

### Limitations
Does not cover all recurrences (e.g., unequal splits like \(T(n)=T(n/3)+T(2n/3)+n\) need recursion trees or Akra–Bazzi).

## Practice Checklist
- State whether a bound is **worst-case**, **amortized**, or **average-case** (hash tables).
- For recurrences, try **Master theorem** first; if it fails, draw a **recursion tree**.

## Related Topics
- `18_Divide_and_Conquer.md`
- `24_Union_Find_Disjoint_Set.md`
- `13_Sorting_Algorithms.md`
