# 🏗️ Build Heap from Array - Complete Guide

## 📋 Table of Contents

1. [Problem Statement](#problem-statement)
2. [Understanding the Problem](#understanding-the-problem)
3. [Naive vs Optimal Approach](#naive-vs-optimal-approach)
4. [Core Algorithm Concept](#core-algorithm-concept)
5. [Detailed Walkthrough](#detailed-walkthrough)
6. [Step-by-Step Examples](#step-by-step-examples)
7. [Code Implementation](#code-implementation)
8. [Complexity Analysis](#complexity-analysis)
9. [Why This Approach Works](#why-this-approach-works)

---

## 🎯 Problem Statement

**Task:** Convert an arbitrary array into a valid min-heap (in-place).

**Input:** Array of integers in any order

**Output:** Same array rearranged to satisfy min-heap property

**Min-Heap Property:** For every node `i`:
- `arr[i] ≤ arr[2*i + 1]` (parent ≤ left child)
- `arr[i] ≤ arr[2*i + 2]` (parent ≤ right child)

### Examples at a Glance

```
Input:  [6, 5, 2, 7, 1, 7]
Output: [1, 5, 2, 7, 6, 7]

Input:  [2, 3, 4, 1, 7, 3, 9, 4, 6]
Output: [1, 2, 3, 3, 7, 4, 9, 4, 6]
```

---

## 🤔 Understanding the Problem

### What is a Valid Min-Heap?

```
Valid Min-Heap:               Invalid Arrangement:
      1                             6
    /   \                         /   \
   5     2                       5     2
  / \   /                       / \   /
 7   6 7                       7   1 7
 
✓ Root is minimum              ✗ 1 < 6 (violated)
✓ Each parent ≤ children       ✗ 1 should be at root
```

### Key Insight

We need to transform:
```
Arbitrary Array → Valid Min-Heap Structure
```

---

## 🔄 Naive vs Optimal Approach

### ❌ Naive Approach: Insert One by One

**Strategy:** Start with empty heap, insert elements one at a time

```
Process: [6, 5, 2, 7, 1, 7]

Step 1: Insert 6
Heap: [6]

Step 2: Insert 5
Heap: [5, 6]  (5 heapified up)

Step 3: Insert 2
Heap: [2, 6, 5]  (2 heapified up)

Step 4: Insert 7
Heap: [2, 6, 5, 7]

... continue for all elements
```

**Complexity:** 
- Each insert: O(log n) for heapify up
- Total: **O(n log n)** ❌

### ✅ Optimal Approach: Bottom-Up Heapify

**Strategy:** Start from last non-leaf node, heapify down to root

```
Process: [6, 5, 2, 7, 1, 7]

Start from index (n/2 - 1) = (6/2 - 1) = 2
Work backwards: 2 → 1 → 0

Heapify only non-leaf nodes!
```

**Complexity:** **O(n)** ✅ (Much faster!)

---

## 💡 Core Algorithm Concept

### The Strategy

```
┌─────────────────────────────────────────┐
│  Why Heapify from Bottom to Top?       │
├─────────────────────────────────────────┤
│  1. Leaf nodes are already valid heaps │
│     (no children to violate property)   │
│                                          │
│  2. Process parents after children      │
│     (ensures subtrees are valid first)  │
│                                          │
│  3. Work upward toward root             │
│     (builds valid heap layer by layer)  │
└─────────────────────────────────────────┘
```

### Step-by-Step Process

```
Given array: [6, 5, 2, 7, 1, 7]
              0  1  2  3  4  5

Step 1: Identify non-leaf nodes
        Range: 0 to ⌊n/2⌋ - 1
        = 0 to 2 (indices 2, 1, 0)

Step 2: Process in reverse order
        Start: index 2
        Then:  index 1
        Last:  index 0

Step 3: For each non-leaf node
        Call heapify_down()
```

### Visual Representation

```
Initial Array: [6, 5, 2, 7, 1, 7]

Tree Structure:
       6 [0]
      /      \
    5 [1]    2 [2]
   /  \      /
  7[3] 1[4] 7[5]

Non-leaf nodes: 0, 1, 2
Leaf nodes: 3, 4, 5

Process Order: 2 → 1 → 0
```

---

## 🔍 Detailed Walkthrough

### Why Start at Index `⌊n/2⌋ - 1`?

```
For array of size n = 6:
⌊n/2⌋ - 1 = ⌊6/2⌋ - 1 = 2

Index 2 is the last non-leaf node

Proof:
- Index 3: children would be at 2*3+1=7, 2*3+2=8 (out of bounds)
- Index 2: children are at 2*2+1=5, 2*2+2=6 (5 is valid)

Therefore: indices 0,1,2 are non-leaf, 3,4,5 are leaf
```

### Why Reverse Order?

```
Process: 2 → 1 → 0 (bottom to top)

Reason: When we heapify a node, we assume 
        its subtrees are already valid heaps

Example:
       6
      / \
     5   2
    / \ /
   7  1 7

If we process 1 before 2:
- Node 1's right subtree (rooted at 2) isn't a valid heap yet
- Heapify_down won't work correctly

But if we process 2 first, then 1:
- When we heapify node 1, both its subtrees are valid
- Heapify_down can correctly find the smallest
```

---

## 📝 Step-by-Step Examples

### Example 1: Small Array

**Input:** `nums = [6, 5, 2, 7, 1, 7]`

```
Initial State:
Array: [6, 5, 2, 7, 1, 7]
Index:  0  1  2  3  4  5

Tree:
       6
      / \
     5   2
    / \ /
   7  1 7

n = 6
Non-leaf range: 0 to (6/2)-1 = 2
Process order: 2, 1, 0
```

#### Iteration 1: Heapify index 2

```
Current node: 2 (value = 2)
Left child: 2*2+1 = 5 (value = 7)
Right child: 2*2+2 = 6 (out of bounds)

Compare: 2, 7
Smallest = 2 (current)
No swap needed ✓

Array: [6, 5, 2, 7, 1, 7]  (unchanged)
Tree:
       6
      / \
     5   2
    / \ /
   7  1 7
```

#### Iteration 2: Heapify index 1

```
Current node: 1 (value = 5)
Left child: 2*1+1 = 3 (value = 7)
Right child: 2*1+2 = 4 (value = 1)

Compare: 5, 7, 1
Smallest = 1 (right child at index 4)
Swap index 1 ↔ 4

Array: [6, 1, 2, 7, 5, 7]
       [6, 5, 2, 7, 1, 7] → [6, 1, 2, 7, 5, 7]
           ↑        ↑

Tree after swap:
       6
      / \
     1   2
    / \ /
   7  5 7

Continue heapify_down from index 4:
- Node 4 (value = 5) has no children
- Stop ✓
```

#### Iteration 3: Heapify index 0

```
Current node: 0 (value = 6)
Left child: 2*0+1 = 1 (value = 1)
Right child: 2*0+2 = 2 (value = 2)

Compare: 6, 1, 2
Smallest = 1 (left child at index 1)
Swap index 0 ↔ 1

Array: [1, 6, 2, 7, 5, 7]
       [6, 1, 2, 7, 5, 7] → [1, 6, 2, 7, 5, 7]
        ↑  ↑

Tree after swap:
       1
      / \
     6   2
    / \ /
   7  5 7

Continue heapify_down from index 1:
Current node: 1 (value = 6)
Left child: 3 (value = 7)
Right child: 4 (value = 5)

Compare: 6, 7, 5
Smallest = 5 (right child at index 4)
Swap index 1 ↔ 4

Array: [1, 5, 2, 7, 6, 7]
           ↑        ↑

Tree after swap:
       1
      / \
     5   2
    / \ /
   7  6 7

Continue heapify_down from index 4:
- Node 4 (value = 6) has no children
- Stop ✓
```

**Final Result:** `[1, 5, 2, 7, 6, 7]` ✓

```
Final Min-Heap:
       1
      / \
     5   2
    / \ /
   7  6 7

Verification:
✓ 1 ≤ 5, 1 ≤ 2
✓ 5 ≤ 7, 5 ≤ 6
✓ 2 ≤ 7
```

---

### Example 2: Larger Array

**Input:** `nums = [2, 3, 4, 1, 7, 3, 9, 4, 6]`

```
Initial State:
Array: [2, 3, 4, 1, 7, 3, 9, 4, 6]
Index:  0  1  2  3  4  5  6  7  8

Tree:
         2
       /   \
      3     4
     / \   / \
    1   7 3   9
   / \
  4   6

n = 9
Non-leaf range: 0 to (9/2)-1 = 3
Process order: 3, 2, 1, 0
```

#### Iteration 1: Heapify index 3

```
Current: index 3 (value = 1)
Left: 2*3+1 = 7 (value = 4)
Right: 2*3+2 = 8 (value = 6)

Compare: 1, 4, 6
Smallest = 1 (current)
No swap ✓

Array: [2, 3, 4, 1, 7, 3, 9, 4, 6]
```

#### Iteration 2: Heapify index 2

```
Current: index 2 (value = 4)
Left: 2*2+1 = 5 (value = 3)
Right: 2*2+2 = 6 (value = 9)

Compare: 4, 3, 9
Smallest = 3 (left child at index 5)
Swap index 2 ↔ 5

Array: [2, 3, 3, 1, 7, 4, 9, 4, 6]
           ↑        ↑

Tree:
         2
       /   \
      3     3
     / \   / \
    1   7 4   9
   / \
  4   6

Continue from index 5:
- No children, stop ✓
```

#### Iteration 3: Heapify index 1

```
Current: index 1 (value = 3)
Left: 2*1+1 = 3 (value = 1)
Right: 2*1+2 = 4 (value = 7)

Compare: 3, 1, 7
Smallest = 1 (left child at index 3)
Swap index 1 ↔ 3

Array: [2, 1, 3, 3, 7, 4, 9, 4, 6]
           ↑     ↑

Tree:
         2
       /   \
      1     3
     / \   / \
    3   7 4   9
   / \
  4   6

Continue from index 3:
Current: index 3 (value = 3)
Left: 7 (value = 4)
Right: 8 (value = 6)

Compare: 3, 4, 6
Smallest = 3 (current)
Stop ✓
```

#### Iteration 4: Heapify index 0

```
Current: index 0 (value = 2)
Left: 2*0+1 = 1 (value = 1)
Right: 2*0+2 = 2 (value = 3)

Compare: 2, 1, 3
Smallest = 1 (left child at index 1)
Swap index 0 ↔ 1

Array: [1, 2, 3, 3, 7, 4, 9, 4, 6]
        ↑  ↑

Tree:
         1
       /   \
      2     3
     / \   / \
    3   7 4   9
   / \
  4   6

Continue from index 1:
Current: index 1 (value = 2)
Left: 3 (value = 3)
Right: 4 (value = 7)

Compare: 2, 3, 7
Smallest = 2 (current)
Stop ✓
```

**Final Result:** `[1, 2, 3, 3, 7, 4, 9, 4, 6]` ✓

```
Final Min-Heap:
         1
       /   \
      2     3
     / \   / \
    3   7 4   9
   / \
  4   6

Verification:
✓ 1 ≤ 2, 1 ≤ 3
✓ 2 ≤ 3, 2 ≤ 7
✓ 3 ≤ 4, 3 ≤ 9
✓ 3 ≤ 4, 3 ≤ 6
```

---

### Example 3: Already Sorted Array

**Input:** `nums = [1, 2, 3, 4, 5, 6]`

```
Initial State:
Array: [1, 2, 3, 4, 5, 6]

Tree:
       1
      / \
     2   3
    / \ /
   4  5 6

This is already a valid min-heap!

Process: 2 → 1 → 0
```

#### Iteration 1: Heapify index 2

```
Current: 2 (value = 3)
Left: 5 (value = 6)
Right: 6 (out of bounds)

Compare: 3, 6
Smallest = 3
No swap ✓
```

#### Iteration 2: Heapify index 1

```
Current: 1 (value = 2)
Left: 3 (value = 4)
Right: 4 (value = 5)

Compare: 2, 4, 5
Smallest = 2
No swap ✓
```

#### Iteration 3: Heapify index 0

```
Current: 0 (value = 1)
Left: 1 (value = 2)
Right: 2 (value = 3)

Compare: 1, 2, 3
Smallest = 1
No swap ✓
```

**Final Result:** `[1, 2, 3, 4, 5, 6]` (unchanged) ✓

---

### Example 4: Reverse Sorted Array

**Input:** `nums = [9, 8, 7, 6, 5, 4]`

```
Initial State:
Array: [9, 8, 7, 6, 5, 4]

Tree:
       9
      / \
     8   7
    / \ /
   6  5 4

This is the worst case!
```

#### Iteration 1: Heapify index 2

```
Current: 2 (value = 7)
Left: 5 (value = 4)
Right: 6 (out of bounds)

Compare: 7, 4
Smallest = 4
Swap 2 ↔ 5

Array: [9, 8, 4, 6, 5, 7]
```

#### Iteration 2: Heapify index 1

```
Current: 1 (value = 8)
Left: 3 (value = 6)
Right: 4 (value = 5)

Compare: 8, 6, 5
Smallest = 5
Swap 1 ↔ 4

Array: [9, 5, 4, 6, 8, 7]

Continue from 4:
No children, stop ✓
```

#### Iteration 3: Heapify index 0

```
Current: 0 (value = 9)
Left: 1 (value = 5)
Right: 2 (value = 4)

Compare: 9, 5, 4
Smallest = 4
Swap 0 ↔ 2

Array: [4, 5, 9, 6, 8, 7]

Continue from 2:
Current: 2 (value = 9)
Left: 5 (value = 7)
Right: 6 (out of bounds)

Compare: 9, 7
Smallest = 7
Swap 2 ↔ 5

Array: [4, 5, 7, 6, 8, 9]

Continue from 5:
No children, stop ✓
```

**Final Result:** `[4, 5, 7, 6, 8, 9]` ✓

```
Final Tree:
       4
      / \
     5   7
    / \ /
   6  8 9
```

---

## 💻 Code Implementation

### Python Implementation

```python
def heapify_down(n, arr, index):
    """
    Heapify down from given index to maintain min-heap property
    
    Args:
        n: size of heap
        arr: array representing heap
        index: current index to heapify
    """
    smallest = index
    left_child = 2 * index + 1
    right_child = 2 * index + 2
    
    # Check if left child exists and is smaller
    if left_child < n and arr[left_child] < arr[smallest]:
        smallest = left_child
    
    # Check if right child exists and is smaller
    if right_child < n and arr[right_child] < arr[smallest]:
        smallest = right_child
    
    # If smallest is not current node, swap and continue
    if smallest != index:
        arr[smallest], arr[index] = arr[index], arr[smallest]
        heapify_down(n, arr, smallest)


class Solution:
    def buildMinHeap(self, nums):
        """
        Build min-heap from array in-place
        
        Args:
            nums: input array
        
        Time: O(n)
        Space: O(log n) for recursion stack
        """
        n = len(nums)
        
        # Start from last non-leaf node and heapify down
        # Last non-leaf node is at index (n//2 - 1)
        for index in range((n // 2) - 1, -1, -1):
            heapify_down(n, nums, index)


# Example usage
solution = Solution()

# Example 1
nums1 = [6, 5, 2, 7, 1, 7]
solution.buildMinHeap(nums1)
print(f"Example 1: {nums1}")  # [1, 5, 2, 7, 6, 7]

# Example 2
nums2 = [2, 3, 4, 1, 7, 3, 9, 4, 6]
solution.buildMinHeap(nums2)
print(f"Example 2: {nums2}")  # [1, 2, 3, 3, 7, 4, 9, 4, 6]
```

### Iterative Heapify Down

```python
def heapify_down_iterative(n, arr, index):
    """Iterative version of heapify down"""
    while True:
        smallest = index
        left = 2 * index + 1
        right = 2 * index + 2
        
        if left < n and arr[left] < arr[smallest]:
            smallest = left
        
        if right < n and arr[right] < arr[smallest]:
            smallest = right
        
        if smallest != index:
            arr[index], arr[smallest] = arr[smallest], arr[index]
            index = smallest
        else:
            break


def build_min_heap_iterative(nums):
    """Build heap using iterative heapify"""
    n = len(nums)
    for index in range((n // 2) - 1, -1, -1):
        heapify_down_iterative(n, nums, index)
```

### Max-Heap Version

```python
def heapify_down_max(n, arr, index):
    """Heapify down for max-heap"""
    largest = index
    left_child = 2 * index + 1
    right_child = 2 * index + 2
    
    if left_child < n and arr[left_child] > arr[largest]:
        largest = left_child
    
    if right_child < n and arr[right_child] > arr[largest]:
        largest = right_child
    
    if largest != index:
        arr[largest], arr[index] = arr[index], arr[largest]
        heapify_down_max(n, arr, largest)


def build_max_heap(nums):
    """Build max-heap from array"""
    n = len(nums)
    for index in range((n // 2) - 1, -1, -1):
        heapify_down_max(n, nums, index)
```

### With Detailed Comments

```python
def buildMinHeap(nums):
    """
    Build min-heap using bottom-up approach
    
    Algorithm:
    1. Identify non-leaf nodes: indices 0 to (n//2 - 1)
    2. Process nodes in reverse order (bottom-up)
    3. For each node, call heapify_down
    
    Why reverse order?
    - Ensures subtrees are valid heaps before processing parent
    - Builds heap layer by layer from bottom to top
    """
    n = len(nums)
    
    # Calculate starting position (last non-leaf node)
    start = (n // 2) - 1
    
    # Process each non-leaf node from bottom to top
    for i in range(start, -1, -1):
        heapify_down(n, nums, i)
        
        # Optional: print state after each heapify
        print(f"After heapifying index {i}: {nums}")
```

---

## ⏱️ Complexity Analysis

### Time Complexity: O(n)

**Detailed Analysis:**

```
Height of tree = h = log(n)

Nodes at height h (leaves): n/2 nodes, 0 swaps needed
Nodes at height h-1: n/4 nodes, max 1 swap each
Nodes at height h-2: n/8 nodes, max 2 swaps each
...
Nodes at height 0 (root): 1 node, max h swaps

Total work:
T(n) = (n/2) * 0 + (n/4) * 1 + (n/8) * 2 + ... + 1 * log(n)

This sum converges to O(n)
```

**Intuitive Explanation:**

```
Most nodes (leaves) do no work: n/2 nodes
Few nodes do a lot of work: only 1 node (root) does log(n) work

Distribution:
┌─────────────────────────────────┐
│ Height │ Nodes  │ Work/Node    │
├─────────────────────────────────┤
│   0    │   1    │   log(n)     │
│   1    │   2    │   log(n)-1   │
│   2    │   4    │   log(n)-2   │
│  ...   │  ...   │     ...      │
│  log n │  n/2   │      0       │
└─────────────────────────────────┘

Total: O(n)  ← Tighter bound than O(n log n)!
```

### Space Complexity

| Version | Space | Reason |
|---------|-------|--------|
| **Recursive** | O(log n) | Recursion call stack depth |
| **Iterative** | O(1) | Only uses local variables |

### Comparison with Naive Approach

| Approach | Time | Space | Method |
|----------|------|-------|--------|
| **Insert one-by-one** | O(n log n) | O(1) | Heapify up for each insert |
| **Bottom-up build** | O(n) | O(log n) | Heapify down from non-leaves |

**Winner:** Bottom-up build is **asymptotically faster**! 🏆

---

## 🎓 Why This Approach Works

### Mathematical Proof Sketch

```
Claim: Building heap from bottom-up is O(n)

Proof:
1. Tree has height h = ⌊log n⌋

2. At height i, there are at most ⌈n/2^(i+1)⌉ nodes

3. Each node at height i can move down at most i levels

4. Total work:
   Σ (nodes at height i) × i
   = Σ ⌈n/2^(i+1)⌉ × i  (for i = 0 to h)
   
5. This sum = n × Σ i/2^(i+1)  (for i = 0 to ∞)
   
6. The series Σ i/2^(i+1) converges to 2

7. Therefore: Total work = n × 2 = O(n)
```

### Visual Intuition

```
Level-wise work distribution:

Level 3 (leaves): ████████ (50% nodes, 0 work each)
Level 2:          ████ (25% nodes, small work)
Level 1:          ██ (12.5% nodes, medium work)
Level 0 (root):   █ (1 node, most work)

Most nodes do little/no work!
Few nodes do lots of work.
Averages out to constant work per node.
```

### Why Not Heapify Up?

```
Heapify Up approach:
- Insert elements one by one
- Each insertion: O(log n)
- Total: n × O(log n) = O(n log n)

Heapify Down approach:
- Process existing array
- Leverages that leaves are already valid
- Only process non-leaf nodes
- Total: O(n)

Difference: We avoid redundant work!
```

---

## 🎯 Key Takeaways

### Critical Points

1. **Start position:** Last non-leaf node at index `⌊n/2⌋ - 1`
2. **Direction:** Process backwards (right to left, bottom to top)
3. **Operation:** Call `heapify_down` for each non-leaf node
4. **Efficiency:** O(n) time, much better than O(n log n) insertion

### Common Mistakes to Avoid

❌ **Starting from index 0**
```python
# Wrong!
for i in range(n):
    heapify_down(n, arr, i)
```

✅ **Starting from last non-leaf**
```python
# Correct!
for i in range((n//2) - 1, -1, -1):
    heapify_down(n, arr, i)
```

❌ **Going forward**
```python
# Wrong!
for i in range(n//2):
    heapify_down(n, arr, i)
```

✅ **Going backward**
```python
# Correct!
for i in range((n//2) - 1, -1, -1):
    heapify_down(n, arr, i)
```

### Quick Reference

```python
# Formula to remember
n = len(arr)
start = (n // 2) - 1  # Last non-leaf node
end = -1              # Before first element
step = -1             # Go backwards

for i in range(start, end, step):
    heapify_down(n, arr, i)
```

---

## 🔗 Applications

Building a heap is the foundation for:

1. **Heap Sort**: Build heap, then repeatedly extract min/max
2. **Priority Queue**: Initialize queue with existing elements
3. **K-th Largest/Smallest**: Build heap, extract k times
4. **Median Finding**: Use two heaps (min and max)
5. **Merge K Sorted Arrays**: Build heap of array heads

---

## 📊 Visual Summary

```
Build Heap Algorithm Summary

Input: Arbitrary Array
  ↓
Identify Non-Leaf Nodes [0 to ⌊n/2⌋-1]
  ↓
Process in Reverse Order (bottom-up)
  ↓
For Each Node: Heapify Down
  ↓
Output: Valid Min-Heap

Time: O(n) ⚡
Space: O(log n) recursive, O(1) iterative
```

---

*Master this algorithm and you've conquered one of the most elegant examples of amortized analysis in computer science!* 🚀
