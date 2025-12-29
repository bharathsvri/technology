# Union-Find (Disjoint Set Union - DSU)

## Definition
Union-Find is a data structure that tracks a set of elements partitioned into disjoint (non-overlapping) subsets. It provides efficient operations to merge sets and find which set an element belongs to.

## Operations

### 1. Find
Determines which subset a particular element belongs to.

### 2. Union
Merges two subsets into a single subset.

## Applications
- Kruskal's algorithm for MST
- Network connectivity
- Image processing
- Least Common Ancestor (LCA)
- Cycle detection in undirected graphs
- Dynamic connectivity problems

## Basic Implementation

```java
public class UnionFind {
    private int[] parent;
    private int[] rank;
    private int count;  // Number of components
    
    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        count = n;
        
        // Initialize: each element is its own parent
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            rank[i] = 0;
        }
    }
    
    // Find with path compression
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);  // Path compression
        }
        return parent[x];
    }
    
    // Union by rank
    public boolean union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) {
            return false;  // Already in same set
        }
        
        // Union by rank
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
        
        count--;
        return true;
    }
    
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
    
    public int getCount() {
        return count;
    }
}
```

## Optimizations

### 1. Path Compression
Makes the tree flatter by making nodes point directly to root during find.

```java
public int find(int x) {
    if (parent[x] != x) {
        parent[x] = find(parent[x]);  // Path compression
    }
    return parent[x];
}

// Iterative version
public int findIterative(int x) {
    int root = x;
    while (parent[root] != root) {
        root = parent[root];
    }
    
    // Path compression
    while (x != root) {
        int next = parent[x];
        parent[x] = root;
        x = next;
    }
    
    return root;
}
```

### 2. Union by Rank
Always attaches smaller tree under root of larger tree.

```java
public void union(int x, int y) {
    int rootX = find(x);
    int rootY = find(y);
    
    if (rootX == rootY) return;
    
    if (rank[rootX] < rank[rootY]) {
        parent[rootX] = rootY;
    } else if (rank[rootX] > rank[rootY]) {
        parent[rootY] = rootX;
    } else {
        parent[rootY] = rootX;
        rank[rootX]++;
    }
}
```

### 3. Union by Size (Alternative)
```java
public class UnionFindBySize {
    private int[] parent;
    private int[] size;
    
    public UnionFindBySize(int n) {
        parent = new int[n];
        size = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }
    
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) return;
        
        if (size[rootX] < size[rootY]) {
            parent[rootX] = rootY;
            size[rootY] += size[rootX];
        } else {
            parent[rootY] = rootX;
            size[rootX] += size[rootY];
        }
    }
    
    public int getSize(int x) {
        return size[find(x)];
    }
}
```

## Time Complexity

| Operation | Without Optimization | With Path Compression | With Both Optimizations |
|-----------|---------------------|----------------------|------------------------|
| Find | O(n) | O(log n) | O(α(n)) ≈ O(1) |
| Union | O(n) | O(log n) | O(α(n)) ≈ O(1) |

Where α(n) is the inverse Ackermann function (extremely slow-growing, effectively constant).

## Applications

### 1. Kruskal's Algorithm for MST
```java
public class KruskalMST {
    static class Edge {
        int u, v, weight;
        Edge(int u, int v, int weight) {
            this.u = u;
            this.v = v;
            this.weight = weight;
        }
    }
    
    public static int kruskalMST(int n, List<Edge> edges) {
        // Sort edges by weight
        edges.sort((a, b) -> Integer.compare(a.weight, b.weight));
        
        UnionFind uf = new UnionFind(n);
        int mstWeight = 0;
        int edgesUsed = 0;
        
        for (Edge edge : edges) {
            if (uf.union(edge.u, edge.v)) {
                mstWeight += edge.weight;
                edgesUsed++;
                if (edgesUsed == n - 1) break;
            }
        }
        
        return mstWeight;
    }
}
```

### 2. Number of Connected Components
```java
public int numberOfComponents(int n, int[][] edges) {
    UnionFind uf = new UnionFind(n);
    for (int[] edge : edges) {
        uf.union(edge[0], edge[1]);
    }
    return uf.getCount();
}
```

### 3. Cycle Detection in Undirected Graph
```java
public boolean hasCycle(int n, int[][] edges) {
    UnionFind uf = new UnionFind(n);
    for (int[] edge : edges) {
        if (!uf.union(edge[0], edge[1])) {
            return true;  // Cycle detected
        }
    }
    return false;
}
```

### 4. Redundant Connection
```java
public int[] findRedundantConnection(int[][] edges) {
    int n = edges.length;
    UnionFind uf = new UnionFind(n);
    
    for (int[] edge : edges) {
        if (!uf.union(edge[0] - 1, edge[1] - 1)) {
            return edge;
        }
    }
    return new int[0];
}
```

### 5. Accounts Merge (String Union-Find)
```java
public class AccountsMerge {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        int n = accounts.size();
        UnionFind uf = new UnionFind(n);
        Map<String, Integer> emailToId = new HashMap<>();
        
        // Union accounts sharing emails
        for (int i = 0; i < n; i++) {
            for (int j = 1; j < accounts.get(i).size(); j++) {
                String email = accounts.get(i).get(j);
                if (emailToId.containsKey(email)) {
                    uf.union(i, emailToId.get(email));
                } else {
                    emailToId.put(email, i);
                }
            }
        }
        
        // Group emails by root
        Map<Integer, TreeSet<String>> groupEmails = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int root = uf.find(i);
            groupEmails.putIfAbsent(root, new TreeSet<>());
            for (int j = 1; j < accounts.get(i).size(); j++) {
                groupEmails.get(root).add(accounts.get(i).get(j));
            }
        }
        
        // Build result
        List<List<String>> result = new ArrayList<>();
        for (Map.Entry<Integer, TreeSet<String>> entry : groupEmails.entrySet()) {
            List<String> account = new ArrayList<>();
            account.add(accounts.get(entry.getKey()).get(0));  // Name
            account.addAll(entry.getValue());
            result.add(account);
        }
        
        return result;
    }
}
```

### 6. Longest Consecutive Sequence (Using DSU)
```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;
    
    UnionFind uf = new UnionFind(nums.length);
    Map<Integer, Integer> valToIndex = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        if (valToIndex.containsKey(nums[i])) continue;
        valToIndex.put(nums[i], i);
        
        if (valToIndex.containsKey(nums[i] - 1)) {
            uf.union(i, valToIndex.get(nums[i] - 1));
        }
        if (valToIndex.containsKey(nums[i] + 1)) {
            uf.union(i, valToIndex.get(nums[i] + 1));
        }
    }
    
    // Count sizes of components
    Map<Integer, Integer> rootCount = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int root = uf.find(i);
        rootCount.put(root, rootCount.getOrDefault(root, 0) + 1);
    }
    
    return Collections.max(rootCount.values());
}
```

## Advanced Features

### Weighted Union-Find
```java
public class WeightedUnionFind {
    private int[] parent;
    private double[] weight;
    
    public WeightedUnionFind(int n) {
        parent = new int[n];
        weight = new double[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            weight[i] = 1.0;
        }
    }
    
    public int find(int x) {
        if (parent[x] != x) {
            int root = find(parent[x]);
            weight[x] *= weight[parent[x]];
            parent[x] = root;
        }
        return parent[x];
    }
    
    public void union(int x, int y, double value) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX == rootY) return;
        
        parent[rootX] = rootY;
        weight[rootX] = value * weight[y] / weight[x];
    }
    
    public double getWeight(int x, int y) {
        if (find(x) != find(y)) return -1;
        return weight[x] / weight[y];
    }
}
```

## Common Problems
- Number of Islands II
- Accounts Merge
- Redundant Connection
- Friend Circles
- Longest Consecutive Sequence
- Most Stones Removed with Same Row or Column
- Regions Cut By Slashes
- Minimize Hamming Distance After Swap Operations
- Remove Max Number of Edges to Keep Graph Fully Traversable

