# Aho–Corasick Multi-Pattern Matching

**Aho–Corasick** finds **all occurrences** of **multiple patterns** in a text in **O(n + m + z)** where **n** is text length, **m** is total pattern length, and **z** is number of matches—ideal for **plagiarism filters**, **IDS signatures**, and **censor lists**.

---

## Idea

Generalize a **trie** of patterns with **failure links** (like KMP’s failure function on a trie): on mismatch, fall back to the **longest proper suffix** that is still a trie prefix—**without** rescanning the text.

---

## Construction (sketch)

1. Build **trie** of all patterns; mark **output** at terminal nodes (may aggregate multiple patterns ending same place).
2. **BFS** to set **failure links**: for node **u** with char **c**, follow failures until a child **c** exists or hit root.
3. **Output links**: follow failure chain to collect all pattern ids ending at suffixes.

---

## Walk

For each text character, follow **goto** or **failure** until match; emit all pattern ids at current node’s **output** closure.

---

## vs repeated KMP / Rabin–Karp

- **Many patterns, one scan**: AC wins.
- **Single pattern, huge alphabet**: KMP or **Boyer–Moore** variants may be simpler.

---

## Related

- [String Algorithms](21_String_Algorithms.md)
- [Tries](10_Tries.md)
