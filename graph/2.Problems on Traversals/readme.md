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


# Number of Islands

## Problem Statement

Given a grid of size N x M (N is the number of rows and M is the number of columns in the grid) consisting of '0's (Water) and '1's (Land), find the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally, vertically, or diagonally (i.e., in all 8 directions).

## Examples

### Example 1
<img src="https://static.takeuforward.org/content/ProblemSetter-cQJHsEaI" />

```
Input: grid = [
    ["1", "1", "1", "0", "1"],
    ["1", "0", "0", "0", "0"],
    ["1", "1", "1", "0", "1"],
    ["0", "0", "0", "1", "1"]
]
Output: 2
```
**Explanation**: This grid contains 2 islands. Each '1' represents a piece of land, and the islands are formed by connecting adjacent lands in all 8 directions. Despite some islands having a common edge, they are considered separate islands because there is no land connectivity between them.

### Example 2
```
Input: grid = [
    ["1", "0", "0", "0", "1"],
    ["0", "1", "0", "1", "0"],
    ["0", "0", "1", "0", "0"],
    ["0", "1", "0", "1", "0"]
]
Output: 1
```
**Explanation**: In the given grid, there's only one island as all the '1's are connected either horizontally, vertically, or diagonally, forming a single contiguous landmass surrounded by water on all sides.

## Intuition

### Graph Representation
- Think of all cells in the grid as nodes/vertices
- Each cell is connected to its 8 neighbors through edges
- Land cells ('1') that are connected form a connected component (island)

### 8-Directional Traversal
The 8 neighbors of any cell (row, col) can be accessed using:
- **Row variations**: row-1, row, row+1 (deltaRow: -1, 0, 1)
- **Column variations**: col-1, col, col+1 (deltaCol: -1, 0, 1)

<img src="https://static.takeuforward.org/premium/Graphs/Traversal%20Problems/Number%20of%20islands/nieghbors-PCKbhKGS" />

## Approach

1. **Initialize**: Determine grid dimensions and create a 2D visited array
2. **Traverse Grid**: Loop through each cell in the grid
3. **Find New Islands**: If cell is land ('1') and not visited, start BFS/DFS
4. **Explore Island**: Use BFS to visit all connected land cells in 8 directions
5. **Count Islands**: Increment counter for each new island found

## Implementation

```python
def solution(grid):
    n, m = len(grid), len(grid[0])
    visited = [[False] * m for _ in range(n)]
    
    def bfs(row, col):
        """Perform BFS to explore all connected land cells"""
        visited[row][col] = True
        queue = [(row, col)]
        
        while queue:
            r, c = queue.pop(0)
            
            # Check all 8 directions
            for i in range(-1, 2):
                for j in range(-1, 2):
                    adj_row = r + i
                    adj_col = c + j
                    
                    # Check bounds
                    if (adj_row >= 0 and adj_col >= 0 and 
                        adj_row < n and adj_col < m):
                        
                        # If it's land and not visited
                        if (grid[adj_row][adj_col] == '1' and 
                            not visited[adj_row][adj_col]):
                            visited[adj_row][adj_col] = True
                            queue.append((adj_row, adj_col))
    
    count = 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '1' and not visited[i][j]:
                count += 1
                bfs(i, j)
    
    return count

# Test the solution
grid = [
    ["1", "0", "0", "0", "1"],
    ["0", "1", "0", "1", "0"],
    ["0", "0", "1", "0", "0"],
    ["0", "1", "0", "1", "0"]
]
print(solution(grid))  # Output: 1
```

## Algorithm Details

- **Time Complexity**: O(N × M) where N is rows and M is columns
- **Space Complexity**: O(N × M) for the visited array and queue
