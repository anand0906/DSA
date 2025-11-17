# Graph Data Structures and Algorithms Guide

## Table of Contents
1. [Data Structure Classification](#data-structure-classification)
2. [What is a Graph?](#what-is-a-graph)
3. [Applications of Graphs](#applications-of-graphs)
4. [Types of Graphs](#types-of-graphs)
5. [Graph Terminology](#graph-terminology)
6. [Graph Representation](#graph-representation)

---

## Table of Contents
1. [Introduction](#introduction)
2. [Graph Representations](#graph-representations)
   - [Adjacency List](#adjacency-list)
   - [Adjacency Matrix](#adjacency-matrix)
3. [Graph Traversal Algorithms](#graph-traversal-algorithms)
   - [Breadth-First Search (BFS)](#breadth-first-search-bfs)
   - [Depth-First Search (DFS)](#depth-first-search-dfs)
4. [Complete Examples](#complete-examples)
5. [Time and Space Complexity](#time-and-space-complexity)

## Data Structure Classification

Data structures can be broadly classified into two main categories:

### Linear Data Structures
Data elements are arranged in a sequential (linear) manner.
- **Examples**: Arrays, Linked Lists, Stacks, Queues

### Non-Linear Data Structures
Data elements are not arranged in a sequential manner. They form a hierarchical or interconnected structure.
- **Examples**: Trees, Graphs

---

## What is a Graph?

A **Graph** is a non-linear data structure consisting of:
- **Nodes/Vertices/Points**: Individual data elements
- **Edges/Lines/Arcs**: Connections between pairs of nodes

### Key Definitions:
- A graph is a finite set of nodes and edges used to connect pairs of nodes
- A pictorial representation of a set of objects where some pairs are connected by links
- Can be considered as a cyclic tree

### Example Graph Structure:
```
Vertices: V = {A, B, C, D, E}
Edges: E = {(A,B), (B,C), (C,E), (E,D), (D,B), (D,A)}
```

---

## Applications of Graphs

Graphs have numerous real-world applications:

### Social Media & E-commerce
- **Recommendation Systems**: Used in social media platforms and e-commerce applications to suggest friends, products, or content

### Computer Science
- **Flow of Computation**: Represent computational processes and data flow

### Navigation Systems
- **Google Maps**: Uses graphs where road intersections are vertices and roads are edges
- **Shortest Path Algorithms**: Calculate optimal routes between locations

### World Wide Web
- **Web Page Linking**: Pages connect to others through hyperlinks, forming a graph structure

---

## Types of Graphs

### 1. Null Graph
- **Definition**: Contains vertices but no edges between them
- **Characteristics**: n nodes, zero edges

### 2. Finite Graph
- **Definition**: Contains a finite number of vertices and edges
- **Characteristics**: Limited, countable elements

### 3. Infinite Graph
- **Definition**: Contains infinite number of vertices and edges
- **Characteristics**: Unbounded structure

### 4. Cyclic Graph
- **Definition**: Contains at least one cycle
- **Characteristics**: Closed paths exist within the graph

### 5. Acyclic Graph
- **Definition**: Contains no cycles
- **Characteristics**: No closed paths, tree-like structure

### 6. Simple Graph
- **Definition**: Only one edge between any pair of vertices
- **Characteristics**: No multiple edges or self-loops

### 7. Multi Graph
- **Definition**: Multiple edges between pairs of vertices
- **Characteristics**: No self-loops but allows parallel edges

### 8. Complete Graph
- **Definition**: Edge exists between every pair of vertices
- **Characteristics**: Every complete graph is also a simple graph
- **Degree**: Each node has degree = (number of vertices - 1)

### 9. Pseudo Graph
- **Definition**: Contains at least one self-loop
- **Characteristics**: Vertices can connect to themselves

### 10. Regular Graph
- **Definition**: All vertices have equal degree
- **Note**: Complete graphs are regular, but not all regular graphs are complete

### 11. Connected Graph
- **Definition**: At least one path exists between every pair of vertices
- **Characteristics**: No isolated components

### 12. Disconnected Graph
- **Definition**: No path exists between at least one pair of vertices
- **Characteristics**: Contains isolated components

### 13. Directed Graph (Digraph)
- **Definition**: All edges have direction indicating traversal order
- **Representation**: G = (V, E) where edges map to ordered pairs (Vi, Vj)
- **Example**: Like following someone on social media (one-way relationship)

### 14. Undirected Graph
- **Definition**: No orientation/direction on edges
- **Characteristics**: All edges are bidirectional
- **Example**: Facebook friendship (mutual relationship)

### 15. Weighted Graph
- **Definition**: Edges have associated values/weights
- **Use Cases**: Represent cost, distance, or capacity

### 16. Unweighted Graph
- **Definition**: No values associated with edges
- **Default**: All graphs are unweighted unless specified

### 17. Dense & Sparse Graphs
- **Dense Graph**: Number of edges close to maximum possible
- **Sparse Graph**: Contains only a few edges

### 18. Bipartite Graph
- **Definition**: Vertex set can be split into two disjoint sets (V1, V2)
- **Characteristics**: Edges only connect vertices between different sets

### 19. Complete Bipartite Graph
- **Definition**: Every vertex in first set connects to every vertex in second set
- **Notation**: Kx,y (x vertices in first set, y in second set)

---

## Graph Terminology

### Basic Elements
- **Vertex (Node)**: Individual data element
- **Edge**: Path/connection between two vertices (endpoints)

### Vertex Relationships
- **Adjacent Vertices**: Two vertices connected by an edge
- **Incident Edge**: Edge connected to a vertex as an endpoint

### Edge Direction (for Directed Graphs)
- **Outgoing Edge**: Edge directed away from a vertex
- **Incoming Edge**: Edge directed toward a vertex

### Degree Concepts
- **Degree**: Number of edges incident to a vertex
- **In-degree**: Number of incoming edges to a vertex
- **Out-degree**: Number of outgoing edges from a vertex

### Special Elements
- **Parallel Edges**: Multiple edges between same pair of vertices
- **Self-loop**: Edge connecting a vertex to itself
- **Forest**: Graph with no cycles
- **Path**: Sequence of successive edges between nodes
- **Path Length**: Number of edges in a path
- **Simple Path**: Path with distinct vertices

### Special Vertices
- **Source Vertex**: In-degree = 0
- **Sink Vertex**: Out-degree = 0

### Connectivity
- **Strongly Connected**: Directed path exists between every pair of vertices (both directions)
- **Weakly Connected**: Becomes connected when all directed edges are replaced with undirected edges
- **Bridge**: Edge whose removal disconnects the graph

---

## Graph Representation

### 1. Adjacency Matrix

**Description**: 2D matrix representation where matrix[i][j] indicates connection between vertex i and vertex j.

**Matrix Size**: V × V (where V = number of vertices)

**Values**:
- **Unweighted**: 1 (edge exists) or 0 (no edge)
- **Weighted**: Actual weight value or 0 (no edge)

**For Directed Graphs**: matrix[i][j] = 1 means edge from vertex i to vertex j

**For Undirected Graphs**: matrix[i][j] = matrix[j][i] = 1 (symmetric matrix)

#### Advantages:
- Easy to understand and implement
- O(1) time complexity for:
  - Checking edge existence
  - Adding an edge
  - Removing an edge

#### Disadvantages:
- O(V²) space complexity (inefficient for sparse graphs)
- O(V²) time complexity for adding/removing vertices

### 2. Adjacency List

**Description**: Array of linked lists where each vertex maintains a list of its neighbors.

**Structure**: 
- Array indexed by vertex number
- Each array element points to a linked list of adjacent vertices

**Implementation Options**:
- Linked list representation
- Array-based representation

#### Advantages:
- Space efficient: O(V + E) where E = number of edges
- O(1) time for adding/deleting edges and vertices
- Clear visualization of adjacent nodes

#### Disadvantages:
- Slower for checking if two vertices are adjacent
- O(V) time complexity for adjacency testing

### 3. Incidence Matrix

**Description**: Matrix representation where rows represent vertices and columns represent edges.

**Matrix Size**: V × E (where V = vertices, E = edges)

**Values**:
- **0**: Vertex not connected to edge
- **1**: Vertex connected as outgoing endpoint
- **-1**: Vertex connected as incoming endpoint

**Use Case**: Particularly useful for directed graphs where edge direction matters

#### Advantages:
- Clear representation of vertex-edge relationships
- Useful for certain graph algorithms

#### Disadvantages:
- Space inefficient: O(V × E)
- More complex than other representations

### 4. Incidence List

**Description**: Each vertex maintains a list of edges incident to it.

**Structure**: Similar to adjacency list but stores edge information instead of just neighboring vertices.

## Introduction

A graph is a collection of nodes (vertices) connected by edges. Graphs can be:
- **Directed**: Edges have a direction (one-way)
- **Undirected**: Edges are bidirectional (two-way)
- **Weighted**: Edges have associated weights/costs
- **Unweighted**: All edges have equal weight

## Graph Representations

### Adjacency List

An adjacency list represents a graph as a dictionary where each key is a node and the value is a collection of its neighbors.

#### 1. Undirected Graph (Unweighted)

```python
# Input format: n (nodes), e (edges), followed by e lines of edge pairs
n, e = map(int, input().split())
edges = [input().split() for i in range(e)]
edges_conv = [(int(i), int(j)) for i, j in edges]

# Initialize graph with empty sets for each node
graph = {i: set() for i in range(1, n+1)}

# Add edges in both directions (undirected)
for i, j in edges_conv:
    graph[i].add(j)
    graph[j].add(i)

print(graph)
```

**Example Input:**
```
4 4
1 2
2 3
3 4
4 1
```

**Output:**
```
{1: {2, 4}, 2: {1, 3}, 3: {2, 4}, 4: {1, 3}}
```

#### 2. Directed Graph (Unweighted)

```python
n, e = map(int, input().split())
edges = [input().split() for i in range(e)]
edges_conv = [(int(i), int(j)) for i, j in edges]

# Initialize graph with empty sets
graph = {i: set() for i in range(1, n+1)}

# Add edges in one direction only (directed)
for i, j in edges_conv:
    graph[i].add(j)

print(graph)
```

#### 3. Undirected Weighted Graph

```python
n, e = map(int, input().split())
edges = [input().split() for i in range(e)]
edges_conv = [(int(i), int(j), int(w)) for i, j, w in edges]

# Initialize graph with empty dictionaries for weights
graph = {i: dict() for i in range(1, n+1)}

# Add weighted edges in both directions
for i, j, w in edges_conv:
    graph[i][j] = w
    graph[j][i] = w

print(graph)
```

**Example Input:**
```
3 3
1 2 5
2 3 3
3 1 2
```

**Output:**
```
{1: {2: 5, 3: 2}, 2: {1: 5, 3: 3}, 3: {2: 3, 1: 2}}
```

#### 4. Directed Weighted Graph

```python
n, e = map(int, input().split())
edges = [input().split() for i in range(e)]
edges_conv = [(int(i), int(j), int(w)) for i, j, w in edges]

# Initialize graph with empty dictionaries
graph = {i: dict() for i in range(1, n+1)}

# Add weighted edges in one direction only
for i, j, w in edges_conv:
    graph[i][j] = w

print(graph)
```

### Adjacency Matrix

An adjacency matrix represents a graph as a 2D array where matrix[i][j] indicates the presence (and weight) of an edge between nodes i and j.

#### 1. Undirected Graph (Unweighted)

```python
n, e = map(int, input().split())
edges = [input().split() for i in range(e)]
# Convert to 0-indexed for matrix
edges_conv = [(int(i)-1, int(j)-1) for i, j in edges]

# Initialize n×n matrix with zeros
graph = [[0]*n for i in range(n)]

# Set matrix[i][j] = 1 for both directions
for i, j in edges_conv:
    graph[i][j] = 1
    graph[j][i] = 1

# Print matrix row by row
print(*graph, sep="\n")
```

#### 2. Directed Graph (Unweighted)

```python
n, e = map(int, input().split())
edges = [input().split() for i in range(e)]
edges_conv = [(int(i)-1, int(j)-1) for i, j in edges]

graph = [[0]*n for i in range(n)]

# Set matrix[i][j] = 1 for one direction only
for i, j in edges_conv:
    graph[i][j] = 1

print(*graph, sep="\n")
```

#### 3. Weighted Undirected Graph

```python
n, e = map(int, input().split())
edges = [input().split() for i in range(e)]
edges_conv = [(int(i)-1, int(j)-1, int(w)) for i, j, w in edges]

graph = [[0]*n for i in range(n)]

# Set matrix[i][j] = weight for both directions
for i, j, w in edges_conv:
    graph[i][j] = w
    graph[j][i] = w

print(*graph, sep="\n")
```

#### 4. Weighted Directed Graph

```python
n, e = map(int, input().split())
edges = [input().split() for i in range(e)]
edges_conv = [(int(i)-1, int(j)-1, int(w)) for i, j, w in edges]

graph = [[0]*n for i in range(n)]

# Set matrix[i][j] = weight for one direction only
for i, j, w in edges_conv:
    graph[i][j] = w

print(*graph, sep="\n")
```

## Graph Traversal Algorithms

### Breadth-First Search (BFS)

BFS explores nodes level by level, visiting all neighbors before moving to the next level. It uses a **queue** (FIFO - First In, First Out).

```python
def bfs(graph, start_node, n):
    queue = []
    visited = {i: False for i in range(1, n+1)}
    
    # Start with the initial node
    queue.append(start_node)
    visited[start_node] = True
    
    while queue:
        # Remove from front of queue
        current = queue.pop(0)
        print(current, end="->")
        
        # Visit all unvisited neighbors
        for neighbor in graph[current]:
            if not visited[neighbor]:
                visited[neighbor] = True
                queue.append(neighbor)
```

**Key Points:**
- Uses a queue for node processing
- Guarantees shortest path in unweighted graphs
- Time complexity: O(V + E)
- Space complexity: O(V)

### Depth-First Search (DFS)

DFS explores as far as possible along each branch before backtracking. It uses a **stack** (LIFO - Last In, First Out).

#### Iterative Implementation

```python
def dfs_iterative(graph, start_node, n):
    stack = []
    visited = {i: False for i in range(1, n+1)}
    
    visited[start_node] = True
    stack.append(start_node)
    
    while stack:
        # Remove from top of stack
        current = stack.pop()
        print(current, end="->")
        
        # Visit all unvisited neighbors
        for neighbor in graph[current]:
            if not visited[neighbor]:
                visited[neighbor] = True
                stack.append(neighbor)
```

#### Recursive Implementation

```python
def dfs_recursive(graph, node, visited):
    visited[node] = True
    print(node, end="->")
    
    # Recursively visit all unvisited neighbors
    for neighbor in graph[node]:
        if not visited[neighbor]:
            dfs_recursive(graph, neighbor, visited)

# Usage for all nodes
visited = {i: False for i in range(1, n+1)}
for i in range(1, n+1):
    if not visited[i]:
        dfs_recursive(graph, i, visited)
```

**Key Points:**
- Uses a stack (or recursion stack) for node processing
- Good for detecting cycles and topological sorting
- Time complexity: O(V + E)
- Space complexity: O(V)

## Complete Examples

### Example 1: Creating and Traversing an Undirected Graph

```python
# Create undirected graph
n, e = 4, 4
edges = [("1", "2"), ("2", "3"), ("3", "4"), ("4", "1")]
edges_conv = [(int(i), int(j)) for i, j in edges]

graph = {i: set() for i in range(1, n+1)}
for i, j in edges_conv:
    graph[i].add(j)
    graph[j].add(i)

print("Graph:", graph)

# BFS traversal
print("\nBFS from node 1:")
bfs(graph, 1, n)

# DFS traversal
print("\nDFS from node 1:")
dfs_recursive(graph, 1, {i: False for i in range(1, n+1)})
```

### Example 2: Working with Weighted Graphs

```python
# Create weighted directed graph
n, e = 3, 3
edges = [("1", "2", "5"), ("2", "3", "3"), ("1", "3", "2")]
edges_conv = [(int(i), int(j), int(w)) for i, j, w in edges]

graph = {i: dict() for i in range(1, n+1)}
for i, j, w in edges_conv:
    graph[i][j] = w

print("Weighted Graph:", graph)

# Find shortest path using BFS (modified for weights)
def shortest_path(graph, start, end):
    # Implementation depends on algorithm (Dijkstra's, etc.)
    pass
```

## Time and Space Complexity

### Adjacency List vs Adjacency Matrix

| Operation | Adjacency List | Adjacency Matrix |
|-----------|----------------|------------------|
| Storage | O(V + E) | O(V²) |
| Add Edge | O(1) | O(1) |
| Remove Edge | O(V) | O(1) |
| Check Edge | O(V) | O(1) |
| BFS/DFS | O(V + E) | O(V²) |
