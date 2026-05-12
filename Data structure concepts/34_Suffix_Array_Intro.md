# Suffix Array and Suffix Tree (Introduction)

## Motivation
Many **string** problems—repeated substrings, longest common substring across strings, pattern occurrences with preprocessing—benefit from indexing all suffixes.

Two classic structures:
- **Suffix trie/tree**: intuitive but heavy on memory for large alphabets.
- **Suffix array**: compact, powerful with companion **LCP** array.

## Suffix Array Definition
For a string **S** of length **n**, the **suffix array SA** is a permutation of indices `0..n-1` sorting all suffixes **lexicographically**.

Example: `S = "banana$"` (sentinel `$` smaller than letters)

Sorted suffixes begin at indices ending with SA like `[5,3,1,0,4,2]` (illustrative—verify locally).

## Longest Common Prefix (LCP) Array
**LCP[i]** = length of longest common prefix between `SA[i]` and `SA[i-1]`.

Enables linear-time solutions to many problems **after O(n log n) or O(n)** construction, depending on algorithm.

## Construction Sketches
1. **Naive**: sort `n` suffixes with `O(n)` comparator → **O(n² log n)**—too slow for large **n**.
2. **Doubling / prefix ranking**: iterative ranking of length-`2^k` prefixes → **O(n log n)**.
3. **SA-IS / skew** (advanced): **O(n)**; implementation-heavy.

## Typical Use Cases
- **Longest repeated substring** (max LCP).
- **Longest common substring** of two strings: build suffix array on `S1#S2$` and restrict LCP to pairs crossing the separator.
- **Pattern matching** with the array + binary search on pattern vs suffix range.

## Practical Note
For competitive programming, **rolling hash** + binary search often substitutes when constraints allow collision risk management. For bioinformatics-scale data, specialized libraries build suffix arrays/FM-indexes.

## Related Topics
- `21_String_Algorithms.md`
- `29_Manacher_Algorithm.md`
