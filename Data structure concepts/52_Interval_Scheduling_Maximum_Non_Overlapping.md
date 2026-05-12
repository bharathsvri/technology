# Interval Scheduling — Maximum Non-Overlapping Intervals

Given intervals **[start, end]**, pick the **maximum cardinality** subset with **no overlaps**—solved greedily by **earliest finishing** intervals.

---

## Greedy rule

Sort by **end time** ascending; iterate, **take** interval if its **start ≥ last chosen end**.

---

## Proof sketch (interview)

Any optimal solution can be **transformed** to match the greedy choice at the first decision point without reducing count—**exchange argument**.

---

## Variants

- **Weighted** interval scheduling → **DP**, not pure greedy.
- **Minimum rooms** for all meetings → **heap** on end times (different problem).

---

## Complexity

**O(n log n)** for sort + **O(n)** scan.

---

## Related

- [Greedy Algorithms](16_Greedy_Algorithms.md)
- [Merging Intervals and Line Sweep](43_Merging_Intervals_and_Line_Sweep.md)
