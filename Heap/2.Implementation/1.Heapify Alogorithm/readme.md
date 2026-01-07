# 🔄 Heapify Algorithm - Complete Guide

## 📋 Table of Contents

1. [Problem Statement](#problem-statement)
2. [Core Concepts](#core-concepts)
3. [Heapify Up Algorithm](#heapify-up-algorithm)
4. [Heapify Down Algorithm](#heapify-down-algorithm)
5. [Complete Heapify Process](#complete-heapify-process)
6. [Detailed Examples](#detailed-examples)
7. [Code Implementation](#code-implementation)
8. [Time & Space Complexity](#time--space-complexity)

---

## 🎯 Problem Statement

**Task:** Given an array representing a heap, modify a value at a specific index and restore the heap property.

**Input:**
- `nums`: Array representing a min-heap or max-heap
- `ind`: Index where value needs to be changed
- `val`: New value to set at index `ind`

**Output:** Modified array maintaining heap property (in-place modification)

---

## 💡 Core Concepts

### Why Do We Need Heapify?

When we change a value in a heap, the **heap property may be violated**. Heapify restores this property.

### Two Directions of Heapify

| Direction | When to Use | Movement |
|-----------|-------------|----------|
| **Heapify Up** | New value is **smaller** (min-heap) or **larger** (max-heap) than original | Move element **towards root** |
| **Heapify Down** | New value is **larger** (min-heap) or **smaller** (max-heap) than original | Move element **towards leaves** |

### Decision Logic

```
For Min-Heap:
├─ If new_value < old_value → Heapify UP (element might be too small)
└─ If new_value > old_value → Heapify DOWN (element might be too large)

For Max-Heap:
├─ If new_value > old_value → Heapify UP (element might be too large)
└─ If new_value < old_value → Heapify DOWN (element might be too small)
```

---

## ⬆️ Heapify Up Algorithm

### Concept

**Move an element upward** by comparing it with its parent and swapping if needed.

### When to Use (Min-Heap)

When the new value is **smaller** than the original value, it might violate the min-heap property with its parent.

### Algorithm Steps

```
1. Start at the modified index
2. Calculate parent index: (index - 1) // 2
3. Compare element with parent
4. If element < parent (min-heap):
   - Swap element with parent
   - Recursively heapify up from parent position
5. Stop when:
   - Element ≥ parent, OR
   - Reached root (index = 0)
```

### Visual Example: Min-Heap Heapify Up

**Scenario:** Change index 5 from 6 to 2

```
Step 1: Initial Min-Heap (Valid)
Array: [1, 4, 5, 5, 7, 6]
Index:  0  1  2  3  4  5

         1
       /   \
      4     5
     / \   /
    5   7 6


Step 2: Set index 5 to 2
Array: [1, 4, 5, 5, 7, 2]  ← Heap property VIOLATED!
                        ↑
         1
       /   \
      4     5
     / \   /
    5   7 2  ← 2 < 5 (parent), needs to move up


Step 3: Heapify Up - Iteration 1
Current index: 5, value: 2
Parent index: (5-1)//2 = 2, value: 5
Check: 2 < 5? YES → SWAP

Array: [1, 4, 2, 5, 7, 5]
              ↑        ↑
         1
       /   \
      4     2  ← Moved up
     / \   /
    5   7 5


Step 4: Heapify Up - Iteration 2
Current index: 2, value: 2
Parent index: (2-1)//2 = 0, value: 1
Check: 2 < 1? NO → STOP

Final Array: [1, 4, 2, 5, 7, 5] ✓ Valid Min-Heap
```

### Another Example: Change index 4 from 7 to 3

```
Step 1: Initial State
Array: [1, 4, 5, 5, 7, 6]

Step 2: Set index 4 to 3
Array: [1, 4, 5, 5, 3, 6]

         1
       /   \
      4     5
     / \   /
    5   3 6  ← 3 < 4 (parent)


Step 3: Iteration 1
Current: index 4, value 3
Parent: index 1, value 4
Swap: 3 < 4 → YES

Array: [1, 3, 5, 5, 4, 6]

         1
       /   \
      3     5  ← Moved up
     / \   /
    5   4 6


Step 4: Iteration 2
Current: index 1, value 3
Parent: index 0, value 1
Swap: 3 < 1 → NO
STOP ✓

Final: [1, 3, 5, 5, 4, 6]
```

---

## ⬇️ Heapify Down Algorithm

### Concept

**Move an element downward** by comparing it with its children and swapping with the smaller child (min-heap).

### When to Use (Min-Heap)

When the new value is **larger** than the original value, it might violate the min-heap property with its children.

### Algorithm Steps

```
1. Start at the modified index
2. Calculate children indices:
   - Left child: 2*index + 1
   - Right child: 2*index + 2
3. Find smallest among: current, left child, right child
4. If current is not smallest:
   - Swap current with smallest child
   - Recursively heapify down from that child's position
5. Stop when:
   - Current is smallest, OR
   - Node is a leaf (no children)
```

### Visual Example: Min-Heap Heapify Down

**Scenario:** Change index 0 from 2 to 7 (Example 2 from problem)

```
Step 1: Initial Min-Heap
Array: [2, 4, 3, 6, 5, 7, 8, 7]
Index:  0  1  2  3  4  5  6  7

           2
        /     \
       4       3
      / \     / \
     6   5   7   8
    /
   7


Step 2: Set index 0 to 7
Array: [7, 4, 3, 6, 5, 7, 8, 7]  ← Heap property VIOLATED!
        ↑
           7  ← Root is now 7 (too large!)
        /     \
       4       3
      / \     / \
     6   5   7   8
    /
   7


Step 3: Heapify Down - Iteration 1
Current: index 0, value 7
Left child: index 1, value 4
Right child: index 2, value 3

Find smallest:
- 7 vs 4 → 4 is smaller
- 4 vs 3 → 3 is smallest

Swap index 0 with index 2 (smallest child)

Array: [3, 4, 7, 6, 5, 7, 8, 7]
        ↑     ↑
           3  ← Moved to root
        /     \
       4       7  ← Moved down
      / \     / \
     6   5   7   8
    /
   7


Step 4: Heapify Down - Iteration 2
Current: index 2, value 7
Left child: index 5, value 7
Right child: index 6, value 8

Find smallest:
- 7 vs 7 → equal
- 7 vs 8 → 7 is smallest

Current is smallest → STOP ✓

Final Array: [3, 4, 7, 6, 5, 7, 8, 7]
```

### Another Example: Change index 1 from 4 to 10

```
Step 1: Initial State
Array: [1, 4, 5, 5, 7, 6]

         1
       /   \
      4     5
     / \   /
    5   7 6


Step 2: Set index 1 to 10
Array: [1, 10, 5, 5, 7, 6]

         1
       /   \
      10    5  ← 10 > 5 and 10 > 7 (children)
     / \   /
    5   7 6


Step 3: Iteration 1
Current: index 1, value 10
Left: index 3, value 5
Right: index 4, value 7

Smallest = 5 (index 3)
Swap with index 3

Array: [1, 5, 5, 10, 7, 6]

         1
       /   \
      5     5
     / \   /
    10  7 6  ← Moved down


Step 4: Iteration 2
Current: index 3, value 10
No children (leaf node)
STOP ✓

Final: [1, 5, 5, 10, 7, 6]
```

---

## 🔄 Complete Heapify Process

### Decision Flow for Min-Heap

```
┌─────────────────────────────────┐
│  Modify value at index i        │
│  old_value → new_value          │
└────────────┬────────────────────┘
             │
             ▼
      ┌──────────────┐
      │ Compare      │
      │ new vs old   │
      └──────┬───────┘
             │
        ┌────┴────┐
        │         │
        ▼         ▼
   new < old   new > old
        │         │
        ▼         ▼
  ┌──────────┐ ┌──────────┐
  │ Heapify  │ │ Heapify  │
  │    UP    │ │   DOWN   │
  └──────────┘ └──────────┘
        │         │
        └────┬────┘
             ▼
    ┌─────────────────┐
    │ Heap Property   │
    │   Restored ✓    │
    └─────────────────┘
```

### Combined Algorithm Logic

```python
def heapify(arr, index, val):
    old_value = arr[index]
    
    if val <= old_value:  # For min-heap
        arr[index] = val
        heapify_up(arr, index)  # Move up if smaller
    else:
        arr[index] = val
        heapify_down(arr, index)  # Move down if larger
```

---

## 📝 Detailed Examples

### Example 1: Min-Heap with Heapify Up

**Input:** `nums = [1, 4, 5, 5, 7, 6]`, `ind = 5`, `val = 2`

```
Initial Tree:              After Setting val=2:      After Heapify:
     1                          1                         1
   /   \                      /   \                     /   \
  4     5                    4     5                   4     2
 / \   /                    / \   /                   / \   /
5   7 6                    5   7 2                   5   7 5

Step-by-step trace:

Index 5: Set to 2
Array: [1, 4, 5, 5, 7, 2]

Heapify Up:
- Current: 5, Parent: 2
- 2 < 5? YES → Swap
- Array: [1, 4, 2, 5, 7, 5]
- Current: 2, Parent: 0
- 2 < 1? NO → Stop

Result: [1, 4, 2, 5, 7, 5] ✓
```

### Example 2: Min-Heap with Heapify Down

**Input:** `nums = [2, 4, 3, 6, 5, 7, 8, 7]`, `ind = 0`, `val = 7`

```
Initial Tree:                After Setting val=7:      After Heapify:
       2                            7                         3
    /     \                      /     \                   /     \
   4       3                    4       3                 4       7
  / \     / \                  / \     / \               / \     / \
 6   5   7   8                6   5   7   8             6   5   7   8
/                            /                         /
7                           7                         7

Step-by-step trace:

Index 0: Set to 7
Array: [7, 4, 3, 6, 5, 7, 8, 7]

Heapify Down - Round 1:
- Current: 0 (val=7)
- Left: 1 (val=4), Right: 2 (val=3)
- Smallest: 3 at index 2
- Swap 0 ↔ 2
- Array: [3, 4, 7, 6, 5, 7, 8, 7]

Heapify Down - Round 2:
- Current: 2 (val=7)
- Left: 5 (val=7), Right: 6 (val=8)
- Smallest: 7 at index 2 (current)
- No swap needed → Stop

Result: [3, 4, 7, 6, 5, 7, 8, 7] ✓
```

### Example 3: Complex Heapify Up

**Input:** `nums = [1, 2, 3, 4, 5, 6, 7]`, `ind = 6`, `val = 0`

```
Initial:                  Set val=0:               After Heapify:
       1                       1                         0
     /   \                   /   \                     /   \
    2     3                 2     3                   1     3
   / \   / \               / \   / \                 / \   / \
  4   5 6   7             4   5 6   0               4   5 6   2
                                    ↑                             ↑

Trace:
[1, 2, 3, 4, 5, 6, 7] → Set index 6 to 0
[1, 2, 3, 4, 5, 6, 0]

Round 1: i=6, parent=2
- 0 < 3? YES → Swap
[1, 2, 0, 4, 5, 6, 3]

Round 2: i=2, parent=0
- 0 < 1? YES → Swap
[0, 2, 1, 4, 5, 6, 3]

Round 3: i=0, parent=-
- At root → Stop

Result: [0, 2, 1, 4, 5, 6, 3]

Final Tree:
       0
     /   \
    2     1
   / \   / \
  4   5 6   3
```

### Example 4: Complex Heapify Down

**Input:** `nums = [1, 2, 3, 4, 5]`, `ind = 0`, `val = 10`

```
Initial:              Set val=10:           After Heapify:
     1                    10                      2
   /   \                /   \                   /   \
  2     3              2     3                 4     3
 / \                  / \                     / \
4   5                4   5                  10   5

Trace:
[1, 2, 3, 4, 5] → Set index 0 to 10
[10, 2, 3, 4, 5]

Round 1: i=0
- Left: 1 (val=2), Right: 2 (val=3)
- Smallest: 2 at index 1
- Swap 0 ↔ 1
[2, 10, 3, 4, 5]

Round 2: i=1
- Left: 3 (val=4), Right: 4 (val=5)
- Smallest: 4 at index 3
- Swap 1 ↔ 3
[2, 4, 3, 10, 5]

Round 3: i=3
- No children → Stop

Result: [2, 4, 3, 10, 5]
```

---

## 💻 Code Implementation

### Min-Heap Implementation

```python
def heapify_up(n, arr, index):
    """
    Move element up the heap until min-heap property is satisfied
    
    Args:
        n: size of heap
        arr: heap array
        index: current index to heapify
    """
    parent = (index - 1) // 2
    
    # If not root and current < parent, swap and continue up
    if index > 0 and arr[index] < arr[parent]:
        arr[index], arr[parent] = arr[parent], arr[index]
        heapify_up(n, arr, parent)


def heapify_down(n, arr, index):
    """
    Move element down the heap until min-heap property is satisfied
    
    Args:
        n: size of heap
        arr: heap array
        index: current index to heapify
    """
    smallest = index
    left_child = (2 * index) + 1
    right_child = (2 * index) + 2
    
    # Find smallest among current, left child, right child
    if left_child < n and arr[left_child] < arr[smallest]:
        smallest = left_child
    
    if right_child < n and arr[right_child] < arr[smallest]:
        smallest = right_child
    
    # If smallest is not current, swap and continue down
    if index != smallest:
        arr[index], arr[smallest] = arr[smallest], arr[index]
        heapify_down(n, arr, smallest)


def heapify(arr, index, val):
    """
    Main heapify function: updates value and restores heap property
    
    Args:
        arr: heap array
        index: index to update
        val: new value
    """
    n = len(arr)
    old_val = arr[index]
    
    if val <= old_val:
        # New value is smaller, might need to move up
        arr[index] = val
        heapify_up(n, arr, index)
    else:
        # New value is larger, might need to move down
        arr[index] = val
        heapify_down(n, arr, index)
```

### Max-Heap Implementation

```python
def heapify_up(n, arr, index):
    """Move element up in max-heap"""
    parent = (index - 1) // 2
    
    # If not root and current > parent, swap and continue up
    if index > 0 and arr[index] > arr[parent]:
        arr[index], arr[parent] = arr[parent], arr[index]
        heapify_up(n, arr, parent)


def heapify_down(n, arr, index):
    """Move element down in max-heap"""
    largest = index
    left_child = (2 * index) + 1
    right_child = (2 * index) + 2
    
    # Find largest among current, left child, right child
    if left_child < n and arr[left_child] > arr[largest]:
        largest = left_child
    
    if right_child < n and arr[right_child] > arr[largest]:
        largest = right_child
    
    # If largest is not current, swap and continue down
    if index != largest:
        arr[index], arr[largest] = arr[largest], arr[index]
        heapify_down(n, arr, largest)


def heapify(arr, index, val):
    """Main heapify function for max-heap"""
    n = len(arr)
    old_val = arr[index]
    
    if val > old_val:
        # New value is larger, might need to move up
        arr[index] = val
        heapify_up(n, arr, index)
    else:
        # New value is smaller, might need to move down
        arr[index] = val
        heapify_down(n, arr, index)
```

### Iterative Implementation (Min-Heap)

```python
def heapify_up_iterative(arr, index):
    """Iterative version of heapify up"""
    while index > 0:
        parent = (index - 1) // 2
        if arr[index] < arr[parent]:
            arr[index], arr[parent] = arr[parent], arr[index]
            index = parent
        else:
            break


def heapify_down_iterative(arr, index):
    """Iterative version of heapify down"""
    n = len(arr)
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
```

---

## ⏱️ Time & Space Complexity

### Time Complexity

| Operation | Best Case | Average Case | Worst Case | Explanation |
|-----------|-----------|--------------|------------|-------------|
| **Heapify Up** | O(1) | O(log n) | O(log n) | Must traverse height of tree |
| **Heapify Down** | O(1) | O(log n) | O(log n) | Must traverse height of tree |
| **Overall Heapify** | O(1) | O(log n) | O(log n) | Height of complete binary tree |

**Why O(log n)?**
- A complete binary tree with n nodes has height ≈ log₂(n)
- In worst case, we traverse from leaf to root (or vice versa)
- Each level requires constant time operations

### Space Complexity

| Implementation | Space Complexity | Explanation |
|----------------|------------------|-------------|
| **Recursive** | O(log n) | Call stack depth = height of tree |
| **Iterative** | O(1) | Only uses a few variables |

---

## 🎯 Key Takeaways

### Decision Matrix

| Scenario | Heap Type | New vs Old | Action |
|----------|-----------|------------|--------|
| Value decreased | Min-Heap | new < old | Heapify UP |
| Value increased | Min-Heap | new > old | Heapify DOWN |
| Value increased | Max-Heap | new > old | Heapify UP |
| Value decreased | Max-Heap | new < old | Heapify DOWN |

### Important Points

1. **Heapify Up**: Compare with parent, swap if needed, move up
2. **Heapify Down**: Compare with both children, swap with smallest/largest, move down
3. **Decision**: Based on whether new value is larger or smaller than old value
4. **In-place**: No extra array needed, modify original
5. **Recursive or Iterative**: Both approaches work equally well

### Common Mistakes to Avoid

- ❌ Forgetting to check array bounds for children
- ❌ Comparing with only one child in heapify down
- ❌ Wrong direction choice (up vs down)
- ❌ Not stopping at correct termination condition
- ❌ Swapping with wrong child in heapify down

---

## 🔗 Related Operations

This heapify algorithm forms the foundation for:
- **Heap Insert**: Insert at end, heapify up
- **Heap Delete**: Remove root, move last to root, heapify down
- **Build Heap**: Heapify down from all non-leaf nodes
- **Heap Sort**: Repeatedly extract max/min using heapify

---
