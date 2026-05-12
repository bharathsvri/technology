# Sweep Line Algorithm

**Sweep line** algorithms process **events** (interval start/end, segment endpoints) in sorted order while maintaining a **dynamic structure** of the active set.

---

## Typical problems

- Maximum **overlap** of intervals on 1D line.
- **Rectangle union** area (often + segment tree / coordinate compression on y).
- **Closest pair of points** (combine with divide & conquer in some solutions).

---

## 1D interval overlap depth

Convert each interval `[L,R)` to events:

- `(L, +1)` start
- `(R, -1)` end

Sort by coordinate; break ties consistently (**end before start** avoids over-counting depending on problem statement).

Sweep cumulative sum **active**; track **max**.

**Time**: **O(n log n)** for sort + **O(n)** sweep.

---

## Geometry sweep

Sort events by **x**, maintain **active segments** in a balanced structure keyed by **y** order for intersections—harder; know it exists for “advanced” interviews.

---

## Implementation tips

- Use **half-open** intervals `[L,R)` to reduce endpoint bugs.
- **Coordinate compression** when coordinates are huge but **few** distinct values.

---

## Related

- `43_Merging_Intervals_and_Line_Sweep.md`, `16_Greedy_Algorithms.md`, `26_Segment_Tree.md`
