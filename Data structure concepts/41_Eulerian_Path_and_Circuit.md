# Eulerian Path and Eulerian Circuit

An **Eulerian trail** (path) uses **every edge exactly once**. An **Eulerian circuit** is a closed trail: same start and end vertex.

This is **about edges**, not vertices (unlike Hamiltonian path, which is NP-hard in general).

---

## Undirected graph — degree conditions

Let **deg(v)** be the number of incident edges (loops counted per convention in your course).

Assume the graph is **connected** after removing **isolated** vertices (no edges).

| Goal | Condition |
|------|-----------|
| **Eulerian circuit** | Every vertex has **even** degree |
| **Eulerian path** (not circuit) | Exactly **two** vertices have **odd** degree (they are the endpoints) |

**Intuition**: every time you enter a vertex you must leave it, except endpoints.

---

## Directed graph

Let **in(v)**, **out(v)** be indegree/outdegree.

| Goal | Condition (on weak connectivity of non-zero degree part) |
|------|-----------------------------------------------------------|
| **Eulerian circuit** | **in(v) = out(v)** for all **v** |
| **Eulerian path** | One **start**: **out = in + 1**; one **end**: **in = out + 1**; all others balanced |

---

## Hierholzer’s algorithm (sketch)

1. Pick a valid **start** (any vertex for undirected circuit if all degrees even).
2. Follow **unused** edges greedily until you **return** to the start of this walk → a **cycle** (or path fragment).
3. If unused edges remain, find a vertex on the current tour that still has unused edges; **splice** in another cycle.
4. Repeat until all edges used.

**Time**: **O(E)** with adjacency lists and marking edges as used (or removing from lists).

---

## Applications

- **Chinese Postman** (minimum route covering all edges with repeats).
- “Draw without lifting the pen” puzzles.
- **De Bruijn** sequences (advanced).

---

## Related

- `07_Graphs.md`
- `28_Advanced_Graph_Algorithms.md`
