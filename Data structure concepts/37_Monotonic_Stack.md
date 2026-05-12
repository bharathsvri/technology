# Monotonic Stack

A **monotonic stack** keeps stack elements in **strictly increasing** or **strictly decreasing** order while scanning a sequence. It is the right tool for “**nearest greater/smaller element**” families in **O(n)**.

## Increasing vs Decreasing
- **Monotonic increasing stack** (bottom → top): often used to find **previous smaller** or **next smaller** depending on scan direction.
- **Monotonic decreasing stack**: often used for **next greater element**.

## Classic Pattern: Next Greater Element
For each index `i`, find smallest `j>i` with `a[j] > a[i]` (or report none).

Algorithm (right-to-left scan with decreasing stack storing indices):
1. Initialize empty stack.
2. Iterate `i` from `n-1` down to `0`:
   - While stack not empty and `a[stack.top] <= a[i]`, **pop** (cannot be answer for `i`).
   - If stack empty, no greater to the right; else answer is `a[stack.top]`.
   - **Push** `i`.

## Daily Temperatures Style Problems
Maintain decreasing stack of indices by **temperature value** while scanning left-to-right to compute **wait days** until warmer.

## Complexity
- Each index **pushed and popped at most once** → **O(n)** time, **O(n)** stack space.

## Related Topics
- `03_Stacks.md`
- `19_Two_Pointers_Technique.md`
- `20_Sliding_Window.md`
