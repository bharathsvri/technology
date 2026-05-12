# Z-Algorithm (Z-Function)

The **Z-array** for string **S** stores **Z[i]** = length of longest substring starting at **i** that matches a **prefix** of **S**. Computed in **O(|S|)**, it powers **linear** substring search and many string problems.

---

## Definition

- **Z[0]** is often defined as **0** or **|S|** depending on textbook; implementations align with the problem (consistency matters in code).

For **i > 0**: **Z[i]** = max **k** such that **S[0..k) = S[i..i+k)**.

---

## Classic use: find pattern **P** in text **T**

Build **S = P + '$' + T`** (separator not in alphabet). Any **Z[i] = |P|** indicates a match at position **`i - |P| - 1`** in **T**.

---

## Linear construction idea

Maintain **interval [L,R]** where **S[L..R]** matches **S[0..R-L]**. When computing **Z[i]**:

- If **i > R**, brute-extend from scratch, update **L,R**.
- Else use **mirror** information from **Z[i-L]** to minimize comparisons, then extend if needed.

---

## Applications

- **Prefix–suffix** enumeration for string DP.
- **Repeated substring** detection.

---

## Related

- [String Algorithms](21_String_Algorithms.md)
- [Manacher's Algorithm](29_Manacher_Algorithm.md)
