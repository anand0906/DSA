# Binary Search Tree (BST) - Revision Notes

## Table of Contents
1. [Search in BST](#1-search-in-bst)
2. [Floor and Ceil in BST](#2-floor-and-ceil-in-bst)
3. [Insert Node in BST](#3-insert-node-in-bst)
4. [Delete Node in BST](#4-delete-node-in-bst)
5. [Kth Smallest and Largest Element](#5-kth-smallest-and-largest-element)
6. [Validate BST](#6-validate-bst)
7. [Lowest Common Ancestor in BST](#7-lowest-common-ancestor-in-bst)
8. [Construct BST from Preorder](#8-construct-bst-from-preorder)
9. [Inorder Successor and Predecessor](#9-inorder-successor-and-predecessor)
10. [BST Iterator](#10-bst-iterator)
11. [Two Sum in BST](#11-two-sum-in-bst)
12. [Recover BST (Two Nodes Swapped)](#12-recover-bst-two-nodes-swapped)
13. [Largest BST in Binary Tree](#13-largest-bst-in-binary-tree)
---

## 1. Search in BST

### Problem Description
Given root of BST and integer val, find the node with value equal to val and return the subtree rooted at that node. Return null if not found.

### Sample Test Cases

**Example 1:**
```
Input: root = [4, 2, 7, 1, 3], val = 2
Output: [2, 1, 3]

Tree Visualization:
       4
      / \
     2   7
    / \
   1   3

Searching for 2:
Step 1: Start at 4, val(2) < 4 → go left
Step 2: Reach 2, val(2) == 2 → FOUND!
Return subtree rooted at 2:
     2
    / \
   1   3
```

**Example 2:**
```
Input: root = [4, 2, 7, 1, 3], val = 5
Output: []

Tree Visualization:
       4
      / \
     2   7
    / \
   1   3

Searching for 5:
Step 1: Start at 4, val(5) > 4 → go right
Step 2: Reach 7, val(5) < 7 → go left
Step 3: Reach null → NOT FOUND!
```

### Approach: Iterative Binary Search

**Intuition:**
- Use BST property: left subtree < node < right subtree
- If val < current, go left; if val > current, go right
- If val == current, found the node

**Code:**
```python
class Solution:
    def searchBST(self, root, val):
        # Start from root
        temp = root
        
        # Continue until we find the value or reach null
        while temp and temp.data != val:
            if val < temp.data:
                # Value is smaller, search in left subtree
                temp = temp.left
            else:
                # Value is larger, search in right subtree
                temp = temp.right
        
        # Return the node (or None if not found)
        return temp
```

**Complexity:**
- **Time:** O(H) where H is height (O(log N) balanced, O(N) skewed)
- **Space:** O(1) - iterative approach uses no extra space

---

## 2. Floor and Ceil in BST

### Problem Description
Find floor (greatest value ≤ key) and ceil (smallest value ≥ key) for a given key in BST. Return -1 if not found.

### Sample Test Cases

**Example 1:**
```
Input: root = [8, 4, 12, 2, 6, 10, 14], key = 11
Output: [10, 12]

Tree Visualization:
           8
         /   \
        4     12
       / \   /  \
      2   6 10  14

Searching for floor and ceil of 11:

Floor (largest ≤ 11):
  8 → 11 > 8, potential floor = 8, go right
  12 → 11 < 12, go left
  10 → 11 > 10, floor = 10, go right
  null → Floor = 10

Ceil (smallest ≥ 11):
  8 → 11 > 8, go right
  12 → 11 < 12, ceil = 12, go left
  10 → 11 > 10, go right
  null → Ceil = 12
```

**Example 2:**
```
Input: root = [8, 4, 12, 2, 6, 10, 14], key = 15
Output: [14, -1]

Tree Visualization:
           8
         /   \
        4     12
       / \   /  \
      2   6 10  14

Floor of 15: Rightmost value = 14
Ceil of 15: No value ≥ 15 → -1
```

### Approach: Two Traversals

**Intuition:**
- **Floor**: Track last node where we went right (node.data < key)
- **Ceil**: Track last node where we went left (node.data > key)
- If key found exactly, both floor and ceil equal key

**Code:**
```python
class Solution:
    def floorCeilOfBST(self, root, key):
        # Find floor (greatest value <= key)
        floor = -1
        temp = root
        
        while temp:
            if temp.data == key:
                # Exact match found
                floor = key
                break
            elif key < temp.data:
                # Key is smaller, go left (no update to floor)
                temp = temp.left
            else:
                # Key is larger, current node could be floor
                floor = temp.data
                temp = temp.right
        
        # Find ceil (smallest value >= key)
        ceil = -1
        temp = root
        
        while temp:
            if temp.data == key:
                # Exact match found
                ceil = key
                break
            elif key < temp.data:
                # Key is smaller, current node could be ceil
                ceil = temp.data
                temp = temp.left
            else:
                # Key is larger, go right (no update to ceil)
                temp = temp.right
        
        return [floor, ceil]
```

**Complexity:**
- **Time:** O(H) - two passes through tree height
- **Space:** O(1)

---

## 3. Insert Node in BST

### Problem Description
Insert a value into BST maintaining BST properties. Return the root after insertion.

### Sample Test Cases

**Example 1:**
```
Input: root = [4, 2, 7, 1, 3], val = 5
Output: [4, 2, 7, 1, 3, 5]

Tree Visualization:
Before:              After:
       4                 4
      / \               / \
     2   7             2   7
    / \               / \   \
   1   3             1   3   5

Insertion Process for val = 5:
Step 1: 5 > 4 → go right
Step 2: 5 < 7 → go left
Step 3: null found → insert 5 as left child of 7
```

**Example 2:**
```
Input: root = [40, 20, 60, 10, 30, 50, 70], val = 25
Output: [40, 20, 60, 10, 30, 50, 70, null, null, 25]

Tree Visualization:
Before:
         40
       /    \
      20     60
     / \    /  \
    10 30  50  70

After inserting 25:
         40
       /    \
      20     60
     / \    /  \
    10 30  50  70
       /
      25

Process: 25 < 40 → left, 25 > 20 → right, 25 < 30 → insert as left child
```

### Approach: Iterative Insertion

**Intuition:**
- Traverse to find correct position (leaf level)
- Compare val with current node to decide left or right
- Insert when reaching null position

**Code:**
```python
class Solution:
    def insertIntoBST(self, root, val):
        # Start from root
        temp = root
        
        while True:
            if val < temp.data:
                # Value should go to left subtree
                if temp.left:
                    # Left child exists, continue traversing
                    temp = temp.left
                else:
                    # Found the position, insert as left child
                    temp.left = TreeNode(val)
                    break
            else:
                # Value should go to right subtree
                if temp.right:
                    # Right child exists, continue traversing
                    temp = temp.right
                else:
                    # Found the position, insert as right child
                    temp.right = TreeNode(val)
                    break
        
        return root
```

**Complexity:**
- **Time:** O(H)
- **Space:** O(1)

---

## 4. Delete Node in BST

### Problem Description
Delete a node with given key from BST. Return root after deletion.

### Sample Test Cases

**Example 1:**
```
Input: root = [5, 3, 6, 2, 4, null, 7], key = 3
Output: [5, 4, 6, 2, null, null, 7]

Tree Visualization:
Before:              After:
       5                 5
      / \               / \
     3   6             4   6
    / \   \           /     \
   2   4   7         2       7

Deleting node 3 (has both children):
Step 1: Find node 3
Step 2: Node has both left(2,4) and right(null) subtrees
Step 3: Attach right subtree to rightmost of left subtree
Step 4: Replace node 3 with its left subtree
Result: 4 becomes new child of 5
```

**Example 2:**
```
Input: root = [5, 3, 6, 2, 4, null, 7], key = 6
Output: [5, 3, 7, 2, 4]

Tree Visualization:
Before:              After:
       5                 5
      / \               / \
     3   6             3   7
    / \   \           / \
   2   4   7         2   4

Deleting node 6 (has only right child):
Simply replace 6 with its right child 7
```

### Approach: Helper Function for Replacement

**Intuition:**
- **Case 1**: Node has no left child → return right child
- **Case 2**: Node has no right child → return left child
- **Case 3**: Node has both children → attach right subtree to rightmost node of left subtree

**Code:**
```python
def findLastRight(node):
    """
    Find the rightmost node in subtree.
    This is where we'll attach the deleted node's right subtree.
    """
    if node.right is None:
        return node
    return findLastRight(node.right)

def helper(node):
    """
    Handle the actual deletion logic.
    Returns the replacement node for the deleted node.
    """
    # Case 1: No left child - replace with right child
    if node.left is None:
        return node.right
    
    # Case 2: No right child - replace with left child
    if node.right is None:
        return node.left
    
    # Case 3: Both children exist
    # Strategy: Keep left subtree, attach right subtree to rightmost of left
    right = node.right
    lastRight = findLastRight(node.left)
    lastRight.right = right
    
    # Return left subtree as replacement
    return node.left

class Solution:
    def deleteNode(self, root, key):
        # Handle empty tree or key is root
        if root is None:
            return root
        if root.data == key:
            return helper(root)
        
        # Find the node to delete
        temp = root
        while temp:
            if key < temp.data:
                # Key is in left subtree
                if temp.left and temp.left.data == key:
                    # Found the parent of node to delete
                    temp.left = helper(temp.left)
                    break
                temp = temp.left
            else:
                # Key is in right subtree
                if temp.right and temp.right.data == key:
                    # Found the parent of node to delete
                    temp.right = helper(temp.right)
                    break
                temp = temp.right
        
        return root
```

**Complexity:**
- **Time:** O(H)
- **Space:** O(1) iterative version

---

## 5. Kth Smallest and Largest Element

### Problem Description
Find kth smallest and kth largest value in BST (1-indexed).

### Sample Test Cases

**Example 1:**
```
Input: root = [3,1,4,null,2], k = 1
Output: [1, 4]

Tree Visualization:
     3
    / \
   1   4
    \
     2

Inorder: [1, 2, 3, 4]
1st smallest = 1 (index 0)
1st largest = 4 (index 3, n-k = 4-1 = 3)
```

**Example 2:**
```
Input: root = [5, 3, 6, 2, null, null, null, 1], k = 3
Output: [3, 3]

Tree Visualization:
       5
      / \
     3   6
    /
   2
  /
 1

Inorder: [1, 2, 3, 5, 6]
3rd smallest = 3 (index 2)
3rd largest = 3 (index 2, n-k = 5-3 = 2)
```

### Approach 1: Inorder Traversal (Simple)

**Intuition:**
- Inorder traversal of BST gives sorted array
- kth smallest = arr[k-1], kth largest = arr[n-k]

**Code:**
```python
def solve(root, k):
    def inorder(node, order):
        """Perform inorder traversal to get sorted values."""
        if node is None:
            return
        # Left -> Root -> Right gives sorted order
        inorder(node.left, order)
        order.append(node.data)
        inorder(node.right, order)
    
    # Get all values in sorted order
    order = []
    inorder(root, order)
    n = len(order)
    
    # kth smallest is at index k-1
    smallest = order[k-1]
    
    # kth largest is at index n-k
    # (counting from end: 1st largest = n-1, 2nd largest = n-2, ...)
    largest = order[n-k]
    
    return [smallest, largest]
```

### Approach 2: Optimized with Counter

**Intuition:**
- Stop inorder traversal once kth element found
- Use reverse inorder (right-root-left) for kth largest

**Code:**
```python
def optimized(root, k):
    # Find kth smallest using standard inorder (left-root-right)
    def inorder(node):
        nonlocal cnt, smallest
        if node is None:
            return
        
        inorder(node.left)
        
        # Process current node
        cnt += 1
        if cnt == k:
            # Found kth smallest, save and return
            smallest = node.data
            return
        
        inorder(node.right)
    
    smallest = -1
    cnt = 0
    inorder(root)
    
    # Find kth largest using reverse inorder (right-root-left)
    def inorder_reverse(node):
        nonlocal cnt, largest
        if node is None:
            return
        
        inorder_reverse(node.right)
        
        # Process current node
        cnt += 1
        if cnt == k:
            # Found kth largest, save and return
            largest = node.data
            return
        
        inorder_reverse(node.left)
    
    largest = -1
    cnt = 0
    inorder_reverse(root)
    
    return [smallest, largest]
```

**Complexity:**
- **Time:** O(N) simple, O(K) optimized
- **Space:** O(N) simple array storage, O(H) optimized recursion stack

---

## 6. Validate BST

### Problem Description
Check if a binary tree is a valid BST.

### Sample Test Cases

**Example 1:**
```
Input: root = [5, 3, 6, 2, 4, null, 7]
Output: true

Tree Visualization:
       5
      / \
     3   6
    / \   \
   2   4   7

Validation:
- All left subtree values (2,3,4) < 5 ✓
- All right subtree values (6,7) > 5 ✓
- Each subtree is also valid BST ✓
```

**Example 2:**
```
Input: root = [5, 3, 6, 4, 2, null, 7]
Output: false

Tree Visualization:
       5
      / \
     3   6
    / \   \
   4   2   7

Violation:
- Node 4 is in left subtree of 5, but 4 < 3 violates BST property ✗
- Node 2 is in left subtree of 5, but 2 < 3 violates BST property ✗
```

### Approach 1: Inorder Check

**Intuition:**
- Inorder traversal should be strictly increasing
- If arr[i] ≤ arr[i-1], not a BST

### Approach 2: Range Validation (Optimal)

**Intuition:**
- Each node must satisfy: minVal < node.data < maxVal
- Left subtree: (minVal, node.data)
- Right subtree: (node.data, maxVal)

**Code:**
```python
def solve(root):
    def isValid(node, minVal, maxVal):
        """
        Check if node and its subtrees form valid BST.
        Node value must be within (minVal, maxVal) range.
        """
        # Base case: empty tree is valid BST
        if node is None:
            return True
        
        # Check if current node violates range constraints
        if node.data <= minVal or node.data >= maxVal:
            return False
        
        # Recursively validate left subtree with updated max bound
        # All left values must be less than current node
        leftTree = isValid(node.left, minVal, node.data)
        
        # Recursively validate right subtree with updated min bound
        # All right values must be greater than current node
        rightTree = isValid(node.right, node.data, maxVal)
        
        # Both subtrees must be valid
        return leftTree and rightTree
    
    # Start with infinite range
    return isValid(root, float('-inf'), float('inf'))
```

**Complexity:**
- **Time:** O(N) - visit each node once
- **Space:** O(H) - recursion stack depth

---

## 7. Lowest Common Ancestor in BST

### Problem Description
Find LCA of two nodes p and q in BST.

### Sample Test Cases

**Example 1:**
```
Input: root = [5, 3, 6, 2, 4, null, 7], p = 2, q = 4
Output: 3

Tree Visualization:
       5
      / \
     3   6
    / \   \
   2   4   7

Finding LCA of 2 and 4:
Step 1: At node 5, both 2 and 4 < 5 → go left
Step 2: At node 3, 2 < 3 and 4 > 3 → SPLIT POINT!
Node 3 is the LCA ✓
```

**Example 2:**
```
Input: root = [5, 3, 6, 2, 4, null, 7], p = 2, q = 7
Output: 5

Tree Visualization:
       5
      / \
     3   6
    / \   \
   2   4   7

Finding LCA of 2 and 7:
Step 1: At node 5, 2 < 5 and 7 > 5 → SPLIT POINT!
Node 5 is the LCA ✓
```

### Approach: BST Property Exploitation

**Intuition:**
- If both p and q < node: LCA is in left subtree
- If both p and q > node: LCA is in right subtree
- Otherwise: current node is LCA (split point)

**Code:**
```python
def solve(root, p, q):
    def lca(node, p, q):
        """
        Find LCA using BST property.
        The split point is where one value goes left and other goes right.
        """
        if node is None:
            return None
        
        # Both values are smaller - LCA must be in left subtree
        if p < node.data and q < node.data:
            return lca(node.left, p, q)
        
        # Both values are larger - LCA must be in right subtree
        elif p > node.data and q > node.data:
            return lca(node.right, p, q)
        
        # Split point found: one value goes left, other goes right
        # OR one/both values equal current node
        else:
            return node
    
    return lca(root, p, q)
```

**Complexity:**
- **Time:** O(H) - one path from root
- **Space:** O(H) - recursion stack

---

## 8. Construct BST from Preorder

### Problem Description
Construct BST from preorder traversal array.

### Sample Test Cases

**Example 1:**
```
Input: preorder = [8, 5, 1, 7, 10, 12]
Output: [8, 5, 10, 1, 7, null, 12]

Tree Construction:
         8
       /   \
      5     10
     / \      \
    1   7     12

Preorder traversal means: Root -> Left -> Right
- 8 is root
- 5,1,7 are left subtree (all < 8)
- 10,12 are right subtree (all > 8)
```

**Example 2:**
```
Input: preorder = [1, 3]
Output: [1, null, 3]

Tree Construction:
    1
     \
      3

- 1 is root
- 3 > 1, so 3 goes to right
```

### Approach: Optimal with Upper Bound

**Intuition:**
- First element is root
- Elements < root go to left, elements > root go to right
- Use upper bound to determine when to stop left subtree

**Code:**
```python
def solve(preorder):
    n = len(preorder)
    
    def buildTree(upperBound):
        """
        Build BST with constraint that all values must be < upperBound.
        Uses global index to track current position in preorder array.
        """
        # Base case: exhausted array or current value exceeds bound
        if index[0] == n or preorder[index[0]] > upperBound:
            return None
        
        # Create node with current preorder value
        node = TreeNode(preorder[index[0]])
        index[0] += 1
        
        # Build left subtree: all values must be < current node
        node.left = buildTree(node.data)
        
        # Build right subtree: all values must be < upperBound
        # but > current node (already handled by preorder sequence)
        node.right = buildTree(upperBound)
        
        return node
    
    # Use list to make index mutable in nested function
    index = [0]
    
    # Start with infinite upper bound
    root = buildTree(float('inf'))
    return root
```

**Complexity:**
- **Time:** O(N) - each element processed once
- **Space:** O(H) - recursion stack

---

## 9. Inorder Successor and Predecessor

### Problem Description
Find inorder predecessor and successor of a given key in BST.

### Sample Test Cases

**Example 1:**
```
Input: root = [5, 2, 10, 1, 4, 7, 12], key = 10
Output: [7, 12]

Tree Visualization:
         5
       /   \
      2     10
     / \   /  \
    1   4 7   12

Inorder: [1, 2, 4, 5, 7, 10, 12]
Predecessor of 10 = 7 (value just before 10)
Successor of 10 = 12 (value just after 10)
```

**Example 2:**
```
Input: root = [5, 2, 10, 1, 4, 7, 12], key = 12
Output: [10, -1]

Tree Visualization:
         5
       /   \
      2     10
     / \   /  \
    1   4 7   12

Inorder: [1, 2, 4, 5, 7, 10, 12]
Predecessor of 12 = 10 (value just before 12)
Successor of 12 = -1 (12 is largest, no successor)
```

### Approach: Optimized Traversal

**Intuition:**
- **Predecessor**: Largest value < key
  - If key has left subtree: rightmost node of left subtree
  - Otherwise: last node where we went right
- **Successor**: Smallest value > key
  - If key has right subtree: leftmost node of right subtree
  - Otherwise: last node where we went left

**Code:**
```python
def solve(root, key):
    pred, succ = -1, -1
    temp = root
    
    while temp:
        if key < temp.data:
            # Key is smaller, current node could be successor
            succ = temp.data
            temp = temp.left
        elif key > temp.data:
            # Key is larger, current node could be predecessor
            pred = temp.data
            temp = temp.right
        else:
            # Found the key node
            
            # Find predecessor: rightmost in left subtree
            if temp.left:
                curr = temp.left
                while curr.right:
                    curr = curr.right
                pred = curr.data
            
            # Find successor: leftmost in right subtree
            if temp.right:
                curr = temp.right
                while curr.left:
                    curr = curr.left
                succ = curr.data
            
            break
    
    return [pred, succ]
```

**Complexity:**
- **Time:** O(H)
- **Space:** O(1)

---

## 10. BST Iterator

### Problem Description
Implement iterator for BST that returns elements in inorder (sorted) sequence.

### Sample Test Cases

**Example 1:**
```
Input: ["BSTIterator", "next", "next", "hasNext", "next"], 
       root = [7, 3, 15, null, null, 9, 20]
Output: [3, 7, true, 9]

Tree Visualization:
       7
      / \
     3   15
        /  \
       9   20

Inorder: [3, 7, 9, 15, 20]
Operations:
- next() → 3
- next() → 7
- hasNext() → true (more elements remain)
- next() → 9
```

### Approach: Controlled Inorder with Stack

**Intuition:**
- Use stack to simulate inorder traversal
- Push all left nodes initially
- On next(): pop node, push all left nodes of right child
- This gives O(1) average time per operation

**Code:**
```python
class BSTIterator:
    def __init__(self, root):
        """
        Initialize iterator by pushing all left nodes.
        Stack top will have the smallest (leftmost) element.
        """
        self.stack = []
        self.pushAll(root)
    
    def hasNext(self):
        """Check if more elements remain."""
        return bool(self.stack)
    
    def next(self):
        """
        Return next smallest element.
        Pop from stack, then push all left nodes of right child.
        """
        # Get next smallest element
        node = self.stack.pop()
        
        # If node has right child, push all its left nodes
        if node.right:
            self.pushAll(node.right)
        
        return node.data
    
    def pushAll(self, node):
        """
        Push node and all its left descendants onto stack.
        This prepares the leftmost (smallest) elements for retrieval.
        """
        temp = node
        while temp:
            self.stack.append(temp)
            temp = temp.left
```

**Complexity:**
- **Time:** O(1) average per operation, O(N) total for all operations
- **Space:** O(H) for stack

---

## 11. Two Sum in BST

### Problem Description
Given the root of a binary search tree and an integer k, return true if there exist two elements in the BST such that their sum is equal to k, otherwise false.

### Sample Test Cases

**Example 1:**
```
Input: root = [5, 3, 6, 2, 4, null, 7], k = 9
Output: true

Tree Visualization:
       5
      / \
     3   6
    / \   \
   2   4   7

Inorder sequence: [2, 3, 4, 5, 6, 7]

Valid pairs that sum to 9:
- 2 + 7 = 9 ✓
- 3 + 6 = 9 ✓
- 4 + 5 = 9 ✓
```

**Example 2:**
```
Input: root = [5, 3, 6, 2, 4, null, 7], k = 14
Output: false

Tree Visualization:
       5
      / \
     3   6
    / \   \
   2   4   7

Inorder sequence: [2, 3, 4, 5, 6, 7]
Maximum possible sum: 6 + 7 = 13 < 14
No pair sums to 14 ✗
```

### Approach: Two Iterators (Two-Pointer Technique)

**Intuition:**
- Use BST iterator for ascending order (left → root → right)
- Use BST iterator for descending order (right → root → left)
- Apply classic two-pointer technique on sorted sequence
- If sum < target: move left pointer forward (get larger value)
- If sum > target: move right pointer backward (get smaller value)
- Stop when pointers meet

**Visual Process:**
```
Inorder: [2, 3, 4, 5, 6, 7], target = 9

Step 1: left=2, right=7, sum=9 → FOUND! ✓

If target = 10:
Step 1: left=2, right=7, sum=9 < 10 → move left
Step 2: left=3, right=7, sum=10 → FOUND! ✓

If target = 14:
Step 1: left=2, right=7, sum=9 < 14 → move left
Step 2: left=3, right=7, sum=10 < 14 → move left
Step 3: left=4, right=7, sum=11 < 14 → move left
Step 4: left=5, right=7, sum=12 < 14 → move left
Step 5: left=6, right=7, sum=13 < 14 → move left
Step 6: left=7, right=7 (pointers meet) → NOT FOUND ✗
```

**Code:**
```python
class BSTIterator:
    def __init__(self, root, reverse=False):
        """
        Initialize BST iterator.
        reverse=False: Ascending order (normal inorder)
        reverse=True: Descending order (reverse inorder)
        """
        self.reverse = reverse
        self.stack = []
        self.pushAll(root)
    
    def hasNext(self):
        """Check if more elements available."""
        return bool(self.stack)
    
    def next(self):
        """
        Get next element in sequence.
        Pop from stack and prepare next elements.
        """
        node = self.stack.pop()
        
        # For ascending: process right subtree next
        # For descending: process left subtree next
        if self.reverse:
            self.pushAll(node.left)
        else:
            self.pushAll(node.right)
        
        return node.data
    
    def pushAll(self, node):
        """
        Push all nodes in one direction onto stack.
        Ascending: push all left nodes (smallest elements)
        Descending: push all right nodes (largest elements)
        """
        temp = node
        while temp:
            self.stack.append(temp)
            if self.reverse:
                temp = temp.right
            else:
                temp = temp.left

def solve(root, target):
    """
    Find if two sum exists using two-pointer technique.
    Uses two iterators: one ascending, one descending.
    """
    # Initialize ascending (left pointer) and descending (right pointer) iterators
    iterAsc = BSTIterator(root)
    iterDesc = BSTIterator(root, True)
    
    # Get initial values from both ends
    left, right = iterAsc.next(), iterDesc.next()
    
    # Two-pointer approach
    while left < right:
        sum_val = left + right
        
        if sum_val == target:
            return True  # Found pair
        
        if sum_val < target:
            # Need larger sum, move left pointer forward
            left = iterAsc.next()
        else:
            # Need smaller sum, move right pointer backward
            right = iterDesc.next()
    
    return False  # No pair found
```

**Complexity:**
- **Time:** O(N) - each node visited at most once by iterators
- **Space:** O(H) - two stacks, each storing at most H nodes

---

## 12. Recover BST (Two Nodes Swapped)

### Problem Description
Given the root of a BST where exactly two nodes were swapped by mistake, recover the tree without changing its structure.

### Sample Test Cases

**Example 1:**
```
Input: root = [1, 3, null, null, 2]
Output: [3, 1, null, null, 2]

Tree Visualization:
Before (Invalid):          After (Valid):
     1                          3
      \                        /
       3                      1
      /                        \
     2                          2

Analysis:
Inorder before: [1, 2, 3] ← looks sorted in values but structure wrong
Issue: 3 > 1 but is right child with left child 2 where 2 < 3
Violation: Node 1 and 3 are adjacent and swapped
Solution: Swap values of nodes 1 and 3
```

**Example 2:**
```
Input: root = [3, 1, 4, null, null, 2]
Output: [2, 1, 4, null, null, 3]

Tree Visualization:
Before (Invalid):          After (Valid):
       3                          2
      / \                        / \
     1   4                      1   4
        /                          /
       2                          3

Analysis:
Inorder before: [1, 2, 3, 4] ← values sorted but structure invalid
Issue: Value 2 in right subtree of 3 violates BST (2 < 3)
Violations detected:
  - First: 3 > 2 (prev=3, curr=2)
  - Second: 4 > 3 would be violation if we track
Node 3 and 2 are non-adjacent and swapped
Solution: Swap values of nodes 3 and 2
```

### Approach 1: Sort and Replace

**Intuition:**
- Get inorder traversal (gives sorted values for valid BST)
- Sort the inorder array to get correct sequence
- Do another inorder and replace misplaced values

### Approach 2: Find Violations (Optimal)

**Intuition:**
- In valid BST inorder: each value > previous value
- When two nodes swapped, inorder has violations: curr < prev
- **Adjacent swap**: One violation → swap first and first_next
- **Non-adjacent swap**: Two violations → swap first and last

**Visual Analysis:**
```
Case 1 - Adjacent Swap:
Valid BST inorder: [1, 2, 3, 4, 5]
After swapping 2 and 3: [1, 3, 2, 4, 5]
                              ↑ violation (3 > 2)
First violation: prev=3, curr=2
No second violation
Swap: first(3) and first_next(2)

Case 2 - Non-Adjacent Swap:
Valid BST inorder: [1, 2, 3, 4, 5]
After swapping 2 and 5: [1, 5, 3, 4, 2]
                              ↑ violation (5 > 3)
                                    ↑ violation (4 > 2)
First violation: prev=5, curr=3 → mark first=5, first_next=3
Second violation: prev=4, curr=2 → mark last=2
Swap: first(5) and last(2)
```

**Code:**
```python
def solve(root):
    """
    Recover BST by finding and swapping the two misplaced nodes.
    Uses inorder traversal to detect violations.
    """
    
    def inorder(node):
        """
        Perform inorder traversal and detect violations.
        In valid BST: prev.data < node.data always
        """
        nonlocal prev, first, first_next, last
        
        if node is None:
            return
        
        # Traverse left subtree
        inorder(node.left)
        
        # Check for violation: current value < previous value
        if prev and node.data < prev.data:
            if first is None:
                # First violation found
                first = prev  # The larger value that's wrongly placed
                first_next = node  # Potential smaller swapped value
            else:
                # Second violation found (non-adjacent swap case)
                last = node  # The actual smaller swapped value
        
        # Update previous node for next comparison
        prev = node
        
        # Traverse right subtree
        inorder(node.right)
    
    # Initialize tracking variables
    prev = None  # Previous node in inorder
    first = None  # First wrongly placed node (larger value)
    first_next = None  # Node right after first violation
    last = None  # Second wrongly placed node (if non-adjacent)
    
    # Find the violations
    inorder(root)
    
    # Swap based on violation pattern
    if first and last:
        # Two violations: non-adjacent swap
        # Example: [1, 5, 3, 4, 2] → swap 5 and 2
        first.data, last.data = last.data, first.data
    elif first and first_next:
        # One violation: adjacent swap
        # Example: [1, 3, 2, 4, 5] → swap 3 and 2
        first.data, first_next.data = first_next.data, first.data
    
    return root
```

**Complexity:**
- **Time:** O(N) - single inorder traversal
- **Space:** O(H) - recursion stack depth

---

## 13. Largest BST in Binary Tree

### Problem Description
Given a binary tree, find the size of the largest subtree which is also a BST.

### Sample Test Cases

**Example 1:**
```
Input: root = [2, 1, 3]
Output: 3

Tree Visualization:
     2
    / \
   1   3

Analysis:
- Entire tree is a valid BST
- Left subtree (1) < Root (2) < Right subtree (3) ✓
- Size = 3 nodes
```

**Example 2:**
```
Input: root = [10, null, 20, null, 30, null, 40, null, 50]
Output: 5

Tree Visualization:
    10
      \
       20
         \
          30
            \
             40
               \
                50

Analysis:
- This is a right-skewed tree
- Forms a valid BST (all values increasing)
- Size = 5 nodes
```

**Example 3 (Complex Case):**
```
Input: root = [10, 5, 15, 1, 8, null, 7]
Output: 3

Tree Visualization:
        10
       /  \
      5    15
     / \     \
    1   8     7

Analysis:
Step-by-step validation:

Full tree (root=10):
- Left subtree max (8) < root (10) ✓
- Right subtree: node 7 < 15 (invalid structure) ✗
- NOT a valid BST

Left subtree (root=5):
- Left child (1) < root (5) ✓
- Right child (8) > root (5) ✓
- Valid BST with size 3 ✓

Right subtree (root=15):
- Right child (7) < root (15) ✗
- NOT a valid BST

Largest BST size = 3
```

### Approach 1: Brute Force

**Intuition:**
- For each node, validate if subtree rooted at it is BST
- Count nodes in valid BST subtrees
- Track maximum size found
- Time: O(N²) - validate each of N nodes taking O(N) time

### Approach 2: Post-Order with Info Passing (Optimal)

**Intuition:**
- Use post-order traversal (left → right → root)
- For each node, return: {minVal, maxVal, maxSize}
- Check if current subtree is valid BST:
  - left.maxVal < node.data < right.minVal ✓
  - If valid: size = left.size + right.size + 1
  - If invalid: propagate max size from children

**Visual Process:**
```
Tree:     10
         /  \
        5    15
       / \     \
      1   8     7

Post-order traversal (bottom-up):

Step 1: Node 1 (leaf)
  Return: {min=1, max=1, size=1}

Step 2: Node 8 (leaf)
  Return: {min=8, max=8, size=1}

Step 3: Node 5 (has children)
  left = {min=1, max=1, size=1}
  right = {min=8, max=8, size=1}
  Check: 1 < 5 < 8 ✓ (valid BST)
  Return: {min=1, max=8, size=3}

Step 4: Node 7 (leaf)
  Return: {min=7, max=7, size=1}

Step 5: Node 15 (has right child)
  left = null
  right = {min=7, max=7, size=1}
  Check: -∞ < 15 < 7 ✗ (invalid!)
  Return: {min=-∞, max=∞, size=1} ← propagate max from children

Step 6: Node 10 (root)
  left = {min=1, max=8, size=3}
  right = {min=-∞, max=∞, size=1}
  Check: 8 < 10 < -∞ ✗ (invalid!)
  Return: {min=-∞, max=∞, size=3} ← propagate max(3, 1)

Answer: 3
```

**Code:**
```python
class Node:
    def __init__(self, minVal, maxVal, maxSize):
        """
        Store information about a subtree.
        minVal: Minimum value in the subtree
        maxVal: Maximum value in the subtree
        maxSize: Size of largest BST found in/under this subtree
        """
        self.minVal = minVal
        self.maxVal = maxVal
        self.maxSize = maxSize

def solve(root):
    """
    Find largest BST subtree using post-order traversal.
    Returns size of largest BST found.
    """
    
    def largestBST(node):
        """
        Post-order traversal: process children before parent.
        Returns Node object with min, max, and size information.
        """
        # Base case: null node
        if node is None:
            # Return values that won't interfere with parent validation
            # min=+∞ ensures any parent value > min
            # max=-∞ ensures any parent value < max
            # size=0 as no nodes present
            return Node(float('inf'), float('-inf'), 0)
        
        # Recursively get info from left and right children
        left = largestBST(node.left)
        right = largestBST(node.right)
        
        # Check if current subtree is a valid BST
        # Condition: left's max < current < right's min
        if left.maxVal < node.data < right.minVal:
            # Valid BST rooted at current node
            return Node(
                min(node.data, left.minVal),  # Minimum of entire subtree
                max(node.data, right.maxVal),  # Maximum of entire subtree
                left.maxSize + right.maxSize + 1  # Total size
            )
        
        # Current subtree is NOT a valid BST
        # Return sentinel values with best size found in children
        return Node(
            float('-inf'),  # Sentinel: ensures parent validation fails
            float('inf'),   # Sentinel: ensures parent validation fails
            max(left.maxSize, right.maxSize)  # Propagate largest BST size
        )
    
    # Start post-order traversal from root
    return largestBST(root).maxSize
```

**Complexity:**
- **Time:** O(N) - single post-order traversal, each node visited once
- **Space:** O(H) - recursion stack depth equals tree height

---


