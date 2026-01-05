# Binary Search Tree (BST) Problems - Complete Guide

## 📑 Table of Contents

1. [Search in BST](#1-search-in-bst)
2. [Floor and Ceil in a BST](#2-floor-and-ceil-in-a-bst)
3. [Insert a given node in BST](#3-insert-a-given-node-in-bst)
4. [Delete a node in BST](#4-delete-a-node-in-bst)
5. [Kth Smallest and Largest element in BST](#5-kth-smallest-and-largest-element-in-bst)
6. [Check if a tree is a BST or not](#6-check-if-a-tree-is-a-bst-or-not)
7. [LCA in BST](#7-lca-in-bst)
8. [Construct a BST from a preorder traversal](#8-construct-a-bst-from-a-preorder-traversal)
9. [Inorder successor and predecessor in BST](#9-inorder-successor-and-predecessor-in-bst)
10. [BST iterator](#10-bst-iterator)
11. [Two sum in BST](#11-two-sum-in-bst)
12. [Correct BST with two nodes swapped](#12-correct-bst-with-two-nodes-swapped)
13. [Largest BST in Binary Tree](#13-largest-bst-in-binary-tree)

---

---

## 1. Search in BST

### Problem Description
Given the root of a binary search tree (BST) and an integer val. Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null.

### Sample Test Cases

**Example 1:**
- **Input:** `root = [4, 2, 7, 1, 3]`, `val = 2`
- **Output:** `[2, 1, 3]`
- **Explanation:**

<img src="https://static.takeuforward.org/content/ProblemSetter-i94avCYR" />

**Example 2:**
- **Input:** `root = [4, 2, 7, 1, 3]`, `val = 5`
- **Output:** `[]`
- **Explanation:**

<img src="https://static.takeuforward.org/content/ProblemSetter-wWcREyG-" />

### Approach: Iterative Search

#### Intuition
A Binary Search Tree has a special property: for any node, all values in its left subtree are smaller, and all values in its right subtree are larger. This means we can efficiently search by comparing the target value with the current node and moving left or right accordingly, just like binary search in a sorted array. We don't need to explore both sides - the BST property tells us exactly which direction to go.

#### Approach Steps
1. Start from the root node
2. While the current node exists and its value doesn't match the target:
   - If target value is less than current node's value, move to the left child
   - If target value is greater than current node's value, move to the right child
3. Return the current node (will be None if value not found, or the matching node if found)

#### Code
```python
class Solution:
    def searchBST(self, root, val):
        temp = root
        # Keep searching until we find the value or reach a null node
        while temp and temp.data != val:
            if val < temp.data:
                # Value is smaller, go left
                temp = temp.left
            else:
                # Value is larger, go right
                temp = temp.right
        return temp
```

#### Complexity Analysis
- **Time Complexity:** O(H) where H is the height of the tree
  - In the worst case (skewed tree): O(N)
  - In the best case (balanced tree): O(log N)
- **Space Complexity:** O(1) - only using a pointer variable

#### Visual Example
```
Searching for val = 2 in tree [4, 2, 7, 1, 3]

Step 1: Start at root (4)
        4
       / \
      2   7
     / \
    1   3
    ↑ temp = 4, val = 2
    2 < 4, go left

Step 2: Move to left child (2)
        4
       / \
     [2]  7
     / \
    1   3
    ↑ temp = 2, val = 2
    Found! Return node 2 with its subtree
```

---

## 2. Floor and Ceil in a BST

### Problem Description
Given a root of binary search tree and a key(node) value, find the floor and ceil value for that particular key value.
- **Floor Value Node:** Node with the greatest data lesser than or equal to the key value
- **Ceil Value Node:** Node with the smallest data larger than or equal to the key value

If a particular floor or ceil value is not present then output -1.

### Sample Test Cases

**Example 1:**
- **Input:** `root = [8, 4, 12, 2, 6, 10, 14]`, `key = 11`
- **Output:** `[10, 12]`
- **Explanation:**

<img src="https://static.takeuforward.org/content/ProblemSetter-eDbjy9Um" />

**Example 2:**
- **Input:** `root = [8, 4, 12, 2, 6, 10, 14]`, `key = 15`
- **Output:** `[14, -1]`
- **Explanation:**

<img src="https://static.takeuforward.org/content/ProblemSetter-D00IL4KV" />

### Approach: Two Separate Traversals

#### Intuition
Floor and ceil are like finding the "closest neighbors" to a number. Floor is the largest number that's still ≤ key, and ceil is the smallest number that's ≥ key. We can use the BST property to efficiently find both:
- For **floor**: When we go right (key > current), we found a potential floor candidate. Keep updating it as we search.
- For **ceil**: When we go left (key < current), we found a potential ceil candidate. Keep updating it as we search.

#### Approach Steps
1. **Finding Floor:**
   - Start from root, initialize floor = -1
   - If current node equals key, that's the floor
   - If key < current node, move left (potential floor must be smaller)
   - If key > current node, update floor = current node value, then move right (try to find something closer)

2. **Finding Ceil:**
   - Start from root, initialize ceil = -1
   - If current node equals key, that's the ceil
   - If key < current node, update ceil = current node value, then move left (try to find something closer)
   - If key > current node, move right (potential ceil must be larger)

#### Code
```python
class Solution:
    def floorCeilOfBST(self, root, key):
        # Finding Floor
        floor = -1
        temp = root
        while temp:
            if temp.data == key:
                # Exact match found
                floor = key
                break
            elif key < temp.data:
                # Key is smaller, go left
                temp = temp.left
            else:
                # Key is larger, current node is a potential floor
                floor = temp.data
                temp = temp.right
        
        # Finding Ceil
        ceil = -1
        temp = root
        while temp:
            if temp.data == key:
                # Exact match found
                ceil = key
                break
            elif key < temp.data:
                # Key is smaller, current node is a potential ceil
                ceil = temp.data
                temp = temp.left
            else:
                # Key is larger, go right
                temp = temp.right
        
        return [floor, ceil]
```

#### Complexity Analysis
- **Time Complexity:** O(2H) = O(H) where H is the height of the tree
  - We traverse the tree twice (once for floor, once for ceil)
- **Space Complexity:** O(1) - only using constant extra space

#### Visual Example
```
Finding floor and ceil for key = 11 in tree [8, 4, 12, 2, 6, 10, 14]

Tree Structure:
           8
         /   \
        4     12
       / \   /  \
      2   6 10  14

Finding Floor (largest ≤ 11):
Step 1: At 8, 11 > 8 → floor = 8, go right
Step 2: At 12, 11 < 12 → go left
Step 3: At 10, 11 > 10 → floor = 10, go right
Step 4: NULL → stop, floor = 10 ✓

Finding Ceil (smallest ≥ 11):
Step 1: At 8, 11 > 8 → go right
Step 2: At 12, 11 < 12 → ceil = 12, go left
Step 3: At 10, 11 > 10 → go right
Step 4: NULL → stop, ceil = 12 ✓

Result: [10, 12]
```

---

## 3. Insert a given node in BST

### Problem Description
Given the root node of a binary search tree (BST) and a value val to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

### Sample Test Cases

**Example 1:**
- **Input:** `root = [4, 2, 7, 1, 3]`, `val = 5`
- **Output:** `[4, 2, 7, 1, 3, 5]`
- **Explanation:** Below is image where the node 5 is inserted

<img src="https://static.takeuforward.org/content/ProblemSetter-E1RInkjk" />

There is another way to insert the given val as shown below.

<img src="https://static.takeuforward.org/content/ProblemSetter-d5Zymok1" />

**Example 2:**
- **Input:** `root = [40, 20, 60, 10, 30, 50, 70]`, `val = 25`
- **Output:** `[40, 20, 60, 10, 30, 50, 70, null, null, 25]`
- **Explanation:** Below is image where the node 25 is inserted

<img src="https://static.takeuforward.org/content/ProblemSetter-5oXCfvkZ" />

### Approach: Iterative Insertion

#### Intuition
In a BST, new nodes are always inserted as leaf nodes. We follow the BST property to find the correct position: go left if the value is smaller than current node, go right if larger. Once we reach a null position, that's where our new node belongs. We keep traversing until we find an empty spot (left or right child that is null) and place the new node there.

#### Approach Steps
1. Start from the root node
2. Run an infinite loop to find the insertion position:
   - If val < current node's value:
     - If left child exists, move to left child
     - If left child is null, insert new node here as left child and break
   - Else (val > current node's value):
     - If right child exists, move to right child
     - If right child is null, insert new node here as right child and break
3. Return the root (tree structure is modified but root remains same)

#### Code
```python
class Solution:
    def insertIntoBST(self, root, val):
        temp = root
        
        while True:
            if val < temp.data:
                # Value should go to left subtree
                if temp.left:
                    temp = temp.left
                else:
                    # Found the position, insert here
                    temp.left = TreeNode(val)
                    break
            else:
                # Value should go to right subtree
                if temp.right:
                    temp = temp.right
                else:
                    # Found the position, insert here
                    temp.right = TreeNode(val)
                    break
        
        return root
```

#### Complexity Analysis
- **Time Complexity:** O(H) where H is the height of the tree
  - In worst case (skewed tree): O(N)
  - In best case (balanced tree): O(log N)
- **Space Complexity:** O(1) - only using a pointer variable

#### Visual Example
```
Inserting val = 5 in tree [4, 2, 7, 1, 3]

Initial Tree:
        4
       / \
      2   7
     / \
    1   3

Step 1: At node 4
        val = 5, 5 > 4 → go right

Step 2: At node 7
        val = 5, 5 < 7 → go left

Step 3: At node 7's left (NULL)
        Found insertion point!
        Create new node with val = 5

Final Tree:
        4
       / \
      2   7
     / \ /
    1  3 5  ← New node inserted here

The BST property is maintained:
- 5 > 4 (right of 4) ✓
- 5 < 7 (left of 7) ✓
```

---

## 4. Delete a node in BST

### Problem Description
Given the root node of a binary search tree (BST) and a value key. Return the root node of the BST after the deletion of the node with the given key value.

### Sample Test Cases

**Example 1:**
- **Input:** `root = [5, 3, 6, 2, 4, null, 7]`, `key = 3`
- **Output:** `[5, 4, 6, 2, null, null, 7]`
- **Explanation:** Below is image of the original BST

<img src="https://static.takeuforward.org/content/ProblemSetter-PcCLLBxP" />

Below is image where the node 3 is deleted

<img src="https://static.takeuforward.org/content/ProblemSetter-kSeVvzcS" />

**Example 2:**
- **Input:** `root = [5, 3, 6, 2, 4, null, 7]`, `key = 0`
- **Output:** `[5, 3, 6, 2, 4, null, 7]`
- **Explanation:** The tree does not have node with value 0.

### Approach: Deletion with Reconnection

#### Intuition
Deleting a node from BST is tricky because we need to maintain the BST property. The strategy is:
1. If the node to delete has both children, we need to reconnect them properly
2. We attach the right subtree to the rightmost node of the left subtree
3. This ensures all left subtree values remain smaller, and right subtree values remain larger

The helper function handles three cases:
- Node has only right child → return right child
- Node has only left child → return left child  
- Node has both children → attach right subtree to rightmost of left subtree, return left subtree

#### Approach Steps
1. **Helper function** - handles reconnection after finding the node:
   - If only left child exists, return right child
   - If only right child exists, return left child
   - If both exist: find rightmost node in left subtree, attach right subtree there, return left subtree

2. **Main deletion logic:**
   - If root itself needs to be deleted, call helper on root
   - Otherwise, traverse to find the node to delete
   - Keep parent pointer to reconnect the tree after deletion
   - When found, replace that node with result from helper function

#### Code
```python
def findLastRight(node):
    # Find the rightmost node in the subtree
    if node.right is None:
        return node
    return findLastRight(node.right)

def helper(node):
    # Handle deletion of the node
    if node.left is None:
        # No left child, return right subtree
        return node.right
    if node.right is None:
        # No right child, return left subtree
        return node.left
    
    # Both children exist
    # Attach right subtree to rightmost of left subtree
    right = node.right
    lastRight = findLastRight(node.left)
    lastRight.right = right
    return node.left

def solve(root, key):
    if root is None:
        return root
    
    # Special case: root needs to be deleted
    if root.data == key:
        return helper(root)
    
    temp = root
    while temp:
        if key < temp.data:
            # Key is in left subtree
            if temp.left and temp.left.data == key:
                # Found the node to delete
                temp.left = helper(temp.left)
                break
            else:
                temp = temp.left
        else:
            # Key is in right subtree
            if temp.right and temp.right.data == key:
                # Found the node to delete
                temp.right = helper(temp.right)
                break
            else:
                temp = temp.right
    
    return root

class Solution:
    def deleteNode(self, root, key):
        return solve(root, key)
```

#### Complexity Analysis
- **Time Complexity:** O(H) where H is the height of the tree
  - Finding the node: O(H)
  - Finding rightmost node: O(H)
  - Overall: O(H)
- **Space Complexity:** O(1) for iterative approach (excluding recursion stack in findLastRight)

#### Visual Example
```
Deleting key = 3 from tree [5, 3, 6, 2, 4, null, 7]

Initial Tree:
        5
       / \
      3   6
     / \   \
    2   4   7

Step 1: Find node to delete (3)
        Found at left child of 5

Step 2: Node 3 has both children
        Left subtree: 2
        Right subtree: 4

Step 3: Find rightmost of left subtree
        Start from 2 → no right child
        So rightmost is 2 itself

Step 4: Attach right subtree (4) to rightmost (2)
        2.right = 4

Step 5: Return left subtree as replacement
        Replace node 3 with subtree rooted at 2

Final Tree:
        5
       / \
      2   6
       \   \
        4   7

The BST property is maintained:
- All values in left of 5 are < 5 ✓
- All values in right of 5 are > 5 ✓
```

---

## 5. Kth Smallest and Largest element in BST

### Problem Description
Given the root node of a binary search tree (BST) and an integer k. Return the kth smallest and largest value (1-indexed) of all values of the nodes in the tree.

### Sample Test Cases

**Example 1:**
- **Input:** `root = [3, 1, 4, null, 2]`, `k = 1`
- **Output:** `[1, 4]`
- **Explanation:**
  - The 1st smallest value in given BST is 1
  - The 1st largest value in given BST is 4

**Example 2:**
- **Input:** `root = [5, 3, 6, 2, null, null, null, 1]`, `k = 3`
- **Output:** `[3, 3]`
- **Explanation:**
  - The 3rd smallest value in given BST is 3
  - The 3rd largest value in given BST is 3

### Approach 1: Inorder Traversal with Array

#### Intuition
Inorder traversal of a BST gives us nodes in sorted order (ascending). If we store all nodes in an array, the kth smallest is simply at index k-1, and the kth largest is at index n-k (where n is total nodes). This is straightforward but uses extra space for the array.

#### Approach Steps
1. Perform inorder traversal (left → root → right) and store all values in array
2. The array will be sorted in ascending order
3. Kth smallest = array[k-1]
4. Kth largest = array[n-k] (where n is total elements)
5. Return both values

#### Code
```python
def solve(root, k):
    def inorder(node, order):
        if node is None:
            return
        # Left → Root → Right gives sorted order
        inorder(node.left, order)
        order.append(node.data)
        inorder(node.right, order)
    
    order = []
    inorder(root, order)
    n = len(order)
    
    # Kth smallest at index k-1
    smallest = order[k - 1]
    # Kth largest at index n-k
    largest = order[n - k]
    
    return [smallest, largest]
```

#### Complexity Analysis
- **Time Complexity:** O(N) - visit all nodes once
- **Space Complexity:** O(N) - store all nodes in array + O(H) recursion stack

---

### Approach 2: Optimized with Counter (Without Array)

#### Intuition
We can optimize space by not storing all elements. Instead, we use a counter during traversal:
- For kth smallest: do inorder (ascending), stop when counter reaches k
- For kth largest: do reverse inorder (descending), stop when counter reaches k

This way we only traverse until we find the answer, not the entire tree.

#### Approach Steps
1. **For kth smallest:**
   - Perform inorder traversal (left → root → right)
   - Increment counter at each node
   - When counter == k, store value and return

2. **For kth largest:**
   - Perform reverse inorder traversal (right → root → left)
   - Increment counter at each node
   - When counter == k, store value and return

#### Code
```python
def optimized(root, k):
    # Finding kth smallest
    def inorder(node):
        nonlocal cnt, smallest
        if node is None:
            return
        inorder(node.left)
        cnt += 1
        if cnt == k:
            smallest = node.data
            return
        inorder(node.right)
    
    smallest = -1
    cnt = 0
    inorder(root)
    
    # Finding kth largest (reverse inorder)
    def inorder_reverse(node):
        nonlocal cnt, largest
        if node is None:
            return
        inorder_reverse(node.right)  # Visit right first
        cnt += 1
        if cnt == k:
            largest = node.data
            return
        inorder_reverse(node.left)
    
    largest = -1
    cnt = 0
    inorder_reverse(root)
    
    return [smallest, largest]

# Note: Use Morris Traversal for even more optimization (O(1) space)
```

#### Complexity Analysis
- **Time Complexity:** O(N) in worst case, but can be O(k) if k is small
- **Space Complexity:** O(H) for recursion stack only (no array storage)

#### Visual Example
```
Finding 2nd smallest and 2nd largest in tree [3, 1, 4, null, 2]

Tree Structure:
      3
     / \
    1   4
     \
      2

Inorder Traversal (ascending): 1 → 2 → 3 → 4
Counter: 1 → 2 → 3 → 4

For k=2:
Step 1: Visit 1, cnt=1 (not k)
Step 2: Visit 2, cnt=2 (equals k!) → smallest = 2 ✓

Reverse Inorder (descending): 4 → 3 → 2 → 1
Counter: 1 → 2 → 3 → 4

For k=2:
Step 1: Visit 4, cnt=1 (not k)
Step 2: Visit 3, cnt=2 (equals k!) → largest = 3 ✓

Result: [2, 3]
```

---

## 6. Check if a tree is a BST or not

### Problem Description
Given the root node of a binary tree. Return true if the given binary tree is a binary search tree(BST) else false.

A valid BST is defined as follows:
- The left subtree of a node contains only nodes with key strictly less than the node's key
- The right subtree of a node contains only nodes with key strictly greater than the node's key
- Both the left and right subtrees must also be binary search trees

### Sample Test Cases

**Example 1:**
- **Input:** `root = [5, 3, 6, 2, 4, null, 7]`
- **Output:** `true`
- **Explanation:** Below is image of the given tree.

<img src="https://static.takeuforward.org/content/ProblemSetter-DDCNyKoE" />

**Example 2:**
- **Input:** `root = [5, 3, 6, 4, 2, null, 7]`
- **Output:** `false`
- **Explanation:** Below is image of the given tree. The node 4 and node 2 violates the BST rule of smaller to left and larger to right.

<img src="https://static.takeuforward.org/content/ProblemSetter-GETEpN6F" />

### Approach 1: Inorder Traversal Check

#### Intuition
The inorder traversal of a BST gives us elements in strictly ascending order. So we can do an inorder traversal, store all values, and check if the array is strictly sorted (each element should be greater than the previous one). If yes, it's a valid BST; otherwise, it's not.

#### Approach Steps
1. Perform inorder traversal of the tree
2. Store all node values in an array
3. Check if array is strictly sorted in ascending order
4. If any element is less than or equal to previous element, return false
5. If all elements are in strictly increasing order, return true

#### Code
```python
def solve(root):
    def inorder(node):
        nonlocal order
        if node is None:
            return
        inorder(node.left)
        order.append(node.data)
        inorder(node.right)
    
    order = []
    inorder(root)
    n = len(order)
    
    # Check if strictly sorted (ascending)
    for i in range(1, n):
        if order[i] <= order[i - 1]:
            return False
    
    return True
```

#### Complexity Analysis
- **Time Complexity:** O(N) - visit all nodes + check array
- **Space Complexity:** O(N) - store all nodes in array

---

### Approach 2: Range Validation (Optimal)

#### Intuition
Instead of storing all values, we can validate the BST property while traversing. Each node must be within a valid range:
- For the root: range is (-infinity, +infinity)
- For left child: range is (parent's min, parent's value)
- For right child: range is (parent's value, parent's max)

This way, we ensure every node satisfies the BST property relative to all its ancestors, not just its immediate parent.

#### Approach Steps
1. Start with root node and range (-∞, +∞)
2. For each node, check if its value is within the valid range
3. If not, return false immediately
4. Recursively validate left subtree with updated range (min, current)
5. Recursively validate right subtree with updated range (current, max)
6. Return true only if both subtrees are valid

#### Code
```python
def solve(root):
    def isValid(node, minVal, maxVal):
        if node is None:
            return True
        
        # Node value must be strictly within the range
        if node.data <= minVal or node.data >= maxVal:
            return False
        
        # Validate left subtree: values must be < node.data
        leftTree = isValid(node.left, minVal, node.data)
        # Validate right subtree: values must be > node.data
        rightTree = isValid(node.right, node.data, maxVal)
        
        return leftTree and rightTree
    
    return isValid(root, float('-inf'), float('inf'))
```

#### Complexity Analysis
- **Time Complexity:** O(N) - visit each node once
- **Space Complexity:** O(H) - recursion stack depth (height of tree)

#### Visual Example
```
Checking if tree [5, 3, 6, 4, 2, null, 7] is BST

Tree Structure:
        5
       / \
      3   6
     / \   \
    4   2   7

Using Range Validation:

Level 0: Node 5, Range(-∞, +∞)
         5 is within range ✓
         
Level 1: Node 3, Range(-∞, 5)
         3 is within range ✓
         Node 6, Range(5, +∞)
         6 is within range ✓

Level 2: Node 4, Range(-∞, 3)
         4 > 3 ✗ VIOLATION!
         4 should be < 3 for left subtree of 3
         
Result: FALSE - Not a valid BST

Why it fails:
- Node 4 is in left subtree of node 3
- But 4 > 3, violating BST property
- Even though locally 4 > 3 might seem ok for right child,
  it violates the global BST property
```

---

## Problem 7: LCA in BST

### 📋 Problem Description

Given the root node of a binary search tree (BST) and two node values `p` and `q`, return the lowest common ancestor (LCA) of the two nodes in BST.

The lowest common ancestor is defined as the lowest node in the tree that has both `p` and `q` as descendants (where a node can be a descendant of itself).

### 🧪 Sample Test Cases

#### Example 1
**Input:** `root = [5, 3, 6, 2, 4, null, 7]`, `p = 2`, `q = 4`  
**Output:** `[3]`  
**Explanation:** Below is the image of the BST

![BST Example 1](https://static.takeuforward.org/content/ProblemSetter-R7f_VyjM)

Node 3 is the lowest common ancestor of nodes 2 and 4 because both nodes are in the left subtree of node 3.

#### Example 2
**Input:** `root = [5, 3, 6, 2, 4, null, 7]`, `p = 2`, `q = 7`  
**Output:** `[5]`  
**Explanation:** Below is the image of the BST

![BST Example 2](https://static.takeuforward.org/content/ProblemSetter-WMGru_wO)

Node 5 is the lowest common ancestor because node 2 is in the left subtree and node 7 is in the right subtree.

---

### 💡 Approach: BST Property Based Solution

#### Intuition

The key insight here is to use the **special property of Binary Search Trees**: 
- All nodes in the **left subtree** have values **less than** the root
- All nodes in the **right subtree** have values **greater than** the root

Think of it this way: if you're standing at a node and both values you're looking for are smaller than your current node's value, then the LCA must be somewhere in the left subtree. Similarly, if both values are larger, the LCA must be in the right subtree.

But here's the magic moment: **if one value is smaller and one is larger** (or one equals the current node), then the **current node itself is the LCA**! This is because this is the split point where the two nodes diverge into different subtrees.

#### Approach Steps

1. **Start at the root node**
2. **Compare both p and q with the current node's value:**
   - If **both p and q are less** than the current node → Move to the **left subtree** (LCA must be on the left)
   - If **both p and q are greater** than the current node → Move to the **right subtree** (LCA must be on the right)
   - If **p and q are on different sides** (one ≤ current node ≤ other) → **Current node is the LCA**
3. **Return the LCA node**

#### Code

```python
def solve(root, p, q):
    def lca(node, p, q):
        # Base case: if we reach a null node, return None
        if(node is None):
            return None
        
        # If both values are smaller than current node
        # LCA must be in the left subtree
        if(p < node.data and q < node.data):
            return lca(node.left, p, q)
        
        # If both values are greater than current node
        # LCA must be in the right subtree
        elif(p > node.data and q > node.data):
            return lca(node.right, p, q)
        
        # If we reach here, it means p and q are on different sides
        # or one of them equals the current node
        # Current node is the LCA
        else:
            return node
    
    return lca(root, p, q)
```

#### ⏱️ Time Complexity: **O(H)**
- Where H is the height of the tree
- In the worst case (skewed tree): O(N) where N is the number of nodes
- In the best case (balanced BST): O(log N)
- We traverse from root to LCA, visiting at most H nodes

#### 💾 Space Complexity: **O(H)**
- Space used by the recursion stack
- In the worst case (skewed tree): O(N)
- In the best case (balanced BST): O(log N)

#### 📊 Visual Example Walkthrough

Let's trace through **Example 2** where we need to find LCA of `p = 2` and `q = 7`:

```
Tree Structure:
       5
      / \
     3   6
    / \   \
   2   4   7
```

**Step-by-step execution:**

```
Step 1: Start at root (5)
        p = 2 < 5 and q = 7 > 5
        → They're on different sides!
        → Return node 5 as LCA ✓

Result: Node 5 is the LCA
```

Let's trace **Example 1** where we need to find LCA of `p = 2` and `q = 4`:

```
Tree Structure:
       5
      / \
     3   6
    / \   \
   2   4   7
```

**Step-by-step execution:**

```
Step 1: Start at root (5)
        p = 2 < 5 and q = 4 < 5
        → Both are smaller!
        → Go to left subtree

Step 2: At node 3
        p = 2 < 3 and q = 4 > 3
        → They're on different sides!
        → Return node 3 as LCA ✓

Result: Node 3 is the LCA
```

**Recursion Flow Diagram:**

```
Example 1: LCA(2, 4)

                [5]
                 |
         p=2<5 and q=4<5
                 ↓
         Go LEFT subtree
                 |
                [3]
                 |
         p=2<3 and q=4>3
                 ↓
        FOUND! Return 3
```

---

## 8. Construct a BST from a preorder traversal

### Problem Description
Given an array of integers preorder, which represents the preorder traversal of a BST (i.e., binary search tree), construct the tree and return its root. It is guaranteed that it is always possible to find a binary search tree with the given requirements for the given test cases.

### Sample Test Cases

**Example 1:**
- **Input:** `preorder = [8, 5, 1, 7, 10, 12]`
- **Output:** `[8, 5, 10, 1, 7, null, 12]`
- **Explanation:** Below is the BST image

<img src="https://static.takeuforward.org/content/ProblemSetter-R4qDfEe_" />

**Example 2:**
- **Input:** `preorder = [1, 3]`
- **Output:** `[1, null, 3]`
- **Explanation:** Below is the BST image

<img src="https://static.takeuforward.org/content/ProblemSetter-VKY7IQpU" />

---

### Approach 1: Insert Each Node One by One

#### Intuition
The simplest approach is to insert each element from the preorder array one by one into the BST. We create the root with the first element, then for each subsequent element, we use the standard BST insertion process. This is straightforward but not the most efficient approach since each insertion takes O(H) time.

#### Approach Steps
1. Create root node with first element of preorder array
2. For each remaining element in the preorder array:
   - Insert it into the BST using standard insertion logic
   - Find the correct position by comparing with nodes
   - Place as left child if smaller, right child if larger
3. Return the constructed root

#### Code
```python
def insertNode(root, val):
    temp = root
    while True:
        if val < temp.data:
            # Value should go to left
            if temp.left:
                temp = temp.left
            else:
                # Found position, insert here
                temp.left = TreeNode(val)
                break
        else:
            # Value should go to right
            if temp.right:
                temp = temp.right
            else:
                # Found position, insert here
                temp.right = TreeNode(val)
                break
    return root

def solve(preorder):
    n = len(preorder)
    # Create root with first element
    root = TreeNode(preorder[0])
    # Insert remaining elements one by one
    for i in range(1, n):
        insertNode(root, preorder[i])
    return root
```

#### Complexity Analysis
- **Time Complexity:** O(N²) in worst case (skewed tree), O(N log N) in average case
  - Each insertion takes O(H) time, and we do N insertions
- **Space Complexity:** O(1) excluding the tree itself

---

### Approach 2: Using Inorder + Preorder

#### Intuition
We know that inorder traversal of a BST gives sorted order. So if we sort the preorder array, we get the inorder traversal. Then we can use the standard "construct tree from preorder and inorder" algorithm. This works because preorder tells us the root sequence, and inorder tells us the left-right split at each level.

#### Approach Steps
1. Sort the preorder array to get inorder traversal
2. Use the standard algorithm to build tree from preorder and inorder:
   - First element of preorder is always root
   - Find root's position in inorder to determine left/right subtree sizes
   - Recursively build left and right subtrees
3. Use a hashmap for O(1) lookup of positions in inorder array

#### Code
```python
def generateTree(preorder, inorder):
    n = len(preorder)
    
    def buildTree(preStart, preEnd, inStart, inEnd):
        """[0] Recursive helper to build tree from array ranges"""
        
        # [1] Base case: invalid range
        if preStart > preEnd or inStart > inEnd:
            return None
        
        # [2] Root is first element in preorder range
        root_val = preorder[preStart]
        root = TreeNode(root_val)
        
        # [3] Find how many nodes in left subtree
        # (distance from inStart to root position in inorder)
        numsLeft = map[root_val] - inStart
        
        # [4] Build left subtree
        # Preorder: preStart+1 to preStart+numsLeft
        # Inorder: inStart to root_position-1
        root.left = buildTree(
            preStart + 1,           # Skip root in preorder
            preStart + numsLeft,    # Include all left nodes
            inStart,                # Start of inorder range
            map[root_val] - 1       # Before root in inorder
        )
        
        # [5] Build right subtree
        # Preorder: after left subtree nodes to end
        # Inorder: after root to end
        root.right = buildTree(
            preStart + numsLeft + 1,  # After root and left nodes
            preEnd,                    # End of preorder range
            map[root_val] + 1,         # After root in inorder
            inEnd                      # End of inorder range
        )
        
        return root
    
    # [6] Create HashMap for O(1) index lookup in inorder
    map = {v: i for i, v in enumerate(inorder)}
    
    # [7] Build tree using full array ranges
    return buildTree(0, n - 1, 0, n - 1)

def solve(preorder):
    # Get inorder by sorting preorder (property of BST)
    inorder = sorted(preorder)
    return generateTree(preorder, inorder)
```

#### Complexity Analysis
- **Time Complexity:** O(N log N) for sorting + O(N) for building tree = O(N log N)
- **Space Complexity:** O(N) for hashmap + O(N) for inorder array + O(H) recursion stack

---

### Approach 3: Optimal - Using Upper Bound (Best Solution)

#### Intuition
We can construct the BST in one pass using the property that in preorder traversal, each node appears before its children. We use an upper bound to determine when to stop adding nodes to the left subtree. For each node, all nodes in its left subtree must be less than the node's value, and all nodes in its right subtree must be between the node's value and the upper bound inherited from its parent.

#### Approach Steps
1. Start with root having upper bound as infinity
2. For each element in preorder:
   - If current element exceeds upper bound, stop (it belongs to parent's right subtree)
   - Create node with current element
   - Recursively build left subtree with upper bound = current node's value
   - Recursively build right subtree with upper bound = parent's upper bound
3. Use a global index to track position in preorder array

#### Code
```python
def solve(preorder):
    n = len(preorder)
    
    def buildTree(upperBound):
        # Check if we've processed all elements or current element exceeds bound
        if index[0] == n or preorder[index[0]] > upperBound:
            return 
        
        # Create node with current element
        node = TreeNode(preorder[index[0]])
        index[0] += 1
        
        # Build left subtree: all values must be < node.data
        node.left = buildTree(node.data)
        # Build right subtree: all values must be < upperBound
        node.right = buildTree(upperBound)
        
        return node
    
    # Use list to maintain reference across recursive calls
    index = [0]
    root = buildTree(float('inf'))
    return root
```

#### Complexity Analysis
- **Time Complexity:** O(N) - each element is processed exactly once
- **Space Complexity:** O(H) for recursion stack where H is height of tree

#### Visual Example
```
Constructing BST from preorder = [8, 5, 1, 7, 10, 12]

Preorder: Root → Left → Right

Step-by-step construction:

Step 1: index=0, val=8, upperBound=∞
        Create node 8
        Build left (upperBound=8)
        
Step 2: index=1, val=5, upperBound=8
        5 < 8 ✓ Create node 5 as left of 8
        Build left (upperBound=5)
        
Step 3: index=2, val=1, upperBound=5
        1 < 5 ✓ Create node 1 as left of 5
        Build left (upperBound=1)
        
Step 4: index=3, val=7, upperBound=1
        7 > 1 ✗ Return (1 has no left child)
        Build right (upperBound=5)
        7 < 5 ✓ Create node 7 as right of 1
        
Step 5: index=4, val=10, upperBound=8
        10 > 8 ✗ Return (5's right subtree ends)
        Build right of 8 (upperBound=∞)
        10 < ∞ ✓ Create node 10 as right of 8
        
Step 6: index=5, val=12, upperBound=∞
        12 < ∞ ✓ Create node 12 as right of 10

Final Tree:
        8
       / \
      5   10
     / \    \
    1   7   12
```

---

## 9. Inorder successor and predecessor in BST

### Problem Description
Given the root node of a binary search tree (BST) and an integer key. Return the Inorder predecessor and successor of the given key from the provided BST.

**Note:** 
- Key will always present in given BST
- If predecessor or successor is missing then return -1

### Sample Test Cases

**Example 1:**
- **Input:** `root = [5, 2, 10, 1, 4, 7, 12]`, `key = 10`
- **Output:** `[7, 12]`
- **Explanation:**

<img src="https://static.takeuforward.org/content/ProblemSetter-8D_p20dq" />

**Example 2:**
- **Input:** `root = [5, 2, 10, 1, 4, 7, 12]`, `key = 12`
- **Output:** `[10, -1]`
- **Explanation:**

<img src="https://static.takeuforward.org/content/ProblemSetter-copgO0cS" />

---

### Approach 1: Inorder Traversal with Array

#### Intuition
The simplest approach is to perform inorder traversal, which gives us nodes in sorted order. Store all values in an array, find the position of the key, then predecessor is the previous element and successor is the next element. This is straightforward but uses extra space for the entire array.

#### Approach Steps
1. Perform inorder traversal and store all values in array
2. Use binary search (bisect_left) to find index of key
3. Predecessor = element at index-1 (or -1 if key is first)
4. Successor = element at index+1 (or -1 if key is last)
5. Return [predecessor, successor]

#### Code
```python
import bisect

def solve(root, key):
    def inorder(node):
        if node is None:
            return 
        inorder(node.left)
        order.append(node.data)
        inorder(node.right)
    
    order = []
    inorder(root)
    n = len(order)
    
    # Find index of key in sorted array
    index = bisect.bisect_left(order, key)
    
    # Check if key is last element
    if index == n - 1:
        successors = -1
    else:
        successors = order[index + 1]
    
    # Check if key is first element
    if index == 0:
        predcessors = -1
    else:
        predcessors = order[index - 1]
    
    return [predcessors, successors]
```

#### Complexity Analysis
- **Time Complexity:** O(N) for inorder traversal + O(log N) for binary search = O(N)
- **Space Complexity:** O(N) for storing all nodes in array

---

### Approach 2: Track Previous During Inorder

#### Intuition
We can optimize space by not storing all elements. During inorder traversal, we track the previous node. When we reach the key node, the previous node is the predecessor. For successor, we continue traversal and the first node greater than key is the successor. This approach uses O(1) extra space besides recursion stack.

#### Approach Steps
1. Perform inorder traversal while tracking previous node
2. When current node equals key:
   - Previous node is the predecessor
3. When current node is greater than key and successor not found yet:
   - Current node is the successor
4. Update previous node as we traverse
5. Return [predecessor, successor]

#### Code
```python
def solve(root, key):
    def inorder(node):
        nonlocal pre, suc
        if node is None:
            return
        
        inorder(node.left)
        
        current = node.data
        # If previous exists and current is key, previous is predecessor
        if prev[0] and current == key:
            pre = prev[0]
        # If successor not found and current > key, it's the successor
        if suc == -1 and current > key:
            suc = current
        
        # Update previous
        prev[0] = current
        
        inorder(node.right)
    
    pre, suc = -1, -1
    prev = [None]
    inorder(root)
    
    return [pre, suc]
```

#### Complexity Analysis
- **Time Complexity:** O(N) - visit all nodes in worst case
- **Space Complexity:** O(H) for recursion stack only

---

### Approach 3: Optimal - BST Property Based (Best Solution)

#### Intuition
We can use BST properties to find predecessor and successor efficiently without traversing all nodes. The key insights are:
- **Predecessor:** Largest value smaller than key. Either found by going left from key, or it's an ancestor we passed while going right.
- **Successor:** Smallest value larger than key. Either found by going right from key, or it's an ancestor we passed while going left.

#### Approach Steps
1. **Finding Predecessor:**
   - Traverse to find key node while tracking potential predecessors
   - When going right, update predecessor (current node < key)
   - Once key found, if it has left child, predecessor is rightmost node of left subtree

2. **Finding Successor:**
   - Traverse to find key node while tracking potential successors
   - When going left, update successor (current node > key)
   - Once key found, if it has right child, successor is leftmost node of right subtree

#### Code
```python
def solve(root, key):
    pred, succ = -1, -1
    temp = root
    
    # Find key and track predecessor/successor along the way
    while temp:
        if key < temp.data:
            # Going left, current could be successor
            succ = temp.data
            temp = temp.left
        elif key > temp.data:
            # Going right, current could be predecessor
            pred = temp.data
            temp = temp.right
        else:
            # Found the key node
            # Check for predecessor in left subtree
            if temp.left:
                curr = temp.left
                # Rightmost node of left subtree
                while curr.right:
                    curr = curr.right
                pred = curr.data
            
            # Check for successor in right subtree
            if temp.right:
                curr = temp.right
                # Leftmost node of right subtree
                while curr.left:
                    curr = curr.left
                succ = curr.data
            
            break
    
    return [pred, succ]
```

#### Complexity Analysis
- **Time Complexity:** O(H) where H is height of tree
  - Best case (balanced): O(log N)
  - Worst case (skewed): O(N)
- **Space Complexity:** O(1) - only using pointers

#### Visual Example
```
Finding predecessor and successor for key=10 in tree [5, 2, 10, 1, 4, 7, 12]

Tree Structure:
         5
       /   \
      2     10
     / \   /  \
    1   4 7   12

Finding Predecessor (largest < 10):
Step 1: At 5, 10 > 5 → pred=5, go right
Step 2: At 10, found key!
        Has left child (7)
        Find rightmost of left subtree
Step 3: At 7, no right child
        pred = 7 ✓

Finding Successor (smallest > 10):
Step 1: At 5, 10 > 5 → go right
Step 2: At 10, found key!
        Has right child (12)
        Find leftmost of right subtree
Step 3: At 12, no left child
        succ = 12 ✓

Result: [7, 12]
```

---

## 10. BST iterator

### Problem Description
Implement the BSTIterator class that represents an iterator over the in-order traversal of a binary search tree (BST):
- `BSTIterator(TreeNode root)` - Initializes an object of the BSTIterator class
- `boolean hasNext()` - Returns true if there exists a number in the traversal to the right of the pointer
- `int next()` - Moves the pointer to the right, then returns the number at the pointer

### Sample Test Cases

**Example 1:**
- **Input:** `["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]`, `root = [7, 3, 15, null, null, 9, 20]`
- **Output:** `[3, 7, true, 9, true, 15, true, 20, false]`
- **Explanation:**
```
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();      // return 3
bSTIterator.next();      // return 7
bSTIterator.hasNext();   // return True
bSTIterator.next();      // return 9
bSTIterator.hasNext();   // return True
bSTIterator.next();      // return 15
bSTIterator.hasNext();   // return True
bSTIterator.next();      // return 20
bSTIterator.hasNext();   // return False
```

**Example 2:**
- **Input:** `["BSTIterator", "next", "next", "next", "hasNext", "next", "hasNext"]`, `root = [7, 3, 15, null, null, 9, 20]`
- **Output:** `[3, 7, 9, true, 15, true]`

---

### Approach 1: Store All Elements (Simple)

#### Intuition
The simplest approach is to perform inorder traversal during initialization and store all elements in an array. Then we just iterate through this array using an index pointer. The `next()` operation is O(1) and `hasNext()` is O(1), but we use O(N) space and the initialization takes O(N) time.

#### Approach Steps
1. During initialization, perform complete inorder traversal
2. Store all elements in array in sorted order
3. Maintain an index pointer starting at 0
4. `hasNext()` checks if index < size of array
5. `next()` returns element at current index and increments it

#### Code
```python
class BSTIterator:
    def __init__(self, root):
        """
        :type root: TreeNode
        :rtype: None
        """
        def inorder(node):
            if node is None:
                return 
            inorder(node.left)
            self.order.append(node.data)
            inorder(node.right)
        
        self.order = []
        inorder(root)
        self.size = len(self.order)
        self.index = 0
    
    def hasNext(self):
        """
        :rtype: boolean
        """
        return self.index < self.size
    
    def next(self):
        """
        :rtype: int
        """
        data = self.order[self.index]
        self.index += 1
        return data
```

#### Complexity Analysis
- **Time Complexity:** 
  - Constructor: O(N)
  - hasNext(): O(1)
  - next(): O(1)
- **Space Complexity:** O(N) for storing all elements

---

### Approach 2: Controlled Inorder with Stack (Optimal)

#### Intuition
We can optimize space by simulating inorder traversal using a stack. Instead of storing all elements, we only store the path to the current node. We push all left children onto the stack, and when we pop a node, we push all left children of its right subtree. This gives us O(H) space complexity while maintaining O(1) average time for next().

#### Approach Steps
1. During initialization:
   - Create empty stack
   - Push all left nodes starting from root onto stack
2. `hasNext()`: check if stack is not empty
3. `next()`:
   - Pop top node from stack (this is the next smallest)
   - If popped node has right child, push all left nodes of right subtree
   - Return popped node's value

#### Code
```python
class BSTIterator:
    def __init__(self, root):
        """
        :type root: TreeNode
        :rtype: None
        """
        self.stack = []
        self.pushAll(root)
    
    def hasNext(self):
        """
        :rtype: boolean
        """
        return bool(self.stack)
    
    def next(self):
        """
        :rtype: int
        """
        # Pop the top node (next smallest element)
        node = self.stack.pop()
        # If it has right child, push all left nodes of right subtree
        if node.right:
            self.pushAll(node.right)
        return node.data
    
    def pushAll(self, node):
        """Push all left nodes starting from given node"""
        temp = node
        while temp:
            self.stack.append(temp)
            temp = temp.left
```

#### Complexity Analysis
- **Time Complexity:**
  - Constructor: O(H) where H is height
  - hasNext(): O(1)
  - next(): O(1) amortized (each node pushed and popped exactly once)
- **Space Complexity:** O(H) for stack (height of tree)

#### Visual Example
```
BST Iterator for tree [7, 3, 15, null, null, 9, 20]

Tree Structure:
        7
       / \
      3   15
         /  \
        9   20

Initialization: pushAll(7)
Stack: [7, 3] (push all left nodes)

next() call 1:
  Pop 3
  3 has no right child
  Return 3
  Stack: [7]

next() call 2:
  Pop 7
  7 has right child (15)
  pushAll(15) → push [15, 9]
  Return 7
  Stack: [15, 9]

hasNext() call:
  Stack not empty
  Return true

next() call 3:
  Pop 9
  9 has no right child
  Return 9
  Stack: [15]

next() call 4:
  Pop 15
  15 has right child (20)
  pushAll(20) → push [20]
  Return 15
  Stack: [20]

next() call 5:
  Pop 20
  20 has no right child
  Return 20
  Stack: []

hasNext() call:
  Stack is empty
  Return false
```

---

## 11. Two sum in BST

### Problem Description
Given the root of a binary search tree and an integer k. Return true if there exist two elements in the BST such that their sum is equal to k otherwise false.

### Sample Test Cases

**Example 1:**
- **Input:** `root = [5, 3, 6, 2, 4, null, 7]`, `k = 9`
- **Output:** `true`
- **Explanation:** The BST contains multiple pair of nodes that sum up to k
  - 3 + 6 => 9
  - 5 + 4 => 9
  - 2 + 7 => 9

**Example 2:**
- **Input:** `root = [5, 3, 6, 2, 4, null, 7]`, `k = 14`
- **Output:** `false`
- **Explanation:** There is no pair in given BST that sum up to k

---

### Approach: Two Iterators (Optimal)

#### Intuition
We can use the two-pointer technique on BST by using two iterators - one for ascending order (inorder) and one for descending order (reverse inorder). This is similar to the two-sum problem in sorted array. We start with the smallest and largest elements, and based on their sum, we move the appropriate pointer. This gives us O(H) space complexity instead of O(N).

#### Approach Steps
1. Create two BST iterators:
   - One for ascending order (normal inorder: left → root → right)
   - One for descending order (reverse inorder: right → root → left)
2. Initialize left pointer to smallest element, right pointer to largest
3. While left < right:
   - Calculate sum of left and right
   - If sum equals target, return true
   - If sum < target, move left pointer forward (get next smallest)
   - If sum > target, move right pointer backward (get next largest)
4. If no pair found, return false

#### Code
```python
class BSTIterator:
    def __init__(self, root, reverse=False):
        """
        :type root: TreeNode
        :rtype: None
        """
        self.reverse = reverse
        self.stack = []
        self.pushAll(root)
    
    def hasNext(self):
        """
        :rtype: boolean
        """
        return bool(self.stack)
    
    def next(self):
        """
        :rtype: int
        """
        node = self.stack.pop()
        # Based on direction, push appropriate subtree
        if self.reverse:
            self.pushAll(node.left)
        else:
            self.pushAll(node.right)
        return node.data
    
    def pushAll(self, node):
        temp = node
        while temp:
            self.stack.append(temp)
            # Push right for descending, left for ascending
            if self.reverse:
                temp = temp.right
            else:
                temp = temp.left

def solve(root, target):
    # Create ascending iterator (smallest first)
    iterAsc = BSTIterator(root)
    # Create descending iterator (largest first)
    iterDesc = BSTIterator(root, True)
    
    # Initialize two pointers
    left, right = iterAsc.next(), iterDesc.next()
    
    # Two-pointer technique
    while left < right:
        sum = left + right
        if sum == target:
            return True
        if sum < target:
            # Need larger sum, move left pointer
            left = iterAsc.next()
        else:
            # Need smaller sum, move right pointer
            right = iterDesc.next()
    
    return False
```

#### Complexity Analysis
- **Time Complexity:** O(N) in worst case (visit all nodes once)
- **Space Complexity:** O(H) for two stacks (height of tree)

#### Visual Example
```
Finding if pair exists with sum=9 in tree [5, 3, 6, 2, 4, null, 7]

Tree Structure:
        5
       / \
      3   6
     / \   \
    2   4   7

Inorder (ascending): 2, 3, 4, 5, 6, 7
Reverse Inorder (descending): 7, 6, 5, 4, 3, 2

Iteration:
Step 1: left=2, right=7, sum=9
        sum == target ✓
        Return true

Alternative pairs if we continued:
- 3 + 6 = 9
- 4 + 5 = 9
- 2 + 7 = 9

All these pairs sum to 9!
```

---

# 🌳 Binary Search Tree Problems - Comprehensive Guide

---

## 📑 Table of Contents

1. [Problem 7: LCA in BST](#problem-7-lca-in-bst)
2. [Problem 12: Correct BST with Two Nodes Swapped](#problem-12-correct-bst-with-two-nodes-swapped)
3. [Problem 13: Largest BST in Binary Tree](#problem-13-largest-bst-in-binary-tree)

---

## Problem 12: Correct BST with Two Nodes Swapped

### 📋 Problem Description

Given the root of a binary search tree (BST), where the values of exactly **two nodes** of the tree were swapped by mistake, recover the tree **without changing its structure**.

### 🧪 Sample Test Cases

#### Example 1
**Input:** `root = [1, 3, null, null, 2]`  
**Output:** `[3, 1, null, null, 2]`  
**Explanation:** 3 cannot be a left child of 1 because 3 > 1. Swapping 1 and 3 makes the BST valid.

**Before Recovery:**

![Before Recovery 1](https://static.takeuforward.org/content/ProblemSetter-H4VTAoRO)

**After Recovery:**

![After Recovery 1](https://static.takeuforward.org/content/ProblemSetter-pNPXRXCn)

#### Example 2
**Input:** `root = [3, 1, 4, null, null, 2]`  
**Output:** `[2, 1, 4, null, null, 3]`  
**Explanation:** 2 cannot be in the right subtree of 3 because 2 < 3. Swapping 2 and 3 makes the BST valid.

**Tree Visualization:**

![BST Example 2](https://static.takeuforward.org/content/ProblemSetter-kVLnOMb9)

---

### 💡 Approach 1: Inorder Traversal with Sorting

#### Intuition

This approach uses a simple but powerful idea: **the inorder traversal of a BST gives us a sorted sequence**. 

Think about it: if two nodes are swapped in a BST, the inorder traversal will have exactly two elements out of place. For example, if the correct sorted order is `[1, 2, 3, 4, 5]` but two nodes were swapped, we might get `[1, 4, 3, 2, 5]`.

The strategy is straightforward:
1. Get the inorder traversal (which will be "almost sorted" with 2 elements swapped)
2. Sort this array to get the correct order
3. Traverse the tree again in inorder and replace each node's value with the corresponding value from the sorted array

This is like taking a scrambled deck of cards, remembering the order, sorting them, and then placing them back in the correct order.

#### Approach Steps

1. **Perform inorder traversal** and store all node values in an array
2. **Sort the array** to get the correct BST order
3. **Perform inorder traversal again** and replace each node's value with values from the sorted array in sequence
4. **Return the corrected tree**

#### Code

```python
def solve(root):
    def inorder(node):
        # Base case: if node is None, return
        if(node is None):
            return 
        
        # Traverse left subtree
        inorder(node.left)
        
        # Store current node's data
        order.append(node.data)
        
        # Traverse right subtree
        inorder(node.right)
    
    # Step 1: Get inorder traversal
    order = []
    inorder(root)
    
    # Step 2: Sort to get correct BST order
    order.sort()
    
    # Step 3: Replace node values with sorted values
    def inorder(node):
        nonlocal index
        if(node is None):
            return 
        
        # Traverse left subtree
        inorder(node.left)
        
        # Replace node value if different from sorted order
        if(order[index] != node.data):
            node.data = order[index]
        index += 1
        
        # Traverse right subtree
        inorder(node.right)
    
    index = 0
    inorder(root)
    return root
```

#### ⏱️ Time Complexity: **O(N log N)**
- First inorder traversal: O(N)
- Sorting the array: O(N log N)
- Second inorder traversal: O(N)
- Overall: O(N log N) dominated by sorting

#### 💾 Space Complexity: **O(N)**
- Array to store inorder traversal: O(N)
- Recursion stack: O(H) where H is height
- Overall: O(N)

---

### 💡 Approach 2: Single Pass with Swapped Node Detection (Optimal)

#### Intuition

This is a brilliant approach that leverages the BST property more cleverly. Let's understand the core insight:

In a correct BST, during inorder traversal, **each node's value should be greater than the previous node's value**. When two nodes are swapped, this property is violated.

**Two scenarios can occur:**

1. **Adjacent nodes swapped** (e.g., [1, 3, 2, 4, 5] → 3 and 2 swapped)
   - We'll find ONE violation: prev=3 > current=2
   - The two swapped nodes are `prev` and `current`

2. **Non-adjacent nodes swapped** (e.g., [1, 5, 3, 4, 2, 6] → 5 and 2 swapped)
   - We'll find TWO violations:
     - First: prev=5 > current=3 → Remember prev (5)
     - Second: prev=4 > current=2 → Remember current (2)
   - The two swapped nodes are the `prev` from first violation and `current` from second violation

#### Approach Steps

1. **Initialize pointers:** `prev`, `first`, `first_next`, `last` as None
2. **Perform inorder traversal** while tracking previous node
3. **Detect violations:**
   - When `current < prev` (BST property violated):
     - If this is the **first violation**: Store `prev` as `first` and `current` as `first_next`
     - If this is the **second violation**: Store `current` as `last`
4. **Swap the identified nodes:**
   - If both violations found: Swap `first` and `last`
   - If only one violation: Swap `first` and `first_next` (adjacent swap case)
5. **Return the corrected tree**

#### Code

```python
def solve(root):
    def inorder(node):
        nonlocal prev, first, first_next, last
        
        # Base case
        if(node is None):
            return
        
        # Traverse left subtree
        inorder(node.left)
        
        # Check if current node violates BST property
        if(prev and node.data < prev.data):
            # First violation found
            if(first is None):
                first = prev           # Store the larger node from first violation
                first_next = node      # Store the smaller node from first violation
            # Second violation found
            else:
                last = node            # Store the smaller node from second violation
        
        # Update previous node
        prev = node
        
        # Traverse right subtree
        inorder(node.right)
    
    # Initialize tracking variables
    prev, first, first_next, last = None, None, None, None
    
    # Perform inorder traversal to detect swapped nodes
    inorder(root)
    
    # Swap the identified nodes
    if(first and last):
        # Non-adjacent swap case: swap first and last
        first.data, last.data = last.data, first.data
    elif(first and first_next):
        # Adjacent swap case: swap first and first_next
        first.data, first_next.data = first_next.data, first.data
    
    return root
```

#### ⏱️ Time Complexity: **O(N)**
- Single inorder traversal visiting each node once
- Much better than Approach 1's O(N log N)

#### 💾 Space Complexity: **O(H)**
- Only recursion stack space needed
- No additional array storage required
- Where H is the height of the tree

#### 📊 Visual Example Walkthrough

**Example: Non-adjacent swap** - `[1, 5, 3, 4, 2, 6]`

```
Correct BST should be: [1, 2, 3, 4, 5, 6]
Swapped nodes: 5 and 2

Inorder traversal simulation:

Node: 1  →  prev=None, current=1     ✓ OK
Node: 5  →  prev=1, current=5        ✓ OK (5 > 1)
Node: 3  →  prev=5, current=3        ✗ VIOLATION! (3 < 5)
              first = 5
              first_next = 3
              
Node: 4  →  prev=3, current=4        ✓ OK (4 > 3)
Node: 2  →  prev=4, current=2        ✗ VIOLATION! (2 < 4)
              last = 2
              
Node: 6  →  prev=2, current=6        ✓ OK (6 > 2)

Result: Swap first (5) with last (2)
```

**Example: Adjacent swap** - `[1, 3, 2, 4, 5]`

```
Correct BST should be: [1, 2, 3, 4, 5]
Swapped nodes: 3 and 2 (adjacent)

Inorder traversal simulation:

Node: 1  →  prev=None, current=1     ✓ OK
Node: 3  →  prev=1, current=3        ✓ OK (3 > 1)
Node: 2  →  prev=3, current=2        ✗ VIOLATION! (2 < 3)
              first = 3
              first_next = 2
              
Node: 4  →  prev=2, current=4        ✓ OK (4 > 2)
Node: 5  →  prev=4, current=5        ✓ OK (5 > 4)

Result: Only one violation found
        Swap first (3) with first_next (2)
```

**Recursion Flow Diagram:**

```
Tree: [3, 1, 4, null, null, 2]
      
         3
        / \
       1   4
          /
         2

Inorder traversal: 1 → 3 → 2 → 4

Step 1: prev=None, node=1
        ✓ No violation

Step 2: prev=1, node=3
        ✓ 3 > 1 (OK)

Step 3: prev=3, node=2
        ✗ 2 < 3 (VIOLATION!)
        first = 3
        first_next = 2

Step 4: prev=2, node=4
        ✓ 4 > 2 (OK)

Result: Only one violation
        → Adjacent swap case
        → Swap first (3) and first_next (2)
        → New tree: [2, 1, 4, null, null, 3] ✓
```

---

## Problem 13: Largest BST in Binary Tree

### 📋 Problem Description

Given the root of a Binary Tree where nodes have integer values, return the **size of the largest subtree** that is also a valid BST.

A binary search tree (BST) is a binary tree with the following properties:
- The left subtree of a node contains only nodes with values **less than** the node's value
- The right subtree of a node contains only nodes with values **greater than** the node's value
- Both the left and right subtrees must also be binary search trees

### 🧪 Sample Test Cases

#### Example 1
**Input:** `root = [2, 1, 3]`  
**Output:** `3`  
**Explanation:** The given complete binary tree is a BST consisting of 3 nodes.

```
     2
    / \
   1   3
```

All three nodes form a valid BST, so the answer is 3.

#### Example 2
**Input:** `root = [10, null, 20, null, 30, null, 40, null, 50]`  
**Output:** `5`  
**Explanation:** If we consider node 10 as the root node, it forms the largest BST.

```
    10
      \
       20
        \
         30
          \
           40
            \
             50
```

The entire tree is a BST (all nodes in right subtree are greater), so the answer is 5.

---

### 💡 Approach 1: Validate Each Subtree

#### Intuition

This is a straightforward brute-force approach that checks every possible subtree to see if it's a valid BST.

The idea is simple:
1. For each node in the tree, check if the subtree rooted at that node is a valid BST
2. If it is valid, count how many nodes are in that subtree
3. Keep track of the maximum count found

To validate a BST, we check if all nodes in the left subtree are less than the root and all nodes in the right subtree are greater than the root. We also need to pass down minimum and maximum bounds to ensure the entire subtree maintains BST properties.

#### Approach Steps

1. **Create a helper function `countNodes`** to count nodes in a subtree using inorder traversal
2. **Create a helper function `validateBST`** to check if a subtree is a valid BST:
   - Check if current node's value is within the allowed range (minValue < node.data < maxValue)
   - Recursively validate left subtree with updated max bound
   - Recursively validate right subtree with updated min bound
3. **Perform inorder traversal** of the entire tree:
   - For each node, validate if its subtree is a BST
   - If valid, count its nodes and update maximum
4. **Return the maximum size found**

#### Code

```python
def countNodes(root):
    def inorder(node):
        nonlocal cnt
        # Base case
        if(node is None):
            return
        
        # Count nodes using inorder traversal
        inorder(node.left)
        cnt += 1  # Count current node
        inorder(node.right)
    
    cnt = 0
    inorder(root)
    return cnt

def solve(root):
    def validateBST(node, minValue, maxValue):
        # Base case: empty tree is a valid BST
        if(node is None):
            return True
        
        # Check if current node violates BST property
        if(not minValue < node.data < maxValue):
            return False
        
        # Recursively validate left subtree (all values must be < node.data)
        leftTree = validateBST(node.left, minValue, node.data)
        
        # Recursively validate right subtree (all values must be > node.data)
        rightTree = validateBST(node.right, node.data, maxValue)
        
        # Both subtrees must be valid BSTs
        return leftTree and rightTree
    
    maxi = 1  # At minimum, a single node is a valid BST
    
    def inorder(node):
        nonlocal maxi
        if(node is None):
            return
        
        # Check left subtree
        inorder(node.left)
        
        # Validate current subtree
        valid = validateBST(node, float('-inf'), float('inf'))
        if(valid):
            # Count nodes in this valid BST
            cnt = countNodes(node)
            maxi = max(maxi, cnt)
        
        # Check right subtree
        inorder(node.right)
    
    inorder(root)
    return maxi
```

#### ⏱️ Time Complexity: **O(N²)**
- For each of N nodes, we potentially validate the entire subtree (O(N))
- In the worst case, we validate N nodes for each of N nodes
- Very inefficient for large trees

#### 💾 Space Complexity: **O(N)**
- Recursion stack for inorder traversal: O(H)
- Recursion stack for validation: O(H)
- Overall: O(N) in worst case for skewed tree

---

### 💡 Approach 2: Bottom-Up with Node Info (Optimal)

#### Intuition

This is an elegant solution that solves the problem in a single traversal using a bottom-up approach. The key insight is to **collect and pass information upward** from child nodes to parent nodes.

Think of it like this: instead of checking each subtree separately (top-down), we build the answer from the leaves up to the root (bottom-up). At each node, we ask three questions:
1. What's the **minimum value** in my subtree?
2. What's the **maximum value** in my subtree?
3. What's the **size of the largest BST** in my subtree?

A subtree rooted at a node is a valid BST if:
- The **maximum value in the left subtree < node's value**
- The **minimum value in the right subtree > node's value**
- Both children are valid BSTs

If these conditions are met, the current subtree is a BST with size = left_size + right_size + 1. If not, we return the maximum BST size found in either child.

#### Approach Steps

1. **Create a Node class** to store three pieces of information:
   - `minVal`: Minimum value in the subtree
   - `maxVal`: Maximum value in the subtree
   - `maxSize`: Size of the largest BST in the subtree

2. **Implement recursive function `largestBST`:**
   - **Base case**: If node is None, return Node with:
     - minVal = infinity (so any real value is smaller)
     - maxVal = -infinity (so any real value is larger)
     - maxSize = 0
   
   - **Recursive case**:
     - Get info from left child
     - Get info from right child
     - **Check if current subtree is a BST:**
       - left.maxVal < node.data < right.minVal
     - **If valid BST:**
       - minVal = min(node.data, left.minVal)
       - maxVal = max(node.data, right.maxVal)
       - maxSize = left.maxSize + right.maxSize + 1
     - **If not valid BST:**
       - Return invalid marker with max size from children

3. **Return the maxSize from the root's Node info**

#### Code

```python
class Node:
    def __init__(self, minVal, maxVal, maxSize):
        self.minVal = minVal    # Minimum value in subtree
        self.maxVal = maxVal    # Maximum value in subtree
        self.maxSize = maxSize  # Size of largest BST in subtree

def solve(root):
    def largestBST(node):
        # Base case: empty tree is a valid BST of size 0
        if node is None:
            return Node(float('inf'), float('-inf'), 0)
        
        # Get information from left and right subtrees
        left = largestBST(node.left)
        right = largestBST(node.right)

        # Check if current subtree is a valid BST
        if(left.maxVal < node.data < right.minVal):
            # Current subtree IS a valid BST
            return Node(
                min(node.data, left.minVal),      # Minimum value in current subtree
                max(node.data, right.maxVal),     # Maximum value in current subtree
                left.maxSize + right.maxSize + 1  # Total size of BST
            )
        
        # Current subtree is NOT a valid BST
        # Return invalid marker but preserve max BST size found in children
        return Node(
            float('-inf'),                        # Invalid marker (min > max)
            float('inf'),                         # Invalid marker (max < min)
            max(left.maxSize, right.maxSize)     # Best BST size from children
        )
    
    return largestBST(root).maxSize
```

#### ⏱️ Time Complexity: **O(N)**
- Visit each node exactly once
- Constant work at each node
- Much better than Approach 1's O(N²)

#### 💾 Space Complexity: **O(H)**
- Recursion stack space where H is the height
- O(log N) for balanced tree, O(N) for skewed tree
- No additional data structures needed

#### 📊 Visual Example Walkthrough

**Example: Mixed tree with some valid BST subtrees**

```
Tree Structure:
         50
        /  \
       30   60
      /  \   \
     5   20  70
```

Let's trace the bottom-up execution:

```
Step 1: Process leaf nodes (5, 20, 70)
        
        Node 5:  left=null, right=null
                 → Valid BST
                 → Return Node(5, 5, 1)
        
        Node 20: left=null, right=null
                 → Valid BST
                 → Return Node(20, 20, 1)
        
        Node 70: left=null, right=null
                 → Valid BST
                 → Return Node(70, 70, 1)

Step 2: Process node 30
        
        left.maxVal=5, node.data=30, right.minVal=20
        Check: 5 < 30 < 20? → FALSE! (30 > 20)
        → NOT a valid BST
        → Return Node(-inf, inf, max(1, 1)) = Node(-inf, inf, 1)

Step 3: Process node 60
        
        left=null (no left child)
        right.minVal=70, node.data=60
        Check: -inf < 60 < 70? → TRUE! ✓
        → Valid BST
        → Return Node(60, 70, 2)  [size = 0 + 1 + 1]

Step 4: Process root 50
        
        left.maxVal=inf (invalid), node.data=50, right.minVal=60
        Check: inf < 50 < 60? → FALSE!
        → NOT a valid BST
        → Return Node(-inf, inf, max(1, 2)) = Node(-inf, inf, 2)

Final Answer: 2 (the subtree rooted at 60)
```

**Recursion Flow Diagram:**

```
Tree:    10
           \
            20
             \
              30
               \
                40
                 \
                  50

Bottom-Up Processing:

Level 5: [50]
         → Node(50, 50, 1)

Level 4: [40]
         → left=null, right=(50,50,1)
         → Check: -inf < 40 < 50? ✓
         → Node(40, 50, 2)

Level 3: [30]
         → left=null, right=(40,50,2)
         → Check: -inf < 30 < 40? ✓
         → Node(30, 50, 3)

Level 2: [20]
         → left=null, right=(30,50,3)
         → Check: -inf < 20 < 30? ✓
         → Node(20, 50, 4)

Level 1: [10]
         → left=null, right=(20,50,4)
         → Check: -inf < 10 < 20? ✓
         → Node(10, 50, 5)

Result: maxSize = 5 (entire tree is a BST)
```

**Step-by-Step Example with Invalid BST:**

```
Tree:     10
         /  \
        5    15
            /  \
           6   20

Processing order (post-order):

1. Process Node(5):   → Node(5, 5, 1)
2. Process Node(6):   → Node(6, 6, 1)
3. Process Node(20):  → Node(20, 20, 1)

4. Process Node(15):
   left.maxVal=6, node=15, right.minVal=20
   Check: 6 < 15 < 20? ✓
   → Node(6, 20, 3)  [Valid BST of size 3]

5. Process Node(10):
   left.maxVal=5, node=10, right.minVal=6
   Check: 5 < 10 < 6? ✗ (10 > 6, invalid!)
   → Node(-inf, inf, max(1, 3))
   → Node(-inf, inf, 3)

Answer: 3 (the right subtree rooted at 15)
```

---

