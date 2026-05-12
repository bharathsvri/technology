# Binary Search on Answer (Search on Value)

Many optimization problems ask: “**What is the minimum/maximum feasible value** satisfying a predicate?” If the predicate is **monotonic** in the parameter, you can **binary search the answer** even when the search space is not an array index.

## Pattern
1. Define `feasible(x)` → boolean.
2. Find **low, high** bounds on the answer.
3. While `low < high`, pick `mid` and shrink interval using monotonicity.

## Classic Examples
- **Minimum capacity ship** within `D` days (LeetCode style).
- **Koko eating bananas** minimum speed.
- **Aggressive cows** (largest minimum distance).
- **Smallest subarray length** with sum ≥ S sometimes uses sliding window instead—know both patterns.

## Why It Works
If `feasible(x)` is false and `feasible(y)` is true for `y > x` (monotone increasing threshold), the breakpoint can be found with binary search.

## Pitfalls
- **Integer overflow** in `mid = (low + high + 1) / 2` vs `(low + high) / 2` depending on lower/upper bound search variant.
- Proving **monotonicity** is essential—otherwise binary search lies.

## Related Topics
- `14_Searching_Algorithms.md`
- `31_Amortized_Analysis_and_Master_Theorem.md`
- `16_Greedy_Algorithms.md` (compare greedy vs binary search on answer)
