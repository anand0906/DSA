# Graph Data Structures and Algorithms Guide

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
