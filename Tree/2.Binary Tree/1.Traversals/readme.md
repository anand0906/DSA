# 🌲 Binary Tree Traversal Problems

## Table of Contents
1. [Inorder Traversal](#1-inorder-traversal)
2. [Preorder Traversal](#2-preorder-traversal)
3. [Postorder Traversal](#3-postorder-traversal)
4. [Level Order Traversal](#4-level-order-traversal)
5. [All Three Traversals in One Pass](#5-all-three-traversals-in-one-pass)

---

## 1. Inorder Traversal

### Problem Statement

Given the root of a binary tree, return the **inorder traversal** of its nodes' values.

In inorder traversal, we visit nodes in the order: **Left → Root → Right**

### Examples

**Example 1:**

```
Input: root = [1, 4, null, 4, 2]
Output: [4, 4, 2, 1]
```

![Inorder Example 1](https://static.takeuforward.org/content/aptitude_1761425379265_eg-26-10.png)

**Example 2:**

```
Input: root = [1, null, 2, 3]
Output: [1, 3, 2]
```

![Inorder Example 2](https://static.takeuforward.org/content/ProblemSetter-YbAPZ5Ug)

---

### Approach 1: Recursive Solution

#### Intuition

The recursive approach follows the natural definition of inorder traversal:
- First, recursively visit all nodes in the left subtree
- Then, visit the current node (add it to result)
- Finally, recursively visit all nodes in the right subtree

This naturally uses the call stack to keep track of where we are in the tree.

#### Approach

1. Base case: If the node is null, return
2. Recursively traverse the left subtree
3. Add the current node's value to the result
4. Recursively traverse the right subtree

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def inorder_recursive(node, ans):
    # Base case: if node is None, return
    if node is None:
        return
    
    # Step 1: Recursively traverse left subtree
    inorder_recursive(node.left, ans)
    
    # Step 2: Visit current node (add to result)
    ans.append(node.data)
    
    # Step 3: Recursively traverse right subtree
    inorder_recursive(node.right, ans)

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

ans = []
inorder_recursive(root, ans)
print(ans)  # Output: [4, 2, 5, 1, 3]
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Recursion stack space where h is the height of the tree
  - Best case (balanced tree): O(log n)
  - Worst case (skewed tree): O(n)

---

### Approach 2: Iterative Solution (Using Stack)

#### Intuition

We can simulate the recursive call stack using an explicit stack data structure. The idea is to:
- Keep going left as far as possible, pushing nodes onto the stack
- When we can't go left anymore, pop a node, process it, and move to its right child
- This mimics the recursive behavior without using function calls

#### Approach

1. Initialize a stack and a current pointer to root
2. While there are nodes to process:
   - If current is not null, push it to stack and go left
   - If current is null, pop from stack, add to result, and go right
3. Continue until stack is empty and current is null

#### Code

```python
def inorder_iterative(root):
    stack = []        # Stack to simulate recursion
    ans = []          # Result array
    current = root    # Pointer to traverse the tree
    
    while True:
        # Keep going left and push all nodes to stack
        if current is not None:
            stack.append(current)
            current = current.left
        else:
            # If we can't go left, check if stack is empty
            if not stack:
                break  # We're done, exit loop
            
            # Pop from stack, add to result
            temp = stack.pop()
            ans.append(temp.data)
            
            # Move to right child
            current = temp.right
    
    return ans

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

print(inorder_iterative(root))  # Output: [4, 2, 5, 1, 3]
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Stack space where h is the height of the tree
  - Best case (balanced tree): O(log n)
  - Worst case (skewed tree): O(n)

---

## 2. Preorder Traversal

### Problem Statement

Given the root of a binary tree, return the **preorder traversal** of its nodes' values.

In preorder traversal, we visit nodes in the order: **Root → Left → Right**

### Examples

**Example 1:**

```
Input: root = [1, 4, null, 4, 2]
Output: [1, 4, 4, 2]
```

**Example 2:**

```
Input: root = [1]
Output: [1]
```

---

### Approach 1: Recursive Solution

#### Intuition

Preorder traversal processes the root first before visiting its children. The recursive approach:
- Visit the current node first (add it to result)
- Then recursively visit the left subtree
- Finally recursively visit the right subtree

This is useful for creating a copy of the tree or prefix expressions.

#### Approach

1. Base case: If the node is null, return
2. Add the current node's value to the result
3. Recursively traverse the left subtree
4. Recursively traverse the right subtree

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def preorder_recursive(node, ans):
    # Base case: if node is None, return
    if node is None:
        return
    
    # Step 1: Visit current node (add to result)
    ans.append(node.data)
    
    # Step 2: Recursively traverse left subtree
    preorder_recursive(node.left, ans)
    
    # Step 3: Recursively traverse right subtree
    preorder_recursive(node.right, ans)

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

ans = []
preorder_recursive(root, ans)
print(ans)  # Output: [1, 2, 4, 5, 3]
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Recursion stack space where h is the height of the tree
  - Best case (balanced tree): O(log n)
  - Worst case (skewed tree): O(n)

---

### Approach 2: Iterative Solution (Method 1)

#### Intuition

Similar to inorder iterative, but we process the node immediately when we encounter it instead of waiting. We:
- Push nodes to stack and add them to result immediately
- Keep going left, adding each node to result
- When we can't go left, pop and move to right child

#### Approach

1. Initialize a stack and current pointer to root
2. While there are nodes to process:
   - If current is not null, add to result, push to stack, go left
   - If current is null, pop from stack and move to right child
3. Continue until stack is empty

#### Code

```python
def preorder_iterative(root):
    stack = []        # Stack to simulate recursion
    ans = []          # Result array
    current = root    # Pointer to traverse the tree
    
    while True:
        # Process node immediately, then go left
        if current is not None:
            stack.append(current)
            ans.append(current.data)  # Add to result immediately
            current = current.left
        else:
            # If we can't go left, check if stack is empty
            if not stack:
                break  # We're done, exit loop
            
            # Pop from stack and move to right child
            temp = stack.pop()
            current = temp.right
    
    return ans

# Example Usage
print(preorder_iterative(root))  # Output: [1, 2, 4, 5, 3]
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Stack space where h is the height of the tree

---

### Approach 3: Iterative Solution (Method 2 - Simpler)

#### Intuition

A more intuitive approach using a stack:
- Start with root in the stack
- Pop a node, add it to result
- Push its right child first (so it's processed later)
- Push its left child (so it's processed before right)
- The stack order ensures left is processed before right

#### Approach

1. Initialize stack with root node
2. While stack is not empty:
   - Pop a node and add to result
   - Push right child if exists
   - Push left child if exists (left will be processed first due to LIFO)
3. Return result

#### Code

```python
def preorder_iterative2(root):
    ans = []              # Result array
    stack = [root]        # Initialize stack with root
    
    while stack:
        # Pop node from stack
        temp = stack.pop()
        ans.append(temp.data)
        
        # Push right child first (will be processed later)
        if temp.right:
            stack.append(temp.right)
        
        # Push left child (will be processed before right due to LIFO)
        if temp.left:
            stack.append(temp.left)
    
    return ans

# Example Usage
print(preorder_iterative2(root))  # Output: [1, 2, 4, 5, 3]
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Stack space where h is the height of the tree

---

## 3. Postorder Traversal

### Problem Statement

Given the root of a binary tree, return the **postorder traversal** of its nodes' values.

In postorder traversal, we visit nodes in the order: **Left → Right → Root**

### Examples

**Example 1:**

```
Input: root = [1, 4, null, 4, 2]
Output: [4, 2, 4, 1]
```

![Postorder Example 1](https://static.takeuforward.org/content/ProblemSetter-ORvQLHw4)

**Example 2:**

```
Input: root = [1, null, 2, 3]
Output: [3, 2, 1]
```

![Postorder Example 2](https://static.takeuforward.org/content/ProblemSetter-11arw0oH)

---

### Approach 1: Recursive Solution

#### Intuition

Postorder traversal processes the root last after visiting both children. This is useful for:
- Deleting a tree (delete children before parent)
- Calculating postfix expressions
- Getting leaf nodes first

The recursive approach naturally handles this by visiting children before the parent.

#### Approach

1. Base case: If the node is null, return
2. Recursively traverse the left subtree
3. Recursively traverse the right subtree
4. Add the current node's value to the result

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def postorder_recursive(node, ans):
    # Base case: if node is None, return
    if node is None:
        return
    
    # Step 1: Recursively traverse left subtree
    postorder_recursive(node.left, ans)
    
    # Step 2: Recursively traverse right subtree
    postorder_recursive(node.right, ans)
    
    # Step 3: Visit current node (add to result)
    ans.append(node.data)

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

ans = []
postorder_recursive(root, ans)
print(ans)  # Output: [4, 5, 2, 3, 1]
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Recursion stack space where h is the height of the tree
  - Best case (balanced tree): O(log n)
  - Worst case (skewed tree): O(n)

---

### Approach 2: Iterative Solution (Using Reverse)

#### Intuition

A clever trick for postorder traversal:
- Postorder: Left → Right → Root
- Reverse of (Root → Right → Left) = (Left → Right → Root)
- So we can do a modified preorder (Root → Right → Left) and reverse it!

This approach:
- Does preorder-like traversal but visits right before left
- Reverses the result to get postorder

#### Approach

1. Use a stack starting with root
2. Pop nodes and add to result
3. Push left child first, then right child (opposite of preorder)
4. Reverse the final result

#### Code

```python
def postorder_iterative(root):
    ans = []              # Result array
    stack = [root]        # Initialize stack with root
    
    while stack:
        # Pop node from stack
        temp = stack.pop()
        ans.append(temp.data)
        
        # Push left child first (will be processed later)
        if temp.left:
            stack.append(temp.left)
        
        # Push right child (will be processed before left due to LIFO)
        if temp.right:
            stack.append(temp.right)
    
    # Reverse to get postorder from root-right-left
    ans.reverse()
    return ans

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

print(postorder_iterative(root))  # Output: [4, 5, 2, 3, 1]
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once, plus O(n) for reversing
- **Space Complexity**: O(h) - Stack space where h is the height of the tree

---

## 4. Level Order Traversal

### Problem Statement

Given the root of a binary tree, return the **level order traversal** of its nodes' values (i.e., from left to right, level by level).

### Examples

**Example 1:**

```
Input: root = [3, 9, 20, null, null, 15, 7]
Output: [[3], [9, 20], [15, 7]]
```

![Level Order Example 1](https://static.takeuforward.org/content/ProblemSetter-rPjEzBZp)

**Example 2:**

```
Input: root = [1, 4, null, 4, 2]
Output: [[1], [4], [4, 2]]
```

![Level Order Example 2](https://static.takeuforward.org/content/ProblemSetter-6BBpQBUc)

---

### Approach: BFS Using Queue

#### Intuition

Level order traversal is a Breadth-First Search (BFS) approach. Unlike DFS which goes deep, BFS explores all nodes at the current level before moving to the next level.

Key insight:
- Use a queue (FIFO) instead of a stack (LIFO)
- Process all nodes at current level before moving to next
- For each node, add its children to queue for next level

This naturally groups nodes by their level in the tree.

#### Approach

1. If root is null, return empty list
2. Initialize queue with root node
3. While queue is not empty:
   - Get the number of nodes at current level (queue size)
   - Process all nodes at this level
   - For each node, add its children to queue
   - Add the level's values to result
4. Return result

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def level_order(root):
    # Edge case: empty tree
    if root is None:
        return []
    
    ans = []              # Result array (list of levels)
    queue = [root]        # Initialize queue with root
    
    while queue:
        n = len(queue)    # Number of nodes at current level
        level = []        # Array to store current level's values
        
        # Process all nodes at current level
        for _ in range(n):
            # Remove first node from queue (FIFO)
            node = queue.pop(0)
            level.append(node.data)
            
            # Add left child to queue for next level
            if node.left:
                queue.append(node.left)
            
            # Add right child to queue for next level
            if node.right:
                queue.append(node.right)
        
        # Add current level to result
        ans.append(level)
    
    return ans

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

print(level_order(root))  # Output: [[1], [2, 3], [4, 5]]
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(w) - Queue space where w is the maximum width (number of nodes at any level)
  - Best case: O(1) for skewed tree
  - Worst case: O(n/2) ≈ O(n) for perfect binary tree (last level has n/2 nodes)

---

## 5. All Three Traversals in One Pass

### Problem Statement

Given a binary tree with root node, return the **Inorder**, **Preorder**, and **Postorder** traversal of the binary tree in a single traversal.

### Examples

**Example 1:**

```
Input: root = [1, 3, 4, 5, 2, 7, 6]
Output: [[5, 3, 2, 1, 7, 4, 6], [1, 3, 5, 2, 4, 7, 6], [5, 2, 3, 7, 6, 4, 1]]
Explanation:
- Inorder: [5, 3, 2, 1, 7, 4, 6]
- Preorder: [1, 3, 5, 2, 4, 7, 6]
- Postorder: [5, 2, 3, 7, 6, 4, 1]
```

![All Traversals Example 1](https://static.takeuforward.org/content/ProblemSetter-rOlkMuo4)

**Example 2:**

```
Input: root = [1, 2, 3, null, null, null, 6]
Output: [[2, 1, 3, 6], [1, 2, 3, 6], [2, 6, 3, 1]]
Explanation:
- Inorder: [2, 1, 3, 6]
- Preorder: [1, 2, 3, 6]
- Postorder: [2, 6, 3, 1]
```

![All Traversals Example 2](https://static.takeuforward.org/content/ProblemSetter-OExDzFDr)

---

### Approach: State Machine with Stack

#### Intuition

The key insight is that each node is visited exactly 3 times in a DFS traversal:
1. **State 1 (Preorder)**: When we first encounter the node (before visiting left child)
2. **State 2 (Inorder)**: After visiting left subtree, before visiting right subtree
3. **State 3 (Postorder)**: After visiting both left and right subtrees

We can use a stack to track nodes along with their current state:
- State 1: Add to preorder, push left child
- State 2: Add to inorder, push right child
- State 3: Add to postorder, done with this node

#### Approach

1. Initialize three empty arrays for the three traversals
2. Use a stack with tuples (node, state) starting with (root, 1)
3. While stack is not empty:
   - Pop node and its state
   - If state is 1 (preorder): add to preorder, increment state, push left child
   - If state is 2 (inorder): add to inorder, increment state, push right child
   - If state is 3 (postorder): add to postorder, done with node
4. Return all three traversals

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def all_traversals(root):
    # Initialize result arrays for all three traversals
    pre = []        # Preorder: Root → Left → Right
    post = []       # Postorder: Left → Right → Root
    inorder = []    # Inorder: Left → Root → Right
    
    # Edge case: empty tree
    if root is None:
        return [inorder, pre, post]
    
    # Stack stores tuples of (node, state)
    # State 1: Preorder position (before left child)
    # State 2: Inorder position (between left and right child)
    # State 3: Postorder position (after right child)
    stack = [(root, 1)]
    
    while stack:
        node, state = stack.pop()
        
        # State 1: Preorder - Visit node before children
        if state == 1:
            # Add to preorder result
            pre.append(node.data)
            
            # Move to state 2 (will be processed after left subtree)
            stack.append((node, 2))
            
            # Push left child with state 1 (if exists)
            if node.left:
                stack.append((node.left, 1))
        
        # State 2: Inorder - Visit node between left and right children
        elif state == 2:
            # Add to inorder result
            inorder.append(node.data)
            
            # Move to state 3 (will be processed after right subtree)
            stack.append((node, 3))
            
            # Push right child with state 1 (if exists)
            if node.right:
                stack.append((node.right, 1))
        
        # State 3: Postorder - Visit node after both children
        else:
            # Add to postorder result
            post.append(node.data)
            # Node is completely processed, nothing more to do
    
    return [inorder, pre, post]

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

result = all_traversals(root)
print("Inorder:", result[0])    # [4, 2, 5, 1, 3]
print("Preorder:", result[1])   # [1, 2, 4, 5, 3]
print("Postorder:", result[2])  # [4, 5, 2, 3, 1]
```

#### Complexity Analysis

- **Time Complexity**: O(n) - Each node is visited exactly 3 times (once for each state)
- **Space Complexity**: O(h) - Stack space where h is the height of the tree
  - Best case (balanced tree): O(log n)
  - Worst case (skewed tree): O(n)

---

## 📋 Quick Comparison Summary

| Traversal | Order | Recursive TC | Recursive SC | Iterative TC | Iterative SC |
|-----------|-------|--------------|--------------|--------------|--------------|
| **Inorder** | Left → Root → Right | O(n) | O(h) | O(n) | O(h) |
| **Preorder** | Root → Left → Right | O(n) | O(h) | O(n) | O(h) |
| **Postorder** | Left → Right → Root | O(n) | O(h) | O(n) | O(h) |
| **Level Order** | Level by level | - | - | O(n) | O(w) |
| **All Three** | All in one pass | - | - | O(n) | O(h) |

*Where: n = number of nodes, h = height of tree, w = maximum width of tree*

---

*Happy Coding! 🌟*
