# Asymptotic Notation: Big-O, Big-Ω, Big-Θ

## Motivation
We classify algorithms by how **cost grows** with input size **n**—time steps, memory cells, comparisons—using asymptotic notation.

## Big-O (Upper Bound)
**O(f(n))**: for large **n**, cost grows **no faster than** a constant multiple of **f(n)** (worst-case or safe upper bound, depending on context).

Examples:
- Linear scan of array: **O(n)**.
- Balanced BST search: **O(log n)**.

## Big-Ω (Lower Bound)
**Ω(f(n))**: cost grows **at least** as fast as **f(n)** (for some definition variant—algorithms vs problems differ in formal courses).

Example: comparison-based sorting requires **Ω(n log n)** **problem** lower bound in the general case.

## Big-Θ (Tight Bound)
**Θ(f(n))**: cost is **sandwiched** between matching upper and lower bounds—same growth rate up to constants.

If an algorithm is **O(n log n)** and **Ω(n log n)** for a given input model, it is **Θ(n log n)**.

## Little-o / Little-ω (Optional)
- **o(f)**: strictly grows slower than **f**.
- **ω(f)**: strictly grows faster than **f**.

## Common Classes (Increasing Growth)
**O(1) ⊂ O(log n) ⊂ O(n) ⊂ O(n log n) ⊂ O(n²) ⊂ O(2^n)**

Constants and lower-order terms are dropped: **3n² + 100n → O(n²)**.

## Worst / Average / Best Case
- **Worst-case** bounds protect against adversarial inputs (e.g., quicksort pivot choices).
- **Average-case** may differ (hash tables with good hash distribution).
- **Amortized** bounds average expensive steps over a sequence (see `31_Amortized_Analysis_and_Master_Theorem.md`).

## Pitfalls
- Asymptotics ignore **constants** that dominate at real **n**.
- **Memory hierarchy** (cache misses) may dominate long after asymptotic winner is clear on paper.

## Related Topics
- `31_Amortized_Analysis_and_Master_Theorem.md`
- `01_Arrays.md` (often introduces Big-O first)
- `13_Sorting_Algorithms.md`
