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

**Universal Set Representation:**
```
Initial State - Each vertex is its own disjoint set:

Universal Set = {1, 2, 3, 4, 5, 6, 7, 8}

Individual Sets:
{1}  {2}  {3}  {4}  {5}  {6}  {7}  {8}

Graph visualization:
1     2     3     4     5     6     7     8
○     ○     ○     ○     ○     ○     ○     ○

(All vertices are disconnected initially)
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

## Problem Statement

**Objective**: Given an undirected graph, use Disjoint Sets (Union-Find) to detect cycles and find the Minimum Spanning Tree (MST).

**Problem**: Process edges in order of their weights and determine:
1. Which edges can be safely added without creating cycles
2. Which edges would create cycles and should be rejected
3. Build the MST using Kruskal's algorithm approach

---

### Complete Graph Specification

**Graph Details**:
- **Vertices**: 8 vertices {1, 2, 3, 4, 5, 6, 7, 8}
- **Total Edges**: 9 edges (more than minimum needed for spanning tree)
- **Expected MST Edges**: 7 edges (for 8 vertices, MST needs exactly 7 edges)

**Complete Graph Structure**:
```
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

**Detailed Edge List with Weights**:

| Edge | Vertices | Weight | Description |
|------|----------|--------|-------------|
| E1 | (1,2) | 1 | Connect vertices 1 and 2 |
| E2 | (3,4) | 2 | Connect vertices 3 and 4 |
| E3 | (5,6) | 3 | Connect vertices 5 and 6 |
| E4 | (7,8) | 4 | Connect vertices 7 and 8 |
| E5 | (2,4) | 5 | Connect vertices 2 and 4 |
| E6 | (2,5) | 6 | Connect vertices 2 and 5 |
| E7 | (1,3) | 7 | Connect vertices 1 and 3 ⚠️ |
| E8 | (6,8) | 8 | Connect vertices 6 and 8 |
| E9 | (5,7) | 9 | Connect vertices 5 and 7 ⚠️ |

⚠️ = These edges will create cycles when processed

**Initial State - Universal Set**:
```
Universal Set: {1, 2, 3, 4, 5, 6, 7, 8}
Each element is its own set

Graph Representation (No edges included yet):
1     2     3     4     5     6     7     8
○     ○     ○     ○     ○     ○     ○     ○

Disjoint Sets:
{1}   {2}   {3}   {4}   {5}   {6}   {7}   {8}
```

**Step-by-step Processing**:

| Step | Edge | Weight | Find(u) | Find(v) | Same Set? | Action | Result Sets | Graph State |
|------|------|--------|---------|---------|-----------|--------|-------------|-------------|
| 1 | (1,2) | 1 | Set{1} | Set{2} | ❌ No | ✅ Union | S1={1,2} | `1—2  3  4  5  6  7  8` |
| 2 | (3,4) | 2 | Set{3} | Set{4} | ❌ No | ✅ Union | S1={1,2}, S2={3,4} | `1—2  3—4  5  6  7  8` |
| 3 | (5,6) | 3 | Set{5} | Set{6} | ❌ No | ✅ Union | S1={1,2}, S2={3,4}, S3={5,6} | `1—2  3—4  5—6  7  8` |
| 4 | (7,8) | 4 | Set{7} | Set{8} | ❌ No | ✅ Union | S1={1,2}, S2={3,4}, S3={5,6}, S4={7,8} | `1—2  3—4  5—6  7—8` |
| 5 | (2,4) | 5 | S1 | S2 | ❌ No | ✅ Union | S5={1,2,3,4}, S3={5,6}, S4={7,8} | `1—2—4—3  5—6  7—8` |
| 6 | (2,5) | 6 | S5 | S3 | ❌ No | ✅ Union | S6={1,2,3,4,5,6}, S4={7,8} | `1—2—5—6  7—8` <br> `  │ │` <br> `  4-3` |
| 7 | (1,3) | 7 | S6 | S6 | ✅ **YES** | ❌ **CYCLE!** | No change | Same as step 6 |
| 8 | (6,8) | 8 | S6 | S4 | ❌ No | ✅ Union | S7={1,2,3,4,5,6,7,8} | All vertices connected |
| 9 | (5,7) | 9 | S7 | S7 | ✅ **YES** | ❌ **CYCLE!** | No change | Same as step 8 |

**Visual Progress of MST Construction**:

```
Step 0: Initial State                Step 1: Add Edge (1,2)              Step 2: Add Edge (3,4)
1     2     3     4     5     6     7     8    1————2     3     4     5     6     7     8    1————2     3————4     5     6     7     8
○     ○     ○     ○     ○     ○     ○     ○    

Step 3: Add Edge (5,6)              Step 4: Add Edge (7,8)              Step 5: Add Edge (2,4)
1————2     3————4     5————6     7     8    1————2     3————4     5————6     7————8    1————2     3————4     5————6     7————8
                                                                                        │       ╲  ╱
                                                                                        └─────────4

Step 6: Add Edge (2,5)              Step 7: Try Edge (1,3)              Step 8: Add Edge (6,8)
1————2————5————6     7————8         1————2————5————6     7————8         1————2————5————6————8
│    │                              │    │                              │    │         │    │
└────4────3                         └────4────3                         └────4────3    └────7
                                    Would create cycle 1→2→4→3→1!        
                                    ❌ REJECT THIS EDGE

Step 9: Try Edge (5,7) → ❌ CYCLE DETECTED!

Final MST (7 edges for 8 vertices):
1————2————5————6————8
│    │              │
└────4────3         7

MST Edges Selected: {(1,2), (3,4), (5,6), (7,8), (2,4), (2,5), (6,8)}
Rejected Edges: {(1,3), (5,7)} - Both would create cycles
Total MST Weight: 1+2+3+4+5+6+8 = 29
```

---

## Graphical Representation

Instead of maintaining actual sets, we use a tree structure where each set has a representative (root). Let's trace through all steps to see how these trees evolve.

### Step-by-Step Tree Evolution

**Initial State - Each vertex is its own tree (forest of single nodes):**
```
Step 0: Universal Set - Each element is its own parent

1     2     3     4     5     6     7     8
○     ○     ○     ○     ○     ○     ○     ○
│     │     │     │     │     │     │     │
1     2     3     4     5     6     7     8
(each points to itself)

Sets: {1} {2} {3} {4} {5} {6} {7} {8}
```

**Step 1: Process Edge (1,2) - Union of vertices 1 and 2**
```
Find(1) = 1, Find(2) = 2 → Different sets → Union them

Before Union:     After Union:
1     2           1
○     ○           ○
│     │           │
1     2           1
                  ↑
                  2

Tree Structure:   Sets: {1,2} {3} {4} {5} {6} {7} {8}
    1             Remaining: 3  4  5  6  7  8
    │                        ○  ○  ○  ○  ○  ○
    2
```

**Step 2: Process Edge (3,4) - Union of vertices 3 and 4**
```
Find(3) = 3, Find(4) = 4 → Different sets → Union them

Tree Structures:    Sets: {1,2} {3,4} {5} {6} {7} {8}
    1       3       Remaining: 5  6  7  8
    │       │                  ○  ○  ○  ○
    2       4
```

**Step 3: Process Edge (5,6) - Union of vertices 5 and 6**
```
Find(5) = 5, Find(6) = 6 → Different sets → Union them

Tree Structures:        Sets: {1,2} {3,4} {5,6} {7} {8}
    1       3       5   Remaining: 7  8
    │       │       │              ○  ○
    2       4       6
```

**Step 4: Process Edge (7,8) - Union of vertices 7 and 8**
```
Find(7) = 7, Find(8) = 8 → Different sets → Union them

Tree Structures:            Sets: {1,2} {3,4} {5,6} {7,8}
    1       3       5       7
    │       │       │       │
    2       4       6       8
```

**Step 5: Process Edge (2,4) - Union of sets containing 2 and 4**
```
Find(2): 2 → 1 (root = 1)
Find(4): 4 → 3 (root = 3)
Different roots → Union based on weight

Weight comparison: Set{1,2} has 2 nodes, Set{3,4} has 2 nodes
Equal weights → Make 1 the parent of 3 (arbitrary choice)

Before Union:           After Union:
    1       3               1
    │       │              ╱│╲
    2       4             2 3 4

Tree Structures:            Sets: {1,2,3,4} {5,6} {7,8}
        1           5       7
       ╱│╲          │       │
      2 3 4         6       8
```

**Step 6: Process Edge (2,5) - Union of sets containing 2 and 5**
```
Find(2): 2 → 1 (root = 1)
Find(5): 5 → 5 (root = 5)
Different roots → Union based on weight

Weight comparison: Set{1,2,3,4} has 4 nodes, Set{5,6} has 2 nodes
Set 1 is larger → Make 1 the parent of 5

Before Union:               After Union:
    1           5               1
   ╱│╲          │              ╱│╲╲
  2 3 4         6             2 3 4 5
                                    │
                                    6

Tree Structures:                Sets: {1,2,3,4,5,6} {7,8}
        1               7
       ╱│╲╲             │
      2 3 4 5           8
            │
            6
```

**Step 7: Process Edge (1,3) - CYCLE DETECTION!**
```
Find(1): 1 → 1 (root = 1)
Find(3): 3 → 1 (root = 1)
SAME ROOT! → Both belong to same set → CYCLE DETECTED!

Current Tree:    Path showing cycle:
    1            1 — 2 — 4 — 3 — 1 (would form cycle)
   ╱│╲╲          ↑             ↑
  2 3 4 5        └─────────────┘
        │        Adding edge (1,3) creates this cycle
        6

❌ REJECT Edge (1,3) - No changes to tree structure
```

**Step 8: Process Edge (6,8) - Union of sets containing 6 and 8**
```
Find(6): 6 → 5 → 1 (root = 1)
Find(8): 8 → 7 (root = 7)
Different roots → Union based on weight

Weight comparison: Set{1,2,3,4,5,6} has 6 nodes, Set{7,8} has 2 nodes
Set 1 is larger → Make 1 the parent of 7

Final Tree Structure:        Sets: {1,2,3,4,5,6,7,8}
        1
      ╱│╲╲╲
     2 3 4 5 7
           │ │
           6 8
```

**Step 9: Process Edge (5,7) - CYCLE DETECTION!**
```
Find(5): 5 → 1 (root = 1)
Find(7): 7 → 1 (root = 1)
SAME ROOT! → Both belong to same set → CYCLE DETECTED!

❌ REJECT Edge (5,7) - No changes to tree structure
```

### Final Tree Representation
```
Complete Disjoint Set Tree:

        1 (Root/Representative)
      ╱─│─╲─╲─╲
     2  3  4  5  7
           │  │
           6  8

Set: {1,2,3,4,5,6,7,8}
Root: 1
Height: 3
```

### Key Concepts Illustrated

**Tree Properties:**
- **Root**: Representative of the entire set (vertex 1)
- **Parent-Child**: Each child points to its parent
- **Path Compression**: Can be applied to make trees flatter
- **Union by Rank**: Larger trees become parents of smaller trees

**Find Operation:**
```
Find(6): 6 → 5 → 1 ✓ (result: 1)
Find(8): 8 → 7 → 1 ✓ (result: 1) 
Find(3): 3 → 1 ✓     (result: 1)
```

**Union Operation Decision Making:**
- Compare tree sizes/ranks
- Attach smaller tree under root of larger tree
- Keeps overall tree height minimal

---

## Array Implementation

### Data Structure
```
parent[] = array where parent[i] represents parent of element i
parent[i] = -1 means i is a root (representative of its set)
parent[i] = -k means i is root with k elements in its set
```

### Initial State - Universal Set
```
Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -1  -1  -1  -1  -1  -1  -1  -1

Graph visualization of Universal Set:
1     2     3     4     5     6     7     8
○     ○     ○     ○     ○     ○     ○     ○
↑     ↑     ↑     ↑     ↑     ↑     ↑     ↑
-1    -1    -1    -1    -1    -1    -1    -1
(Each element is its own parent/root)

Disjoint Sets Representation:
{1}   {2}   {3}   {4}   {5}   {6}   {7}   {8}
```

### Step-by-step Array Changes

Let's trace through each step showing both the array representation and corresponding tree structure.

**Initial State - Universal Set**:
```
Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -1  -1  -1  -1  -1  -1  -1  -1

Tree Structure:
1     2     3     4     5     6     7     8
○     ○     ○     ○     ○     ○     ○     ○

Explanation: Each element is its own parent (value -1 means root with 1 element)
```

**Step 1: Process Edge (1,2)**
```
Find(1) = 1 (parent[1] = -1, so 1 is root)
Find(2) = 2 (parent[2] = -1, so 2 is root)
Union(1,2): Make 1 parent of 2, update count

Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -2   1  -1  -1  -1  -1  -1  -1

Tree Structure:
    1     3     4     5     6     7     8
   /      ○     ○     ○     ○     ○     ○
  2

Explanation: 
- parent[1] = -2 (root with 2 elements)
- parent[2] = 1 (2's parent is 1)
```

**Step 2: Process Edge (3,4)**
```
Find(3) = 3 (parent[3] = -1, so 3 is root)
Find(4) = 4 (parent[4] = -1, so 4 is root)
Union(3,4): Make 3 parent of 4, update count

Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -2   1  -2   3  -1  -1  -1  -1

Tree Structure:
    1     3     5     6     7     8
   /     /      ○     ○     ○     ○
  2     4

Explanation:
- parent[3] = -2 (root with 2 elements)
- parent[4] = 3 (4's parent is 3)
```

**Step 3: Process Edge (5,6)**
```
Find(5) = 5 (parent[5] = -1, so 5 is root)
Find(6) = 6 (parent[6] = -1, so 6 is root)
Union(5,6): Make 5 parent of 6, update count

Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -2   1  -2   3  -2   5  -1  -1

Tree Structure:
    1     3     5     7     8
   /     /     /      ○     ○
  2     4     6

Explanation:
- parent[5] = -2 (root with 2 elements)
- parent[6] = 5 (6's parent is 5)
```

**Step 4: Process Edge (7,8)**
```
Find(7) = 7 (parent[7] = -1, so 7 is root)
Find(8) = 8 (parent[8] = -1, so 8 is root)
Union(7,8): Make 7 parent of 8, update count

Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -2   1  -2   3  -2   5  -2   7

Tree Structure:
    1     3     5     7
   /     /     /     /
  2     4     6     8

Explanation:
- parent[7] = -2 (root with 2 elements)
- parent[8] = 7 (8's parent is 7)
```

**Step 5: Process Edge (2,4) - Union of different sets**
```
Find(2): 2 → parent[2] = 1 → parent[1] = -2 ✓ (root = 1)
Find(4): 4 → parent[4] = 3 → parent[3] = -2 ✓ (root = 3)
Different roots! Weight comparison: both have -2 (2 elements each)
Union(1,3): Make 1 parent of 3 (arbitrary choice since equal weights)

Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -4   1   1   3  -2   5  -2   7

Tree Structure:
        1         5     7
      / | \      /     /
     2  3  4    6     8

Explanation:
- parent[1] = -4 (root with 4 elements: 1,2,3,4)
- parent[3] = 1 (3's parent is now 1)
- Total elements in set 1: 2 + 2 = 4
```

**Step 6: Process Edge (2,5) - Union of different sets**
```
Find(2): 2 → parent[2] = 1 → parent[1] = -4 ✓ (root = 1)
Find(5): 5 → parent[5] = -2 ✓ (root = 5)
Different roots! Weight comparison: parent[1] = -4, parent[5] = -2
Set 1 is larger (4 > 2), so make 1 parent of 5

Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -6   1   1   3   1   5  -2   7

Tree Structure:
          1           7
      /  /|\ \       /
     2  3 4  5      8
            |
            6

Explanation:
- parent[1] = -6 (root with 6 elements: 1,2,3,4,5,6)
- parent[5] = 1 (5's parent is now 1)
- Total elements in set 1: 4 + 2 = 6
```

**Step 7: Process Edge (1,3) - CYCLE DETECTION!**
```
Find(1): parent[1] = -6 ✓ (root = 1)
Find(3): 3 → parent[3] = 1 → parent[1] = -6 ✓ (root = 1)
SAME ROOT! Both belong to set with root 1 → CYCLE DETECTED!

Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -6   1   1   3   1   5  -2   7
        ↑ NO CHANGES - Edge rejected ↑

Tree Structure: (No changes)
          1           7
      /  /|\ \       /
     2  3 4  5      8
            |
            6

❌ Edge (1,3) would create cycle: 1→2→4→3→1
```

**Step 8: Process Edge (6,8) - Union of different sets**
```
Find(6): 6 → parent[6] = 5 → parent[5] = 1 → parent[1] = -6 ✓ (root = 1)
Find(8): 8 → parent[8] = 7 → parent[7] = -2 ✓ (root = 7)
Different roots! Weight comparison: parent[1] = -6, parent[7] = -2
Set 1 is larger (6 > 2), so make 1 parent of 7

Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -8   1   1   3   1   5   1   7

Final Tree Structure:
            1
        / / | \ \ \
       2 3  4  5  7 
               |  |
               6  8

Explanation:
- parent[1] = -8 (root with 8 elements: all vertices)
- parent[7] = 1 (7's parent is now 1)
- All vertices now belong to one connected component
```

**Step 9: Process Edge (5,7) - CYCLE DETECTION!**
```
Find(5): 5 → parent[5] = 1 → parent[1] = -8 ✓ (root = 1)
Find(7): 7 → parent[7] = 1 → parent[1] = -8 ✓ (root = 1)
SAME ROOT! Both belong to set with root 1 → CYCLE DETECTED!

Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -8   1   1   3   1   5   1   7
        ↑ NO CHANGES - Edge rejected ↑

❌ Edge (5,7) would create another cycle in the graph
```

### Array Representation Summary

**Key Array Conventions:**
- **Negative values**: Indicate root nodes, magnitude = number of elements in set
- **Positive values**: Point to parent node
- **Find Operation**: Follow parent pointers until reaching negative value (root)
- **Union Operation**: Make root of smaller set point to root of larger set

**Final State Analysis:**
```
Index:  [1] [2] [3] [4] [5] [6] [7] [8]
Value:  -8   1   1   3   1   5   1   7

Root: 1 (parent[1] = -8, meaning 8 elements)
Tree paths:
- Find(2): 2 → 1 ✓
- Find(3): 3 → 1 ✓  
- Find(4): 4 → 3 → 1 ✓
- Find(5): 5 → 1 ✓
- Find(6): 6 → 5 → 1 ✓
- Find(7): 7 → 1 ✓
- Find(8): 8 → 7 → 1 ✓
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
