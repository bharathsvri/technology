# Matrix Exponentiation

**Matrix exponentiation** computes **M^n** in **O(k³ log n)** for **k×k** matrices using **binary exponentiation**—essential for linear recurrences (Fibonacci), graph **walk counts**, and some DP optimizations.

## Binary Exponentiation Idea

```text
M^n = M^(binary expansion)
```

Multiply running result when the corresponding bit of **n** is set—same pattern as fast integer power.

## Fibonacci Example
\[
\begin{pmatrix} F_{n+1} & F_n \end{pmatrix}
=
\begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix}^n
\begin{pmatrix} 1 & 0 \end{pmatrix}
\]
(Exact layout conventions vary—verify matrix formulation for your recurrence.)

## Graph: Paths of Length L
If **A** is an adjacency matrix (0/1 or weighted with counting semantics), **(A^L)[i][j]** counts **walks** of length **L** from **i** to **j** in **O(V³ log L)** dense form—often too heavy; use when **V** is small.

## Modular Arithmetic
When answers require **mod p**, reduce every multiply/add to avoid overflow; use **`long`** intermediates or `BigInteger` for contests.

## Related Topics
- `15_Dynamic_Programming.md`
- `23_Bit_Manipulation.md` (binary exponent pattern)
- `28_Advanced_Graph_Algorithms.md`
