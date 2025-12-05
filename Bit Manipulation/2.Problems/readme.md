# Bit Manipulation Problems - Revision Notes

## Table of Contents
1. [Minimum Bit Flips to Convert Number](#1-minimum-bit-flips-to-convert-number)
2. [Single Number - I](#2-single-number---i)
3. [Single Number - II](#3-single-number---ii)
4. [Single Number - III](#4-single-number---iii)
5. [Divide Two Numbers](#5-divide-two-numbers)
6. [Power Set using Bit Manipulation](#6-power-set-using-bit-manipulation)
7. [XOR of Numbers in a Range](#7-xor-of-numbers-in-a-range)

---

## 1. Minimum Bit Flips to Convert Number

### Problem Description
Given two integers `start` and `goal`, find the minimum number of bit flips needed to convert start into goal. A bit flip changes a bit from 0 to 1 or 1 to 0.

### Examples

#### Example 1
**Input:** start = 10, goal = 7  
**Output:** 3  
**Explanation:**
- 10 in binary: `1010`
- 7 in binary:  `0111`
- Differences at positions: 0, 2, 3 → 3 flips needed

#### Example 2
**Input:** start = 3, goal = 4  
**Output:** 3  
**Explanation:**
- 3 in binary: `011`
- 4 in binary: `100`
- All 3 bits are different → 3 flips

### Solution

#### Intuition
When we XOR two numbers, the result has 1s only at positions where the bits differ:
- `1 ^ 1 = 0` (same bits → no flip needed)
- `0 ^ 0 = 0` (same bits → no flip needed)
- `1 ^ 0 = 1` (different → flip needed)
- `0 ^ 1 = 1` (different → flip needed)

So XOR gives us all positions that need flipping! Count the 1s in XOR result = minimum flips.

#### Visual Example
```
start = 10:  1 0 1 0
goal  = 7:   0 1 1 1
           -----------
XOR result:  1 1 0 1  → 3 ones → 3 flips needed
             ↑ ↑   ↑
          positions that differ
```

#### Approach Steps
1. Perform XOR: `num = start ^ goal`
2. Count number of set bits (1s) in `num`
3. Return the count

**Three Methods to Count Set Bits:**

**Method 1 - Division:** Keep dividing by 2, check remainder
**Method 2 - Bit Shift:** Right shift, check LSB using `& 1`
**Method 3 - Brian Kernighan:** `n & (n-1)` removes rightmost set bit (fastest!)

#### Code

```python
# Method 1: Using division
def countOfSetBits(num):
    cnt = 0
    while num > 0:
        rem = num % 2
        if rem == 1:
            cnt += 1
        num = num // 2
    return cnt

# Method 2: Using bit operations
def countOfSetBits2(num):
    cnt = 0
    while num > 0:
        rem = num & 1  # Check last bit
        if rem == 1:
            cnt += 1
        num = num >> 1  # Right shift
    return cnt

# Method 3: Brian Kernighan's Algorithm (Optimal)
def countOfSetBits3(num):
    cnt = 0
    while num > 0:
        cnt += 1
        num = num & (num - 1)  # Remove rightmost set bit
    return cnt

def solve(start, goal):
    # XOR to find differing bits
    num = start ^ goal
    
    # Count set bits
    cnt = countOfSetBits3(num)
    
    return cnt
```

#### Time and Space Complexity
- **Time Complexity:** O(log n) - Where n is max(start, goal), iterate through bits
- **Space Complexity:** O(1) - Only using variables

---

## 2. Single Number - I

### Problem Description
Given an array where every element appears twice except one, find the element that appears only once.

### Examples

#### Example 1
**Input:** nums = [1, 2, 2, 4, 3, 1, 4]  
**Output:** 3

#### Example 2
**Input:** nums = [5]  
**Output:** 5

### Solution

### Approach 1: Hashing

#### Intuition
Count frequency of each number using a dictionary. The number with frequency 1 is our answer.

#### Approach Steps
1. Create empty frequency dictionary
2. Traverse array and count occurrences of each element
3. Iterate through dictionary and find element with count = 1
4. Return that element

#### Code
```python
def solve(n, arr):
    count = {}
    
    # Count frequencies
    for i in range(n):
        if arr[i] in count:
            count[arr[i]] += 1
        else:
            count[arr[i]] = 1
    
    # Find element with frequency 1
    for i, v in count.items():
        if v == 1:
            return i
    
    return -1
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single pass through array + dictionary iteration
- **Space Complexity:** O(n) - Dictionary stores up to n elements

---

### Approach 2: XOR (Optimal)

#### Intuition
XOR has magical properties for this problem:
- `a ^ a = 0` (same numbers cancel out)
- `a ^ 0 = a` (XOR with 0 gives the number itself)
- XOR is commutative: `a ^ b ^ c = a ^ c ^ b`

When we XOR all elements, pairs cancel out to 0, leaving only the single element!

#### Visual Example
```
Array: [1, 2, 2, 4, 3, 1, 4]

XOR all:
  1 ^ 2 ^ 2 ^ 4 ^ 3 ^ 1 ^ 4

Rearrange (commutative):
= (1 ^ 1) ^ (2 ^ 2) ^ (4 ^ 4) ^ 3
=    0    ^    0    ^    0    ^ 3
= 3 ✓
```

#### Approach Steps
1. Initialize `xor = 0`
2. Traverse array and XOR each element with xor
3. Return the final xor value

#### Code
```python
def solve(n, arr):
    xor = 0
    
    # XOR all elements
    for i in range(n):
        xor = xor ^ arr[i]
    
    return xor
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single pass through array
- **Space Complexity:** O(1) - Only one variable used

---

## 3. Single Number - II

### Problem Description
Given an array where every element appears three times except one, find the element that appears only once.

### Examples

#### Example 1
**Input:** nums = [2, 2, 2, 3]  
**Output:** 3

#### Example 2
**Input:** nums = [1, 0, 3, 0, 1, 1, 3, 3, 10, 0]  
**Output:** 10

### Solution

### Approach 1: Hashing

#### Intuition
Count frequency of each element. Return the element with count 1.

#### Approach Steps
1. Create frequency dictionary
2. Count occurrences
3. Return element with frequency 1

#### Code
```python
def solve(n, arr):
    count = {}
    for i in range(n):
        if arr[i] in count:
            count[arr[i]] += 1
        else:
            count[arr[i]] = 1
    
    for i, v in count.items():
        if v == 1:
            return i
    return -1
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single pass + dictionary iteration
- **Space Complexity:** O(n) - Dictionary storage

---

### Approach 2: Sorting

#### Intuition
After sorting, elements appearing three times will be grouped together. Check for the element that doesn't fit this pattern.

#### Approach Steps
1. Sort the array
2. Handle edge case: single element array
3. Check first element separately
4. Check last element separately
5. For middle elements, check if different from both neighbors
6. Return the element that doesn't match its neighbors

#### Code
```python
def solve(n, arr):
    arr.sort()
    
    if n == 1:
        return arr[0]
    
    # Check first element
    if arr[0] != arr[1]:
        return arr[0]
    
    # Check last element
    if arr[n-1] != arr[n-2]:
        return arr[n-1]
    
    # Check middle elements
    for i in range(1, n-1):
        if arr[i] != arr[i-1] and arr[i] != arr[i+1]:
            return arr[i]
    
    return -1
```

#### Time and Space Complexity
- **Time Complexity:** O(n log n) - Sorting dominates
- **Space Complexity:** O(1) - In-place sorting (or O(n) depending on sort implementation)

---

### Approach 3: Bit Manipulation - Array Method

#### Intuition
Count how many times each bit position is set (has value 1) across all numbers. Since numbers appear 3 times, bit counts will be multiples of 3, except for the single number's bits. If `count[bit] % 3 != 0`, the single number has that bit set.

#### Visual Example
```
Array: [2, 2, 2, 3]
Binary:
  2 → 0 1 0
  2 → 0 1 0
  2 → 0 1 0
  3 → 0 1 1
      -----
Count: 0 4 1

Bit 0: count = 1, 1 % 3 = 1 → set in answer
Bit 1: count = 4, 4 % 3 = 1 → set in answer
Bit 2: count = 0, 0 % 3 = 0 → not set

Answer: 011 (binary) = 3 ✓
```

#### Approach Steps
1. Create count array of size 32 (for 32-bit integers)
2. For each number in array:
   - Check each bit position (0 to 31)
   - If bit is set, increment count[bitIndex]
3. Build answer:
   - For each bit position, if count % 3 == 1, set that bit in answer
4. Handle negative numbers using two's complement

#### Code
```python
def solve(n, arr):
    count = [0] * 32
    
    # Count set bits at each position
    for i in range(n):
        for bitIndex in range(32):
            check = arr[i] & (1 << bitIndex)
            if check:
                count[bitIndex] += 1
    
    ans = 0
    for bitIndex in range(32):
        check = count[bitIndex] % 3
        if check:
            ans += (2 ** bitIndex)
    
    # Handle negative numbers
    if ans >= 2**31:
        ans -= 2**32
    
    return ans
```

#### Time and Space Complexity
- **Time Complexity:** O(32n) = O(n) - For each of n numbers, check 32 bits
- **Space Complexity:** O(32) = O(1) - Fixed size count array

---

### Approach 4: Bit Manipulation - Optimized

#### Intuition
Instead of storing counts in an array, count on-the-fly for each bit position. This saves space.

#### Approach Steps
1. For each bit position (0 to 31):
   - Count how many numbers have this bit set
   - If count % 3 == 1, set this bit in answer
2. Handle negative numbers

#### Code
```python
def solve(n, arr):
    ans = 0
    
    for bitIndex in range(32):
        count = 0
        # Count this bit across all numbers
        for i in range(n):
            check = arr[i] & (1 << bitIndex)
            if check:
                count += 1
        
        # If count % 3 == 1, set this bit
        if count % 3 == 1:
            ans += 1 << bitIndex
    
    # Handle negative numbers
    if ans >= 2**31:
        ans -= 2**32
    
    return ans
```

#### Time and Space Complexity
- **Time Complexity:** O(32n) = O(n) - For each of 32 bits, check n numbers
- **Space Complexity:** O(1) - No extra arrays

---

### Approach 5: Ones and Twos Method (Most Elegant)

#### Intuition
Use two variables `ones` and `twos` to track bits seen once and twice:
- `ones` stores bits that have appeared 1 or 4 or 7... times (1 mod 3)
- `twos` stores bits that have appeared 2 or 5 or 8... times (2 mod 3)
- When a bit appears 3 times, remove it from both

Think of it like a counter that resets every 3 occurrences!

#### Visual Example
```
Array: [2, 2, 2, 3]

Initially: ones = 0, twos = 0

Process 2 (010):
  ones = (0 ^ 010) & ~0 = 010
  twos = (0 ^ 010) & ~010 = 0

Process 2 again:
  ones = (010 ^ 010) & ~0 = 0
  twos = (0 ^ 010) & ~0 = 010

Process 2 third time:
  ones = (0 ^ 010) & ~010 = 0
  twos = (010 ^ 010) & ~0 = 0
  (Both reset after 3 occurrences!)

Process 3 (011):
  ones = (0 ^ 011) & ~0 = 011
  twos = (0 ^ 011) & ~011 = 0

Answer: ones = 011 = 3 ✓
```

#### Approach Steps
1. Initialize `ones = 0`, `twos = 0`
2. For each number:
   - Update ones: `ones = (ones ^ num) & (~twos)`
   - Update twos: `twos = (twos ^ num) & (~ones)`
3. Return `ones`

#### Code
```python
def solve(n, arr):
    ones, twos = 0, 0
    
    for i in range(n):
        ones = (ones ^ arr[i]) & (~twos)
        twos = (twos ^ arr[i]) & (~ones)
    
    return ones
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single pass through array
- **Space Complexity:** O(1) - Only two variables

---

## 4. Single Number - III

### Problem Description
Given an array where every element appears twice except for TWO elements, find both elements that appear once. Return them in ascending order.

### Examples

#### Example 1
**Input:** nums = [1, 2, 1, 3, 5, 2]  
**Output:** [3, 5]

#### Example 2
**Input:** nums = [-1, 0]  
**Output:** [-1, 0]

### Solution

### Approach 1: Hashing

#### Intuition
Count frequencies and return elements with count 1.

#### Approach Steps
1. Create frequency dictionary
2. Count occurrences
3. Collect elements with frequency 1 into list
4. Sort and return

#### Code
```python
def solve(n, arr):
    count = {}
    for i in range(n):
        if arr[i] in count:
            count[arr[i]] += 1
        else:
            count[arr[i]] = 1
    
    ans = []
    for i, v in count.items():
        if v == 1:
            ans.append(i)
    
    ans.sort()
    return ans
```

#### Time and Space Complexity
- **Time Complexity:** O(n + k log k) where k=2 → O(n)
- **Space Complexity:** O(n) - Dictionary storage

---

### Approach 2: XOR and Bit Separation (Optimal)

#### Intuition
If we XOR all elements, pairs cancel out, leaving `a ^ b` (the two unique numbers).

Key insight: Find a bit where `a` and `b` differ. Use this bit to separate all numbers into two groups:
- Group 1: numbers with this bit set
- Group 2: numbers with this bit clear

Each group will have one unique number! XOR each group separately to find both numbers.

#### Visual Example
```
Array: [1, 2, 1, 3, 5, 2]

Step 1: XOR all
1 ^ 2 ^ 1 ^ 3 ^ 5 ^ 2 = 3 ^ 5 = 6 (binary: 110)

Step 2: Find rightmost set bit of 6
6 = 110
6-1 = 101
6 & 5 = 100
6 ^ 100 = 010 (bit at position 1)

Step 3: Separate by bit 1
Group 1 (bit 1 set):    [2, 3, 2] → XOR = 3
Group 2 (bit 1 clear):  [1, 1, 5] → XOR = 5

Answer: [3, 5] ✓
```

#### Approach Steps
1. XOR all elements → get `xor = a ^ b`
2. Find rightmost set bit: `rightMost = (xor & (xor-1)) ^ xor`
   - This isolates one differing bit between a and b
3. Separate numbers into two groups based on this bit:
   - Group 1 (bit set): XOR to get first unique number
   - Group 2 (bit clear): XOR to get second unique number
4. Sort and return both numbers

#### Code
```python
def solve(n, arr):
    xor = 0
    
    # XOR all elements
    for i in range(n):
        xor ^= arr[i]
    
    # Find rightmost set bit (where a and b differ)
    rightMost = (xor & (xor - 1)) ^ xor
    
    b1, b2 = 0, 0
    
    # Separate into two groups and XOR each
    for i in range(n):
        if arr[i] & rightMost:
            b1 = b1 ^ arr[i]
        else:
            b2 = b2 ^ arr[i]
    
    ans = [b1, b2]
    ans.sort()
    return ans
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Two passes through array + O(1) sort
- **Space Complexity:** O(1) - Only a few variables

---

## 5. Divide Two Numbers

### Problem Description
Divide two integers without using multiplication, division, or mod operators. Return the quotient truncated toward zero. Handle 32-bit signed integer overflow.

### Examples

#### Example 1
**Input:** dividend = 10, divisor = 3  
**Output:** 3  
**Explanation:** 10/3 = 3.33... → truncate to 3

#### Example 2
**Input:** dividend = 7, divisor = -3  
**Output:** -2  
**Explanation:** 7/-3 = -2.33... → truncate to -2

### Solution

### Approach 1: Repeated Subtraction

#### Intuition
Division is repeated subtraction! Keep subtracting divisor from dividend and count how many times we can subtract.

#### Approach Steps
1. Handle sign: determine if result should be positive/negative
2. Work with absolute values
3. Keep subtracting divisor from dividend
4. Count subtractions → this is the quotient
5. Apply sign and return

#### Code
```python
def solve(n, d):
    if n == d:
        return 1
    
    # Determine sign
    isPositive = True
    if (n < 0 and d >= 0) or (n >= 0 and d < 0):
        isPositive = False
    
    # Work with absolute values
    n, d = abs(n), abs(d)
    
    sum_val = 0
    count = 0
    
    # Repeated subtraction
    while (sum_val + d) <= n:
        sum_val += d
        count += 1
    
    # Apply sign
    if not isPositive:
        count = (-1) * count
    
    return count
```

#### Time and Space Complexity
- **Time Complexity:** O(n/d) - Can be very slow for large n and small d
- **Space Complexity:** O(1)

---

### Approach 2: Bit Manipulation (Optimal)

#### Intuition
Instead of subtracting divisor one at a time, subtract it in powers of 2! This is like binary search for the quotient.

**Example:** 10 / 3
- Can we subtract 3×8? No (24 > 10)
- Can we subtract 3×4? No (12 > 10)
- Can we subtract 3×2? Yes! (6 ≤ 10), quotient += 2, remainder = 4
- Can we subtract 3×1? Yes! (3 ≤ 4), quotient += 1, remainder = 1
- Result: quotient = 3 ✓

#### Visual Example
```
Divide 10 by 3:

Step 1: Find largest power of 2
  3×1 = 3  ≤ 10 ✓
  3×2 = 6  ≤ 10 ✓
  3×4 = 12 > 10 ✗
  
  Use 3×2: quotient += 2, n = 10-6 = 4

Step 2: Continue with remainder 4
  3×1 = 3 ≤ 4 ✓
  3×2 = 6 > 4 ✗
  
  Use 3×1: quotient += 1, n = 4-3 = 1

Step 3: Remainder 1 < 3, stop

Final quotient = 2 + 1 = 3 ✓
```

#### Approach Steps
1. Handle special case: dividend == divisor
2. Determine result sign
3. Work with absolute values
4. While dividend >= divisor:
   - Find largest bitPosition where `divisor × 2^bitPosition ≤ dividend`
   - Add `2^bitPosition` to quotient
   - Subtract `divisor × 2^bitPosition` from dividend
5. Handle overflow cases (±2^31)
6. Apply sign and return

#### Code
```python
def solve(n, d):
    if n == d:
        return 1
    
    # Determine sign
    isPositive = True
    if (n < 0 and d >= 0) or (n >= 0 and d < 0):
        isPositive = False
    
    n, d = abs(n), abs(d)
    count = 0
    
    while n >= d:
        bitPosition = 0
        
        # Find largest power of 2
        while (d * (1 << (bitPosition + 1))) <= n:
            bitPosition += 1
        
        # Add to quotient and subtract from dividend
        count += (1 << bitPosition)
        n -= (d * (1 << bitPosition))
    
    # Handle overflow
    if count == 2**31 and isPositive:
        return 2**31 - 1
    if count == 2**31 and not isPositive:
        return -2**31
    
    if not isPositive:
        count = (-1) * count
    
    return count
```

#### Time and Space Complexity
- **Time Complexity:** O(log²n) - Outer loop runs O(log n) times, inner loop also O(log n)
- **Space Complexity:** O(1)

---

## 6. Power Set using Bit Manipulation

### Problem Description
Given an array of unique integers, return all possible subsets (the power set). Do not include duplicates.

### Examples

#### Example 1
**Input:** nums = [1, 2, 3]  
**Output:** [[], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]]

#### Example 2
**Input:** nums = [1, 2]  
**Output:** [[], [1], [2], [1,2]]

### Solution

#### Intuition
For n elements, there are 2^n subsets (each element can be included or excluded).

We can represent each subset as a binary number:
- Bit 0 set → include arr[0]
- Bit 1 set → include arr[1]
- And so on...

Loop from 0 to 2^n - 1, and for each number, check which bits are set to determine which elements to include!

#### Visual Example
```
Array: [1, 2, 3]
Total subsets = 2³ = 8

Number  Binary  Subset
  0     000     []
  1     001     [1]          (bit 0 set)
  2     010     [2]          (bit 1 set)
  3     011     [1, 2]       (bits 0,1 set)
  4     100     [3]          (bit 2 set)
  5     101     [1, 3]       (bits 0,2 set)
  6     110     [2, 3]       (bits 1,2 set)
  7     111     [1, 2, 3]    (all bits set)
```

#### Approach Steps
1. Calculate total subsets: `total = 2^n`
2. For each number from 0 to total-1:
   - Create empty subset
   - For each bit position j (0 to n-1):
     - If bit j is set in current number, include arr[j]
   - Add subset to answer
3. Return all subsets

#### Code
```python
def solve(n, arr):
    total = 2 ** n
    ans = []
    
    # Generate all subsets
    for i in range(total):
        temp = []
        
        # Check each bit
        for j in range(n):
            if i & (1 << j):  # If jth bit is set
                temp.append(arr[j])
        
        ans.append(temp)
    
    return ans

# Example
arr = [1, 2, 3]
n = len(arr)
print(solve(n, arr))
```

#### Time and Space Complexity
- **Time Complexity:** O(n × 2^n) - Generate 2^n subsets, each takes O(n) to build
- **Space Complexity:** O(n × 2^n) - Store all subsets (excluding output array)

---

## 7. XOR of Numbers in a Range

### Problem Description
Given two integers L and R, find the XOR of all numbers in the range [L, R].

### Examples

#### Example 1
**Input:** L = 3, R = 5  
**Output:** 2  
**Explanation:** 3 ^ 4 ^ 5 = 2

#### Example 2
**Input:** L = 1, R = 3  
**Output:** 0  
**Explanation:** 1 ^ 2 ^ 3 = 0

### Solution

### Approach 1: Brute Force

#### Intuition
Simply XOR all numbers from L to R.

#### Approach Steps
1. Initialize `ans = 0`
2. Loop from L to R
3. XOR each number with ans
4. Return ans

#### Code
```python
def solve(left, right):
    ans = 0
    for i in range(left, right + 1):
        ans ^= i
    return ans
```

#### Time and Space Complexity
- **Time Complexity:** O(R - L) - Can be slow for large ranges
- **Space Complexity:** O(1)

---

### Approach 2: Pattern Recognition (Optimal)

#### Intuition
XOR from 1 to n follows a pattern based on n % 4:
- If n % 4 == 0: XOR = n
- If n % 4 == 1: XOR = 1
- If n % 4 == 2: XOR = n + 1
- If n % 4 == 3: XOR = 0

To get XOR from L to R: `XOR(L to R) = XOR(1 to R) ^ XOR(1 to L-1)`

#### Visual Pattern
```
n   Binary  XOR(1 to n)
1   0001    1
2   0010    3 (1^2)
3   0011    0 (1^2^3)
4   0100    4 (1^2^3^4)
5   0101    1 (1^2^3^4^5)
6   0110    7
7   0111    0
8   1000    8
9   1001    1
...

Pattern repeats every 4 numbers!
```

#### Approach Steps
1. Create helper function `findXor(n)`:
   - If n % 4 == 0: return n
   - If n % 4 == 1: return 1
   - If n % 4 == 2: return n + 1
   - If n % 4 == 3: return 0
2. Return `findXor(L-1) ^ findXor(R)`

#### Code
```python
def findXor(n):
    if n % 4 == 0:
        return n
    elif n % 4 == 1:
        return 1
    elif n % 4 == 2:
        return n + 1
    else:  # n % 4 == 3
        return 0

def solve(left, right):
    return findXor(left - 1) ^ findXor(right)

# Example
left, right = 3, 5
print(solve(left, right))  # Output: 2
```

#### Time and Space Complexity
- **Time Complexity:** O(1) - Constant time calculations
- **Space Complexity:** O(1)

---

## Summary Table

| Problem | Best Approach | Time | Space |
|---------|--------------|------|-------|
| Bit Flips | XOR + Count Bits | O(log n) | O(1) |
| Single Number I | XOR | O(n) | O(1) |
| Single Number II | Ones-Twos Method | O(n) | O(1) |
| Single Number III | XOR + Bit Separation | O(n) | O(1) |
| Divide Numbers | Bit Manipulation | O(log²n) | O(1) |
| Power Set | Bit Masking | O(n × 2^n) | O(1) |
| XOR Range | Pattern (n%4) | O(1) | O(1) |

---

## Key Bit Manipulation Concepts

### Important Operations
- `x & (x-1)` - Removes rightmost set bit
- `x & -x` - Isolates rightmost set bit
- `x ^ x = 0` - XOR of same numbers is 0
- `x ^ 0 = x` - XOR with 0 gives the number
- `1 << n` - Computes 2^n (left shift)
- `x >> n` - Divides by 2
