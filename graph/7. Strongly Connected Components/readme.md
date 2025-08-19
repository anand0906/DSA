# Strongly Connected Components (SCC) - Complete Guide

## Table of Contents
1. [Strongly Connected Graph](#strongly-connected-graph)
2. [Strongly Connected Components](#strongly-connected-components)
3. [SCC in Directed vs Undirected Graphs](#scc-in-directed-vs-undirected-graphs)
4. [Brute Force Solution using Floyd-Warshall](#brute-force-solution-using-floyd-warshall)
5. [Graph Transpose and SCC Behavior](#graph-transpose-and-scc-behavior)
6. [Visual Examples](#visual-examples)

---

## Strongly Connected Graph

### Definition
A **strongly connected graph** is a directed graph where there exists a path from every vertex to every other vertex. In other words, for any two vertices u and v, there must be a directed path from u to v AND a directed path from v to u.

### Key Properties
- Every vertex can reach every other vertex
- The entire graph forms one single strongly connected component
- If you can traverse from any vertex and return to it while visiting all other vertices, the graph is strongly connected

### Visual Example

```
Strongly Connected Graph:
    A → B
    ↑   ↓
    D ← C

Paths exist:
A→B→C→D→A (main cycle)
A→B, A→C, A→D, B→A, B→C, B→D, C→A, C→B, C→D, D→A, D→B, D→C
```

```
NOT Strongly Connected Graph:
    A → B → C
        ↓
        D

Missing paths: B→A, C→A, C→B, D→A, D→B, D→C (among others)
No way to return to earlier vertices
```

---

## Strongly Connected Components

### Definition
A **Strongly Connected Component (SCC)** is a maximal set of vertices in a directed graph such that:
1. Every vertex in the component can reach every other vertex in the same component
2. No vertex outside the component can be added while maintaining the strongly connected property

### Key Characteristics
- **Maximal**: You cannot add any more vertices to an SCC without breaking the strongly connected property
- **Partition**: SCCs partition the vertices of a directed graph (every vertex belongs to exactly one SCC)
- **Single Vertex**: An isolated vertex (or vertex with no incoming/outgoing edges within its component) forms its own SCC

### Visual Example

```
Graph with Multiple SCCs:

    SCC1: {A, B}     SCC2: {C}     SCC3: {D, E}
    
    A → B  →  C  →  D → E
    ↑   ↓           ↑   ↓
    └───┘           └───┘
    
    - SCC1: A and B form a cycle (A→B→A)
    - SCC2: C is alone (can't reach back to A,B and can't be reached from D,E)  
    - SCC3: D and E form a cycle (D→E→D)
```

---

## SCC in Directed vs Undirected Graphs

### Directed Graphs
- SCCs are the main concept we work with
- Direction of edges matters critically
- A graph can have multiple SCCs
- Each SCC is a maximal strongly connected subgraph

```
Directed Graph Example:
    1 → 2 → 3
    ↑       ↓
    6 ← 5 ← 4

SCCs: {1,2,3,4,5,6} - This forms ONE SCC because:
1→2→3→4→5→6→1 (cycle exists)
```

### Undirected Graphs
- In undirected graphs, the concept becomes **Connected Components**
- Every edge can be traversed in both directions
- If there's a path between two vertices, they're in the same connected component
- Each connected component in an undirected graph would be an SCC if we consider each undirected edge as two directed edges

```
Undirected Graph Example:
    1—2—3    7—8
    |   |    |
    6—5—4    9

Connected Components: {1,2,3,4,5,6} and {7,8,9}
If converted to directed: each edge becomes two directed edges
```

---

## Brute Force Solution using Floyd-Warshall

### Algorithm Overview
Floyd-Warshall finds shortest paths between all pairs of vertices. We can use it to check strong connectivity by verifying if every vertex can reach every other vertex.

### Steps for SCC Detection using Floyd-Warshall

1. **Create Reachability Matrix**: Use Floyd-Warshall to find if vertex i can reach vertex j
2. **Check Strong Connectivity**: A graph is strongly connected if distance[i][j] != float('inf') for all pairs (i,j)
3. **Find SCCs**: Group vertices that can reach each other

### Algorithm Implementation Logic

```
1. Initialize reachability matrix with direct edges
2. Apply Floyd-Warshall to find transitive closure
3. For each vertex pair (i,j):
   - If reachability[i][j] AND reachability[j][i] are not equal to infinity,
     then i and j are in the same SCC
4. Group vertices into SCCs based on mutual reachability
```

### Time Complexity
- **Time**: O(V³) where V is number of vertices
- **Space**: O(V²) for reachability matrix
- **Not optimal** for large graphs (better algorithms exist like Tarjan's or Kosaraju's)

---

## Graph Transpose and SCC Behavior

### What is Graph Transpose?
The **transpose of a directed graph G** (denoted as G^T) is obtained by reversing the direction of all edges.

```
Original Graph G:        Transpose G^T:
    A → B                   A ← B
    ↓   ↓                   ↑   ↑
    D ← C                   D → C
```

### Key Properties of Transpose

1. **Same SCCs**: G and G^T have exactly the same strongly connected components
2. **Reversed Edges**: Only edge directions are reversed, not the connectivity within SCCs
3. **SCC Structure Preserved**: The internal structure of each SCC remains the same

### Why SCCs Remain Same in Transpose?

**Proof by Logic**:
- If vertices u and v are in the same SCC in G:
  - There exists path u → v in G
  - There exists path v → u in G
- In G^T:
  - Path u → v in G becomes v → u in G^T
  - Path v → u in G becomes u → v in G^T
  - So u and v can still reach each other in G^T
  - Therefore, they remain in the same SCC

### Visual Example of Transpose Effect

```
Original Graph:
    A → B → C
    ↑       ↓
    E ← D ←─┘
    
SCC: {A,B,C,D,E} - All vertices form one cycle: A→B→C→D→E→A

Transpose Graph:
    A ← B ← C
    ↓       ↑
    E → D →─┘
    
SCC: {A,B,C,D,E} - Same vertices, cycle becomes: A→E→D→C→B→A

The SCC structure is preserved!
```

### Applications of Transpose in SCC Algorithms

**Kosaraju's Algorithm** uses this property:
1. Perform DFS on original graph to get finish times
2. Create transpose graph
3. Perform DFS on transpose in decreasing order of finish times
4. Each DFS tree in step 3 gives one SCC

---

## Visual Examples

### Example 1: Simple SCC

```
Graph:
    1 → 2
    ↑   ↓
    4 ← 3

Analysis:
- Path 1→2→3→4→1 exists
- All vertices can reach all others
- This is ONE SCC: {1,2,3,4}
```

### Example 2: Multiple SCCs

```
Graph:
    1 → 2 → 3 → 7 → 8
    ↑       ↓   ↑   ↓
    6 ← 5 ← 4   10← 9

Analysis:
SCC1: {1,2,3,4,5,6} - cycle: 1→2→3→4→5→6→1
SCC2: {7,8,9,10} - cycle: 7→8→9→10→7
```

### Example 3: Linear Chain

```
Graph: 1 → 2 → 3 → 4 → 5

Analysis:
- No vertex can reach back to previous vertices
- Each vertex forms its own SCC
- SCCs: {1}, {2}, {3}, {4}, {5}
```

---

### Key Takeaways

1. **Strongly Connected Graph**: Every vertex can reach every other vertex
2. **SCC**: Maximal strongly connected subgraph
3. **Direction Matters**: Only applies to directed graphs
4. **Floyd-Warshall Approach**: O(V³) brute force method using reachability
5. **Transpose Property**: G and G^T have identical SCCs
6. **Practical Applications**: Social networks, web page ranking, circuit design

### When to Use Each Approach

- **Floyd-Warshall**: Small graphs, when you also need shortest paths
- **Tarjan's Algorithm**: O(V+E), single pass, optimal for most cases
- **Kosaraju's Algorithm**: O(V+E), easy to understand, uses transpose property

