# Meet-in-the-Middle

**Meet-in-the-middle** splits a set of **n** items into **two halves** of size **≈ n/2**, enumerates **subset sums** (or states) for each half, then **combines** with **two pointers** or **hash map**—turns **O(2^n)** into **O(2^(n/2))** for many subset problems.

---

## Classic: subset sum target

1. Split items into **A**, **B**.
2. Enumerate **all sums** of subsets of **A** into array **SA** (size **2^(n/2)**).
3. Enumerate subset sums of **B** into **SB**; sort one side.
4. For each **x** in **SA**, binary search **`target - x`** in **SB**.

---

## Memory

Often **exponential in n/2**—feasible only up to **~40–42** total n in contests.

---

## Related

- [Bit Manipulation](23_Bit_Manipulation.md)
- [Divide and Conquer](18_Divide_and_Conquer.md)
