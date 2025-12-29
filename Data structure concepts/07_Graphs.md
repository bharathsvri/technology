# Graphs

## Definition
A graph is a non-linear data structure consisting of a finite set of vertices (nodes) and edges that connect pairs of vertices. Graphs represent relationships between objects.

## Characteristics
- **Vertices (Nodes)**: Represent entities
- **Edges**: Represent relationships between vertices
- **Non-linear**: No hierarchical structure like trees
- **Flexible**: Can model complex relationships
- **Cyclic or Acyclic**: May contain cycles

## Types of Graphs

### Based on Direction
1. **Directed Graph (Digraph)**
   - Edges have direction
   - (A, B) ≠ (B, A)
   - Example: Web links, social media follows

2. **Undirected Graph**
   - Edges have no direction
   - (A, B) = (B, A)
   - Example: Friendships, road networks

### Based on Weight
1. **Weighted Graph**
   - Edges have weights/costs
   - Example: Distance between cities, network latency

2. **Unweighted Graph**
   - All edges have equal weight
   - Example: Social network connections

### Based on Cycles
1. **Cyclic Graph**
   - Contains at least one cycle

2. **Acyclic Graph**
   - No cycles present
   - Example: Tree (special case of graph)

## Graph Terminology

### Basic Terms
- **Vertex/Node**: An entity in the graph
- **Edge**: Connection between two vertices
- **Adjacent**: Two vertices connected by an edge
- **Degree**: Number of edges connected to a vertex
  - In-degree: Incoming edges (directed graph)
  - Out-degree: Outgoing edges (directed graph)
- **Path**: Sequence of vertices connected by edges
- **Cycle**: Path that starts and ends at the same vertex
- **Connected Graph**: Path exists between any two vertices
- **Disconnected Graph**: Contains isolated components

### Special Graphs
- **Complete Graph**: Every vertex connected to every other vertex
- **Bipartite Graph**: Vertices can be divided into two sets
- **DAG (Directed Acyclic Graph)**: Directed graph with no cycles
- **Tree**: Connected acyclic graph (n vertices, n-1 edges)

## Representation Methods

### 1. Adjacency Matrix
- 2D array of size V×V
- Matrix[i][j] = 1 if edge exists, 0 otherwise
- **Space**: O(V²)
- **Check edge**: O(1)
- **List neighbors**: O(V)

### 2. Adjacency List
- Array of lists
- Each vertex has a list of adjacent vertices
- **Space**: O(V + E)
- **Check edge**: O(V) worst case
- **List neighbors**: O(degree of vertex)

### 3. Edge List
- List of all edges
- Simple but less efficient
- **Space**: O(E)
- **Check edge**: O(E)

## Graph Traversal Algorithms

### 1. Depth-First Search (DFS)
- Explore as far as possible before backtracking
- Uses stack (recursion or explicit stack)
- **Time**: O(V + E)
- **Space**: O(V)
- Applications: Cycle detection, path finding, topological sort

### 2. Breadth-First Search (BFS)
- Explore level by level
- Uses queue
- **Time**: O(V + E)
- **Space**: O(V)
- Applications: Shortest path (unweighted), level-order traversal

## Important Algorithms

### Shortest Path
1. **Dijkstra's Algorithm**
   - Single-source shortest path (non-negative weights)
   - **Time**: O((V + E) log V) with priority queue

2. **Bellman-Ford Algorithm**
   - Single-source shortest path (handles negative weights)
   - **Time**: O(VE)

3. **Floyd-Warshall Algorithm**
   - All-pairs shortest path
   - **Time**: O(V³)

### Minimum Spanning Tree (MST)
1. **Kruskal's Algorithm**
   - Uses Union-Find data structure
   - **Time**: O(E log E)

2. **Prim's Algorithm**
   - Similar to Dijkstra's
   - **Time**: O((V + E) log V)

### Topological Sort
- Ordering of vertices in a DAG
- Applications: Course prerequisites, task scheduling
- **Time**: O(V + E)

## Time Complexity Summary

| Operation | Adjacency Matrix | Adjacency List |
|-----------|------------------|----------------|
| Add Vertex | O(V²) | O(1) |
| Add Edge | O(1) | O(1) |
| Remove Edge | O(1) | O(V) |
| Check Edge | O(1) | O(V) |
| Space | O(V²) | O(V + E) |

## Space Complexity
- Adjacency Matrix: O(V²)
- Adjacency List: O(V + E)
- Where V = vertices, E = edges

## Advantages
- Models real-world relationships effectively
- Flexible representation
- Powerful algorithms available
- Can represent complex systems

## Disadvantages
- High memory usage (adjacency matrix)
- Algorithms can be complex
- Traversal overhead
- Difficult to visualize for large graphs

## Use Cases
- Social networks (friends, connections)
- Web page linking (directed graph)
- Road networks (navigation systems)
- Computer networks (routing)
- Dependency graphs (package managers)
- Recommendation systems
- Game development (pathfinding)
- GPS navigation
- Task scheduling (project management)
- Neural networks

## Common Problems
- Graph Traversal (DFS, BFS)
- Detect Cycle in Graph
- Find Shortest Path
- Minimum Spanning Tree
- Topological Sort
- Strongly Connected Components
- Bipartite Graph Check
- Network Flow Problems
- Graph Coloring

## Example Applications

### Social Network
```
Vertices: Users
Edges: Friendships (undirected)
Find: Mutual friends, friend recommendations
```

### Navigation System
```
Vertices: Intersections
Edges: Roads (weighted by distance/time)
Find: Shortest route using Dijkstra's
```

### Web Crawling
```
Vertices: Web pages
Edges: Hyperlinks (directed)
Traverse: DFS/BFS to index pages
```

