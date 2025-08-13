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
