# Directed Graph Problems - Revision Notes

## Table of Contents
1. [Topological Sort](#1-topological-sort)
2. [Detect Cycle in Directed Graph](#2-detect-cycle-in-directed-graph)
3. [Find Eventual Safe States](#3-find-eventual-safe-states)
4. [Course Schedule I](#4-course-schedule-i)
5. [Course Schedule II](#5-course-schedule-ii)
6. [Comparison Table](#comparison-table)

---

## 1. Topological Sort

### Problem Description
Given a Directed Acyclic Graph (DAG) with V vertices (0 to V-1) represented as an adjacency list, find any topological sorting. In topological sorting, node u always appears before node v if there's a directed edge u → v.

### Sample Test Cases
```python
# Example 1
V = 6, adj = [[], [], [3], [1], [0,1], [5], [0,2]]
Output: [5, 4, 2, 3, 1, 0]
# Multiple valid orderings possible

# Example 2
V = 4, adj = [[], [0], [0], [0]]
Output: [3, 2, 1, 0]

# Example 3
V = 3, adj = [[1], [2], []]
Output: [0, 1, 2]
```

### Approach 1: DFS with Stack

**Intuition**: Like getting dressed - you must wear underwear before pants. Go as deep as possible first, and only after handling all dependencies, add yourself to the final order. Use a stack because the last person to finish dependencies should be first in final answer.

**Code**:
```python
def solution(nodes, adj_list):
    graph = adj_list
    visited = {i: False for i in nodes}  # Track visited nodes
    stack = []  # Stack to store topological order
    
    def dfs(node):
        """DFS to process node after all dependencies"""
        visited[node] = True  # Mark current node as visited
        
        # Visit all adjacent nodes (dependencies)
        for adj_node in graph[node]:
            if not visited[adj_node]:
                dfs(adj_node)  # Recursive DFS call
        
        # Add node to stack after processing all dependencies
        stack.append(node)
    
    # Step 1: Start DFS from all unvisited nodes
    for i in nodes:
        if not visited[i]:
            dfs(i)
    
    # Step 2: Pop from stack to get topological order
    ans = []
    while stack:
        ans.append(stack.pop())
    
    return ans
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

### Approach 2: Kahn's Algorithm (BFS)

**Intuition**: Like taking college courses - start with courses that have no prerequisites (in-degree = 0), take them first, then see what new courses become available. Process things in the order they become available.

**Code**:
```python
from collections import deque

def solution(nodes, graph):
    # Step 1: Calculate in-degree for each node
    inDegree = {i: 0 for i in nodes}
    for node, adj_nodes in graph.items():
        for i in adj_nodes:
            inDegree[i] += 1  # Count incoming edges
    
    # Step 2: Add all nodes with in-degree 0 to queue
    queue = deque()
    for node, degree in inDegree.items():
        if degree == 0:  # No prerequisites
            queue.append(node)
    
    # Step 3: Process nodes level by level
    ans = []
    while queue:
        node = queue.popleft()  # Process node with no dependencies
        ans.append(node)
        
        # Reduce in-degree of adjacent nodes
        for adj_node in graph[node]:
            inDegree[adj_node] -= 1
            if inDegree[adj_node] == 0:  # If no more dependencies
                queue.append(adj_node)
    
    return ans
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

---

## 2. Detect Cycle in Directed Graph

### Problem Description
Given a directed graph with V vertices (0 to V-1) represented as an adjacency list, determine if the graph contains any cycles.

### Sample Test Cases
```python
# Example 1
V = 6, adj = [[1], [2, 5], [3], [4], [1], []]
Output: True
# Cycle: 1 → 2 → 3 → 4 → 1

# Example 2
V = 4, adj = [[1,2], [2], [], [0,2]]
Output: False
```

### Approach 1: DFS with Path Tracking

**Intuition**: Like walking through a maze with one-way streets. A cycle exists if you can come back to a place on your current path. Track two things: places you've visited ever, and places on your current path. If you find your own breadcrumbs (current path), you're going in circles!

**Code**:
```python
def solution(nodes, adj_list):
    graph = adj_list
    visited = {i: False for i in nodes}  # Track all visited nodes
    pathVisited = {i: False for i in nodes}  # Track nodes in current path
    
    def dfs(node):
        """DFS to detect cycles using path tracking"""
        visited[node] = True  # Mark as visited
        pathVisited[node] = True  # Add to current path
        
        # Check all adjacent nodes
        for adj_node in graph[node]:
            if not visited[adj_node]:  # If not visited, explore
                check = dfs(adj_node)
                if check == True:  # Cycle found in recursion
                    return True
            else:  # If already visited
                if pathVisited[adj_node]:  # If in current path
                    return True  # Cycle found!
        
        pathVisited[node] = False  # Remove from current path (backtrack)
        return False
    
    # Check each unvisited node
    for i in nodes:
        if not visited[i]:
            check = dfs(i)
            if check == True:  # Cycle found
                return True
    
    return False  # No cycle found
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

### Approach 2: Topological Sort (Kahn's Algorithm)

**Intuition**: Try to arrange all tasks in proper order. If you can arrange ALL V nodes, no cycle exists. If some nodes get stuck waiting for each other in a circle, a cycle exists!

**Code**:
```python
from collections import deque

def solution(nodes, graph):
    def topologicalSort(nodes, graph):
        """Try to perform topological sort"""
        # Calculate in-degree for each node
        inDegree = {i: 0 for i in nodes}
        for node, adj_nodes in graph.items():
            for i in adj_nodes:
                inDegree[i] += 1
        
        # Add all nodes with in-degree 0 to queue
        queue = deque()
        for node, degree in inDegree.items():
            if degree == 0:
                queue.append(node)
        
        ans = []
        while queue:
            node = queue.popleft()  # Process node with no dependencies
            ans.append(node)
            
            # Reduce in-degree of adjacent nodes
            for adj_node in graph[node]:
                inDegree[adj_node] -= 1
                if inDegree[adj_node] == 0:
                    queue.append(adj_node)
        
        return ans
    
    topoSort = topologicalSort(nodes, graph)
    
    # If all nodes processed, no cycle
    if len(topoSort) == len(nodes):
        return False
    return True  # Some nodes stuck, cycle exists
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

---

## 3. Find Eventual Safe States

### Problem Description
Given a directed graph with V vertices represented as an adjacency list, find all safe nodes. A node is safe if every possible path starting from that node leads to a terminal node (node with no outgoing edges). Return safe nodes in ascending order.

### Sample Test Cases
```python
# Example 1
V = 7, adj = [[1,2], [2,3], [5], [0], [5], [], []]
Output: [2, 4, 5, 6]
# Nodes 5, 6 are terminal. Nodes 2, 4 lead only to terminal nodes.

# Example 2
V = 4, adj = [[1], [2], [0,3], []]
Output: [3]
# Only node 3 is terminal and safe
```

### Approach 1: Topological Sort on Reversed Graph

**Intuition**: Like a river system flowing to the ocean. Safe nodes will eventually reach the ocean (terminal nodes). Reverse all edges and start from terminal nodes - any node reachable backwards from terminal nodes is safe!

**Code**:
```python
from collections import deque

def solution(nodes, edges):
    def topologicalSort(nodes, graph):
        """Topological sort on reversed graph"""
        # Calculate in-degree for each node in reversed graph
        inDegree = {i: 0 for i in nodes}
        for node, adj_nodes in graph.items():
            for i in adj_nodes:
                inDegree[i] += 1
        
        # Add nodes with in-degree 0 (terminal nodes in original)
        queue = deque()
        for node, degree in inDegree.items():
            if degree == 0:
                queue.append(node)
        
        ans = []
        while queue:
            node = queue.popleft()  # Process safe node
            ans.append(node)
            
            # Reduce in-degree (nodes that lead to this safe node)
            for adj_node in graph[node]:
                inDegree[adj_node] -= 1
                if inDegree[adj_node] == 0:  # All paths lead to safe nodes
                    queue.append(adj_node)
        
        return ans
    
    # Step 1: Create reversed graph
    graph = {i: set() for i in nodes}
    for i, j in edges:
        graph[j].add(i)  # Reverse: i→j becomes j→i
    
    # Step 2: Apply topological sort
    topoSort = topologicalSort(nodes, graph)
    topoSort.sort()  # Return in ascending order
    
    return topoSort
```

**Time Complexity**: O(V + E + V log V)  
**Space Complexity**: O(V + E)

### Approach 2: DFS with Safe State Tracking

**Intuition**: Like exploring a haunted house. A room is safe if ALL paths from it lead to exits (terminals), not to loops. Mark nodes as safe only after confirming all paths are safe.

**Code**:
```python
def solution(nodes, adj_list):
    graph = adj_list
    visited = {i: False for i in nodes}  # Track visited nodes
    pathVisited = {i: False for i in nodes}  # Track current path
    safe = {i: False for i in nodes}  # Track safe nodes
    
    def dfs(node):
        """DFS to determine if node is safe"""
        visited[node] = True  # Mark as visited
        pathVisited[node] = True  # Add to current path
        
        # Explore all adjacent nodes
        for adj_node in graph[node]:
            if not visited[adj_node]:  # Not visited
                if not dfs(adj_node):  # If adjacent unsafe
                    pathVisited[node] = False
                    return False  # Current node also unsafe
            elif pathVisited[adj_node]:  # Cycle detected
                pathVisited[node] = False
                return False  # Unsafe
            elif not safe[adj_node]:  # Known unsafe node
                pathVisited[node] = False
                return False  # Current node also unsafe
        
        pathVisited[node] = False  # Backtrack
        safe[node] = True  # Mark as safe
        return True
    
    # Check each unvisited node
    for i in nodes:
        if not visited[i]:
            dfs(i)
    
    # Return all safe nodes in sorted order
    result = [node for node in nodes if safe[node]]
    return result
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

---

## 4. Course Schedule I

### Problem Description
Given N tasks (0 to N-1) and an array of dependencies where [a, b] means you must complete task b before task a, determine if it's possible to finish all tasks.

### Sample Test Cases
```python
# Example 1
N = 4, arr = [[1,0], [2,1], [3,2]]
Output: True
# Order: 0 → 1 → 2 → 3

# Example 2
N = 4, arr = [[0,1], [3,2], [1,3], [3,0]]
Output: False
# Circular dependency exists
```

### Approach: Topological Sort (Kahn's Algorithm)

**Intuition**: Like planning college courses with prerequisites. You can graduate if you can arrange all courses without circular dependencies. Use topological sort - if all N courses can be arranged, it's possible; if some get stuck in circular dependencies, it's impossible.

**Code**:
```python
from collections import deque

def solution(nodes, edges):
    def topologicalSort(nodes, graph):
        """Check if all courses can be arranged"""
        # Calculate in-degree (number of prerequisites)
        inDegree = {i: 0 for i in nodes}
        for node, adj_nodes in graph.items():
            for i in adj_nodes:
                inDegree[i] += 1
        
        # Add courses with no prerequisites to queue
        queue = deque()
        for node, degree in inDegree.items():
            if degree == 0:
                queue.append(node)
        
        ans = []
        while queue:
            node = queue.popleft()  # Take course with no prerequisites
            ans.append(node)
            
            # Remove as prerequisite for other courses
            for adj_node in graph[node]:
                inDegree[adj_node] -= 1
                if inDegree[adj_node] == 0:  # All prerequisites done
                    queue.append(adj_node)
        
        return ans
    
    # Build graph: for [a,b], add edge b → a
    graph = {i: set() for i in nodes}
    for i, j in edges:
        graph[j].add(i)  # j must come before i
    
    topoSort = topologicalSort(nodes, graph)
    
    # If all courses arranged, possible
    if len(topoSort) == len(nodes):
        return True
    return False  # Circular dependency exists
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V + E)

---

## 5. Course Schedule II

### Problem Description
Given N tasks (0 to N-1) and an array of dependencies where [a, b] means you must complete task b before task a, find the order to finish all tasks. Return empty array if impossible.

### Sample Test Cases
```python
# Example 1
N = 4, arr = [[1,0], [2,1], [3,2]]
Output: [0, 1, 2, 3]

# Example 2
N = 4, arr = [[0,1], [3,2], [1,3], [3,0]]
Output: []
# Circular dependency, impossible
```

### Approach: Topological Sort (Kahn's Algorithm)

**Intuition**: Like Course Schedule I, but now we need the actual study plan, not just whether it's possible. Use topological sort to build the complete semester-by-semester plan. If we can plan all N courses, return the plan; if stuck due to circular dependencies, return empty array.

**Code**:
```python
from collections import deque

def solution(nodes, edges):
    def topologicalSort(nodes, graph):
        """Generate course completion order"""
        # Calculate in-degree (number of prerequisites)
        inDegree = {i: 0 for i in nodes}
        for node, adj_nodes in graph.items():
            for i in adj_nodes:
                inDegree[i] += 1
        
        # Add courses with no prerequisites to queue
        queue = deque()
        for node, degree in inDegree.items():
            if degree == 0:
                queue.append(node)
        
        ans = []
        while queue:
            node = queue.popleft()  # Take next available course
            ans.append(node)  # Add to study plan
            
            # Remove as prerequisite for other courses
            for adj_node in graph[node]:
                inDegree[adj_node] -= 1
                if inDegree[adj_node] == 0:  # Course becomes available
                    queue.append(adj_node)
        
        return ans
    
    # Build graph: for [a,b], add edge b → a
    graph = {i: set() for i in nodes}
    for i, j in edges:
        graph[j].add(i)  # j must come before i
    
    topoSort = topologicalSort(nodes, graph)
    
    # If all courses planned, return plan
    if len(topoSort) == len(nodes):
        return topoSort
    return []  # Return empty if impossible
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V + E)

---

## Comparison Table

| # | Problem | Key Technique | Graph Type | Time | Space | Key Insight |
|---|---------|---------------|------------|------|-------|-------------|
| 1 | Topological Sort | DFS/BFS (Kahn's) | Directed | O(V+E) | O(V) | Process dependencies first, then self |
| 2 | Cycle Detection (Directed) | DFS Path Tracking / Kahn's | Directed | O(V+E) | O(V) | Track current path; or check if topo sort fails |
| 3 | Eventual Safe States | Reverse + Kahn's / DFS | Directed | O(V+E) | O(V+E) | Reverse graph from terminals; or track safe paths |
| 4 | Course Schedule I | Kahn's Algorithm | Directed | O(V+E) | O(V+E) | Check if topo sort includes all nodes |
| 5 | Course Schedule II | Kahn's Algorithm | Directed | O(V+E) | O(V+E) | Return topo sort order or empty array |

### Problem Categories

**Topological Sort Based**: 1, 2, 3, 4, 5  
**Cycle Detection**: 2, 4  
**Path Property**: 3  

### Key Concepts

**In-Degree**: Number of incoming edges (prerequisites)  
**Kahn's Algorithm**: BFS approach for topological sort using in-degree  
**DFS with Stack**: Post-order traversal for topological sort  
**Path Tracking**: Detect cycles by tracking current recursion path  
**Graph Reversal**: Solve safe states by reversing edges from terminals
