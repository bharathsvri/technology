# Topological Sort — Kahn’s Algorithm vs DFS Postorder

**Topological ordering** exists only on **DAGs**. Two classic approaches appear in interviews: **Kahn (BFS indegree)** and **DFS postorder** reverse.

---

## Kahn’s algorithm

1. Compute **indegree** for each node.
2. Push **indegree 0** nodes to queue.
3. Pop, append to order, decrement neighbors’ indegree; repeat.

If processed count **< |V|**, graph has a **cycle**.

**Complexity**: **O(V+E)** with adjacency lists.

---

## DFS approach

Run DFS, emit nodes on **postorder**, **reverse** for topo order—or detect **back edges** for cycle.

---

## Applications

- Build systems, **task scheduling**, course prerequisites.

---

## Related

- [Graphs](07_Graphs.md)
- [Advanced Graph Algorithms](28_Advanced_Graph_Algorithms.md)
