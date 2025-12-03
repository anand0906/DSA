# Hashing - Quick Reference Guide

> Simple notes and complexity calculation guide

---

## 📚 What is Hashing?

**Simple Definition:** A technique to store and find data super fast (O(1) time)

**How it works:**
1. Take any key (number, string, anything)
2. Put it through a **hash function** → Get a number (index)
3. Store/find data at that index in an array (hash table)

**Example:**
```
Key: "Alice" → Hash Function → Index: 5 → Store in array[5]
```

---

## 🔑 Three Main Parts

### 1. Key
- The data you want to store
- Can be: number, string, object, anything

### 2. Hash Function
- Converts key → index number
- Example: `h(key) = key % 10`

### 3. Hash Table
- An array that stores the data
- Uses hash function to find where to store

---

## 🎯 Why Use Hashing?

**Problem with other structures:**
```
Array (unsorted): Search takes O(n) - too slow!
Array (sorted):   Insert takes O(n) - too slow!
Linked List:      Search takes O(n) - too slow!
Tree:             Operations take O(log n) - good but not best

Hashing:          Everything takes O(1) - BEST! ⚡
```

---

## 🔢 Common Hash Functions

### 1. Division Method (Most Common)
```
h(key) = key % table_size

Example: h(25) = 25 % 10 = 5
```

### 2. For Strings
```
Add ASCII values of characters

"abc" → 97 + 98 + 99 = 294
```

---

## ⚡ Hash Collision

**What is it?**
When two different keys produce the same hash value.

**Example:**
```
h(15) = 15 % 10 = 5
h(25) = 25 % 10 = 5  ← Both want index 5! (Collision!)
```

**Why it happens:**
- Infinite possible keys
- Finite array slots
- Some keys MUST share slots!

---

## 🔧 Fixing Collisions - Two Ways

### Method 1: Chaining (Easy way)
**Idea:** Each slot holds a list of items

```
Index 5 → [15] → [25] → [35] → NULL
          (All keys that hash to 5)
```

**When to use:**
- ✅ When you're okay with extra memory
- ✅ When you don't know how many items
- ✅ When you want simple code

### Method 2: Open Addressing (Array-only way)
**Idea:** If slot is full, find next empty slot

**Three types:**

**A. Linear Probing** (Check next slot)
```
Try index 5 → Full
Try index 6 → Full
Try index 7 → Empty! Store here!
```

**B. Quadratic Probing** (Jump by squares)
```
Try index 5 → Full
Try index 6 (5+1²) → Full
Try index 9 (5+2²=5+4) → Empty! Store here!
```

**C. Double Hashing** (Use second hash for jumps)
```
h1(key) = starting position
h2(key) = how much to jump

Try position, jump by h2(key) each time
```

**When to use:**
- ✅ When memory is limited
- ✅ When you want all data in one array
- ✅ When you know max number of items

---

## ⏱️ Time Complexity

### Best Case (What we want):
```
Insert:  O(1)  - Put directly in empty slot
Search:  O(1)  - Found immediately
Delete:  O(1)  - Remove directly
```

### With Chaining:
```
Average: O(1 + α)  where α = items/table_size

If α is small (table not full): ≈ O(1) ✓
If α is large (table very full): ≈ O(n) ✗
```

### With Open Addressing:
```
Average: O(1/(1-α))

If α < 0.5 (table half full):    Fast ✓
If α > 0.7 (table very full):    Slow ✗
If α = 1.0 (table completely full): Can't insert! ✗
```

---

## 📊 How to Calculate Complexity in Problems

### Rule 1: Count Hash Function Calls

**Example Problem:** Insert n items into hash table
```
For each item:
  - Calculate hash: O(1)
  - Insert: O(1)

Total: n × O(1) = O(n)
```

### Rule 2: Consider Load Factor (α)

**Load Factor (α) = Number of items / Table size**

**Example:**
```
10 items in table of size 20
α = 10/20 = 0.5

Search time = O(1 + 0.5) = O(1.5) ≈ O(1)
```

### Rule 3: Worst Case = All Collisions

**Example:** All n items hash to same index
```
Chaining: Search through list of n items = O(n)
```

### Rule 4: Operations Complexity

**Single Operation:**
```
Insert one item:  O(1) average
Search one item:  O(1) average
Delete one item:  O(1) average
```

**Multiple Operations:**
```
Insert n items:   O(n)
Search for k items: O(k)
```

### Rule 5: Consider Collision Resolution

**Chaining:**
```
Best case:  O(1) - direct access
Average:    O(1 + α)
Worst case: O(n) - all in one chain
```

**Open Addressing:**
```
Best case:  O(1) - first try
Average:    O(1/(1-α))
Worst case: O(n) - probe entire table
```

---

## 🧮 Complexity Calculation Examples

### Example 1: Insert 100 items
```
Question: Insert 100 items into hash table of size 200

Solution:
- Each insert: O(1)
- 100 inserts: 100 × O(1) = O(100) = O(n)
- Load factor: α = 100/200 = 0.5

Answer: O(n) where n = number of items
```

### Example 2: Search for item
```
Question: Search for key in hash table with α = 0.5

Solution:
Chaining: O(1 + α) = O(1 + 0.5) = O(1)
Open Addressing: O(1/(1-0.5)) = O(1/0.5) = O(2) = O(1)

Answer: O(1) average case
```

### Example 3: Insert then Search
```
Question: Insert n items, then search for each one

Solution:
- Insert n items: O(n)
- Search n times: n × O(1) = O(n)
- Total: O(n) + O(n) = O(2n) = O(n)

Answer: O(n)
```

### Example 4: Worst Case Scenario
```
Question: All items collide at same index (chaining)

Solution:
- Insert creates chain of n items
- Search requires traversing chain: O(n)

Answer: O(n) worst case
```

---

## 🎯 Quick Decision Guide

### Choose Chaining if:
- Don't know how many items
- Okay with extra memory
- Want simple code
- Many deletions

### Choose Open Addressing if:
- Know max items
- Limited memory
- Want cache-friendly (faster)
- Few deletions

### Choose Load Factor:
- Keep α < 0.7 for good performance
- Resize table when α > 0.7

---

## 📝 Common Problem Patterns

### Pattern 1: Count Frequencies
```
Problem: Count how many times each element appears

Solution:
hash_table = {}
for item in array:          # O(n)
    hash_table[item] += 1   # O(1)

Time: O(n)
Space: O(n)
```

### Pattern 2: Find Duplicates
```
Problem: Check if array has duplicates

Solution:
seen = {}
for item in array:          # O(n)
    if item in seen:        # O(1)
        return True
    seen[item] = True       # O(1)

Time: O(n)
Space: O(n)
```

### Pattern 3: Two Sum
```
Problem: Find two numbers that add to target

Solution:
seen = {}
for num in array:           # O(n)
    complement = target - num
    if complement in seen:  # O(1)
        return True
    seen[num] = True        # O(1)

Time: O(n)
Space: O(n)
```

---

## 💡 Key Points to Remember

### 1. Time Complexity
```
✓ Average: O(1) for insert, search, delete
✗ Worst:   O(n) when all collide
```

### 2. Space Complexity
```
Always O(n) where n = number of items stored
```

### 3. When Hashing is Best
```
✓ Need fast lookup
✓ Key-value pairs
✓ Checking existence
✓ Counting frequencies
✓ Finding duplicates
```

### 4. When NOT to Use Hashing
```
✗ Need sorted order
✗ Need min/max quickly
✗ Need range queries
✗ Memory is very limited
```

---

## 🔍 Complexity Cheat Sheet

### Basic Operations
| Operation | Average | Worst | Space |
|-----------|---------|-------|-------|
| Insert | O(1) | O(n) | O(n) |
| Search | O(1) | O(n) | O(n) |
| Delete | O(1) | O(n) | O(n) |

### With Load Factor α
| α Value | Performance | Recommendation |
|---------|-------------|----------------|
| α < 0.5 | Excellent | Keep it here! |
| 0.5 < α < 0.7 | Good | Still okay |
| α > 0.7 | Poor | Resize table! |
| α = 1.0 | Very Poor | Must resize! |

### Collision Methods
| Method | Average | Worst | Memory |
|--------|---------|-------|--------|
| Chaining | O(1+α) | O(n) | High |
| Linear Probing | O(1/(1-α)) | O(n) | Low |
| Double Hashing | O(1/(1-α)) | O(n) | Low |

---

## 📐 Formula Sheet

### Hash Functions
```
Division:      h(k) = k % m
Multiplication: h(k) = floor(m × (k×A mod 1))
String:        h(s) = Σ ASCII(s[i])
```

### Collision Resolution
```
Linear Probing:    h(k,i) = (h(k) + i) % m
Quadratic Probing: h(k,i) = (h(k) + i²) % m
Double Hashing:    h(k,i) = (h₁(k) + i×h₂(k)) % m
```

### Load Factor
```
α = n / m
where n = number of items
      m = table size
```

### Performance
```
Chaining:        Time = O(1 + α)
Open Addressing: Time = O(1/(1-α))
```

---

## 🎓 Simple Summary

**What:** Store and find data instantly using a hash function

**How:** Key → Hash Function → Index → Array

**Problem:** Collisions (two keys → same index)

**Solution:** 
- Chaining: Use lists at each index
- Open Addressing: Find another empty slot

**Time:** O(1) average, O(n) worst

**Best for:** Fast lookup, counting, finding duplicates

**Remember:** Keep table size bigger than number of items (α < 0.7)!

---

## 🧠 Mental Model

Think of hashing like:
- **Phone book organized by first letter** (hash function = first letter)
- **Post office boxes** (each box number is a hash value)
- **Restaurant table assignment** (host assigns table based on party size)

The key insight: **Calculate where to go, don't search for it!**
