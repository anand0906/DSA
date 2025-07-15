# Topological Sort or Kahn's Algorithm

## Problem Statement
Given a Directed Acyclic Graph (DAG) with V vertices labeled from 0 to V-1. The graph is represented using an adjacency list where adj[i] lists all nodes connected to node. Find any Topological Sorting of that Graph.

In topological sorting, node u will always appear before node v if there is a directed edge from node u towards node v (u -> v).

The Output will be your topological sort list.

## Examples
<img src="https://static.takeuforward.org/content/ProblemSetter-YmZlLBfA" />

### Example 1:
**Input:** V = 6, adj = [[], [], [3], [1], [0,1], [0,2]]
**Output:** [5, 4, 2, 3, 1, 0]

**Explanation:** A graph may have multiple topological sortings. The result is one of them. The necessary conditions for the ordering are:
- According to edge 5 -> 0, node 5 must appear before node 0 in the ordering.
- According to edge 4 -> 0, node 4 must appear before node 0 in the ordering.
- According to edge 5 -> 2, node 5 must appear before node 2 in the ordering.
- According to edge 2 -> 3, node 2 must appear before node 3 in the ordering.
- According to edge 3 -> 1, node 3 must appear before node 1 in the ordering.
- According to edge 4 -> 1, node 4 must appear before node 1 in the ordering.

The above result satisfies all the necessary conditions. [4, 5, 2, 3, 1, 0] is also one such topological sorting that satisfies all the conditions.

### Example 2:
<img src="https://static.takeuforward.org/content/ProblemSetter-ZqsXEYq1" />

**Input:** V = 4, adj = [[], [0], [0], [0]]
**Output:** [3, 2, 1, 0]

**Explanation:** The necessary conditions for the ordering are:
- For edge 1 -> 0 node 1 must appear before node 0.
- For edge 2 -> 0 node 2 must appear before node 0.
- For edge 3 -> 0 node 3 must appear before node 0.

### Example 3:
**Input:** V = 3, adj = [[1], [2], []]
**Output:** [0, 1, 2]

## Solution 1: Using Depth First Search (DFS)

### Intuition
Think of it like getting dressed - you must put on your underwear before your pants, and socks before shoes. 

In DFS approach: We go as deep as possible first (like checking what you need to wear under your shirt), and only when we've handled everything that depends on us, we add ourselves to the final order. It's like saying "I can only be placed in the final order after all my dependencies are done."

We use a stack because the last person to finish their dependencies should be first in our final answer - just like when you finish getting dressed, the last thing you put on is often the first thing people see.

### Approach Steps
1. Initialize a visited dictionary to track processed nodes
2. Create an empty stack to store the topological order
3. For each unvisited node, perform DFS:
   - Mark the current node as visited
   - Recursively visit all adjacent nodes that haven't been visited
   - After processing all adjacent nodes, push the current node to the stack
4. Pop all elements from the stack to get the topological order

### Code
```python
# using depth first search
def solution(nodes,adj_list):
    graph=adj_list
    visited={i:False for i in nodes}  # Track visited nodes
    stack=[]  # Stack to store topological order
    
    def dfs(node):
        visited[node]=True  # Mark current node as visited
        for adj_node in graph[node]:  # Visit all adjacent nodes
            if(not visited[adj_node]):
                dfs(adj_node)  # Recursive DFS call
        stack.append(node)  # Add node to stack after processing all dependencies
    
    # Start DFS from all unvisited nodes
    for i in nodes:
        if not visited[i]:
            dfs(i)
    
    # Pop from stack to get topological order
    ans=[]
    while stack:
        ans.append(stack.pop())
    return ans
```

### Time and Space Complexity
- **Time Complexity:** O(V + E) where V is the number of vertices and E is the number of edges
- **Space Complexity:** O(V) for the visited dictionary, stack, and recursion stack

## Solution 2: Using Kahn's Algorithm (BFS approach)

### Intuition
Think of it like taking courses in college - you can't take Advanced Math until you've completed Basic Math first.

In Kahn's algorithm: We start with courses that have no prerequisites (in-degree = 0), take them first, and then see what new courses become available. It's like saying "What can I do right now?" and then updating the list as we complete each task.

We use a queue because we want to process things in the order they become available - first come, first served for things that are ready to be done.

### Approach Steps
1. Calculate the in-degree (number of incoming edges) for each node
2. Add all nodes with in-degree 0 to a queue
3. While the queue is not empty:
   - Remove a node from the queue and add it to the result
   - For each adjacent node, decrease its in-degree by 1
   - If the adjacent node's in-degree becomes 0, add it to the queue
4. Return the result list

### Code
```python
# using depth first search
from collections import deque
def solution(nodes,graph):
    # Calculate in-degree for each node
    inDegree={i:0 for i in nodes}
    for node,adj_nodes in graph.items():
        for i in adj_nodes:
            inDegree[i]+=1
    
    # Add all nodes with in-degree 0 to queue
    queue=deque()
    for node,degree in inDegree.items():
        if(degree==0):
            queue.append(node)
    
    ans=[]
    while queue:
        node=queue.popleft()  # Process node with no dependencies
        ans.append(node)
        
        # Reduce in-degree of adjacent nodes
        for adj_node in graph[node]:
            inDegree[adj_node]-=1
            if(inDegree[adj_node]==0):  # If no more dependencies, add to queue
                queue.append(adj_node)
    return ans
```

### Time and Space Complexity
- **Time Complexity:** O(V + E) where V is the number of vertices and E is the number of edges
- **Space Complexity:** O(V) for the in-degree dictionary, queue, and result list

---

# Detect a Cycle in a Directed Graph

## Problem Statement
Given a directed graph with V vertices labeled from 0 to V-1. The graph is represented using an adjacency list where adj[i] lists all nodes connected to node. Determine if the graph contains any cycles.

## Examples

### Example 1:
**Input:** V = 6, adj = [[1], [2, 5], [3], [4], [1], []]
**Output:** True

**Explanation:** The graph contains a cycle: 1 -> 2 -> 3 -> 4 -> 1.

### Example 2:
**Input:** V = 4, adj = [[1,2], [2], [], [0,2]]
**Output:** False

**Explanation:** The graph does not contain a cycle.

## Solution 1: Using Depth First Search (DFS)

### Intuition
Think of it like walking through a maze where some paths are one-way streets. A cycle exists if you can start walking from any point and eventually come back to a place you've already been to in your current journey.

The key insight: It's not enough to just remember places you've visited before - you need to remember which places are part of your current path. If you meet someone who's already on your current path, you've found a cycle! But if you meet someone who visited earlier via a different route, that's okay.

It's like leaving breadcrumbs on your current path - if you find your own breadcrumbs, you're going in circles!

### Approach Steps
1. Keep track of two things for each node:
   - `visited`: Have we ever been to this node?
   - `pathVisited`: Is this node part of our current path?
2. For each unvisited node, start a DFS:
   - Mark the current node as visited and add it to current path
   - Visit all adjacent nodes
   - If we find an unvisited adjacent node, recursively check it
   - If we find a node that's already in our current path, we found a cycle!
   - Before returning, remove the current node from the path (backtrack)
3. If any DFS finds a cycle, return True; otherwise return False

### Code
```python
# using depth first search
def solution(nodes,adj_list):
    graph=adj_list
    visited={i:False for i in nodes}  # Track all visited nodes
    pathVisited={i:False for i in nodes}  # Track nodes in current path
    
    def dfs(node):
        visited[node]=True  # Mark as visited
        pathVisited[node]=True  # Add to current path
        for adj_node in graph[node]:  # Check all adjacent nodes
            if(not visited[adj_node]):  # If not visited, explore recursively
                check=dfs(adj_node)
                if(check==True):  # If cycle found in recursion, return True
                    return True
            else:  # If already visited
                if(pathVisited[adj_node]):  # If in current path, cycle found!
                    return True
        pathVisited[node]=False  # Remove from current path (backtrack)
        return False
    
    # Check each unvisited node
    for i in nodes:
        if not visited[i]:
            check=dfs(i)
            if(check==True):  # If cycle found, return True
                return True
    return False  # No cycle found
```

### Time and Space Complexity
- **Time Complexity:** O(V + E) where V is the number of vertices and E is the number of edges
- **Space Complexity:** O(V) for the visited arrays and recursion stack

## Solution 2: Using Topological Sort (Kahn's Algorithm)

### Intuition
Think of it like arranging tasks where some tasks must be done before others. If you can arrange ALL tasks in a proper order where no task depends on itself (directly or indirectly), then there's no cycle.

But if you can't arrange all tasks because some tasks are waiting for each other in a circle (like "Task A needs Task B, Task B needs Task C, Task C needs Task A"), then there's a cycle!

The clever trick: Use topological sort to try arranging all nodes. If we can arrange all V nodes, no cycle exists. If we can't arrange all nodes (some are stuck waiting), a cycle exists.

### Approach Steps
1. Use Kahn's algorithm to perform topological sort:
   - Calculate in-degree for each node
   - Add all nodes with in-degree 0 to queue
   - Process nodes one by one, reducing in-degree of their neighbors
   - Keep track of how many nodes we successfully processed
2. If we processed all V nodes, no cycle exists (return False)
3. If we couldn't process all nodes, a cycle exists (return True)

### Code
```python
from collections import deque

def solution(nodes,graph):
    def topologicalSort(node,graph):
        # Calculate in-degree for each node
        inDegree={i:0 for i in nodes}
        for node,adj_nodes in graph.items():
            for i in adj_nodes:
                inDegree[i]+=1
        
        # Add all nodes with in-degree 0 to queue
        queue=deque()
        for node,degree in inDegree.items():
            if(degree==0):
                queue.append(node)
        
        ans=[]
        while queue:
            node=queue.popleft()  # Process node with no dependencies
            ans.append(node)
            
            # Reduce in-degree of adjacent nodes
            for adj_node in graph[node]:
                inDegree[adj_node]-=1
                if(inDegree[adj_node]==0):  # If no more dependencies, add to queue
                    queue.append(adj_node)
        return ans

    topoSort=topologicalSort(nodes,graph)
    if(len(topoSort)==len(nodes)):  # If all nodes processed, no cycle
        return False
    return True  # If some nodes couldn't be processed, cycle exists
```

### Time and Space Complexity
- **Time Complexity:** O(V + E) where V is the number of vertices and E is the number of edges
- **Space Complexity:** O(V) for the in-degree dictionary, queue, and result list



