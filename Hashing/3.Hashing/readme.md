# Complete Guide to Hashing

> Overcoming Direct Access Table limitations with O(1) operations for any data type

---

## 📋 Table of Contents

- [Introduction](#introduction)
- [Limitations of Direct Access Tables](#limitations-of-direct-access-tables)
- [What is Hashing?](#what-is-hashing)
- [Components of Hashing](#components-of-hashing)
- [Hash Functions](#hash-functions)
- [String Hashing](#string-hashing)
- [Collision Handling](#collision-handling)
- [Summary](#summary)

---

## 🎯 Introduction

While Direct Access Tables (DATs) offer efficient O(1) operations, they have significant limitations that make them impractical for real-world applications. **Hashing** solves these problems by using a hash function to map keys of any type to array indices, maintaining O(1) complexity while being memory-efficient.

---

## ❌ Limitations of Direct Access Tables

### 1. Large Key Space Problem

**Memory Usage Issue:**
```
Key Range: 0 to 4,294,967,295 (32-bit integers)
Required Array Size: 4 billion elements
Memory Needed: ~16 GB (for boolean values)

Actual Keys Used: 100
Memory Efficiency: 0.0000023%
```

**Wasted Space Visualization:**
```
Used:    ■ (100 elements)
Unused:  □□□□□□□□□□□□□□□□□□□□□□□□ (4+ billion elements)
         ↑
         99.99% waste!
```

### 2. Fixed Key Range

**Limited Flexibility:**
- ❌ Cannot handle dynamic key ranges
- ❌ Must know all possible keys in advance
- ❌ Cannot adapt to changing requirements

**Example:**
```python
# Works only for predetermined range
dat = [False] * 1001  # Keys: 0-1000

# What if we need to store key 5000? 
# Must recreate entire structure!
```

### 3. Inefficiency for Complex Keys

**Non-Integer Keys Problem:**

```
✅ Works:  42, 100, 999 (integers)
❌ Fails:  "john@email.com" (string)
❌ Fails:  3.14159 (float)
❌ Fails:  {"name": "Alice"} (object)
```

**Challenge:**
```
How to use "john@email.com" as array index?
array["john@email.com"] ← Not possible in most languages!
```

---

## 🔐 What is Hashing?

Hashing uses a **hash function** to convert keys of any data type into small integer indices for storage in a **hash table**.

### Basic Concept

```
     Any Key Type           Hash Function         Array Index
         ↓                       ↓                      ↓
    "john@email.com"  ──→  h("john@email.com")  ──→    7
         42           ──→       h(42)           ──→    2
       3.14159        ──→     h(3.14159)        ──→    5
```

### Visual Representation

```
Keys (Any Type)              Hash Function              Hash Table
                                  ↓
   "Alice"    ──┐                                    Index:
        11    ──┤                                    ┌───┬───────┐
     "Bob"    ──┤──→  Hash Function H(x)  ──→      0│   │       │
       150    ──┤                                    ├───┼───────┤
   "Charlie"  ──┘                                   1│ T │ "Bob" │
                                                     ├───┼───────┤
                                                    2│ T │  11   │
                                                     ├───┼───────┤
                                                    3│   │       │
                                                     ├───┼───────┤
                                                    4│ T │ 150   │
                                                     ├───┼───────┤
                                                    5│ T │"Alice"│
                                                     └───┴───────┘
```

### Simple Example

**Hash Function:** `H(x) = x % 10`

**Keys:** [11, 12, 13, 14, 15]

```
Key    Calculation       Index   Hash Table
11  →  11 % 10 = 1   →    1    ┌───┬────┐
12  →  12 % 10 = 2   →    2    0│   │    │
13  →  13 % 10 = 3   →    3    ├───┼────┤
14  →  14 % 10 = 4   →    4    1│ T │ 11 │
15  →  15 % 10 = 5   →    5    ├───┼────┤
                               2│ T │ 12 │
                               ├───┼────┤
                               3│ T │ 13 │
                               ├───┼────┤
                               4│ T │ 14 │
                               ├───┼────┤
                               5│ T │ 15 │
                               ├───┼────┤
                               6│   │    │
                               ├───┼────┤
                               7│   │    │
                               ├───┼────┤
                               8│   │    │
                               ├───┼────┤
                               9│   │    │
                               └───┴────┘
```

---

## 🧩 Components of Hashing

### 1. Key
**Definition:** The input data that needs to be stored or retrieved.

**Types:**
- ✅ Integers: `42`, `1000`, `-5`
- ✅ Strings: `"Alice"`, `"john@email.com"`
- ✅ Floats: `3.14`, `2.718`
- ✅ Objects: `{"id": 123, "name": "Bob"}`

### 2. Hash Function
**Definition:** A function that converts keys into array indices.

**Properties of a Good Hash Function:**
```
1. Deterministic:  Same key → Same hash value always
2. Efficient:      Fast computation
3. Uniform:        Distributes keys evenly
4. Minimize:       Reduces collisions
```

**Visual Process:**
```
Input Key → Hash Function → Hash Value (Index)
    ↓             ↓              ↓
  "Alice"    H("Alice")         3
    ↓             ↓              ↓
Calculate  →  Process  →  Return Index
```

### 3. Hash Table
**Definition:** An array that stores data using hash values as indices.

**Structure:**
```
Hash Table = Array + Hash Function + Collision Handling

┌─────────────────────────────────────┐
│         Hash Table Structure        │
├─────────────────────────────────────┤
│  Index  │  Status  │     Value      │
├─────────┼──────────┼────────────────┤
│    0    │  Empty   │       -        │
│    1    │  Filled  │  "Bob", 25     │
│    2    │  Empty   │       -        │
│    3    │  Filled  │  "Alice", 30   │
│    4    │  Empty   │       -        │
│    5    │  Filled  │  "Charlie", 35 │
└─────────┴──────────┴────────────────┘
```

### Component Interaction

```
┌─────────┐
│   Key   │ (Any data type)
└────┬────┘
     │
     ▼
┌─────────────────┐
│  Hash Function  │ (Converts key to index)
└────┬────────────┘
     │
     ▼
┌─────────────────┐
│   Hash Table    │ (Stores data at index)
└─────────────────┘
```

---

## 🔢 Hash Functions

### 1. Division Method

**Overview:** Simplest hash function using modulo operation.

**Formula:** `h(k) = k mod M`

**Where:**
- `k` = key value
- `M` = hash table size (preferably prime number)

**Example:**
```
Key: k = 12345
Table Size: M = 95

Calculation:
h(12345) = 12345 mod 95 = 90

Result: Store at index 90
```

**Visual Process:**
```
    12345
      ↓
   12345 ÷ 95 = 130 remainder 90
      ↓
  h(12345) = 90

Hash Table:
Index:  ... 88  89  [90]  91  92 ...
              │    │    │    │
              Empty  12345  Empty Empty
```

**✅ Pros:**
- Extremely fast (single division)
- Simple to implement
- Works for any table size

**❌ Cons:**
- Poor distribution with bad M choices
- Consecutive keys → consecutive indices
- Clustering problems

**Best Practices:**
```python
# ✅ Good: M is prime
M = 97
h(k) = k % 97

# ❌ Bad: M is power of 2
M = 128  # Only uses last 7 bits
h(k) = k % 128
```

### 2. Mid Square Method

**Overview:** Square the key and extract middle digits.

**Formula:** `h(k) = middle digits of (k × k)`

**Steps:**
```
Step 1: Square the key → k²
Step 2: Extract middle r digits
Step 3: Use as hash value
```

**Example:**
```
Key: k = 60
Table Size: 100 (need 2 digits)

Step 1: k² = 60 × 60 = 3600
Step 2: Extract middle 2 digits
        3[60]0
         ↑↑
Step 3: h(60) = 60
```

**Visual Process:**
```
    60
     ↓  Square
  60 × 60 = 3600
     ↓  Extract middle
  3 [6 0] 0
     ↓
  Index = 60
```

**More Examples:**
```
k = 51  →  51² = 2601  →  [60]  →  h(51) = 60
k = 70  →  70² = 4900  →  [90]  →  h(70) = 90
k = 80  →  80² = 6400  →  [40]  →  h(80) = 40
```

**✅ Pros:**
- Uses all digits of key
- Better distribution than simple methods
- Less affected by digit patterns

**❌ Cons:**
- Large keys create huge squares
- Overflow issues with big numbers
- Collisions still possible

### 3. Digit Folding Method

**Overview:** Divide key into parts, sum them up.

**Formula:** `h(k) = k₁ + k₂ + k₃ + ... + kₙ`

**Example:**
```
Key: k = 12345

Step 1: Divide into parts
        k₁ = 12
        k₂ = 34
        k₃ = 5

Step 2: Sum the parts
        h(12345) = 12 + 34 + 5 = 51
```

**Visual Process:**
```
    12345
      ↓  Divide
   12 | 34 | 5
    ↓    ↓    ↓
   12 + 34 + 5 = 51
              ↓
        Index = 51
```

**Phone Number Example:**
```
Phone: 555-1234-567

Method 1: Two-digit parts
555 + 12 + 34 + 56 + 7 = 664

Method 2: Three-digit parts
555 + 123 + 456 + 7 = 1141
```

**Boundary Folding Variation:**
```
Key: 12345

Standard:  12 + 34 + 5 = 51

Boundary:  12 + 43(reversed) + 5 = 60
           ↓     ↑
        Reverse middle parts
```

**✅ Pros:**
- Simple implementation
- Works with variable key lengths
- Good for phone numbers, IDs

**❌ Cons:**
- Collisions still occur
- Sum may exceed table size
- Loses some key information

### 4. Multiplication Method

**Overview:** Multiply key by constant, extract fractional part, scale by table size.

**Formula:** `h(k) = floor(M × (k × A mod 1))`

**Where:**
- `k` = key
- `M` = table size
- `A` = constant (0 < A < 1)
- `mod 1` = fractional part

**Example:**
```
Key: k = 12345
Constant: A = 0.357840
Table Size: M = 100

Step 1: k × A = 12345 × 0.357840 = 4417.5348

Step 2: Extract fractional part
        4417.5348 mod 1 = 0.5348

Step 3: Multiply by M
        0.5348 × 100 = 53.48

Step 4: Take floor
        h(12345) = floor(53.48) = 53
```

**Visual Process:**
```
    12345
      ↓  × 0.357840
  4417.5348
      ↓  Fractional part
    0.5348
      ↓  × 100
    53.48
      ↓  Floor
   Index = 53
```

**Golden Ratio Constant:**
```
Optimal A ≈ (√5 - 1) / 2 ≈ 0.6180339887

Why? Provides excellent distribution properties!
```

**Step-by-Step with Golden Ratio:**
```
k = 123, M = 10, A = 0.618

1. k × A = 123 × 0.618 = 76.014
2. Fractional = 0.014
3. × M = 0.014 × 10 = 0.14
4. Floor = 0

Result: h(123) = 0
```

**✅ Pros:**
- Works well with non-prime M
- Less clustering than division
- Good distribution

**❌ Cons:**
- More complex computation
- A must be chosen carefully
- Floating-point precision issues

### Hash Function Comparison

| Method | Computation | Distribution | Collision Risk | Best For |
|--------|-------------|--------------|----------------|----------|
| Division | Fast | Fair | Medium | General use |
| Mid Square | Medium | Good | Medium-Low | Numeric keys |
| Digit Folding | Fast | Fair | Medium | IDs, Phone # |
| Multiplication | Medium | Excellent | Low | All scenarios |

---

## 🔤 String Hashing

### Why String Hashing is Different

**Challenge:**
```
Strings are sequences of characters
Cannot directly use as array index
Need to convert: "hello" → integer

ASCII Values: 'h'=104, 'e'=101, 'l'=108, 'o'=111
```

### Method 1: Simple Hashing (Summing ASCII)

**Formula:** `h(S) = Σ ASCII(S[i])`

**Example:**
```
String: "abc"

'a' = 97
'b' = 98  
'c' = 99
────────
Sum = 294

h("abc") = 294
```

**Visual:**
```
 "abc"
  ↓ ↓ ↓
  a b c
  ↓ ↓ ↓
 97+98+99 = 294
      ↓
 h("abc") = 294
```

**Problem - Anagrams Collide:**
```
"abc" → 97+98+99 = 294
"bca" → 98+99+97 = 294  ← Same hash! ⚠️
"cab" → 99+97+98 = 294  ← Collision!
```

**✅ Pros:** Very simple
**❌ Cons:** Terrible distribution, many collisions

### Method 2: Polynomial Hashing (Rolling Hash)

**Formula:** `h(S) = Σ ASCII(S[i]) × p^(n-1-i) mod M`

**Where:**
- `p` = prime base (usually 31 or 53)
- `n` = string length
- `M` = large prime for modulo

**Example:**
```
String: "abc"
Base: p = 31

h("abc") = ASCII('a')×31² + ASCII('b')×31¹ + ASCII('c')×31⁰
         = 97×961 + 98×31 + 99×1
         = 93217 + 3038 + 99
         = 96354
```

**Visual:**
```
    "abc"
     ↓ ↓ ↓
     a b c
     ↓ ↓ ↓
    97 98 99
     ↓ ↓ ↓
×31² ×31¹ ×31⁰
     ↓ ↓ ↓
93217+3038+99 = 96354
```

**Why This Works Better:**
```
"abc" → 97×31² + 98×31 + 99 = 96354
"bca" → 98×31² + 99×31 + 97 = 97217  ← Different!
"cab" → 99×31² + 97×31 + 98 = 97186  ← Different!

Position matters! No more anagram collisions.
```

**Code Implementation:**
```python
def polynomial_hash(s, p=31, m=1e9+9):
    hash_value = 0
    p_pow = 1
    
    for char in s:
        hash_value = (hash_value + ord(char) * p_pow) % m
        p_pow = (p_pow * p) % m
    
    return hash_value

# Example
print(polynomial_hash("abc"))  # 96354
print(polynomial_hash("bca"))  # 97217
```

**✅ Pros:** 
- Excellent distribution
- Position-sensitive
- Widely used in practice

**❌ Cons:** 
- More computation
- Overflow concerns with large strings

### Method 3: DJB2 Hash Function

**Overview:** Created by Daniel J. Bernstein, uses bit shifting.

**Formula:**
```
h = 5381
For each character c:
    h = ((h << 5) + h) + ASCII(c)
    h = (h × 33) + ASCII(c)
```

**Example:**
```
String: "abc"

Initial: h = 5381

Character 'a' (ASCII 97):
h = ((5381 << 5) + 5381) + 97
h = (172192 + 5381) + 97
h = 177670

Character 'b' (ASCII 98):
h = ((177670 << 5) + 177670) + 98
h = 5863208

Character 'c' (ASCII 99):
h = ((5863208 << 5) + 5863208) + 99
h = 193485963
```

**Visual Process:**
```
     5381 (Start)
       ↓
× 33 + 'a' → 177670
       ↓
× 33 + 'b' → 5863208
       ↓
× 33 + 'c' → 193485963
       ↓
   Final Hash
```

**Bit Shift Explanation:**
```
h << 5 means shift left by 5 bits
h << 5 = h × 32

So: (h << 5) + h = 32h + h = 33h

That's why it's called "multiply by 33"
```

**Code Implementation:**
```python
def djb2_hash(s):
    hash_value = 5381
    
    for char in s:
        hash_value = ((hash_value << 5) + hash_value) + ord(char)
        # Same as: hash_value = hash_value * 33 + ord(char)
    
    return hash_value

# Example
print(djb2_hash("abc"))  # 193485963
```

**Why 5381 and 33?**
```
5381: Magic number chosen empirically for good distribution
33:   Prime-like properties (32 + 1), efficient bit operations
```

**✅ Pros:**
- Very fast (uses bit shifting)
- Excellent distribution
- Widely used in hash tables
- Simple to implement

**❌ Cons:**
- Not cryptographically secure
- Can overflow (use modulo)

### String Hashing Comparison

| Method | Speed | Distribution | Collisions | Use Case |
|--------|-------|--------------|------------|----------|
| Simple Sum | Very Fast | Poor | High | Never use |
| Polynomial | Medium | Excellent | Low | String matching |
| DJB2 | Fast | Excellent | Low | Hash tables |

---

## ⚔️ Collision Handling

**What is a Collision?**
```
When two different keys hash to the same index:

h("john") = 5
h("jane") = 5  ← Collision!

Both want to occupy index 5 in hash table
```

### Common Collision Resolution Techniques

#### 1. Chaining (Separate Chaining)

**Concept:** Each table slot contains a linked list of all elements that hash to that index.

**Visual:**
```
Hash Table with Chaining:

Index    Linked List
  0   →  Empty
  1   →  [Bob, 25] → [Tom, 30] → NULL
  2   →  Empty
  3   →  [Alice, 28] → NULL
  4   →  Empty
  5   →  [John, 35] → [Jane, 27] → [Jim, 40] → NULL
  6   →  Empty
```

**✅ Pros:** Simple, never fills up, easy deletion
**❌ Cons:** Extra memory for pointers, cache unfriendly

#### 2. Open Addressing (Linear Probing)

**Concept:** If slot is full, check next slot until empty one found.

**Visual:**
```
Insert key with h(k) = 3, but index 3 is full:

Index:  0    1    2    3    4    5    6
       ┌────┬────┬────┬────┬────┬────┬────┐
       │    │    │    │ X  │    │    │    │
       └────┴────┴────┴▲───┴────┴────┴────┘
                       │ Full!
                       ↓ Try next
                     ┌────┬────┬────┐
                     │ X  │ ✓  │    │
                     └────┴────┴────┘
                       3    4    5
                            ↑
                     Store here!
```

**✅ Pros:** No extra memory, cache friendly
**❌ Cons:** Clustering, table can fill up

---

## 📊 Summary

### Hashing vs Direct Access Table

| Feature | Direct Access Table | Hashing |
|---------|-------------------|---------|
| Key Types | Integers only | Any type |
| Memory | O(key range) | O(actual items) |
| Operations | O(1) guaranteed | O(1) average |
| Space Efficiency | Poor | Excellent |
| Flexibility | Low | High |

### Key Takeaways

1. **Hashing solves DAT limitations** by using hash functions to map any key type to indices
2. **Hash functions** must be fast, deterministic, and distribute keys uniformly
3. **Multiple methods** exist: Division, Mid Square, Folding, Multiplication
4. **String hashing** requires special techniques: Simple Sum, Polynomial, DJB2
5. **Collisions** are inevitable; handle via chaining or open addressing
6. **Trade-off:** Guaranteed O(1) → Average O(1), but gain flexibility and efficiency

### When to Use Hashing

✅ **Use Hashing When:**
- Need fast lookups with any key type
- Keys are strings, floats, or objects
- Memory efficiency matters
- Dynamic key range

❌ **Use Direct Access Table When:**
- Keys are small integers in known range
- Need guaranteed O(1) (not average)
- Memory is abundant
- Simplicity over flexibility

**Hashing is the foundation** of hash tables, hash maps, dictionaries, sets, and countless other data structures used in modern programming!
