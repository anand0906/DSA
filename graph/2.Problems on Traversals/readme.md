# Connected Components

## Problem Statement

Given an undirected graph consisting of V vertices numbered from 0 to V-1 and E edges, find the number of connected components in the graph.

The ith edge is represented by [ai, bi], denoting an edge between vertex ai and bi. Two vertices u and v belong to the same component if there is a path from u to v or v to u.

**Definition**: A connected component is a subgraph of a graph in which there exists a path between any two vertices, and no vertex of the subgraph shares an edge with a vertex outside of the subgraph.

## Examples

### Example 1
<img src="https://static.takeuforward.org/content/ProblemSetter-g_oC8sRD" />

```
Input: V = 4, edges = [[0,1], [1,2]]
Output: 2
```
**Explanation**: Vertices {0, 1, 2} form the first component and vertex 3 forms the second component.

### Example 2
```
Input: V = 7, edges = [[0, 1], [1, 2], [2, 3], [4, 5]]
Output: 3
```
**Explanation**: 
- The edges [0, 1], [1, 2], [2, 3] form a connected component with vertices {0, 1, 2, 3}
- The edge [4, 5] forms another connected component with vertices {4, 5}
- Vertex 6 is isolated and forms its own component
- Therefore, the graph has 3 connected components: {0, 1, 2, 3}, {4, 5}, and {6}

## Approach

1. **Convert to Adjacency List**: Transform the given edges into an adjacency list representation for easy traversal
2. **Initialize Data Structures**: 
   - Create a visited array to mark nodes as visited
   - Initialize a counter to count the number of components
3. **Traverse Components**: 
   - For each unvisited node, increment the counter (new component found)
   - Perform BFS/DFS to visit all nodes in the current component
4. **Return Result**: The counter gives us the total number of connected components

## Implementation

```python
def solution(n, edges):
    # Create adjacency list
    nodes = [i for i in range(n)]
    graph = {i: set() for i in nodes}
    
    # Build the graph
    for i, j in edges:
        graph[i].add(j)
        graph[j].add(i)
    
    # Initialize visited array
    visited = {i: False for i in nodes}
    
    def bfs(graph, node):
        """Perform BFS to visit all nodes in current component"""
        visited[node] = True
        queue = [node]
        
        while queue:
            temp = queue.pop(0)
            for adj in graph[temp]:
                if not visited[adj]:
                    queue.append(adj)
                    visited[adj] = True
    
    # Count connected components
    count = 0
    for node in nodes:
        if not visited[node]:
            count += 1
            bfs(graph, node)
    
    return count

# Test the solution
v = 7
edges = [[0, 1], [1, 2], [2, 3], [4, 5]]
print(solution(v, edges))  # Output: 3
```

## Algorithm Details

- **Time Complexity**: O(V + E) where V is the number of vertices and E is the number of edges
- **Space Complexity**: O(V + E) for the adjacency list and visited array

# Number of Provinces

## Problem Statement

Given an undirected graph with V vertices represented as an n x n adjacency matrix, find the number of provinces in the graph.

Two vertices u and v belong to a single province if there is a path from u to v or v to u. The graph is given as an n x n matrix `adj` where `adj[i][j] = 1` if the ith city and the jth city are directly connected, and `adj[i][j] = 0` otherwise.

**Definition**: A province is a group of directly or indirectly connected cities and no other cities outside of the group.

## Examples

### Example 1
```
Input: adj = [
    [1, 0, 0, 1],
    [0, 1, 1, 0],
    [0, 1, 1, 0],
    [1, 0, 0, 1]
]
Output: 2
```
<img src="https://static.takeuforward.org/content/ProblemSetter-WdAmb0xn" />
**Explanation**: 
- Province 1: Cities [0, 3] - City 0 and city 3 have a path between them
- Province 2: Cities [1, 2] - City 1 and city 2 have a path between them
- There is no path between any city in province 1 and any city in province 2

### Example 2
```
Input: adj = [
    [1, 0, 1],
    [0, 1, 0],
    [1, 0, 1]
]
Output: 2
```
**Explanation**: 
- Province 1: Cities [0, 2] - City 0 and city 2 have a path between them
- Province 2: City [1] - City 1 has no path to city 0 or city 2, so it forms its own province

## Approach

1. **Convert Adjacency Matrix to List**: Transform the given adjacency matrix into an adjacency list representation for easier traversal
2. **Initialize Data Structures**: 
   - Create a visited array to mark nodes as visited
   - Initialize a counter to count the number of provinces
3. **Traverse Provinces**: 
   - For each unvisited node, increment the counter (new province found)
   - Perform BFS to visit all nodes in the current province
4. **Return Result**: The counter gives us the total number of provinces

## Implementation

```python
def solution(adj_matrix):
    n = len(adj_matrix)
    nodes = [i for i in range(n)]
    
    # Convert adjacency matrix to adjacency list
    graph = {i: set() for i in nodes}
    for i in range(n):
        for j in range(n):
            if adj_matrix[i][j] == 1:
                graph[i].add(j)
    
    # Initialize visited array
    visited = {i: False for i in nodes}
    
    def bfs(graph, node):
        """Perform BFS to visit all nodes in current province"""
        visited[node] = True
        queue = [node]
        
        while queue:
            temp = queue.pop(0)
            for adj in graph[temp]:
                if not visited[adj]:
                    queue.append(adj)
                    visited[adj] = True
    
    # Count provinces
    count = 0
    for node in nodes:
        if not visited[node]:
            count += 1
            bfs(graph, node)
    
    return count

# Test the solution
adj_matrix = [
    [1, 0, 0, 1],
    [0, 1, 1, 0],
    [0, 1, 1, 0],
    [1, 0, 0, 1]
]
print(solution(adj_matrix))  # Output: 2
```

## Optimized Implementation (Direct Matrix Traversal)

```python
def solution_optimized(adj_matrix):
    n = len(adj_matrix)
    visited = [False] * n
    
    def dfs(node):
        """Perform DFS directly on adjacency matrix"""
        visited[node] = True
        for neighbor in range(n):
            if adj_matrix[node][neighbor] == 1 and not visited[neighbor]:
                dfs(neighbor)
    
    count = 0
    for i in range(n):
        if not visited[i]:
            count += 1
            dfs(i)
    
    return count
```

## Algorithm Details

- **Time Complexity**: O(V²) where V is the number of vertices (due to adjacency matrix traversal)
- **Space Complexity**: O(V²) for the adjacency list and visited array


