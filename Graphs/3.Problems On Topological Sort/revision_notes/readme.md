# Directed Graph Problems - Revision Notes

## Table of Contents
1. [Topological Sort](#1-topological-sort)
2. [Detect Cycle in Directed Graph](#2-detect-cycle-in-directed-graph)
3. [Find Eventual Safe States](#3-find-eventual-safe-states)
4. [Course Schedule I](#4-course-schedule-i)
5. [Course Schedule II](#5-course-schedule-ii)
6. [Alien Dictionary](#6-alien-dictionary)
7. [Comparison Table](#comparison-table)

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

## 6. Alien Dictionary

### Problem Description
Given a sorted dictionary of N words in an alien language with K starting alphabets, find the order of characters in the alien language. The words are sorted lexicographically according to the alien language's rules. Return any valid ordering as a string, or empty string "" if the arrangement is inconsistent.

### Sample Test Cases
```python
# Example 1
N = 5, K = 4, dict = ["baa", "abcd", "abca", "cab", "cad"]
Output: "bdac"
# Analysis:
# "baa" vs "abcd" → b comes before a
# "abcd" vs "abca" → d comes before a
# "abca" vs "cab" → a comes before c
# "cab" vs "cad" → b comes before d

# Example 2
N = 3, K = 3, dict = ["caa", "aaa", "aab"]
Output: "cab"
# "caa" vs "aaa" → c comes before a
# "aaa" vs "aab" → a comes before b

# Example 3
N = 2, K = 2, dict = ["abc", "ab"]
Output: ""
# Inconsistent: longer word cannot come before its prefix
```

### Approach: Topological Sort (Kahn's Algorithm)

**Intuition**: Like discovering alphabet order from a sorted dictionary. When two words differ at position j, the character in the first word must come before the character in the second word. Build a directed graph where edges represent "comes before" relationships, then use topological sort to find the character order. This is essentially finding dependencies between characters!

**Key Steps**:
1. Compare consecutive words to extract character ordering edges
2. Build directed graph from these edges
3. Apply topological sort to get final character order
4. If topological sort succeeds for all K characters, return the order

**Code**:
```python
from collections import deque

def solution(n, k, dict_words):
    def getEdges(n, arr):
        """Extract character ordering from consecutive word pairs"""
        edges = []
        for i in range(1, n):
            word1 = arr[i-1]
            word2 = arr[i]
            
            # Find first differing character
            min_len = min(len(word1), len(word2))
            for j in range(min_len):
                if word1[j] != word2[j]:
                    # word1[j] comes before word2[j]
                    edges.append([word1[j], word2[j]])
                    break
            else:
                # All characters match up to min_len
                # If word1 is longer, dictionary is invalid
                if len(word1) > len(word2):
                    return None  # Inconsistent ordering
        
        return edges
    
    def topologicalSort(nodes, graph):
        """Kahn's algorithm for topological sort"""
        # Calculate in-degree for each character
        inDegree = {i: 0 for i in nodes}
        for node, adj_nodes in graph.items():
            for adj in adj_nodes:
                inDegree[adj] += 1
        
        # Start with characters that have no prerequisites
        queue = deque()
        for node, degree in inDegree.items():
            if degree == 0:
                queue.append(node)
        
        ans = []
        while queue:
            node = queue.popleft()  # Process character
            ans.append(node)
            
            # Reduce in-degree of dependent characters
            for adj_node in graph[node]:
                inDegree[adj_node] -= 1
                if inDegree[adj_node] == 0:
                    queue.append(adj_node)
        
        return ans
    
    # Step 1: Generate nodes (first k letters of alphabet)
    nodes = [chr(ord('a') + i) for i in range(k)]
    
    # Step 2: Extract ordering edges from consecutive words
    edges = getEdges(n, dict_words)
    if edges is None:  # Inconsistent dictionary
        return ""
    
    # Step 3: Build adjacency list graph
    graph = {char: [] for char in nodes}
    for char1, char2 in edges:
        graph[char1].append(char2)
    
    # Step 4: Apply topological sort
    ans = topologicalSort(nodes, graph)
    
    # Step 5: Verify all characters are included (no cycle)
    if len(ans) == k:
        return "".join(ans)
    return ""  # Cycle detected, inconsistent ordering
```

**Time Complexity**: O(N × L + K), where L is average word length  
**Space Complexity**: O(K + E), where E is number of edges

**Edge Cases**:
- Longer word appearing before its prefix (e.g., "abc" before "ab") → invalid, return ""
- Cycle in character dependencies → invalid, return ""
- Multiple valid orderings → return any valid one

---

## Comparison Table

| # | Problem | Key Technique | Graph Type | Time | Space | Key Insight |
|---|---------|---------------|------------|------|-------|-------------|
| 1 | Topological Sort | DFS/BFS (Kahn's) | Directed | O(V+E) | O(V) | Process dependencies first, then self |
| 2 | Cycle Detection (Directed) | DFS Path Tracking / Kahn's | Directed | O(V+E) | O(V) | Track current path; or check if topo sort fails |
| 3 | Eventual Safe States | Reverse + Kahn's / DFS | Directed | O(V+E) | O(V+E) | Reverse graph from terminals; or track safe paths |
| 4 | Course Schedule I | Kahn's Algorithm | Directed | O(V+E) | O(V+E) | Check if topo sort includes all nodes |
| 5 | Course Schedule II | Kahn's Algorithm | Directed | O(V+E) | O(V+E) | Return topo sort order or empty array |
| 6 | Alien Dictionary | Kahn's Algorithm | Directed | O(N×L+K) | O(K+E) | Extract ordering from word pairs, apply topo sort |

### Problem Categories

**Topological Sort Based**: 1, 2, 3, 4, 5, 6  
**Cycle Detection**: 2, 4, 6  
**Path Property**: 3  
**String/Character Ordering**: 6

### Key Concepts

**In-Degree**: Number of incoming edges (prerequisites)  
**Kahn's Algorithm**: BFS approach for topological sort using in-degree  
**DFS with Stack**: Post-order traversal for topological sort  
**Path Tracking**: Detect cycles by tracking current recursion path  
**Graph Reversal**: Solve safe states by reversing edges from terminals  
**Character Ordering**: Extract dependencies by comparing consecutive strings
