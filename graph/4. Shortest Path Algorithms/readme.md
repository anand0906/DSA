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

---

# Shortest Path in Undirected Graph with Unit Weights

## Problem Description
Given an undirected graph with N vertices (numbered 0 to N-1) and M edges where all edges have unit weight (weight = 1), find the shortest path from source vertex 0 to all other vertices. If any vertex is unreachable from the source, return -1 for that vertex.

Think of it like finding the minimum number of steps needed to reach every location from your starting point, where you can move in any direction along connected paths.

## Examples

### Input
```
n = 9, m = 10
edges = [[0,1],[0,3],[3,4],[4,5],[5,6],[1,2],[2,6],[6,7],[7,8],[6,8]]
```

### Output
```
[0, 1, 2, 1, 2, 3, 3, 4, 4]
```

### Explanation
Starting from vertex 0:
- Distance to vertex 0: 0 (source)
- Distance to vertex 1: 1 (direct connection: 0→1)
- Distance to vertex 2: 2 (path: 0→1→2)
- Distance to vertex 3: 1 (direct connection: 0→3)
- Distance to vertex 4: 2 (path: 0→3→4)
- Distance to vertex 5: 3 (path: 0→3→4→5)
- Distance to vertex 6: 3 (shortest path: 0→1→2→6 or 0→3→4→5→6)
- Distance to vertex 7: 4 (path: 0→1→2→6→7)
- Distance to vertex 8: 4 (path: 0→1→2→6→8)

### Input
```
n = 8, m = 10
edges = [[1,0],[2,1],[0,3],[3,7],[3,4],[7,4],[7,6],[4,5],[4,6],[6,5]]
```

### Output
```
[0, 1, 2, 1, 2, 3, 3, 2]
```

### Explanation
- Distance to vertex 7: 2 (path: 0→3→7, shorter than going through other nodes)
- All distances represent the minimum number of edges needed to reach each vertex from source 0

## Solution
Use BFS (Breadth-First Search) traversal starting from the source vertex. Since all edges have unit weight, BFS naturally finds the shortest path because it explores nodes level by level, ensuring the first time a node is reached, it's via the shortest path.

## Intuition
The key insight is that BFS explores nodes level by level. In an unweighted graph (or unit weight graph), this means:
- Level 0: Source node (distance 0)
- Level 1: All nodes directly connected to source (distance 1)
- Level 2: All nodes reachable in 2 steps (distance 2)
- And so on...

Since BFS visits nodes in increasing order of their distance from source, the first time we reach any node is guaranteed to be via the shortest path. This is much simpler than using Dijkstra's algorithm for weighted graphs.

## Approach Steps

1. **Build the graph**: Create an adjacency list for the undirected graph (add edges in both directions)
2. **Initialize data structures**: 
   - Distance array with infinity for all nodes except source (set to 0)
   - Queue for BFS traversal, starting with source node
3. **BFS traversal**: 
   - Process each node from queue
   - For each unvisited neighbor, update its distance and add to queue
4. **Handle unreachable nodes**: Convert any remaining infinity distances to -1
5. **Return result**: Return the distance array

## Code

### Method 1: Standard BFS with Visited Array
```python
from collections import deque

def solution(nodes, graph, source):
    # Initialize visited and distance arrays
    visited = {i: False for i in nodes}
    distance = {i: float('inf') for i in nodes}
    
    # Set source distance and mark as visited
    distance[source] = 0
    visited[source] = True
    
    # BFS using queue
    queue = deque()
    queue.append(source)
    
    while queue:
        node = queue.popleft()
        
        # Process all adjacent nodes
        for adj_node in graph[node]:
            if not visited[adj_node]:
                # Update distance (always current distance + 1 for unit weights)
                distance[adj_node] = distance[node] + 1
                visited[adj_node] = True
                queue.append(adj_node)
    
    # Convert unreachable nodes to -1
    for node, dist in distance.items():
        if dist == float('inf'):
            distance[node] = -1
    
    return list(distance.values())

class Solution:
    def shortestPath(self, edges, N, M):
        # Create nodes and adjacency list
        nodes = [i for i in range(N)]
        graph = {i: set() for i in nodes}
        
        # Build undirected graph (add edge in both directions)
        for i, j in edges:
            graph[i].add(j)
            graph[j].add(i)
        
        return solution(nodes, graph, 0)
```

### Method 2: Optimized BFS (Distance Array as Visited Array)
```python
from collections import deque

def shortestPathOptimized(N, M, edges):
    # Build adjacency list
    graph = [[] for _ in range(N)]
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
    
    # Initialize distance array
    distance = [float('inf')] * N
    distance[0] = 0  # Source distance is 0
    
    # BFS traversal
    queue = deque([0])
    
    while queue:
        node = queue.popleft()
        
        for neighbor in graph[node]:
            # If not visited (distance is still infinity)
            if distance[neighbor] == float('inf'):
                distance[neighbor] = distance[node] + 1
                queue.append(neighbor)
    
    # Convert unreachable nodes to -1
    for i in range(N):
        if distance[i] == float('inf'):
            distance[i] = -1
    
    return distance
```

### Method 3: Clean Implementation
```python
from collections import deque

def shortestPath(N, M, edges):
    # Create adjacency list
    adj = [[] for _ in range(N)]
    for u, v in edges:
        adj[u].append(v)
        adj[v].append(v)
    
    # BFS from source (node 0)
    dist = [-1] * N  # Initialize with -1 (unreachable)
    dist[0] = 0      # Source distance is 0
    queue = deque([0])
    
    while queue:
        current = queue.popleft()
        
        for neighbor in adj[current]:
            if dist[neighbor] == -1:  # Not visited
                dist[neighbor] = dist[current] + 1
                queue.append(neighbor)
    
    return dist
```

## Time and Space Complexity

**Time Complexity: O(N + M)**
- We visit each vertex exactly once: O(N)
- We examine each edge exactly twice (once from each endpoint): O(M)
- Overall: O(N + M) where N is vertices and M is edges

**Space Complexity: O(N + M)**
- Adjacency list representation takes O(M) space to store all edges
- Distance array takes O(N) space
- BFS queue can contain at most O(N) vertices in worst case
- Overall: O(N + M)

**Key Advantages:**
- Much simpler than Dijkstra's algorithm for unweighted graphs
- Optimal time complexity for this problem
- BFS guarantees shortest path for unit weight graphs
- No need for priority queue unlike Dijkstra's algorithm

---

# Dijkstra's Algorithm

## Problem Description
Given a weighted, undirected graph with V vertices (numbered 0 to V-1) and a source node S, find the shortest distance from the source vertex to all other vertices. If a vertex is not reachable from the source, return 10^9 for that vertex.

Think of it like finding the shortest route (by total distance/cost) from your home to all other locations in a city, where roads have different lengths/costs.

## Examples

### Input
```
V = 2, adj = [[[1, 9]], [[0, 9]]], S = 0
```
<img src="https://static.takeuforward.org/content/ProblemSetter-_DG48OHc" />

### Output
```
[0, 9]
```

### Explanation
- Distance from source 0 to itself: 0
- Distance from source 0 to vertex 1: 9 (direct path with weight 9)

### Input
```
V = 3, adj = [[[1, 1], [2, 6]], [[2, 3], [0, 1]], [[1, 3], [0, 6]]], S = 2
```
<img src="https://static.takeuforward.org/content/ProblemSetter-GlKv5O6L" />

### Output
```
[4, 3, 0]
```

### Explanation
Starting from source vertex 2:
- Distance to vertex 0: 4 (path: 2→1→0 with weights 3+1=4, shorter than direct path 2→0 with weight 6)
- Distance to vertex 1: 3 (direct path: 2→1 with weight 3)
- Distance to vertex 2: 0 (source itself)

## Solution
Use Dijkstra's algorithm with a priority queue (min-heap) to always process the node with the smallest known distance first. This greedy approach ensures we find the optimal shortest path to all vertices.

## Intuition
Dijkstra's algorithm works on the principle that once we've found the shortest path to a vertex, we can use that vertex to potentially find shorter paths to other vertices. It's like solving a puzzle piece by piece - once we're certain about the shortest distance to a vertex, we can use it as a stepping stone to reach other vertices.

The key insight is to always process the vertex with the smallest tentative distance first. This ensures that when we process a vertex, we've already found its shortest path from the source.

**Why does this work?**
- When we extract a vertex with minimum distance from the priority queue, that distance is guaranteed to be the shortest possible
- Any alternative path to this vertex would have to go through other vertices with larger distances, making the total path longer

## Approach Steps

1. **Initialize data structures**: 
   - Distance array with infinity for all nodes except source (set to 0)
   - Priority queue (min-heap) to store {distance, node} pairs
2. **Add source to queue**: Push {0, source} to priority queue
3. **Main algorithm loop**:
   - Extract node with minimum distance from queue
   - For each neighbor, perform edge relaxation:
     - If current_distance + edge_weight < neighbor_distance, update it
     - Push the updated {new_distance, neighbor} to queue
4. **Continue until queue is empty**: All reachable nodes will have optimal distances
5. **Return result**: Distance array contains shortest paths from source

## Code

### Method 1: Using Min-Heap (Provided Implementation)
```python
import heapq

def solution(nodes, graph, source):
    # Initialize priority queue and distance array
    queue = []
    heapq.heappush(queue, (0, source))
    distance = {i: 10**9 for i in nodes}
    distance[source] = 0
    
    while queue:
        dist, node = heapq.heappop(queue)
        
        # Skip if we've already found a better path
        if dist > distance[node]:
            continue
            
        # Process all neighbors
        for adj_node, adj_weight in graph[node].items():
            if distance[node] + adj_weight < distance[adj_node]:
                distance[adj_node] = distance[node] + adj_weight
                heapq.heappush(queue, (distance[adj_node], adj_node))
    
    return list(distance.values())

class Solution:
    def dijkstra(self, V, adj, S):
        # Convert adjacency list to dictionary format
        graph = {}
        for i, v in enumerate(adj):
            graph[i] = dict(v)
        
        nodes = [i for i in range(V)]
        ans = solution(nodes, graph, S)
        return ans
```

### Method 2: Clean Implementation with Standard Format
```python
import heapq

def dijkstra(V, adj, source):
    # Initialize distance array
    dist = [10**9] * V
    dist[source] = 0
    
    # Priority queue: (distance, node)
    pq = [(0, source)]
    
    while pq:
        current_dist, u = heapq.heappop(pq)
        
        # Skip if we've already processed this node with better distance
        if current_dist > dist[u]:
            continue
        
        # Process all neighbors
        for neighbor, weight in adj[u]:
            new_dist = dist[u] + weight
            
            # If we found a shorter path
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    
    return dist

class Solution:
    def dijkstra(self, V, adj, S):
        return dijkstra(V, adj, S)
```

### Method 3: With Visited Set Optimization
```python
import heapq

def dijkstraOptimized(V, adj, source):
    # Initialize distance and visited arrays
    dist = [10**9] * V
    dist[source] = 0
    visited = [False] * V
    
    # Priority queue
    pq = [(0, source)]
    
    while pq:
        current_dist, u = heapq.heappop(pq)
        
        # Skip if already processed
        if visited[u]:
            continue
            
        visited[u] = True
        
        # Process neighbors
        for neighbor, weight in adj[u]:
            if not visited[neighbor]:
                new_dist = dist[u] + weight
                if new_dist < dist[neighbor]:
                    dist[neighbor] = new_dist
                    heapq.heappush(pq, (new_dist, neighbor))
    
    return dist
```

## Time and Space Complexity

**Time Complexity: O((V + E) log V)**
- Each vertex is extracted from the priority queue at most once: O(V log V)
- Each edge is relaxed at most once, and each relaxation involves a heap operation: O(E log V)
- Overall: O((V + E) log V) where V is vertices and E is edges

**Space Complexity: O(V + E)**
- Adjacency list representation takes O(E) space
- Distance array takes O(V) space  
- Priority queue can contain at most O(E) elements in worst case
- Overall: O(V + E)

**Important Notes:**
- **Works only with non-negative edge weights** - negative weights can break the greedy property
- **Handles both directed and undirected graphs** - just ensure adjacency list is built correctly
- **More efficient than Bellman-Ford** for graphs without negative edges
- **Priority queue ensures optimal substructure** - always processes minimum distance node first

**When to use Dijkstra's:**
- Single-source shortest path with non-negative weights
- Need shortest path to all vertices from one source
- Graph is dense (many edges) - better than multiple BFS calls
- Real-world applications: GPS navigation, network routing, social networks

---

# Print Shortest Path

## Problem Description
Given a weighted undirected graph with n vertices (numbered 1 to n) and m edges, find the shortest path between vertex 1 and vertex n. If no path exists, return [-1]. If a path exists, return a list where the first element is the total weight of the shortest path, followed by the vertices in the shortest path from 1 to n.

Think of it like finding the cheapest route from city 1 to city n, and not just knowing the cost, but also remembering which cities you passed through.

## Examples

### Input
```
n = 5, m = 6
edges = [[1,2,2], [2,5,5], [2,3,4], [1,4,1], [4,3,3], [3,5,1]]
```
<img src="https://static.takeuforward.org/content/ProblemSetter-Bqkfe0u6" />

### Output
```
[5, 1, 4, 3, 5]
```

### Explanation
- Shortest path from 1 to 5 has total weight 5
- Path: 1→4 (weight 1) → 3 (weight 3) → 5 (weight 1)
- Total: 1 + 3 + 1 = 5
- Alternative path 1→2→5 would cost 2 + 5 = 7 (more expensive)

### Input
```
n = 4, m = 4
edges = [[1,2,2], [2,3,4], [1,4,1], [4,3,3]]
```
<img src="https://static.takeuforward.org/content/ProblemSetter-Jhz16YEs" />
### Output
```
[1, 1, 4]
```

### Explanation
- Shortest path from 1 to 4 has total weight 1
- Path: 1→4 (direct connection with weight 1)
- Alternative path 1→2→3→4 would cost 2 + 4 + 3 = 9 (much more expensive)

## Solution
Use a modified Dijkstra's algorithm that not only finds shortest distances but also tracks the parent of each vertex to reconstruct the shortest path. After finding shortest distances, trace back from destination to source using parent information.

## Intuition
Regular Dijkstra's algorithm finds the shortest distance to all vertices, but doesn't remember the actual path. To print the shortest path, we need to track how we reached each vertex optimally.

The key modification is maintaining a parent array alongside the distance array. When we relax an edge and find a shorter path to a vertex, we not only update its distance but also record which vertex we came from (its parent in the shortest path tree).

**Path Reconstruction Logic:**
- Start from the destination vertex
- Keep following parent pointers until we reach the source
- Reverse this path to get source→destination order

## Approach Steps

1. **Initialize data structures**: 
   - Distance array with infinity for all nodes except source (set to 0)
   - Parent array to track the shortest path tree
   - Priority queue for Dijkstra's algorithm
2. **Run modified Dijkstra's**:
   - Process vertices in order of shortest distance
   - When relaxing edges, update both distance and parent information
3. **Check if destination is reachable**: If distance to destination is still infinity, return [-1]
4. **Reconstruct path**: Trace back from destination to source using parent array
5. **Format result**: Return [total_weight, vertex1, vertex2, ..., destination]

## Code

### Method 1: Modified Implementation (Based on Provided Code)
```python
import heapq

def solution(nodes, graph, source, destination):
    # Initialize data structures
    queue = []
    distance = {i: float('inf') for i in nodes}
    parent = {i: None for i in nodes}
    
    # Set source distance and add to queue
    distance[source] = 0
    heapq.heappush(queue, (0, source))
    
    while queue:
        dist, node = heapq.heappop(queue)
        
        # Skip if we've found better path already
        if dist > distance[node]:
            continue
        
        # Process all neighbors
        for adj_node, adj_weight in graph[node].items():
            new_dist = dist + adj_weight
            if new_dist < distance[adj_node]:
                distance[adj_node] = new_dist
                parent[adj_node] = node
                heapq.heappush(queue, (new_dist, adj_node))
    
    # Check if destination is reachable
    if distance[destination] == float('inf'):
        return [-1]
    
    # Reconstruct path from destination to source
    path = []
    current = destination
    while current is not None:
        path.append(current)
        current = parent[current]
    
    # Reverse to get source to destination path
    path.reverse()
    
    # Return [total_weight, path...]
    return [distance[destination]] + path

class Solution:
    def shortestPath(self, n, m, edges):
        # Create nodes and graph
        nodes = [i for i in range(1, n + 1)]
        graph = {i: {} for i in nodes}
        
        # Build undirected graph
        for u, v, weight in edges:
            graph[u][v] = weight
            graph[v][u] = weight
        
        return solution(nodes, graph, 1, n)
```

### Method 2: Clean Standard Implementation
```python
import heapq

def printShortestPath(n, m, edges):
    # Build adjacency list
    graph = [[] for _ in range(n + 1)]
    for u, v, weight in edges:
        graph[u].append((v, weight))
        graph[v].append((u, weight))
    
    # Dijkstra's with path tracking
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
                parent[v] = u
                heapq.heappush(pq, (new_dist, v))
    
    # Check if destination is reachable
    if dist[n] == float('inf'):
        return [-1]
    
    # Reconstruct path
    path = []
    current = n
    while current != -1:
        path.append(current)
        current = parent[current]
    
    path.reverse()
    return [dist[n]] + path

class Solution:
    def shortestPath(self, n, m, edges):
        return printShortestPath(n, m, edges)
```

### Method 3: With Early Termination Optimization
```python
import heapq

def shortestPathOptimized(n, m, edges):
    # Build graph
    graph = [[] for _ in range(n + 1)]
    for u, v, weight in edges:
        graph[u].append((v, weight))
        graph[v].append((u, weight))
    
    # Dijkstra's algorithm
    dist = [float('inf')] * (n + 1)
    parent = [-1] * (n + 1)
    visited = [False] * (n + 1)
    
    dist[1] = 0
    pq = [(0, 1)]
    
    while pq:
        d, u = heapq.heappop(pq)
        
        if visited[u]:
            continue
            
        visited[u] = True
        
        # Early termination if we reached destination
        if u == n:
            break
        
        for v, weight in graph[u]:
            if not visited[v]:
                new_dist = dist[u] + weight
                if new_dist < dist[v]:
                    dist[v] = new_dist
                    parent[v] = u
                    heapq.heappush(pq, (new_dist, v))
    
    # Check reachability and reconstruct path
    if dist[n] == float('inf'):
        return [-1]
    
    path = []
    current = n
    while current != -1:
        path.append(current)
        current = parent[current]
    
    path.reverse()
    return [dist[n]] + path
```

## Time and Space Complexity

**Time Complexity: O((V + E) log V)**
- Same as standard Dijkstra's algorithm
- Path reconstruction takes O(V) additional time
- Overall: O((V + E) log V) where V is vertices and E is edges

**Space Complexity: O(V + E)**
- Adjacency list: O(E) space
- Distance and parent arrays: O(V) space each
- Priority queue: O(E) space in worst case
- Path reconstruction: O(V) space for storing path
- Overall: O(V + E)

**Key Differences from Standard Dijkstra's:**
- **Parent tracking**: Additional parent array to remember shortest path tree
- **Path reconstruction**: Trace back from destination to source using parent pointers
- **Output format**: Return both distance and actual path vertices
- **Early termination**: Can stop once destination is reached (optimization)

**Important Notes:**
- **Works only with non-negative weights** like standard Dijkstra's
- **Path uniqueness**: Multiple shortest paths may exist, algorithm returns one of them
- **1-indexed vertices**: Problem uses 1-based indexing instead of 0-based
- **Undirected graph**: Each edge is added in both directions

---

# Cheapest Flights within K Stops

## Problem Description
Given n cities connected by flights, find the cheapest price to travel from source city to destination city with at most k stops. Each flight has a cost, and you need to minimize the total cost while respecting the maximum number of stops allowed. If no valid route exists, return -1.

Think of it like booking the cheapest flight ticket with a layover limit - you want the lowest price but can't have too many connecting flights.

## Examples

### Input
```
n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
```

<img src="https://static.takeuforward.org/content/ProblemSetter-lTPXKKMA" />

### Output
```
700
```

### Explanation
- Path 0→1→3: Cost = 100 + 600 = 700 (1 stop, valid)
- Path 0→1→2→3: Cost = 100 + 100 + 200 = 400 (2 stops, invalid as k=1)
- The cheaper path is invalid due to stop limit, so answer is 700

### Input
```
n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
```

<img src="https://static.takeuforward.org/content/ProblemSetter-CIPwuM01" />

### Output
```
200
```

### Explanation
- Direct path 0→2: Cost = 500 (0 stops)
- Path with stop 0→1→2: Cost = 100 + 100 = 200 (1 stop)
- The path with 1 stop is cheaper: 200

## Solution
Use a modified Dijkstra's algorithm that prioritizes by number of stops instead of distance. Since stops increase monotonically, we can use a simple queue instead of a priority queue for better time complexity.

## Intuition
This problem combines two constraints: minimizing cost and limiting stops. Regular Dijkstra's won't work directly because:

1. **Standard Dijkstra's prioritizes minimum distance** - but a longer distance path might have fewer stops
2. **We need to track both cost and stops** - not just the minimum cost

**Key insight**: Since stops increase by exactly 1 at each level, we can use BFS-like traversal where we process paths level by level based on number of stops. This ensures we don't exceed the stop limit.

**Why prioritize stops over cost?**
- If we prioritize cost first, we might get stuck with expensive paths that have fewer stops
- By processing paths with fewer stops first, we ensure we don't miss valid solutions due to exceeding the stop limit

## Approach Steps

1. **Build the graph**: Create adjacency list representation from flights
2. **Initialize data structures**:
   - Distance array to track minimum cost to reach each city
   - Queue to store (stops, city, cost) tuples
3. **BFS-like traversal**:
   - Start from source with 0 stops and 0 cost
   - For each city, explore all neighboring cities
   - Only proceed if stops ≤ k and we found a cheaper path
4. **Update distances**: Keep track of minimum cost to reach each city
5. **Return result**: Return minimum cost to destination, or -1 if unreachable

## Code

### Method 1: Modified Dijkstra's (Based on Provided Code)
```python
import heapq

def solution(nodes, graph, source, destination, k):
    # Use priority queue: (stops, node, cost)
    queue = []
    distance = {i: float('inf') for i in nodes}
    
    # Start from source with 0 stops and 0 cost
    heapq.heappush(queue, (0, source, 0))
    distance[source] = 0
    
    while queue:
        steps, node, cost = heapq.heappop(queue)
        
        # Skip if exceeded stop limit
        if steps > k:
            continue
        
        # Process all neighbors
        for adj_node, flight_cost in graph[node].items():
            new_cost = cost + flight_cost
            
            # If we found a cheaper path and within stop limit
            if new_cost < distance[adj_node]:
                distance[adj_node] = new_cost
                # Add to queue with incremented stops
                heapq.heappush(queue, (steps + 1, adj_node, new_cost))
    
    return distance[destination] if distance[destination] != float('inf') else -1

class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        nodes = [i for i in range(n)]
        graph = {i: {} for i in nodes}
        
        # Build directed graph
        for u, v, cost in flights:
            graph[u][v] = cost
        
        return solution(nodes, graph, src, dst, k)
```

### Method 2: BFS with Queue (More Efficient)
```python
from collections import deque

def findCheapestPriceBFS(n, flights, src, dst, k):
    # Build adjacency list
    graph = [[] for _ in range(n)]
    for u, v, cost in flights:
        graph[u].append((v, cost))
    
    # Use simple queue since stops increase monotonically
    queue = deque([(src, 0, 0)])  # (city, cost, stops)
    min_cost = [float('inf')] * n
    min_cost[src] = 0
    
    while queue:
        city, cost, stops = queue.popleft()
        
        # Skip if exceeded stop limit
        if stops > k:
            continue
        
        # Process all neighbors
        for next_city, flight_cost in graph[city]:
            new_cost = cost + flight_cost
            
            # Only proceed if we found a cheaper path
            if new_cost < min_cost[next_city]:
                min_cost[next_city] = new_cost
                queue.append((next_city, new_cost, stops + 1))
    
    return min_cost[dst] if min_cost[dst] != float('inf') else -1
```

### Method 3: Bellman-Ford Variation (Alternative Approach)
```python
def findCheapestPriceBellmanFord(n, flights, src, dst, k):
    # Initialize distance array
    dist = [float('inf')] * n
    dist[src] = 0
    
    # Relax edges at most k+1 times (k stops = k+1 edges)
    for i in range(k + 1):
        temp = dist.copy()  # Use previous iteration's distances
        
        for u, v, cost in flights:
            if dist[u] != float('inf'):
                temp[v] = min(temp[v], dist[u] + cost)
        
        dist = temp
    
    return dist[dst] if dist[dst] != float('inf') else -1
```

### Method 4: DFS with Memoization (For Learning)
```python
def findCheapestPriceDFS(n, flights, src, dst, k):
    # Build adjacency list
    graph = [[] for _ in range(n)]
    for u, v, cost in flights:
        graph[u].append((v, cost))
    
    # Memoization: (city, stops_left) -> min_cost
    memo = {}
    
    def dfs(city, stops_left):
        if city == dst:
            return 0
        if stops_left < 0:
            return float('inf')
        if (city, stops_left) in memo:
            return memo[(city, stops_left)]
        
        min_cost = float('inf')
        for next_city, flight_cost in graph[city]:
            cost = flight_cost + dfs(next_city, stops_left - 1)
            min_cost = min(min_cost, cost)
        
        memo[(city, stops_left)] = min_cost
        return min_cost
    
    result = dfs(src, k + 1)  # k stops = k+1 edges
    return result if result != float('inf') else -1
```

## Time and Space Complexity

### Method 1 & 2 (BFS/Modified Dijkstra's):
**Time Complexity: O(V + E × K)**
- Each city can be visited multiple times (up to k+1 times)
- Each edge is processed at most k+1 times
- V is number of cities, E is number of flights

**Space Complexity: O(V + E)**
- Adjacency list: O(E) space
- Queue and distance array: O(V) space

### Method 3 (Bellman-Ford):
**Time Complexity: O(K × E)**
- K+1 iterations of edge relaxation
- Each iteration processes all E flights

**Space Complexity: O(V)**
- Only distance arrays needed

## Key Points to Remember

**Why not standard Dijkstra's?**
- Standard Dijkstra's prioritizes minimum distance
- Here we need to balance cost vs. number of stops
- A cheaper path might exceed the stop limit

**Why BFS works better here?**
- Stops increase monotonically (always +1)
- No need for priority queue's logarithmic operations
- Simpler and more efficient for this specific problem

**Edge Cases:**
- Direct flight exists (0 stops)
- No path exists within k stops
- k = 0 (only direct flights allowed)
- Source equals destination

**Real-world Applications:**
- Flight booking systems with layover limits
- Network routing with hop count restrictions
- Supply chain optimization with intermediate stops

---

# Number of Ways to Arrive at Destination

## Problem Description
Given a city with n intersections (numbered 0 to n-1) connected by bi-directional roads, find the number of ways to travel from intersection 0 to intersection n-1 in the shortest possible time. Each road has a travel time, and you need to count all different shortest paths. Return the answer modulo 10^9 + 7.

Think of it like finding all the fastest routes on GPS - there might be multiple different paths that all take the same minimum time.

## Examples

### Input
```
n = 7, m = 10
roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
```

### Output
```
4
```

### Explanation
Shortest time from 0 to 6 is 7 minutes. The four ways to achieve this are:
- Path 1: 0→6 (time: 7)
- Path 2: 0→4→6 (time: 5+2 = 7)  
- Path 3: 0→1→2→5→6 (time: 2+3+1+1 = 7)
- Path 4: 0→1→3→5→6 (time: 2+3+1+1 = 7)

### Input
```
n = 6, m = 8
roads = [[0,5,8],[0,2,2],[0,1,1],[1,3,3],[1,2,3],[2,5,6],[3,4,2],[4,5,2]]
```

### Output
```
3
```

### Explanation
Shortest time from 0 to 5 is 8 minutes. The three ways are:
- Path 1: 0→5 (time: 8)
- Path 2: 0→2→5 (time: 2+6 = 8)
- Path 3: 0→1→3→4→5 (time: 1+3+2+2 = 8)

## Solution
Use a modified Dijkstra's algorithm that tracks both the shortest distance and the number of ways to achieve that shortest distance to each node. When we find a path of equal length, we add the ways; when we find a shorter path, we replace the ways.

## Intuition
This problem combines shortest path finding with path counting. The key insight is:

**Number of ways to reach a node in shortest time = Sum of ways to reach all parent nodes that can lead to this node in shortest time**

For example, if we can reach node C in shortest time from both node A and node B:
- ways[C] = ways[A] + ways[B]

**Modified Dijkstra's Logic:**
1. **Shorter path found**: Update distance and reset ways count
2. **Equal path found**: Keep same distance but add to ways count  
3. **Longer path found**: Ignore (standard Dijkstra's behavior)

This works because when we process nodes in order of shortest distance (thanks to priority queue), we guarantee that we've found the optimal way to reach each node.

## Approach Steps

1. **Initialize data structures**:
   - Distance array with infinity for all nodes except source (set to 0)
   - Ways array with 0 for all nodes except source (set to 1)  
   - Priority queue for Dijkstra's algorithm
2. **Run modified Dijkstra's**:
   - Process nodes in order of shortest distance
   - For each neighbor, check if we found shorter, equal, or longer path
3. **Handle three cases**:
   - **Shorter path**: Update distance and reset ways count
   - **Equal path**: Keep distance, add to ways count
   - **Longer path**: Ignore
4. **Apply modulo**: Use modulo 10^9+7 to prevent integer overflow
5. **Return result**: Return ways[destination]

## Code

### Method 1: Modified Implementation (Based on Provided Code)
```python
import heapq

MOD = 10**9 + 7

def solution(nodes, graph, src, dest):
    # Initialize distance and ways arrays
    distance = {i: float('inf') for i in nodes}
    ways = {i: 0 for i in nodes}
    
    # Priority queue: (distance, node)
    queue = [(0, src)]
    distance[src] = 0
    ways[src] = 1
    
    while queue:
        dist, node = heapq.heappop(queue)
        
        # Skip if we've already processed this node with better distance
        if dist > distance[node]:
            continue
        
        # Process all neighbors
        for adj_node, adj_weight in graph[node].items():
            new_dist = distance[node] + adj_weight
            
            if new_dist < distance[adj_node]:
                # Found shorter path: update distance and reset ways
                distance[adj_node] = new_dist
                ways[adj_node] = ways[node]
                heapq.heappush(queue, (new_dist, adj_node))
                
            elif new_dist == distance[adj_node]:
                # Found equal path: add to ways count
                ways[adj_node] = (ways[node] + ways[adj_node]) % MOD
    
    return ways[dest]

class Solution:
    def countPaths(self, n, roads):
        nodes = [i for i in range(n)]
        graph = {i: {} for i in nodes}
        
        # Build undirected graph
        for u, v, weight in roads:
            graph[u][v] = weight
            graph[v][u] = weight
        
        return solution(nodes, graph, 0, n - 1)
```

### Method 2: Clean Standard Implementation
```python
import heapq

def countPaths(n, roads):
    MOD = 10**9 + 7
    
    # Build adjacency list
    graph = [[] for _ in range(n)]
    for u, v, time in roads:
        graph[u].append((v, time))
        graph[v].append((u, time))
    
    # Dijkstra's with path counting
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
                # Shorter path found
                dist[v] = new_dist
                ways[v] = ways[u]
                heapq.heappush(pq, (new_dist, v))
                
            elif new_dist == dist[v]:
                # Equal path found
                ways[v] = (ways[v] + ways[u]) % MOD
    
    return ways[n - 1]

class Solution:
    def countPaths(self, n, roads):
        return countPaths(n, roads)
```

### Method 3: With Visited Set (Alternative)
```python
import heapq

def countPathsWithVisited(n, roads):
    MOD = 10**9 + 7
    
    # Build graph
    graph = [[] for _ in range(n)]
    for u, v, time in roads:
        graph[u].append((v, time))
        graph[v].append((u, time))
    
    # Initialize arrays
    dist = [float('inf')] * n
    ways = [0] * n
    processed = [False] * n
    
    dist[0] = 0
    ways[0] = 1
    
    pq = [(0, 0)]
    
    while pq:
        d, u = heapq.heappop(pq)
        
        if processed[u]:
            continue
            
        processed[u] = True
        
        for v, time in graph[u]:
            new_dist = dist[u] + time
            
            if new_dist < dist[v]:
                dist[v] = new_dist
                ways[v] = ways[u]
                if not processed[v]:
                    heapq.heappush(pq, (new_dist, v))
                    
            elif new_dist == dist[v] and not processed[v]:
                ways[v] = (ways[v] + ways[u]) % MOD
    
    return ways[n - 1]
```

### Method 4: Debug Version (For Understanding)
```python
import heapq

def countPathsDebug(n, roads):
    MOD = 10**9 + 7
    
    graph = [[] for _ in range(n)]
    for u, v, time in roads:
        graph[u].append((v, time))
        graph[v].append((u, time))
    
    dist = [float('inf')] * n
    ways = [0] * n
    
    dist[0] = 0
    ways[0] = 1
    
    pq = [(0, 0)]
    
    while pq:
        d, u = heapq.heappop(pq)
        
        if d > dist[u]:
            continue
        
        print(f"Processing node {u} with distance {d}")
        
        for v, time in graph[u]:
            new_dist = dist[u] + time
            
            if new_dist < dist[v]:
                print(f"  Shorter path to {v}: {new_dist} (was {dist[v]})")
                dist[v] = new_dist
                ways[v] = ways[u]
                heapq.heappush(pq, (new_dist, v))
                
            elif new_dist == dist[v]:
                print(f"  Equal path to {v}: {new_dist}, adding {ways[u]} ways")
                ways[v] = (ways[v] + ways[u]) % MOD
    
    print(f"Final distances: {dist}")
    print(f"Final ways: {ways}")
    
    return ways[n - 1]
```

## Time and Space Complexity

**Time Complexity: O((V + E) log V)**
- Same as standard Dijkstra's algorithm
- Each vertex is processed at most once when extracted from priority queue
- Each edge relaxation involves heap operations
- V is number of intersections, E is number of roads

**Space Complexity: O(V + E)**
- Adjacency list representation: O(E) space
- Distance and ways arrays: O(V) space each
- Priority queue: O(V) space in worst case
- Overall: O(V + E)

## Key Points to Remember

**Why modulo is needed:**
- Number of paths can grow exponentially
- Without modulo, integer overflow would occur
- Apply modulo only when adding ways, not when comparing distances

**Critical insight:**
- We only add ways when distances are equal
- This ensures we're only counting shortest paths
- The ways array represents number of shortest paths to each node

**Edge cases:**
- Direct path exists (might not be shortest)
- Multiple equal shortest paths
- No path exists (shouldn't happen given problem constraints)
- Single node graph (n=1)

**Common mistakes:**
- Forgetting to apply modulo
- Adding ways for longer paths (should only add for equal distances)
- Not handling duplicate entries in priority queue properly

**Real-world applications:**
- GPS routing systems (finding all fastest routes)
- Network routing (load balancing across equal-cost paths)
- Transportation optimization
- Supply chain route planning

---
