# Bloom Filter

A **Bloom filter** is a **space-efficient, probabilistic** structure for **set membership**: it answers **“maybe in the set”** or **“definitely not in the set.”** Standard Bloom filters have **no false negatives** (if you inserted **x**, query always says “maybe”) but **allow false positives** (“maybe” when **x** was never inserted).

---

## When interviewers bring it up

- **Disk / network avoidance**: skip expensive lookups when “probably absent.”
- **Distributed storage**: SSTables, block indexes, peer metadata.
- Trade-off: **memory vs accuracy** — you choose **m** (bits) and **k** (hashes).

---

## Internals

1. Bit array of length **m**, all **0**.
2. **k** hash functions **h₁…hₖ** map each element to **k** positions in `[0, m)`.
3. **Insert(x)**: set bits **h₁(x)…hₖ(x)** to **1**.
4. **Query(x)**: return **“maybe”** iff **all** those bits are **1**; else **“definitely not.”**

If any bit is **0**, **x** was never inserted (assuming no deletions in classic form).

---

## False positive rate (intuition)

After **n** inserts, a given bit is still **0** with probability roughly **(1 − 1/m)^(k·n)**. Optimal **k** and **m** for target **p** and **n** follow standard formulas; in interviews, saying **“tune m and k for acceptable p”** is often enough unless they ask for the math.

---

## Trade-offs and limitations

| Topic | Detail |
|--------|--------|
| **Space** | Much smaller than an exact hash set for huge universes when **p** is OK |
| **Deletion** | Classic: **no** safe delete (setting bits to 0 breaks other keys). **Counting Bloom** / **Cuckoo filter** for some delete needs |
| **Iteration** | Cannot list members |
| **Equality** | Two filters built differently are not trivially mergeable unless designed for it |

---

## Applications (soundbite-ready)

- **Cassandra / RocksDB**-style: reduce I/O by skipping files that cannot contain a key.
- **CDN / cache**: “do we have this URL’s body on disk?”
- **Crawler**: skip URLs probably seen before.

---

## Related

- `08_Hash_Tables.md`
- `36_B_Trees_and_B_Plus_Trees.md` (exact indexing vs probabilistic membership)
