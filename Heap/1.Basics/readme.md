# 🌳 Heap Data Structure - Complete Guide

## 📚 Prerequisite

> **Important:** A strong understanding of **trees** is essential before learning about heaps.

---

## 🔷 Almost Complete Binary Tree

An **almost complete binary tree** is a special type of binary tree where every node can have at most two children. It must satisfy two key conditions:

### Conditions

1. **All levels except the last are completely filled**
   - Every level from root to second-last must have maximum nodes possible
   - Level 0: 1 node, Level 1: 2 nodes, Level 2: 4 nodes, etc.

2. **Last level is filled from left to right**
   - Nodes at the last level are arranged as far left as possible
   - No gaps between nodes

### Visual Example

```
        10                  ✓ Valid Almost Complete Binary Tree
       /  \
      5    15               All levels filled completely
     / \   /
    3   7 12                Last level filled left to right


        10                  ✗ Invalid - Gap in last level
       /  \
      5    15
     /      \
    3       12              Node 12 creates a gap


        10                  ✗ Invalid - Level 1 not complete
       /
      5
     / \
    3   7
```

### Why It Matters

The heap data structure is **internally maintained as an Almost Complete Binary Tree**, making this concept fundamental to understanding heaps.

---

## 🏔️ Heap

A **heap** is a special tree-based data structure that satisfies the heap property. It's used for:
- Efficiently solving priority-based problems
- Implementing sorting algorithms (Heap Sort)
- Finding smallest or largest elements quickly

---

## 📏 Heap Property

The heap property defines the relationship between parent and child nodes. There are two types:

### 1. Min-Heap

**Rule:** Parent ≤ Children

- The value of each parent node is **less than or equal to** its children
- The **smallest element** is always at the root

#### Example: Min-Heap

```
          1                  
        /   \
       3     2               1 ≤ 3, 1 ≤ 2
      / \   / \              3 ≤ 7, 3 ≤ 4
     7   4 5   6             2 ≤ 5, 2 ≤ 6

Root: 1 (minimum element)
```

### 2. Max-Heap

**Rule:** Parent ≥ Children

- The value of each parent node is **greater than or equal to** its children
- The **largest element** is always at the root

#### Example: Max-Heap

```
          90                 
        /    \
       70    80              90 ≥ 70, 90 ≥ 80
      / \    / \             70 ≥ 30, 70 ≥ 50
     30 50  60  40           80 ≥ 60, 80 ≥ 40

Root: 90 (maximum element)
```

### Key Points About Heap Property

| Aspect | Description |
|--------|-------------|
| **Local Relationship** | Property only applies between parent and immediate children |
| **Sibling Order** | No specific order required between sibling nodes |
| **Cross-Level Order** | No ordering constraint across different levels |
| **Restoration** | **Heapification** process restores property after insertions/deletions |

---

## 💾 Internal Representation of Heap

Heaps use **array representation** instead of actual tree structures for efficient memory usage and operations.

### Array to Tree Mapping

For **0-based indexing**, a node at index `i` has:

| Relationship | Formula | Example (i=1) |
|--------------|---------|---------------|
| **Left Child** | `2*i + 1` | `2*1 + 1 = 3` |
| **Right Child** | `2*i + 2` | `2*1 + 2 = 4` |
| **Parent** | `⌊i/2⌋ - 1` or `(i-1)/2` | `(1-1)/2 = 0` |

### Visual Example: Tree to Array Conversion

```
Tree Structure:                Array Representation:
                               Index: 0  1  2  3  4  5  6
        10                     Value: 10 20 30 40 50 60 70
       /  \
      20   30
     / \   / \
    40 50 60 70


Index Mapping:
├─ 10 at index 0 (root)
├─ 20 at index 1 (left child of 0)
├─ 30 at index 2 (right child of 0)
├─ 40 at index 3 (left child of 1)
├─ 50 at index 4 (right child of 1)
├─ 60 at index 5 (left child of 2)
└─ 70 at index 6 (right child of 2)
```

### Verification Example

Let's verify the parent-child relationships for node at index 2 (value 30):

```
Node: 30 (index 2)

Left Child:  2*2 + 1 = 5  → Array[5] = 60 ✓
Right Child: 2*2 + 2 = 6  → Array[6] = 70 ✓
Parent:      (2-1)/2 = 0  → Array[0] = 10 ✓
```

---

## 🍃 Indices of Leaf and Non-Leaf Nodes

For a heap with **N elements**:

### Leaf Nodes

**Definition:** Nodes without any children (appear in the last level)

**Index Range:** `⌊N/2⌋` to `N-1` (inclusive)

### Non-Leaf Nodes

**Definition:** Nodes with at least one child

**Index Range:** `0` to `⌊N/2⌋ - 1` (inclusive)

### Example with N = 7

```
Array: [10, 20, 30, 40, 50, 60, 70]
Index:   0   1   2   3   4   5   6

N = 7
⌊N/2⌋ = ⌊7/2⌋ = 3

Non-Leaf Nodes: Indices 0 to 2 → [10, 20, 30]
Leaf Nodes:     Indices 3 to 6 → [40, 50, 60, 70]


Tree Visualization:
        10  ← Non-leaf (index 0)
       /  \
      20  30  ← Non-leaf (indices 1, 2)
     / \  / \
    40 50 60 70  ← Leaf nodes (indices 3, 4, 5, 6)
```

### Example with N = 10

```
Array: [15, 10, 20, 8, 7, 9, 3, 2, 4, 6]
Index:  0   1   2  3  4  5  6  7  8  9

N = 10
⌊N/2⌋ = ⌊10/2⌋ = 5

Non-Leaf Nodes: Indices 0 to 4 → [15, 10, 20, 8, 7]
Leaf Nodes:     Indices 5 to 9 → [9, 3, 2, 4, 6]


Tree Visualization:
           15  ← Non-leaf (index 0)
         /    \
       10      20  ← Non-leaf (indices 1, 2)
      / \      / \
     8   7    9   3  ← Index 3,4 = Non-leaf, 5,6 = Leaf
    / \  /
   2  4 6  ← Leaf nodes (indices 7, 8, 9)
```

### Why This Formula Works

In an almost complete binary tree:
- First half of array contains all internal nodes
- Second half contains all leaf nodes
- The split point is always at index `⌊N/2⌋`

---

## 📊 Quick Reference Table

| Property | Formula/Range | Example (N=7) |
|----------|---------------|---------------|
| **Root Index** | `0` | `0` |
| **Left Child of i** | `2*i + 1` | Node 1: `2*1+1=3` |
| **Right Child of i** | `2*i + 2` | Node 1: `2*1+2=4` |
| **Parent of i** | `(i-1)/2` | Node 3: `(3-1)/2=1` |
| **Non-Leaf Nodes** | `0` to `⌊N/2⌋-1` | `0` to `2` |
| **Leaf Nodes** | `⌊N/2⌋` to `N-1` | `3` to `6` |

---

## 🎯 Summary

1. **Heaps are almost complete binary trees** that satisfy the heap property
2. **Min-Heap**: Smallest at root, parent ≤ children
3. **Max-Heap**: Largest at root, parent ≥ children
4. **Array representation** provides efficient storage and access
5. **Index formulas** enable quick parent-child navigation
6. **Leaf and non-leaf nodes** can be identified using `⌊N/2⌋` as the boundary

---
