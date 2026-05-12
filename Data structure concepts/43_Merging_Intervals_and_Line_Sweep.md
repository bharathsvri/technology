# Merging Intervals and Line Sweep Patterns

Given a list of intervals **[l, r]**, common tasks are: **merge overlaps**, find **gaps**, compute **maximum overlap depth**, or build a **union length**.

## Merge Intervals (Classic)
1. **Sort** by start time `l` (tie-break by `r`).
2. Sweep left to right maintaining **`current`** merged interval.
3. If next interval **overlaps** `current` (`next.l <= current.r` with inclusive semantics), **extend** `current.r = max(current.r, next.r)`; else **push** `current` and start a new one.

**Time**: **O(n log n)** from sorting, **O(n)** sweep.

## Union Length
After merging, sum **`r - l`** (with consistent half-open vs closed interval conventions).

## Maximum Concurrent Events
Convert to **point events** `(time, +1/-1)` and sweep—see `38_Sweep_Line_Algorithm.md`.

## Common Pitfalls
- **Inclusive vs half-open** `[l, r)` avoids off-by-one in many implementations.
- Floating-point boundaries—prefer integers or rational types.

## Related Topics
- `38_Sweep_Line_Algorithm.md`
- `16_Greedy_Algorithms.md`
- `19_Two_Pointers_Technique.md`
