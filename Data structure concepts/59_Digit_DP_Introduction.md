# Digit DP (Count Numbers With Digit Constraints)

**Digit DP** counts integers in **[0, N]`** satisfying **digit rules** (no leading zeros, forbidden substrings, sum of digits **≤ K**) by **DP on positional state** while processing **decimal string** of **N** from **MSD** to **LSD**.

---

## States (typical)

- **position** index in digit string.
- **tight** flag: prefix so far equals **N** prefix (future digits bounded).
- **started** flag: whether number has begun (leading zero handling).
- extra masks: **even/odd**, **modulo** remainder.

---

## Complexity

**O(pos × tight × started × … × alphabet)** — practical for **N** up to **10^18** as string length **≤ 19**.

---

## Related

- [Advanced DP Patterns](30_Advanced_DP_Patterns.md)
- [Bit Manipulation](23_Bit_Manipulation.md)
