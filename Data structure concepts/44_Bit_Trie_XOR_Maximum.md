# Trie XOR Maximum (Bit Trie)

A **binary trie** stores integers **bit by bit** from **most significant bit (MSB)** to **least (LSB)**. Each level is a decision **0** or **1**. For fixed width **B** (32 or 64), **insert** and **best XOR partner** queries are **O(B)**.

---

## Why MSB first?

**XOR** is maximized by making the **highest differing bit** a **1**, which means at that bit position you want **opposite** bits in **x** and **y**. Greedy walk from MSB: at each bit, take **child[1−bit]** if it exists, else fall back to **same** bit.

---

## Classic: maximum XOR of two numbers in an array

**Approach**: build trie **incrementally** (or of full set). For each **x**, query trie for **y** that maximizes **x XOR y** before inserting **x** (for “pair” problems, order depends on statement).

**Time**: **O(n · B)** with **B = 32** (constant).

---

## Node structure (conceptual)

```text
children[0], children[1]  // next bit 0 or 1; null if absent
```

Optional: **count** at node for “how many numbers pass through here” (subarray / multiset variants).

---

## Subarray maximum XOR

Let **P[i] = A[0] XOR … XOR A[i-1]** (prefix XOR). Then **A[l..r] XOR = P[r+1] XOR P[l]**.

For each index, maintain trie of **previous prefixes** and query best partner for **current prefix** → **O(n · B)**.

---

## Variants

- **Minimum XOR pair**: greedy **same** bit when possible (dual rule).
- **XOR with bound** / **count pairs** with XOR **≤ K** → trie + counting on branches (harder).

---

## Pitfalls

- **Sign bits** for signed integers: often treat as **unsigned 32-bit** for trie walking.
- **Duplicate** values: trie with **counts** avoids wrong “partner is self” logic when needed.

---

## Related

- `10_Tries.md`
- `23_Bit_Manipulation.md`
