# Rabin–Karp Rolling Hash

**Rabin–Karp** finds pattern occurrences using a **rolling hash**—expected **O(n+m)** on random data, worst **O(nm)** if hashes collide frequently (mitigated by **double hash** or **verification**).

---

## Idea

Compare **hash(pattern)** to **hash(every window of length m)** in text; on **match**, **verify** characters to avoid false positives.

---

## Rolling update

Update hash from window **[i..i+m)** to **[i+1..i+m+1)** in **O(1)** using base power precomputation (similar to sliding numeric fingerprint).

---

## vs KMP / Z-function

- **Multiple patterns** with shared rolling machinery (extension).
- **Single pattern**, no wildcards: **KMP** deterministic **O(n+m)**.

---

## Related

- [String Algorithms](21_String_Algorithms.md)
- [Z-Algorithm](46_Z_Algorithm.md)
