# Merging Intervals and Line Sweep Patterns

Intervals **[l, r]** appear constantly in scheduling, resource allocation, and geometry prep. Master **merge**, **union length**, and **event sweep** for overlap depth.

---

## Merge overlapping intervals

1. **Sort** by **start** **l** (tie-break by **r** if you like).
2. Maintain **`current`** = first interval.
3. For each **next**:
   - If **next.l ≤ current.r** (for **closed** **[l,r]**; adjust for half-open), they **overlap** → set **`current.r = max(current.r, next.r)`**.
   - Else **push** `current`, set **`current = next`**.

**Time**: **O(n log n)** sort + **O(n)** scan.

---

## Union length

After merging, sum **(r − l)** (or half-open **r − l** without +1). Keep **closed vs [l,r)** consistent everywhere.

---

## Insert interval into a merged list

If intervals stay sorted and disjoint, **binary search** insertion point then **merge** neighbors — useful for streaming APIs.

---

## Maximum concurrent / overlap depth

**Line sweep** on events:

- **(l, +1)** start
- **(r, −1)** end — for **closed** **[l,r]**, use **(r+1, −1)** or prefer **half-open [l, r+1)** to avoid fence errors.

Sort events, sweep running **active** count, track **max**.

See also `38_Sweep_Line_Algorithm.md`.

---

## Common pitfalls

- **Inclusive endpoints**: `[1,2]` and `[2,3]` overlap at point **2**.
- **Floating-point** boundaries → prefer integers or rationals.
- **Empty list** and **single interval** edge cases.

---

## Related

- `38_Sweep_Line_Algorithm.md`
- `16_Greedy_Algorithms.md`
- `19_Two_Pointers_Technique.md`
