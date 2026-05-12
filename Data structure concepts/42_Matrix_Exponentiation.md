# Matrix Exponentiation

**Matrix exponentiation** computes **M^n** for a **k × k** matrix in **O(k³ log n)** using **binary exponentiation** (same doubling idea as fast integer power). It encodes **linear recurrences** and **bounded-state transitions**.

---

## Binary exponentiation on matrices

```text
result = I (identity)
base = M
while n > 0:
    if n is odd: result = result * base
    base = base * base
    n /= 2
```

Each **multiply** is **O(k³)** naively; **k** is often small (2–5) in contest/interview problems.

---

## Fibonacci (2×2 form)

One standard formulation:

\[
\begin{pmatrix} F_{n+1} & F_n \\ F_n & F_{n-1} \end{pmatrix}
=
\begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix}^n
\]

(Layouts differ across sources — **verify** before coding.)

**Complexity**: **O(log n)** multiplies of **2×2** → **O(1)** per multiply → **O(log n)** time.

---

## Graph: walks of length L

If **A** is an adjacency matrix (**A[u][v] = 1** if edge **u→v**), then **(A^L)[i][j]** counts **walks** of length **L** from **i** to **j** (combinatorially; watch multigraph conventions).

**Cost**: **O(V³ log L)** dense — only feasible for **small V** or sparse tricks elsewhere.

---

## Modular arithmetic and overflow

- Reduce mod **p** after every add/mul in contests.
- Use **64-bit** intermediates or language **bigint** when products overflow.

---

## When to mention in interviews

- “**Linear recurrence** with huge **n**” → matrix form + fast pow.
- “**DP over n steps** with **fixed small state**” → sometimes linear transform + exponentiation.

---

## Related

- `15_Dynamic_Programming.md`
- `23_Bit_Manipulation.md` (binary exponent pattern on integers)
- `28_Advanced_Graph_Algorithms.md`
