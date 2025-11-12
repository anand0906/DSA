# Graph Problems - Revision Notes

## Table of Contents
1. [Connected Components](#1-connected-components)
2. [Number of Provinces](#2-number-of-provinces)
3. [Number of Islands](#3-number-of-islands)
4. [Number of Enclaves](#4-number-of-enclaves)
5. [Surrounded Regions](#5-surrounded-regions)
6. [Flood Fill Algorithm](#6-flood-fill-algorithm)
7. [Rotten Oranges](#7-rotten-oranges)
8. [Distance of Nearest Cell Having One](#8-distance-of-nearest-cell-having-one)
9. [Cycle Detection in Undirected Graphs](#9-cycle-detection-in-undirected-graphs)
10. [Bipartite Graph Detection](#10-bipartite-graph-detection)
11. [Comparison Table](#comparison-table)

---

## 1. Connected Components

### Problem Description
Given an undirected graph with V vertices (0 to V-1) and E edges, find the number of connected components. A connected component is a subgraph where there exists a path between any two vertices, and no vertex shares an edge with vertices outside the subgraph.

### Sample Test Cases
```python
# Example 1
V = 4, edges = [[0,1], [1,2]]
Output: 2
# Vertices {0, 1, 2} form one component, vertex 3 forms another

# Example 2
V = 7, edges = [[0, 1], [1, 2], [2, 3], [4, 5]]
Output: 3
# Components: {0, 1, 2, 3}, {4, 5}, {6}
```

### Approach: BFS Traversal

**Intuition**: For each unvisited node, start BFS to explore all connected nodes (one component). Count how many times we start a new BFS traversal.

**Code**:
```python
def solution(n, edges):
    # Step 1: Build adjacency list representation
    nodes = [i for i in range(n)]
    graph = {i: set() for i in nodes}  # Use set for O(1) lookup
    
    # Step 2: Add edges (undirected graph - add both directions)
    for i, j in edges:
        graph[i].add(j)
        graph[j].add(i)
    
    # Step 3: Initialize visited tracker
    visited = {i: False for i in nodes}
    
    def bfs(graph, node):
        """BFS to mark all nodes in current component"""
        visited[node] = True
        queue = [node]
        
        while queue:
            temp = queue.pop(0)  # Dequeue first element
            # Explore all adjacent nodes
            for adj in graph[temp]:
                if not visited[adj]:
                    queue.append(adj)
                    visited[adj] = True
    
    # Step 4: Count connected components
    count = 0
    for node in nodes:
        if not visited[node]:
            count += 1  # New component found
            bfs(graph, node)  # Mark all nodes in this component
    
    return count
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V + E)

---

## 2. Number of Provinces

### Problem Description
Given an n×n adjacency matrix representing an undirected graph, find the number of provinces. A province is a group of directly or indirectly connected cities with no connections outside the group.

### Sample Test Cases
```python
# Example 1
adj = [
    [1, 0, 0, 1],
    [0, 1, 1, 0],
    [0, 1, 1, 0],
    [1, 0, 0, 1]
]
Output: 2
# Province 1: Cities [0, 3], Province 2: Cities [1, 2]

# Example 2
adj = [
    [1, 0, 1],
    [0, 1, 0],
    [1, 0, 1]
]
Output: 2
# Province 1: Cities [0, 2], Province 2: City [1]
```

### Approach 1: BFS with Adjacency List Conversion

**Intuition**: Convert adjacency matrix to list, then count connected components using BFS.

**Code**:
```python
def solution(adj_matrix):
    n = len(adj_matrix)
    nodes = [i for i in range(n)]
    
    # Step 1: Convert adjacency matrix to adjacency list for easier traversal
    graph = {i: set() for i in nodes}
    for i in range(n):
        for j in range(n):
            if adj_matrix[i][j] == 1:
                graph[i].add(j)
    
    # Step 2: Initialize visited tracker
    visited = {i: False for i in nodes}
    
    def bfs(graph, node):
        """BFS to mark all cities in current province"""
        visited[node] = True
        queue = [node]
        
        while queue:
            temp = queue.pop(0)
            # Explore all connected cities
            for adj in graph[temp]:
                if not visited[adj]:
                    queue.append(adj)
                    visited[adj] = True
    
    # Step 3: Count provinces
    count = 0
    for node in nodes:
        if not visited[node]:
            count += 1  # New province found
            bfs(graph, node)
    
    return count
```

**Time Complexity**: O(V²)  
**Space Complexity**: O(V²)

### Approach 2: Direct DFS on Matrix

**Intuition**: Perform DFS directly on the adjacency matrix without conversion.

**Code**:
```python
def solution_optimized(adj_matrix):
    n = len(adj_matrix)
    visited = [False] * n
    
    def dfs(node):
        """DFS directly on adjacency matrix"""
        visited[node] = True
        # Check all possible neighbors
        for neighbor in range(n):
            # If connected (=1) and not visited, explore
            if adj_matrix[node][neighbor] == 1 and not visited[neighbor]:
                dfs(neighbor)
    
    # Count provinces by starting DFS from unvisited nodes
    count = 0
    for i in range(n):
        if not visited[i]:
            count += 1  # New province
            dfs(i)  # Mark all cities in this province
    
    return count
```

**Time Complexity**: O(V²)  
**Space Complexity**: O(V)

---

## 3. Number of Islands

### Problem Description
Given an N×M grid with '0' (water) and '1' (land), find the number of islands. An island is formed by connecting adjacent lands in all 8 directions (horizontal, vertical, and diagonal).

### Sample Test Cases
```python
# Example 1
grid = [
    ["1", "1", "1", "0", "1"],
    ["1", "0", "0", "0", "0"],
    ["1", "1", "1", "0", "1"],
    ["0", "0", "0", "1", "1"]
]
Output: 2

# Example 2
grid = [
    ["1", "0", "0", "0", "1"],
    ["0", "1", "0", "1", "0"],
    ["0", "0", "1", "0", "0"],
    ["0", "1", "0", "1", "0"]
]
Output: 1
```

### Approach: BFS with 8-Directional Traversal

**Intuition**: Each '1' and its connected '1's (in 8 directions) form one island. Use BFS to explore and mark all connected land cells for each island.

**Code**:
```python
def solution(grid):
    n, m = len(grid), len(grid[0])
    visited = [[False] * m for _ in range(n)]
    
    def bfs(row, col):
        """BFS to explore all connected land cells in 8 directions"""
        visited[row][col] = True
        queue = [(row, col)]
        
        while queue:
            r, c = queue.pop(0)
            
            # Check all 8 directions (combinations of -1, 0, 1 for row and col)
            for i in range(-1, 2):
                for j in range(-1, 2):
                    adj_row = r + i
                    adj_col = c + j
                    
                    # Check if within bounds
                    if (adj_row >= 0 and adj_col >= 0 and 
                        adj_row < n and adj_col < m):
                        
                        # If it's land ('1') and not visited
                        if (grid[adj_row][adj_col] == '1' and 
                            not visited[adj_row][adj_col]):
                            visited[adj_row][adj_col] = True
                            queue.append((adj_row, adj_col))
    
    # Step: Count islands by starting BFS from each unvisited land cell
    count = 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '1' and not visited[i][j]:
                count += 1  # New island found
                bfs(i, j)  # Mark all land cells in this island
    
    return count
```

**Time Complexity**: O(N × M)  
**Space Complexity**: O(N × M)

---

## 4. Number of Enclaves

### Problem Description
Given an N×M binary grid (0=sea, 1=land), find the number of land cells that cannot reach the grid boundary by moving in 4 directions (up, down, left, right).

### Sample Test Cases
```python
# Example 1
grid = [
    [0, 0, 0, 0],
    [1, 0, 1, 0],
    [0, 1, 1, 0],
    [0, 0, 0, 0]
]
Output: 3
# Land cells at (1,2), (2,1), (2,2) are enclosed

# Example 2
grid = [
    [0, 0, 0, 1],
    [0, 0, 0, 1],
    [0, 1, 1, 0],
    [0, 0, 1, 0],
    [0, 0, 0, 0]
]
Output: 3
```

### Approach: Boundary BFS

**Intuition**: Mark all land cells connected to the boundary (these can reach the edge). Count the remaining unmarked land cells as enclaves.

**Code**:
```python
def solution(grid):
    n, m = len(grid), len(grid[0])
    visited = [[False] * m for _ in range(n)]
    
    def bfs(row, col):
        """BFS to mark all land cells connected to boundary"""
        visited[row][col] = True
        queue = [(row, col)]
        
        while queue:
            r, c = queue.pop(0)
            
            # Check 4 directions: right, left, up, down
            for i, j in zip([0, 0, -1, 1], [1, -1, 0, 0]):
                adj_row = r + i
                adj_col = c + j
                
                # Check if within bounds
                if (adj_row >= 0 and adj_col >= 0 and 
                    adj_row < n and adj_col < m):
                    
                    # If it's land (1) and not visited
                    if (grid[adj_row][adj_col] == 1 and 
                        not visited[adj_row][adj_col]):
                        visited[adj_row][adj_col] = True
                        queue.append((adj_row, adj_col))
    
    # Step 1: Mark all boundary-connected land cells (these can reach edge)
    for i in range(n):
        for j in range(m):
            # Check if cell is on boundary
            if i == 0 or i == n-1 or j == 0 or j == m-1:
                if grid[i][j] == 1 and not visited[i][j]:
                    bfs(i, j)
    
    # Step 2: Count unvisited land cells (these are enclaves)
    count = 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1 and not visited[i][j]:
                count += 1
    
    return count
```

**Time Complexity**: O(N × M)  
**Space Complexity**: O(N × M)

---

## 5. Surrounded Regions

### Problem Description
Given an N×M grid with 'O' and 'X', replace all 'O' cells surrounded by 'X' with 'X'. An 'O' is surrounded if it's enclosed by 'X' in all 4 directions.

### Sample Test Cases
```python
# Example 1
mat = [
    ["X", "X", "X", "X"],
    ["X", "O", "O", "X"],
    ["X", "X", "O", "X"],
    ["X", "O", "X", "X"]
]
Output = [
    ["X", "X", "X", "X"],
    ["X", "X", "X", "X"],
    ["X", "X", "X", "X"],
    ["X", "O", "X", "X"]
]
# Only (3,1) remains 'O' as it's on boundary

# Example 2
mat = [
    ["X", "X", "X"],
    ["X", "O", "X"],
    ["X", "X", "X"]
]
Output = [
    ["X", "X", "X"],
    ["X", "X", "X"],
    ["X", "X", "X"]
]
```

### Approach: Boundary BFS with Replacement

**Intuition**: Mark all 'O' cells connected to boundary (these cannot be surrounded). Replace all unmarked 'O' cells with 'X'.

**Code**:
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
                
                # Check if within bounds
                if (adj_row >= 0 and adj_col >= 0 and 
                    adj_row < n and adj_col < m):
                    
                    # If it's 'O' and not visited
                    if (grid[adj_row][adj_col] == 'O' and 
                        not visited[adj_row][adj_col]):
                        visited[adj_row][adj_col] = True
                        queue.append((adj_row, adj_col))
    
    # Step 1: Mark all boundary-connected 'O' cells (these are safe)
    for i in range(n):
        for j in range(m):
            # Check if cell is on boundary
            if i == 0 or i == n-1 or j == 0 or j == m-1:
                if grid[i][j] == 'O' and not visited[i][j]:
                    bfs(i, j)
    
    # Step 2: Replace unvisited 'O' cells with 'X' (these are surrounded)
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 'O' and not visited[i][j]:
                grid[i][j] = 'X'
    
    return grid
```

**Time Complexity**: O(N × M)  
**Space Complexity**: O(N × M)

---

## 6. Flood Fill Algorithm

### Problem Description
Given a 2D image (grid of integers), a starting pixel (sr, sc), and a new color, perform flood fill. Change all connected pixels (4-directionally) that have the same color as the starting pixel to the new color.

### Sample Test Cases
```python
# Example 1
image = [[1, 1, 1], 
         [1, 1, 0], 
         [1, 0, 1]]
sr = 1, sc = 1, newColor = 2
Output = [[2, 2, 2], 
          [2, 2, 0], 
          [2, 0, 1]]

# Example 2
image = [[0, 1, 0], 
         [1, 1, 0], 
         [0, 0, 1]]
sr = 2, sc = 2, newColor = 3
Output = [[0, 1, 0], 
          [1, 1, 0], 
          [0, 0, 3]]
```

### Approach: BFS with Color Change

**Intuition**: Starting from the given pixel, use BFS to explore all 4-directionally connected pixels with the same initial color and change them to the new color.

**Code**:
```python
def flood_fill(grid, srcRow, srcCol, newColor):
    n, m = len(grid), len(grid[0])
    queue = [(srcRow, srcCol)]
    initColor = grid[srcRow][srcCol]
    
    # Edge case: If new color is same as initial, no change needed
    if newColor == initColor:
        return grid
    
    # Mark starting pixel with new color
    grid[srcRow][srcCol] = newColor
    
    # BFS to fill connected pixels
    while queue:
        r, c = queue.pop(0)
        
        # Check all 4 directions: right, left, up, down
        for i, j in zip([0, 0, -1, 1], [1, -1, 0, 0]):
            adj_row = r + i
            adj_col = c + j
            
            # Check if within bounds
            if (adj_row >= 0 and adj_col >= 0 and 
                adj_row < n and adj_col < m):
                # If adjacent pixel has initial color
                if grid[adj_row][adj_col] == initColor:
                    grid[adj_row][adj_col] = newColor  # Fill with new color
                    queue.append((adj_row, adj_col))  # Add to queue for further exploration
    
    return grid
```

**Time Complexity**: O(N × M)  
**Space Complexity**: O(N × M)

---

## 7. Rotten Oranges

### Problem Description
Given an N×M grid where 2=rotten orange, 1=fresh orange, 0=empty cell. Every minute, fresh oranges adjacent (4-directionally) to rotten oranges become rotten. Return minimum minutes to rot all oranges, or -1 if impossible.

### Sample Test Cases
```python
# Example 1
grid = [[2, 1, 1], 
        [0, 1, 1], 
        [1, 0, 1]]
Output: -1
# Orange at (2,0) cannot be reached

# Example 2
grid = [[2, 1, 1], 
        [1, 1, 0], 
        [0, 1, 1]]
Output: 4
```

### Approach: Multi-Source BFS

**Intuition**: Start BFS from all initially rotten oranges simultaneously. Each level of BFS represents one minute. Track the maximum time taken.

**Code**:
```python
def solution(grid):
    n, m = len(grid), len(grid[0])
    queue = []
    fresh = 0
    ans = 0
    
    # Step 1: Find all initially rotten oranges and count fresh ones
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 2:
                queue.append((i, j, 0))  # (row, col, time)
            elif grid[i][j] == 1:
                fresh += 1
    
    # Step 2: BFS to spread rot level by level
    while queue:
        r, c, time = queue.pop(0)
        
        # Check all 4 directions: right, left, up, down
        for i, j in zip([0, 0, -1, 1], [1, -1, 0, 0]):
            adj_row = r + i
            adj_col = c + j
            
            # Check if within bounds
            if (adj_row >= 0 and adj_col >= 0 and 
                adj_row < n and adj_col < m):
                
                # If adjacent cell has fresh orange
                if grid[adj_row][adj_col] == 1:
                    grid[adj_row][adj_col] = 2  # Make it rotten
                    fresh -= 1  # Decrease fresh count
                    queue.append((adj_row, adj_col, time + 1))  # Add with incremented time
                    ans = max(ans, time + 1)  # Track maximum time
    
    # Step 3: Check if all oranges are rotten
    if fresh > 0:
        return -1  # Some oranges cannot be rotten
    return ans
```

**Time Complexity**: O(N × M)  
**Space Complexity**: O(N × M)

---

## 8. Distance of Nearest Cell Having One

### Problem Description
Given an N×M binary grid, find the distance of the nearest '1' for each cell. Distance is calculated as Manhattan distance: |i1-i2| + |j1-j2|.

### Sample Test Cases
```python
# Example 1
grid = [[0, 1, 1, 0], 
        [1, 1, 0, 0], 
        [0, 0, 1, 1]]
Output = [[1, 0, 0, 1], 
          [0, 0, 1, 1], 
          [1, 1, 0, 0]]

# Example 2
grid = [[1, 0, 1], 
        [1, 1, 0], 
        [1, 0, 0]]
Output = [[0, 1, 0], 
          [0, 0, 1], 
          [0, 1, 2]]
```

### Approach: Multi-Source BFS

**Intuition**: Start BFS from all '1' cells simultaneously. BFS naturally explores cells in order of increasing distance, ensuring the first visit to each cell is via the shortest path.

**Code**:
```python
def solution(grid):
    n, m = len(grid), len(grid[0])
    queue = []
    
    # Step 1: Add all '1' cells to queue with distance 0
    # Mark them as visited temporarily with -1
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1:
                queue.append((i, j, 0))  # (row, col, distance)
                grid[i][j] = -1  # Mark as visited
    
    # Step 2: BFS to find minimum distances from all '1' cells simultaneously
    while queue:
        r, c, time = queue.pop(0)
        
        # Check all 4 directions: right, left, up, down
        for i, j in zip([0, 0, -1, 1], [1, -1, 0, 0]):
            adj_row = r + i
            adj_col = c + j
            
            # Check if within bounds
            if (adj_row >= 0 and adj_col >= 0 and 
                adj_row < n and adj_col < m):
                
                # If unvisited '0' cell
                if grid[adj_row][adj_col] == 0:
                    grid[adj_row][adj_col] = time + 1  # Set distance
                    queue.append((adj_row, adj_col, time + 1))  # Continue BFS
    
    # Step 3: Restore '1' cells (marked as -1) back to distance 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == -1:
                grid[i][j] = 0
    
    return grid
```

**Time Complexity**: O(N × M)  
**Space Complexity**: O(N × M)

---

## 9. Cycle Detection in Undirected Graphs

### Problem Description
Given an undirected graph with V vertices (0 to V-1) represented as an adjacency list, determine if the graph contains any cycles.

### Sample Test Cases
```python
# Example 1
V = 6, adj = [[1, 3], [0, 2, 4], [1, 5], [0, 4], [1, 3, 5], [2, 4]]
Output: True
# Cycle exists: 0 → 1 → 2 → 5 → 4 → 1

# Example 2
V = 4, adj = [[1, 2], [0], [0, 3], [2]]
Output: False
```

### Approach 1: Depth-First Search (DFS)

**Intuition**: During DFS traversal, if we encounter a visited node that is not the immediate parent of the current node, a cycle exists (we found an alternative path).

**Code**:
```python
def solution(n, edges):
    # Step 1: Build adjacency list
    nodes = [i for i in range(n)]
    graph = {node: set() for node in nodes}
    for i, j in edges:
        graph[i].add(j)
        graph[j].add(i)
    
    def dfs(node, parent):
        """DFS with parent tracking to detect cycles"""
        visited[node] = True
        # Explore all adjacent nodes
        for adj_node in graph[node]:
            if not visited[adj_node]:
                # Recursively explore unvisited neighbor
                check = dfs(adj_node, node)
                if check == True:
                    return True  # Cycle found in recursion
            else:
                # If visited and not parent, cycle exists
                if adj_node != parent:
                    return True
        return False
    
    # Step 2: Check all components for cycles
    visited = {i: False for i in nodes}
    for node in nodes:
        if not visited[node]:
            check = dfs(node, -1)  # Start DFS with no parent (-1)
            if check == True:
                return True
    return False
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

### Approach 2: Breadth-First Search (BFS)

**Intuition**: Use BFS with (node, parent) pairs. If we encounter a visited node that is not the parent, a cycle exists.

**Code**:
```python
from collections import deque

def solution(n, edges):
    # Step 1: Build adjacency list
    nodes = [i for i in range(n)]
    graph = {node: set() for node in nodes}
    for i, j in edges:
        graph[i].add(j)
        graph[j].add(i)
    
    def bfs(node):
        """BFS with parent tracking to detect cycles"""
        visited[node] = True
        queue = deque()
        queue.append((node, -1))  # (current_node, parent)
        
        while queue:
            node, parent = queue.popleft()
            # Explore all adjacent nodes
            for adj_node in graph[node]:
                if not visited[adj_node]:
                    queue.append((adj_node, node))  # Mark node as parent
                    visited[adj_node] = True
                else:
                    # If visited and not parent, cycle exists
                    if adj_node != parent:
                        return True
        return False
    
    # Step 2: Check all components for cycles
    visited = {i: False for i in nodes}
    for node in nodes:
        if not visited[node]:
            check = bfs(node)
            if check == True:
                return True
    return False
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

---

## 10. Bipartite Graph Detection

### Problem Description
Given an undirected graph with V vertices represented as an adjacency list, determine if the graph is bipartite. A graph is bipartite if nodes can be partitioned into two independent sets A and B such that every edge connects a node in A and a node in B.

### Sample Test Cases
```python
# Example 1
V = 4, adj = [[1,3], [0,2], [1,3], [0,2]]
Output: True
# Partition: {0, 2} and {1, 3}

# Example 2
V = 4, adj = [[1,2,3], [0,2], [0,1,3], [0,2]]
Output: False
# Contains odd cycle, cannot partition
```

### Approach 1: DFS with 2-Coloring

**Intuition**: Try to color the graph with 2 colors alternately. If any adjacent nodes have the same color, the graph is not bipartite (contains odd cycle).

**Code**:
```python
def solution(n, edges):
    # Step 1: Build adjacency list
    nodes = [i for i in range(n)]
    graph = {node: set() for node in nodes}
    for i, j in edges:
        graph[i].add(j)
        graph[j].add(i)
    
    def dfs(node, current_color):
        """DFS to color nodes alternately"""
        visited[node] = True
        color[node] = current_color
        
        # Try to color all adjacent nodes with opposite color
        for adj_node in graph[node]:
            if not visited[adj_node]:
                # Color with opposite color (0 becomes 1, 1 becomes 0)
                check = dfs(adj_node, int(not current_color))
                if check == False:
                    return False  # Conflict found in recursion
            else:
                # If already colored with same color, not bipartite
                if color[adj_node] == current_color:
                    return False
        return True
    
    # Step 2: Check all components
    visited = {i: False for i in nodes}
    color = {i: -1 for i in nodes}
    for node in nodes:
        if not visited[node]:
            check = dfs(node, 0)  # Start coloring with color 0
            if check == False:
                return False
    return True
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

### Approach 2: BFS with 2-Coloring

**Intuition**: Use BFS to color nodes level by level with alternating colors. Check for color conflicts.

**Code**:
```python
from collections import deque

def solution(n, edges):
    nodes = [i for i in range(n)]
    graph = {node: set() for node in nodes}
    for i, j in edges:
        graph[i].add(j)
        graph[j].add(i)
    
    def bfs(node):
        visited[node] = True
        queue = deque()
        queue.append(node)
        color[node] = 0
        while queue:
            node = queue.popleft()
            for adj_node in graph[node]:
                if not visited[adj_node]:
                    queue.append(adj_node)
                    visited[adj_node] = True
                    color[adj_node] = int(not color[node])
                else:
                    if color[node] == color[adj_node]:  # Conflict
                        return False
        return True
    
    visited = {i: False for i in nodes}
    color = {i: -1 for i in nodes}
    for node in nodes:
        if not visited[node]:
            check = bfs(node)
            if check == False:
                return False
    return True
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

---

## Comparison Table

| # | Problem | Key Technique | Directions | Time | Space | Key Insight |
|---|---------|---------------|------------|------|-------|-------------|
| 1 | Connected Components | BFS/DFS | N/A | O(V+E) | O(V+E) | Count BFS/DFS starts |
| 2 | Number of Provinces | BFS/DFS | N/A | O(V²) | O(V²) | Same as connected components with matrix |
| 3 | Number of Islands | BFS | 8-directional | O(N×M) | O(N×M) | Connected '1's form islands |
| 4 | Number of Enclaves | Boundary BFS | 4-directional | O(N×M) | O(N×M) | Mark boundary-connected, count rest |
| 5 | Surrounded Regions | Boundary BFS | 4-directional | O(N×M) | O(N×M) | Mark safe cells, replace rest |
| 6 | Flood Fill | BFS | 4-directional | O(N×M) | O(N×M) | Change connected cells with same color |
| 7 | Rotten Oranges | Multi-Source BFS | 4-directional | O(N×M) | O(N×M) | Level-wise BFS tracks time |
| 8 | Distance to Nearest 1 | Multi-Source BFS | 4-directional | O(N×M) | O(N×M) | BFS from all '1's simultaneously |
| 9 | Cycle Detection | BFS/DFS with Parent | N/A | O(V+E) | O(V) | Visited node ≠ parent → cycle |
| 10 | Bipartite Graph | BFS/DFS with Coloring | N/A | O(V+E) | O(V) | 2-color graph, check conflicts |

### Problem Categories

**Component-Based Problems**: 1, 2  
**Grid Traversal Problems**: 3, 4, 5, 6, 7, 8  
**Graph Property Detection**: 9, 10  

**Multi-Source BFS**: 7, 8  
**Boundary Processing**: 4, 5  
**Parent Tracking**: 9  
**Coloring Technique**: 10
