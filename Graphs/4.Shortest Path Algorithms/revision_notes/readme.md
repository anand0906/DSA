# Shortest Path Algorithms - Revision Notes

## Table of Contents
1. [Shortest Path in DAG](#1-shortest-path-in-dag)
2. [Shortest Path in Unweighted Graph](#2-shortest-path-in-unweighted-graph)
3. [Dijkstra's Algorithm](#3-dijkstras-algorithm)
4. [Print Shortest Path](#4-print-shortest-path)
5. [Cheapest Flights within K Stops](#5-cheapest-flights-within-k-stops)
6. [Number of Ways to Arrive at Destination](#6-number-of-ways-to-arrive-at-destination)
7. [Word Ladder I](#7-word-ladder-i)
8. [Word Ladder II](#8-word-ladder-ii)
9. [Minimum Multiplications to Reach End](#9-minimum-multiplications-to-reach-end)
10. [Shortest Distance in Binary Maze](#10-shortest-distance-in-binary-maze)
11. [Path with Minimum Effort](#11-path-with-minimum-effort)
12. [Bellman-Ford Algorithm](#12-bellman-ford-algorithm)
13. [Floyd-Warshall Algorithm](#13-floyd-warshall-algorithm)
14. [Find City with Smallest Number of Neighbors](#14-find-city-with-smallest-number-of-neighbors)
15. [A* Search Algorithm](#15-a-search-algorithm)
16. [Johnson's Algorithm](#16-johnsons-algorithm)
17. [Comparison Table](#comparison-table)

---

## 1. Shortest Path in DAG

### Problem Description
Given a Directed Acyclic Graph (DAG) with N vertices (0 to N-1) and M weighted edges, find the shortest path from vertex 0 to all other vertices. Return -1 for unreachable vertices.

### Sample Test Cases
```python
# Example 1
N = 4, M = 2
edges = [[0,1,2], [0,2,1]]
Output: [0, 2, 1, -1]

# Example 2
N = 6, M = 7
edges = [[0,1,2], [0,4,1], [4,5,4], [4,2,2], [1,2,3], [2,3,6], [5,3,1]]
Output: [0, 2, 3, 6, 1, 5]
```

### Approach: Topological Sort + DP

**Intuition**: In a DAG, if we process vertices in topological order, by the time we reach any vertex, we've already found shortest paths to all vertices that can reach it. No need to revisit!

**Code**:
```python
from collections import defaultdict

def shortestPath(N, M, edges):
    # Step 1: Build adjacency list
    graph = defaultdict(list)
    for u, v, weight in edges:
        graph[u].append((v, weight))
    
    # Step 2: Topological sort using DFS
    def topological_sort():
        visited = [False] * N
        stack = []
        
        def dfs(node):
            visited[node] = True
            for neighbor, _ in graph[node]:
                if not visited[neighbor]:
                    dfs(neighbor)
            stack.append(node)  # Add after processing all dependencies
        
        for i in range(N):
            if not visited[i]:
                dfs(i)
        
        return stack[::-1]  # Reverse to get correct order
    
    # Step 3: Get topological order
    topo_order = topological_sort()
    
    # Step 4: Initialize distances
    dist = [float('inf')] * N
    dist[0] = 0  # Source distance is 0
    
    # Step 5: Process vertices in topological order
    for node in topo_order:
        if dist[node] != float('inf'):  # If node is reachable
            for neighbor, weight in graph[node]:
                # Relax the edge
                if dist[node] + weight < dist[neighbor]:
                    dist[neighbor] = dist[node] + weight
    
    # Step 6: Convert unreachable vertices to -1
    for i in range(N):
        if dist[i] == float('inf'):
            dist[i] = -1
    
    return dist
```

**Time Complexity**: O(N + M)  
**Space Complexity**: O(N + M)

---

## 2. Shortest Path in Unweighted Graph

### Problem Description
Given an undirected graph with N vertices and M edges where all edges have unit weight, find the shortest path from vertex 0 to all other vertices. Return -1 for unreachable vertices.

### Sample Test Cases
```python
# Example 1
n = 9, m = 10
edges = [[0,1],[0,3],[3,4],[4,5],[5,6],[1,2],[2,6],[6,7],[7,8],[6,8]]
Output: [0, 1, 2, 1, 2, 3, 3, 4, 4]

# Example 2
n = 8, m = 10
edges = [[1,0],[2,1],[0,3],[3,7],[3,4],[7,4],[7,6],[4,5],[4,6],[6,5]]
Output: [0, 1, 2, 1, 2, 3, 3, 2]
```

### Approach: BFS

**Intuition**: BFS explores nodes level by level. In unweighted graphs, this means the first time we reach any node is via the shortest path!

**Code**:
```python
from collections import deque

def shortestPath(N, M, edges):
    # Step 1: Build adjacency list (undirected)
    graph = [[] for _ in range(N)]
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
    
    # Step 2: Initialize distance array
    dist = [-1] * N  # Initialize with -1 (unreachable)
    dist[0] = 0      # Source distance is 0
    
    # Step 3: BFS traversal
    queue = deque([0])
    
    while queue:
        current = queue.popleft()
        
        # Process all neighbors
        for neighbor in graph[current]:
            if dist[neighbor] == -1:  # Not visited
                dist[neighbor] = dist[current] + 1
                queue.append(neighbor)
    
    return dist
```

**Time Complexity**: O(N + M)  
**Space Complexity**: O(N + M)

---

## 3. Dijkstra's Algorithm

### Problem Description
Given a weighted undirected graph with V vertices and a source vertex S, find the shortest distance from S to all other vertices. Return 10^9 for unreachable vertices.

### Sample Test Cases
```python
# Example 1
V = 2, adj = [[[1, 9]], [[0, 9]]], S = 0
Output: [0, 9]

# Example 2
V = 3, adj = [[[1, 1], [2, 6]], [[2, 3], [0, 1]], [[1, 3], [0, 6]]], S = 2
Output: [4, 3, 0]
```

### Approach: Priority Queue (Min-Heap)

**Intuition**: Always process the vertex with smallest known distance first. This greedy approach ensures we find optimal shortest paths.

**Code**:
```python
import heapq

def dijkstra(V, adj, source):
    # Step 1: Initialize distance array
    dist = [10**9] * V
    dist[source] = 0
    
    # Step 2: Priority queue: (distance, node)
    pq = [(0, source)]
    
    while pq:
        current_dist, u = heapq.heappop(pq)
        
        # Skip if we've already processed with better distance
        if current_dist > dist[u]:
            continue
        
        # Step 3: Process all neighbors
        for neighbor, weight in adj[u]:
            new_dist = dist[u] + weight
            
            # Step 4: If we found a shorter path
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    
    return dist
```

**Time Complexity**: O((V + E) log V)  
**Space Complexity**: O(V + E)

---

## 4. Print Shortest Path

### Problem Description
Given a weighted undirected graph with n vertices (1 to n) and m edges, find the shortest path from vertex 1 to vertex n. Return [total_weight, vertex1, vertex2, ..., vertexN] or [-1] if no path exists.

### Sample Test Cases
```python
# Example 1
n = 5, m = 6
edges = [[1,2,2], [2,5,5], [2,3,4], [1,4,1], [4,3,3], [3,5,1]]
Output: [5, 1, 4, 3, 5]

# Example 2
n = 4, m = 4
edges = [[1,2,2], [2,3,4], [1,4,1], [4,3,3]]
Output: [1, 1, 4]
```

### Approach: Modified Dijkstra with Parent Tracking

**Intuition**: Track parent of each vertex during Dijkstra's. After finding shortest distances, backtrack from destination to source using parents.

**Code**:
```python
import heapq

def shortestPath(n, m, edges):
    # Step 1: Build adjacency list
    graph = [[] for _ in range(n + 1)]
    for u, v, weight in edges:
        graph[u].append((v, weight))
        graph[v].append((u, weight))
    
    # Step 2: Dijkstra with parent tracking
    dist = [float('inf')] * (n + 1)
    parent = [-1] * (n + 1)
    
    dist[1] = 0
    pq = [(0, 1)]  # (distance, node)
    
    while pq:
        d, u = heapq.heappop(pq)
        
        if d > dist[u]:
            continue
        
        for v, weight in graph[u]:
            new_dist = dist[u] + weight
            if new_dist < dist[v]:
                dist[v] = new_dist
                parent[v] = u  # Track parent
                heapq.heappush(pq, (new_dist, v))
    
    # Step 3: Check if destination is reachable
    if dist[n] == float('inf'):
        return [-1]
    
    # Step 4: Reconstruct path from n to 1
    path = []
    current = n
    while current != -1:
        path.append(current)
        current = parent[current]
    
    path.reverse()  # Reverse to get 1 to n
    return [dist[n]] + path
```

**Time Complexity**: O((V + E) log V)  
**Space Complexity**: O(V + E)

---

## 5. Cheapest Flights within K Stops

### Problem Description
Given n cities and flights with costs, find the cheapest price from source to destination with at most k stops. Return -1 if no valid route exists.

### Sample Test Cases
```python
# Example 1
n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]]
src = 0, dst = 3, k = 1
Output: 700

# Example 2
n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 1
Output: 200
```

### Approach: BFS with Stop Counting

**Intuition**: Process paths level by level based on number of stops. Since stops increase monotonically, use simple queue instead of priority queue.

**Code**:
```python
from collections import deque

def findCheapestPrice(n, flights, src, dst, k):
    # Step 1: Build adjacency list
    graph = [[] for _ in range(n)]
    for u, v, cost in flights:
        graph[u].append((v, cost))
    
    # Step 2: Use simple queue since stops increase monotonically
    queue = deque([(src, 0, 0)])  # (city, cost, stops)
    min_cost = [float('inf')] * n
    min_cost[src] = 0
    
    while queue:
        city, cost, stops = queue.popleft()
        
        # Skip if exceeded stop limit
        if stops > k:
            continue
        
        # Step 3: Process all neighbors
        for next_city, flight_cost in graph[city]:
            new_cost = cost + flight_cost
            
            # Only proceed if we found a cheaper path
            if new_cost < min_cost[next_city]:
                min_cost[next_city] = new_cost
                queue.append((next_city, new_cost, stops + 1))
    
    return min_cost[dst] if min_cost[dst] != float('inf') else -1
```

**Time Complexity**: O(V + E × K)  
**Space Complexity**: O(V + E)

---

## 6. Number of Ways to Arrive at Destination

### Problem Description
Given n intersections (0 to n-1) connected by bi-directional roads with travel times, find the number of ways to travel from 0 to n-1 in shortest time. Return answer modulo 10^9 + 7.

### Sample Test Cases
```python
# Example 1
n = 7, m = 10
roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
Output: 4

# Example 2
n = 6, m = 8
roads = [[0,5,8],[0,2,2],[0,1,1],[1,3,3],[1,2,3],[2,5,6],[3,4,2],[4,5,2]]
Output: 3
```

### Approach: Modified Dijkstra with Path Counting

**Intuition**: Track both shortest distance and number of ways to achieve it. When we find equal-length path, add ways; when shorter, replace ways.

**Code**:
```python
import heapq

MOD = 10**9 + 7

def countPaths(n, roads):
    # Step 1: Build adjacency list
    graph = [[] for _ in range(n)]
    for u, v, time in roads:
        graph[u].append((v, time))
        graph[v].append((u, time))
    
    # Step 2: Dijkstra with path counting
    dist = [float('inf')] * n
    ways = [0] * n
    
    dist[0] = 0
    ways[0] = 1
    
    pq = [(0, 0)]  # (distance, node)
    
    while pq:
        d, u = heapq.heappop(pq)
        
        # Skip outdated entries
        if d > dist[u]:
            continue
        
        for v, time in graph[u]:
            new_dist = dist[u] + time
            
            if new_dist < dist[v]:
                # Shorter path found: update distance and reset ways
                dist[v] = new_dist
                ways[v] = ways[u]
                heapq.heappush(pq, (new_dist, v))
                
            elif new_dist == dist[v]:
                # Equal path found: add to ways count
                ways[v] = (ways[v] + ways[u]) % MOD
    
    return ways[n - 1]
```

**Time Complexity**: O((V + E) log V)  
**Space Complexity**: O(V + E)

---

## 7. Word Ladder I

### Problem Description
Given startWord, targetWord, and wordList, find the length of shortest transformation sequence. Each transformation changes only one letter, and each intermediate word must be in wordList.

### Sample Test Cases
```python
# Example 1
wordList = ["des","der","dfr","dgt","dfs"]
startWord = "der", targetWord = "dfs"
Output: 3  # der → dfr → dfs

# Example 2
wordList = ["hot", "dot", "dog", "lot", "log"]
startWord = "hit", targetWord = "cog"
Output: 0  # "cog" not in wordList
```

### Approach: BFS on Word Graph

**Intuition**: BFS explores words level by level. First time we reach target is guaranteed shortest transformation sequence.

**Code**:
```python
from collections import deque

def wordLadderLength(startWord, targetWord, wordList):
    # Early check: if target not in list, return 0
    if targetWord not in wordList:
        return 0
    
    # Convert to set for O(1) operations
    wordSet = set(wordList)
    queue = deque([(startWord, 1)])
    wordSet.discard(startWord)  # Remove to avoid revisiting
    
    while queue:
        word, steps = queue.popleft()
        
        # Check if we reached target
        if word == targetWord:
            return steps
        
        # Try all possible single character transformations
        for i in range(len(word)):
            for c in 'abcdefghijklmnopqrstuvwxyz':
                newWord = word[:i] + c + word[i+1:]
                
                # If transformation is valid and exists in wordList
                if newWord in wordSet:
                    queue.append((newWord, steps + 1))
                    wordSet.discard(newWord)  # Mark as visited
    
    return 0  # No transformation sequence found
```

**Time Complexity**: O(M² × N) where M = word length, N = wordList size  
**Space Complexity**: O(N × M)

---

## 8. Word Ladder II

### Problem Description
Find ALL shortest transformation sequences from startWord to targetWord. Return all possible shortest paths.

### Sample Test Cases
```python
# Example 1
startWord = "der", targetWord = "dfs"
wordList = ["des", "der", "dfr", "dgt", "dfs"]
Output: [["der", "dfr", "dfs"], ["der", "des", "dfs"]]

# Example 2
startWord = "gedk", targetWord = "geek"
wordList = ["geek", "gefk"]
Output: [["gedk", "geek"]]
```

### Approach: BFS + DFS Backtracking

**Intuition**: Use BFS to build level map showing shortest distance to each word. Then use DFS to reconstruct all paths by only following edges that are part of shortest paths.

**Code**:
```python
from collections import deque, defaultdict

def findSequences(beginWord, endWord, wordList):
    if endWord not in wordList:
        return []
    
    wordSet = set(wordList)
    
    # Phase 1: BFS to build level map
    queue = deque([(beginWord, 1)])
    wordSet.discard(beginWord)
    
    levels = defaultdict(int)
    levels[beginWord] = 1
    
    while queue:
        word, steps = queue.popleft()
        
        for i in range(len(word)):
            for char in "abcdefghijklmnopqrstuvwxyz":
                newWord = word[:i] + char + word[i+1:]
                
                if newWord in wordSet:
                    queue.append((newWord, steps + 1))
                    levels[newWord] = steps + 1
                    wordSet.discard(newWord)
    
    # Phase 2: DFS to reconstruct all paths
    def dfs(word, path):
        if word == beginWord:
            ans.append(path[::-1])  # Reverse since we go target→start
            return
        
        for i in range(len(word)):
            for char in "abcdefghijklmnopqrstuvwxyz":
                newWord = word[:i] + char + word[i+1:]
                
                # Only follow edges part of shortest paths
                if newWord in levels and levels[word] == (levels[newWord] + 1):
                    dfs(newWord, path + [newWord])
    
    ans = []
    if endWord in levels:
        dfs(endWord, [endWord])
    
    return ans
```

**Time Complexity**: O(N × M² × 26^L) where L = path length  
**Space Complexity**: O(N × M + P × L) where P = number of paths

---

## 9. Minimum Multiplications to Reach End

### Problem Description
Given start number, end number, and array of multipliers, find minimum steps to transform start into end. At each step, multiply current number by any array element, then mod 100000.

### Sample Test Cases
```python
# Example 1
arr = [2, 5, 7], start = 3, end = 30
Output: 2  # 3 → 6 → 30

# Example 2
arr = [3, 4, 65], start = 7, end = 66175
Output: 4  # 7 → 21 → 63 → 4095 → 66175
```

### Approach: BFS on State Space

**Intuition**: Each number (0-99999) is a node. Multiplying by array elements creates edges. BFS finds shortest path from start to end node.

**Code**:
```python
def minimumMultiplications(arr, start, end):
    # Base case: already at target
    if start == end:
        return 0
    
    # BFS setup
    queue = [(start, 0)]  # (current_number, steps_taken)
    min_steps = [float('inf')] * 100000
    min_steps[start] = 0
    
    while queue:
        current_num, steps = queue.pop(0)
        
        # Try multiplying with each number in array
        for multiplier in arr:
            new_num = (current_num * multiplier) % 100000
            
            # Found target!
            if new_num == end:
                return steps + 1
            
            # Found shorter path to this number
            if steps + 1 < min_steps[new_num]:
                min_steps[new_num] = steps + 1
                queue.append((new_num, steps + 1))
    
    return -1  # Target unreachable
```

**Time Complexity**: O(100000 × M) where M = array size  
**Space Complexity**: O(100000)

---

## 10. Shortest Distance in Binary Maze

### Problem Description
Given n×m grid with 0s (walls) and 1s (paths), find shortest distance from source to destination. Can only move through 1s in 4 directions.

### Sample Test Cases
```python
# Example 1
grid = [[1, 1, 1, 1],
        [1, 1, 0, 1],
        [1, 1, 1, 1],
        [1, 1, 0, 0],
        [1, 0, 0, 1]]
source = [0, 1], destination = [2, 2]
Output: 3

# Example 2
grid = [[1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 0],
        [1, 0, 1, 0, 1]]
source = [0, 0], destination = [3, 4]
Output: -1
```

### Approach: BFS on Grid

**Intuition**: BFS naturally finds shortest path in unweighted grids by exploring cells level by level.

**Code**:
```python
from collections import deque

def shortestPath(grid, source, destination):
    n, m = len(grid), len(grid[0])
    sr, sc = source
    dr, dc = destination
    
    # Validation
    if grid[sr][sc] == 0 or grid[dr][dc] == 0:
        return -1
    if (sr, sc) == (dr, dc):
        return 0
    
    # Distance matrix
    dist = [[10**9] * m for _ in range(n)]
    dist[sr][sc] = 0
    
    # Direction vectors: up, right, down, left
    directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]
    
    # BFS queue
    q = deque([(sr, sc)])
    
    while q:
        r, c = q.popleft()
        current = dist[r][c]
        
        for dr_delta, dc_delta in directions:
            nr, nc = r + dr_delta, c + dc_delta
            
            # Check valid neighbor and passable cell
            if 0 <= nr < n and 0 <= nc < m and grid[nr][nc] == 1:
                # Relax edge if shorter
                if current + 1 < dist[nr][nc]:
                    dist[nr][nc] = current + 1
                    # Early exit if destination reached
                    if (nr, nc) == (dr, dc):
                        return dist[nr][nc]
                    q.append((nr, nc))
    
    return -1
```

**Time Complexity**: O(n × m)  
**Space Complexity**: O(n × m)

---

## 11. Path with Minimum Effort

### Problem Description
Given a 2D heights grid, find path from top-left to bottom-right that minimizes the maximum absolute difference in heights between consecutive cells.

### Sample Test Cases
```python
# Example 1
heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2  # Path: 1→3→5→3→5

# Example 2
heights = [[1,2,3],[3,8,4],[5,3,5]]
Output: 1  # Path: 1→2→3→4→5
```

### Approach: Modified Dijkstra

**Intuition**: Instead of summing distances, track the maximum difference seen so far. Use min-heap to always expand path with smallest current effort.

**Code**:
```python
import heapq

def minimumEffortPath(heights):
    n, m = len(heights), len(heights[0])
    difference = [[float('inf')] * m for _ in range(n)]
    difference[0][0] = 0
    
    pq = [(0, 0, 0)]  # (effort, row, col)
    directions = [(1,0), (-1,0), (0,-1), (0,1)]
    
    while pq:
        effort, r, c = heapq.heappop(pq)
        
        # If reached destination, return effort
        if (r, c) == (n-1, m-1):
            return effort
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            
            if 0 <= nr < n and 0 <= nc < m:
                # Calculate effort as max of current and height difference
                curr_diff = abs(heights[nr][nc] - heights[r][c])
                new_effort = max(effort, curr_diff)
                
                if new_effort < difference[nr][nc]:
                    difference[nr][nc] = new_effort
                    heapq.heappush(pq, (new_effort, nr, nc))
    
    return -1
```

**Time Complexity**: O(n × m × log(n × m))  
**Space Complexity**: O(n × m)

---

## 12. Bellman-Ford Algorithm

### Problem Description
Given weighted directed graph with V vertices and E edges, find shortest distances from source S to all vertices. Can handle negative edges and detect negative cycles.

### Sample Test Cases
```python
# Example 1
V = 6
Edges = [[3,2,6], [5,3,1], [0,1,5], [1,5,-3], [1,2,-2], [3,4,-2], [2,4,3]]
S = 0
Output: [0, 5, 3, 3, 1, 2]

# Example 2
V = 2
Edges = [[0,1,9]]
S = 0
Output: [0, 9]
```

### Approach: Edge Relaxation V-1 Times

**Intuition**: In graph with V vertices, longest simple path has at most V-1 edges. Relax all edges V-1 times to find shortest paths. If we can still improve after V-1 iterations, negative cycle exists.

**Code**:
```python
def bellman_ford(V, edges, S):
    # Step 1: Initialize distances
    distance = [float('inf')] * V
    distance[S] = 0
    
    # Step 2: Relax edges V-1 times
    for _ in range(V - 1):
        for source, dest, weight in edges:
            # Edge relaxation
            if distance[source] != float('inf') and distance[source] + weight < distance[dest]:
                distance[dest] = distance[source] + weight
    
    # Step 3: Check for negative cycles
    for source, dest, weight in edges:
        if distance[source] != float('inf') and distance[source] + weight < distance[dest]:
            return [-1]  # Negative cycle detected
    
    # Step 4: Handle unreachable vertices
    for i in range(V):
        if distance[i] == float('inf'):
            distance[i] = 10**9
    
    return distance
```

**Time Complexity**: O(V × E)  
**Space Complexity**: O(V)

---

## 13. Floyd-Warshall Algorithm

### Problem Description
Given weighted directed graph as adjacency matrix, find shortest distances between all pairs of vertices. Matrix[i][j] = -1 means no edge from i to j.

### Sample Test Cases
```python
# Example 1
matrix = [[0, 2, -1, -1],
          [1, 0, 3, -1],
          [-1, -1, 0, 1],
          [3, 5, 4, 0]]
Output = [[0, 2, 5, 6],
          [1, 0, 3, 4],
          [4, 6, 0, 1],
          [3, 5, 4, 0]]

# Example 2
matrix = [[0, 25],
          [-1, 0]]
Output = [[0, 25],
          [-1, 0]]
```

### Approach: Dynamic Programming with Intermediate Vertices

**Intuition**: For each intermediate vertex k, check if path i→k→j is shorter than direct path i→j. Try all possible intermediate vertices systematically.

**Code**:
```python
def shortest_distance(matrix):
    n = len(matrix)
    
    # Step 1: Replace -1 with infinity
    for i in range(n):
        for j in range(n):
            if matrix[i][j] == -1:
                matrix[i][j] = float('inf')
    
    # Step 2: Floyd-Warshall algorithm
    # Try each vertex k as intermediate vertex
    for k in range(n):
        for i in range(n):
            for j in range(n):
                # Check if path via k is shorter
                matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j])
    
    # Step 3: Convert infinity back to -1
    for i in range(n):
        for j in range(n):
            if matrix[i][j] == float('inf'):
                matrix[i][j] = -1
    
    return matrix
```

**Time Complexity**: O(V³)  
**Space Complexity**: O(1)

---

## 14. Find City with Smallest Number of Neighbors

### Problem Description
Given n cities with weighted edges and a distance threshold, find the city with smallest number of cities reachable within the threshold. If tie, return city with largest index.

### Sample Test Cases
```python
# Example 1
N = 4, M = 4
edges = [[0,1,3], [1,2,1], [1,3,4], [2,3,1]]
distanceThreshold = 4
Output: 3

# Example 2
N = 3, M = 2
edges = [[0,1,1], [0,2,3]]
distanceThreshold = 2
Output: 2
```

### Approach: Floyd-Warshall + Counting

**Intuition**: Use Floyd-Warshall to find all-pairs shortest distances. Then count neighbors within threshold for each city.

**Code**:
```python
def findCity(n, m, edges, distanceThreshold):
    # Step 1: Initialize distance matrix
    matrix = [[float('inf')] * n for _ in range(n)]
    
    # Set direct edges (bidirectional)
    for source, dest, weight in edges:
        matrix[source][dest] = weight
        matrix[dest][source] = weight
    
    # Distance from city to itself is 0
    for i in range(n):
        matrix[i][i] = 0
    
    # Step 2: Apply Floyd-Warshall
    for k in range(n):
        for i in range(n):
            for j in range(n):
                matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j])
    
    # Step 3: Count reachable neighbors for each city
    result_city = 0
    min_neighbors = float('inf')
    
    for i in range(n):
        neighbor_count = 0
        
        # Count cities reachable within threshold
        for j in range(n):
            if i != j and matrix[i][j] <= distanceThreshold:
                neighbor_count += 1
        
        # Update result (choose city with min neighbors, largest index in tie)
        if neighbor_count <= min_neighbors:
            min_neighbors = neighbor_count
            result_city = i
    
    return result_city
```

**Time Complexity**: O(N³)  
**Space Complexity**: O(N²)

---

## 15. A* Search Algorithm

### Problem Description
Given a grid with obstacles (0=free, 1=obstacle), find shortest path from start to end using A* algorithm with Manhattan distance heuristic.

### Sample Test Cases
```python
# Example 1
grid = [[0, 0, 0, 0, 0],
        [0, 1, 1, 0, 0],
        [0, 0, 0, 0, 0],
        [0, 1, 1, 1, 0],
        [0, 0, 0, 0, 0]]
start = (0, 0), end = (4, 4)
Output: Path with cost 8

# Example 2
grid = [[0, 0, 0],
        [0, 1, 0],
        [0, 0, 0]]
start = (0, 0), end = (2, 2)
Output: Path with cost 4
```

### Approach: Heuristic-Guided Search

**Intuition**: Combine actual distance (g-cost) with estimated remaining distance (h-cost). Always expand node with smallest f-cost = g-cost + h-cost.

**Code**:
```python
import heapq

def astar_search(grid, start, end):
    n, m = len(grid), len(grid[0])
    
    def heuristic(pos1, pos2):
        """Manhattan distance"""
        return abs(pos1[0] - pos2[0]) + abs(pos1[1] - pos2[1])
    
    def is_valid(row, col):
        return 0 <= row < n and 0 <= col < m and grid[row][col] == 0
    
    # Priority queue: (f_cost, g_cost, position)
    queue = [(0, 0, start)]
    in_queue = {start}
    visited = set()
    
    # Cost from start to each node
    g_cost = {start: 0}
    parent = {start: None}
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while queue:
        current_f, current_g, current_pos = heapq.heappop(queue)
        
        if current_pos in visited:
            continue
        
        visited.add(current_pos)
        in_queue.discard(current_pos)
        
        # Check if we reached goal
        if current_pos == end:
            # Reconstruct path
            path = []
            current = end
            while current is not None:
                path.append(current)
                current = parent[current]
            return path[::-1], current_g
        
        # Explore neighbors
        for dr, dc in directions:
            neighbor_pos = (current_pos[0] + dr, current_pos[1] + dc)
            
            if not is_valid(neighbor_pos[0], neighbor_pos[1]) or neighbor_pos in visited:
                continue
            
            tentative_g = current_g + 1
            
            if neighbor_pos not in g_cost or tentative_g < g_cost[neighbor_pos]:
                g_cost[neighbor_pos] = tentative_g
                parent[neighbor_pos] = current_pos
                
                if neighbor_pos not in in_queue:
                    h_cost = heuristic(neighbor_pos, end)
                    f_cost = tentative_g + h_cost
                    heapq.heappush(queue, (f_cost, tentative_g, neighbor_pos))
                    in_queue.add(neighbor_pos)
    
    return None
```

**Time Complexity**: O(b^d) where b=branching factor, d=depth  
**Space Complexity**: O(b^d)

---

## 16. Johnson's Algorithm

### Problem Description
Given weighted directed graph with possible negative edges, find all-pairs shortest paths efficiently. Combines Bellman-Ford and Dijkstra algorithms.

### Sample Test Cases
```python
# Example 1
V = 4
edges = [[0, 1, -5], [0, 2, 2], [0, 3, 3], [1, 2, 4], [2, 3, 1]]
Output: [[0, -5, -1, 0],
         [inf, 0, 4, 5],
         [inf, inf, 0, 1],
         [inf, inf, inf, 0]]

# Example 2 (Negative Cycle)
V = 3
edges = [[0, 1, 1], [1, 2, -3], [2, 0, 1]]
Output: "Negative cycle detected"
```

### Approach: Reweighting + Multiple Dijkstra

**Intuition**: Add auxiliary vertex connected to all vertices with weight 0. Use Bellman-Ford to find reweighting values. Reweight edges to make all non-negative. Run Dijkstra from each vertex. Convert back to original weights.

**Code**:
```python
import heapq
from collections import defaultdict

def johnsons_algorithm(edges, V):
    # Step 1: Build graph
    graph = defaultdict(list)
    for u, v, w in edges:
        graph[u].append((v, w))
    
    # Step 2: Add auxiliary vertex V connected to all with weight 0
    auxiliary = V
    for i in range(V):
        graph[auxiliary].append((i, 0))
    
    # Step 3: Run Bellman-Ford from auxiliary vertex
    def bellman_ford(src, V_extended):
        dist = [float('inf')] * V_extended
        dist[src] = 0
        
        # Relax edges V times (including auxiliary)
        for _ in range(V_extended - 1):
            for u in graph:
                if dist[u] != float('inf'):
                    for v, weight in graph[u]:
                        if dist[u] + weight < dist[v]:
                            dist[v] = dist[u] + weight
        
        # Check for negative cycles
        for u in graph:
            if dist[u] != float('inf'):
                for v, weight in graph[u]:
                    if dist[u] + weight < dist[v]:
                        return None  # Negative cycle
        return dist
    
    h = bellman_ford(auxiliary, V + 1)
    if h is None:
        return None  # Negative cycle detected
    
    # Step 4: Remove auxiliary vertex and reweight edges
    graph.clear()
    for u, v, w in edges:
        new_weight = w + h[u] - h[v]
        graph[u].append((v, new_weight))
    
    # Step 5: Run Dijkstra from each vertex
    def dijkstra(src, V):
        dist = [float('inf')] * V
        dist[src] = 0
        pq = [(0, src)]
        visited = set()
        
        while pq:
            d, u = heapq.heappop(pq)
            if u in visited:
                continue
            visited.add(u)
            
            for v, weight in graph[u]:
                if dist[u] + weight < dist[v]:
                    dist[v] = dist[u] + weight
                    heapq.heappush(pq, (dist[v], v))
        return dist
    
    result = []
    for src in range(V):
        reweighted_dist = dijkstra(src, V)
        
        # Convert back to original weights
        original_dist = []
        for dest in range(V):
            if reweighted_dist[dest] == float('inf'):
                original_dist.append(float('inf'))
            else:
                original_dist.append(reweighted_dist[dest] - h[src] + h[dest])
        
        result.append(original_dist)
    
    return result
```

**Time Complexity**: O(V² log V + VE)  
**Space Complexity**: O(V² + E)

---

## Comparison Table

| # | Problem | Algorithm | Graph Type | Time | Space | Key Feature |
|---|---------|-----------|------------|------|-------|-------------|
| 1 | Shortest Path in DAG | Topo Sort + DP | Weighted DAG | O(V+E) | O(V+E) | Process in topological order |
| 2 | Unweighted Graph | BFS | Unweighted | O(V+E) | O(V+E) | Level-wise exploration |
| 3 | Dijkstra's Algorithm | Priority Queue | Non-negative weights | O((V+E) log V) | O(V+E) | Greedy shortest path |
| 4 | Print Shortest Path | Modified Dijkstra | Non-negative weights | O((V+E) log V) | O(V+E) | Track parent pointers |
| 5 | Cheapest Flights K Stops | BFS with stops | Weighted with constraint | O(V+E×K) | O(V+E) | Stop-limited search |
| 6 | Number of Ways | Modified Dijkstra | Non-negative weights | O((V+E) log V) | O(V+E) | Count equal paths |
| 7 | Word Ladder I | BFS | Unweighted | O(M²×N) | O(N×M) | Word transformation |
| 8 | Word Ladder II | BFS + DFS | Unweighted | O(N×M²×26^L) | O(N×M+P×L) | All shortest paths |
| 9 | Min Multiplications | BFS | State space | O(100000×M) | O(100000) | Number transformation |
| 10 | Binary Maze | BFS | Grid (unweighted) | O(n×m) | O(n×m) | Grid pathfinding |
| 11 | Min Effort Path | Modified Dijkstra | Grid (weighted) | O(nm log(nm)) | O(n×m) | Minimize max edge |
| 12 | Bellman-Ford | Edge Relaxation | Any weights | O(V×E) | O(V) | Handles negative edges |
| 13 | Floyd-Warshall | DP | Any weights | O(V³) | O(1) | All-pairs shortest path |
| 14 | City with Min Neighbors | Floyd-Warshall | Any weights | O(V³) | O(V²) | Distance threshold |
| 15 | A* Search | Heuristic Search | Grid | O(b^d) | O(b^d) | Guided by heuristic |
| 16 | Johnson's Algorithm | Reweight + Dijkstra | Any weights | O(V² log V + VE) | O(V²+E) | Efficient all-pairs |

### Algorithm Selection Guide

**Single-Source Shortest Path:**
- **Unweighted graph** → BFS
- **Non-negative weights** → Dijkstra
- **Negative weights** → Bellman-Ford
- **DAG** → Topological Sort + DP

**All-Pairs Shortest Path:**
- **Small dense graph** → Floyd-Warshall
- **Large sparse graph** → Johnson's Algorithm
- **Non-negative weights** → Run Dijkstra V times

**Special Constraints:**
- **Path reconstruction** → Track parent pointers
- **Count paths** → Modified Dijkstra with counting
- **Limited steps/stops** → BFS with step tracking
- **Grid pathfinding** → BFS or A*
- **Minimize max edge** → Modified Dijkstra with max operation

### Key Patterns

**Pattern 1: Basic Shortest Path**
- Direct application of standard algorithms
- Choose based on graph properties

**Pattern 2: Modified Cost Function**
- Adapt relaxation step for custom cost
- Example: max instead of sum (min effort)

**Pattern 3: Constrained Paths**
- Add constraint to state space
- Example: stops count, threshold distance

**Pattern 4: Path Counting/Reconstruction**
- Track additional information during search
- Example: parent pointers, path counts

**Pattern 5: State Space Search**
- Transform problem into graph
- Example: word transformations, number operations
