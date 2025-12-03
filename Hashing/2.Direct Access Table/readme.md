# Direct Access Table (DAT)

> Also known as **Direct Address Table** - A data structure for constant-time operations

---

## 📋 Table of Contents

- [Introduction](#introduction)
- [How It Works](#how-it-works)
- [Visual Representation](#visual-representation)
- [Operations](#operations)
- [Implementation Example](#implementation-example)
- [Time Complexity](#time-complexity)
- [Advantages & Disadvantages](#advantages--disadvantages)

---

## 🎯 Introduction

A **Direct Access Table** is a data structure that allows direct access to elements based on their keys. It's particularly useful when:

- The universe of possible keys is **reasonably small**
- Keys are **known in advance**
- You need **constant time O(1)** operations

### Key Concept

An array is used where the **index corresponds to the key**, allowing:
- ✅ Insertion in O(1)
- ✅ Deletion in O(1)
- ✅ Lookup in O(1)

---

## ⚙️ How It Works

### 1. Array Representation
A Direct Access Table is represented by an array where each position corresponds to a key in the universe. The key serves as the index in the array.

### 2. Key-Value Storage
Each key directly points to a specific location in the array where the value associated with that key is stored.

### 3. Core Operations

| Operation | Description | Time Complexity |
|-----------|-------------|-----------------|
| **Insertion** | Place the value at the array index corresponding to the key | O(1) |
| **Deletion** | Remove the value by setting the array index to null/sentinel value | O(1) |
| **Lookup** | Access the value by directly using the key as the index | O(1) |

---

## 🎨 Visual Representation

### Concept Diagram

```
Key (Number) → Index in Array → Value (True/False)

Example: Storing numbers 1-100

Index:  0    1    2    3    ...   42   ...   75   ...  100
       ┌────┬────┬────┬────┬─────┬────┬─────┬────┬─────┬────┐
Value: │ F  │ F  │ F  │ F  │ ... │ T  │ ... │ T  │ ... │ F  │
       └────┴────┴────┴────┴─────┴────┴─────┴────┴─────┴────┘
                                    ↑           ↑
                                   42          75
                              (Present)    (Present)
```

### Insert Operation Visual

```
Before Insert(42):
Index 42: [False] ❌

After Insert(42):
Index 42: [True] ✅

direct_access_table[42] = True
```

### Search Operation Visual

```
Search(42):
Check Index 42 → [True] ✅ → Found!

Search(50):
Check Index 50 → [False] ❌ → Not Found!
```

### Delete Operation Visual

```
Before Delete(42):
Index 42: [True] ✅

After Delete(42):
Index 42: [False] ❌

direct_access_table[42] = False
```

---

## 💻 Implementation Example

### Problem Statement
Create a data structure for random numbers between 1-100 that supports insertion, deletion, and searching in O(1) time complexity.

### Step 1: Initialize the Direct Access Table

```python
# Create array of size 101 (index 0 to 100)
direct_access_table = [False] * 101
```

**Visual:**
```
Index:  0    1    2    3    4    ...   99   100
       ┌────┬────┬────┬────┬────┬─────┬────┬────┐
Value: │ F  │ F  │ F  │ F  │ F  │ ... │ F  │ F  │
       └────┴────┴────┴────┴────┴─────┴────┴────┘
        All initialized to False
```

### Step 2: Insert Operation

```python
def insert(number):
    """Insert a number into the table - O(1)"""
    direct_access_table[number] = True

# Example
insert(42)
insert(75)
insert(10)
```

**Visual After Insertions:**
```
Index:  0    1    ...  10   ...   42   ...   75   ...  100
       ┌────┬────┬─────┬────┬─────┬────┬─────┬────┬─────┬────┐
Value: │ F  │ F  │ ... │ T  │ ... │ T  │ ... │ T  │ ... │ F  │
       └────┴────┴─────┴────┴─────┴────┴─────┴────┴─────┴────┘
                        ✅          ✅          ✅
                        10          42          75
```

### Step 3: Search Operation

```python
def search(number):
    """Search for a number in the table - O(1)"""
    return direct_access_table[number]

# Examples
print(search(42))  # Output: True ✅
print(search(50))  # Output: False ❌
```

### Step 4: Delete Operation

```python
def delete(number):
    """Delete a number from the table - O(1)"""
    direct_access_table[number] = False

# Example
delete(42)
```

**Visual After Deletion:**
```
Index:  0    1    ...  10   ...   42   ...   75   ...  100
       ┌────┬────┬─────┬────┬─────┬────┬─────┬────┬─────┬────┐
Value: │ F  │ F  │ ... │ T  │ ... │ F  │ ... │ T  │ ... │ F  │
       └────┴────┴─────┴────┴─────┴────┴─────┴────┴─────┴────┘
                        ✅          ❌          ✅
                        10      (deleted)      75
```

---

## 🔄 Complete Example in Action

```python
# Initialize the direct access table
direct_access_table = [False] * 101

# Insert some numbers
insert(42)
insert(75)
insert(10)

# Search for numbers
print(search(42))  # Output: True (42 is present) ✅
print(search(50))  # Output: False (50 is not present) ❌

# Delete a number
delete(42)

# Search again after deletion
print(search(42))  # Output: False (42 has been deleted) ❌
```

### Operation Flow Diagram

```
┌─────────────┐
│  Insert(42) │
└──────┬──────┘
       │
       ▼
┌─────────────────────────┐
│ table[42] = True        │
│ Time: O(1)              │
└──────┬──────────────────┘
       │
       ▼
┌─────────────┐
│  Search(42) │
└──────┬──────┘
       │
       ▼
┌─────────────────────────┐
│ return table[42]        │
│ Result: True ✅         │
│ Time: O(1)              │
└──────┬──────────────────┘
       │
       ▼
┌─────────────┐
│  Delete(42) │
└──────┬──────┘
       │
       ▼
┌─────────────────────────┐
│ table[42] = False       │
│ Time: O(1)              │
└─────────────────────────┘
```

---

## ⏱️ Time Complexity

| Operation | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Insert | O(1) | O(n) |
| Search | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Initialize | O(n) | O(n) |

*where n = size of the universe of keys*

---

## ⚖️ Advantages & Disadvantages

### ✅ Advantages

1. **Constant Time Operations**
   - All operations (insert, search, delete) execute in O(1) time
   - No traversal or searching required

2. **Simple Implementation**
   - Easy to understand and implement
   - Minimal code required

3. **Predictable Performance**
   - Guaranteed constant time for all operations
   - No worst-case scenarios

### ❌ Disadvantages

1. **Limited to Integer Keys**
   - ❌ Cannot use float values as array indices
   - ❌ Cannot use string values as array indices
   - Only works with integer keys in a known range

2. **Memory Wastage**
   - Must allocate space for entire key range
   - Unused indices waste memory
   - Example: For keys 1-1,000,000, need array of size 1,000,000 even if storing only 10 values

3. **Fixed Range Requirement**
   - Key universe must be known in advance
   - Not suitable for dynamic or large key ranges

### Memory Waste Example

```
Storing only 3 numbers (10, 42, 75) in range 1-100:

Index:  0    1    2    ...  10   ...   42   ...   75   ...  100
       ┌────┬────┬────┬─────┬────┬─────┬────┬─────┬────┬─────┬────┐
Value: │ F  │ F  │ F  │ ... │ T  │ ... │ T  │ ... │ T  │ ... │ F  │
       └────┴────┴────┴─────┴────┴─────┴────┴─────┴────┴─────┴────┘
        
        97 unused spaces!
        Memory Efficiency: 3/101 = 2.97%
```

---

## 🎓 When to Use Direct Access Table

### ✅ Good Use Cases
- Small, known key range (e.g., 1-100, 1-1000)
- Integer keys only
- Need guaranteed O(1) operations
- Memory is not a constraint

### ❌ Avoid When
- Key range is very large (e.g., 1-1,000,000,000)
- Keys are strings or floats
- Memory is limited
- Sparse data (few values in large range)

### Alternative: Hash Tables
For scenarios where Direct Access Table is not suitable, consider using **Hash Tables** which:
- Support string and float keys
- Use hash functions to map keys to indices
- More memory efficient for sparse data
- Still provide near O(1) operations

---

## 📊 Summary

Direct Access Table provides the **fastest possible** data structure for integer key lookups when the key range is small and known. While it trades memory efficiency for speed, it's an excellent choice for specific scenarios requiring guaranteed constant-time operations.

**Key Takeaway:** Direct Access Table is perfect when you need blazing-fast O(1) operations with integer keys in a small, known range!
