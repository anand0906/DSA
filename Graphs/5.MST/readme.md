
# Disjoint Set & MST - Revision Notes

## Table of Contents
1. [Disjoint Set (Union-Find)](#1-disjoint-set-union-find)
2. [Minimum Spanning Tree - Prim's Algorithm](#2-minimum-spanning-tree---prims-algorithm)
3. [Minimum Spanning Tree - Kruskal's Algorithm](#3-minimum-spanning-tree---kruskals-algorithm)
4. [Real-World Applications](#4-real-world-applications)
5. [Comparison Table](#comparison-table)

---

## 1. Disjoint Set (Union-Find)

### Problem Description
Design a data structure that efficiently manages non-overlapping sets in a dynamic graph. Support operations: initialize n separate sets, merge sets (union), and check if elements belong to same set (find).

Think of it like organizing people into groups where groups can merge over time, and you need to quickly check if two people belong to the same group.

### Sample Test Cases
```python
# Example 1
ds = DisjointSet(5)     # Initialize: {0}, {1}, {2}, {3}, {4}
ds.unionByRank(0, 1)    # Merge: {0,1}, {2}, {3}, {4}
ds.unionBySize(2, 3)    # Merge: {0,1}, {2,3}, {4}
print(ds.find(0, 1))    # True - both in same set
print(ds.find(0, 3))    # False - different sets

# Example 2
ds = DisjointSet(3)     # Initialize: {0}, {1}, {2}
ds.unionBySize(0, 1)    # Merge: {0,1}, {2}
ds.unionBySize(1, 2)    # Merge all: {0,1,2}
print(ds.find(0, 2))    # True - all in same set
```

### Approach: Parent Tracking with Path Compression

**Intuition**: Like a company hierarchy - each person reports to a manager, ultimately leading to a CEO. Path compression creates direct shortcuts to the CEO for faster future lookups.

**Key Optimizations**:
1. **Path Compression**: During find, connect each node directly to root
2. **Union by Rank**: Attach shorter tree under taller tree (keeps structure shallow)
3. **Union by Size**: Attach smaller component under larger (more intuitive, sizes stay accurate)

**Code**:
```python
class DisjointSet:
    def __init__(self, n: int):
        """
        Initialize Disjoint Set for n elements.
        Each node is its own parent initially.
        """
        # Step 1: Initialize arrays
        self.parent = [i for i in range(n)]  # parent[i] = parent of node i
        self.rank = [0] * n                   # rank[i] = height of tree at i
        self.size = [1] * n                   # size[i] = size of component at i
    
    def find_ultimate_parent(self, node: int) -> int:
        """
        Find the ultimate parent (root) with path compression.
        Path compression: Connect each node directly to root during traversal.
        """
        if node == self.parent[node]:
            return node  # Found root
        
        # Path compression: directly connect to ultimate parent
        self.parent[node] = self.find_ultimate_parent(self.parent[node])
        return self.parent[node]
    
    def union_by_rank(self, u: int, v: int):
        """
        Union two sets by rank (tree height).
        Attach tree with smaller rank under tree with larger rank.
        """
        # Step 1: Find ultimate parents
        parent_u = self.find_ultimate_parent(u)
        parent_v = self.find_ultimate_parent(v)
        
        if parent_u == parent_v:
            return  # Already in same set
        
        # Step 2: Attach based on rank
        if self.rank[parent_u] < self.rank[parent_v]:
            self.parent[parent_u] = parent_v  # Attach u's tree under v
        elif self.rank[parent_v] < self.rank[parent_u]:
            self.parent[parent_v] = parent_u  # Attach v's tree under u
        else:
            # Equal ranks: choose one as parent and increment rank
            self.parent[parent_v] = parent_u
            self.rank[parent_u] += 1
    
    def union_by_size(self, u: int, v: int):
        """
        Union two sets by size (number of elements).
        Attach smaller component under larger component.
        """
        # Step 1: Find ultimate parents
        parent_u = self.find_ultimate_parent(u)
        parent_v = self.find_ultimate_parent(v)
        
        if parent_u == parent_v:
            return  # Already in same set
        
        # Step 2: Attach smaller under larger
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]  # Update size
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]  # Update size
    
    def find(self, u: int, v: int) -> bool:
        """
        Check if two nodes belong to same set.
        Returns True if they have same ultimate parent.
        """
        return self.find_ultimate_parent(u) == self.find_ultimate_parent(v)
```

**Time Complexity**: O(α(n)) ≈ O(1) - Nearly constant time (α is inverse Ackermann function)  
**Space Complexity**: O(n) - Three arrays of size n

---

## 2. Minimum Spanning Tree - Prim's Algorithm

### Problem Description
Given a weighted, undirected, connected graph with V vertices and E edges, find the sum of weights of edges in the Minimum Spanning Tree (MST). An MST connects all vertices with exactly V-1 edges having minimum total weight.

### Sample Test Cases
```python
# Example 1
V = 4
Edges = [[0,1,1], [1,2,2], [2,3,3], [0,3,4]]
Output: 6  # MST edges: [0,1,1], [1,2,2], [2,3,3]

# Example 2
V = 3
Edges = [[0,1,5], [1,2,10], [2,0,15]]
Output: 15  # MST edges: [0,1,5], [1,2,10]
```

### Approach: Greedy with Min-Heap

**Intuition**: Start from any node and keep growing the MST by always choosing the smallest edge that connects a new vertex to the current tree. Like building a network by always picking the cheapest connection.

**Key Idea**: Use a min-heap to always pick the smallest available edge leading to an unvisited node.

**Code**:
```python
import heapq

def prim_mst(V, adj):
    """
    Prim's Algorithm for Minimum Spanning Tree.
    
    Args:
        V: Number of vertices
        adj: Adjacency list where adj[u] = [(v, weight), ...]
    
    Returns:
        Total weight of MST
    """
    # Step 1: Initialize data structures
    visited = [False] * V
    min_heap = [(0, 0)]  # (weight, node) - start from node 0 with weight 0
    mst_weight = 0
    
    # Step 2: Process nodes in order of minimum edge weight
    while min_heap:
        weight, node = heapq.heappop(min_heap)
        
        # Skip if already visited
        if visited[node]:
            continue
        
        # Step 3: Add node to MST
        visited[node] = True
        mst_weight += weight
        
        # Step 4: Add all edges from this node to unvisited neighbors
        for neighbor, edge_weight in adj[node]:
            if not visited[neighbor]:
                heapq.heappush(min_heap, (edge_weight, neighbor))
    
    return mst_weight

# Example usage
V = 4
adj = [
    [(1, 1), (3, 4)],           # Node 0 connected to 1 (weight 1), 3 (weight 4)
    [(0, 1), (2, 2)],           # Node 1 connected to 0 (weight 1), 2 (weight 2)
    [(1, 2), (3, 3)],           # Node 2 connected to 1 (weight 2), 3 (weight 3)
    [(0, 4), (2, 3)]            # Node 3 connected to 0 (weight 4), 2 (weight 3)
]
print(prim_mst(V, adj))  # Output: 6
```

**Time Complexity**: O(E log V) - Each edge pushed to heap once  
**Space Complexity**: O(V + E) - Adjacency list and heap

---

## 3. Minimum Spanning Tree - Kruskal's Algorithm

### Problem Description
Same as Prim's - find MST sum, but using a different approach that sorts all edges first.

### Sample Test Cases
```python
# Same examples as Prim's algorithm
V = 4
Edges = [[0,1,1], [1,2,2], [2,3,3], [0,3,4]]
Output: 6
```

### Approach: Sort Edges + Disjoint Set

**Intuition**: Sort all edges by weight. Pick edges from smallest to largest, adding each edge only if it doesn't create a cycle. Use Disjoint Set to efficiently detect cycles.

**Key Idea**: Greedy approach - always pick smallest edge that connects two different components.

**Code**:
```python
class DisjointSet:
    def __init__(self, n):
        """Initialize Union-Find structure"""
        self.parent = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, node):
        """Find with path compression"""
        if node == self.parent[node]:
            return node
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union(self, u, v):
        """
        Union by size. Returns True if union performed, False if already connected.
        """
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return False  # Already connected - would create cycle
        
        # Attach smaller under larger
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]
        
        return True

def kruskal_mst(V, edges):
    """
    Kruskal's Algorithm for Minimum Spanning Tree.
    
    Args:
        V: Number of vertices
        edges: List of edges as [u, v, weight]
    
    Returns:
        Total weight of MST
    """
    # Step 1: Sort edges by weight
    edges.sort(key=lambda x: x[2])
    
    # Step 2: Initialize Disjoint Set
    ds = DisjointSet(V)
    mst_weight = 0
    edges_used = 0
    
    # Step 3: Process edges in sorted order
    for u, v, weight in edges:
        # If u and v are not connected, add this edge
        if ds.union(u, v):
            mst_weight += weight
            edges_used += 1
            
            # MST has exactly V-1 edges
            if edges_used == V - 1:
                break
    
    return mst_weight

# Example usage
V = 4
edges = [[0, 1, 1], [1, 2, 2], [2, 3, 3], [0, 3, 4]]
print(kruskal_mst(V, edges))  # Output: 6
```

**Time Complexity**: O(E log E) - Dominated by sorting edges  
**Space Complexity**: O(V + E) - Edge list and disjoint set

---

## 4. Real-World Applications

### Disjoint Set Use Cases

| Application | Description | Real-World Example |
|-------------|-------------|-------------------|
| **Cycle Detection** | Check if adding edge creates cycle | Road network loop detection |
| **Network Connectivity** | Check if all nodes connected | LAN connectivity verification |
| **Social Networks** | Friend circles / communities | Facebook friend groups |
| **Image Processing** | Connected components in images | MRI scan tumor detection |
| **Clustering** | Group nearby points | Geographical district formation |
| **Account Merging** | Merge duplicate accounts | Bank KYC deduplication |
| **Percolation** | Connectivity analysis | Water flow simulation |

### MST Use Cases

| Application | Description | Real-World Example |
|-------------|-------------|-------------------|
| **Network Design** | Minimum cost to connect all nodes | Telecom network planning |
| **Circuit Design** | Connect components efficiently | PCB trace routing |
| **Clustering** | Hierarchical clustering | Data analysis grouping |
| **Approximation Algorithms** | TSP approximation | Delivery route optimization |
| **Image Segmentation** | Partition images | Computer vision preprocessing |

---

## Comparison Table

### Disjoint Set Operations

| Operation | Time Complexity | Use Case |
|-----------|----------------|----------|
| Initialize | O(n) | Setup n separate sets |
| Find | O(α(n)) ≈ O(1) | Get root of element |
| Union by Rank | O(α(n)) ≈ O(1) | Merge based on tree height |
| Union by Size | O(α(n)) ≈ O(1) | Merge based on component size |
| Connected Query | O(α(n)) ≈ O(1) | Check if same set |

### MST Algorithms Comparison

| Feature | Prim's Algorithm | Kruskal's Algorithm |
|---------|------------------|---------------------|
| **Approach** | Grow tree from starting vertex | Sort edges, add smallest valid |
| **Data Structure** | Min-heap (priority queue) | Disjoint Set (Union-Find) |
| **Time Complexity** | O(E log V) | O(E log E) |
| **Space Complexity** | O(V + E) | O(V + E) |
| **Best For** | Dense graphs (many edges) | Sparse graphs (few edges) |
| **Edge Processing** | Process as encountered | Process in sorted order |
| **Starting Point** | Requires starting vertex | No starting vertex needed |

### When to Use Which MST Algorithm?

**Use Prim's Algorithm when:**
- Graph is dense (E ≈ V²)
- You have a natural starting vertex
- You need to grow tree incrementally
- Working with adjacency matrix representation

**Use Kruskal's Algorithm when:**
- Graph is sparse (E << V²)
- You need to find MST weight only
- Edges are already sorted or easy to sort
- Using edge list representation

---

## Key Insights

### Disjoint Set
- **Path Compression** is crucial for near-constant time operations
- **Union by Size** preferred over Union by Rank (more intuitive, stays accurate)
- **α(n) ≤ 4** for all practical inputs, so effectively O(1)
- Perfect for **dynamic connectivity** problems

### MST Algorithms
- Both guarantee **optimal solution** (minimum total weight)
- Both are **greedy algorithms** that work correctly
- **Prim's** grows one tree, **Kruskal's** merges multiple trees
- MST is **unique** if all edge weights are distinct

### Common Patterns

**Cycle Detection Pattern**:
```python
# Add edge only if doesn't create cycle
if ds.find(u) != ds.find(v):
    ds.union(u, v)
    # Add edge to result
```

**Connected Components Pattern**:
```python
# Count unique parents at the end
unique_parents = len(set(ds.find(i) for i in range(n)))
```

**MST Edge Selection Pattern**:
```python
# For Kruskal's: sort edges first
edges.sort(key=lambda x: x[2])
for u, v, weight in edges:
    if ds.union(u, v):  # If connects different components
        mst_weight += weight
```
