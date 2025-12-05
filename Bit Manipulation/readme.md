
# Bit Manipulation - Complete Index

## 📑 Complete Problem Catalog (15 Problems)

---

## 🔹 **BASIC BIT OPERATIONS**
*Fundamental bit manipulation techniques*

### **Subtype: Core Bit Operations**
1. Swapping Two Numbers Without a Third Variable
2. Checking if the i-th Bit is Set
3. Setting the i-th Bit
4. Clearing the i-th Bit
5. Toggling the i-th Bit
6. Removing the Last Set Bit
7. Checking if a Number is a Power of 2
8. Counting the Number of Set Bits

---

## 🔹 **ADVANCED BIT MANIPULATION**
*Problem-solving using bit tricks*

### **Subtype: XOR Properties**
9. Minimum Bit Flips to Convert Number
10. Single Number - I
11. Single Number - II
12. Single Number - III

### **Subtype: Arithmetic & Generation**
13. Divide Two Numbers
14. Power Set using Bit Manipulation
15. XOR of Numbers in a Range

---

## 📊 **SUMMARY BY CATEGORY**

### **🟢 Basic Bit Operations** (8 problems)
→ Problems: 1-8

### **🟡 Advanced Bit Manipulation** (7 problems)
→ Problems: 9-15

---

## 📈 **LEARNING PATH RECOMMENDATION**

### **Phase 1: Bit Basics** (Start Here)
```
2 → 3 → 4 → 5 → 6 → 7 → 8
```
*Master fundamental bit operations*

### **Phase 2: Bit Tricks**
```
1 → 9
```
*Learn XOR for swapping and flipping*

### **Phase 3: XOR Pattern**
```
10 → 11 → 12
```
*Single number variants using XOR properties*

### **Phase 4: Advanced Applications**
```
13 → 14 → 15
```
*Complex bit manipulation problems*

---

## 🎯 **CONCEPT CLUSTERING**

### **Cluster 1: Bit Position Operations**
*Manipulating specific bits*
- Check/Set/Clear/Toggle: 2, 3, 4, 5
- Remove Last Set: 6

### **Cluster 2: Bit Counting & Properties**
*Analyzing bit patterns*
- Count Set Bits: 8
- Power of 2: 7

### **Cluster 3: XOR Magic**
*Using XOR properties*
- Swapping: 1
- Finding Unique: 10, 11, 12
- Bit Flips: 9
- Range XOR: 15

### **Cluster 4: Arithmetic with Bits**
*Mathematical operations*
- Division: 13
- Generation: 14

---

## 💡 **PROBLEM PAIRING** *(Similar Concepts)*

**Pair 1:** Check i-th Bit (2) ↔ Set i-th Bit (3) ↔ Clear i-th Bit (4) ↔ Toggle i-th Bit (5)

**Pair 2:** Single Number I (10) ↔ Single Number II (11) ↔ Single Number III (12)

**Pair 3:** Power of 2 (7) ↔ Remove Last Set Bit (6)

**Pair 4:** Count Set Bits (8) ↔ Bit Flips (9)

---

## 🔍 **BY DIFFICULTY LEVEL**

### **🟢 Easy** (Beginner-Friendly)
- Problems: 1, 2, 3, 4, 5, 6, 7, 8, 10

### **🟡 Medium** (Core Concepts)
- Problems: 9, 11, 13, 14, 15

### **🔴 Hard** (Advanced Techniques)
- Problems: 12

---

## 🎓 **TECHNIQUE-WISE GROUPING**

### **AND Operations (&)**
→ Problems: 2, 4, 6, 7, 8

### **OR Operations (|)**
→ Problems: 3

### **XOR Operations (^)**
→ Problems: 1, 9, 10, 11, 12, 15

### **Left Shift (<<)**
→ Problems: 3, 7, 13, 14

### **Right Shift (>>)**
→ Problems: 8, 13

### **NOT Operation (~)**
→ Problems: 4, 5

---

## 📚 **PATTERN RECOGNITION GUIDE**

### **When you see: "Swap without extra space..."**
→ Use: XOR swap (Problem 1)

### **When you see: "Check/Set/Clear/Toggle i-th bit..."**
→ Use: Bit masking (Problems 2, 3, 4, 5)

### **When you see: "Is power of 2..."**
→ Use: n & (n-1) == 0 (Problem 7)

### **When you see: "Count 1's in binary..."**
→ Use: Brian Kernighan's Algorithm (Problem 8)

### **When you see: "Find unique element (all others appear k times)..."**
→ Use: XOR properties (Problems 10, 11, 12)

### **When you see: "Generate all subsets..."**
→ Use: Bitmask iteration (Problem 14)

### **When you see: "Divide without operator..."**
→ Use: Bit shifts and subtraction (Problem 13)

### **When you see: "XOR in range..."**
→ Use: XOR prefix property (Problem 15)

---

## 🏆 **MILESTONE PROBLEMS**
*Master these to understand core patterns*

1. **Check i-th Bit (2)** - Foundation of bit operations
2. **Count Set Bits (8)** - Brian Kernighan's algorithm
3. **Power of 2 (7)** - Classic bit trick
4. **Single Number I (10)** - XOR cancellation property
5. **Single Number II (11)** - Bit counting modulo
6. **Power Set (14)** - Bitmask for generation
7. **Swap Without Variable (1)** - XOR swap trick
8. **Divide Two Numbers (13)** - Bit shift arithmetic

---

## 🗺️ **PROBLEM BREAKDOWN BY TYPE**

### **Bit Position Manipulation** (4 problems)
→ 2, 3, 4, 5

### **Bit Analysis** (3 problems)
→ 6, 7, 8

### **XOR Applications** (5 problems)
→ 1, 9, 10, 12, 15

### **Bit Counting Problems** (2 problems)
→ 8, 11

### **Generation & Arithmetic** (2 problems)
→ 13, 14

---

## 📊 **TOTAL STATISTICS**

- **Total Problems:** 15
- **Basic Bit Operations:** 8 problems (53%)
- **Advanced Bit Manipulation:** 7 problems (47%)
- **Easy:** 9 problems (60%)
- **Medium:** 5 problems (33%)
- **Hard:** 1 problem (7%)

---

## 🔄 **PREREQUISITE RELATIONSHIPS**

### **Master First (Foundation):**
2, 3, 4, 5, 6 → Basic bit operations

### **Then Learn (Properties):**
7, 8 → Power of 2 and counting

### **Build Upon (XOR):**
1, 9, 10 → XOR properties and applications

### **Advanced Techniques:**
11, 12, 13, 14, 15 → Complex bit manipulation

---

## 🎯 **COMMON PATTERNS & TEMPLATES**

### **Template 1: Check i-th Bit**
```python
def checkBit(n, i):
    return (n & (1 << i)) != 0
```
→ Used in: 2, 14

### **Template 2: Set i-th Bit**
```python
def setBit(n, i):
    return n | (1 << i)
```
→ Used in: 3

### **Template 3: Clear i-th Bit**
```python
def clearBit(n, i):
    return n & ~(1 << i)
```
→ Used in: 4

### **Template 4: Toggle i-th Bit**
```python
def toggleBit(n, i):
    return n ^ (1 << i)
```
→ Used in: 5

### **Template 5: Remove Last Set Bit**
```python
def removeLastSetBit(n):
    return n & (n - 1)
```
→ Used in: 6, 7, 8

### **Template 6: Check Power of 2**
```python
def isPowerOfTwo(n):
    return n > 0 and (n & (n - 1)) == 0
```
→ Used in: 7

### **Template 7: Count Set Bits (Brian Kernighan)**
```python
def countSetBits(n):
    count = 0
    while n:
        n &= (n - 1)  # Remove last set bit
        count += 1
    return count
```
→ Used in: 8, 9

### **Template 8: XOR Swap**
```python
def swap(a, b):
    a ^= b
    b ^= a
    a ^= b
    return a, b
```
→ Used in: 1

### **Template 9: Single Number (XOR)**
```python
def singleNumber(nums):
    result = 0
    for num in nums:
        result ^= num
    return result
```
→ Used in: 10

### **Template 10: Generate Subsets**
```python
def subsets(nums):
    n = len(nums)
    result = []
    for mask in range(1 << n):  # 2^n subsets
        subset = []
        for i in range(n):
            if mask & (1 << i):
                subset.append(nums[i])
        result.append(subset)
    return result
```
→ Used in: 14

---

## 🌟 **KEY BIT MANIPULATION FACTS**

### **XOR Properties:**
```
a ^ a = 0          (Self-cancellation)
a ^ 0 = a          (Identity)
a ^ b ^ a = b      (Commutative & Associative)
```

### **Power of 2 Check:**
```
n & (n-1) == 0     (Only works if n > 0)
Example: 8 = 1000, 7 = 0111
         8 & 7 = 0000
```

### **Remove Last Set Bit:**
```
n & (n-1)
Example: 12 = 1100, 11 = 1011
         12 & 11 = 1000 (removed rightmost 1)
```

### **Get Rightmost Set Bit:**
```
n & -n or n & (~n + 1)
Example: 12 = 1100
         -12 = ...10100 (2's complement)
         12 & -12 = 0100 (isolated rightmost 1)
```

### **Set All Bits:**
```
~0 = all bits set to 1
```

### **Count Set Bits (Built-in):**
```python
bin(n).count('1')  # Python
__builtin_popcount(n)  # C++
Integer.bitCount(n)  # Java
```

---

## 🎯 **BIT MANIPULATION TRICKS**

### **1. Multiply by 2^k:**
```
n << k  (Left shift by k)
Example: 5 << 2 = 20 (5 * 4)
```

### **2. Divide by 2^k:**
```
n >> k  (Right shift by k)
Example: 20 >> 2 = 5 (20 / 4)
```

### **3. Check if Odd/Even:**
```
n & 1 == 1  (Odd)
n & 1 == 0  (Even)
```

### **4. Toggle All Bits:**
```
~n
```

### **5. Get Lowest k Bits:**
```
n & ((1 << k) - 1)
Example: Get lowest 3 bits of 13 (1101)
         13 & 7 = 13 & 0111 = 0101 = 5
```

### **6. Check if Two Numbers Have Opposite Signs:**
```
(a ^ b) < 0
```

### **7. Absolute Value without Branching:**
```
mask = n >> 31  (or 63 for 64-bit)
abs_n = (n ^ mask) - mask
```

---

## 🎯 **QUICK REFERENCE BY NUMBER**

**1-8:** Basic Bit Operations & Tricks  
**9-15:** Advanced Problems (XOR, Arithmetic, Generation)

---

## ✅ **COMPLETENESS STATUS**

### **Coverage: 85% COMPLETE** ✓

**What You Have:**
- ✅ All fundamental bit operations
- ✅ XOR properties and applications
- ✅ Single number variants (all 3)
- ✅ Power of 2 check
- ✅ Bit counting
- ✅ Power set generation
- ✅ Arithmetic with bits

**Minor Missing Topics (15%):**
- ⭕ **Gray Code** (1 problem)
- ⭕ **Reverse Bits** (1 problem)
- ⭕ **Hamming Distance** (1 problem)
- ⭕ **Total Hamming Distance** (1 problem)
- ⭕ **Maximum XOR of Two Numbers** (1 problem)
- ⭕ **Bitwise AND of Numbers Range** (1 problem)

**Verdict:** Excellent coverage of core bit manipulation! All essentials included! 🎉

---

## 🎓 **INTERVIEW FREQUENCY**

### **Very High Frequency** ⭐⭐⭐
→ 7, 8, 10

### **High Frequency** ⭐⭐
→ 2, 3, 4, 5, 9, 11, 14

### **Medium Frequency** ⭐
→ 1, 6, 12, 13, 15

### **Low Frequency**
→ (None in this list)

Focus on **Power of 2 (7)**, **Count Set Bits (8)**, and **Single Number I (10)** - these are asked very frequently in interviews!

---

## 💡 **PRO TIPS**

1. **Always think in binary** - Visualize numbers as bit patterns
2. **XOR is your friend** - Most unique element problems use XOR
3. **n & (n-1) is magic** - Removes rightmost set bit (many applications)
4. **Bit shifts = multiplication/division by powers of 2** - Very efficient
5. **Use bitmask for subset generation** - Cleaner than recursion
6. **Practice bit manipulation mentally** - Should become second nature
