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

---

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

---

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

---

# Number of Enclaves

## Problem Statement

Given an N x M binary matrix grid, where 0 represents a sea cell and 1 represents a land cell. A move consists of walking from one land cell to another adjacent (4-directionally) land cell or walking off the boundary of the grid.

Find the number of land cells in the grid for which we cannot walk off the boundary of the grid in any number of moves.

## Examples

### Example 1
```
Input: grid = [
    [0, 0, 0, 0],
    [1, 0, 1, 0],
    [0, 1, 1, 0],
    [0, 0, 0, 0]
]
Output: 3
```

<img src="https://static.takeuforward.org/content/ProblemSetter-BX7diqs5" />
<img src="https://static.takeuforward.org/content/ProblemSetter-4DMweAfF" />
**Explanation**: The land cells at positions (1,2), (2,1), and (2,2) are completely surrounded and cannot reach the boundary. Therefore, there are 3 enclaves.

### Example 2
```
Input: grid = [
    [0, 0, 0, 1],
    [0, 0, 0, 1],
    [0, 1, 1, 0],
    [0, 0, 1, 0],
    [0, 0, 0, 0]
]
Output: 3
```

<img src="https://static.takeuforward.org/content/ProblemSetter-JKtYrVqX" />
<img src="https://static.takeuforward.org/content/ProblemSetter-zgfKADbg" />
**Explanation**: The land cells that cannot reach the boundary form the enclaves. The highlighted cells represent these enclosed land cells.

## Intuition

The problem requires finding land cells that are completely surrounded by other land cells and cannot connect to the boundary of the grid by moving in 4 directions (up, down, left, right).

**Key Insight**: Any land cell directly connected to the boundary or indirectly connected via other land cells should not be counted as an enclave. Therefore, we can:
1. Identify and mark all land cells reachable from the boundary
2. Count the remaining unmarked land cells as enclaves

## Approach

1. **Setup**: Create arrays for 4-directional movement and boundary checking
2. **BFS Implementation**: Create a function to traverse connected land cells
3. **Boundary Marking**: Start BFS from all boundary land cells to mark reachable cells
4. **Count Enclaves**: Count all unmarked land cells as they represent enclaves

## Implementation

```python
def solution(grid):
    n, m = len(grid), len(grid[0])
    visited = [[False] * m for _ in range(n)]
    
    def bfs(row, col):
        """Perform BFS to mark all land cells connected to boundary"""
        visited[row][col] = True
        queue = [(row, col)]
        
        while queue:
            r, c = queue.pop(0)
            
            # Check 4 directions: right, left, up, down
            for i, j in zip([0, 0, -1, 1], [1, -1, 0, 0]):
                adj_row = r + i
                adj_col = c + j
                
                # Check bounds
                if (adj_row >= 0 and adj_col >= 0 and 
                    adj_row < n and adj_col < m):
                    
                    # If it's land and not visited
                    if (grid[adj_row][adj_col] == 1 and 
                        not visited[adj_row][adj_col]):
                        visited[adj_row][adj_col] = True
                        queue.append((adj_row, adj_col))
    
    # Mark all boundary-connected land cells
    for i in range(n):
        for j in range(m):
            # Check if cell is on boundary
            if i == 0 or i == n-1 or j == 0 or j == m-1:
                if grid[i][j] == 1 and not visited[i][j]:
                    bfs(i, j)
    
    # Count unvisited land cells (enclaves)
    count = 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1 and not visited[i][j]:
                count += 1
    
    return count

# Test the solution
grid = [
    [0, 0, 0, 0],
    [1, 0, 1, 0],
    [0, 1, 1, 0],
    [0, 0, 0, 0]
]
print(solution(grid))  # Output: 3
```

## Optimized Implementation (In-place Modification)

```python
def solution_optimized(grid):
    """
    Modify the grid in-place to avoid extra space for visited array
    """
    n, m = len(grid), len(grid[0])
    
    def dfs(row, col):
        if (row < 0 or col < 0 or row >= n or col >= m or 
            grid[row][col] != 1):
            return
        
        # Mark as visited by changing to 2
        grid[row][col] = 2
        
        # Explore 4 directions
        directions = [(0, 1), (0, -1), (-1, 0), (1, 0)]
        for dr, dc in directions:
            dfs(row + dr, col + dc)
    
    # Mark all boundary-connected land cells
    for i in range(n):
        for j in range(m):
            if i == 0 or i == n-1 or j == 0 or j == m-1:
                if grid[i][j] == 1:
                    dfs(i, j)
    
    # Count remaining land cells (enclaves)
    count = 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1:
                count += 1
    
    # Optional: Restore the grid
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 2:
                grid[i][j] = 1
    
    return count
```

## Step-by-Step Visualization

For the first example:
```
Original Grid:          After Boundary BFS:      Enclaves Count:
[0, 0, 0, 0]           [0, 0, 0, 0]            [0, 0, 0, 0]
[1, 0, 1, 0]    →      [V, 0, 1, 0]     →      [-, 0, E, 0]
[0, 1, 1, 0]           [0, 1, 1, 0]            [0, E, E, 0]
[0, 0, 0, 0]           [0, 0, 0, 0]            [0, 0, 0, 0]

V = Visited (boundary-connected)
E = Enclave (3 total)
```

## Algorithm Details

- **Time Complexity**: O(N × M) where N is rows and M is columns
- **Space Complexity**: O(N × M) for the visited array and queue
- **Method**: Breadth-First Search (BFS) from boundary cells

---

# Surrounded Regions

## Problem Statement

Given a matrix mat of size N x M where every element is either 'O' or 'X'. Replace all 'O' with 'X' that is surrounded by 'X'.

An 'O' (or a set of 'O') is considered to be surrounded by 'X' if there are 'X' at locations just below, above, left, and right of it.

## Examples

### Example 1
```
Input: mat = [
    ["X", "X", "X", "X"],
    ["X", "O", "O", "X"],
    ["X", "X", "O", "X"],
    ["X", "O", "X", "X"]
]

Output: [
    ["X", "X", "X", "X"],
    ["X", "X", "X", "X"],
    ["X", "X", "X", "X"],
    ["X", "O", "X", "X"]
]
```
**Explanation**: 
<img src="https://static.takeuforward.org/content/ProblemSetter-sA3BL8cs" />
<img src="https://static.takeuforward.org/content/ProblemSetter-jd2Z8EHv" />
- The 'O' cells at positions (1,1), (1,2), and (2,2) are completely surrounded by 'X' cells, so they are replaced with 'X'
- The 'O' at position (3,1) is adjacent to the boundary, so it cannot be completely surrounded and remains unchanged

### Example 2
```
Input: mat = [
    ["X", "X", "X"],
    ["X", "O", "X"],
    ["X", "X", "X"]
]

Output: [
    ["X", "X", "X"],
    ["X", "X", "X"],
    ["X", "X", "X"]
]
```
**Explanation**: The only 'O' cell at position (1,1) is completely surrounded by 'X' cells in all directions, so it is replaced with 'X'.

## Intuition

**Key Insight**: Boundary elements cannot be replaced with 'X' as they are not surrounded by 'X' from all 4 directions. If an 'O' (or a set of 'O') is connected to a boundary 'O', then it cannot be replaced with 'X'.

**Strategy**: Instead of finding surrounded regions directly, we can:
1. Find all 'O' cells that are connected to the boundary
2. Mark these cells as "safe" (cannot be replaced)
3. Replace all remaining 'O' cells with 'X'

## Approach

1. **Initialize**: Create a visited array and direction vectors for 4-directional movement
2. **Boundary Traversal**: Start BFS/DFS from all boundary 'O' cells
3. **Mark Connected**: Mark all 'O' cells connected to boundary as visited (safe)
4. **Replace Surrounded**: Convert all unvisited 'O' cells to 'X'

## Implementation

```python
def solution(grid):
    n, m = len(grid), len(grid[0])
    visited = [[False] * m for _ in range(n)]
    
    def bfs(row, col):
        """BFS to mark all 'O' cells connected to boundary"""
        visited[row][col] = True
        queue = [(row, col)]
        
        while queue:
            r, c = queue.pop(0)
            
            # Check 4 directions: right, left, up, down
            for i, j in zip([0, 0, -1, 1], [1, -1, 0, 0]):
                adj_row = r + i
                adj_col = c + j
                
                # Check bounds
                if (adj_row >= 0 and adj_col >= 0 and 
                    adj_row < n and adj_col < m):
                    
                    # If it's 'O' and not visited
                    if (grid[adj_row][adj_col] == 'O' and 
                        not visited[adj_row][adj_col]):
                        visited[adj_row][adj_col] = True
                        queue.append((adj_row, adj_col))
    
    # Mark all boundary-connected 'O' cells
    for i in range(n):
        for j in range(m):
            # Check if cell is on boundary
            if i == 0 or i == n-1 or j == 0 or j == m-1:
                if grid[i][j] == 'O' and not visited[i][j]:
                    bfs(i, j)
    
    # Replace all unvisited 'O' cells with 'X'
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'O' and not visited[i][j]:
                grid[i][j] = 'X'
    
    return grid

# Test the solution
mat = [
    ["X", "X", "X", "X"],
    ["X", "O", "O", "X"],
    ["X", "X", "O", "X"],
    ["X", "O", "X", "X"]
]
print(solution(mat))
```

## Space-Optimized Implementation

```python
def solution_optimized(grid):
    """
    Modify the grid in-place to avoid extra space for visited array
    """
    n, m = len(grid), len(grid[0])
    
    def dfs(row, col):
        if (row < 0 or col < 0 or row >= n or col >= m or 
            grid[row][col] != 'O'):
            return
        
        # Mark as safe by changing to a temporary character
        grid[row][col] = '#'
        
        # Explore 4 directions
        directions = [(0, 1), (0, -1), (-1, 0), (1, 0)]
        for dr, dc in directions:
            dfs(row + dr, col + dc)
    
    # Mark all boundary-connected 'O' cells as safe
    for i in range(n):
        for j in range(m):
            if i == 0 or i == n-1 or j == 0 or j == m-1:
                if grid[i][j] == 'O':
                    dfs(i, j)
    
    # Process the grid: replace 'O' with 'X' and '#' back to 'O'
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'O':
                grid[i][j] = 'X'  # Surrounded region
            elif grid[i][j] == '#':
                grid[i][j] = 'O'  # Safe region
    
    return grid
```

## Algorithm Details

- **Time Complexity**: O(N × M) where N is rows and M is columns
- **Space Complexity**: O(N × M) for the visited array and recursion stack
  
---


# Flood Fill Algorithm - Complete Guide

## Problem Statement

An image is represented by a 2-D array of integers, where each integer represents the pixel value of the image. Given a coordinate `(sr, sc)` representing the starting pixel (row and column) of the flood fill, and a pixel value `newColor`, perform a "flood fill" operation on the image.

### What is Flood Fill?

To perform a flood fill:
1. Consider the starting pixel
2. Find all pixels connected **4-directionally** to the starting pixel that have the **same color** as the starting pixel
3. Continue finding pixels connected 4-directionally to those pixels (also with the same color)
4. Replace the color of all these connected pixels with the `newColor`

## Examples

### Example 1
<img src="https://static.takeuforward.org/content/ProblemSetter-1vytgU1a" />

**Input:**
```
image = [[1, 1, 1], 
         [1, 1, 0], 
         [1, 0, 1]]
sr = 1, sc = 1, newColor = 2
```

**Output:**
```
[[2, 2, 2], 
 [2, 2, 0], 
 [2, 0, 1]]
```

**Explanation:** Starting from position (1, 1) with value 1, all connected pixels with value 1 are changed to 2. The bottom-right corner (2, 2) is not changed because it's not 4-directionally connected to the starting pixel.

### Example 2
**Input:**
```
image = [[0, 1, 0], 
         [1, 1, 0], 
         [0, 0, 1]]
sr = 2, sc = 2, newColor = 3
```

**Output:**
```
[[0, 1, 0], 
 [1, 1, 0], 
 [0, 0, 3]]
```

**Explanation:** Starting from position (2, 2) with value 1, only this single pixel needs to be changed since no adjacent pixels have the same color.

## Intuition

### Graph Perspective
- Think of all pixels in the image as **nodes/vertices**
- Each pixel is connected to its 4 neighbors (up, right, down, left) via **edges**
- Use any traversal algorithm (DFS/BFS) to find all neighbors with the same pixel value
- During traversal, change each pixel's value to the new color

### Efficient Neighbor Traversal
The 4 neighbors of any cell can be accessed using direction vectors:
```
delRow = [-1, 0, 1, 0]  # up, right, down, left
delCol = [0, 1, 0, -1]  # up, right, down, left
```

## Approach

1. **Initialize:** Create a new image to store the flood-filled result (optional - can modify original)
2. **Direction Vectors:** Define vectors for moving up, right, down, and left
3. **Boundary Check:** Create helper function to ensure pixels are within image bounds
4. **Traversal:** Starting from the given pixel, perform DFS/BFS traversal
5. **Color Change:** During traversal, mark all pixels with the same initial color using the new color
6. **Return:** Once traversal terminates, return the flood-filled image

## Implementation

### BFS Approach

```python
def flood_fill(grid, srcRow, srcCol, newColor):
    n, m = len(grid), len(grid[0])
    queue = [(srcRow, srcCol)]
    initColor = grid[srcRow][srcCol]
    
    # If new color is same as initial color, no change needed
    if newColor == initColor:
        return grid
    
    # Mark starting pixel with new color
    grid[srcRow][srcCol] = newColor
    
    # BFS traversal
    while queue:
        r, c = queue.pop(0)
        
        # Check all 4 directions
        for i, j in zip([0, 0, -1, 1], [1, -1, 0, 0]):
            adj_row = r + i
            adj_col = c + j
            
            # Check bounds and color match
            if (adj_row >= 0 and adj_col >= 0 and 
                adj_row < n and adj_col < m):
                if grid[adj_row][adj_col] == initColor:
                    grid[adj_row][adj_col] = newColor
                    queue.append((adj_row, adj_col))
    
    return grid

# Test the algorithm
grid = [[1, 1, 1], 
        [1, 1, 0], 
        [1, 0, 1]]
sr, sc = 1, 1
newColor = 2
result = flood_fill(grid, sr, sc, newColor)
print(result)
```

### DFS Approach

```python
def flood_fill_dfs(grid, srcRow, srcCol, newColor):
    n, m = len(grid), len(grid[0])
    initColor = grid[srcRow][srcCol]
    
    if newColor == initColor:
        return grid
    
    def dfs(row, col):
        # Check bounds and color match
        if (row < 0 or row >= n or col < 0 or col >= m or 
            grid[row][col] != initColor):
            return
        
        # Change color
        grid[row][col] = newColor
        
        # Recursively fill 4 directions
        dfs(row - 1, col)  # up
        dfs(row + 1, col)  # down
        dfs(row, col - 1)  # left
        dfs(row, col + 1)  # right
    
    dfs(srcRow, srcCol)
    return grid
```

## Algorithm Details

- **Time Complexity:** O(n × m) where n and m are the dimensions of the image
- **Space Complexity:** 
  - BFS: O(n × m) for the queue in worst case
  - DFS: O(n × m) for the recursion stack in worst case


---

# Rotten Oranges

## Problem Statement

Given an n x m grid, where each cell has the following values:
- **2** - represents a rotten orange
- **1** - represents a fresh orange  
- **0** - represents an empty cell

Every minute, if a fresh orange is adjacent to a rotten orange in 4-direction (upward, downwards, right, and left) it becomes rotten.

Return the minimum number of minutes required such that none of the cells has a fresh orange. If it's not possible, return -1.

## Examples

### Example 1
**Input:**
```
grid = [[2, 1, 1], 
        [0, 1, 1], 
        [1, 0, 1]]
```

**Output:**
```
-1
```

<img src="https://static.takeuforward.org/content/ProblemSetter-NWsocTcc"/>
**Explanation:** Orange at (2,0) cannot be rotten because it's not connected to any rotten orange through adjacent cells.

### Example 2
**Input:**
```
grid = [[2, 1, 1], 
        [1, 1, 0], 
        [0, 1, 1]]
```

**Output:**
```
4
```

<img src="https://static.takeuforward.org/content/ProblemSetter-yuB56suz"/>
**Explanation:** 
- Minute 0: Initial state with one rotten orange at (0,0)
- Minute 1: Orange at (0,1) and (1,0) become rotten
- Minute 2: Orange at (0,2) and (1,1) become rotten  
- Minute 3: Orange at (2,1) becomes rotten
- Minute 4: Orange at (2,2) becomes rotten

## Intuition

The key insight is that for each rotten orange, fresh oranges in its 4 directions will become rotten in the next minute. This creates a **multi-source BFS** problem where:

- Each minute represents a **level** in BFS traversal
- All oranges at the same level rot simultaneously
- We need to track the maximum time taken for any orange to rot

**Multi-Source BFS Strategy:**
1. Start with all initially rotten oranges in the queue
2. Process level by level (minute by minute)
3. For each rotten orange, spread rot to adjacent fresh oranges
4. Continue until no more oranges can be rotten

## Approach

1. **Initialize:** Create a queue for BFS and count total fresh oranges
2. **Find Initial Rotten:** Traverse grid to find all initially rotten oranges and add them to queue with time = 0
3. **BFS Traversal:** Process queue level by level:
   - For each rotten orange, check its 4 neighbors
   - If neighbor is fresh, make it rotten and add to queue with incremented time
   - Track maximum time encountered
4. **Validation:** Check if all fresh oranges became rotten
5. **Return:** Return maximum time if all rotten, else -1

## Implementation

```python
def solution(grid):
    n, m = len(grid), len(grid[0])
    queue = []
    fresh = 0
    ans = 0
    
    # Find all initially rotten oranges and count fresh ones
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 2:
                queue.append((i, j, 0))  # (row, col, time)
            elif grid[i][j] == 1:
                fresh += 1
    
    # BFS to spread rot
    while queue:
        r, c, time = queue.pop(0)
        
        # Check all 4 directions
        for i, j in zip([0, 0, -1, 1], [1, -1, 0, 0]):
            adj_row = r + i
            adj_col = c + j
            
            # Check bounds
            if (adj_row >= 0 and adj_col >= 0 and 
                adj_row < n and adj_col < m):
                
                # If adjacent cell has fresh orange
                if grid[adj_row][adj_col] == 1:
                    grid[adj_row][adj_col] = 2  # Make it rotten
                    fresh -= 1  # Decrease fresh count
                    queue.append((adj_row, adj_col, time + 1))
                    ans = max(ans, time + 1)  # Update max time
    
    # Check if all oranges are rotten
    if fresh > 0:
        return -1
    return ans

# Test the algorithm
grid = [[2, 1, 1], 
        [1, 1, 0], 
        [0, 1, 1]]
print(solution(grid))  # Output: 4
```

## Algorithm Details

- **Time Complexity:** O(n × m) where n and m are the dimensions of the grid
- **Space Complexity:** O(n × m) for the queue in worst case (when all cells are rotten initially)


# Distance of Nearest Cell Having One

## Problem Statement

Given a binary grid of N x M. Find the distance of the nearest 1 in the grid for each cell.

The distance is calculated as |i1 - i2| + |j1 - j2|, where i1, j1 are the row number and column number of the current cell, and i2, j2 are the row number and column number of the nearest cell having value 1.

## Examples

### Example 1
**Input:**
```
grid = [[0, 1, 1, 0], 
        [1, 1, 0, 0], 
        [0, 0, 1, 1]]
```

**Output:**
```
[[1, 0, 0, 1], 
 [0, 0, 1, 1], 
 [1, 1, 0, 0]]
```

<img src="https://static.takeuforward.org/content/ProblemSetter-RW6Xyxa0" />
<img src="https://static.takeuforward.org/content/ProblemSetter-b0o2o0hT" />
**Explanation:** 
- 0's at (0,0), (0,3), (1,2), (1,3), (2,0) and (2,1) are at a distance of 1 from 1's at (0,1), (0,2), (0,2), (2,3), (1,0) and (1,1) respectively.

### Example 2
**Input:**
```
grid = [[1, 0, 1], 
        [1, 1, 0], 
        [1, 0, 0]]
```

**Output:**
```
[[0, 1, 0], 
 [0, 0, 1], 
 [0, 1, 2]]
```

**Explanation:** 
- 0's at (0,1), (1,2), (2,1) and (2,2) are at a distance of 1, 1, 1 and 2 from 1's at (0,0), (0,2), (2,0) and (1,1) respectively.

## Intuition

To find the nearest 1 in the grid for each cell, the **multi-source BFS** algorithm is perfect. The key insight is:

- Start BFS from all cells containing '1' simultaneously
- BFS naturally explores cells in order of increasing distance
- The first time we reach any cell, it's guaranteed to be via the shortest path

**Why BFS over DFS?**
BFS ensures that when we first visit a cell, we do so via the shortest possible path. This is because BFS explores all cells at distance 1 before moving to distance 2, and so on.

**Multi-Source BFS Strategy:**
1. Add all '1' cells to the queue with distance 0
2. Process level by level (distance by distance)
3. For each cell, explore its 4 neighbors
4. First time we reach a '0' cell determines its minimum distance

## Approach

1. **Initialize:** Create a queue for BFS and mark all '1' cells as visited
2. **Multi-Source Start:** Add all '1' cells to queue with distance 0
3. **BFS Traversal:** Process queue level by level:
   - For each cell, check its 4 neighbors
   - If neighbor is unvisited '0', mark it with current distance + 1
   - Add the neighbor to queue for further exploration
4. **Result:** The grid now contains minimum distances for each cell

## Implementation

```python
def solution(grid):
    n, m = len(grid), len(grid[0])
    queue = []
    
    # Add all '1' cells to queue and mark them as visited
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1:
                queue.append((i, j, 0))  # (row, col, distance)
                grid[i][j] = -1  # Mark as visited (will be restored to 0 later)
    
    # BFS to find minimum distances
    while queue:
        r, c, time = queue.pop(0)
        
        # Check all 4 directions
        for i, j in zip([0, 0, -1, 1], [1, -1, 0, 0]):
            adj_row = r + i
            adj_col = c + j
            
            # Check bounds
            if (adj_row >= 0 and adj_col >= 0 and 
                adj_row < n and adj_col < m):
                
                # If unvisited '0' cell
                if grid[adj_row][adj_col] == 0:
                    grid[adj_row][adj_col] = time + 1
                    queue.append((adj_row, adj_col, time + 1))
    
    # Restore '1' cells (marked as -1) back to distance 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == -1:
                grid[i][j] = 0
    
    return grid

# Test the algorithm
grid = [[0, 1, 1, 0], 
        [1, 1, 0, 0], 
        [0, 0, 1, 1]]
print(solution(grid))
```

## Alternative Implementation (Using Separate Distance Matrix)

```python
def solution_separate_matrix(grid):
    n, m = len(grid), len(grid[0])
    distance = [[float('inf')] * m for _ in range(n)]
    queue = []
    
    # Initialize distance matrix and queue
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1:
                distance[i][j] = 0
                queue.append((i, j))
    
    directions = [(0, 1), (0, -1), (-1, 0), (1, 0)]
    
    # BFS traversal
    while queue:
        r, c = queue.pop(0)
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < n and 0 <= nc < m and 
                distance[nr][nc] > distance[r][c] + 1):
                distance[nr][nc] = distance[r][c] + 1
                queue.append((nr, nc))
    
    return distance
```



## Algorithm Details

- **Time Complexity:** O(n × m) where n and m are the dimensions of the grid
- **Space Complexity:** O(n × m) for the queue in worst case


---

# Cycle Detection in Undirected Graphs

## Problem Statement

Given an undirected graph with V vertices labeled from 0 to V-1, determine if the graph contains any cycles. The graph is represented using an adjacency list where `adj[i]` lists all nodes connected to node `i`.

**Note**: The graph does not contain any self-edges (edges where a vertex is connected to itself).

## Examples

### Example 1
<img src="https://static.takeuforward.org/content/ProblemSetter-Eymk2V2R" />

```
Input: V = 6, adj = [[1, 3], [0, 2, 4], [1, 5], [0, 4], [1, 3, 5], [2, 4]]
Output: True
Explanation: The graph contains a cycle: 0 → 1 → 2 → 5 → 4 → 1
```

### Example 2
<img src="https://static.takeuforward.org/content/ProblemSetter-ddy9fw2d" />

```
Input: V = 4, adj = [[1, 2], [0], [0, 3], [2]]
Output: False
Explanation: The graph does not contain any cycles
```

## Intuition

In an undirected graph, a cycle is formed when a path exists that returns to the starting vertex without reusing an edge. The key insight is:

**During traversal, if we encounter a vertex that has already been visited and is not the immediate parent of the current vertex, a cycle exists.**

This works because:
- If we visit a node that's already been visited
- AND it's not our immediate parent (which would just be backtracking)
- Then we've found an alternative path to reach that node, forming a cycle

## Approach 1: Depth-First Search (DFS)

### Algorithm
1. Create an adjacency list representation of the graph using sets
2. Initialize a visited dictionary to track visited nodes
3. For each unvisited node, perform DFS
4. During DFS, if we encounter a visited node that's not the parent, return True (cycle found)
5. If no cycles found after checking all components, return False

### Implementation

```python
#Using depth first search
def solution(n,edges):
    nodes=[i for i in range(n)]
    graph={node:set() for node in nodes}
    for i,j in edges:
        graph[i].add(j)
        graph[j].add(i)
    def dfs(node,parent):
        visited[node]=True
        for adj_node in graph[node]:
            if(not visited[adj_node]):
                check=dfs(adj_node,node)
                if(check==True):
                    return True
            else:
                if(adj_node!=parent):
                    return True
        return False
    visited={i:False for i in nodes}
    for node in nodes:
        if(not visited[node]):
            check=dfs(node,-1)
            if(check==True):
                return True
    return False
```

### Time Complexity: O(V + E)
- V: Number of vertices
- E: Number of edges
- We visit each vertex once and each edge twice (once from each endpoint)

### Space Complexity: O(V)
- Visited dictionary: O(V)
- Recursion stack: O(V) in worst case
- Adjacency list with sets: O(V + E)

## Approach 2: Breadth-First Search (BFS)

### Algorithm
1. Create an adjacency list representation of the graph using sets
2. Initialize a visited dictionary to track visited nodes
3. For each unvisited node, perform BFS
4. Use a deque to store (node, parent) pairs
5. During BFS, if we encounter a visited node that's not the parent, return True
6. If no cycles found after checking all components, return False

### Implementation

```python
#Using breadth first search

from collections import deque
def solution(n,edges):
    nodes=[i for i in range(n)]
    graph={node:set() for node in nodes}
    for i,j in edges:
        graph[i].add(j)
        graph[j].add(i)

    def bfs(node):
        visited[node]=True
        queue=deque()
        queue.append((node,-1))
        while queue:
            node,parent=queue.popleft()
            for adj_node in graph[node]:
                if(not visited[adj_node]):
                    queue.append((adj_node,node))
                    visited[adj_node]=True
                else:
                    if(adj_node!=parent):
                        return True
        return False

    visited={i:False for i in nodes}
    for node in nodes:
        if(not visited[node]):
            check=bfs(node)
            if(check==True):
                return True
    return False
```

### Time Complexity: O(V + E)
- Same as DFS approach

### Space Complexity: O(V)
- Visited dictionary: O(V)
- Queue: O(V) in worst case
- Adjacency list with sets: O(V + E)

## Test Example

```python
n=6
edges=[(0, 1), (0, 3), (1, 2), (1, 4), (2, 5), (3, 4), (4, 5)]
print(solution(n,edges))
```

