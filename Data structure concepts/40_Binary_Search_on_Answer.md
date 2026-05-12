# Binary Search on Answer (Search on Value)

Many problems ask for the **minimum or maximum value** of a parameter such that some **constraint holds**. If **feasible(x)** is **monotone** in **x**, you can **binary search on the answer** even when there is **no sorted array** — only a **predicate**.

---

## Monotone predicate (typical “minimize x” setup)

You want the **smallest** **x** such that **feasible(x) = true**, and:

- If **feasible(x)** is true, then **feasible(y)** is true for all **y ≥ x** (non-decreasing feasibility).

Then the set of good answers is a **suffix** `[x*, ∞)`; binary search finds **x***.

For **“maximize x such that feasible”** you get a **prefix**; adjust comparisons and mid rounding.

---

## Template (lower-bound style)

```text
lo = min_possible_answer
hi = max_possible_answer   // inclusive or exclusive per style

while lo < hi:
    mid = (lo + hi) / 2        // or (lo + hi + 1) / 2 for upper-bound variant
    if feasible(mid):
        hi = mid               // shrink toward smaller feasible
    else:
        lo = mid + 1
return lo
```

**Critical**: Pick **mid** rounding so the loop **terminates** and you land on the **correct** boundary (off-by-one is the #1 bug).

---

## Classic examples

- **Minimum ship capacity** to ship within **D** days.
- **Koko eating bananas**: minimum eating rate so all piles finish in **H** hours.
- **Aggressive cows**: maximize minimum distance between stalls.
- **Split array largest sum**: minimize largest chunk sum with **m** splits.

Each has an **O(n)** (or **O(n log sum)**) **feasible(mid)** check inside **O(log range)** iterations.

---

## Proof sketch you should be able to say

“If **mid** works, any larger capacity also works” → **monotone**; binary search finds the **threshold**.

---

## Pitfalls

1. **Integer overflow**: use **`mid = lo + (hi - lo) / 2`** or **long** for sums.
2. **Wrong mid bias**: `(lo + hi + 1) / 2` vs `(lo + hi) / 2` depends on whether you shrink **lo** or **hi** when equalities hit.
3. **Non-monotone predicate** → binary search is **wrong**; you need DP, greedy, or another structure.

---

## Related

- `14_Searching_Algorithms.md`
- `16_Greedy_Algorithms.md` (compare: greedy constructs answer; BS-on-answer validates threshold)
- `31_Amortized_Analysis_and_Master_Theorem.md`
