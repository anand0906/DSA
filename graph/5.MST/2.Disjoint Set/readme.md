# Disjoint Sets (Union-Find) - Complete Guide

## 📋 Table of Contents
- [What are Disjoint Sets?](#what-are-disjoint-sets)
- [Basic Operations](#basic-operations)
- [Cycle Detection in Graphs](#cycle-detection-in-graphs)
- [Graphical Representation](#graphical-representation)
- [Array Implementation](#array-implementation)
- [Optimizations](#optimizations)
- [Applications](#applications)

---

## What are Disjoint Sets?

Disjoint sets are a data structure that keeps track of a set of elements partitioned into disjoint (non-overlapping) subsets. They are different from mathematical sets as they are optimized for algorithmic purposes.

### Key Properties
- **Disjoint**: No element is common between any two sets
- **Partition**: Every element belongs to exactly one set
- **Dynamic**: Sets can be merged using Union operation

### Example Visualization
```
Graph with two components:
Component 1: {1, 2, 3, 4}
Component 2: {5, 6, 7, 8}

S1 ∩ S2 = ∅ (empty set - they are disjoint)
```

---

## Basic Operations

### 1. Find Operation
**Purpose**: Determine which set an element belongs to

```
find(5) → Set 2
find(3) → Set 1
find(7) → Set 2
```

### 2. Union Operation
**Purpose**: Merge two sets containing different elements

**Trigger**: When adding an edge (u,v) in a graph:
1. Find set of u
2. Find set of v
3. If different sets → Union them
4. If same set → Cycle detected!

```
Before: S1 = {1,2,3,4}, S2 = {5,6,7,8}
Add edge 4-8
After: S3 = {1,2,3,4,5,6,7,8}
```

---

## Cycle Detection in Graphs

Disjoint sets are particularly useful for detecting cycles in undirected graphs (used in Kruskal's algorithm).

### Algorithm Steps
1. Initialize each vertex as its own set
2. For each edge (u,v):
   - Find set of u
   - Find set of v
   - If same set → **Cycle detected!**
   - If different sets → Union them

### Example Walkthrough

**Graph**: 8 vertices with edges to be processed in order

Original Graph with ALL possible edges:

    1 ————— 2 ————— 5
    |   ╲   |   ╱   |
    |    ╲  |  ╱    |
    3 ————— 4       6
            |       |
            |       |
            8 ————— 7

All Edges in the Graph:
(1,2), (1,3), (2,4), (2,5), (3,4), (4,8), (5,6), (6,8), (7,8)

Note: Some edges like (1,3) and (5,7) will create cycles

```
Initial State:
Universal Set: {1, 2, 3, 4, 5, 6, 7, 8}
Each element is its own set
```

**Step-by-step Processing**:

| Step | Edge | Find(u) | Find(v) | Action | Result Sets |
|------|------|---------|---------|---------|-------------|
| 1 | 1-2 | Set{1} | Set{2} | Union | S1={1,2} |
| 2 | 3-4 | Set{3} | Set{4} | Union | S1={1,2}, S2={3,4} |
| 3 | 5-6 | Set{5} | Set{6} | Union | S1={1,2}, S2={3,4}, S3={5,6} |
| 4 | 7-8 | Set{7} | Set{8} | Union | S1={1,2}, S2={3,4}, S3={5,6}, S4={7,8} |
| 5 | 2-4 | S1 | S2 | Union | S5={1,2,3,4}, S3={5,6}, S4={7,8} |
| 6 | 2-5 | S5 | S3 | Union | S6={1,2,3,4,5,6}, S4={7,8} |
| 7 | 1-3 | S6 | S6 | **CYCLE!** | Don't include this edge |

---

## Graphical Representation

Instead of maintaining actual sets, we use a tree structure where each set has a representative (root).

### Tree Structure
```
Set Representation using Parent-Child relationships:

Set 1: {1,2,3,4}    Set 2: {5,6}       Set 3: {7,8}
      1                  5                 7
     /|\                 |                 |
    2 3 4                6                 8
```

### Union Operation in Trees
When performing Union(Set1, Set2):
1. Find root of Set1
2. Find root of Set2
3. Make one root the parent of the other

**Weight-based Union**: Always make the root of the larger set the parent of the smaller set's root.

---

## Array Implementation

### Data Structure
```
parent[] = array where parent[i] represents parent of element i
parent[i] = -1 means i is a root (representative of its set)
parent[i] = -k means i is root with k elements in its set
```

### Initial State
```
Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -1  -1  -1  -1  -1  -1  -1  -1
```

### Step-by-step Array Changes

**After Edge 1-2**:
```
Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -2   1  -1  -1  -1  -1  -1  -1
```

**After Edge 3-4**:
```
Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -2   1  -2   3  -1  -1  -1  -1
```

**After Union of sets containing 2 and 4**:
```
Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -4   1   1   3  -1  -1  -1  -1
```

### Find Operation Implementation
```python
def find(x):
    if parent[x] < 0:
        return x  # x is root
    else:
        return find(parent[x])  # recursively find root
```

### Union Operation Implementation
```python
def union(x, y):
    root_x = find(x)
    root_y = find(y)
    
    if root_x == root_y:
        return "CYCLE DETECTED"
    
    # Weighted union: smaller tree becomes subtree of larger tree
    if parent[root_x] < parent[root_y]:  # root_x has more elements
        parent[root_x] += parent[root_y]
        parent[root_y] = root_x
    else:
        parent[root_y] += parent[root_x]
        parent[root_x] = root_y
```

---

## Optimizations

### 1. Union by Rank (Weight)
- Always attach the smaller tree under the root of the larger tree
- Keeps the tree height minimal
- **Time Complexity**: O(log n) for find operation

### 2. Path Compression (Collapsing Find)
- During find operation, make every node point directly to the root
- Subsequent find operations become O(1)

**Before Path Compression**:
```
    1
   /
  3
 /
4
```

**After finding 4**:
```
    1
   /|
  3 4
```

**Implementation**:
```python
def find_with_compression(x):
    if parent[x] < 0:
        return x
    else:
        parent[x] = find_with_compression(parent[x])  # Path compression
        return parent[x]
```

### Combined Time Complexity
With both optimizations: **Nearly O(1)** amortized time for both Union and Find operations.

---

## Applications

### 1. Kruskal's Minimum Spanning Tree Algorithm
- Sort edges by weight
- For each edge, check if it creates a cycle using Union-Find
- If no cycle, include the edge in MST

### 2. Network Connectivity
- Determine if two nodes are connected
- Find connected components in a network

### 3. Image Processing
- Connected component labeling
- Region growing algorithms

### 4. Social Network Analysis
- Find groups of connected people
- Detect communities in social graphs

---

## Implementation Alternatives

### Linked List Representation
- Each set is a linked list
- First element is the representative
- Union by appending one list to another
- **Time Complexity**: O(n) for union in worst case

### Comparison Table

| Operation | Array (Basic) | Array (Optimized) | Linked List |
|-----------|---------------|-------------------|-------------|
| Find | O(n) | O(α(n)) ≈ O(1) | O(1) |
| Union | O(n) | O(α(n)) ≈ O(1) | O(n) |
| Space | O(n) | O(n) | O(n) |

*α(n) is the inverse Ackermann function, which is practically constant for all reasonable values of n.*

---

## Summary

Disjoint Sets (Union-Find) is a powerful data structure for:
- ✅ Cycle detection in undirected graphs
- ✅ Finding connected components
- ✅ Kruskal's MST algorithm
- ✅ Dynamic connectivity queries

**Key Takeaways**:
1. Two main operations: Find and Union
2. Cycle detected when both endpoints of an edge belong to the same set
3. Optimizations make operations nearly constant time
4. Simple array implementation is sufficient for most use cases
