# Shortest Path in DAG

## Problem Description
Given a Directed Acyclic Graph (DAG) with N vertices (numbered 0 to N-1) and M edges, find the shortest path from source vertex 0 to all other vertices. Each edge has a weight/distance. If any vertex cannot be reached from the source, return -1 for that vertex.

Think of it like finding the shortest route from your home (vertex 0) to all other places in a city where roads are one-way and there are no circular routes.

## Examples

### Input
```
N = 4, M = 2
edges = [[0,1,2], [0,2,1]]
```
<img src="https://static.takeuforward.org/content/ProblemSetter-yfJ4sh6m" />

### Output
```
[0, 2, 1, -1]
```

### Explanation
- Distance to vertex 0 (source): 0 (starting point)
- Distance to vertex 1: 2 (direct path 0→1 with weight 2)
- Distance to vertex 2: 1 (direct path 0→2 with weight 1)
- Distance to vertex 3: -1 (no path exists from 0 to 3)

### Input
```
N = 6, M = 7
edges = [[0,1,2], [0,4,1], [4,5,4], [4,2,2], [1,2,3], [2,3,6], [5,3,1]]
```

### Output
```
[0, 2, 3, 6, 1, 5]
```

### Explanation
- Distance to vertex 0: 0 (source)
- Distance to vertex 1: 2 (path 0→1, weight 2)
- Distance to vertex 2: 3 (path 0→4→2, weight 1+2=3, shorter than 0→1→2 which is 2+3=5)
- Distance to vertex 3: 6 (path 0→4→5→3, weight 1+4+1=6)
- Distance to vertex 4: 1 (path 0→4, weight 1)
- Distance to vertex 5: 5 (path 0→4→5, weight 1+4=5)

## Solution
Use topological sorting to process vertices in the correct order, then relax edges to find shortest distances. Since it's a DAG, we can process vertices in topological order to ensure we always have the shortest path to a vertex before processing its neighbors.

## Intuition
The key insight is that in a DAG, if we process vertices in topological order, by the time we reach any vertex, we've already found the shortest paths to all vertices that can reach it. This is like solving a puzzle where you need to complete certain pieces before others - topological order tells us the correct sequence.

Since there are no cycles, once we find the shortest path to a vertex, it's guaranteed to be optimal. We don't need to worry about finding a better path later (unlike in graphs with cycles).

## Approach Steps

1. **Build the graph**: Create an adjacency list representation from the edge list
2. **Topological sorting**: Use DFS to get the topological order of vertices
3. **Initialize distances**: Set distance to source as 0, all others as infinity
4. **Process vertices**: For each vertex in topological order:
   - If the vertex is reachable (distance < infinity)
   - Update distances to all its neighbors using edge relaxation
5. **Handle unreachable vertices**: Set distance of unreachable vertices to -1
6. **Return result**: Return the distance array

## Code

### Method 1: Topological Sort with DFS
```python
from collections import defaultdict

def shortestPath(N, M, edges):
    # Build adjacency list
    graph = defaultdict(list)
    for u, v, weight in edges:
        graph[u].append((v, weight))
    
    # Topological sort using DFS
    def topological_sort():
        visited = [False] * N
        stack = []
        
        def dfs(node):
            visited[node] = True
            for neighbor, _ in graph[node]:
                if not visited[neighbor]:
                    dfs(neighbor)
            stack.append(node)
        
        for i in range(N):
            if not visited[i]:
                dfs(i)
        
        return stack[::-1]  # Reverse to get correct topological order
    
    # Get topological order
    topo_order = topological_sort()
    
    # Initialize distances
    dist = [float('inf')] * N
    dist[0] = 0  # Source vertex distance is 0
    
    # Process vertices in topological order
    for node in topo_order:
        if dist[node] != float('inf'):  # If node is reachable
            for neighbor, weight in graph[node]:
                # Relax the edge
                if dist[node] + weight < dist[neighbor]:
                    dist[neighbor] = dist[node] + weight
    
    # Convert unreachable vertices distance to -1
    for i in range(N):
        if dist[i] == float('inf'):
            dist[i] = -1
    
    return dist
```

### Method 2: Kahn's Algorithm for Topological Sort
```python
from collections import defaultdict, deque

def shortestPathKahn(N, M, edges):
    # Build adjacency list and calculate in-degrees
    graph = defaultdict(list)
    in_degree = [0] * N
    
    for u, v, weight in edges:
        graph[u].append((v, weight))
        in_degree[v] += 1
    
    # Topological sort using Kahn's algorithm
    queue = deque()
    for i in range(N):
        if in_degree[i] == 0:
            queue.append(i)
    
    topo_order = []
    while queue:
        node = queue.popleft()
        topo_order.append(node)
        
        for neighbor, _ in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    # Initialize distances and process
    dist = [float('inf')] * N
    dist[0] = 0
    
    for node in topo_order:
        if dist[node] != float('inf'):
            for neighbor, weight in graph[node]:
                if dist[node] + weight < dist[neighbor]:
                    dist[neighbor] = dist[node] + weight
    
    # Convert unreachable to -1
    for i in range(N):
        if dist[i] == float('inf'):
            dist[i] = -1
    
    return dist
```

### Method 3: BFS-like Approach (Alternative Implementation)
```python
from collections import deque

def solution(nodes, graph, source):
    # Initialize distance array
    distance = {i: float('inf') for i in nodes}
    distance[source] = 0
    
    # Use queue for BFS-like traversal
    queue = deque()
    queue.append(source)
    
    while queue:
        node = queue.popleft()
        # Process all adjacent nodes
        for adj_node in graph[node].keys():
            # Relax the edge if shorter path found
            if distance[node] + graph[node][adj_node] < distance[adj_node]:
                distance[adj_node] = distance[node] + graph[node][adj_node]
                queue.append(adj_node)
    
    # Convert unreachable nodes to -1
    for node, dist in distance.items():
        if dist == float('inf'):
            distance[node] = -1
    
    return list(distance.values())

class Solution:
    def shortestPath(self, N, M, edges):
        # Create nodes list and adjacency list representation
        nodes = [i for i in range(N)]
        graph = {i: {} for i in nodes}
        
        # Build the graph
        for i, j, v in edges:
            graph[i][j] = v
        
        return solution(nodes, graph, 0)
```

**Note**: Method 3 uses a BFS-like approach but may not work correctly for all DAG cases as it doesn't guarantee topological processing order. Methods 1 and 2 are recommended for reliable results.

## Time and Space Complexity

**Time Complexity: O(N + M)**
- Topological sorting takes O(N + M) time where we visit each vertex once and traverse each edge once
- Processing vertices in topological order and relaxing edges takes O(M) time
- Overall: O(N + M)

**Space Complexity: O(N + M)**
- Adjacency list representation takes O(M) space to store all edges
- Topological sorting requires O(N) space for visited array and recursion stack (DFS) or queue (Kahn's)
- Distance array takes O(N) space
- Overall: O(N + M)

The algorithm is very efficient because we only visit each vertex and edge once, making it much faster than general shortest path algorithms like Dijkstra's for DAGs.
