# 🌲 Binary Tree Problems - Medium Level

## Table of Contents
1. [Maximum Depth in Binary Tree](#1-maximum-depth-in-binary-tree)
2. [Check if Two Trees are Identical](#2-check-if-two-trees-are-identical)
3. [Check for Balanced Binary Tree](#3-check-for-balanced-binary-tree)
4. [Diameter of Binary Tree](#4-diameter-of-binary-tree)
5. [Maximum Path Sum](#5-maximum-path-sum)
6. [Check for Symmetrical Binary Tree](#6-check-for-symmetrical-binary-tree)

---

## 1. Maximum Depth in Binary Tree

### Problem Statement

Given the root of a binary tree, return its **maximum depth**.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

### Examples

**Example 1:**

```
Input: root = [1, 2, 3, null, null, null, 6]
Output: 3
Explanation: The path from root node 1 to node with value 6 has maximum depth with 3 nodes along path.
```

![Example 1](https://static.takeuforward.org/content/ProblemSetter-iaO2S02r)

**Example 2:**

```
Input: root = [3, 9, 20, null, null, 15, 7]
Output: 3
Explanation: The path from root node 3 to node with value 15 has maximum depth with 3 nodes along path.
```

![Example 2](https://static.takeuforward.org/content/ProblemSetter-6P7ar26j)

---

### Approach 1: Recursive DFS

#### Intuition

The maximum depth of a tree is essentially asking "how many levels does this tree have?" 

We can think recursively:
- The depth of an empty tree is 0
- The depth of any tree is 1 (for the current node) + the maximum depth of its left or right subtree

This naturally follows the tree's recursive structure - we find the depth of left and right subtrees, take the maximum, and add 1 for the current node.

#### Approach

1. Base case: If the node is null, return 0 (no depth)
2. Recursively find the depth of the left subtree
3. Recursively find the depth of the right subtree
4. Return 1 + max(left depth, right depth)

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def max_depth_recursive(node):
    # Base case: empty tree has depth 0
    if node is None:
        return 0
    
    # Recursively find depth of left subtree
    left = max_depth_recursive(node.left)
    
    # Recursively find depth of right subtree
    right = max_depth_recursive(node.right)
    
    # Return 1 (current node) + maximum of left and right depths
    return 1 + max(left, right)

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

print(max_depth_recursive(root))  # Output: 3
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Recursion stack space where h is the height of the tree
  - Best case (balanced tree): O(log n)
  - Worst case (skewed tree): O(n)

---

### Approach 2: Iterative BFS (Level Order Traversal)

#### Intuition

Instead of going deep (DFS), we can count levels while traversing the tree level by level (BFS).

The idea is simple:
- Use a queue to process all nodes at each level
- Count how many levels we process
- The number of levels = maximum depth

This is intuitive because each level we process adds 1 to the depth.

#### Approach

1. Initialize queue with root node and level counter to 0
2. While queue is not empty:
   - Get the size of the queue (number of nodes at current level)
   - Process all nodes at current level
   - Add their children to queue for next level
   - Increment level counter
3. Return the level counter

#### Code

```python
def max_depth_iterative(root):
    # Edge case: empty tree
    if root is None:
        return 0
    
    queue = [root]    # Initialize queue with root
    level = 0         # Counter for depth/levels
    
    while queue:
        # Number of nodes at current level
        n = len(queue)
        
        # Process all nodes at current level
        for i in range(n):
            # Remove first node from queue (FIFO)
            node = queue.pop(0)
            
            # Add left child to queue for next level
            if node.left:
                queue.append(node.left)
            
            # Add right child to queue for next level
            if node.right:
                queue.append(node.right)
        
        # Increment level after processing current level
        level += 1
    
    return level

# Example Usage
print(max_depth_iterative(root))  # Output: 3
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(w) - Queue space where w is the maximum width of the tree
  - Best case: O(1) for skewed tree
  - Worst case: O(n/2) ≈ O(n) for perfect binary tree

---

## 2. Check if Two Trees are Identical

### Problem Statement

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are **structurally identical**, and the nodes have the **same value**.

### Examples

**Example 1:**

```
Input: p = [1, 2, 3], q = [1, 2, 3]
Output: true
Explanation: Both trees are structurally identical with same values
```

![Tree P](https://static.takeuforward.org/content/ProblemSetter-hF_hLlOD)
![Tree Q](https://static.takeuforward.org/content/ProblemSetter-hF_hLlOD)

**Example 2:**

```
Input: p = [1, 2, 1], q = [1, 1, 2]
Output: false
Explanation: Trees have different structure
```

![Tree P](https://static.takeuforward.org/content/ProblemSetter-hF_hLlOD)
![Tree Q](https://static.takeuforward.org/content/ProblemSetter-D4Rbxpht)

---

### Approach 1: Inorder Traversal Comparison (Limited)

#### Intuition

One naive approach is to get the inorder traversal of both trees and compare them. If they match, the trees might be identical.

**Important**: This approach only works if all node values are unique! It fails when there are duplicate values because different tree structures can produce the same inorder traversal.

#### Approach

1. Perform inorder traversal on both trees
2. Compare the resulting arrays
3. Return true if they match

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def is_identical_inorder(root1, root2):
    """
    WARNING: This approach only works for trees with unique values!
    It will fail for trees with duplicate values.
    """
    def inorder(node, order):
        # Base case: empty node
        if not node:
            return
        
        # Inorder: Left -> Root -> Right
        inorder(node.left, order)
        order.append(node.data)
        inorder(node.right, order)
    
    # Get inorder traversals of both trees
    order1 = []
    order2 = []
    inorder(root1, order1)
    inorder(root2, order2)
    
    # Compare the traversals
    return order1 == order2

# Example Usage
root1 = Node(1)
root1.left = Node(2)
root1.right = Node(3)

root2 = Node(1)
root2.left = Node(2)
root2.right = Node(3)

print(is_identical_inorder(root1, root2))  # Output: True
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node in both trees
- **Space Complexity**: O(n) - For storing inorder traversals and recursion stack
- **⚠️ Limitation**: Only works for unique values

---

### Approach 2: Recursive Comparison (Optimal)

#### Intuition

The correct way to check if two trees are identical is to compare them node by node recursively.

Two trees are identical if:
1. Both nodes are null (base case - empty trees are identical)
2. Both nodes exist, have the same value, AND their left and right subtrees are identical

If one node is null and the other isn't, or if their values don't match, they're not identical.

#### Approach

1. Base case 1: If both nodes are null, return True
2. Base case 2: If only one node is null, return False
3. Base case 3: If values don't match, return False
4. Recursively check if left subtrees are identical
5. Recursively check if right subtrees are identical
6. Return True only if both subtrees are identical

#### Code

```python
def is_identical(node1, node2):
    # Base case 1: Both trees are empty - they are identical
    if node1 is None and node2 is None:
        return True
    
    # Base case 2: One tree is empty, other is not - not identical
    if node1 is None or node2 is None:
        return False
    
    # Base case 3: Values don't match - not identical
    if node1.data != node2.data:
        return False
    
    # Recursively check if left subtrees are identical
    left = is_identical(node1.left, node2.left)
    
    # Recursively check if right subtrees are identical
    right = is_identical(node1.right, node2.right)
    
    # Trees are identical only if both left and right subtrees are identical
    return left and right

# Example Usage
root1 = Node(1)
root1.left = Node(2)
root1.right = Node(3)
root1.left.left = Node(4)
root1.left.right = Node(5)

root2 = Node(1)
root2.left = Node(2)
root2.right = Node(3)
root2.left.left = Node(4)
root2.left.right = Node(5)

print(is_identical(root1, root2))  # Output: True
```

#### Complexity Analysis

- **Time Complexity**: O(min(n, m)) - Where n and m are the number of nodes in the two trees. We visit nodes until we find a mismatch or traverse the smaller tree completely.
- **Space Complexity**: O(min(h1, h2)) - Recursion stack space where h1 and h2 are the heights of the two trees

---

## 3. Check for Balanced Binary Tree

### Problem Statement

Given a binary tree root, find if it is **height-balanced** or not.

A tree is height-balanced if the difference between the heights of left and right subtrees is **not more than one** for **all nodes** of the tree.

### Examples

**Example 1:**

```
Input: root = [3, 9, 20, null, null, 15, 7]
Output: Yes
Explanation: For every node, the height difference between left and right subtrees is ≤ 1
```

![Balanced Tree](https://static.takeuforward.org/content/ProblemSetter-MQ-fF3Hv)

**Example 2:**

```
Input: root = [1, 2, null, null, 3]
Output: No
Explanation: Root node has height difference of 2 (left: 2, right: 0)
```

![Unbalanced Tree](https://static.takeuforward.org/content/ProblemSetter-BEw-GjtR)

---

### Approach 1: Naive Recursive (Top-Down)

#### Intuition

The straightforward approach is to check each node:
1. Calculate the height of left subtree
2. Calculate the height of right subtree
3. Check if the difference is ≤ 1
4. Recursively check if left and right subtrees are also balanced

This works but recalculates heights multiple times for the same nodes.

#### Approach

1. For each node, find the height of left and right subtrees
2. Check if the absolute difference is ≤ 1
3. Recursively check if both subtrees are balanced
4. Return True only if current node is balanced AND both subtrees are balanced

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def find_height(node):
    """Helper function to find height of a tree"""
    # Base case: empty tree has height 0
    if node is None:
        return 0
    
    # Height = 1 + max height of subtrees
    left = 1 + find_height(node.left)
    right = 1 + find_height(node.right)
    
    return max(left, right)

def is_balanced_naive(node):
    # Base case: empty tree is balanced
    if node is None:
        return True
    
    # Find heights of left and right subtrees
    left = find_height(node.left)
    right = find_height(node.right)
    
    # Check three conditions:
    # 1. Current node is balanced (height difference ≤ 1)
    # 2. Left subtree is balanced
    # 3. Right subtree is balanced
    return (abs(right - left) <= 1 and 
            is_balanced_naive(node.left) and 
            is_balanced_naive(node.right))

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

print(is_balanced_naive(root))  # Output: True
```

#### Complexity Analysis

- **Time Complexity**: O(n²) - For each node (n), we calculate height (n) in worst case
- **Space Complexity**: O(h) - Recursion stack space where h is the height

---

### Approach 2: Optimized Recursive (Bottom-Up)

#### Intuition

The naive approach recalculates heights repeatedly. We can optimize by:
- Calculating height and checking balance in a single pass
- Using -1 as a sentinel value to indicate imbalance
- If any subtree is imbalanced, propagate -1 upward immediately

This way, we calculate each node's height only once and stop early if we find an imbalance.

#### Approach

1. Create a helper function that returns height, but uses -1 to indicate imbalance
2. For each node:
   - Calculate left subtree height (returns -1 if imbalanced)
   - If left is imbalanced, propagate -1
   - Calculate right subtree height (returns -1 if imbalanced)
   - If right is imbalanced, propagate -1
   - Check if current node is balanced
   - If not balanced, return -1
   - Otherwise, return 1 + max(left, right)
3. Final check: if result is -1, tree is imbalanced

#### Code

```python
def is_balanced_optimized(root):
    def find_height(node):
        # Base case: empty tree has height 0
        if node is None:
            return 0
        
        # Calculate left subtree height
        left_height = find_height(node.left)
        # If left subtree is imbalanced, propagate -1
        if left_height == -1:
            return -1
        
        # Calculate right subtree height
        right_height = find_height(node.right)
        # If right subtree is imbalanced, propagate -1
        if right_height == -1:
            return -1
        
        # Check if current node creates imbalance
        if abs(left_height - right_height) > 1:
            return -1  # Mark as imbalanced
        
        # Return actual height (1 + max of subtree heights)
        return 1 + max(left_height, right_height)
    
    # Tree is balanced if result is not -1
    return find_height(root) != -1

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

print(is_balanced_optimized(root))  # Output: True
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Recursion stack space where h is the height of the tree

---

## 4. Diameter of Binary Tree

### Problem Statement

Given the root of a binary tree, return the **length of the diameter** of the tree.

The diameter of a binary tree is the **length of the longest path** between any two nodes in a tree. This path **may or may not pass through the root**.

**Note**: The length of a path is represented by the number of edges between nodes.

### Examples

**Example 1:**

```
Input: root = [1, 2, 3, 4, 5]
Output: 3
Explanation: The path length between node 4 and 3 is of length 3 (4->2->1->3)
```

![Diameter Example 1](https://static.takeuforward.org/content/ProblemSetter-PtG_ttvW)

**Example 2:**

```
Input: root = [1, 2, 3, null, 4, null, 5]
Output: 4
Explanation: The path length between node 4 and 5 is of length 4 (4->2->1->3->5)
```

![Diameter Example 2](https://static.takeuforward.org/content/ProblemSetter-klKGu6hl)

---

### Approach 1: Naive Recursive

#### Intuition

For each node, the diameter can be one of three things:
1. The diameter of the left subtree
2. The diameter of the right subtree
3. The longest path that goes through the current node (left height + right height)

We need to check all three possibilities for every node and return the maximum.

This approach recalculates heights multiple times, making it inefficient.

#### Approach

1. For each node, calculate:
   - Height of left subtree
   - Height of right subtree
   - Diameter through current node = left height + right height
   - Diameter of left subtree (recursively)
   - Diameter of right subtree (recursively)
2. Return the maximum of all three diameters

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def find_height(node):
    """Helper function to find height of a tree"""
    # Base case: empty tree has height 0
    if node is None:
        return 0
    
    # Height = 1 + max height of subtrees
    left = find_height(node.left)
    right = find_height(node.right)
    
    return 1 + max(left, right)

def diameter_naive(node):
    # Base case: empty tree has diameter 0
    if node is None:
        return 0
    
    # Find heights of left and right subtrees
    left_height = find_height(node.left)
    right_height = find_height(node.right)
    
    # Diameter through current node
    diameter = left_height + right_height
    
    # Find diameter of left subtree
    left_dia = diameter_naive(node.left)
    
    # Find diameter of right subtree
    right_dia = diameter_naive(node.right)
    
    # Return maximum of all three possibilities
    return max(diameter, max(left_dia, right_dia))

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

print(diameter_naive(root))  # Output: 3
```

#### Complexity Analysis

- **Time Complexity**: O(n²) - For each node (n), we calculate height (n) in worst case
- **Space Complexity**: O(h) - Recursion stack space where h is the height

---

### Approach 2: Optimized Recursive (Single Pass)

#### Intuition

Similar to the balanced tree optimization, we can calculate diameter and height in a single pass.

Key insight:
- While calculating height of each node, also check if the path through that node is the maximum diameter
- Use a variable to track the maximum diameter seen so far
- Each recursive call returns the height, but updates the diameter as a side effect

This way, we visit each node only once.

#### Approach

1. Initialize a variable to track maximum diameter
2. Create a helper function that:
   - Calculates height recursively
   - Updates maximum diameter by checking left_height + right_height
   - Returns height for parent nodes
3. Return the maximum diameter found

#### Code

```python
def diameter_optimized(root):
    # Variable to track maximum diameter
    maxi = 0
    
    def find_height(node):
        nonlocal maxi  # Access outer variable to update diameter
        
        # Base case: empty tree has height 0
        if node is None:
            return 0
        
        # Calculate height of left subtree
        left = find_height(node.left)
        
        # Calculate height of right subtree
        right = find_height(node.right)
        
        # Update maximum diameter if path through current node is larger
        # Diameter through current node = left height + right height
        maxi = max(maxi, left + right)
        
        # Return height for parent node calculation
        return 1 + max(left, right)
    
    # Start the recursion
    find_height(root)
    
    # Return the maximum diameter found
    return maxi

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

print(diameter_optimized(root))  # Output: 3
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Recursion stack space where h is the height of the tree

---

## 5. Maximum Path Sum

### Problem Statement

In a binary tree, a **path** is a sequence of nodes where each pair of adjacent nodes has an edge connecting them. A node can only appear in the sequence at most once.

The **path sum** of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the **maximum path sum** of any non-empty path.

**Note**: The path does not have to go through the root.

### Examples

**Example 1:**

```
Input: root = [20, 9, -10, null, null, 15, 7]
Output: 34
Explanation: The path is 15 -> -10 -> 20 -> 9, which gives sum = 34
```

![Path Sum Example 1](https://static.takeuforward.org/content/ProblemSetter-gLEBhSXO)

**Example 2:**

```
Input: root = [-10, 9, 20, null, null, 15, 7]
Output: 42
Explanation: The path is 15 -> 20 -> 7, which gives sum = 42
```

![Path Sum Example 2](https://static.takeuforward.org/content/ProblemSetter-BBBySXwH)

---

### Approach: Optimized Recursive

#### Intuition

This problem is similar to finding the diameter, but instead of counting nodes, we're summing values.

Key insights:
1. For each node, the maximum path sum that goes through it = node.val + left path sum + right path sum
2. However, if a subtree's path sum is negative, we shouldn't include it (use 0 instead)
3. When returning to parent, we can only return one path (either left or right), not both
4. Track the global maximum as we explore the tree

Think of it as: at each node, we check if making a "bridge" through this node (connecting left and right paths) gives us a better sum than what we've seen before.

#### Approach

1. Initialize a variable to track maximum path sum (negative infinity to handle all negative trees)
2. Create a helper function that:
   - Returns the maximum path sum going down from current node
   - Updates global maximum by considering path through current node
   - Takes max(0, path) to ignore negative contributions
3. For each node:
   - Calculate best path from left child (or 0 if negative)
   - Calculate best path from right child (or 0 if negative)
   - Update global max with: node.val + left + right (path through current node)
   - Return to parent: node.val + max(left, right) (can only take one path up)

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def max_path_sum(root):
    # Variable to track maximum path sum
    # Initialize to negative infinity to handle all-negative trees
    maxi = float('-inf')
    
    def find_sum(node):
        nonlocal maxi  # Access outer variable to update max sum
        
        # Base case: empty tree contributes 0 to path sum
        if node is None:
            return 0
        
        # Calculate max path sum from left subtree
        # Use max(0, ...) to ignore negative contributions
        left = max(0, find_sum(node.left))
        
        # Calculate max path sum from right subtree
        # Use max(0, ...) to ignore negative contributions
        right = max(0, find_sum(node.right))
        
        # Update global maximum with path through current node
        # Path through node = node value + left path + right path
        maxi = max(maxi, node.data + left + right)
        
        # Return max sum going down one path from current node
        # (can only take one path when returning to parent)
        return node.data + max(left, right)
    
    # Start the recursion
    find_sum(root)
    
    # Return the maximum path sum found
    return maxi

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

print(max_path_sum(root))  # Output: 11 (path: 4->2->5)
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node exactly once
- **Space Complexity**: O(h) - Recursion stack space where h is the height of the tree
  - Best case (balanced tree): O(log n)
  - Worst case (skewed tree): O(n)

---

## 6. Check for Symmetrical Binary Tree

### Problem Statement

Given the root of a binary tree, check whether it is a **mirror of itself** (i.e., symmetric around its center).

### Examples

**Example 1:**

```
Input: root = [1, 2, 2, 3, 4, 4, 3]
Output: true
Explanation: The tree is symmetric when folded around the vertical center line
```

![Symmetric Tree](https://static.takeuforward.org/content/ProblemSetter-xz280fOG)

**Example 2:**

```
Input: root = [1, 2, 2, null, 3, null, 3]
Output: false
Explanation: When folded, the node 3s don't align properly with null nodes
```

![Asymmetric Tree](https://static.takeuforward.org/content/ProblemSetter-C3w4YXKm)

---

### Approach: Recursive Mirror Check

#### Intuition

A tree is symmetric if:
- The left subtree is a mirror reflection of the right subtree

To check if two trees are mirrors:
1. Both are empty (symmetric)
2. Both exist, have the same value, AND:
   - Left's left subtree mirrors Right's right subtree
   - Left's right subtree mirrors Right's left subtree

Notice how we compare opposite sides - this is the key to checking symmetry!

#### Approach

1. Create a helper function to check if two trees are mirrors
2. For two nodes to be mirrors:
   - Base case 1: Both null → True (mirrors)
   - Base case 2: One null, one not → False (not mirrors)
   - Base case 3: Values don't match → False
   - Recursive case: Check if:
     - left.left mirrors right.right
     - left.right mirrors right.left
3. Call the helper with root.left and root.right

#### Code

```python
class Node:
    def __init__(self, val):
        self.data = val      # Node's value
        self.left = None     # Pointer to left child
        self.right = None    # Pointer to right child

def check_mirror(left, right):
    """Helper function to check if two trees are mirrors of each other"""
    
    # Base case 1: Both subtrees are empty - they are mirrors
    if left is None and right is None:
        return True
    
    # Base case 2: One subtree is empty, other is not - not mirrors
    if left is None or right is None:
        return False
    
    # Base case 3: Values don't match - not mirrors
    if left.data != right.data:
        return False
    
    # Recursive case: Check if opposite sides are mirrors
    # Left's left should mirror Right's right
    # Left's right should mirror Right's left
    return (check_mirror(left.left, right.right) and 
            check_mirror(left.right, right.left))

def is_symmetric(root):
    """Main function to check if tree is symmetric"""
    
    # Edge case: empty tree is symmetric
    if root is None:
        return True
    
    # Check if left and right subtrees are mirrors
    return check_mirror(root.left, root.right)

# Example Usage
root = Node(1)
root.left = Node(2)
root.right = Node(2)
root.left.left = Node(3)
root.left.right = Node(4)
root.right.left = Node(4)
root.right.right = Node(3)

print(is_symmetric(root))  # Output: True
```

#### Complexity Analysis

- **Time Complexity**: O(n) - We visit each node at most once
- **Space Complexity**: O(h) - Recursion stack space where h is the height of the tree
  - Best case (balanced tree): O(log n)
  - Worst case (skewed tree): O(n)

---
