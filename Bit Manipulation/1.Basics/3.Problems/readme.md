# Comprehensive Bit Manipulation Guide

## Table of Contents
1. [Swapping Two Numbers Without a Third Variable](#1-swapping-two-numbers)
2. [Checking if the i-th Bit is Set](#2-checking-if-the-ith-bit-is-set)
3. [Setting the i-th Bit](#3-setting-the-ith-bit)
4. [Clearing the i-th Bit](#4-clearing-the-ith-bit)
5. [Toggling the i-th Bit](#5-toggling-the-ith-bit)
6. [Removing the Last Set Bit](#6-removing-the-last-set-bit)
7. [Checking if a Number is a Power of 2](#7-checking-power-of-2)
8. [Counting the Number of Set Bits](#8-counting-set-bits)

---

## 1. Swapping Two Numbers Without a Third Variable

### Concept
XOR (exclusive OR) has a special property: `A ^ A = 0` and `A ^ 0 = A`. Using this property, we can swap two numbers without needing extra memory.

### How XOR Works
```
XOR Truth Table:
A | B | A^B
0 | 0 |  0
0 | 1 |  1
1 | 0 |  1
1 | 1 |  0
```
**Key Property**: XOR returns 1 when bits are different, 0 when same.

### Detailed Example
**Given**: A = 5, B = 6

```
Initial State:
A = 5 = 0101 (binary)
B = 6 = 0110 (binary)

Step 1: A = A ^ B
A = 0101 ^ 0110 = 0011 = 3
Now: A = 3, B = 6

Step 2: B = A ^ B
B = 0011 ^ 0110 = 0101 = 5
Now: A = 3, B = 5

Step 3: A = A ^ B
A = 0011 ^ 0101 = 0110 = 6
Final: A = 6, B = 5
```

### Why It Works (Mathematical Proof)
```
After Step 1: A = A ^ B
After Step 2: B = A ^ B = (A ^ B) ^ B = A ^ (B ^ B) = A ^ 0 = A
After Step 3: A = A ^ B = (A ^ B) ^ A = (A ^ A) ^ B = 0 ^ B = B
```

### Code Implementation
```python
def swap(a, b):
    a = a ^ b  # a now holds XOR of both
    b = a ^ b  # b gets original a
    a = a ^ b  # a gets original b
    return a, b

# Usage
a, b = 5, 6
a, b = swap(a, b)
print(f"After swap: a = {a}, b = {b}")  # Output: a = 6, b = 5
```

### Multiple Examples
```
Example 1: A = 10, B = 20
Binary: A = 1010, B = 10100
After swap: A = 20, B = 10

Example 2: A = 100, B = 200
After swap: A = 200, B = 100

Example 3: A = 0, B = 5
After swap: A = 5, B = 0
```

**Complexity**: Time O(1), Space O(1)

---

## 2. Checking if the i-th Bit is Set

### Concept
Bit positions are counted from right to left, starting at 0. We need to check if the bit at position `i` is 1 or 0.

### Bit Position Reference
```
Number: 13 = 1101 (binary)
Position:    3210
Bits:        1101
```

### Method 1: Left Shift Approach

**Idea**: Create a mask with only the i-th bit set, then AND with the number.

```
Number = 13 = 1101
Check if bit at position 2 is set:

Step 1: Create mask = (1 << 2) = 0100
Step 2: Perform AND
        1101  (13)
      & 0100  (mask)
      -------
        0100  (result = 4, non-zero means bit is set)
```

**Code**:
```python
def is_set(n, i):
    return (n & (1 << i)) != 0

# Usage
print(is_set(13, 2))  # True (bit 2 is set)
print(is_set(13, 1))  # False (bit 1 is not set)
```

### Method 2: Right Shift Approach

**Idea**: Shift the number right by i positions, then check the least significant bit.

```
Number = 13 = 1101
Check if bit at position 2 is set:

Step 1: Right shift by 2 = (13 >> 2) = 0011 (3)
Step 2: AND with 1
        0011  (3)
      & 0001  (1)
      -------
        0001  (result = 1, non-zero means bit is set)
```

**Code**:
```python
def is_set(n, i):
    return ((n >> i) & 1) != 0

# Usage
print(is_set(13, 2))  # True
print(is_set(13, 1))  # False
```

### More Examples
```
Number = 20 = 10100
Position:     43210

Check bit 2: (20 & (1 << 2)) = 10100 & 00100 = 00100 ✓ (SET)
Check bit 1: (20 & (1 << 1)) = 10100 & 00010 = 00000 ✗ (NOT SET)
Check bit 4: (20 & (1 << 4)) = 10100 & 10000 = 10000 ✓ (SET)
```

**Complexity**: Time O(1), Space O(1)

---

## 3. Setting the i-th Bit

### Concept
Setting a bit means forcing it to become 1, regardless of its current value. We use the OR operator.

### OR Operator Properties
```
OR Truth Table:
A | B | A|B
0 | 0 | 0
0 | 1 | 1
1 | 0 | 1
1 | 1 | 1
```
**Key**: `A | 1 = 1` (always becomes 1)

### Detailed Example
**Set bit 2 in number 9**

```
Number = 9 = 1001
Target position = 2

Step 1: Create mask = (1 << 2) = 0100
Step 2: OR operation
        1001  (9)
      | 0100  (mask)
      -------
        1101  (13)

Result: 13
```

### Visual Walkthrough
```
Before: 1001 (bit 2 is 0)
        ↓
Mask:   0100 (only bit 2 is 1)
        ↓
After:  1101 (bit 2 is now 1)
```

### Code Implementation
```python
def set_bit(n, i):
    return n | (1 << i)

# Usage
print(set_bit(9, 2))   # Output: 13
print(set_bit(8, 0))   # Output: 9
print(set_bit(0, 3))   # Output: 8
```

### More Examples
```
Example 1: Set bit 0 in 8 (1000)
1000 | 0001 = 1001 = 9

Example 2: Set bit 3 in 0 (0000)
0000 | 1000 = 1000 = 8

Example 3: Set bit 1 in 13 (1101) - already set
1101 | 0010 = 1111 = 15
```

**Note**: If the bit is already set, it remains 1.

**Complexity**: Time O(1), Space O(1)

---

## 4. Clearing the i-th Bit

### Concept
Clearing a bit means forcing it to become 0. We use AND with the complement of the mask.

### NOT Operator
```
NOT (~) inverts all bits:
~0100 = 1011 (in 4-bit representation)
```

### Detailed Example
**Clear bit 2 in number 13**

```
Number = 13 = 1101
Target position = 2

Step 1: Create mask = (1 << 2) = 0100
Step 2: Complement mask = ~(0100) = 1011
Step 3: AND operation
        1101  (13)
      & 1011  (~mask)
      -------
        1001  (9)

Result: 9
```

### Why This Works
```
Original number:    1101
Mask:               0100 (bit we want to clear)
Complement (~):     1011 (all bits 1 except position 2)
AND operation:      1101 & 1011 = 1001
                    ↑
                    This bit becomes 0, others unchanged
```

### Code Implementation
```python
def clear_bit(n, i):
    return n & ~(1 << i)

# Usage
print(clear_bit(13, 2))  # Output: 9
print(clear_bit(15, 0))  # Output: 14
print(clear_bit(12, 3))  # Output: 4
```

### More Examples
```
Example 1: Clear bit 0 in 15 (1111)
1111 & ~0001 = 1111 & 1110 = 1110 = 14

Example 2: Clear bit 3 in 12 (1100)
1100 & ~1000 = 1100 & 0111 = 0100 = 4

Example 3: Clear bit 1 in 8 (1000) - already clear
1000 & ~0010 = 1000 & 1101 = 1000 = 8
```

**Complexity**: Time O(1), Space O(1)

---

## 5. Toggling the i-th Bit

### Concept
Toggling means flipping a bit: 0→1 or 1→0. We use XOR operator.

### XOR for Toggling
```
XOR with 1 flips the bit:
0 ^ 1 = 1 (flip 0 to 1)
1 ^ 1 = 0 (flip 1 to 0)
```

### Detailed Example
**Toggle bit 1 in number 13**

```
Number = 13 = 1101
Target position = 1

Step 1: Create mask = (1 << 1) = 0010
Step 2: XOR operation
        1101  (13)
      ^ 0010  (mask)
      -------
        1111  (15)

Result: 15
```

### Multiple Toggle Examples
```
Example 1: Toggle bit 2 in 9 (1001)
1001 ^ 0100 = 1101 = 13 (0→1)

Example 2: Toggle bit 2 in 13 (1101)
1101 ^ 0100 = 1001 = 9 (1→0)

Example 3: Toggle bit 0 in 10 (1010)
1010 ^ 0001 = 1011 = 11 (0→1)

Example 4: Toggle bit 3 in 15 (1111)
1111 ^ 1000 = 0111 = 7 (1→0)
```

### Code Implementation
```python
def toggle_bit(n, i):
    return n ^ (1 << i)

# Usage
print(toggle_bit(9, 2))   # Output: 13 (0→1)
print(toggle_bit(13, 2))  # Output: 9  (1→0)
print(toggle_bit(10, 0))  # Output: 11 (0→1)
print(toggle_bit(15, 3))  # Output: 7  (1→0)
```

**Use Case**: Useful for switching states, enabling/disabling flags.

**Complexity**: Time O(1), Space O(1)

---

## 6. Removing the Last Set Bit

### Concept
The "last set bit" is the rightmost 1 in the binary representation. The operation `n & (n-1)` removes it.

### Why n & (n-1) Works

When you subtract 1 from a number:
- All bits after the rightmost set bit are flipped
- The rightmost set bit becomes 0
- All bits before remain the same

```
Example: n = 12 = 1100
         n-1 = 11 = 1011

Notice: 
- Rightmost set bit (position 2) becomes 0
- Bits after it (positions 0,1) flip from 00 to 11
```

### Detailed Example
**Remove last set bit from 13**

```
Number = 13 = 1101

Step 1: Calculate n-1
13 - 1 = 12 = 1100

Step 2: AND operation
        1101  (13)
      & 1100  (12)
      -------
        1100  (12)

The rightmost 1 (at position 0) is removed!
```

### Visual Breakdown
```
n = 1101 (13)
    ^^^┗━ rightmost set bit at position 0

n-1 = 1100 (12)
      ││└─ became 0
      ││
      └┴─ everything before stays same

n & (n-1) = 1100 (12)
```

### More Examples
```
Example 1: n = 8 = 1000
n-1 = 7 = 0111
n & (n-1) = 1000 & 0111 = 0000 = 0

Example 2: n = 20 = 10100
n-1 = 19 = 10011
n & (n-1) = 10100 & 10011 = 10000 = 16

Example 3: n = 7 = 0111
n-1 = 6 = 0110
n & (n-1) = 0111 & 0110 = 0110 = 6
```

### Code Implementation
```python
def remove_last_set_bit(n):
    return n & (n - 1)

# Usage
print(remove_last_set_bit(13))  # Output: 12
print(remove_last_set_bit(8))   # Output: 0
print(remove_last_set_bit(20))  # Output: 16
print(remove_last_set_bit(7))   # Output: 6
```

**Use Case**: Used in counting set bits, checking power of 2.

**Complexity**: Time O(1), Space O(1)

---

## 7. Checking if a Number is a Power of 2

### Concept
A power of 2 has exactly one set bit in binary representation. Using `n & (n-1)`, we can check this efficiently.

### Powers of 2 in Binary
```
1  = 2^0 = 0001 (1 set bit)
2  = 2^1 = 0010 (1 set bit)
4  = 2^2 = 0100 (1 set bit)
8  = 2^3 = 1000 (1 set bit)
16 = 2^4 = 10000 (1 set bit)

Non-powers:
3  = 0011 (2 set bits)
5  = 0101 (2 set bits)
6  = 0110 (2 set bits)
7  = 0111 (3 set bits)
```

### Why n & (n-1) = 0 Works

For powers of 2:
```
n = 8 = 1000 (only 1 set bit)
n-1 = 7 = 0111 (all bits after set bit become 1)
n & (n-1) = 1000 & 0111 = 0000 = 0 ✓
```

For non-powers:
```
n = 6 = 0110 (2 set bits)
n-1 = 5 = 0101
n & (n-1) = 0110 & 0101 = 0100 ≠ 0 ✗
```

### Detailed Examples

**Example 1: Check if 16 is power of 2**
```
16 = 10000
15 = 01111
16 & 15 = 10000 & 01111 = 00000 = 0 ✓
```

**Example 2: Check if 18 is power of 2**
```
18 = 10010
17 = 10001
18 & 17 = 10010 & 10001 = 10000 ≠ 0 ✗
```

**Example 3: Check if 32 is power of 2**
```
32 = 100000
31 = 011111
32 & 31 = 100000 & 011111 = 000000 = 0 ✓
```

### Code Implementation
```python
def is_power_of_two(n):
    return n > 0 and (n & (n - 1)) == 0

# Usage
print(is_power_of_two(1))    # True  (2^0)
print(is_power_of_two(2))    # True  (2^1)
print(is_power_of_two(16))   # True  (2^4)
print(is_power_of_two(3))    # False
print(is_power_of_two(6))    # False
print(is_power_of_two(0))    # False
print(is_power_of_two(-4))   # False
```

**Complexity**: Time O(1), Space O(1)

---

## 8. Counting the Number of Set Bits

### Method 1: Loop and Bitwise AND (Basic Approach)

### Concept
Check each bit one by one using `n & 1`, then right shift to check the next bit.

### How It Works
```
Number = 13 = 1101

Iteration 1: 
1101 & 0001 = 0001 = 1 (bit is set, count++)
Shift right: 1101 >> 1 = 0110 = 6

Iteration 2:
0110 & 0001 = 0000 = 0 (bit not set)
Shift right: 0110 >> 1 = 0011 = 3

Iteration 3:
0011 & 0001 = 0001 = 1 (bit is set, count++)
Shift right: 0011 >> 1 = 0001 = 1

Iteration 4:
0001 & 0001 = 0001 = 1 (bit is set, count++)
Shift right: 0001 >> 1 = 0000 = 0

Total count = 3
```

### Code Implementation
```python
def count_set_bits(n):
    count = 0
    while n > 0:
        count += (n & 1)  # Add 1 if last bit is set
        n >>= 1           # Right shift by 1
    return count

# Usage
print(count_set_bits(13))  # Output: 3
print(count_set_bits(10))  # Output: 2
print(count_set_bits(7))   # Output: 3
```

### Step-by-Step Example for n = 10
```
10 = 1010

Step 1: 1010 & 1 = 0 → count = 0, n = 0101 (5)
Step 2: 0101 & 1 = 1 → count = 1, n = 0010 (2)
Step 3: 0010 & 1 = 0 → count = 1, n = 0001 (1)
Step 4: 0001 & 1 = 1 → count = 2, n = 0000 (0)

Result: 2 set bits
```

**Complexity**: Time O(log n), Space O(1)
- We process each bit, and there are log₂(n) bits

---

### Method 2: Optimized Using n & (n-1) (Brian Kernighan's Algorithm)

### Concept
Instead of checking every bit, we only process the set bits by repeatedly removing the rightmost set bit.

### Why It's Faster
- Basic method: Processes ALL bits (including 0s)
- Optimized method: Only processes SET bits (1s)

Example: For 1000000 (64), basic method does 7 iterations, optimized does only 1!

### How It Works
```
Number = 13 = 1101

Iteration 1:
n = 1101 (13)
n & (n-1) = 1101 & 1100 = 1100 (12)
count = 1 ✓

Iteration 2:
n = 1100 (12)
n & (n-1) = 1100 & 1011 = 1000 (8)
count = 2 ✓

Iteration 3:
n = 1000 (8)
n & (n-1) = 1000 & 0111 = 0000 (0)
count = 3 ✓

n = 0, stop
Result: 3 set bits
```

### Code Implementation
```python
def count_set_bits_optimized(n):
    count = 0
    while n > 0:
        n = n & (n - 1)  # Remove rightmost set bit
        count += 1
    return count

# Usage
print(count_set_bits_optimized(13))  # Output: 3
print(count_set_bits_optimized(20))  # Output: 2
print(count_set_bits_optimized(7))   # Output: 3
print(count_set_bits_optimized(15))  # Output: 4
print(count_set_bits_optimized(16))  # Output: 1
```

### Comparison Example: n = 20 (10100)

**Basic Method** (5 iterations):
```
10100 & 1 = 0, shift → 01010
01010 & 1 = 0, shift → 00101
00101 & 1 = 1, count=1, shift → 00010
00010 & 1 = 0, shift → 00001
00001 & 1 = 1, count=2, shift → 00000
```

**Optimized Method** (2 iterations):
```
10100 & 10011 = 10000, count=1
10000 & 01111 = 00000, count=2
```

### More Examples
```
Example 1: n = 7 (0111) - 3 set bits
Iteration 1: 0111 & 0110 = 0110
Iteration 2: 0110 & 0101 = 0100
Iteration 3: 0100 & 0011 = 0000
Count = 3

Example 2: n = 15 (1111) - 4 set bits
Iteration 1: 1111 & 1110 = 1110
Iteration 2: 1110 & 1101 = 1100
Iteration 3: 1100 & 1011 = 1000
Iteration 4: 1000 & 0111 = 0000
Count = 4

Example 3: n = 16 (10000) - 1 set bit
Iteration 1: 10000 & 01111 = 00000
Count = 1
```

**Complexity**: Time O(k) where k = number of set bits, Space O(1)
- Best case: O(1) for numbers with 1 set bit
- Worst case: O(log n) for numbers with all bits set

---

## Summary Comparison Table

| Operation | Operator Used | Time | Space | Use Case |
|-----------|--------------|------|-------|----------|
| Swap | XOR (^) | O(1) | O(1) | Memory-efficient swapping |
| Check bit | AND (&) + Shift | O(1) | O(1) | Checking flags/status |
| Set bit | OR (\|) | O(1) | O(1) | Setting flags/permissions |
| Clear bit | AND (&) + NOT (~) | O(1) | O(1) | Clearing flags |
| Toggle bit | XOR (^) | O(1) | O(1) | Switching states |
| Remove last bit | AND (&) | O(1) | O(1) | Optimization tricks |
| Power of 2 | AND (&) | O(1) | O(1) | Validation checks |
| Count bits (basic) | AND (&) + Shift | O(log n) | O(1) | Simple counting |
| Count bits (optimized) | AND (&) | O(k) | O(1) | Efficient counting |

---

## Quick Reference: Bitwise Operators

```
&  (AND)     → Both bits must be 1
|  (OR)      → At least one bit must be 1
^  (XOR)     → Bits must be different
~  (NOT)     → Inverts all bits
<< (Left shift)  → Multiply by 2^n
>> (Right shift) → Divide by 2^n
```

## Common Patterns to Remember

1. **Mask creation**: `1 << i` creates a mask with only i-th bit set
2. **Check bit**: `n & (1 << i)` or `(n >> i) & 1`
3. **Set bit**: `n | (1 << i)`
4. **Clear bit**: `n & ~(1 << i)`
5. **Toggle bit**: `n ^ (1 << i)`
6. **Remove last bit**: `n & (n - 1)`
7. **Isolate last bit**: `n & (-n)`
8. **Check power of 2**: `n > 0 && (n & (n-1)) == 0`
