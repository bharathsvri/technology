# Sweep Line Algorithm

**Sweep line** algorithms process geometric or interval events in sorted order, maintaining a **dynamic structure** (often a balanced BST, indexed tree, or segment tree) of the “active” set.

## Typical Use Cases
- Counting **overlapping intervals** or maximum overlap depth.
- **Rectangle union area/perimeter** (with segment tree or coordinate compression).
- **Closest pair of points** (combine with divide & conquer).

## Core Steps
1. **Create events**: e.g., for interval `[L,R)`, emit `(L, +1)` and `(R, -1)` on a 1D line.
2. **Sort events** by coordinate; break ties consistently (end before start, or vice versa—problem dependent).
3. **Sweep**: update a running aggregate (active count, max height) as events are processed.

## Example Sketch: Max Concurrent Intervals
- Events: start `+1`, end `-1` after sorting by time.
- Track **running sum**; global max is peak concurrency.

## Complexity
Usually **O(n log n)** from sorting; structure updates may add log factors.

## Related Topics
- `16_Greedy_Algorithms.md`
- `26_Segment_Tree.md` (when y-dimension needs range structure)
- `18_Divide_and_Conquer.md`
