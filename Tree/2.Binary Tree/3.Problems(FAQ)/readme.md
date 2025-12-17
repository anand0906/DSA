# 🌳 Binary Tree Traversal Problems

A comprehensive guide to various binary tree traversal techniques with detailed explanations, multiple approaches, and complexity analysis.

---

## 📑 Table of Contents

1. [Zig Zag (Spiral) Traversal](#1-zig-zag-spiral-traversal)
2. [Boundary Traversal](#2-boundary-traversal)
3. [Vertical Order Traversal](#3-vertical-order-traversal)
4. [Top View of Binary Tree](#4-top-view-of-binary-tree)
5. [Bottom View of Binary Tree](#5-bottom-view-of-binary-tree)
6. [Right/Left View of Binary Tree](#6-rightleft-view-of-binary-tree)
7. [Print Root to Node Path](#7-print-root-to-node-path)
8. [Print All Nodes at Distance K](#8-print-all-nodes-at-distance-k)
9. [Minimum Time to Burn Tree](#9-minimum-time-to-burn-tree)
10. [Maximum Width of Binary Tree](#10-maximum-width-of-binary-tree)
11. [Count Nodes in Complete Binary Tree](#11-count-nodes-in-complete-binary-tree)
12. [Lowest Common Ancestor (LCA)](#12-lowest-common-ancestor-lca)

---

## 1. Zig Zag (Spiral) Traversal

### 📌 Problem Statement

Given the root of a binary tree, return the **zigzag level order traversal** of its nodes' values. This means traversing from left to right, then right to left for the next level, and alternating between these directions.

### 🎯 Examples

#### Example 1
```
Input: root = [1, 2, 3, null, 4, 8, 5]
Output: [[1], [3, 2], [4, 8, 5]]

Tree Structure:
        1
       / \
      2   3
       \ / \
       4 8  5

Explanation:
Level 0: [1]         → Left to Right
Level 1: [3, 2]      → Right to Left
Level 2: [4, 8, 5]   → Left to Right
```

#### Example 2
```
Input: root = [3, 9, 20, null, null, 15, 7]
Output: [[3], [20, 9], [15, 7]]

Tree Structure:
        3
       / \
      9   20
         /  \
        15   7

Explanation:
Level 0: [3]       → Left to Right
Level 1: [20, 9]   → Right to Left
Level 2: [15, 7]   → Left to Right
```

---

### 🔍 Approach 1: Level Order with Reverse

#### Intuition
- Perform standard level-order traversal (BFS)
- Collect nodes at each level normally
- After collecting each level, reverse the array if needed
- Use a boolean flag to track when to reverse

#### Approach
1. Initialize queue with root and a `reverse` flag as `False`
2. For each level:
   - Process all nodes at current level
   - Add their values to `order` array
   - Add children to queue
3. If `reverse` is `True`, reverse the `order` array
4. Toggle the `reverse` flag for next level
5. Add the level array to result

#### Code
```python
def solve(root):
    ans = []                    # [0] Result array to store all levels
    queue = [root]              # [1] Queue for BFS traversal
    reverse = False             # [2] Flag to track reversal direction
    
    while queue:
        n = len(queue)          # [3] Number of nodes at current level
        order = []              # [4] Array to store current level nodes
        
        for _ in range(n):      # [5] Process all nodes at current level
            node = queue.pop(0) # [6] Dequeue front node
            order.append(node.val)  # [7] Add node value to current level
            
            # [8] Add children to queue for next level
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        # [9] Reverse if needed
        if reverse:
            order.reverse()
            reverse = False
        else:
            reverse = True
        
        ans.append(order)       # [10] Add current level to result
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Visit each node once
- **Space Complexity:** `O(N)` - Queue can hold up to N/2 nodes at the last level

---

### 🔍 Approach 2: Direct Placement (Optimized)

#### Intuition
- Instead of reversing arrays, place values directly at correct positions
- Pre-allocate array of size `n` for each level
- Use index calculation to place values:
  - Normal order: place at index `i`
  - Reverse order: place at index `n-i-1`

#### Approach
1. Initialize queue and `reverse` flag
2. For each level:
   - Create array of fixed size `n`
   - For each node at index `i`:
     - If `reverse`: place at `order[n-i-1]`
     - Else: place at `order[i]`
3. Toggle `reverse` flag
4. No need for array reversal

#### Code
```python
def solve(root):
    ans = []                    # [0] Result array
    queue = [root]              # [1] Queue for BFS
    reverse = False             # [2] Direction flag
    
    while queue:
        n = len(queue)          # [3] Current level size
        order = [0] * n         # [4] Pre-allocate array with size n
        
        for i in range(n):      # [5] Process each node
            node = queue.pop(0) # [6] Dequeue node
            
            # [7] Place value at correct position based on direction
            if reverse:
                order[n - i - 1] = node.val  # Reverse position
            else:
                order[i] = node.val          # Normal position
            
            # [8] Add children for next level
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        reverse = not reverse   # [9] Toggle direction
        ans.append(order)       # [10] Add level to result
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Visit each node once, no reversal needed
- **Space Complexity:** `O(N)` - Queue space

---

## 2. Boundary Traversal

### 📌 Problem Statement

Perform the **boundary traversal** of a binary tree in anticlockwise direction starting from the root. The boundary includes:
1. Root node
2. Left boundary (excluding leaf nodes)
3. All leaf nodes (left to right)
4. Right boundary in reverse (excluding leaf nodes)

### 🎯 Examples

#### Example 1
```
Input: root = [1, 2, 3, 4, 5, 6, 7, null, null, 8, 9]
Output: [1, 2, 4, 8, 9, 6, 7, 3]

Tree Structure:
          1
        /   \
       2     3
      / \   / \
     4   5 6   7
        / \
       8   9

Boundary Path (Anticlockwise):
1 (root) → 2 (left boundary) → 4 (left boundary) 
→ 8, 9 (leaves) → 6, 7 (leaves) → 3 (right boundary)
```

#### Example 2
```
Input: root = [1, 2, null, 4, 9, 6, 5, 3, null, null, null, null, null, 7, 8]
Output: [1, 2, 4, 6, 5, 7, 8]

Explanation: Traverses anticlockwise collecting boundary nodes
```

---

### 🔍 Approach: Divide and Conquer

#### Intuition
- Split the problem into three parts:
  1. **Left Boundary:** Traverse left side, skip leaves
  2. **Leaf Nodes:** Collect all leaves (left to right)
  3. **Right Boundary:** Traverse right side, skip leaves, then reverse
- A leaf node has no left and right children

#### Approach
1. Add root to result (if exists)
2. Traverse left boundary (skip leaves)
3. Add all leaf nodes using DFS
4. Traverse right boundary and reverse it

#### Code
```python
def isLeafNode(node):
    """[0] Check if node is a leaf (no children)"""
    return node.left is None and node.right is None

def leftBoundary(node):
    """[1] Collect left boundary excluding leaves"""
    order = []
    temp = node
    
    while temp:
        # [2] Add only non-leaf nodes
        if not isLeafNode(temp):
            order.append(temp.data)
        
        # [3] Prefer left child, then right
        if temp.left:
            temp = temp.left
        else:
            temp = temp.right
    
    return order

def leafNodes(node, ans):
    """[4] Collect all leaf nodes using DFS"""
    if not node:
        return
    
    # [5] Recursively traverse left subtree
    if node.left:
        leafNodes(node.left, ans)
    
    # [6] Recursively traverse right subtree
    if node.right:
        leafNodes(node.right, ans)
    
    # [7] Add leaf node
    if isLeafNode(node):
        ans.append(node.data)

def rightBoundary(node):
    """[8] Collect right boundary excluding leaves"""
    order = []
    temp = node
    
    while temp:
        # [9] Add only non-leaf nodes
        if not isLeafNode(temp):
            order.append(temp.data)
        
        # [10] Prefer right child, then left
        if temp.right:
            temp = temp.right
        else:
            temp = temp.left
    
    return order

def solve(root):
    ans = []
    
    if not root:
        return ans
    
    # [11] Add root
    if root:
        ans.append(root.data)
    
    # [12] Add left boundary (if left child exists)
    if root.left:
        ans = ans + leftBoundary(root.left)
    
    # [13] Add all leaf nodes
    if root.left or root.right:
        leafNodes(root, ans)
    
    # [14] Add right boundary in reverse (if right child exists)
    if root.right:
        ans = ans + rightBoundary(root.right)[::-1]
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Visit each node once
- **Space Complexity:** `O(H)` - Recursion stack for leaf traversal, where H is height

---

## 3. Vertical Order Traversal

### 📌 Problem Statement

Return the **vertical order traversal** of a binary tree. Nodes are positioned at coordinates:
- Root at `(row=0, col=0)`
- Left child at `(row+1, col-1)`
- Right child at `(row+1, col+1)`

Return nodes column by column (left to right). If multiple nodes have same position, sort by value.

### 🎯 Examples

#### Example 1
```
Input: root = [3, 9, 20, null, null, 15, 7]
Output: [[9], [3, 15], [20], [7]]

Tree Structure with Coordinates:
         3(0,0)
        /      \
    9(-1,1)   20(1,1)
              /      \
          15(0,2)   7(2,2)

Columns:
-1: [9]
 0: [3, 15]
 1: [20]
 2: [7]
```

#### Example 2
```
Input: root = [1, 2, 3, 4, 5, 6, 7]
Output: [[4], [2], [1, 5, 6], [3], [7]]

Explanation:
Column -2: [4]
Column -1: [2]
Column  0: [1, 5, 6] (sorted by value at same position)
Column  1: [3]
Column  2: [7]
```

---

### 🔍 Approach: BFS with Coordinate Mapping

#### Intuition
- Use BFS to traverse level by level
- Track each node's column (`x`) and row (`y`) coordinates
- Group nodes by column, then by row
- Sort nodes with same position by value

#### Approach
1. Use queue to store `(node, x, y)` tuples
2. Use nested dictionary: `map[column][row] = [values]`
3. Process columns from left to right
4. Within each column, process rows top to bottom
5. Sort values at same position

#### Code
```python
from collections import defaultdict

def solve(root):
    queue = [(root, 0, 0)]      # [0] (node, x-coordinate, y-coordinate)
    
    # [1] Nested dictionary: column -> row -> list of values
    map = defaultdict(lambda: defaultdict(list))
    
    while queue:
        node, x, y = queue.pop(0)  # [2] Dequeue node with coordinates
        
        # [3] Add node value to its (x, y) position
        map[x][y].append(node.data)
        
        # [4] Add children with updated coordinates
        if node.left:
            queue.append((node.left, x - 1, y - 1))  # Left: x-1, y-1
        if node.right:
            queue.append((node.right, x + 1, y - 1))  # Right: x+1, y-1
    
    order = []
    
    # [5] Process columns from left to right
    for i in sorted(map):
        temp = []
        
        # [6] Process rows from top to bottom (y descending)
        for j in sorted(map[i], reverse=True):
            # [7] Sort values at same position and add to column
            temp.extend(sorted(map[i][j]))
        
        order.append(temp)  # [8] Add column to result
    
    return order
```

#### Complexity Analysis
- **Time Complexity:** `O(N log N)` - N nodes + sorting nodes at same position
- **Space Complexity:** `O(N)` - Queue and map storage

---

## 4. Top View of Binary Tree

### 📌 Problem Statement

Return the **top view** of a binary tree - the set of nodes visible when viewing the tree from the top. Return nodes from leftmost to rightmost position.

### 🎯 Examples

#### Example 1
```
Input: root = [1, 2, 3, 4, 5, 6, 7]
Output: [4, 2, 1, 3, 7]

Tree Structure:
          1
        /   \
       2     3
      / \   / \
     4   5 6   7

Top View (looking from above):
4 (column -2)
2 (column -1)
1 (column 0)
3 (column 1)
7 (column 2)

Node 5, 6 are hidden behind 1
```

#### Example 2
```
Input: root = [10, 20, 30, 40, 60, 90, 100]
Output: [40, 20, 10, 30, 100]
```

---

### 🔍 Approach 1: Using Nested Dictionary

#### Intuition
- Track column positions of all nodes
- For each column, keep only the first node encountered (topmost)
- Use BFS to ensure we see nodes level by level

#### Approach
1. Traverse tree with BFS, tracking (x, y) coordinates
2. For each column, store all nodes with their levels
3. For top view, take the node with minimum y (highest level)

#### Code
```python
from collections import defaultdict

def solve(root):
    if not root:
        return []
    
    queue = [(root, 0, 0)]      # [0] (node, column, level)
    
    # [1] Dictionary: column -> level -> list of values
    map = defaultdict(lambda: defaultdict(list))
    
    while queue:
        node, x, y = queue.pop(0)  # [2] Dequeue node
        
        # [3] Store node at its column and level
        map[x][y].append(node.data)
        
        # [4] Add children with updated positions
        if node.left:
            queue.append((node.left, x - 1, y + 1))   # Left child
        if node.right:
            queue.append((node.right, x + 1, y + 1))  # Right child
    
    ans = []
    
    # [5] Process columns left to right
    for x in sorted(map.keys()):
        # [6] Get minimum level (top node) in this column
        key = sorted(map[x].keys())[0]
        # [7] Add first node from top level
        ans.append(map[x][key][0])
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** `O(N log N)` - BFS + sorting columns
- **Space Complexity:** `O(N)` - Dictionary storage

---

### 🔍 Approach 2: Optimized with Simple Dictionary

#### Intuition
- We only need the first node in each column
- Use simple dictionary, add only if column not seen before
- BFS guarantees we see top nodes first

#### Approach
1. Use BFS with column tracking
2. Store only first occurrence of each column
3. No need for nested structure

#### Code
```python
def solve(root):
    if not root:
        return []
    
    queue = [(root, 0, 0)]      # [0] (node, column, level)
    map = {}                     # [1] Simple dictionary: column -> value
    
    while queue:
        node, x, y = queue.pop(0)  # [2] Dequeue node
        
        # [3] Add node only if column not seen before (first = top)
        if x not in map:
            map[x] = node.data
        
        # [4] Add children with updated positions
        if node.left:
            queue.append((node.left, x - 1, y + 1))
        if node.right:
            queue.append((node.right, x + 1, y + 1))
    
    ans = []
    
    # [5] Collect values from left to right columns
    for key in sorted(map):
        ans.append(map[key])
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** `O(N log N)` - BFS + sorting columns
- **Space Complexity:** `O(W)` - Width of tree for dictionary

---

## 5. Bottom View of Binary Tree

### 📌 Problem Statement

Return the **bottom view** of a binary tree - nodes visible when viewing from the bottom. For nodes at same column, keep the one that appears later in level-order traversal.

### 🎯 Examples

#### Example 1
```
Input: root = [20, 8, 22, 5, 3, null, 25, null, null, 10, 14]
Output: [5, 10, 3, 14, 25]

Tree Structure:
           20
          /  \
         8    22
        / \     \
       5   3    25
          / \
         10 14

Bottom View (column by column):
-2: 5
-1: 10 (overwrites 8)
 0: 3 (overwrites 20)
 1: 14 (overwrites 22)
 2: 25
```

#### Example 2
```
Input: root = [20, 8, 22, 5, 3, 4, 25, null, null, 10, 14]
Output: [5, 10, 4, 14, 25]

Explanation: Node 4 appears later than 3, so it's visible from bottom
```

---

### 🔍 Approach: Overwriting Strategy

#### Intuition
- Similar to top view, but keep updating values
- BFS ensures level-order traversal
- Last value for each column = bottom view
- Keep overwriting as we go deeper

#### Approach
1. Use BFS with column tracking
2. Keep updating value for each column (overwrite)
3. Last update = bottommost node
4. Return columns left to right

#### Code
```python
def solve(root):
    if not root:
        return []
    
    queue = [(root, 0, 0)]      # [0] (node, column, level)
    map = {}                     # [1] Dictionary: column -> value
    
    while queue:
        node, x, y = queue.pop(0)  # [2] Dequeue node
        
        # [3] Keep updating - last update will be bottom node
        map[x] = node.data
        
        # [4] Add children with updated positions
        if node.left:
            queue.append((node.left, x - 1, y + 1))
        if node.right:
            queue.append((node.right, x + 1, y + 1))
    
    ans = []
    
    # [5] Collect values from left to right columns
    for key in sorted(map):
        ans.append(map[key])
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** `O(N log N)` - BFS + sorting columns
- **Space Complexity:** `O(W)` - Width of tree

---

## 6. Right/Left View of Binary Tree

### 📌 Problem Statement

Return the **right view** of a binary tree - values of nodes visible when viewing from the right side, arranged top to bottom. (Same approach works for left view)

### 🎯 Examples

#### Example 1
```
Input: root = [1, 2, 3, null, 5, null, 4]
Output: [1, 3, 4]

Tree Structure:
      1         ← Level 0: See 1
     / \
    2   3       ← Level 1: See 3 (rightmost)
     \   \
      5   4     ← Level 2: See 4 (rightmost)

Right View: [1, 3, 4]
```

#### Example 2
```
Input: root = [1, 2, 3, 6, 5, 8, 4]
Output: [1, 3, 4]
```

---

### 🔍 Approach: Level-wise Last Node

#### Intuition
- Rightmost node at each level = right view
- Use BFS and track levels
- Keep overwriting for each level (last update = rightmost)
- For left view, would take first node of each level

#### Approach
1. Use BFS with level tracking
2. Store one value per level
3. Keep updating - last node processed = rightmost
4. Return values level by level

#### Code
```python
def solve(root):
    if not root:
        return []
    
    queue = [(root, 0, 0)]      # [0] (node, column, level)
    map = {}                     # [1] Dictionary: level -> value
    
    while queue:
        node, x, y = queue.pop(0)  # [2] Dequeue node
        
        # [3] Keep updating for each level (last = rightmost)
        map[y] = node.data
        
        # [4] Add children to next level
        if node.left:
            queue.append((node.left, x - 1, y + 1))
        if node.right:
            queue.append((node.right, x + 1, y + 1))
    
    ans = []
    
    # [5] Collect values level by level (top to bottom)
    for key in sorted(map):
        ans.append(map[key])
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Visit each node once
- **Space Complexity:** `O(H)` - Height of tree for dictionary

---

## 7. Print Root to Node Path

### 📌 Problem Statement

Return all **root-to-leaf paths** in a binary tree. A leaf is a node with no children.

### 🎯 Examples

#### Example 1
```
Input: root = [1, 2, 3, null, 5, null, 4]
Output: [[1, 2, 5], [1, 3, 4]]

Tree Structure:
      1
     / \
    2   3
     \   \
      5   4

Paths:
1 → 2 → 5
1 → 3 → 4
```

#### Example 2
```
Input: root = [1, 2, 3, 4, 5]
Output: [[1, 2, 4], [1, 2, 5], [1, 3]]

Tree Structure:
      1
     / \
    2   3
   / \
  4   5

Paths:
1 → 2 → 4
1 → 2 → 5
1 → 3
```

---

### 🔍 Approach: DFS Backtracking

#### Intuition
- Use DFS to explore all paths
- Maintain current path while traversing
- When reaching leaf, save the complete path
- Pass path by value to avoid backtracking manually

#### Approach
1. Start DFS from root with empty path
2. Add current node to path
3. Recursively explore left and right subtrees
4. When leaf is found, add path to result
5. Path is passed by value, so no explicit backtracking needed

#### Code
```python
def solve(root):
    def dfs(node, path):
        """[0] DFS helper to explore all paths"""
        if not node:
            return
        
        # [1] Add current node to path (creates new list)
        path = path + [node.data]
        
        # [2] Recursively explore left subtree
        dfs(node.left, path)
        
        # [3] Recursively explore right subtree
        dfs(node.right, path)
        
        # [4] If leaf node, save the path
        if node.left is None and node.right is None:
            ans.append(path)
    
    ans = []              # [5] Result array for all paths
    dfs(root, [])         # [6] Start DFS with empty path
    return ans
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Visit each node once
- **Space Complexity:** `O(H × L)` - H for recursion depth, L for storing leaf paths

---

## 8. Print All Nodes at Distance K

### 📌 Problem Statement

Given a binary tree, a target node, and integer K, return all nodes that are **distance K** from the target node. Distance includes moving up through parent nodes.

### 🎯 Examples

#### Example 1
```
Input: root = [3, 5, 1, 6, 2, 0, 8, N, N, 7, 4], target = 5, k = 2
Output: [1, 4, 7]

Tree Structure:
         3
       /   \
      5     1
     / \   / \
    6   2 0   8
       / \
      7   4

From target 5:
- Distance 1: 3, 6, 2
- Distance 2: 1, 7, 4  ← Answer
```

#### Example 2
```
Input: root = [3, 5, 1, 6, 2, 0, 8, N, N, 7, 4], target = 5, k = 3
Output: [0, 8]

From target 5:
- Distance 3: 0, 8
```

---

### 🔍 Approach: BFS with Parent Pointers

#### Intuition
- Tree edges only allow downward movement
- Need parent pointers to move upward
- Problem becomes: BFS in undirected graph from target
- Use visited set to avoid cycles

#### Approach
1. First pass: Assign parent pointers to all nodes
2. Use BFS from target node
3. Explore in all 3 directions: left, right, parent
4. Track visited nodes to avoid cycles
5. Stop when distance = K

#### Code
```python
class TreeNode(object):
    def __init__(self, val=0, left=None, right=None):
        self.data = val
        self.left = left
        self.right = right
        self.parent = None      # [0] Add parent reference

def assignParent(root):
    """[1] Assign parent pointers to all nodes using BFS"""
    queue = [root]
    
    while queue:
        node = queue.pop(0)     # [2] Process node
        
        # [3] Set parent for left child
        if node.left:
            node.left.parent = node
            queue.append(node.left)
        
        # [4] Set parent for right child
        if node.right:
            node.right.parent = node
            queue.append(node.right)
    
    return root

def solve(root, target, k):
    assignParent(root)          # [5] First, assign all parent pointers
    
    queue = [target]            # [6] Start BFS from target
    visited = set()             # [7] Track visited nodes
    visited.add(target)
    level = 0                   # [8] Current distance from target
    
    while queue:
        if level == k:          # [9] Found nodes at distance K
            break
        
        n = len(queue)
        
        # [10] Process all nodes at current distance
        for _ in range(n):
            node = queue.pop(0)
            
            # [11] Explore left child
            if node.left and node.left not in visited:
                queue.append(node.left)
                visited.add(node.left)
            
            # [12] Explore right child
            if node.right and node.right not in visited:
                queue.append(node.right)
                visited.add(node.right)
            
            # [13] Explore parent (upward movement)
            if node.parent and node.parent not in visited:
                queue.append(node.parent)
                visited.add(node.parent)
        
        level += 1              # [14] Increment distance
    
    # [15] Extract values from nodes at distance K
    return [node.data for node in queue]
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Two passes: parent assignment + BFS
- **Space Complexity:** `O(N)` - Parent pointers + queue + visited set

---

## 9. Minimum Time to Burn Tree

### 📌 Problem Statement

Given a target node in a binary tree, if the target is set on fire, determine the **minimum time** needed to burn the entire tree. In 1 second, fire spreads to all connected nodes (left child, right child, parent).

### 🎯 Examples

#### Example 1
```
Input: root = [1, 2, 3, 4, null, 5, 6, null, 7], target = 1
Output: 3

Tree Structure:
         1 (target, t=0)
       /   \
      2     3         (t=1)
     /     / \
    4     5   6       (t=2)
     \
      7               (t=3)

Time progression:
t=0: Node 1 burns
t=1: Nodes 2, 3 burn
t=2: Nodes 4, 5, 6 burn
t=3: Node 7 burns
```

#### Example 2
```
Input: root = [1, 2, 3, null, 5, null, 4], target = 4
Output: 4

Fire spreads: 4 → 3 → 1 → 2 → 5
```

---

### 🔍 Approach: BFS from Target with Parent Pointers

#### Intuition
- Similar to "nodes at distance K" problem
- Need to spread fire in all directions: left, right, parent
- Use BFS to simulate burning level by level
- Each level = 1 unit of time
- Count levels until all nodes burned

#### Approach
1. Assign parent pointers and find target node
2. Use BFS from target with visited tracking
3. Process all nodes level by level
4. Each complete level = 1 second
5. Return total levels - 1

#### Code
```python
class TreeNode(object):
    def __init__(self, val=0, left=None, right=None):
        self.data = val
        self.left = left
        self.right = right
        self.parent = None      # [0] Parent reference for upward movement

def assignParent(root, start):
    """[1] Assign parent pointers and find target node"""
    queue = [root]
    targetNode = None           # [2] Reference to target node
    
    while queue:
        node = queue.pop(0)
        
        # [3] Check if this is the target node
        if node.data == start:
            targetNode = node
        
        # [4] Assign parent to children
        if node.left:
            node.left.parent = node
            queue.append(node.left)
        
        if node.right:
            node.right.parent = node
            queue.append(node.right)
    
    return targetNode           # [5] Return target node reference

def solve(root, start):
    target = assignParent(root, start)  # [6] Setup parents & find target
    
    queue = [target]            # [7] Start burning from target
    visited = set()             # [8] Track burned nodes
    visited.add(target)
    level = 0                   # [9] Time elapsed
    
    while queue:
        n = len(queue)
        
        # [10] Burn all nodes at current level (spread fire)
        for _ in range(n):
            node = queue.pop(0)
            
            # [11] Spread fire to left child
            if node.left and node.left not in visited:
                queue.append(node.left)
                visited.add(node.left)
            
            # [12] Spread fire to right child
            if node.right and node.right not in visited:
                queue.append(node.right)
                visited.add(node.right)
            
            # [13] Spread fire to parent (upward)
            if node.parent and node.parent not in visited:
                queue.append(node.parent)
                visited.add(node.parent)
        
        level += 1              # [14] One second passed
    
    return level - 1            # [15] Subtract 1 (initial level count)
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Two passes over all nodes
- **Space Complexity:** `O(N)` - Parent pointers + queue + visited set

---

## 10. Maximum Width of Binary Tree

### 📌 Problem Statement

Return the **maximum width** of a binary tree. Width of a level is the distance between leftmost and rightmost non-null nodes, counting null nodes that would exist in a complete binary tree.

### 🎯 Examples

#### Example 1
```
Input: root = [1, 3, 2, 5, 3, null, 9]
Output: 4

Tree Structure:
         1
       /   \
      3     2
     / \     \
    5   3     9

Level-wise indexing (0-based):
Level 0: [1] (index 0)
Level 1: [3, 2] (indices 0, 1)
Level 2: [5, 3, null, 9] (indices 0, 1, 2, 3)

Width = 3 - 0 + 1 = 4
```

#### Example 2
```
Input: root = [1, 3, 2, 5, null, null, 9, 6, null, 7]
Output: 7

Width calculation includes null positions in complete tree
```

---

### 🔍 Approach: Index-based BFS

#### Intuition
- Assign index to each node as if it were in a complete binary tree
- Left child of node at index `i` → `2*i + 1`
- Right child of node at index `i` → `2*i + 2`
- Width = last_index - first_index + 1
- Normalize indices at each level to prevent overflow

#### Approach
1. Use BFS with (node, index) pairs
2. For each level, track first and last indices
3. Calculate width = last - first + 1
4. Normalize indices by subtracting minimum to avoid overflow
5. Return maximum width seen

#### Code
```python
def solve(root):
    queue = [(root, 0)]         # [0] (node, index) pairs
    ans = 0                     # [1] Maximum width found
    
    while queue:
        n = len(queue)          # [2] Nodes at current level
        first, last = -1, -1    # [3] First and last indices
        mini = queue[0][1]      # [4] Minimum index for normalization
        
        for i in range(n):      # [5] Process all nodes at this level
            node, index = queue.pop(0)  # [6] Get node and its index
            
            # [7] Normalize index to prevent overflow
            index = index - mini
            
            # [8] Track first index of level
            if i == 0:
                first = index
            
            # [9] Track last index of level
            if i == n - 1:
                last = index
            
            # [10] Add children with their indices
            if node.left:
                queue.append((node.left, 2 * index + 1))
            
            if node.right:
                queue.append((node.right, 2 * index + 2))
        
        # [11] Calculate width for this level
        width = last - first + 1
        ans = max(ans, width)   # [12] Update maximum width
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Visit each node once
- **Space Complexity:** `O(W)` - Maximum width for queue storage

---

## 11. Count Nodes in Complete Binary Tree

### 📌 Problem Statement

Return the **number of nodes** in a complete binary tree. In a complete binary tree, every level except possibly the last is completely filled, and all nodes in the last level are as far left as possible.

### 🎯 Examples

#### Example 1
```
Input: root = [1, 2, 3, 4, 5, 6]
Output: 6

Tree Structure:
      1
     / \
    2   3
   / \ /
  4  5 6

Complete tree with 6 nodes
```

#### Example 2
```
Input: root = [1, 2, 3, 4, 5, 6, 7, 8, 9]
Output: 9

Tree Structure:
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
  / \
 8   9
```

---

### 🔍 Approach: Optimized Height-based Counting

#### Intuition
- Property of complete tree: left and right heights
- If left_height == right_height → perfect binary tree
  - Nodes = 2^height - 1
- Otherwise, recursively count left and right subtrees
- Much faster than counting every node

#### Approach
1. Find leftmost height (go only left)
2. Find rightmost height (go only right)
3. If equal → perfect tree → use formula
4. Otherwise → 1 + count(left) + count(right)

#### Code
```python
def findLeftHeight(node):
    """[0] Find height by going leftmost"""
    count = 0
    while node:
        count += 1
        node = node.left    # [1] Always go left
    return count

def findRightHeight(node):
    """[2] Find height by going rightmost"""
    count = 0
    while node:
        count += 1
        node = node.right   # [3] Always go right
    return count

def solve(node):
    if node is None:
        return 0            # [4] Base case: empty tree
    
    # [5] Calculate left and right heights
    left = findLeftHeight(node)
    right = findRightHeight(node)
    
    # [6] If heights equal, it's a perfect binary tree
    if left == right:
        return (2 ** left) - 1  # [7] Use formula: 2^h - 1
    
    # [8] Otherwise, recursively count both subtrees
    return 1 + solve(node.left) + solve(node.right)
```

#### Complexity Analysis
- **Time Complexity:** `O(log²N)` - At most log(N) recursive calls, each doing log(N) height calculation
- **Space Complexity:** `O(log N)` - Recursion stack depth

**Why O(log²N)?**
- Height finding: O(log N)
- Recursive calls: At most O(log N) levels
- Each level does O(log N) work
- Total: O(log N × log N) = O(log²N)

---

## 12. Lowest Common Ancestor (LCA)

### 📌 Problem Statement

Find the **lowest common ancestor** (LCA) of two nodes p and q in a binary tree. LCA is the lowest node that has both p and q as descendants (a node can be its own descendant).

### 🎯 Examples

#### Example 1
```
Input: root = [3, 5, 1, 6, 2, 0, 8, null, null, 7, 4], p = 5, q = 1
Output: 3

Tree Structure:
         3 ← LCA
       /   \
      5     1
     / \   / \
    6   2 0   8
       / \
      7   4

Explanation: LCA of 5 and 1 is 3
```

#### Example 2
```
Input: root = [3, 5, 1, 6, 2, 0, 8, null, null, 7, 4], p = 5, q = 4
Output: 5

Explanation: LCA of 5 and 4 is 5 (node can be ancestor of itself)
```

---

### 🔍 Approach 1: Parent Pointers with Ancestor Lists

#### Intuition
- Assign parent pointers to all nodes
- Trace path from p to root (ancestor list)
- Trace path from q to root
- First common ancestor in both lists = LCA

#### Approach
1. Build parent pointers for all nodes
2. Create ancestor list for node1 (from node to root)
3. Create ancestor list for node2
4. Find first common element in both lists

#### Code
```python
def assignParent(root):
    """[0] Assign parent pointers and create node lookup"""
    nodes = {}              # [1] Dictionary: value -> node
    queue = [root]
    
    while queue:
        node = queue.pop(0)
        nodes[node.data] = node  # [2] Store node reference
        
        # [3] Assign parent to children
        if node.left:
            node.left.parent = node
            queue.append(node.left)
        
        if node.right:
            node.right.parent = node
            queue.append(node.right)
    
    return nodes

def solve(root, node1, node2):
    nodes = assignParent(root)  # [4] Setup parent pointers
    root.parent = None          # [5] Root has no parent
    
    # [6] Build ancestor list for node1
    ancestors_1 = []
    temp = node1
    while temp:
        ancestors_1.append(temp.data)
        temp = temp.parent
    
    # [7] Build ancestor list for node2
    ancestors_2 = []
    temp = node2
    while temp:
        ancestors_2.append(temp.data)
        temp = temp.parent
    
    # [8] Find first common ancestor
    for i in ancestors_1:
        if i in ancestors_2:
            return nodes[i]     # [9] Return LCA node
    
    return -1                   # [10] No common ancestor (shouldn't happen)
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Parent assignment + ancestor tracing
- **Space Complexity:** `O(N)` - Parent pointers + ancestor lists

---

### 🔍 Approach 2: Recursive DFS (Optimal)

#### Intuition
- Recursively search for p and q in left and right subtrees
- If found p or q, return that node upward
- If both left and right return non-null → current node is LCA
- If only one side returns non-null → that's the LCA (or the path)

#### Approach
1. Base case: if node is null or equals p or q, return node
2. Recursively search in left subtree
3. Recursively search in right subtree
4. If both return non-null → current node is LCA
5. Otherwise, return whichever is non-null

#### Code
```python
def solve(node, p, q):
    # [0] Base cases
    if node is None or node == p or node == q:
        return node         # [1] Found target or reached null
    
    # [2] Search in left subtree
    left = solve(node.left, p, q)
    
    # [3] Search in right subtree
    right = solve(node.right, p, q)
    
    # [4] If both sides found something, current node is LCA
    if left and right:
        return node
    
    # [5] Return whichever side found something
    if left is None:
        return right        # [6] LCA is in right subtree
    
    if right is None:
        return left         # [7] LCA is in left subtree
```

#### Complexity Analysis
- **Time Complexity:** `O(N)` - Visit each node once in worst case
- **Space Complexity:** `O(H)` - Recursion stack, where H is height

**Key Insight:** This approach elegantly handles all cases:
- Both nodes in different subtrees → return current
- Both nodes in same subtree → that subtree returns LCA
- One node is ancestor of other → first found returns upward

---

## 📊 Summary Table

| Problem | Key Technique | Time Complexity | Space Complexity |
|---------|---------------|-----------------|------------------|
| Zig Zag Traversal | BFS + Direction Flag | O(N) | O(N) |
| Boundary Traversal | Three Separate Traversals | O(N) | O(H) |
| Vertical Order | BFS + Column Mapping | O(N log N) | O(N) |
| Top View | BFS + First Column Occurrence | O(N log N) | O(W) |
| Bottom View | BFS + Last Column Occurrence | O(N log N) | O(W) |
| Right/Left View | BFS + Last Level Node | O(N) | O(H) |
| Root to Node Path | DFS Backtracking | O(N) | O(H×L) |
| Nodes at Distance K | BFS + Parent Pointers | O(N) | O(N) |
| Minimum Burn Time | BFS from Target | O(N) | O(N) |
| Maximum Width | Index-based BFS | O(N) | O(W) |
| Count Complete Tree Nodes | Height-based Formula | O(log²N) | O(log N) |
| Lowest Common Ancestor | Recursive DFS | O(N) | O(H) |

**Legend:**
- N = Number of nodes
- H = Height of tree
- W = Width of tree
- L = Number of leaf nodes

---

## 🎯 Key Patterns

### 1. **Level Order Variations**
- Use BFS with queue
- Track levels using queue size
- Applications: Zig-zag, Views, Width

### 2. **Coordinate Mapping**
- Assign (x, y) coordinates to nodes
- Use dictionaries for grouping
- Applications: Vertical order, Top/Bottom view

### 3. **Parent Pointer Technique**
- Enable upward traversal
- Convert tree to graph-like structure
- Applications: Distance K, Burn time, LCA

### 4. **DFS Path Tracking**
- Maintain path during traversal
- Backtrack or use immutable paths
- Applications: Root-to-node paths

### 5. **Complete Tree Properties**
- Use mathematical formulas
- Height-based optimizations
- Applications: Count nodes efficiently

---

## 💡 Tips & Tricks

1. **For Views:** Keep track of first/last occurrence per column/level
2. **For Distance Problems:** Use parent pointers + BFS
3. **For Width Problems:** Use indexing like complete binary tree
4. **For Complete Trees:** Exploit mathematical properties
5. **For LCA:** Recursive DFS is most elegant

---

## 🔗 Related Topics

- Binary Tree Basics
- Depth-First Search (DFS)
- Breadth-First Search (BFS)
- Tree Recursion
- Graph Algorithms
- Queue Data Structure

---

<div align="center">

**Happy Coding! 🚀**

*Master these patterns and you'll ace any tree traversal problem!*

</div>
