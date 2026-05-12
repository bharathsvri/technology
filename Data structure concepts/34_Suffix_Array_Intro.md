# Suffix Array — Introduction

A **suffix array** is a sorted list of the starting positions of all suffixes of a string. Together with an **LCP (longest common prefix) array**, it powers many **string** interview problems and bioinformatics indexes.

---

## What is a suffix array?

For text **T** of length **n**, list all suffixes **T[i..n-1]** for **i = 0..n-1**. Sort them lexicographically. The **suffix array SA** stores the **starting indices** in sorted order.

A **sentinel** `$` (unique, smallest) is often appended so no suffix is a prefix of another.

---

## LCP array

**Height array / LCP**: `LCP[k]` = longest common prefix of the **k-th** and **(k-1)-th** sorted suffixes.

**Why useful?** Many combinatorial string questions reduce to **range max query** on LCP or scanning LCP once.

---

## Construction complexity (interview)

| Algorithm | Time | Notes |
|-----------|------|--------|
| Naive sort suffixes | **O(n² log n)** | bad comparator cost |
| Doubling / prefix ranking | **O(n log n)** | common teaching construction |
| SA-IS / skew | **O(n)** | heavy to implement cold |

---

## Applications (name them)

1. **Longest repeated substring** — max LCP.
2. **Longest common substring** of two strings — concatenate with separators; max LCP crossing boundary.
3. **Distinct substrings** — formula using LCP sums (classic exercise).

---

## Comparison to trie of suffixes

**Suffix trie** intuitive but huge memory. **Suffix array + LCP** is compact; **FM-index** goes further for DNA-scale search.

---

## Related

- `21_String_Algorithms.md`, `44_Bit_Trie_XOR_Maximum.md` (different domain), `28_Advanced_Graph_Algorithms.md`
