# Bloom Filter

A **Bloom filter** is a **probabilistic** set membership structure: it can say “**definitely not in the set**” or “**maybe in the set**,” with **no false negatives** (for standard variants) but **possible false positives**.

## Internals
- Bit array of length **m**, initially zeros.
- **k** independent hash functions map an element to `k` bit positions.
- **Insert**: set all `k` bits to 1.
- **Query**: return “maybe present” iff all `k` bits are 1.

## Trade-offs
- **Space**: far smaller than a hash set for large universes when error rate is acceptable.
- **Deletion**: classic Bloom filters do not support deletes; **Counting Bloom filters** or **Cuckoo filters** address some use cases.

## Choosing Parameters
Given expected **n** insertions and acceptable false positive rate **p**, pick **m** and **k** via standard formulas (calculators exist online).

## Applications
- **Web caches** / CDNs avoiding disk lookups.
- **Database** query avoidance (e.g., **Cassandra** SSTable bloom filters).
- **Distributed systems** gossip metadata (compact membership hints).

## Related Topics
- `08_Hash_Tables.md`
- `36_B_Trees_and_B_Plus_Trees.md` (contrast: exact vs probabilistic structures)
