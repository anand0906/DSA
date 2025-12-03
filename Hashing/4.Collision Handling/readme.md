# Hash Collisions & Resolution Techniques

> Complete guide to understanding and handling hash collisions

---

## 📋 Table of Contents

- [What is a Hash Collision?](#what-is-a-hash-collision)
- [Collision Resolution Techniques](#collision-resolution-techniques)
- [Open Chaining (Separate Chaining)](#open-chaining-separate-chaining)
- [Open Addressing](#open-addressing)
- [Comparison & Best Practices](#comparison--best-practices)

---

## ⚡ What is a Hash Collision?

### Definition

A **hash collision** occurs when two different keys produce the same hash value and map to the same index in the hash table.

### Why Collisions Happen

```
Hash Function: Maps infinite possible keys → Finite hash table slots

Pigeonhole Principle: If you have more items than slots,
                      at least two items must share a slot!
```

### Simple Example

**Hash Function:** `h(key) = len(key) % 10`

```
Input: "cat"
Length: 3
Hash: 3 % 10 = 3  ← Index 3

Input: "dog"  
Length: 3
Hash: 3 % 10 = 3  ← Index 3 (COLLISION!)
```

**Visual:**
```
Hash Table:
Index:  0    1    2   [3]   4    5    6
       ┌────┬────┬────┬────┬────┬────┬────┐
       │    │    │    │ ?? │    │    │    │
       └────┴────┴────┴────┴────┴────┴────┘
                        ↑
                   Both "cat" and "dog"
                   want this slot!
```

### Real-World Example

**Employee ID System:**
```
Hash Function: h(id) = id % 100

Employee 10523 → 10523 % 100 = 23
Employee 20623 → 20623 % 100 = 23  ← Collision!

Both IDs hash to slot 23
```

### Collision Visualization

```
Keys: 15, 25, 35, 20
Hash Function: h(k) = k % 10

15 % 10 = 5  ──┐
25 % 10 = 5  ──┼──→ All hash to index 5!
35 % 10 = 5  ──┘
20 % 10 = 0  ────→ Index 0

Index:  0    1    2    3    4    5    6
       ┌────┬────┬────┬────┬────┬────┬────┐
       │ 20 │    │    │    │    │ ?? │    │
       └────┴────┴────┴────┴────┴────┴────┘
                                    ↑
                            3 keys collide!
```

---

## 🔧 Collision Resolution Techniques

### Two Main Approaches

```
┌─────────────────────────────────────┐
│   Collision Resolution Methods      │
├─────────────────────────────────────┤
│                                     │
│  1. Open Chaining                   │
│     (Separate Chaining)             │
│     • Store multiple items per slot │
│     • Use linked lists/trees        │
│                                     │
│  2. Open Addressing                 │
│     • Find another empty slot       │
│     • Probe sequence:               │
│       - Linear Probing              │
│       - Quadratic Probing           │
│       - Double Hashing              │
│                                     │
└─────────────────────────────────────┘
```

---

## 🔗 Open Chaining (Separate Chaining)

### Concept

Each hash table slot contains a **linked list** (or other data structure) to store all elements that hash to that index.

### How It Works

```
Step 1: Calculate hash index
Step 2: If collision occurs, add to linked list at that index
Step 3: Search by traversing the linked list
Step 4: Delete by finding in list and removing
```

### Visual Representation

```
Hash Table with Chaining:

Index    Linked List
─────────────────────────────────────────
  0   →  [20] → NULL
  
  1   →  NULL
  
  2   →  NULL
  
  3   →  NULL
  
  4   →  NULL
  
  5   →  [15] → [25] → [35] → NULL
         ↑      ↑      ↑
      First  Second  Third
      
  6   →  NULL
  
  7   →  NULL
  
  8   →  NULL
  
  9   →  NULL
```

### Step-by-Step Example

**Hash Function:** `h(k) = k % 10`
**Keys to Insert:** 15, 25, 35, 20

#### Step 1: Insert 15
```
h(15) = 15 % 10 = 5

Index 5: Empty
Action: Create linked list with [15]

  5   →  [15] → NULL
```

#### Step 2: Insert 25
```
h(25) = 25 % 10 = 5

Index 5: Contains [15]
Action: Add 25 to the list (COLLISION!)

  5   →  [15] → [25] → NULL
```

#### Step 3: Insert 35
```
h(35) = 35 % 10 = 5

Index 5: Contains [15] → [25]
Action: Add 35 to the list (COLLISION!)

  5   →  [15] → [25] → [35] → NULL
```

#### Step 4: Insert 20
```
h(20) = 20 % 10 = 0

Index 0: Empty
Action: Create linked list with [20]

  0   →  [20] → NULL
```

### Operations

#### Search Operation
```
Search for key 25:

Step 1: Calculate h(25) = 5
Step 2: Go to index 5
Step 3: Traverse linked list:
        [15] ≠ 25, continue
        [25] = 25, FOUND! ✓

Time Complexity: O(1 + α)
where α = load factor (n/m)
```

#### Delete Operation
```
Delete key 25:

Step 1: Calculate h(25) = 5
Step 2: Go to index 5
Step 3: Traverse and remove:
        [15] → [25] → [35]
               ↓ Remove
        [15] → [35]

Before: [15] → [25] → [35] → NULL
After:  [15] → [35] → NULL
```

### Python Implementation

```python
class MyHash:
    def __init__(self, b):
        self.BUCKET = b
        self.table = [[] for x in range(b)]
    
    def insert(self, x):
        i = x % self.BUCKET
        self.table[i].append(x)
    
    def remove(self, x):
        i = x % self.BUCKET
        if x in self.table[i]:
            self.table[i].remove(x)
    
    def search(self, x):
        i = x % self.BUCKET
        return x in self.table[i]

# Example usage
h = MyHash(7)
h.insert(70)
h.insert(71)
h.insert(9)
h.insert(56)
h.insert(72)
print(h.search(56))  # True
h.remove(56)
print(h.search(56))  # False
```

### Advanced Data Structures for Chaining

#### 1. Linked Lists (Standard)
```
Time Complexity:
• Insert: O(1)
• Search: O(n)
• Delete: O(n)

Memory: Low overhead
```

#### 2. Binary Search Trees (BST/AVL/Red-Black)
```
Time Complexity:
• Insert: O(log n)
• Search: O(log n)
• Delete: O(log n)

Memory: Moderate overhead
Best for: Many collisions
```

#### 3. Python Lists
```
Time Complexity:
• Insert: O(1) average
• Search: O(n)
• Delete: O(n)

Memory: Low overhead
Best for: Simple implementation
```

### Comparison of Chain Data Structures

| Data Structure | Insert | Search | Delete | Memory | Complexity |
|---------------|--------|--------|--------|---------|------------|
| Linked List | O(1) | O(n) | O(n) | Low | Simple |
| AVL Tree | O(log n) | O(log n) | O(log n) | Moderate | Complex |
| Red-Black Tree | O(log n) | O(log n) | O(log n) | Moderate | Complex |
| Python List | O(1) | O(n) | O(n) | Low | Simple |

### Advantages of Open Chaining

✅ **Simple to Implement**
- Straightforward logic
- Easy to understand

✅ **No Table Overflow**
- Can handle any number of collisions
- Lists grow dynamically

✅ **Easy Deletion**
- Remove from linked list
- No special handling needed

✅ **Less Sensitive to Hash Function**
- Poor hash functions still work reasonably
- Graceful degradation

### Disadvantages of Open Chaining

❌ **Extra Memory for Pointers**
- Each node needs pointer storage
- Overhead can be significant

❌ **Cache Performance**
- Linked lists are cache-unfriendly
- Random memory access

❌ **Degraded Performance with High Load**
- Long chains slow down operations
- Needs good hash function

---

## 🎯 Open Addressing

### Concept

All elements are stored **within the hash table itself**. When a collision occurs, we **probe** for the next available slot using a systematic sequence.

### General Formula

```
h(k, i) = (h'(k) + f(i)) % table_size

where:
  k = key
  i = probe attempt (0, 1, 2, 3, ...)
  h'(k) = primary hash function
  f(i) = probing function
```

### Three Main Techniques

```
1. Linear Probing:     f(i) = i
2. Quadratic Probing:  f(i) = i²
3. Double Hashing:     f(i) = i × h₂(k)
```

---

## 📏 Linear Probing

### Formula

```
h(k, i) = (h(k) + i) % table_size

If collision at h(k), try:
• h(k) + 0
• h(k) + 1
• h(k) + 2
• h(k) + 3
... until empty slot found
```

### Visual Example

**Keys:** 50, 700, 76, 85, 92, 73, 101
**Hash Function:** `h(k) = k % 10`

#### Step-by-Step Insertion

**1. Insert 50:**
```
h(50) = 50 % 10 = 0
Slot 0 empty → Insert

Index:  0    1    2    3    4    5    6    7    8    9
       [50] [ ] [ ] [ ] [ ] [ ] [ ] [ ] [ ] [ ]
```

**2. Insert 700:**
```
h(700) = 700 % 10 = 0
Slot 0 occupied → Try next
Slot 1 empty → Insert

Index:  0    1    2    3    4    5    6    7    8    9
       [50][700][ ] [ ] [ ] [ ] [ ] [ ] [ ] [ ]
```

**3. Insert 76:**
```
h(76) = 76 % 10 = 6
Slot 6 empty → Insert

Index:  0    1    2    3    4    5    6    7    8    9
       [50][700][ ] [ ] [ ] [ ] [76] [ ] [ ] [ ]
```

**4. Insert 85:**
```
h(85) = 85 % 10 = 5
Slot 5 empty → Insert

Index:  0    1    2    3    4    5    6    7    8    9
       [50][700][ ] [ ] [ ] [85][76] [ ] [ ] [ ]
```

**5. Insert 92:**
```
h(92) = 92 % 10 = 2
Slot 2 empty → Insert

Index:  0    1    2    3    4    5    6    7    8    9
       [50][700][92][ ] [ ] [85][76] [ ] [ ] [ ]
```

**6. Insert 73:**
```
h(73) = 73 % 10 = 3
Slot 3 empty → Insert

Index:  0    1    2    3    4    5    6    7    8    9
       [50][700][92][73][ ] [85][76] [ ] [ ] [ ]
```

**7. Insert 101:**
```
h(101) = 101 % 10 = 1
Slot 1 occupied → Try next (linear probing)

Attempt 0: (1 + 0) % 10 = 1  [Occupied by 700]
Attempt 1: (1 + 1) % 10 = 2  [Occupied by 92]
Attempt 2: (1 + 2) % 10 = 3  [Occupied by 73]
Attempt 3: (1 + 3) % 10 = 4  [Empty] → Insert

Index:  0    1    2    3    4    5    6    7    8    9
       [50][700][92][73][101][85][76][ ] [ ] [ ]
```

### Primary Clustering Problem

```
When consecutive slots fill up:

Bad Distribution:
[50][700][92][73][101][85][76][ ][ ][ ]
 ◆─◆──◆──◆───◆───◆──◆  (cluster)

Problem: New insertions likely to add to cluster!

If we insert key with hash 4, 5, 6, or 7:
• Must probe through entire cluster
• Cluster grows longer
• Performance degrades
```

### Python Implementation

```python
class LinearProbingHashTable:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size
    
    def _hash(self, key):
        return key % self.size
    
    def insert(self, key):
        index = self._hash(key)
        i = 0
        while self.table[(index + i) % self.size] is not None:
            i += 1
        self.table[(index + i) % self.size] = key
    
    def search(self, key):
        index = self._hash(key)
        i = 0
        while self.table[(index + i) % self.size] is not None:
            if self.table[(index + i) % self.size] == key:
                return True
            i += 1
        return False
```

---

## 📐 Quadratic Probing

### Formula

```
h(k, i) = (h(k) + i²) % table_size

If collision at h(k), try:
• h(k) + 0² = h(k) + 0
• h(k) + 1² = h(k) + 1
• h(k) + 2² = h(k) + 4
• h(k) + 3² = h(k) + 9
• h(k) + 4² = h(k) + 16
```

### Visual Example

**Keys:** 50, 700, 76, 85, 92, 73, 101
**Hash Function:** `h(k) = k % 10`

#### Step-by-Step with Quadratic Probing

**Insert 101:**
```
h(101) = 101 % 10 = 1
Slot 1 occupied → Quadratic probing

i=0: (1 + 0²) % 10 = 1  [Occupied]
i=1: (1 + 1²) % 10 = 2  [Occupied]
i=2: (1 + 2²) % 10 = 5  [Occupied]
i=3: (1 + 3²) % 10 = 10 % 10 = 0  [Occupied]
i=4: (1 + 4²) % 10 = 17 % 10 = 7  [Empty] → Insert

Final Table:
Index:  0    1    2    3    4    5    6    7    8    9
       [50][700][92][73][ ] [85][76][101][ ] [ ]
```

### Probing Sequence Comparison

```
Linear vs Quadratic Probing:

Starting at index 1:

Linear:     1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9
Quadratic:  1 → 2 → 5 → 0 → 7 → 6 → 7 → 0 → 5
            (+0)(+1)(+4)(+9)(+16)

Quadratic jumps around more!
Reduces primary clustering.
```

### Secondary Clustering

```
Problem: Keys with same h(k) follow same probe sequence

Example:
Key A: h(A) = 3 → tries 3, 4, 7, 2, 9, ...
Key B: h(B) = 3 → tries 3, 4, 7, 2, 9, ... (same!)

Secondary clustering: Keys hashing to same slot
                     probe identically
```

### Python Implementation

```python
class QuadraticProbingHashTable:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size
    
    def _hash(self, key):
        return key % self.size
    
    def insert(self, key):
        index = self._hash(key)
        i = 0
        while self.table[(index + i * i) % self.size] is not None:
            i += 1
        self.table[(index + i * i) % self.size] = key
    
    def search(self, key):
        index = self._hash(key)
        i = 0
        while self.table[(index + i * i) % self.size] is not None:
            if self.table[(index + i * i) % self.size] == key:
                return True
            i += 1
        return False
```

---

## 🔀 Double Hashing

### Concept

Uses **two hash functions** to determine probe sequence. Most effective open addressing method!

### Formula

```
h(k, i) = (h₁(k) + i × h₂(k)) % table_size

where:
  h₁(k) = primary hash (initial position)
  h₂(k) = secondary hash (step size)
  
Common choices:
  h₁(k) = k % table_size
  h₂(k) = 1 + (k % (table_size - 1))
```

### Why Two Hash Functions?

```
Different keys → Different probe sequences!

Key A: h₁(A) = 5, h₂(A) = 3
       Probes: 5 → 8 → 1 → 4 → 7 → ...

Key B: h₁(B) = 5, h₂(B) = 7
       Probes: 5 → 2 → 9 → 6 → 3 → ...

Same starting point, DIFFERENT paths!
Eliminates secondary clustering.
```

### Detailed Example

**Keys:** 50, 700, 76, 85, 92, 73, 101
**Table Size:** 10
**h₁(k) = k % 10**
**h₂(k) = 1 + (k % 9)**

#### Insert 50
```
h₁(50) = 50 % 10 = 0
h₂(50) = 1 + (50 % 9) = 1 + 5 = 6

Slot 0 empty → Insert at 0

  0    1    2    3    4    5    6    7    8    9
[50] [ ] [ ] [ ] [ ] [ ] [ ] [ ] [ ] [ ]
```

#### Insert 700
```
h₁(700) = 700 % 10 = 0 (collision!)
h₂(700) = 1 + (700 % 9) = 1 + 7 = 8

Probe sequence:
i=0: (0 + 0×8) % 10 = 0  [Occupied]
i=1: (0 + 1×8) % 10 = 8  [Empty] → Insert

  0    1    2    3    4    5    6    7    8    9
[50] [ ] [ ] [ ] [ ] [ ] [ ] [ ][700][ ]
```

#### Insert 76
```
h₁(76) = 76 % 10 = 6
h₂(76) = 1 + (76 % 9) = 1 + 4 = 5

Slot 6 empty → Insert at 6

  0    1    2    3    4    5    6    7    8    9
[50] [ ] [ ] [ ] [ ] [ ] [76] [ ][700][ ]
```

#### Insert 85
```
h₁(85) = 85 % 10 = 5
h₂(85) = 1 + (85 % 9) = 1 + 4 = 5

Slot 5 empty → Insert at 5

  0    1    2    3    4    5    6    7    8    9
[50] [ ] [ ] [ ] [ ] [85][76] [ ][700][ ]
```

#### Insert 92
```
h₁(92) = 92 % 10 = 2
h₂(92) = 1 + (92 % 9) = 1 + 2 = 3

Slot 2 empty → Insert at 2

  0    1    2    3    4    5    6    7    8    9
[50] [ ] [92][ ] [ ] [85][76] [ ][700][ ]
```

#### Insert 73
```
h₁(73) = 73 % 10 = 3
h₂(73) = 1 + (73 % 9) = 1 + 1 = 2

Slot 3 empty → Insert at 3

  0    1    2    3    4    5    6    7    8    9
[50] [ ] [92][73][ ] [85][76] [ ][700][ ]
```

#### Insert 101
```
h₁(101) = 101 % 10 = 1
h₂(101) = 1 + (101 % 9) = 1 + 2 = 3

Slot 1 empty → Insert at 1

  0    1    2    3    4    5    6    7    8    9
[50][101][92][73][ ] [85][76] [ ][700][ ]
```

### Probe Sequence Visualization

```
Key 101: h₁ = 1, h₂ = 3

Probe path if collisions occurred:
i=0: (1 + 0×3) % 10 = 1
i=1: (1 + 1×3) % 10 = 4
i=2: (1 + 2×3) % 10 = 7
i=3: (1 + 3×3) % 10 = 0
i=4: (1 + 4×3) % 10 = 3
i=5: (1 + 5×3) % 10 = 6
i=6: (1 + 6×3) % 10 = 9
i=7: (1 + 7×3) % 10 = 2
i=8: (1 + 8×3) % 10 = 5
i=9: (1 + 9×3) % 10 = 8

Visits all slots! (when table_size and step are coprime)
```

### Python Implementation

```python
class DoubleHashingHashTable:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size
    
    def _hash1(self, key):
        return key % self.size
    
    def _hash2(self, key):
        return 1 + (key % (self.size - 1))
    
    def insert(self, key):
        index = self._hash1(key)
        step_size = self._hash2(key)
        i = 0
        while self.table[(index + i * step_size) % self.size] is not None:
            i += 1
        self.table[(index + i * step_size) % self.size] = key
    
    def search(self, key):
        index = self._hash1(key)
        step_size = self._hash2(key)
        i = 0
        while self.table[(index + i * step_size) % self.size] is not None:
            if self.table[(index + i * step_size) % self.size] == key:
                return True
            i += 1
        return False
```

---

## 📊 Comparison & Best Practices

### Method Comparison Table

| Method | Time (Avg) | Time (Worst) | Memory | Clustering | Complexity |
|--------|-----------|--------------|---------|------------|------------|
| **Chaining** | O(1+α) | O(n) | High | None | Simple |
| **Linear Probing** | O(1/(1-α)) | O(n) | Low | Primary | Simple |
| **Quadratic Probing** | O(1/(1-α)) | O(n) | Low | Secondary | Medium |
| **Double Hashing** | O(1/(1-α)) | O(n) | Low | Minimal | Medium |

*α = load factor (n/m)*

### Load Factor Impact

```
Load Factor (α) = Number of keys / Table size

Examples:
• 10 keys, 20 slots → α = 0.5 (50% full)
• 15 keys, 20 slots → α = 0.75 (75% full)
• 18 keys, 20 slots → α = 0.9 (90% full)

Performance vs Load Factor:

α = 0.5  ██████                    (Good)
α = 0.75 █████████                 (OK)
α = 0.9  ███████████████           (Poor)
α = 1.0  ████████████████████████  (Full!)

Recommendation:
• Chaining: α < 1.0 (can exceed)
• Open Addressing: α < 0.7 (resize before this)
```

### When to Use Each Method

#### Use Chaining When:
✅ Load factor may exceed 0.7
✅ Deletions are frequent
✅ Memory isn't critically constrained
✅ Want simple implementation
✅ Unpredictable number of keys

#### Use Linear Probing When:
✅ Load factor stays low (< 0.5)
✅ Cache performance matters
✅ Memory is limited
✅ Simple implementation needed
✅ Few deletions

#### Use Quadratic Probing When:
✅ Want better distribution than linear
✅ Load factor moderate (< 0.7)
✅ Avoiding primary clustering matters
✅ Table size is prime

#### Use Double Hashing When:
✅ Best performance needed
✅ Load factor can be higher (< 0.7)
✅ Willing to implement two hash functions
✅ Minimizing clustering is critical
✅ Distribution quality matters most

### Practical Guidelines

**1. Hash Function Quality**
```
Good hash function is CRITICAL for open addressing!

Bad:  h(k) = k % 10     (only uses last digit)
Good: h(k) = (k × 2654435761) >> (32 - log₂(m))
```

**2. Table Size**
```
✅ Use prime numbers for table size
✅ Especially important for:
   • Division method
   • Quadratic probing
   • Double hashing

Examples: 7, 11, 13, 17, 23, 31, 53, 97, 199, 401
```

**3. Resizing Strategy**
```
When to resize:
• Chaining: α > 1.0
• Open Addressing: α > 0.7

How to resize:
1. Create new table (2× size, next prime)
2. Rehash all elements
3. Replace old table

Cost: O(n) but amortized O(1)
```

**4. Deletion Handling**
```
Open Addressing Problem:
Deleting creates gaps → breaks search!

Solution: Use "tombstone" markers
• Mark slot as "deleted" (not empty)
• Search continues through tombstones
• Insert can reuse tombstone slots

[50][DEL][92][73] → Search continues past DEL
```

### Performance Summary

**Best Overall:** Double Hashing
- Minimal clustering
- Good distribution
- Handles high load factors

**Simplest:** Chaining with Linked Lists
- Easy to implement
- Works with any load factor
- Predictable behavior

**Most Cache-Friendly:** Linear Probing
- Sequential memory access
- Best for small tables
- Low load factors only

**Balanced Choice:** Chaining with Python Lists
- Simple implementation
- Good performance
- Flexible and practical

---

## 🎓 Summary

### Key Takeaways

1. **Collisions are Inevitable**
   - Finite table, infinite keys
   - Good hash functions minimize but don't eliminate

2. **Two Main Resolution Strategies**
   - **Chaining:** Store multiple items per slot
   - **Open Addressing:** Find another empty slot

3. **Trade-offs Exist**
   - Memory vs Speed
   - Simplicity vs Performance
   - Guaranteed vs Average complexity

4. **Choice Depends on Context**
   - Load factor expectations
   - Memory constraints
   - Performance requirements
   - Implementation complexity tolerance

5. **Quality Matters**
   - Hash function quality is critical
   - Table size affects performance
   - Load factor management is essential

### Final Recommendations

**For Learning:** Start with chaining using Python lists
**For Production:** Consider double hashing or chaining with BSTs
**For Embedded Systems:** Linear probing with low load factor
**For General Use:** Chaining with linked lists (most implementations use this)

Understanding collision resolution is fundamental to implementing efficient hash-based data structures like hash tables, dictionaries, and sets!
