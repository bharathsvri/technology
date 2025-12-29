# Greedy Algorithms

## Definition
A greedy algorithm makes the locally optimal choice at each stage with the hope of finding a global optimum. It never reconsiders previous choices.

## Characteristics
- **Greedy Choice Property**: Locally optimal choice leads to globally optimal solution
- **Optimal Substructure**: Problem can be broken into subproblems
- **No Backtracking**: Once a choice is made, it's never reconsidered
- **Fast**: Usually O(n log n) or O(n) due to sorting or single pass

## When to Use Greedy
1. **Greedy Choice Property**: Locally optimal choice is globally optimal
2. **Optimal Substructure**: Optimal solution contains optimal subproblem solutions
3. **No need to explore all possibilities**: Unlike DP, don't need all subproblems

## Greedy vs Dynamic Programming
- **Greedy**: Makes one choice and moves forward
- **DP**: Explores all possibilities and chooses optimal

## Classic Greedy Problems

### 1. Activity Selection Problem
**Problem**: Select maximum number of non-overlapping activities

```python
def activity_selection(start, finish):
    # Sort by finish time
    activities = sorted(zip(start, finish), key=lambda x: x[1])
    
    selected = [0]  # First activity always selected
    last_finish = activities[0][1]
    
    for i in range(1, len(activities)):
        if activities[i][0] >= last_finish:
            selected.append(i)
            last_finish = activities[i][1]
    
    return selected

# With activity details
def activity_selection_detailed(activities):
    # activities = [(start, finish, name), ...]
    activities.sort(key=lambda x: x[1])  # Sort by finish time
    
    selected = [activities[0]]
    
    for activity in activities[1:]:
        if activity[0] >= selected[-1][1]:  # No overlap
            selected.append(activity)
    
    return selected
```

**Time Complexity**: O(n log n) due to sorting
**Greedy Choice**: Always pick activity with earliest finish time

---

### 2. Fractional Knapsack
**Problem**: Maximize value with weight constraint (can take fractions of items)

```python
def fractional_knapsack(weights, values, capacity):
    # Calculate value per unit weight
    items = [(values[i] / weights[i], weights[i], values[i]) 
             for i in range(len(weights))]
    
    # Sort by value per unit weight (descending)
    items.sort(reverse=True)
    
    total_value = 0
    remaining_capacity = capacity
    
    for value_per_unit, weight, value in items:
        if remaining_capacity >= weight:
            # Take entire item
            total_value += value
            remaining_capacity -= weight
        else:
            # Take fraction
            total_value += value_per_unit * remaining_capacity
            break
    
    return total_value
```

**Time Complexity**: O(n log n)
**Greedy Choice**: Always take item with highest value/weight ratio

---

### 3. Huffman Coding
**Problem**: Construct optimal prefix code for data compression

```python
import heapq

class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None
    
    def __lt__(self, other):
        return self.freq < other.freq

def build_huffman_tree(char_freq):
    # Create priority queue (min heap)
    heap = [Node(char, freq) for char, freq in char_freq.items()]
    heapq.heapify(heap)
    
    while len(heap) > 1:
        # Extract two nodes with minimum frequency
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        
        # Create internal node
        merged = Node(None, left.freq + right.freq)
        merged.left = left
        merged.right = right
        
        heapq.heappush(heap, merged)
    
    return heap[0]  # Root of Huffman tree

def generate_codes(root, code="", codes={}):
    if root.char:
        codes[root.char] = code if code else "0"
    else:
        generate_codes(root.left, code + "0", codes)
        generate_codes(root.right, code + "1", codes)
    return codes
```

**Time Complexity**: O(n log n)
**Greedy Choice**: Always merge two nodes with smallest frequency

---

### 4. Minimum Spanning Tree (Kruskal's Algorithm)
**Problem**: Find minimum weight spanning tree

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        root_x, root_y = self.find(x), self.find(y)
        if root_x == root_y:
            return False
        
        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1
        return True

def kruskal_mst(edges, n):
    # Sort edges by weight
    edges.sort(key=lambda x: x[2])
    
    uf = UnionFind(n)
    mst = []
    total_weight = 0
    
    for u, v, weight in edges:
        if uf.union(u, v):
            mst.append((u, v, weight))
            total_weight += weight
            if len(mst) == n - 1:
                break
    
    return mst, total_weight
```

**Time Complexity**: O(E log E) where E is number of edges
**Greedy Choice**: Always add edge with minimum weight that doesn't form cycle

---

### 5. Dijkstra's Algorithm (Shortest Path)
**Problem**: Find shortest path from source to all vertices (non-negative weights)

```python
import heapq

def dijkstra(graph, start):
    # graph: adjacency list {node: [(neighbor, weight), ...]}
    n = len(graph)
    dist = [float('inf')] * n
    dist[start] = 0
    visited = set()
    pq = [(0, start)]  # (distance, node)
    
    while pq:
        current_dist, u = heapq.heappop(pq)
        
        if u in visited:
            continue
        
        visited.add(u)
        
        for v, weight in graph[u]:
            if v not in visited:
                new_dist = current_dist + weight
                if new_dist < dist[v]:
                    dist[v] = new_dist
                    heapq.heappush(pq, (new_dist, v))
    
    return dist
```

**Time Complexity**: O((V + E) log V) with priority queue
**Greedy Choice**: Always explore vertex with minimum distance first

---

### 6. Job Scheduling (Weighted)
**Problem**: Schedule jobs with deadlines and profits to maximize profit

```python
def job_scheduling(jobs):
    # jobs = [(deadline, profit), ...]
    jobs.sort(key=lambda x: x[1], reverse=True)  # Sort by profit
    
    max_deadline = max(job[0] for job in jobs)
    schedule = [-1] * (max_deadline + 1)
    total_profit = 0
    
    for deadline, profit in jobs:
        # Find latest available slot
        for i in range(deadline, 0, -1):
            if schedule[i] == -1:
                schedule[i] = profit
                total_profit += profit
                break
    
    return total_profit, schedule
```

**Time Complexity**: O(n²)
**Greedy Choice**: Always schedule job with highest profit at latest available time

---

### 7. Coin Change (Greedy - when it works)
**Problem**: Minimum coins to make amount (works only for certain coin systems)

```python
def coin_change_greedy(coins, amount):
    # Only works if coin system is canonical (like [1, 5, 10, 25])
    coins.sort(reverse=True)  # Largest first
    count = 0
    
    for coin in coins:
        if amount >= coin:
            num_coins = amount // coin
            count += num_coins
            amount -= num_coins * coin
    
    return count if amount == 0 else -1
```

**Note**: Greedy doesn't always give optimal solution for coin change!
- Works for: [1, 5, 10, 25]
- Doesn't work for: [1, 3, 4] with amount 6 (greedy: 4+1+1=3 coins, optimal: 3+3=2 coins)

---

### 8. Minimum Platforms Required
**Problem**: Find minimum number of platforms needed for trains

```python
def min_platforms(arrival, departure):
    arrival.sort()
    departure.sort()
    
    platforms_needed = 1
    result = 1
    i, j = 1, 0
    
    while i < len(arrival) and j < len(departure):
        if arrival[i] <= departure[j]:
            platforms_needed += 1
            i += 1
        else:
            platforms_needed -= 1
            j += 1
        
        result = max(result, platforms_needed)
    
    return result
```

**Time Complexity**: O(n log n)
**Greedy Choice**: Process events in chronological order

---

### 9. Gas Station Problem
**Problem**: Can you complete circular route? Start from which station?

```python
def can_complete_circuit(gas, cost):
    total_gas = sum(gas)
    total_cost = sum(cost)
    
    if total_gas < total_cost:
        return -1
    
    start = 0
    current_gas = 0
    
    for i in range(len(gas)):
        current_gas += gas[i] - cost[i]
        if current_gas < 0:
            start = i + 1
            current_gas = 0
    
    return start
```

**Time Complexity**: O(n)

---

### 10. Meeting Rooms II
**Problem**: Minimum number of meeting rooms needed

```python
def min_meeting_rooms(intervals):
    if not intervals:
        return 0
    
    start_times = sorted([i[0] for i in intervals])
    end_times = sorted([i[1] for i in intervals])
    
    rooms = 0
    end_ptr = 0
    
    for start in start_times:
        if start < end_times[end_ptr]:
            rooms += 1
        else:
            end_ptr += 1
    
    return rooms

# Using priority queue
import heapq

def min_meeting_rooms_pq(intervals):
    if not intervals:
        return 0
    
    intervals.sort(key=lambda x: x[0])
    heap = [intervals[0][1]]  # Store end times
    
    for start, end in intervals[1:]:
        if start >= heap[0]:
            heapq.heappop(heap)
        heapq.heappush(heap, end)
    
    return len(heap)
```

---

## Greedy Algorithm Design Steps

1. **Understand the Problem**: What are we optimizing?
2. **Identify Greedy Choice**: What's the locally optimal choice?
3. **Prove Greedy Choice Property**: Show locally optimal = globally optimal
4. **Implement**: Code the greedy algorithm
5. **Verify**: Test with various inputs

## Proving Greedy Correctness

### Method 1: Greedy Choice Property
- Show that making greedy choice is safe
- Always leads to optimal solution

### Method 2: Exchange Argument
- Show that any optimal solution can be transformed to greedy solution
- Without decreasing quality

### Method 3: Induction
- Base case: Greedy works for smallest subproblem
- Inductive step: If greedy works for smaller problems, works for larger

## Common Greedy Patterns

1. **Sorting First**: Many greedy algorithms start by sorting
2. **Priority Queue**: Use heap for greedy choice
3. **Interval Problems**: Process by start/end times
4. **Greedy + DP**: Sometimes combine both approaches

## When Greedy Fails

- **0/1 Knapsack**: Need DP (can't take fraction)
- **Coin Change**: May need DP for arbitrary coin systems
- **Longest Path**: Greedy doesn't work, need other approaches

## Common Problems
- Activity Selection
- Fractional Knapsack
- Huffman Coding
- Minimum Spanning Tree (Kruskal, Prim)
- Shortest Path (Dijkstra)
- Job Scheduling
- Meeting Rooms
- Gas Station
- Minimum Platforms
- Maximum Units on a Truck
- Partition Labels

