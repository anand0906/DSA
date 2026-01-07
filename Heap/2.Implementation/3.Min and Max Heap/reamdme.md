# 🏗️ Complete Heap Implementation Guide

## 📋 Table of Contents

1. [Overview](#overview)
2. [Min Heap Implementation](#min-heap-implementation)
3. [Max Heap Implementation](#max-heap-implementation)
4. [Core Operations Explained](#core-operations-explained)
5. [Detailed Examples](#detailed-examples)
6. [Complexity Analysis](#complexity-analysis)
7. [Common Use Cases](#common-use-cases)

---

## 🎯 Overview

### What is a Heap?

A heap is a specialized tree-based data structure that satisfies the **heap property**. It's implemented using an array for efficient memory usage and operations.

### Two Types of Heaps

| Type | Property | Root Contains |
|------|----------|---------------|
| **Min Heap** | Parent ≤ Children | Minimum element |
| **Max Heap** | Parent ≥ Children | Maximum element |

### Key Features

```
✓ Complete Binary Tree (almost complete)
✓ Array-based implementation
✓ Efficient insertion: O(log n)
✓ Efficient extraction: O(log n)
✓ Constant-time peek: O(1)
```

---

## 🔵 Min Heap Implementation

### Class Structure

```python
class MinHeap:
    def __init__(self):
        self.arr = []      # Array to store heap elements
        self.size = 0      # Current number of elements
```

### Visual Representation

```
Min Heap Example:
       1                    Array: [1, 3, 2, 7, 5, 6, 4]
     /   \                 Index:  0  1  2  3  4  5  6
    3     2
   / \   / \               size = 7
  7   5 6   4

Property: Every parent ≤ its children
```

---

## 📚 Core Operations Explained

### 1. Initialize Heap

```python
def initializeHeap(self):
    """
    Clear the heap and reset size
    Time: O(1)
    """
    self.arr.clear()
    self.size = 0
```

**Usage:**
```python
heap = MinHeap()
heap.insert(5)
heap.insert(3)
print(heap.arr)  # [3, 5]

heap.initializeHeap()
print(heap.arr)  # []
print(heap.size) # 0
```

---

### 2. Insert Operation

```python
def insert(self, key):
    """
    Insert new element and maintain heap property
    
    Steps:
    1. Add element at end of array
    2. Heapify up to restore heap property
    3. Increment size
    
    Time: O(log n)
    """
    self.arr.append(key)
    self.heapifyUp(self.size)
    self.size += 1
```

#### Visual Example: Insert into Min Heap

**Insert 1 into heap [3, 5, 6, 7, 8]**

```
Step 1: Add to end
Array: [3, 5, 6, 7, 8, 1]
Index:  0  1  2  3  4  5

Tree:
       3
     /   \
    5     6
   / \   /
  7   8 1  ← New element added


Step 2: Heapify Up from index 5
Compare 1 with parent at index 2 (value 6)
1 < 6? YES → Swap

Array: [3, 5, 1, 7, 8, 6]
            ↑        ↑

Tree:
       3
     /   \
    5     1  ← Moved up
   / \   /
  7   8 6


Step 3: Continue heapify up from index 2
Compare 1 with parent at index 0 (value 3)
1 < 3? YES → Swap

Array: [1, 5, 3, 7, 8, 6]
        ↑     ↑

Final Tree:
       1  ← New root
     /   \
    5     3
   / \   /
  7   8 6
```

**Another Example: Insert 4**

```
Initial: [1, 5, 3, 7, 8, 6]

Step 1: Add 4
Array: [1, 5, 3, 7, 8, 6, 4]

       1
     /   \
    5     3
   / \   / \
  7   8 6   4  ← Added here


Step 2: Heapify up from index 6
Parent at index 2 (value 3)
4 > 3? NO → Stop

Final: [1, 5, 3, 7, 8, 6, 4] ✓
```

---

### 3. Extract Min Operation

```python
def extractMin(self):
    """
    Remove and return the minimum element (root)
    
    Steps:
    1. Store root value (to return)
    2. Move last element to root
    3. Remove last element
    4. Heapify down from root
    5. Decrement size
    
    Time: O(log n)
    """
    num = self.arr[0]
    self.arr[0] = self.arr[self.size - 1]
    self.arr.pop()
    self.size -= 1
    if self.size > 0:
        self.heapifyDown(0)
    return num
```

#### Visual Example: Extract Min

**Extract from [1, 5, 3, 7, 8, 6, 4]**

```
Step 1: Store root (1) to return
Root: 1

Step 2: Move last element (4) to root
Array: [4, 5, 3, 7, 8, 6]
        ↑              ↑ removed

Tree:
       4  ← Last element moved here
     /   \
    5     3
   / \   /
  7   8 6


Step 3: Heapify down from index 0
Compare 4 with children:
- Left child (index 1): 5
- Right child (index 2): 3

Smallest = 3 (right child)
Swap index 0 ↔ 2

Array: [3, 5, 4, 7, 8, 6]
        ↑     ↑

Tree:
       3  ← Moved up
     /   \
    5     4  ← Moved down
   / \   /
  7   8 6


Step 4: Continue heapify down from index 2
Compare 4 with children:
- Left child (index 5): 6
- Right child (index 6): out of bounds

4 < 6? YES → Stop

Final: [3, 5, 4, 7, 8, 6]
Return: 1
```

**Complete Sequence:**

```
Initial:     [1, 5, 3, 7, 8, 6, 4]
Extract 1 -> [3, 5, 4, 7, 8, 6]
Extract 3 -> [4, 5, 6, 7, 8]
Extract 4 -> [5, 7, 6, 8]
Extract 5 -> [6, 7, 8]
Extract 6 -> [7, 8]
Extract 7 -> [8]
Extract 8 -> []
```

---

### 4. Get Min Operation

```python
def getMin(self):
    """
    Return minimum element without removing it
    Time: O(1)
    """
    return self.arr[0]
```

**Example:**
```python
heap = MinHeap()
heap.insert(5)
heap.insert(3)
heap.insert(7)

print(heap.getMin())  # 3
print(heap.arr)       # [3, 5, 7] (unchanged)
```

---

### 5. Change Key Operation

```python
def changeKey(self, index, new_val):
    """
    Change value at index and restore heap property
    
    Decision:
    - If new_val < old_val: heapify up
    - If new_val > old_val: heapify down
    
    Time: O(log n)
    """
    if new_val < self.arr[index]:
        self.arr[index] = new_val
        self.heapifyUp(index)
    else:
        self.arr[index] = new_val
        self.heapifyDown(index)
```

#### Visual Example: Change Key

**Scenario 1: Decrease value (heapify up)**

```
Heap: [1, 5, 3, 7, 8, 6]
Change index 4 from 8 to 2

Tree Before:
       1
     /   \
    5     3
   / \   /
  7   8 6
      ↑ Change to 2

Step 1: Set new value
Array: [1, 5, 3, 7, 2, 6]

Step 2: Heapify up from index 4
Parent at index 1 (value 5)
2 < 5? YES → Swap

Array: [1, 2, 3, 7, 5, 6]
           ↑        ↑

Tree:
       1
     /   \
    2     3  ← Moved up
   / \   /
  7   5 6

Step 3: Continue from index 1
Parent at index 0 (value 1)
2 > 1? NO → Stop

Final: [1, 2, 3, 7, 5, 6] ✓
```

**Scenario 2: Increase value (heapify down)**

```
Heap: [1, 5, 3, 7, 8, 6]
Change index 0 from 1 to 10

Tree Before:
       1  ← Change to 10
     /   \
    5     3
   / \   /
  7   8 6

Step 1: Set new value
Array: [10, 5, 3, 7, 8, 6]

Step 2: Heapify down from index 0
Left: 5, Right: 3
Smallest = 3
Swap 0 ↔ 2

Array: [3, 5, 10, 7, 8, 6]
        ↑     ↑

Tree:
       3
     /   \
    5    10  ← Moved down
   / \   /
  7   8 6

Step 3: Continue from index 2
Left: 6, Right: none
6 < 10? YES → Swap

Array: [3, 5, 6, 7, 8, 10]
              ↑        ↑

Final Tree:
       3
     /   \
    5     6
   / \   /
  7   8 10  ✓
```

---

### 6. Helper Operations

```python
def isEmpty(self):
    """Check if heap is empty"""
    return self.size == 0

def heapSize(self):
    """Return number of elements"""
    return self.size
```

---

## 🔴 Max Heap Implementation

### Key Differences from Min Heap

| Operation | Min Heap | Max Heap |
|-----------|----------|----------|
| **Heap Property** | parent ≤ children | parent ≥ children |
| **Root Contains** | Minimum | Maximum |
| **Heapify Up** | Swap if child < parent | Swap if child > parent |
| **Heapify Down** | Find smallest child | Find largest child |
| **Extract** | extractMin() | extractMax() |
| **Peek** | getMin() | getMax() |

### Visual Comparison

```
Min Heap:                    Max Heap:
     1                           9
   /   \                       /   \
  3     2                     7     8
 / \   /                     / \   /
5   4 6                     3   5 6

Root: 1 (minimum)           Root: 9 (maximum)
```

### Max Heap Code

```python
class MaxHeap:
    def __init__(self):
        self.arr = []
        self.size = 0
    
    def heapifyDown(self, index):
        """Find LARGEST among parent and children"""
        largest = index
        left = 2 * index + 1
        right = 2 * index + 2
        
        if left < self.size and self.arr[left] > self.arr[largest]:
            largest = left
        
        if right < self.size and self.arr[right] > self.arr[largest]:
            largest = right
        
        if index != largest:
            self.arr[index], self.arr[largest] = self.arr[largest], self.arr[index]
            self.heapifyDown(largest)
    
    def heapifyUp(self, index):
        """Swap if child GREATER than parent"""
        parent = (index - 1) // 2
        if index > 0 and self.arr[index] > self.arr[parent]:
            self.arr[index], self.arr[parent] = self.arr[parent], self.arr[index]
            self.heapifyUp(parent)
    
    def insert(self, key):
        self.arr.append(key)
        self.heapifyUp(self.size)
        self.size += 1
    
    def extractMax(self):
        """Extract MAXIMUM element"""
        num = self.arr[0]
        self.arr[0] = self.arr[self.size - 1]
        self.arr.pop()
        self.size -= 1
        if self.size > 0:
            self.heapifyDown(0)
        return num
    
    def getMax(self):
        """Return MAXIMUM without removing"""
        return self.arr[0]
```

---

## 📝 Detailed Examples

### Example 1: Building Min Heap Step by Step

```python
heap = MinHeap()

# Insert sequence: 5, 3, 7, 1, 9, 2
```

**Insert 5:**
```
Array: [5]
Tree:  5
```

**Insert 3:**
```
Before heapify: [5, 3]
       5
      /
     3

After heapify: [3, 5]
       3
      /
     5
```

**Insert 7:**
```
Before: [3, 5, 7]
       3
      / \
     5   7

7 > 3? YES → No swap needed
Final: [3, 5, 7] ✓
```

**Insert 1:**
```
Before: [3, 5, 7, 1]
       3
      / \
     5   7
    /
   1

Heapify up:
1 < 5? YES → Swap
Array: [3, 1, 7, 5]

       3
      / \
     1   7
    /
   5

1 < 3? YES → Swap
Array: [1, 3, 7, 5]

       1
      / \
     3   7
    /
   5
```

**Insert 9:**
```
Array: [1, 3, 7, 5, 9]

       1
      / \
     3   7
    / \
   5   9

9 > 3? YES → No swap
Final: [1, 3, 7, 5, 9] ✓
```

**Insert 2:**
```
Before: [1, 3, 7, 5, 9, 2]

       1
      / \
     3   7
    / \ /
   5  9 2

Heapify up from index 5:
2 < 7? YES → Swap
Array: [1, 3, 2, 5, 9, 7]

       1
      / \
     3   2
    / \ /
   5  9 7

2 > 1? NO → Stop
Final: [1, 3, 2, 5, 9, 7] ✓
```

**Final Min Heap:**
```
Array: [1, 3, 2, 5, 9, 7]

       1
      / \
     3   2
    / \ / \
   5  9 7
```

---

### Example 2: Extract Operations

```python
heap = MinHeap()
elements = [5, 3, 7, 1, 9, 2, 8]
for e in elements:
    heap.insert(e)

# Final heap: [1, 3, 2, 5, 9, 7, 8]
```

**Extract sequence:**

```
1. Extract Min:
   Before: [1, 3, 2, 5, 9, 7, 8]
           1
         /   \
        3     2
       / \   / \
      5  9  7   8

   After: [2, 3, 7, 5, 9, 8]
          2
        /   \
       3     7
      / \   /
     5  9  8
   
   Extracted: 1


2. Extract Min:
   Before: [2, 3, 7, 5, 9, 8]
   After:  [3, 5, 7, 8, 9]
   Extracted: 2


3. Extract Min:
   Before: [3, 5, 7, 8, 9]
   After:  [5, 8, 7, 9]
   Extracted: 3


4. Extract Min:
   Before: [5, 8, 7, 9]
   After:  [7, 8, 9]
   Extracted: 5


5. Extract Min:
   Before: [7, 8, 9]
   After:  [8, 9]
   Extracted: 7


6. Extract Min:
   Before: [8, 9]
   After:  [9]
   Extracted: 8


7. Extract Min:
   Before: [9]
   After:  []
   Extracted: 9
```

**Result:** Extracted in sorted order: 1, 2, 3, 5, 7, 8, 9

---

### Example 3: Change Key Operations

```python
heap = MinHeap()
heap.arr = [2, 4, 3, 8, 6, 7, 5]
heap.size = 7
```

**Initial Heap:**
```
       2
      / \
     4   3
    / \ / \
   8  6 7  5
```

**Change index 3 from 8 to 1:**
```
Before: [2, 4, 3, 8, 6, 7, 5]
              index 3 ↑

1 < 8? YES → Heapify UP

Step 1: Swap with parent (index 1)
Array: [2, 1, 3, 4, 6, 7, 5]

       2
      / \
     1   3
    / \ / \
   4  6 7  5

Step 2: Continue up
1 < 2? YES → Swap
Array: [1, 2, 3, 4, 6, 7, 5]

       1
      / \
     2   3
    / \ / \
   4  6 7  5
```

**Change index 0 from 1 to 10:**
```
Before: [1, 2, 3, 4, 6, 7, 5]

10 > 1? YES → Heapify DOWN

Step 1: Compare with children
Left: 2, Right: 3
Smallest = 2
Swap 0 ↔ 1

Array: [2, 10, 3, 4, 6, 7, 5]

       2
      / \
    10   3
    / \ / \
   4  6 7  5

Step 2: Continue down from index 1
Left: 4, Right: 6
Smallest = 4
Swap 1 ↔ 3

Array: [2, 4, 3, 10, 6, 7, 5]

       2
      / \
     4   3
    / \ / \
  10  6 7  5

Final ✓
```

---

## ⏱️ Complexity Analysis

### Time Complexity

| Operation | Best Case | Average Case | Worst Case | Explanation |
|-----------|-----------|--------------|------------|-------------|
| **insert** | O(1) | O(log n) | O(log n) | Element may travel to root |
| **extractMin/Max** | O(log n) | O(log n) | O(log n) | Must heapify from root |
| **getMin/Max** | O(1) | O(1) | O(1) | Just return root |
| **changeKey** | O(1) | O(log n) | O(log n) | May heapify up or down |
| **isEmpty** | O(1) | O(1) | O(1) | Check size |
| **heapSize** | O(1) | O(1) | O(1) | Return size |

### Space Complexity

| Component | Space |
|-----------|-------|
| **Array storage** | O(n) |
| **Recursive calls** | O(log n) |
| **Total** | O(n) |

### Why O(log n) for Insert and Extract?

```
Height of complete binary tree with n nodes = ⌊log₂ n⌋

Worst case operations:
- Insert: Element bubbles from leaf to root (height levels)
- Extract: Element sinks from root to leaf (height levels)

Each level: O(1) comparison and swap
Total levels: O(log n)
Overall: O(log n)
```

---

## 🎯 Common Use Cases

### 1. Priority Queue

```python
# Task scheduling - process highest priority first
max_heap = MaxHeap()
max_heap.insert((5, "High priority task"))
max_heap.insert((2, "Low priority task"))
max_heap.insert((8, "Critical task"))

# Extract in priority order
while not max_heap.isEmpty():
    priority, task = max_heap.extractMax()
    print(f"Processing: {task} (priority: {priority})")

# Output:
# Processing: Critical task (priority: 8)
# Processing: High priority task (priority: 5)
# Processing: Low priority task (priority: 2)
```

### 2. Find K Largest/Smallest Elements

```python
# Find 3 smallest elements from array
def find_k_smallest(arr, k):
    min_heap = MinHeap()
    for num in arr:
        min_heap.insert(num)
    
    result = []
    for _ in range(k):
        result.append(min_heap.extractMin())
    return result

arr = [7, 10, 4, 3, 20, 15]
print(find_k_smallest(arr, 3))  # [3, 4, 7]
```

### 3. Heap Sort

```python
def heap_sort(arr):
    """Sort array using min heap"""
    heap = MinHeap()
    
    # Build heap: O(n log n)
    for num in arr:
        heap.insert(num)
    
    # Extract all: O(n log n)
    result = []
    while not heap.isEmpty():
        result.append(heap.extractMin())
    
    return result

arr = [5, 2, 8, 1, 9]
sorted_arr = heap_sort(arr)
print(sorted_arr)  # [1, 2, 5, 8, 9]
```

### 4. Merge K Sorted Lists

```python
def merge_k_sorted(lists):
    """Merge k sorted lists using min heap"""
    min_heap = MinHeap()
    
    # Add first element from each list
    for i, lst in enumerate(lists):
        if lst:
            min_heap.insert((lst[0], i, 0))  # (value, list_idx, elem_idx)
    
    result = []
    while not min_heap.isEmpty():
        val, list_idx, elem_idx = min_heap.extractMin()
        result.append(val)
        
        # Add next element from same list
        if elem_idx + 1 < len(lists[list_idx]):
            next_val = lists[list_idx][elem_idx + 1]
            min_heap.insert((next_val, list_idx, elem_idx + 1))
    
    return result
```

### 5. Running Median

```python
class MedianFinder:
    """Find median of stream using two heaps"""
    def __init__(self):
        self.max_heap = MaxHeap()  # Lower half
        self.min_heap = MinHeap()  # Upper half
    
    def add_num(self, num):
        # Add to appropriate heap
        if self.max_heap.isEmpty() or num <= self.max_heap.getMax():
            self.max_heap.insert(num)
        else:
            self.min_heap.insert(num)
        
        # Balance heaps
        if self.max_heap.heapSize() > self.min_heap.heapSize() + 1:
            self.min_heap.insert(self.max_heap.extractMax())
        elif self.min_heap.heapSize() > self.max_heap.heapSize():
            self.max_heap.insert(self.min_heap.extractMin())
    
    def find_median(self):
        if self.max_heap.heapSize() == self.min_heap.heapSize():
            return (self.max_heap.getMax() + self.min_heap.getMin()) / 2
        return self.max_heap.getMax()
```

---

## 🔍 Complete Working Example

```python
# Demonstrate all operations
def demonstrate_heap():
    print("=== Min Heap Demo ===\n")
    
    heap = MinHeap()
    
    # 1. Insert elements
    print("Inserting: 5, 3, 7, 1, 9, 2")
    for num in [5, 3, 7, 1, 9, 2]:
        heap.insert(num)
        print(f"After inserting {num}: {heap.arr}")
    
    print(f"\nFinal heap: {heap.arr}")
    print(f"Size: {heap.heapSize()}")
    
    # 2. Get minimum
    print(f"\nMinimum element: {heap.getMin()}")
    
    # 3. Change key
    print("\nChanging index 4 from 9 to 0")
    heap.changeKey(4, 0)
    print(f"After change: {heap.arr}")
    
    # 4. Extract minimum
    print("\nExtracting minimum elements:")
    while not heap.isEmpty():
        min_val = heap.extractMin()
        print(f"Extracted: {min_val}, Remaining: {heap.arr}")
    
    print("\n=== Max Heap Demo ===\n")
    
    max_heap = MaxHeap()
    
    # Insert same elements
    print("Inserting: 5, 3, 7, 1, 9, 2")
    for num in [5, 3, 7, 1, 9, 2]:
        max_heap.insert(num)
        print(f"After inserting {num}: {max_heap.arr}")
    
    print(f"\nFinal heap: {max_heap.arr}")
    print(f"Maximum element: {max_heap.getMax()}")
    
    # Extract all
    print("\nExtracting maximum elements:")
    while not max_heap.isEmpty():
        max_val = max_heap.extractMax()
        print(f"Extracted: {max_val}, Remaining: {max_heap.arr}")

# Run demonstration
demonstrate_heap()
```

**Output:**
```
=== Min Heap Demo ===

Inserting: 5, 3, 7, 1, 9, 2
After inserting 5: [5]
After inserting 3: [3, 5]
After inserting 7: [3, 5, 7]
After inserting 1: [1, 3, 7, 5]
After inserting 9: [1, 3, 7, 5, 9]
After inserting 2: [1, 3, 2, 5, 9, 7]

Final heap: [1, 3, 2, 5, 9, 7]
Size: 6

Minimum element: 1

Changing index 4 from 9 to 0
After change: [0, 1, 2, 5, 3, 7]

Extracting minimum elements:
Extracted: 0, Remaining: [1, 3, 2, 5, 7]
Extracted: 1, Remaining: [2, 3, 7, 5]
Extracted: 2, Remaining: [3, 5, 7]
Extracted: 3, Remaining: [5, 7]
Extracted: 5, Remaining: [7]
Extracted: 7, Remaining: []

=== Max Heap Demo ===

Inserting: 5, 3, 7, 1, 9, 2
After inserting 5: [5]
After inserting 3: [5, 3]
After inserting 7: [7, 3, 5]
After inserting 1: [7, 3, 5, 1]
After inserting 9: [9, 7, 5, 1, 3]
After inserting 2: [9, 7, 5, 1, 3, 2]

Final heap: [9, 7, 5, 1, 3, 2]
Maximum element: 9

Extracting maximum elements:
Extracted: 9, Remaining: [7, 3, 5, 1, 2]
Extracted: 7, Remaining: [5, 3, 2, 1]
Extracted: 5, Remaining: [3, 1, 2]
Extracted: 3, Remaining: [2, 1]
Extracted: 2, Remaining: [1]
Extracted: 1, Remaining: []
```

---

## 🎯 Key Takeaways

### Design Decisions

1. **Array-based:** Efficient memory usage, cache-friendly
2. **0-indexed:** Standard Python convention
3. **Size tracking:** Faster than len(arr) calls
4. **Separate methods:** heapifyUp and heapifyDown for clarity

### Common Patterns

```python
# Always check bounds
if left_child < self.size:
    # Safe to access

# Always update size
self.size += 1  # after insert
self.size -= 1  # after extract

# Direction choice
if new < old:
    heapify_up()    # Element decreased
else:
    heapify_down()  # Element increased
```

### Best Practices

✅ **DO:**
- Check isEmpty() before extract/get operations
- Keep size synchronized with array
- Use heapifyUp for insert, heapifyDown for extract

❌ **DON'T:**
- Access arr[0] on empty heap
- Forget to decrement size after extract
- Mix up min and max heap logic

---

## 📚 Related Topics

- **Heap Sort**: O(n log n) sorting using heap
- **Priority Queue**: Primary application of heaps
- **Dijkstra's Algorithm**: Uses min heap for shortest path
- **Huffman Coding**: Uses min heap for compression
- **K-way Merge**: Uses heap to merge multiple sorted streams

---

*Master these heap implementations and you'll have a powerful tool for solving countless algorithmic problems!* 🚀
