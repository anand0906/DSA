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
<img src="https://static.takeuforward.org/content/ProblemSetter-Do8SOPHS"/>

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

---
# Find Eventual Safe States

## Problem Statement
Given a directed graph with V vertices labeled from 0 to V-1. The graph is represented using an adjacency list where adj[i] lists all nodes adjacent to node i, meaning there is an edge from node i to each node in adj[i]. A node is a terminal node if there are no outgoing edges. A node is a safe node if every possible path starting from that node leads to a terminal node. Return an array containing all the safe nodes of the graph in ascending order.

## Examples

<img src="https://static.takeuforward.org/content/ProblemSetter-rrMbBwFC" />

### Example 1:
**Input:** V = 7, adj = [[1,2], [2,3], [5], [0], [5], [], []]
**Output:** [2, 4, 5, 6]

**Explanation:** 
- From node 0: two paths are there 0->2->5 and 0->1->3->0. The second path does not end at a terminal node. So it is not a safe node.
- From node 1: two paths exist: 1->3->0->1 and 1->2->5. But the first one does not end at a terminal node. So it is not a safe node.
- From node 2: only one path: 2->5 and 5 is a terminal node. So it is a safe node.
- From node 3: two paths: 3->0->1->3 and 3->0->2->5 but the first path does not end at a terminal node. So it is not a safe node.
- From node 4: Only one path: 4->5 and 5 is a terminal node. So it is also a safe node.
- From node 5: It is a terminal node. So it is a safe node as well.
- From node 6: It is a terminal node. So it is a safe node as well.

### Example 2:
<img src="https://static.takeuforward.org/content/ProblemSetter-3KCPYxOn" />

**Input:** V = 4, adj = [[1], [2], [0,3], []]
**Output:** [3]

**Explanation:** Node 3 itself is a terminal node and it is a safe node as well. But all the paths from other nodes do not lead to a terminal node. So they are excluded from the answer.

## Solution 1: Using Topological Sort (Kahn's Algorithm)

### Intuition
Think of it like a river system flowing to the ocean. Safe nodes are like water sources that will eventually reach the ocean (terminal nodes) no matter which path they take. Unsafe nodes are like water sources that get stuck in whirlpools (cycles) and never reach the ocean.

The trick: Instead of following water downstream, we trace backwards from the ocean! We reverse all the rivers (edges) so that terminal nodes become sources, and then we see which original nodes we can reach by flowing backwards. If we can reach a node by flowing backwards from terminal nodes, it means all paths from that node lead to terminal nodes.

### Approach Steps
1. **Reverse the graph**: Change all edges from A→B to B→A
2. **Apply topological sort on reversed graph**:
   - Terminal nodes (originally outdegree 0) become sources (indegree 0 in reversed graph)
   - Start from these terminal nodes and work backwards
   - Only nodes reachable from terminal nodes will be processed
3. **Sort the result**: Return all processed nodes in ascending order

### Code
```python
from collections import deque
def solution(nodes,edges):
    def topologicalSort(nodes,graph):
        # Calculate in-degree for each node in reversed graph
        inDegree={i:0 for i in nodes}
        for node,adj_nodes in graph.items():
            for i in adj_nodes:
                inDegree[i]+=1
        
        # Add all nodes with in-degree 0 to queue (terminal nodes in original graph)
        queue=deque()
        for node,degree in inDegree.items():
            if(degree==0):
                queue.append(node)
        
        ans=[]
        while queue:
            node=queue.popleft()  # Process safe node
            ans.append(node)
            
            # Reduce in-degree of adjacent nodes (nodes that lead to this safe node)
            for adj_node in graph[node]:
                inDegree[adj_node]-=1
                if(inDegree[adj_node]==0):  # If all paths lead to safe nodes
                    queue.append(adj_node)
        return ans
    
    # Create reversed graph
    graph={i:set() for i in nodes}
    for i,j in edges:
        graph[j].add(i)  # reverse edges: i->j becomes j->i
    
    topoSort=topologicalSort(nodes,graph)
    topoSort.sort()  # Return in ascending order
    return topoSort
```

### Time and Space Complexity
- **Time Complexity:** O(V + E) for topological sort + O(V log V) for sorting = O(V + E + V log V)
- **Space Complexity:** O(V + E) for the reversed graph and auxiliary data structures

## Solution 2: Using Depth First Search (DFS)

### Intuition
Think of it like exploring a haunted house with multiple rooms. You want to find rooms that are "safe" - meaning no matter which door you take from that room, you'll eventually reach an exit (terminal node) and won't get trapped in a loop of scary rooms.

The key insight: A room is unsafe if you can get stuck in a loop of rooms (cycle) or if any path from that room leads to such a loop. We explore each room and mark it as safe only if ALL paths from it lead to exits, not to loops.

### Approach Steps
1. **Track three states for each node**:
   - `visited`: Have we explored this node before?
   - `pathVisited`: Is this node part of our current exploration path?
   - `safe`: Is this node confirmed to be safe?
2. **For each unvisited node, perform DFS**:
   - Mark current node as visited and add to current path
   - Assume current node is unsafe initially
   - Explore all adjacent nodes
   - If any adjacent node leads to a cycle, current node is unsafe
   - If all paths are safe, mark current node as safe
   - Remove current node from path (backtrack)
3. **Return all safe nodes in sorted order**

### Code
```python
def solution(nodes, adj_list):
    graph = adj_list
    visited = {i: False for i in nodes}  # Track visited nodes
    pathVisited = {i: False for i in nodes}  # Track nodes in current path
    safe = {i: False for i in nodes}  # Track safe nodes
    
    def dfs(node):
        visited[node] = True  # Mark as visited
        pathVisited[node] = True  # Add to current path
        
        for adj_node in graph[node]:  # Explore all adjacent nodes
            if not visited[adj_node]:  # If not visited, explore recursively
                if not dfs(adj_node):  # If adjacent node is unsafe
                    pathVisited[node] = False  # Remove from path
                    return False  # Current node is also unsafe
            elif pathVisited[adj_node]:  # If in current path, cycle detected
                pathVisited[node] = False  # Remove from path
                return False  # Current node is unsafe
            elif not safe[adj_node]:  # If adjacent node is known to be unsafe
                pathVisited[node] = False  # Remove from path
                return False  # Current node is also unsafe
        
        pathVisited[node] = False  # Remove from current path (backtrack)
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

### Time and Space Complexity
- **Time Complexity:** O(V + E) where V is the number of vertices and E is the number of edges
- **Space Complexity:** O(V) for the visited arrays and recursion stack

---

# Course Schedule I

## Problem Statement
There are a total of N tasks, labeled from 0 to N-1. Given an array arr where arr[i] = [a, b] indicates that you must take course b first if you want to take course a. Find if it is possible to finish all tasks.

## Examples

### Example 1:
**Input:** N = 4, arr = [[1,0],[2,1],[3,2]]
**Output:** True

**Explanation:** It is possible to finish all the tasks in the order: 0 1 2 3.
First, we will finish task 0. Then we will finish task 1, task 2, and task 3.

### Example 2:
**Input:** N = 4, arr = [[0,1],[3,2],[1,3],[3,0]]
**Output:** False

**Explanation:** It is impossible to finish all the tasks. Let's analyze the pairs:
- For pair {0, 1} -> we need to finish task 1 first and then task 0. (order: 1 0).
- For pair {3, 2} -> we need to finish task 2 first and then task 3. (order: 2 3).
- For pair {1, 3} -> we need to finish task 3 first and then task 1. (order: 3 1).
- But for pair {3, 0} -> we need to finish task 0 first and then task 3 but task 0 requires task 1 and task 1 requires task 3. So, it is not possible to finish all the tasks.

## How this problem can be identified as a Graph problem?
The problem suggests that some courses must be completed before other courses. This is analogous to Topological Sort Algorithm in graph which helps to find a ordering where a node must come before other nodes in the ordering.

Hence, the courses can be represented as nodes of graphs and dependencies of courses can be shown as edges.

Now, For the graph formed, if the Topological sort can be found containing all the nodes (courses), all the courses can be completed in the order returned by topological sort. Else it is not possible to complete all the courses.

## How to form the graph?
The pair [a,b] represents that the Course b must be completed before Course a.

Hence in the graph, two nodes representing Course a and b can be created with a directed edge from Node b to Node a. This way the topological sort will return Node b before Node a.

## Solution: Using Topological Sort (Kahn's Algorithm)

### Intuition
Think of it like planning your college courses where some courses have prerequisites. You can graduate (complete all courses) if and only if you can arrange all your courses in a valid order where you never take a course before completing its prerequisites.

The key insight: If there's a circular dependency (like "Course A needs Course B, Course B needs Course C, Course C needs Course A"), then it's impossible to complete all courses - you'll be stuck in an endless loop of prerequisites!

We use topological sort to try arranging all courses in a valid order. If we can arrange all N courses, then it's possible to complete them all. If some courses get stuck due to circular dependencies, it's impossible.

### Approach Steps
1. **Build the graph**: Create a directed graph where each course is a node, and there's an edge from prerequisite to course (b → a for dependency [a,b])
2. **Apply topological sort using Kahn's algorithm**:
   - Calculate in-degree for each course (how many prerequisites it has)
   - Start with courses that have no prerequisites (in-degree = 0)
   - Process courses one by one, removing them and updating prerequisites count
   - Keep track of how many courses we successfully processed
3. **Check if all courses can be completed**:
   - If we processed all N courses, return True (possible to complete all)
   - If some courses remain unprocessed, return False (circular dependency exists)

### Code
```python
from collections import deque
def solution(nodes,edges):
    def topologicalSort(nodes,graph):
        # Calculate in-degree for each node (number of prerequisites)
        inDegree={i:0 for i in nodes}
        for node,adj_nodes in graph.items():
            for i in adj_nodes:
                inDegree[i]+=1
        
        # Add all nodes with in-degree 0 to queue (no prerequisites)
        queue=deque()
        for node,degree in inDegree.items():
            if(degree==0):
                queue.append(node)
        
        ans=[]
        while queue:
            node=queue.popleft()  # Take a course with no remaining prerequisites
            ans.append(node)
            
            # Remove this course as prerequisite for other courses
            for adj_node in graph[node]:
                inDegree[adj_node]-=1
                if(inDegree[adj_node]==0):  # If all prerequisites completed
                    queue.append(adj_node)
        return ans
    
    # Build the graph: for [a,b], add edge b -> a
    graph={i:set() for i in nodes}
    for i,j in edges:
        graph[j].add(i)  # j must come before i
    
    topoSort=topologicalSort(nodes,graph)
    if(len(topoSort)==len(nodes)):  # If all courses can be arranged
        return True
    return False  # If some courses stuck due to circular dependencies
```

```python
n=4
edges= [[1,0],[2,1],[3,2]]
nodes=[i for i in range(n)]
print(solution(nodes,edges))
```

### Time and Space Complexity
- **Time Complexity:** O(V + E) where V is the number of courses and E is the number of prerequisite relationships
- **Space Complexity:** O(V + E) for the graph representation and auxiliary data structures

---

# Course Schedule II

## Problem Statement
There are a total of N tasks, labeled from 0 to N-1. Given an array arr where arr[i] = [a, b] indicates that you must take course b first if you want to take course a. Find the order of tasks you should pick to finish all tasks.
If no such ordering exists, return an empty array.

## Examples

### Example 1:
**Input:** N = 4, arr = [[1,0],[2,1],[3,2]]
**Output:** [0, 1, 2, 3]

**Explanation:** First, finish task 0, as it has no prerequisites. Then, finish task 1, since it depends only on task 0. After that, finish task 2, since it depends only on task 1. Finally, finish task 3, since it depends only on task 2.

### Example 2:
**Input:** N = 4, arr = [[0,1],[3,2],[1,3],[3,0]]
**Output:** []

**Explanation:** It is impossible to finish all the tasks. Let's analyze the pairs:
- For pair {0, 1} → we need to finish task 1 first and then task 0 (order: 1 → 0).
- For pair {3, 2} → we need to finish task 2 first and then task 3 (order: 2 → 3).
- For pair {1, 3} → we need to finish task 3 first and then task 1 (order: 2 → 3 → 1 → 0).
- But for pair {3, 0} → we need to finish task 0 first and then task 3, which contradicts the previous order. So, it is not possible to finish all the tasks.

## Solution: Using Topological Sort (Kahn's Algorithm)

### Intuition
Think of it like creating a step-by-step study plan for your entire college curriculum. You need to figure out the exact order in which to take all your courses so that you never take a course before completing its prerequisites.

This is exactly like Course Schedule I, but now instead of just asking "Can I graduate?", we're asking "Give me the exact semester-by-semester plan to graduate!"

The approach is simple:
1. Start with courses that have no prerequisites (you can take them right away)
2. Take those courses and "unlock" the next set of courses 
3. Keep going until you've planned all courses, or get stuck due to circular dependencies

If you can plan all N courses, return the plan. If you get stuck (due to circular dependencies), return an empty plan to indicate it's impossible.

### Approach Steps
1. **Build the graph**: Create a directed graph where each course is a node, and there's an edge from prerequisite to course (b → a for dependency [a,b])
2. **Apply topological sort using Kahn's algorithm**:
   - Calculate in-degree for each course (how many prerequisites it has)
   - Start with courses that have no prerequisites (in-degree = 0)
   - Process courses one by one, adding them to our study plan
   - For each course taken, reduce the prerequisite count of dependent courses
   - Add newly available courses (those with no remaining prerequisites) to the queue
3. **Return the result**:
   - If we planned all N courses, return the complete study plan
   - If some courses couldn't be planned (due to circular dependencies), return empty array

### Code
```python
from collections import deque
def solution(nodes,edges):
    def topologicalSort(nodes,graph):
        # Calculate in-degree for each node (number of prerequisites)
        inDegree={i:0 for i in nodes}
        for node,adj_nodes in graph.items():
            for i in adj_nodes:
                inDegree[i]+=1
        
        # Add all nodes with in-degree 0 to queue (no prerequisites)
        queue=deque()
        for node,degree in inDegree.items():
            if(degree==0):
                queue.append(node)
        
        ans=[]
        while queue:
            node=queue.popleft()  # Take next course with no remaining prerequisites
            ans.append(node)  # Add to our study plan
            
            # Remove this course as prerequisite for other courses
            for adj_node in graph[node]:
                inDegree[adj_node]-=1
                if(inDegree[adj_node]==0):  # If all prerequisites completed
                    queue.append(adj_node)  # Course becomes available
        return ans
    
    # Build the graph: for [a,b], add edge b -> a
    graph={i:set() for i in nodes}
    for i,j in edges:
        graph[j].add(i)  # j must come before i
    
    topoSort=topologicalSort(nodes,graph)
    if(len(topoSort)==len(nodes)):  # If all courses can be planned
        return topoSort  # Return the complete study plan
    return []  # Return empty array if impossible due to circular dependencies
```

## Test Case
```python
n=4
edges= [[1,0],[2,1],[3,2]]
nodes=[i for i in range(n)]
print(solution(nodes,edges))
```

### Time and Space Complexity
- **Time Complexity:** O(V + E) where V is the number of courses and E is the number of prerequisite relationships
- **Space Complexity:** O(V + E) for the graph representation and auxiliary data structures

---

# Alien Dictionary

## Problem Description

Given a sorted dictionary of an alien language having `N` words and `K` starting alphabets of a standard dictionary, find the order of characters in the alien language.

There may be multiple valid orders for a particular test case, thus you may return any valid order as a string. The output will be `True` if the order returned by the function is correct, else `False` denoting an incorrect order. If the given arrangement of words is inconsistent with any possible letter ordering, return an empty string `""`.

**Notes on constraints:**
- The dictionary is sorted according to the alien language's alphabetical order
- We need to deduce the character ordering from the sorted word list
- Multiple valid orderings may exist for the same input
- If the input is inconsistent (contains a cycle), return an empty string

---

## Sample Test Cases With Explanation

### Test Case 1
```
Input: N = 5, K = 4, dict = ["baa","abcd","abca","cab","cad"]
Output: b d a c
```
**Explanation:** We analyze every consecutive pair to find the order of characters:
- Pair "baa" and "abcd": First differing character is 'b' vs 'a' → 'b' comes before 'a'
- Pair "abcd" and "abca": First differing character is 'd' vs 'a' → 'd' comes before 'a'
- Pair "abca" and "cab": First differing character is 'a' vs 'c' → 'a' comes before 'c'
- Pair "cab" and "cad": First differing character is 'b' vs 'd' → 'b' comes before 'd'

So, ['b', 'd', 'a', 'c'] is a valid ordering.

### Test Case 2
```
Input: N = 3, K = 3, dict = ["caa","aaa","aab"]
Output: c a b
```
**Explanation:** Analyzing consecutive pairs:
- Pair "caa" and "aaa": First differing character is 'c' vs 'a' → 'c' comes before 'a'
- Pair "aaa" and "aab": First differing character is 'a' vs 'b' → 'a' comes before 'b'

So, ['c', 'a', 'b'] is a valid ordering.

---

## Approaches

### Approach 1: Topological Sort Using Kahn's Algorithm (BFS)

**Summary:** Build a directed graph from character dependencies extracted from consecutive word pairs, then perform topological sort to find the character ordering.

---

**Intuition**

The key insight is that when two consecutive words in the sorted dictionary differ at position `i`, the character at position `i` in the first word must come before the character at position `i` in the second word in the alien alphabet.

For example:
- If "baa" comes before "abcd", comparing character by character: 'b' vs 'a' → 'b' comes before 'a'
- If "abcd" comes before "abca", comparing: 'a'='a', 'b'='b', 'c'='c', 'd' vs 'a' → 'd' comes before 'a'

This creates a directed graph where an edge from 'x' to 'y' means 'x' comes before 'y'. The problem becomes finding a topological ordering of this graph.

We use Kahn's algorithm (BFS-based topological sort):
1. Build the graph and calculate in-degrees
2. Start with nodes having in-degree 0 (no dependencies)
3. Process nodes in order, reducing in-degrees of neighbors
4. The processing order gives us the character ordering

---

**Approach Steps**

1. **Extract character relationships (edges):**
   - Compare each consecutive pair of words
   - Find the first position where characters differ
   - Add an edge from the character in the first word to the character in the second word
   - Example: "baa" vs "abcd" → edge from 'b' to 'a'

2. **Build the directed graph:**
   - Create nodes for all K characters (first K letters of the alphabet)
   - Create adjacency list from the extracted edges
   - Calculate in-degree for each node (number of incoming edges)

3. **Perform topological sort using Kahn's algorithm:**
   - Initialize a queue with all nodes having in-degree 0
   - While queue is not empty:
     - Dequeue a node and add it to the result
     - For each neighbor of this node:
       - Decrease its in-degree by 1
       - If in-degree becomes 0, add it to the queue

4. **Return the result:**
   - Join the characters in the result list to form the ordering string
   - If the result doesn't contain all K characters, there's a cycle (inconsistent input)

---

**Example**

Consider `dict = ["baa","abcd","abca","cab","cad"]` and `K = 4`:

**Step 1:** Extract edges from consecutive pairs

- **Pair 1:** "baa" vs "abcd"
  - Position 0: 'b' ≠ 'a' → Edge: b → a
  
- **Pair 2:** "abcd" vs "abca"
  - Position 0: 'a' = 'a', continue
  - Position 1: 'b' = 'b', continue
  - Position 2: 'c' = 'c', continue
  - Position 3: 'd' ≠ 'a' → Edge: d → a
  
- **Pair 3:** "abca" vs "cab"
  - Position 0: 'a' ≠ 'c' → Edge: a → c
  
- **Pair 4:** "cab" vs "cad"
  - Position 0: 'c' = 'c', continue
  - Position 1: 'a' = 'a', continue
  - Position 2: 'b' ≠ 'd' → Edge: b → d

**Edges:** [b→a, d→a, a→c, b→d]

**Step 2:** Build graph and calculate in-degrees

- **Graph:**
  ```
  a: [c]
  b: [a, d]
  c: []
  d: [a]
  ```

- **In-degrees:**
  ```
  a: 2 (from b and d)
  b: 0
  c: 1 (from a)
  d: 1 (from b)
  ```

**Step 3:** Topological sort

- **Initial queue:** [b] (only node with in-degree 0)

- **Iteration 1:**
  - Dequeue: b
  - Result: [b]
  - Process neighbors: a, d
  - Update in-degrees: a=1, d=0
  - Queue: [d]

- **Iteration 2:**
  - Dequeue: d
  - Result: [b, d]
  - Process neighbors: a
  - Update in-degrees: a=0
  - Queue: [a]

- **Iteration 3:**
  - Dequeue: a
  - Result: [b, d, a]
  - Process neighbors: c
  - Update in-degrees: c=0
  - Queue: [c]

- **Iteration 4:**
  - Dequeue: c
  - Result: [b, d, a, c]
  - No neighbors
  - Queue: []

**Answer = "bdac"**

---

**Code**

```python
def getEdges(n, arr):
    # Extract character ordering relationships from consecutive word pairs
    edges = []
    for i in range(1, n):
        a = arr[i - 1]
        b = arr[i]
        
        # Compare characters until we find the first difference
        for j in range(min(len(a), len(b))):
            if(a[j] != b[j]):
                # First differing character gives us the ordering
                edges.append([a[j], b[j]])
                break
    return edges

def topoSort(nodes, edges, graph):
    # Calculate in-degree for each node
    inDegree = {i: 0 for i in nodes}
    for i, j in edges:
        inDegree[j] += 1
    
    # Initialize queue with nodes having in-degree 0
    queue = []
    for node, degree in inDegree.items():
        if(degree == 0):
            queue.append(node)
    
    # Perform BFS-based topological sort (Kahn's algorithm)
    ans = []
    while queue:
        # Process node with no remaining dependencies
        node = queue.pop(0)
        ans.append(node)
        
        # Reduce in-degree of neighbors
        for adj in graph[node]:
            inDegree[adj] -= 1
            # If in-degree becomes 0, add to queue
            if(inDegree[adj] == 0):
                queue.append(adj)
    
    return ans

def solve(n, k, arr):
    # Create nodes for first K characters
    nodes = [chr(ord('a') + i) for i in range(k)]
    
    # Extract edges from consecutive word pairs
    edges = getEdges(n, arr)
    
    # Build adjacency list representation of the graph
    graph = {i: [] for i in nodes}
    for i, j in edges:
        graph[i].append(j)
    
    # Perform topological sort to get character ordering
    ans = topoSort(nodes, edges, graph)
    
    return "".join(ans)
    
n, k = 5, 4
arr = ["baa", "abcd", "abca", "cab", "cad"]
print(solve(n, k, arr))
```

---

**Time Complexity**

O(N × L + K) where:
- N is the number of words
- L is the average length of words
- K is the number of unique characters

**Breakdown:**
- Extracting edges: O(N × L) — comparing consecutive word pairs
- Building graph: O(E) where E is the number of edges (at most N-1)
- Topological sort: O(K + E) — visiting all nodes and edges once

---

**Space Complexity**

O(K + E) where:
- K is for storing nodes, in-degrees, and result
- E is for storing edges in the adjacency list

---
