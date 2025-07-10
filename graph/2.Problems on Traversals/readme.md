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

