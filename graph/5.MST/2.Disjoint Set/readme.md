# Disjoint Set

Design a disjoint set (also called union-find) data structure that efficiently manages a collection of non-overlapping sets in a dynamic graph environment. Think of it like organizing people into groups where groups can merge over time, and you need to quickly check if two people belong to the same group.

The data structure should support:
- **Initialization**: Create n separate sets, each containing one element
- **Union by Rank**: Merge two sets using rank optimization (keeps trees shallow based on height)
- **Union by Size**: Merge two sets using size optimization (attaches smaller component to larger)
- **Find**: Check if two elements belong to the same set by comparing their ultimate parents

This is particularly useful for **dynamic graphs** - graphs that continuously change their configuration as edges are added one by one.

## Examples

### Example 1
#### Input
```python
["DisjointSet", "unionByRank", "unionBySize", "find", "find"]
[[5], [0, 1], [2, 3], [0, 1], [0, 3]]
```

#### Output
```python
[null, null, null, true, false]
```

#### Explanation
```python
ds = DisjointSet(5)     # Initialize with 5 elements: {0}, {1}, {2}, {3}, {4}
ds.unionByRank(0, 1)    # Merge sets: {0,1}, {2}, {3}, {4}
ds.unionBySize(2, 3)    # Merge sets: {0,1}, {2,3}, {4}
ds.find(0, 1)           # True - both in same set {0,1}
ds.find(0, 3)           # False - 0 in {0,1}, 3 in {2,3}
```

### Example 2
#### Input
```python
["DisjointSet", "unionBySize", "unionBySize", "find", "find"]
[[3], [0, 1], [1, 2], [0, 2], [0, 1]]
```

#### Output
```python
[null, null, null, true, true]
```

#### Explanation
```python
ds = DisjointSet(3)     # Initialize: {0}, {1}, {2}
ds.unionBySize(0, 1)    # Merge: {0,1}, {2}
ds.unionBySize(1, 2)    # Merge all: {0,1,2}
ds.find(0, 2)           # True - all in same set
ds.find(0, 1)           # True - all in same set
```

Think of it like merging friend groups on social media - when two people become friends, their entire friend networks can potentially connect.

## Solution

Use arrays to track parent-child relationships and component properties:
- **Parent array**: stores the ultimate parent (root) of each node
- **Rank array**: tracks tree height for union by rank optimization
- **Size array**: tracks component size for union by size optimization
- **Path compression**: flattens tree structure during find operations for efficiency

## Intuition

Imagine organizing a company merger scenario:
- **Union by Rank**: When companies merge, the one with deeper management hierarchy becomes the parent (keeps structure shallow)
- **Union by Size**: The larger company absorbs the smaller one (more intuitive and maintains accurate sizes)
- **Path compression**: Employees get direct reporting lines to the CEO after each query (flattens hierarchy)

The key insight is maintaining efficient tree structures while enabling fast connectivity queries. Union by Size is often preferred because it's more intuitive and doesn't get distorted by path compression like ranks do.

## Approach Steps

1. **Initialize**: 
   - Each element starts as its own parent: `parent[i] = i`
   - Set initial rank = 0 and size = 1 for all elements

2. **Find Operation with Path Compression**:
   - Follow parent pointers until reaching root (self-parent)
   - During backtracking, connect each node directly to root
   - This flattens the tree for future queries

3. **Union by Rank**:
   - Find ultimate parents of both nodes
   - If different, attach tree with smaller rank under tree with larger rank
   - If ranks equal, choose one as parent and increment its rank
   - **Note**: Ranks can become inaccurate after path compression

4. **Union by Size**:
   - Find ultimate parents of both nodes
   - If different, attach smaller component under larger component
   - Update size of new root by adding both component sizes
   - **Advantage**: Sizes remain accurate even with path compression

5. **Find Query**:
   - Use find operation to get ultimate parents of both nodes
   - Return true if ultimate parents are the same

## Code

```python
class DisjointSet:
    def __init__(self, n: int):
        """
        Initialize Disjoint Set (Union-Find) for n elements.
        Each node is its own parent initially.
        """
        self.parent = [i for i in range(n)]       # parent[i] = parent of i
        self.rank = [0] * n                       # rank[i] = height of tree rooted at i
        self.size = [1] * n                       # size[i] = size of component rooted at i
    
    def find_ultimate_parent(self, node: int) -> int:
        """
        Find the ultimate parent (root) of a node with path compression.
        Path compression: Connect each node directly to root during backtracking.
        """
        if node == self.parent[node]:
            return node
        
        # Path compression: directly connect node to its ultimate parent
        self.parent[node] = self.find_ultimate_parent(self.parent[node])
        return self.parent[node]
    
    def union_by_rank(self, u: int, v: int):
        """
        Union two sets by rank (tree height).
        Attach tree with smaller rank under tree with larger rank.
        """
        parent_u = self.find_ultimate_parent(u)
        parent_v = self.find_ultimate_parent(v)
        
        if parent_u == parent_v:
            return  # Already in same set
        
        # Attach smaller rank tree under larger rank tree
        if self.rank[parent_u] < self.rank[parent_v]:
            self.parent[parent_u] = parent_v
        elif self.rank[parent_v] < self.rank[parent_u]:
            self.parent[parent_v] = parent_u
        else:
            # Equal ranks: choose one as parent and increment its rank
            self.parent[parent_v] = parent_u
            self.rank[parent_u] += 1
    
    def union_by_size(self, u: int, v: int):
        """
        Union two sets by size (number of elements).
        Attach smaller component under larger component.
        """
        parent_u = self.find_ultimate_parent(u)
        parent_v = self.find_ultimate_parent(v)
        
        if parent_u == parent_v:
            return  # Already in same set
        
        # Attach smaller component under larger component
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]
    
    def find(self, u: int, v: int) -> bool:
        """
        Check if two nodes belong to the same set.
        Returns True if they have the same ultimate parent.
        """
        return self.find_ultimate_parent(u) == self.find_ultimate_parent(v)

# Example Usage
ds = DisjointSet(5)

# Initial state: {0}, {1}, {2}, {3}, {4}
print(ds.find(0, 1))    # False

ds.union_by_rank(0, 1)   # {0,1}, {2}, {3}, {4}
ds.union_by_size(2, 3)   # {0,1}, {2,3}, {4}

print(ds.find(0, 1))    # True
print(ds.find(0, 3))    # False
print(ds.find(2, 3))    # True

ds.union_by_size(1, 2)   # {0,1,2,3}, {4}
print(ds.find(0, 3))    # True
```

## Time and Space Complexity

**Time Complexity:**
- **Find with Path Compression**: O(α(n)) - Nearly constant time, where α is the inverse Ackermann function
- **Union by Rank/Size**: O(α(n)) - Same as find since we need to find roots first
- **Overall**: O(4α) ≈ O(1) for practical purposes (α(n) ≤ 4 for any reasonable input size)

**Space Complexity:**
- **O(n)** - We need arrays for parent, rank, and size, each of size n
- **Total**: O(3n) = O(n)

**Why Union by Size is Preferred:**
- **Intuitive**: Size represents actual number of elements, easier to understand
- **Robust**: Sizes remain accurate even after path compression, unlike ranks
- **Performance**: Same time complexity as union by rank but with more meaningful metadata

**Dynamic Graph Advantage:**
Instead of running DFS/BFS (O(N+E)) every time the graph changes, disjoint set allows:
- Adding edges: O(α(n)) per edge
- Connectivity queries: O(α(n)) per query
- Perfect for scenarios where graph structure changes frequently and connectivity queries are frequent


----

# Simple Disjoint Set

Design a disjoint set (also called union-find) data structure that efficiently manages a collection of non-overlapping sets. Think of it like organizing people into groups - initially everyone is in their own group, but you can merge groups together and quickly find which group someone belongs to.

The data structure should support:
- **Initialization**: Create n separate sets, each containing one element
- **Union by Rank**: Merge two sets using rank optimization (keeps trees shallow)
- **Union by Size**: Merge two sets using size optimization (attaches smaller tree to larger)
- **Find**: Determine which set an element belongs to (find the group leader)

## Examples

### Input
```python
# Create disjoint set with 5 elements (0, 1, 2, 3, 4)
ds = DisjointSet(4)

# Initially: {0}, {1}, {2}, {3}, {4}
print(ds.find(0))  # 0 (0 is its own parent)
print(ds.find(1))  # 1 (1 is its own parent)

# Union 1 and 2
ds.union_by_rank(1, 2)
# Now: {0}, {1, 2}, {3}, {4}

# Union 2 and 3
ds.union_by_rank(2, 3)
# Now: {0}, {1, 2, 3}, {4}
```

### Output
```python
print(ds.find(0))  # 0 (still its own parent)
print(ds.find(1))  # 1 (representative of {1, 2, 3})
print(ds.find(2))  # 1 (same group as 1)
print(ds.find(3))  # 1 (same group as 1)
```

### Explanation

Think of it like a company hierarchy:
- Initially, each person is their own "department head"
- When departments merge, one head becomes the boss of the other
- To find someone's ultimate boss, you keep going up the chain until you reach the top
- Path compression is like creating direct shortcuts to the top boss for faster future lookups

## Solution

Use two arrays to track relationships:
- **Parent array**: stores the immediate parent of each node
- **Rank/Size arrays**: help decide which tree should become the parent during union operations
- **Path compression**: flattens the tree structure during find operations for efficiency

## Intuition

Imagine you're organizing a tournament bracket:
- **Union by Rank**: When merging teams, put the smaller bracket under the larger one to keep the overall structure shallow
- **Union by Size**: Attach the team with fewer members to the team with more members
- **Path compression**: Once you find the champion, create direct connections so future searches are faster

The key insight is that we want to keep our "family trees" as flat as possible to make searches super fast.

## Approach Steps

1. **Initialize**: 
   - Each element starts as its own parent
   - Set initial rank = 0 and size = 1 for all elements

2. **Find Operation**:
   - Follow parent pointers until you reach the root (self-parent)
   - Apply path compression: make every node point directly to the root

3. **Union by Rank**:
   - Find roots of both elements
   - If they're the same, do nothing (already connected)
   - Attach the tree with smaller rank under the tree with larger rank
   - If ranks are equal, choose one as parent and increment its rank

4. **Union by Size**:
   - Find roots of both elements
   - Attach the smaller tree under the larger tree
   - Update the size of the new root

## Code

```python
class DisjointSet:
    def __init__(self, n: int):
        """
        Initialize Disjoint Set (Union-Find) for n elements.
        Each node is its own parent initially, with rank = 0 and size = 1.
        """
        self.parent = [i for i in range(n + 1)]   # parent[i] = parent of i
        self.rank = [0] * (n + 1)                 # rank[i] = rank of tree rooted at i
        self.size = [1] * (n + 1)                 # size[i] = size of tree rooted at i
    
    def find(self, node: int) -> int:
        """
        Find the ultimate parent (representative) of a node.
        Uses path compression optimization.
        """
        if node == self.parent[node]:
            return node
        # Path compression: directly connect node to its ultimate parent
        self.parent[node] = self.find(self.parent[node])
        return self.parent[node]
    
    def union_by_size(self, u: int, v: int):
        """
        Union two sets by size.
        Attach the smaller tree under the larger one.
        """
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return  # already in the same set
        
        if self.size[parent_u] < self.size[parent_v]:
            self.parent[parent_u] = parent_v
            self.size[parent_v] += self.size[parent_u]
        else:
            self.parent[parent_v] = parent_u
            self.size[parent_u] += self.size[parent_v]
    
    def union_by_rank(self, u: int, v: int):
        """
        Union two sets by rank.
        Attach the smaller rank tree under the higher rank tree.
        If ranks are equal, attach one under the other and increase rank.
        """
        parent_u = self.find(u)
        parent_v = self.find(v)
        
        if parent_u == parent_v:
            return  # already in the same set
        
        if self.rank[parent_u] < self.rank[parent_v]:
            self.parent[parent_u] = parent_v
        elif self.rank[parent_v] < self.rank[parent_u]:
            self.parent[parent_v] = parent_u
        else:
            self.parent[parent_v] = parent_u
            self.rank[parent_u] += 1

# Example Usage
ds = DisjointSet(5)
ds.union_by_rank(1, 2)
ds.union_by_rank(3, 4)
ds.union_by_size(2, 4)
print(ds.find(1) == ds.find(4))  # True - they're in the same set
```

## Time and Space Complexity

**Time Complexity:**
- **Find**: O(α(n)) - Nearly constant time due to path compression, where α is the inverse Ackermann function
- **Union**: O(α(n)) - Same as find since we need to find roots first
- In practical terms, α(n) ≤ 4 for any reasonable input size, so it's essentially O(1)

**Space Complexity:**
- **O(n)** - We need three arrays (parent, rank, size) each of size n

**Why it's so fast:**
- Path compression flattens the tree during find operations
- Union by rank/size keeps trees balanced and shallow
- Together, these optimizations make operations nearly constant time

**Real-world analogy**: It's like having a company directory that gets automatically updated with shortcuts every time someone looks up their boss's boss - eventually, everyone has a direct line to the CEO!

---

Perfect question 🚀. Disjoint Set Union (DSU), also called **Union-Find**, is one of the most useful data structures in Graphs and Network-related problems. Let’s go step by step.

---

# 🔹 What is a Disjoint Set (Union-Find)?

A **Disjoint Set** is a data structure that keeps track of a **partition of elements into groups (or sets)**.

* Each element belongs to exactly one set.
* You can quickly find which set an element belongs to (`find` operation).
* You can merge two sets together (`union` operation).

This is super useful when you need to know **connectivity**:
👉 “Are these two elements in the same group or not?”

---

# 🔹 Core Operations

1. **Find** → Identify which group an element belongs to.
2. **Union** → Merge two groups into one.
3. **Connected?** → Check if two elements are in the same set.

---

# 🔹 Real-Time Use Cases with Examples

---

## 1. **Cycle Detection in Graphs**

📌 **Problem:** You are given an undirected graph. Check if it has a cycle.

👉 **How DSU helps:**

* Initially, each node is in its own set.
* For every edge `(u, v)`:

  * If `find(u) == find(v)`, then `u` and `v` are already connected → adding this edge creates a cycle.
  * Otherwise, union them.

✅ **Real-life analogy:**
Think of **roads between cities**. If connecting two cities creates a loop of roads where you can return to the starting point without retracing, that’s a cycle.

---

## 2. **Minimum Spanning Tree (MST) – Kruskal’s Algorithm**

📌 **Problem:** You want to connect all cities with roads such that the total road cost is minimum (no cycles allowed).

👉 **How DSU helps:**

* Sort all edges by weight.
* For each edge `(u, v)`:

  * If `u` and `v` are in different sets → union them and add the edge to MST.
  * If they are in the same set → skip (to avoid cycles).

✅ **Real-life analogy:**
Building a **telecom network** or **electricity grid** → connect all stations with minimum wire cost while ensuring no unnecessary loops.

---

## 3. **Network Connectivity**

📌 **Problem:** You are given computers and cables. You need to check if all computers are connected or if extra cables are required.

👉 **How DSU helps:**

* Each computer starts in its own set.
* For each cable `(u, v)`, do union.
* At the end, check if all computers have the same parent (or count unique parents).

✅ **Real-life analogy:**
Think of **LAN connections in an office**. If some computers are isolated, they won’t communicate → DSU helps detect and fix this.

---

## 4. **Dynamic Connectivity (Friend Circles / Social Networks)**

📌 **Problem:** You have a social network. When two people become friends, they should belong to the same group.

👉 **How DSU helps:**

* Initially, each person is alone.
* On each "friendship" `(u, v)`, do union.
* To check if two people are connected (directly or indirectly), just check `find(u) == find(v)`.

✅ **Real-life analogy:**
Facebook/LinkedIn → Checking if **two users are in the same friend circle**.

---

## 5. **Image Processing (Connected Components in a Grid)**

📌 **Problem:** Given a binary image (0 = background, 1 = object), count connected components of `1`s.

👉 **How DSU helps:**

* Treat each pixel as a node.
* If two adjacent pixels are `1`, do union.
* At the end, count distinct parents → number of connected objects.

✅ **Real-life analogy:**
Medical Imaging (MRI scans) → Identify separate tumors in an image.

---

## 6. **Percolation / Clustering Problems**

📌 **Problem:** Given points in 2D/3D, group them into clusters if they are close enough.

👉 **How DSU helps:**

* Treat each point as a node.
* If distance between two points < threshold → union them.
* Finally, count number of groups.

✅ **Real-life analogy:**
Geographical clustering (villages within a certain distance form a “district”).

---

## 7. **Accounts Merge Problem (Leetcode 721)**

📌 **Problem:** You are given multiple user accounts with emails. If two accounts share at least one email, they belong to the same user.

👉 **How DSU helps:**

* Each account/email is a node.
* Union accounts if they share an email.
* At the end, collect merged groups.

✅ **Real-life analogy:**
Bank KYC system → Merge duplicate accounts of the same customer.

---

# 🔹 Summary Table

| Use Case             | Real-world Analogy                                      |
| -------------------- | ------------------------------------------------------- |
| Cycle Detection      | Detecting loops in road maps                            |
| MST (Kruskal)        | Building cheapest telecom/electric grid                 |
| Network Connectivity | Ensuring computers in a LAN are connected               |
| Social Networks      | Friend circles (are two people connected?)              |
| Image Processing     | Identifying separate objects in MRI scans               |
| Clustering           | Forming districts from nearby villages                  |
| Accounts Merge       | Merging duplicate user accounts in banking/social media |

---

👉 So, in **one line**:
**Disjoint Set is best whenever you need to manage “groups” dynamically and answer queries like: “Are these two in the same group?” or “Merge these two groups.”**

---

Would you like me to also **draw a visual flow (with diagrams) for one of these use cases** (like MST or cycle detection) so you can use it in your YouTube/explaining content?

