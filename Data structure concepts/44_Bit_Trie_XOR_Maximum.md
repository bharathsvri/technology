# Trie XOR Maximum (Bitwise Trie)

A **binary trie** (bit trie) stores integers bit-by-bit from **most significant** to **least significant**. It supports **O(B)** operations per insert/query where **B** is bit width (often 32).

## Classic Problem: Maximum XOR Pair
Given array **A**, find **max(A[i] XOR A[j])**.

Approach:
1. Build a bit trie incrementally (or of the whole set).
2. For each value **x**, query the trie for the **best partner** that flips as many high bits as possible toward **1** in `x XOR y`.

## Node Structure (Conceptual)

```text
children[0], children[1]  // next bit 0 or 1
```

Insert follows bits of the number; query greedily chooses the opposite bit when available to maximize XOR at that position.

## Variants
- **Subarray max XOR** with prefix XOR + trie.
- **Minimum XOR** by flipping greedy choice.

## Complexity
- **O(n · B)** for fixed **B** (32/64).

## Related Topics
- `10_Tries.md`
- `23_Bit_Manipulation.md`
