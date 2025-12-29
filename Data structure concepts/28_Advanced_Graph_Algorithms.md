# Advanced Graph Algorithms

## Strongly Connected Components (SCC)

### Kosaraju's Algorithm
Finds all strongly connected components in a directed graph.

```java
import java.util.*;

public class KosarajuSCC {
    private int n;
    private List<List<Integer>> graph;
    private List<List<Integer>> reverseGraph;
    private boolean[] visited;
    private Stack<Integer> stack;
    private List<List<Integer>> scc;
    
    public KosarajuSCC(int n) {
        this.n = n;
        graph = new ArrayList<>();
        reverseGraph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
            reverseGraph.add(new ArrayList<>());
        }
        visited = new boolean[n];
        stack = new Stack<>();
        scc = new ArrayList<>();
    }
    
    public void addEdge(int u, int v) {
        graph.get(u).add(v);
        reverseGraph.get(v).add(u);
    }
    
    public List<List<Integer>> findSCC() {
        // Step 1: Fill stack with vertices in order of finishing times
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs1(i);
            }
        }
        
        // Step 2: Process vertices in reverse order
        Arrays.fill(visited, false);
        while (!stack.isEmpty()) {
            int v = stack.pop();
            if (!visited[v]) {
                List<Integer> component = new ArrayList<>();
                dfs2(v, component);
                scc.add(component);
            }
        }
        
        return scc;
    }
    
    private void dfs1(int v) {
        visited[v] = true;
        for (int u : graph.get(v)) {
            if (!visited[u]) {
                dfs1(u);
            }
        }
        stack.push(v);
    }
    
    private void dfs2(int v, List<Integer> component) {
        visited[v] = true;
        component.add(v);
        for (int u : reverseGraph.get(v)) {
            if (!visited[u]) {
                dfs2(u, component);
            }
        }
    }
}
```

### Tarjan's Algorithm
```java
public class TarjanSCC {
    private int n;
    private List<List<Integer>> graph;
    private int[] ids;
    private int[] low;
    private boolean[] onStack;
    private Stack<Integer> stack;
    private int id;
    private List<List<Integer>> scc;
    
    public TarjanSCC(int n) {
        this.n = n;
        graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        ids = new int[n];
        low = new int[n];
        onStack = new boolean[n];
        stack = new Stack<>();
        Arrays.fill(ids, -1);
        scc = new ArrayList<>();
        id = 0;
    }
    
    public void addEdge(int u, int v) {
        graph.get(u).add(v);
    }
    
    public List<List<Integer>> findSCC() {
        for (int i = 0; i < n; i++) {
            if (ids[i] == -1) {
                dfs(i);
            }
        }
        return scc;
    }
    
    private void dfs(int v) {
        stack.push(v);
        onStack[v] = true;
        ids[v] = low[v] = id++;
        
        for (int u : graph.get(v)) {
            if (ids[u] == -1) {
                dfs(u);
            }
            if (onStack[u]) {
                low[v] = Math.min(low[v], low[u]);
            }
        }
        
        if (ids[v] == low[v]) {
            List<Integer> component = new ArrayList<>();
            while (true) {
                int node = stack.pop();
                onStack[node] = false;
                component.add(node);
                if (node == v) break;
            }
            scc.add(component);
        }
    }
}
```

## Articulation Points (Cut Vertices)

```java
public class ArticulationPoints {
    private int n;
    private List<List<Integer>> graph;
    private int[] ids;
    private int[] low;
    private boolean[] visited;
    private boolean[] isArticulation;
    private int id;
    
    public ArticulationPoints(int n) {
        this.n = n;
        graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        ids = new int[n];
        low = new int[n];
        visited = new boolean[n];
        isArticulation = new boolean[n];
        id = 0;
    }
    
    public void addEdge(int u, int v) {
        graph.get(u).add(v);
        graph.get(v).add(u);
    }
    
    public boolean[] findArticulationPoints() {
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i, i, -1);
            }
        }
        return isArticulation;
    }
    
    private void dfs(int root, int v, int parent) {
        if (parent == root) id++;
        visited[v] = true;
        low[v] = ids[v] = id++;
        int children = 0;
        
        for (int u : graph.get(v)) {
            if (u == parent) continue;
            
            if (!visited[u]) {
                children++;
                dfs(root, u, v);
                low[v] = Math.min(low[v], low[u]);
                
                // Check if v is articulation point
                if ((parent == -1 && children > 1) || 
                    (parent != -1 && low[u] >= ids[v])) {
                    isArticulation[v] = true;
                }
            } else {
                low[v] = Math.min(low[v], ids[u]);
            }
        }
    }
}
```

## Bridges (Critical Edges)

```java
public class Bridges {
    private int n;
    private List<List<Integer>> graph;
    private int[] ids;
    private int[] low;
    private boolean[] visited;
    private List<int[]> bridges;
    private int id;
    
    public Bridges(int n) {
        this.n = n;
        graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        ids = new int[n];
        low = new int[n];
        visited = new boolean[n];
        bridges = new ArrayList<>();
        id = 0;
    }
    
    public void addEdge(int u, int v) {
        graph.get(u).add(v);
        graph.get(v).add(u);
    }
    
    public List<int[]> findBridges() {
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i, -1);
            }
        }
        return bridges;
    }
    
    private void dfs(int v, int parent) {
        visited[v] = true;
        low[v] = ids[v] = id++;
        
        for (int u : graph.get(v)) {
            if (u == parent) continue;
            
            if (!visited[u]) {
                dfs(u, v);
                low[v] = Math.min(low[v], low[u]);
                
                if (low[u] > ids[v]) {
                    bridges.add(new int[]{v, u});
                }
            } else {
                low[v] = Math.min(low[v], ids[u]);
            }
        }
    }
}
```

## Network Flow - Ford-Fulkerson

```java
public class FordFulkerson {
    private int n;
    private int[][] graph;
    private int[] parent;
    private boolean[] visited;
    
    public FordFulkerson(int n) {
        this.n = n;
        graph = new int[n][n];
        parent = new int[n];
        visited = new boolean[n];
    }
    
    public void addEdge(int u, int v, int capacity) {
        graph[u][v] = capacity;
    }
    
    public int maxFlow(int source, int sink) {
        int maxFlow = 0;
        
        while (bfs(source, sink)) {
            int pathFlow = Integer.MAX_VALUE;
            
            // Find minimum residual capacity
            for (int v = sink; v != source; v = parent[v]) {
                int u = parent[v];
                pathFlow = Math.min(pathFlow, graph[u][v]);
            }
            
            // Update residual graph
            for (int v = sink; v != source; v = parent[v]) {
                int u = parent[v];
                graph[u][v] -= pathFlow;
                graph[v][u] += pathFlow;
            }
            
            maxFlow += pathFlow;
        }
        
        return maxFlow;
    }
    
    private boolean bfs(int source, int sink) {
        Arrays.fill(visited, false);
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(source);
        visited[source] = true;
        parent[source] = -1;
        
        while (!queue.isEmpty()) {
            int u = queue.poll();
            for (int v = 0; v < n; v++) {
                if (!visited[v] && graph[u][v] > 0) {
                    visited[v] = true;
                    parent[v] = u;
                    queue.offer(v);
                    if (v == sink) return true;
                }
            }
        }
        return false;
    }
}
```

## Time Complexity

| Algorithm | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Kosaraju's SCC | O(V + E) | O(V + E) |
| Tarjan's SCC | O(V + E) | O(V) |
| Articulation Points | O(V + E) | O(V) |
| Bridges | O(V + E) | O(V) |
| Ford-Fulkerson | O(E × max_flow) | O(V + E) |

## Common Problems
- Critical Connections
- Strongly Connected Components
- Network Delay Time
- Cheapest Flights Within K Stops
- Minimum Cost to Cut a Stick
- Redundant Connection II
- Critical Routers
- Maximum Flow problems

