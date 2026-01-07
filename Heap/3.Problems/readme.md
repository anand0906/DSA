# Heap Problems - Comprehensive Guide

## Table of Contents
1. [Check if Array Represents Min Heap](#problem-1)
2. [Convert Min Heap to Max Heap](#problem-2)
3. [Heap Sort](#problem-3)
4. [K-th Largest Element in Array](#problem-4)
5. [Kth Largest Element in Stream](#problem-5)

---

<a name="problem-1"></a>
## Problem 1: Check if an Array Represents a Min Heap

### 📋 Problem Description
Given an array of integers `nums`, check whether the array represents a binary min-heap or not. Return `true` if it does, otherwise return `false`.

**Min Heap Property:** Each parent node must be smaller than or equal to its children.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [10, 20, 30, 21, 23]
Output: true

Visual representation:
       10
      /  \
    20    30
   /  \
  21  23

✓ 10 ≤ 20, 10 ≤ 30
✓ 20 ≤ 21, 20 ≤ 23
```

**Example 2:**
```
Input: nums = [10, 20, 30, 25, 15]
Output: false

Visual representation:
       10
      /  \
    20    30
   /  \
  25  15

✓ 10 ≤ 20, 10 ≤ 30
✗ 20 > 15 (VIOLATION!)
```

### 💡 Intuition

Think of the array as a complete binary tree:
- **Array indices represent tree positions**
- For any element at index `i`:
  - Left child is at index `2*i + 1`
  - Right child is at index `2*i + 2`

**Key Insight:** We only need to check parent nodes (first half of array) because leaf nodes have no children to compare with.

### 📝 Approach Steps

1. **Calculate the number of parent nodes**: `n//2` (last parent is at index `n//2 - 1`)
2. **Iterate through all parent nodes** from `n//2 - 1` down to `0`
3. **For each parent node**:
   - Check if left child exists and if parent ≤ left child
   - Check if right child exists and if parent ≤ right child
4. **Return false** if any violation found, otherwise **return true**

### 💻 Code

```python
class Solution:
    def isHeap(self, nums):
        arr = nums
        n = len(arr)
        
        # Iterate through all parent nodes (from last parent to root)
        # Range: (n//2 - 1) down to 0
        for index in range(n//2 - 1, -1, -1):
            current = arr[index]
            leftChild = 2 * index + 1
            rightChild = 2 * index + 2
            
            # Check left child: parent should be ≤ left child
            if leftChild < n and current > arr[leftChild]:
                return False
            
            # Check right child: parent should be ≤ right child
            if rightChild < n and current > arr[rightChild]:
                return False
        
        return True
```

### ⏱️ Complexity Analysis

- **Time Complexity:** O(n)
  - We iterate through n//2 parent nodes
  - Each parent check takes O(1)
  
- **Space Complexity:** O(1)
  - Only using a few variables
  - No extra data structures

---

<a name="problem-2"></a>
## Problem 2: Convert Min Heap to Max Heap

### 📋 Problem Description
Given a min-heap in array representation named `nums`, convert it into a max-heap and return the resulting array.

**Transformation:** The smallest element at root → The largest element at root

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [10, 20, 30, 21, 23]
Output: [30, 21, 23, 10, 20]

Min Heap:              Max Heap:
    10                    30
   /  \                  /  \
  20   30      →       21    23
 /  \                 /  \
21  23               10   20
```

**Example 2:**
```
Input: nums = [-5, -4, -3, -2, -1]
Output: [-1, -2, -3, -4, -5]

Min Heap:              Max Heap:
    -5                    -1
   /  \                  /  \
  -4   -3     →        -2   -3
 /  \                 /  \
-2  -1              -4   -5
```

### 💡 Intuition

**Simple Idea:** We need to "heapify" the array but in reverse order (max heap instead of min heap).

**Why build from bottom-up?**
- Start from the last parent node
- Work our way up to the root
- Each "heapify" operation ensures subtree maintains max-heap property

**Think of it like organizing boxes:**
- Start with the smallest groups (leaf level parents)
- Bubble up the largest values
- By the time we reach the root, everything is organized

### 📝 Approach Steps

1. **Identify the last parent node**: Index `n//2 - 1`
2. **Heapify from last parent to root**: Iterate backwards
3. **For each node (maxHeapifyDown)**:
   - Compare with left and right children
   - Find the largest among parent and children
   - If parent is not largest, swap with largest child
   - Recursively heapify the affected subtree

### 💻 Code

```python
def maxHeapifyDown(n, arr, index):
    """
    Ensures the subtree rooted at 'index' satisfies max-heap property
    """
    largest = index
    leftChild = 2 * index + 1
    rightChild = 2 * index + 2
    
    # Check if left child is larger than current largest
    if leftChild < n and arr[leftChild] > arr[largest]:
        largest = leftChild
    
    # Check if right child is larger than current largest
    if rightChild < n and arr[rightChild] > arr[largest]:
        largest = rightChild
    
    # If largest is not the current node, swap and continue heapifying
    if index != largest:
        arr[index], arr[largest] = arr[largest], arr[index]
        # Recursively heapify the affected subtree
        maxHeapifyDown(n, arr, largest)
    
    return

def solve(n, arr):
    """
    Convert min heap to max heap
    """
    # Start from last parent and move up to root
    # Build max heap from bottom to top
    for index in range(n//2 - 1, -1, -1):
        maxHeapifyDown(n, arr, index)
    
    return arr
```

### 🎯 Detailed Example Walkthrough

```
Array: [10, 20, 30, 21, 23]
Index:  0   1   2   3   4

Step 1: Start from index 1 (last parent)
  Compare 20 with children (21, 23)
  23 is largest → swap(20, 23)
  Result: [10, 23, 30, 21, 20]

Step 2: Process index 0 (root)
  Compare 10 with children (23, 30)
  30 is largest → swap(10, 30)
  Result: [30, 23, 10, 21, 20]
  
  Now heapify at index 2:
  Compare 10 with no children (leaf)
  No change needed
  
Final: [30, 23, 10, 21, 20]
(Note: Multiple valid max heaps exist)
```

### ⏱️ Complexity Analysis

- **Time Complexity:** O(n)
  - Building heap takes O(n)
  - Each heapify operation: O(log n)
  - Total: O(n) due to mathematical series

- **Space Complexity:** O(log n)
  - Recursion stack depth is the height of tree
  - Height = log n

---

<a name="problem-3"></a>
## Problem 3: Heap Sort

### 📋 Problem Description
Given an array of integers `nums`, sort the array in non-decreasing order using the heap sort algorithm.

**Non-decreasing order:** a₁ ≤ a₂ ≤ a₃ ≤ ... ≤ aₙ

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [7, 4, 1, 5, 3]
Output: [1, 3, 4, 5, 7]

Process:
[7, 4, 1, 5, 3] → Build Max Heap → [7, 5, 1, 4, 3]
→ Swap & Heapify → [5, 4, 1, 3, 7]
→ Swap & Heapify → [4, 3, 1, 5, 7]
→ Swap & Heapify → [3, 1, 4, 5, 7]
→ Swap & Heapify → [1, 3, 4, 5, 7] ✓
```

**Example 2:**
```
Input: nums = [5, 4, 4, 1, 1]
Output: [1, 1, 4, 4, 5]
```

### 💡 Intuition

**The Big Idea:**
1. **Max Heap Property:** The largest element is always at the root
2. **Exploitation:** Keep extracting the largest element and place it at the end
3. **Result:** Array gets sorted from right to left

**Think of it like:**
- Organizing students by height
- Always pick the tallest person (root of max heap)
- Place them at the back of the line
- Reorganize remaining people to find next tallest
- Repeat until all are sorted

### 📝 Approach Steps

1. **Build Max Heap** from the unsorted array
2. **Repeat until heap is empty:**
   - Swap root (largest element) with last element
   - Reduce heap size by 1
   - Heapify the root to restore max-heap property
3. **Result:** Sorted array in ascending order

### 💻 Code

```python
def maxHeapifyDown(n, arr, index):
    """
    Maintains max-heap property for subtree rooted at index
    """
    largest = index
    leftChild = 2 * index + 1
    rightChild = 2 * index + 2
    
    # Find largest among parent and children
    if leftChild < n and arr[leftChild] > arr[largest]:
        largest = leftChild
    
    if rightChild < n and arr[rightChild] > arr[largest]:
        largest = rightChild
    
    # Swap and continue heapifying if needed
    if index != largest:
        arr[index], arr[largest] = arr[largest], arr[index]
        maxHeapifyDown(n, arr, largest)
    
    return

def buildMaxHeap(n, arr):
    """
    Converts array into a max heap
    """
    # Start from last parent node and heapify each node
    for index in range(n//2 - 1, -1, -1):
        maxHeapifyDown(n, arr, index)
    return arr

def heapSort(n, arr):
    """
    Sorts array using heap sort algorithm
    """
    # Step 1: Build max heap
    buildMaxHeap(n, arr)
    
    # Step 2: Extract elements one by one
    last = n - 1
    while last > 0:
        # Move current root (largest) to end
        arr[0], arr[last] = arr[last], arr[0]
        last -= 1
        
        # Heapify the reduced heap
        if last > 0:
            maxHeapifyDown(last + 1, arr, 0)
```

### 🎯 Detailed Visualization

```
Array: [7, 4, 1, 5, 3]

Step 1: Build Max Heap
[7, 4, 1, 5, 3] → [7, 5, 1, 4, 3]
        7
       / \
      5   1
     / \
    4   3

Step 2: Extract & Sort
Iteration 1: Swap 7 with 3, heapify
[3, 5, 1, 4 | 7]
     ↓
[5, 4, 1, 3 | 7]

Iteration 2: Swap 5 with 3, heapify
[3, 4, 1 | 5, 7]
     ↓
[4, 3, 1 | 5, 7]

Iteration 3: Swap 4 with 1, heapify
[1, 3 | 4, 5, 7]
     ↓
[3, 1 | 4, 5, 7]

Iteration 4: Swap 3 with 1
[1 | 3, 4, 5, 7] ✓
```

### ⏱️ Complexity Analysis

- **Time Complexity:** O(n log n)
  - Building heap: O(n)
  - n extractions × O(log n) heapify = O(n log n)
  
- **Space Complexity:** O(log n)
  - Recursion stack for heapify
  - In-place sorting (no extra array)

---

<a name="problem-4"></a>
## Problem 4: K-th Largest Element in Array

### 📋 Problem Description
Given an array `nums`, return the kth largest element in the array.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [1, 2, 3, 4, 5], k = 2
Output: 4
Explanation: Sorted = [5, 4, 3, 2, 1], 2nd largest = 4
```

**Example 2:**
```
Input: nums = [-5, 4, 1, 2, -3], k = 5
Output: -5
Explanation: Sorted = [4, 2, 1, -3, -5], 5th largest = -5
```

---

## Approach 1: Sorting (Simple)

### 💡 Intuition
Sort the array and pick the kth largest element from the end.

### 📝 Steps
1. Sort the array in ascending order
2. Return element at index `n - k`

### 💻 Code

```python
def solve(n, arr, k):
    # Sort array and return (n-k)th element
    # In sorted array: smallest to largest
    # n-k gives us kth largest
    return sorted(arr)[n - k]
```

### ⏱️ Complexity
- **Time:** O(n log n) - sorting
- **Space:** O(n) - sorted creates new array

---

## Approach 2: Min Heap (Optimal for small k)

### 💡 Intuition

**Key Insight:** We only need to track the k largest elements!

**How it works:**
- Maintain a min heap of size k
- The smallest element in this heap is the kth largest overall
- As we see larger elements, we remove the smallest from heap

**Visual Example (k=3):**
```
Array: [7, 10, 4, 3, 20, 15]

Step 1: [7]
Step 2: [7, 10]
Step 3: [7, 10, 4]       Min Heap of size 3
Step 4: [7, 10, 4]       (3 < 4, skip)
Step 5: [10, 4, 7] → [20, 10, 7]  (remove 4, add 20)
Step 6: [20, 10, 7] → [20, 15, 10] (remove 7, add 15)

Answer: min of heap = 10 (3rd largest)
```

### 📝 Steps

1. Build min heap with first k elements
2. For remaining elements:
   - If element > heap minimum, remove minimum and add element
3. The minimum element in heap is kth largest

### 💻 Code

```python
import heapq

def solve(n, arr, k):
    pq = []  # Min heap
    
    # Step 1: Add first k elements to heap
    for i in range(k):
        heapq.heappush(pq, arr[i])
    
    # Step 2: Process remaining elements
    for i in range(k, n):
        # If current element is larger than smallest in heap
        if arr[i] > pq[0]:
            heapq.heappop(pq)      # Remove smallest
            heapq.heappush(pq, arr[i])  # Add current
    
    # Step 3: Top of min heap is kth largest
    return pq[0]
```

### ⏱️ Complexity
- **Time:** O(n log k)
  - k insertions: O(k log k)
  - (n-k) operations: O((n-k) log k)
  
- **Space:** O(k) - heap size

---

## Approach 3: QuickSelect (Optimal average case)

### 💡 Intuition

**Based on QuickSort's partition:**
- Partition array around a pivot
- Elements larger than pivot go left
- Elements smaller go right

**Key Insight:**
- After partition, pivot is in its final sorted position
- If pivot position = k-1, we found the answer!
- Otherwise, recursively search left or right half

**Visual Example:**
```
Array: [7, 10, 4, 3, 20, 15], k=3

Partition around pivot=10:
[20, 15, 10, 7, 4, 3]
         ↑
      index=2

k-1 = 2, so pivot is 3rd largest! Answer = 10
```

### 📝 Steps

1. **Choose random pivot** (for better average performance)
2. **Partition array:**
   - Put elements > pivot on left
   - Put elements ≤ pivot on right
3. **Check pivot position:**
   - If position = k-1: Found answer!
   - If position > k-1: Search left half
   - If position < k-1: Search right half

### 💻 Code

```python
import random

class Solution:
    def kthLargestElement(self, nums, k):
        # Handle invalid k
        if k > len(nums):
            return -1
        
        # Initialize pointers for working portion
        left, right = 0, len(nums) - 1
        
        # Keep partitioning until kth largest is found
        while True:
            # Get random pivot index
            pivotIndex = self.randomIndex(left, right)
            
            # Partition and get new pivot position
            pivotIndex = self.partitionAndReturnIndex(
                nums, pivotIndex, left, right
            )
            
            # Check if we found kth largest
            if pivotIndex == k - 1:
                return nums[pivotIndex]
            
            # Adjust search range
            elif pivotIndex > k - 1:
                right = pivotIndex - 1  # Search left
            else:
                left = pivotIndex + 1   # Search right
                
    def randomIndex(self, left, right):
        """Return random index in range [left, right]"""
        return random.randint(left, right)

    def partitionAndReturnIndex(self, nums, pivotIndex, left, right):
        """
        Partition array so that:
        - Elements > pivot are on left
        - Elements < pivot are on right
        Returns final position of pivot
        """
        pivot = nums[pivotIndex]
        
        # Move pivot to leftmost position
        nums[left], nums[pivotIndex] = nums[pivotIndex], nums[left]
        
        # Index marking start of right portion (elements < pivot)
        ind = left + 1
        
        # Partition elements
        for i in range(left + 1, right + 1):
            # If element is greater than pivot
            if nums[i] > pivot:
                # Swap to left portion
                nums[ind], nums[i] = nums[i], nums[ind]
                ind += 1
        
        # Place pivot in correct position
        nums[left], nums[ind - 1] = nums[ind - 1], nums[left]
        
        return ind - 1
```

### 🎯 Partition Example

```
Array: [7, 10, 4, 3, 20, 15], pivot=10

Initial: [10, 7, 4, 3, 20, 15]
          ↑
        pivot

After partition:
[20, 15, 10, 7, 4, 3]
         ↑
    pivot at index 2
```

### ⏱️ Complexity
- **Time:** 
  - Average: O(n)
  - Worst: O(n²) (bad pivot choices)
  
- **Space:** O(1) - in-place

---

<a name="problem-5"></a>
## Problem 5: Kth Largest Element in Stream

### 📋 Problem Description
Implement a class `KthLargest` that finds the kth largest number in a stream of integers.

**Methods:**
- `KthLargest(k, nums)`: Initialize with k and initial stream
- `add(val)`: Add value to stream and return kth largest

### 🧪 Sample Test Cases

**Example 1:**
```
Input: KthLargest(3, [1, 2, 3, 4])

Initial stream: [1, 2, 3, 4]
k = 3, so 3rd largest = 2

add(5): stream = [1, 2, 3, 4, 5] → return 3
add(2): stream = [1, 2, 2, 3, 4, 5] → return 3
add(7): stream = [1, 2, 2, 3, 4, 5, 7] → return 4
```

**Example 2:**
```
Input: KthLargest(2, [5, 5, 5, 5])

Initial stream: [5, 5, 5, 5]
k = 2, so 2nd largest = 5

add(2): stream = [2, 5, 5, 5, 5] → return 5
add(6): stream = [2, 5, 5, 5, 5, 6] → return 5
add(60): stream = [2, 5, 5, 5, 5, 6, 60] → return 6
```

### 💡 Intuition

**Same concept as Problem 4, but dynamic!**

**Key Idea:** Maintain a min heap of size k with the k largest elements
- The minimum element in this heap is always the kth largest
- When adding new elements:
  - If heap size < k: Simply add
  - If new element > heap minimum: Replace minimum with new element

**Visual Flow:**
```
k=3, Initial: [1, 2, 3, 4]

Build heap: [2, 3, 4]
            Min = 2 (3rd largest)

add(5):
  5 > 2, so remove 2, add 5
  Heap: [3, 4, 5]
  Return min = 3 ✓

add(2):
  2 < 3, don't add
  Heap: [3, 4, 5]
  Return min = 3 ✓
```

### 📝 Approach Steps

**Initialization:**
1. Create empty min heap
2. For each number in initial stream:
   - If heap size < k: Add to heap
   - Else if number > heap min: Remove min, add number

**Add operation:**
1. If heap size < k: Add value, return heap min
2. If value > heap min: Remove min, add value
3. Return heap min (kth largest)

### 💻 Code

```python
import heapq

class KthLargest:
    def __init__(self, k, nums):
        """
        Initialize with k and initial stream
        """
        n = len(nums)
        self.k = k
        self.n = n
        self.pq = []  # Min heap
        
        # Build initial heap with k largest elements
        for i in range(n):
            if len(self.pq) < k:
                # Heap not full, add element
                heapq.heappush(self.pq, nums[i])
            elif nums[i] > self.pq[0]:
                # Element is larger than current kth largest
                heapq.heappop(self.pq)        # Remove smallest
                heapq.heappush(self.pq, nums[i])  # Add current

    def add(self, val):
        """
        Add value to stream and return kth largest
        """
        # If heap not full yet, simply add
        if len(self.pq) < self.k:
            heapq.heappush(self.pq, val)
            return self.pq[0]
        
        # If new value is larger than kth largest
        if val > self.pq[0]:
            heapq.heappop(self.pq)      # Remove current kth largest
            heapq.heappush(self.pq, val)  # Add new value
        
        # Top of heap is kth largest
        return self.pq[0]
```

### 🎯 Step-by-Step Example

```
k=3, nums=[1, 2, 3, 4]

Initialization:
  Process 1: heap = [1]
  Process 2: heap = [1, 2]
  Process 3: heap = [1, 2, 3]
  Process 4: 4 > 1, remove 1, add 4
             heap = [2, 3, 4]
  kth largest = 2

add(5):
  5 > 2, remove 2, add 5
  heap = [3, 4, 5]
  return 3 ✓

add(2):
  2 < 3, don't modify heap
  heap = [3, 4, 5]
  return 3 ✓

add(7):
  7 > 3, remove 3, add 7
  heap = [4, 5, 7]
  return 4 ✓
```

### ⏱️ Complexity Analysis

**Initialization:**
- **Time:** O(n log k)
  - n elements, each insert/remove is O(log k)
- **Space:** O(k) - heap storage

**Add Operation:**
- **Time:** O(log k)
  - Heap push/pop operations
- **Space:** O(1) - no extra space

**Why this is efficient:**
- We only maintain k elements, not entire stream
- Perfect for streaming data where n can be huge!

---

## 📊 Summary Table

| Problem | Best Approach | Time | Space |
|---------|--------------|------|-------|
| Check Min Heap | Iteration | O(n) | O(1) |
| Min→Max Heap | Heapify | O(n) | O(log n) |
| Heap Sort | In-place | O(n log n) | O(log n) |
| Kth Largest | Min Heap | O(n log k) | O(k) |
| Kth in Stream | Min Heap | O(log k) per add | O(k) |

---

## 🎓 Key Takeaways

1. **Array as Tree:** Index `i` → children at `2i+1` and `2i+2`
2. **Heapify Direction:** 
   - Bottom-up: Building heap
   - Top-down: Maintaining after extraction
3. **Min Heap for Kth Largest:** Counter-intuitive but efficient!
4. **QuickSelect:** O(n) average for one-time queries
5. **Streaming Data:** Min heap keeps memory O(k) regardless of stream size
