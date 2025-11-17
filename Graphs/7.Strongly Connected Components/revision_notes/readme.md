# Graph Algorithms - Revision Notes

## Table of Contents
1. [Kosaraju's Algorithm (Strongly Connected Components)](#1-kosarajus-algorithm-strongly-connected-components)
2. [Bridges in Graph (Tarjan's Algorithm)](#2-bridges-in-graph-tarjans-algorithm)
3. [Articulation Points (Cut Vertices)](#3-articulation-points-cut-vertices)
4. [Problems Comparison](#problems-comparison)

---

## 1. Kosaraju's Algorithm (Strongly Connected Components)

### Problem Description
Given a directed graph with V vertices and adjacency list, find the number of **Strongly Connected Components (SCCs)**. An SCC is a maximal set of vertices where every vertex is reachable from every other vertex within that set.

### Sample Test Cases

**Example 1:**
```
Input: V = 5, Adj = [[2,3], [0], [1], [4], []]
Output: 3
Explanation: SCCs are {0,1,2}, {3}, and {4}
```

**Example 2:**
```
Input: V = 8, Adj = [[1], [2], [0,3], [4], [5,7], [6], [4,7], []]
Output: 4
Explanation: Four SCCs exist
```

### Approach: Kosaraju's Algorithm (DFS → Transpose → DFS)

**Intuition:**
- **Problem**: Simple DFS visits all nodes across SCCs without identifying boundaries
- **Solution**: Use finishing times + graph transpose to isolate SCCs
- **Key Formula**: Number of DFS calls on transposed graph = Number of SCCs
- **Why it works**:
  1. First DFS gives finishing order (nodes finishing last are SCC "roots")
  2. Transpose reverses edges but preserves SCCs
  3. Second DFS on transpose processes SCCs in correct order without cross-contamination

**Code:**
```python
class Solution:
    def kosaraju(self, V, adj):
        # Step 1: First DFS to get finishing order
        def dfs1(node, graph, visited, stack):
            visited[node] = True
            for neighbor in graph[node]:
                if not visited[neighbor]:
                    dfs1(neighbor, graph, visited, stack)
            stack.append(node)  # Post-order: add when finishing
        
        # Step 2: Create transposed graph
        def create_transpose(V, adj):
            transpose = [[] for _ in range(V)]
            for u in range(V):
                for v in adj[u]:
                    transpose[v].append(u)  # Reverse edge
            return transpose
        
        # Step 3: Second DFS on transposed graph
        def dfs2(node, graph, visited):
            visited[node] = True
            for neighbor in graph[node]:
                if not visited[neighbor]:
                    dfs2(neighbor, graph, visited)
        
        # Execute Step 1
        visited = [False] * V
        stack = []
        for i in range(V):
            if not visited[i]:
                dfs1(i, adj, visited, stack)
        
        # Execute Step 2
        transpose_adj = create_transpose(V, adj)
        
        # Execute Step 3
        visited = [False] * V
        scc_count = 0
        
        while stack:
            node = stack.pop()
            if not visited[node]:
                dfs2(node, transpose_adj, visited)
                scc_count += 1  # Each DFS call = one SCC
        
        return scc_count
```

**Complexity:**
- **Time:** O(V + E) - Two DFS passes + transpose creation
- **Space:** O(V + E) - Graph storage, transpose, stack, visited arrays

---

## 2. Bridges in Graph (Tarjan's Algorithm)

### Problem Description
Given an undirected connected graph with V vertices and E edges, find all **bridges**. A bridge is an edge whose removal increases the number of connected components (disconnects the graph).

### Sample Test Cases

**Example 1:**
```
Input: V = 4, edges = [[0,1], [1,2], [2,0], [1,3]]
Output: [[1,3]]
Explanation: Removing edge [1,3] splits graph into {0,1,2} and {3}
```

**Example 2:**
```
Input: V = 3, edges = [[0,1], [1,2], [2,0]]
Output: []
Explanation: Triangle - no edge is a bridge (all part of cycle)
```

### Approach: Tarjan's Algorithm

**Intuition:**
- **Bridge condition**: Edge (u,v) is a bridge if `low[v] > tin[u]`
- **Why**: If `low[v] > tin[u]`, then v's subtree cannot reach any ancestor of u
- **Discovery time (tin[v])**: When node v was first visited
- **Low time (low[v])**: Earliest discovered node reachable from v's subtree
- **Back edges prevent bridges**: They provide alternative paths

**Code:**
```python
class Solution:
    def criticalConnections(self, n, connections):
        # Build adjacency list
        graph = [[] for _ in range(n)]
        for u, v in connections:
            graph[u].append(v)
            graph[v].append(u)
        
        tin = [0] * n      # Discovery time
        low = [0] * n      # Lowest reachable time
        visited = [False] * n
        bridges = []
        timer = [0]
        
        def dfs(node, parent):
            visited[node] = True
            timer[0] += 1
            tin[node] = low[node] = timer[0]
            
            for neighbor in graph[node]:
                if neighbor == parent:
                    continue  # Skip parent
                
                if not visited[neighbor]:
                    # Tree edge
                    dfs(neighbor, node)
                    low[node] = min(low[node], low[neighbor])
                    
                    # Bridge condition: strictly greater
                    if low[neighbor] > tin[node]:
                        bridges.append([node, neighbor])
                else:
                    # Back edge
                    low[node] = min(low[node], tin[neighbor])
        
        # Handle all connected components
        for i in range(n):
            if not visited[i]:
                dfs(i, -1)
        
        return bridges
```

**Complexity:**
- **Time:** O(V + E) - Single DFS traversal
- **Space:** O(V + E) - Adjacency list, arrays, recursion stack

---

## 3. Articulation Points (Cut Vertices)

### Problem Description
Given an undirected graph with V vertices and adjacency list, find all **articulation points**. An articulation point is a vertex whose removal (with all incident edges) increases the number of connected components.

### Sample Test Cases

**Example 1:**
```
Input: V = 7, adj = [[1,2,3], [0], [0,3,4,5], [2,0], [2,6], [5,2,6], [4,5]]
Output: [0, 2]
Explanation: Removing 0 or 2 disconnects the graph
```

**Example 2:**
```
Input: V = 5, adj = [[1], [0,4], [3,4], [2,4], [1,2,3]]
Output: [1, 4]
Explanation: Vertices 1 and 4 are critical for connectivity
```

### Approach: Tarjan's Algorithm for Articulation Points

**Intuition:**
- **Non-root condition**: Vertex u is articulation point if `low[child] >= tin[u]`
- **Root condition**: Root is articulation point if it has more than 1 child in DFS tree
- **Key difference from bridges**: `>=` instead of `>` (equal case means child can reach u but not u's ancestors)
- **Why it works**: Condition captures when removing u would isolate child's subtree

**Code:**
```python
class Solution:
    def articulationPoints(self, n, adj):
        tin = [0] * n
        low = [0] * n
        visited = [False] * n
        is_articulation = [False] * n  # Avoid duplicates
        timer = [0]
        
        def dfs(u, parent):
            visited[u] = True
            timer[0] += 1
            tin[u] = low[u] = timer[0]
            children = 0
            
            for v in adj[u]:
                if v == parent:
                    continue
                
                if not visited[v]:
                    # Tree edge
                    children += 1
                    dfs(v, u)
                    low[u] = min(low[u], low[v])
                    
                    # Articulation point conditions
                    if parent != -1 and low[v] >= tin[u]:
                        is_articulation[u] = True
                else:
                    # Back edge
                    low[u] = min(low[u], tin[v])
            
            # Root condition
            if parent == -1 and children > 1:
                is_articulation[u] = True
        
        # Process all connected components
        for i in range(n):
            if not visited[i]:
                dfs(i, -1)
        
        # Collect results
        result = [i for i in range(n) if is_articulation[i]]
        return result if result else [-1]
```

**Complexity:**
- **Time:** O(V + E) - Single DFS traversal
- **Space:** O(V + E) - Adjacency list, arrays, recursion stack

---

## Problems Comparison

| Problem | Type | Key Condition | Algorithm | Time | Space |
|---------|------|---------------|-----------|------|-------|
| **Kosaraju's SCC** | Directed | Mutual reachability | 2 DFS + Transpose | O(V+E) | O(V+E) |
| **Bridges** | Undirected | `low[v] > tin[u]` | Tarjan DFS | O(V+E) | O(V+E) |
| **Articulation Points** | Undirected | `low[v] >= tin[u]` or root children > 1 | Tarjan DFS | O(V+E) | O(V+E) |

### Key Concepts Summary

**Strongly Connected Components (SCC)**
- Maximal set where every vertex reaches every other vertex
- G and G^T (transpose) have identical SCCs
- Kosaraju uses finishing times to determine processing order

**Bridges**
- Critical edges whose removal disconnects the graph
- Condition: `low[child] > tin[parent]` (strictly greater)
- Back edges prevent bridges by providing alternative paths

**Articulation Points**
- Critical vertices whose removal disconnects the graph
- Non-root: `low[child] >= tin[parent]` (greater or equal)
- Root: Has more than 1 child in DFS tree
- Removing vertex also removes all its incident edges

### Common Patterns

1. **All use DFS traversal** for efficient exploration
2. **Discovery time (tin[])** tracks when node was first visited
3. **Low time (low[])** tracks earliest reachable ancestor
4. **Single pass algorithms** - O(V+E) optimal complexity
5. **Parent tracking** prevents immediate backtracking to parent
6. **Back edges** indicate cycles and alternative paths

### Real-World Applications

**Kosaraju's SCC:**
- Deadlock detection in operating systems
- Web page clustering for search engines
- Social network community detection
- Compiler dependency analysis

**Bridges:**
- Network vulnerability analysis
- Critical infrastructure identification
- Circuit design optimization
- Transportation network planning

**Articulation Points:**
- Single point of failure detection
- Network reliability analysis
- Router/server criticality assessment
- Supply chain bottleneck identification
