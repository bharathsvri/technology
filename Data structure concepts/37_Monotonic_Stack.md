# Monotonic Stack

A **monotonic stack** keeps stack entries strictly **increasing** or **decreasing** (bottom → top) while scanning a sequence. Classic **O(n)** tool for **nearest greater/smaller element** problems.

---

## Increasing vs decreasing

- **Decreasing stack** (from bottom): often used for **next greater element** (scan right-to-left or left-to-right depending on variant).
- **Increasing stack**: **next smaller** patterns.

---

## Template: next greater element (right)

Scan **indices** from **right to left** with a **decreasing** stack of indices:

1. Pop while `a[stack.top] <= a[i]` (cannot be answer for **i**).
2. If stack empty → no greater to the right; else answer is `a[stack.top]`.
3. Push **i**.

**Time O(n)** — each index pushed/popped at most once.

---

## “Daily temperatures” pattern

Monotonic stack on **values** or **indices** while scanning left-to-right to compute **days until warmer**.

---

## Pitfalls

- Off-by-one on **strict vs non-strict** inequalities (`>` vs `>=`) changes duplicate handling.
- For **circular array** NGE, duplicate array or scan **2n** with stack discipline.

---

## Related

- `03_Stacks.md`, `19_Two_Pointers_Technique.md`, `20_Sliding_Window.md`
