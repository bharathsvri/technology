# Eulerian Path and Eulerian Circuit

An **Eulerian trail (path)** visits **every edge exactly once**. An **Eulerian circuit** is a closed trail (starts and ends at the same vertex).

## Undirected Graph Conditions
Let **deg(v)** be vertex degree (number of incident edges).

- **Eulerian circuit** exists iff the graph is **connected** (ignoring isolated vertices) and **every vertex has even degree**.
- **Eulerian path (not circuit)** exists iff exactly **two** vertices have **odd** degree (they are the trail endpoints) and the graph is otherwise connected as above.

## Directed Graph Conditions
- **Eulerian circuit**: strongly connected when ignoring zero-degree vertices, and **in-degree(v) = out-degree(v)** for all **v**.
- **Eulerian path**: at most one vertex with **out = in + 1** (start), at most one with **in = out + 1** (end), others balanced.

## Hierholzer’s Algorithm (Sketch)
1. Start from a valid start vertex (any for circuit).
2. Walk along **unused** edges greedily until returning to the start vertex of this walk, forming a **cycle**.
3. Splice cycles until all edges consumed.

**Time**: **O(E)** with adjacency lists and careful edge marking.

## Applications
- **Chinese Postman** problem base case.
- Drawing a figure **without lifting the pen**.
- **De Bruijn** sequences construction (advanced).

## Related Topics
- `07_Graphs.md`
- `28_Advanced_Graph_Algorithms.md`
