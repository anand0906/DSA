# 🎯 Heap Data Structure - Quick Revision Notes

---

## 📌 1. What is a Heap?

A **heap** is a complete binary tree stored as an array that satisfies the heap property.

### Types
- **Min Heap**: Parent ≤ Children (Root = Minimum)
- **Max Heap**: Parent ≥ Children (Root = Maximum)

### Key Properties
```
✓ Complete Binary Tree (filled left to right)
✓ Array-based implementation
✓ Root at index 0
✓ Parent-child relationship via formulas
```

---

## 📐 2. Array Representation Formulas

For a node at index `i`:

| Relation | Formula | Example (i=1) |
|----------|---------|---------------|
| **Left Child** | `2*i + 1` | `2*1 + 1 = 3` |
| **Right Child** | `2*i + 2` | `2*1 + 2 = 4` |
| **Parent** | `(i-1) // 2` | `(1-1)//2 = 0` |

### Example
```
Array: [1, 3, 2, 5, 7, 6]
Index:  0  1  2  3  4  5

Tree:        1 [0]
           /   \
         3[1]   2[2]
        /  \   /
      5[3] 7[4] 6[5]
```

---

## 🔢 3. Leaf vs Non-Leaf Nodes

For array of size `n`:

| Type | Index Range | Formula |
|------|-------------|---------|
| **Non-Leaf** | `0` to `⌊n/2⌋ - 1` | `0` to `(n//2) - 1` |
| **Leaf** | `⌊n/2⌋` to `n-1` | `n//2` to `n-1` |

**Example:** n = 6
- Non-leaf: indices 0, 1, 2
- Leaf: indices 3, 4, 5

---

## 🔄 4. Heapify Operations

### Heapify Up (Bubble Up)
**Used in:** Insert operation  
**Direction:** Move element towards root  
**When:** New element might be too small (min-heap) or too large (max-heap)

```python
def heapify_up(index):
    parent = (index - 1) // 2
    if index > 0 and arr[index] < arr[parent]:  # Min-heap
        swap(arr[index], arr[parent])
        heapify_up(parent)
```

**Steps:**
1. Compare with parent
2. If violates heap property, swap
3. Recursively continue up
4. Stop at root or when property satisfied

### Heapify Down (Bubble Down)
**Used in:** Extract, Build Heap operations  
**Direction:** Move element towards leaves  
**When:** Element might be too large (min-heap) or too small (max-heap)

```python
def heapify_down(index):
    smallest = index
    left = 2 * index + 1
    right = 2 * index + 2
    
    if left < n and arr[left] < arr[smallest]:
        smallest = left
    if right < n and arr[right] < arr[smallest]:
        smallest = right
    
    if smallest != index:
        swap(arr[index], arr[smallest])
        heapify_down(smallest)
```

**Steps:**
1. Compare with both children
2. Find smallest (min-heap) or largest (max-heap)
3. Swap if needed
4. Recursively continue down
5. Stop at leaf or when property satisfied

---

## 🏗️ 5. Build Heap from Array

**Goal:** Convert arbitrary array into valid heap

### Algorithm
```python
def build_heap(arr):
    n = len(arr)
    # Start from last non-leaf node
    for i in range((n // 2) - 1, -1, -1):
        heapify_down(arr, i, n)
```

### Why This Works
1. **Start position:** `(n//2) - 1` (last non-leaf node)
2. **Direction:** Go backwards (right to left)
3. **Reason:** Process parents AFTER their children
4. **Efficiency:** O(n) time complexity

### Visual Example
```
Input: [6, 5, 2, 7, 1, 7]

Process order: index 2 → 1 → 0

Step 1 (i=2): Heapify [6, 5, 2, 7, 1, 7]
              No change

Step 2 (i=1): Heapify [6, 5, 2, 7, 1, 7]
              Swap 5 ↔ 1
              Result: [6, 1, 2, 7, 5, 7]

Step 3 (i=0): Heapify [6, 1, 2, 7, 5, 7]
              Swap 6 ↔ 1, then 6 ↔ 5
              Result: [1, 5, 2, 7, 6, 7] ✓
```

---

## 🛠️ 6. Core Heap Operations

### Insert
**Time:** O(log n)

```python
def insert(key):
    arr.append(key)          # Add at end
    heapify_up(size)         # Bubble up
    size += 1
```

**Steps:**
1. Add element at end
2. Heapify up to maintain property
3. Increment size

**Visual:**
```
Insert 1 into [3, 5, 7]

Step 1: [3, 5, 7, 1]
         3
        / \
       5   7
      /
     1

Step 2: Heapify up
        [1, 3, 7, 5]
         1
        / \
       3   7
      /
     5
```

---

### Extract Min/Max
**Time:** O(log n)

```python
def extract_min():
    min_val = arr[0]              # Store root
    arr[0] = arr[size - 1]        # Move last to root
    arr.pop()                     # Remove last
    size -= 1
    if size > 0:
        heapify_down(0)           # Restore heap
    return min_val
```

**Steps:**
1. Save root value (to return)
2. Move last element to root
3. Remove last element
4. Heapify down from root
5. Decrement size

**Visual:**
```
Extract from [1, 3, 2, 5, 7]

Step 1: Store 1
Step 2: Move 7 to root [7, 3, 2, 5]
Step 3: Heapify down
        [2, 3, 7, 5] ✓
Return: 1
```

---

### Get Min/Max
**Time:** O(1)

```python
def get_min():
    return arr[0]  # Just return root
```

---

### Change Key
**Time:** O(log n)

```python
def change_key(index, new_val):
    old_val = arr[index]
    arr[index] = new_val
    
    if new_val < old_val:      # Min-heap
        heapify_up(index)      # Value decreased
    else:
        heapify_down(index)    # Value increased
```

**Decision Logic:**
- **Decreased value:** Heapify UP (might violate with parent)
- **Increased value:** Heapify DOWN (might violate with children)

---

## 📊 7. Min Heap vs Max Heap

| Aspect | Min Heap | Max Heap |
|--------|----------|----------|
| **Root** | Minimum element | Maximum element |
| **Heap Property** | `parent ≤ children` | `parent ≥ children` |
| **Heapify Up** | Swap if `child < parent` | Swap if `child > parent` |
| **Heapify Down** | Choose smallest child | Choose largest child |
| **Extract** | `extractMin()` | `extractMax()` |
| **Peek** | `getMin()` | `getMax()` |

### Code Differences

**Min Heap:**
```python
# Heapify Up
if arr[child] < arr[parent]:
    swap()

# Heapify Down
smallest = min(parent, left_child, right_child)
```

**Max Heap:**
```python
# Heapify Up
if arr[child] > arr[parent]:
    swap()

# Heapify Down
largest = max(parent, left_child, right_child)
```

---

## ⏱️ 8. Time Complexity Summary

| Operation | Time Complexity | Explanation |
|-----------|----------------|-------------|
| **Insert** | O(log n) | Heapify up through height |
| **Extract Min/Max** | O(log n) | Heapify down through height |
| **Get Min/Max** | O(1) | Just access root |
| **Change Key** | O(log n) | Heapify up or down |
| **Build Heap** | O(n) | Special case (not n log n!) |
| **isEmpty** | O(1) | Check size |
| **heapSize** | O(1) | Return size |

**Space Complexity:**
- Array storage: O(n)
- Recursive calls: O(log n) stack space

---

## 🎓 9. Important Concepts

### Why Build Heap is O(n)?

```
Most nodes are at bottom (leaves) → do NO work
Few nodes are at top → do LOTS of work

Distribution:
- n/2 leaf nodes: 0 swaps
- n/4 nodes: max 1 swap each
- n/8 nodes: max 2 swaps each
- ...
- 1 root: max log(n) swaps

Total work: n/4 + 2n/8 + 3n/16 + ... ≈ 2n = O(n)
```

### Why Process Non-Leaf Nodes Backwards?

```
✓ Ensures subtrees are valid heaps first
✓ Parent can safely compare with children
✓ Builds heap layer by layer (bottom-up)

✗ Forward processing would violate assumptions
```

### Complete Binary Tree Property

```
Why array representation works:
1. No gaps in tree structure
2. All levels filled except possibly last
3. Last level filled left to right
4. Perfect mapping to array indices
```

---

## 💡 10. Quick Reference

### Building Min Heap
```python
# Step 1: Start from last non-leaf
start = (n // 2) - 1

# Step 2: Process backwards
for i in range(start, -1, -1):
    heapify_down(i)
```

### Min Heap Class Structure
```python
class MinHeap:
    arr = []           # Array to store elements
    size = 0           # Number of elements
    
    insert(key)        # O(log n)
    extractMin()       # O(log n)
    getMin()           # O(1)
    changeKey(i, val)  # O(log n)
    isEmpty()          # O(1)
    heapSize()         # O(1)
```

### Index Calculations (Memorize!)
```python
left_child = 2 * i + 1
right_child = 2 * i + 2
parent = (i - 1) // 2

last_non_leaf = (n // 2) - 1
first_leaf = n // 2
```

---

## 🎯 11. Common Patterns

### Pattern 1: Heap Sort
```python
# Build heap: O(n)
build_heap(arr)

# Extract all: O(n log n)
for i in range(n):
    result[i] = extract_min()

Total: O(n log n)
```

### Pattern 2: K Largest/Smallest
```python
# K smallest: Use Min Heap
heap = MinHeap()
for num in arr:
    heap.insert(num)

result = [heap.extractMin() for _ in range(k)]
```

### Pattern 3: Priority Queue
```python
# Max heap for highest priority first
heap = MaxHeap()
heap.insert((priority, task))

# Process tasks by priority
while not heap.isEmpty():
    priority, task = heap.extractMax()
    process(task)
```

---

## 📝 12. Key Formulas to Remember

```
1. Parent Index:        (i - 1) // 2
2. Left Child Index:    2 * i + 1
3. Right Child Index:   2 * i + 2
4. Last Non-Leaf:       (n // 2) - 1
5. First Leaf:          n // 2

6. Tree Height:         ⌊log₂(n)⌋
7. Max Nodes at level k: 2^k
8. Total nodes in tree: 2^(h+1) - 1
```

---

## ⚠️ 13. Common Mistakes to Avoid

```
❌ Starting build heap from index 0
✅ Start from (n//2) - 1

❌ Going forward in build heap
✅ Go backwards (right to left)

❌ Forgetting to check array bounds
✅ Always check: if left < n and right < n

❌ Not updating size after operations
✅ size++ after insert, size-- after extract

❌ Accessing arr[0] on empty heap
✅ Check isEmpty() first

❌ Using wrong comparison in heapify
✅ Min heap: <, Max heap: >
```

---

## 🔍 14. Visual Memory Aid

```
MIN HEAP STRUCTURE:

       1  ← Root (minimum)
      / \
     3   2
    / \ / \
   5  4 6  7

Array: [1, 3, 2, 5, 4, 6, 7]
Index:  0  1  2  3  4  5  6

Property Check:
✓ 1 ≤ 3, 1 ≤ 2  (root ≤ children)
✓ 3 ≤ 5, 3 ≤ 4  (level 1 ≤ children)
✓ 2 ≤ 6, 2 ≤ 7  (level 1 ≤ children)
```

---

## 🎬 15. Quick Algorithm Flow

### Insert Flow
```
Add element → Heapify UP → Done
[at end]      [to root]    [O(log n)]
```

### Extract Flow
```
Save root → Move last to root → Remove last → Heapify DOWN → Return saved
[O(1)]      [O(1)]              [O(1)]        [O(log n)]      [O(1)]
```

### Build Heap Flow
```
Find last non-leaf → Loop backwards → Heapify down each → Done
[(n//2)-1]          [to index 0]      [O(n) total]       [O(n)]
```

### Change Key Flow
```
Update value → Compare new vs old → Heapify direction → Done
[O(1)]         [O(1)]                [O(log n)]          [O(log n)]
              ↙                    ↘
         new < old              new > old
         Heapify UP            Heapify DOWN
```

---

## 📚 16. Practice Checklist

Before exam, ensure you can:

```
☐ Draw heap as both tree and array
☐ Calculate parent/child indices mentally
☐ Trace heapify up step-by-step
☐ Trace heapify down step-by-step
☐ Build heap from arbitrary array
☐ Perform insert operation
☐ Perform extract operation
☐ Identify when to use heapify up vs down
☐ Calculate time complexity of operations
☐ Explain why build heap is O(n)
☐ Convert between min and max heap
☐ Implement basic heap class
```

---

## 🚀 17. One-Page Cheat Sheet

```
HEAP = Complete Binary Tree + Heap Property

TYPES:
  Min: parent ≤ children (root = min)
  Max: parent ≥ children (root = max)

ARRAY FORMULAS:
  parent(i) = (i-1)//2
  left(i) = 2*i + 1
  right(i) = 2*i + 2

OPERATIONS:
  insert(key)        → Add end, heapify UP    → O(log n)
  extract()          → Save root, heapify DOWN → O(log n)
  get()              → Return arr[0]           → O(1)
  changeKey(i, val)  → Update, heapify UP/DOWN → O(log n)

BUILD HEAP:
  for i in range((n//2)-1, -1, -1):
      heapify_down(i)
  Time: O(n) - NOT O(n log n)!

HEAPIFY UP:
  - Used in INSERT
  - Compare with PARENT
  - Move towards ROOT

HEAPIFY DOWN:
  - Used in EXTRACT, BUILD
  - Compare with CHILDREN
  - Move towards LEAVES

REMEMBER:
  ✓ Always check array bounds
  ✓ Update size after operations
  ✓ Process backwards in build heap
  ✓ Height = log(n)
```

---

## 🎯 Final Tips

1. **Visual learner?** Always draw the tree alongside array
2. **Practice:** Build heap from [5,3,7,1,9,2,8] manually
3. **Understand WHY:** Don't just memorize, understand the logic
4. **Test edge cases:** Empty heap, single element, all same values
5. **Compare:** Know differences between min and max heap cold

---

*Review this document before exams for quick recall of all heap concepts!* 📖✨
