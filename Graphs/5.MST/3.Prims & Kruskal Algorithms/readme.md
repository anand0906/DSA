# Minimum Spanning Tree (MST) – Notes

## Problem Description

We are given a **weighted, undirected, and connected graph** with `V` vertices (numbered from 0 to V-1) and `E` edges.
Each edge is represented as `[ai, bi, wi]`, where:

* `ai` and `bi` are the endpoints of the edge
* `wi` is the weight of the edge

We need to **find the sum of the weights of the edges in the Minimum Spanning Tree (MST)** of the graph.

👉 A **Minimum Spanning Tree (MST)** is a subset of edges of a graph that:

* Connects all vertices
* Contains exactly `V-1` edges (no cycles)
* Has the **minimum possible total edge weight**

---

## Examples

### Example 1

**Input:**

```text
V = 4
Edges = [[0, 1, 1], [1, 2, 2], [2, 3, 3], [0, 3, 4]]
```

<img src="https://static.takeuforward.org/content/ProblemSetter-VMx0t_X_" />

**Output:**

```text
6
```

**Explanation:**

* Chosen edges for MST:

  * \[0, 1, 1]
  * \[1, 2, 2]
  * \[2, 3, 3]
* Total MST weight = 1 + 2 + 3 = **6**

### Example 2

**Input:**

```text
V = 3
Edges = [[0, 1, 5], [1, 2, 10], [2, 0, 15]]
```

<img src="https://static.takeuforward.org/content/ProblemSetter-TRzOGiy0" />

**Output:**

```text
15
```

**Explanation:**

* Chosen edges for MST:

  * \[0, 1, 5]
  * \[1, 2, 10]
* Total MST weight = 5 + 10 = **15**

---

## Solution

We can solve this problem using two famous algorithms:

1. **Prim’s Algorithm** – Grow the MST by choosing the smallest edge from the tree to a new vertex.
2. **Kruskal’s Algorithm** – Choose the smallest edges one by one, avoiding cycles (using Disjoint Set/Union-Find).

Both algorithms guarantee the minimum spanning tree weight.

---

## Intuition

* The MST must always choose the **smallest possible edges** that connect all nodes without forming cycles.
* **Greedy approach works** here because choosing the smallest available edge at each step still leads to the optimal MST.
* * In **Prim’s Algorithm**, we start from a node and keep expanding with the smallest available edge.
  * In **Kruskal’s Algorithm**, we sort edges by weight and keep picking the smallest edge if it doesn’t form a cycle.

---

## Approach Steps

### Prim’s Algorithm

1. Start with an arbitrary node.
2. Use a **min-heap** to always pick the smallest edge leading to an unvisited node.
3. Mark the node as visited and add the edge weight to the MST sum.
4. Push all edges from this node to the heap.
5. Repeat until all nodes are visited.

### Kruskal’s Algorithm

1. Extract all edges from the graph.
2. Sort edges by their weight.
3. Initialize a **Disjoint Set (Union-Find)** to keep track of connected components.
4. For each edge in sorted order:

   * If the two nodes are not already connected, add the edge to MST.
   * Union their sets in the disjoint set.
5. Continue until we have exactly `V-1` edges in MST.

---

## Code

### Prim’s Algorithm (Python)

```python
import heapq

def prim_mst(nodes, graph):
    src = nodes[0]  # Start from the first node
    visited = {i: False for i in nodes}  # Keep track of visited nodes
    queue = []  # Min-heap for edges
    heapq.heappush(queue, (0, src))  # Push starting node with weight 0
    MSTWeight = 0  # Total MST weight

    while queue:
        weight, node = heapq.heappop(queue)  # Get edge with minimum weight
        if visited[node]:
            continue  # Skip if node already visited
        visited[node] = True  # Mark node as visited
        MSTWeight += weight  # Add weight to MST

        # Push all adjacent edges into heap
        for adj_node, adj_weight in graph[node]:
            if not visited[adj_node]:
                heapq.heappush(queue, (adj_weight, adj_node))
    return MSTWeight

class Solution:
    def spanningTree(self, V, adj):
        return prim_mst(list(range(V)), adj)
```

### Kruskal’s Algorithm (Python)

```python
class DisjointSet:
    def __init__(self, n):
        # Parent array and size array for Union-Find
        self.parent = [i for i in range(n + 1)]
        self.size = [1] * (n + 1)

    def find(self, node):
        # Path compression to find root parent
        if node == self.parent[node]:
            return node
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]

    def union(self, u, v):
        # Union by size
        parent_u = self.find(u)
        parent_v = self.find(v)
        if parent_u == parent_v:
            return False  # Already connected
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]
        return True

def kruskal_mst(nodes, graph):
    edges = []
    # Collect all edges (avoid duplicates in undirected graph)
    for node in graph:
        for adj_node, adj_weight in graph[node]:
            if node < adj_node:
                edges.append((adj_weight, node, adj_node))

    # Sort edges by weight
    edges.sort()
    ds = DisjointSet(len(nodes))
    mst_weight = 0

    # Process edges in increasing weight order
    for wt, u, v in edges:
        if ds.union(u, v):  # If u and v are not already connected
            mst_weight += wt  # Add edge weight to MST
    return mst_weight

class Solution:
    def spanningTree(self, V, adj):
        return kruskal_mst(list(range(V)), adj)
```

---

## Time and Space Complexity

### Prim’s Algorithm

* **Time Complexity:** `O(E log V)`

  * Each edge can be pushed into the heap → `E log V`
* **Space Complexity:** `O(V + E)`

  * For adjacency list and heap

### Kruskal’s Algorithm

* **Time Complexity:** `O(E log E)` (sorting edges) + `O(E α(V))` (Union-Find operations)

  * Effectively `O(E log E)`
* **Space Complexity:** `O(V + E)`

  * For edge list and disjoint set

---
