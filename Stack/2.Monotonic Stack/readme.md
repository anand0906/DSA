# Monotonic Stack Problems
---

## Table of Contents

1. [Monotonic Stack Basics](#1-monotonic-stack-basics)
2. [NGE Circular Array](#2-nge-circular-array)
3. [Daily Temperatures](#3-daily-temperatures)
4. [Stock Span](#4-stock-span)
5. [Remove K Digits](#5-remove-k-digits)
6. [Remove Duplicate Letters](#6-remove-duplicate-letters)
7. [Most Competitive Subsequence](#7-most-competitive-subsequence)
8. [132 Pattern](#8-132-pattern)
9. [Sum of Subarray Minimums](#9-sum-of-subarray-minimums)
10. [Largest Rectangle in Histogram](#10-largest-rectangle-in-histogram)
11. [Maximal Rectangle](#11-maximal-rectangle)
12. [Next Greater Node in Linked List](#12-next-greater-node-in-linked-list)
13. [Remove Nodes from Linked List](#13-remove-nodes-from-linked-list)
14. [Steps to Make Array Non-decreasing](#14-steps-to-make-array-non-decreasing)

---

## 1. Monotonic Stack Basics

### Intuition
A monotonic stack maintains elements in either increasing or decreasing order. It's perfect for finding next/previous greater/smaller elements efficiently. The key insight: when a new element breaks the monotonic property, we've found the answer for elements in the stack!

### Four Core Patterns

#### Next Greater Element (NGE)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def nextGreater(n, arr):
    NGE = [-1] * n  # Initialize result array
    stack = []  # Stack stores indices
    
    for i in range(n):
        # Pop elements smaller than current (they found their NGE)
        while stack and arr[i] > arr[stack[-1]]:
            temp = stack.pop()
            NGE[temp] = arr[i]
        stack.append(i)  # Push current index
    
    return NGE
```

#### Next Smaller Element (NSE)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def nextSmaller(n, arr):
    NSE = [-1] * n
    stack = []
    
    for i in range(n):
        # Pop elements larger than current (they found their NSE)
        while stack and arr[i] < arr[stack[-1]]:
            temp = stack.pop()
            NSE[temp] = arr[i]
        stack.append(i)
    
    return NSE
```

#### Previous Greater Element (PGE)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def previousGreater(n, arr):
    PGE = [-1] * n
    stack = []
    
    for i in range(n):
        # Remove elements <= current (they're not the answer)
        while stack and arr[i] >= arr[stack[-1]]:
            stack.pop()
        
        # Top of stack is the previous greater element
        if stack:
            PGE[i] = arr[stack[-1]]
        
        stack.append(i)
    
    return PGE
```

#### Previous Smaller Element (PSE)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def previousSmaller(n, arr):
    PSE = [-1] * n
    stack = []
    
    for i in range(n):
        # Remove elements >= current (they're not the answer)
        while stack and arr[i] <= arr[stack[-1]]:
            stack.pop()
        
        # Top of stack is the previous smaller element
        if stack:
            PSE[i] = arr[stack[-1]]
        
        stack.append(i)
    
    return PSE
```

### Example Usage
```python
arr = [2, 5, 3, 8, 9, 1]
n = len(arr)
print("Next Greater Elements:", nextGreater(n, arr))
print("Next Smaller Elements:", nextSmaller(n, arr))
print("Previous Greater Elements:", previousGreater(n, arr))
print("Previous Smaller Elements:", previousSmaller(n, arr))
```

---

## 2. NGE Circular Array

### Problem
Find next greater element in a circular array (where last element wraps to first).

**Examples:**
- Input: `[1,2,1]` → Output: `[2,-1,2]`
- Input: `[1,2,3,4,3]` → Output: `[2,3,4,-1,4]`

### Intuition
**Trick:** Simulate circular array by iterating twice (2*n iterations) using modulo operator. This way, each element can "see" elements that come after it in circular order.

### Brute Force Approach
**Time Complexity:** O(n²) | **Space Complexity:** O(n)

```python
def bruteForce(n, arr):
    NGE = [-1] * n
    
    for i in range(n):
        # Check next n elements in circular manner
        for j in range(i + 1, i + n):
            j = j % n  # Wrap around using modulo
            if arr[j] > arr[i]:
                NGE[i] = arr[j]
                break
    
    return NGE
```

### Optimized Approach (Monotonic Stack)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def optimized(n, arr):
    NGE = [-1] * n
    stack = []
    
    # Iterate twice to handle circular nature
    for i in range(2 * n):
        i = i % n  # Wrap index
        
        # Pop smaller elements (they found their NGE)
        while stack and arr[i] > arr[stack[-1]]:
            temp = stack.pop()
            NGE[temp] = arr[i]
        
        stack.append(i)
    
    return NGE
```

```python
arr = [1, 2, 1, 3]
n = len(arr)
print(bruteForce(n, arr))
print(optimized(n, arr))
```

---

## 3. Daily Temperatures

### Problem
Find how many days you must wait for a warmer temperature. Return 0 if no warmer day exists.

**Examples:**
- Input: `[73,74,75,71,69,72,76,73]` → Output: `[1,1,4,2,1,1,0,0]`
- Input: `[30,40,50,60]` → Output: `[1,1,1,0]`

### Intuition
This is essentially finding the next greater element, but instead of storing values, we store the **distance** (number of days) between indices.

### Brute Force Approach
**Time Complexity:** O(n²) | **Space Complexity:** O(n)

```python
def bruteForce(n, arr):
    ans = [0] * n
    
    for i in range(n):
        # Check all future days
        for j in range(i + 1, n):
            if arr[j] > arr[i]:
                ans[i] = j - i  # Store distance
                break
    
    return ans
```

### Optimized Approach (Monotonic Stack)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def optimized(n, arr):
    ans = [0] * n
    stack = []  # Stores indices of days
    
    for i in range(n):
        # Pop days with lower temperature (they found warmer day)
        while stack and arr[i] > arr[stack[-1]]:
            index = stack.pop()
            ans[index] = i - index  # Calculate distance
        
        stack.append(i)
    
    return ans
```

```python
arr = [73, 74, 75, 71, 69, 72, 76, 73]
n = len(arr)
print(bruteForce(n, arr))
print(optimized(n, arr))
```

---

## 4. Stock Span

### Problem
Calculate the span of stock price: maximum consecutive days (including current) where price was ≤ current day's price.

**Example:**
- Input: `[100, 80, 60, 70, 60, 75, 85]` → Output: `[1, 1, 1, 2, 1, 4, 6]`

### Intuition
Find the **previous greater element** for each day. The span is the distance from that previous greater element (or from start if none exists). This tells us how many consecutive days had lower or equal prices.

### Brute Force Approach
**Time Complexity:** O(n²) | **Space Complexity:** O(n)

```python
def bruteForce(n, arr):
    ans = [1] * n
    
    for i in range(n):
        # Look backwards for first greater price
        for j in range(i - 1, -1, -1):
            if arr[j] > arr[i]:
                ans[i] = i - j
                break
        else:
            # No greater element found, span is from start
            ans[i] = i + 1
    
    return ans
```

### Optimized Approach (Monotonic Stack)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def optimized(n, arr):
    ans = [1] * n
    stack = []  # Stores indices
    
    for i in range(n):
        # Pop days with price <= current
        while stack and arr[i] >= arr[stack[-1]]:
            stack.pop()
        
        # Calculate span based on previous greater element
        if stack:
            index = stack[-1]
            ans[i] = i - index
        else:
            ans[i] = i + 1  # No greater element, span from start
        
        stack.append(i)
    
    return ans
```

```python
arr = [100, 80, 60, 70, 60, 75, 85]
n = len(arr)
print(bruteForce(n, arr))
print(optimized(n, arr))
```

---

## 5. Remove K Digits

### Problem
Remove k digits from a number string to get the smallest possible number.

**Examples:**
- Input: `num = "1432219", k = 3` → Output: `"1219"`
- Input: `num = "10200", k = 1` → Output: `"200"`

### Intuition
To minimize the number, remove larger digits that appear before smaller digits. Use a monotonic increasing stack: whenever we find a digit smaller than the stack top, pop the larger digit (this makes the number smaller). Continue until we've removed k digits.

### Brute Force Approach
**Time Complexity:** O(n*k) | **Space Complexity:** O(n)

```python
def bruteForce(num, k):
    num = list(num)
    
    for i in range(k):
        n = len(num)
        flag = True
        
        # Find first decreasing position
        for j in range(1, n):
            if num[j] < num[j-1]:
                del num[j-1]  # Remove larger digit
                flag = False
                break
        else:
            # No decreasing position, remove last digit
            del num[n-1]
    
    # Remove leading zeros
    num = "".join(num).lstrip('0')
    return "0" if num == "" else num
```

### Optimized Approach (Monotonic Stack)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def optimized(num, k):
    stack = []
    n = len(num)
    
    for i in range(n):
        # Pop larger digits when we find smaller digit
        while stack and num[i] < stack[-1] and k > 0:
            stack.pop()
            k -= 1
        
        stack.append(num[i])
    
    # If k still remains, remove from end
    while k > 0:
        stack.pop()
        k -= 1
    
    # Convert to string and remove leading zeros
    num = "".join(stack).lstrip('0')
    return num if num else "0"
```

```python
num = "1432219"
k = 3
print(bruteForce(num, k))
print(optimized(num, k))
```

---

## 6. Remove Duplicate Letters

### Problem
Remove duplicate letters so each letter appears once, and result is smallest in lexicographical order.

**Examples:**
- Input: `"bcabc"` → Output: `"abc"`
- Input: `"cbacdcbc"` → Output: `"acdb"`

### Intuition
Use a greedy approach with a monotonic stack. For each character, if it's smaller than stack top AND the stack top appears later in the string, we can safely remove the stack top (we'll add it later). Track last position of each character and visited status.

**Time Complexity:** O(n) | **Space Complexity:** O(26) = O(1) for alphabet

```python
def solve(s):
    stack = []
    n = len(s)
    
    # Track last occurrence of each character
    lastPos = {char: i for i, char in enumerate(s)}
    
    # Track which characters are already in result
    visited = {i: False for i in set(s)}
    
    for i in range(n):
        # Skip if already in result
        if visited[s[i]]:
            continue
        
        # Pop larger characters that appear later
        while stack and s[i] < stack[-1] and lastPos[stack[-1]] > i:
            temp = stack.pop()
            visited[temp] = False
        
        # Add current character
        stack.append(s[i])
        visited[s[i]] = True
    
    return "".join(stack)
```

```python
s = "bcabc"
print(solve(s))
```

---

## 7. Most Competitive Subsequence

### Problem
Find the most competitive subsequence of size k. A subsequence is more competitive if at the first differing position, it has a smaller number.

**Examples:**
- Input: `nums = [3,5,2,6], k = 2` → Output: `[2,6]`
- Input: `nums = [2,4,3,3,5,4,9,6], k = 4` → Output: `[2,3,3,4]`

### Intuition
This is similar to "Remove K Digits"! We need to remove (n-k) elements. Use a monotonic increasing stack, popping larger elements when we find smaller ones, ensuring we keep k elements.

**Time Complexity:** O(n) | **Space Complexity:** O(k)

```python
def removeKdigits(n, arr, k):
    stack = []
    
    for i in range(n):
        # Pop larger elements when we find smaller
        while stack and arr[i] < stack[-1] and k > 0:
            stack.pop()
            k -= 1
        
        stack.append(arr[i])
    
    # Remove remaining if needed
    while stack and k > 0:
        stack.pop()
        k -= 1
    
    return stack

def solve(n, arr, k):
    m = n - k  # Number of elements to remove
    ans = removeKdigits(n, arr, m)
    return ans
```

```python
arr = [3, 5, 2, 6]
n = len(arr)
k = 2
print(solve(n, arr, k))
```

---

## 8. 132 Pattern

### Problem
Find if there exists a subsequence of three integers nums[i], nums[j], nums[k] where i < j < k and nums[i] < nums[k] < nums[j].

**Examples:**
- Input: `[1,2,3,4]` → Output: `false`
- Input: `[3,1,4,2]` → Output: `true` (pattern: [1, 4, 2])

### Intuition
For 132 pattern, we need: small number (1), large number (3), medium number (2). Use a stack to track potential "3" values while maintaining minimum "1" value seen so far.

### Brute Force Approach
**Time Complexity:** O(n³) | **Space Complexity:** O(1)

```python
def bruteforce(n, arr):
    # Try all triplets
    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                if arr[i] < arr[k] < arr[j]:
                    return True
    return False
```

### Better Approach (Using PGE)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def better(n, arr):
    # Find previous greater element for each position
    PGE = [-1] * n
    stack = []
    for i in range(n):
        while stack and arr[i] >= arr[stack[-1]]:
            stack.pop()
        if stack:
            PGE[i] = stack[-1]
        stack.append(i)
    
    # Track minimum element up to each position
    mini = [float('inf')] * n
    current = float('inf')
    for i in range(n):
        mini[i] = current
        current = min(current, arr[i])
    
    # Check if 132 pattern exists
    for k in range(n):
        if PGE[k] != -1 and arr[k] > mini[PGE[k]]:
            return True
    
    return False
```

### Optimized Approach (Single Pass)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def optimized(n, arr):
    mini = float('inf')  # Track minimum seen so far
    stack = []  # Stack stores (index, minimum_before_this_index)
    
    for i in range(n):
        # Pop elements >= current
        while stack and arr[i] >= arr[stack[-1][0]]:
            stack.pop()
        
        # Check if 132 pattern exists
        # arr[i] is "2", stack top's min is "1", stack top is "3"
        if stack and arr[i] > stack[-1][1]:
            return True
        
        # Push current element with minimum before it
        stack.append((i, mini))
        mini = min(arr[i], mini)
    
    return False
```

```python
arr = [1, 3, 2]
n = len(arr)
print(bruteforce(n, arr))
print(better(n, arr))
print(optimized(n, arr))
```

---

## 9. Sum of Subarray Minimums

### Problem
Find sum of minimum elements across all subarrays.

**Examples:**
- Input: `[3,1,2,4]` → Output: `17`
- Input: `[11,81,94,43,3]` → Output: `444`

### Intuition
For each element, calculate: **how many subarrays have this element as minimum?** 
- Count subarrays where element is minimum = (left_count) × (right_count)
- Left count: distance to previous smaller element
- Right count: distance to next smaller element

### Brute Force Approach
**Time Complexity:** O(n²) | **Space Complexity:** O(1)

```python
def bruteforce(n, arr):
    sum = 0
    
    # Generate all subarrays
    for i in range(n):
        temp = float('inf')
        for j in range(i, n):
            temp = min(temp, arr[j])  # Track minimum
            sum += temp
    
    return sum
```

### Better Approach (PSE + NSE)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

**Formula Explanation:**
- For element at index `i`:
  - `left = i - PSE[i] - 1` → number of elements we can extend to the left
  - `right = NSE[i] - i` → number of elements we can extend to the right
  - Total subarrays with `arr[i]` as minimum = `(left + 1) × (right)`
  - Contribution = `arr[i] × (left + 1) × (right)`

**Example:** `arr = [3,1,2,4]`, consider element `1` at index 1:
- PSE[1] = -1 (no previous smaller)
- NSE[1] = 4 (no next smaller, use n)
- left = 1 - (-1) - 1 = 1 (can extend 1 position left)
- right = 4 - 1 = 3 (can extend 3 positions right)
- Subarrays: [1], [3,1], [1,2], [1,2,4], [3,1,2], [3,1,2,4] = 6 subarrays
- Formula: (1+1) × 3 = 6 ✓
- Contribution: 1 × 6 = 6

```python
def better(n, arr):
    # Find Previous Smaller Element
    PSE = [-1] * n
    stack = []
    for i in range(n):
        while stack and arr[i] < arr[stack[-1]]:
            stack.pop()
        if stack:
            PSE[i] = stack[-1]
        stack.append(i)
    
    # Find Next Smaller Element
    NSE = [n] * n
    stack = []
    for i in range(n):
        while stack and arr[i] < arr[stack[-1]]:
            temp = stack.pop()
            NSE[temp] = i
        stack.append(i)
    
    # Calculate sum
    sum = 0
    for i in range(n):
        left = i - PSE[i] - 1  # Elements to the left
        right = NSE[i] - i  # Elements to the right
        sum += ((right * left) + right) * arr[i]
    
    return sum
```

### Optimized Approach (Single Pass)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

**Formula Explanation:**
- Build result incrementally: `result[i]` = sum of minimums for all subarrays ending at `i`
- For element at `i`:
  - If no PSE: `result[i] = arr[i] × (i + 1)` (arr[i] is minimum for all subarrays ending at i)
  - If PSE exists at `j`: `result[i] = result[j] + arr[i] × (i - j)`
    - We reuse `result[j]` (all subarrays from 0 to j)
    - Add new contribution: arr[i] appears as minimum in (i-j) new subarrays

**Example:** `arr = [3,1,2,4]`
- i=0: result[0] = 3×1 = 3 (subarray [3])
- i=1: PSE=-1, result[1] = 1×2 = 2 (subarrays [3,1], [1])
- i=2: PSE=1, result[2] = result[1] + 2×(2-1) = 2 + 2 = 4 (adds [1,2], [2])
- i=3: PSE=2, result[3] = result[2] + 4×(3-2) = 4 + 4 = 8 (adds [2,4], [4])
- Sum = 3 + 2 + 4 + 8 = 17 ✓

```python
def optimized(n, arr):
    result = [0] * n
    stack = []
    
    for i in range(n):
        # Pop elements >= current
        while stack and arr[i] < arr[stack[-1]]:
            stack.pop()
        
        if stack:
            PSE = stack[-1]
            # Reuse previous result + new contribution
            result[i] = result[PSE] + (i - PSE) * arr[i]
        else:
            # No previous smaller, arr[i] is minimum for all subarrays ending at i
            result[i] = arr[i] * (i + 1)
        
        stack.append(i)
    
    return sum(result)
```

```python
arr = [3, 1, 2, 4]
n = len(arr)
print(bruteforce(n, arr))
print(better(n, arr))
print(optimized(n, arr))
```

---

## 10. Largest Rectangle in Histogram

### Problem
Find the area of the largest rectangle in a histogram where each bar has width 1.

**Examples:**
- Input: `[2,1,5,6,2,3]` → Output: `10`
- Input: `[2,4]` → Output: `4`

### Intuition
For each bar, find the maximum width rectangle using this bar's height. Width = distance between previous smaller element and next smaller element. The bar can extend left until we hit a smaller bar, and right until we hit a smaller bar.

### Brute Force Approach
**Time Complexity:** O(n²) | **Space Complexity:** O(1)

```python
def bruteforce(n, arr):
    maxArea = 0
    
    for i in range(n):
        # Find left boundary (previous smaller)
        leftMin = -1
        for j in range(i - 1, -1, -1):
            if arr[j] < arr[i]:
                leftMin = j
                break
        
        # Find right boundary (next smaller)
        rightMin = n
        for j in range(i + 1, n):
            if arr[j] < arr[i]:
                rightMin = j
                break
        
        # Calculate area with arr[i] as height
        width = rightMin - leftMin - 1
        area = width * arr[i]
        maxArea = max(maxArea, area)
    
    return maxArea
```

### Better Approach (PSE + NSE)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def better(n, arr):
    # Find Previous Smaller Element
    PSE = [-1] * n
    stack = []
    for i in range(n):
        while stack and arr[i] < arr[stack[-1]]:
            stack.pop()
        if stack:
            PSE[i] = stack[-1]
        stack.append(i)
    
    # Find Next Smaller Element
    NSE = [n] * n
    stack = []
    for i in range(n):
        while stack and arr[i] < arr[stack[-1]]:
            temp = stack.pop()
            NSE[temp] = i
        stack.append(i)
    
    # Calculate maximum area
    maxArea = 0
    for i in range(n):
        left = PSE[i]
        right = NSE[i]
        width = right - left - 1
        area = width * arr[i]
        maxArea = max(maxArea, area)
    
    return maxArea
```

```python
arr = [2, 1, 5, 6, 2, 3]
n = len(arr)
print(bruteforce(n, arr))
print(better(n, arr))
```

---

## 11. Maximal Rectangle

### Problem
Find the largest rectangle containing only 1's in a binary matrix.

**Example:**
```
Input: [["1","0","1","0","0"],
        ["1","0","1","1","1"],
        ["1","1","1","1","1"],
        ["1","0","0","1","0"]]
Output: 6
```

### Intuition
Convert each row into a histogram problem! For each row, calculate height of consecutive 1's above (including current row). Then apply "Largest Rectangle in Histogram" algorithm on each row.

**Time Complexity:** O(n×m) | **Space Complexity:** O(n×m)

```python
def better(n, arr):
    # Same as Largest Rectangle in Histogram
    PSE = [-1] * n
    stack = []
    for i in range(n):
        while stack and arr[i] < arr[stack[-1]]:
            stack.pop()
        if stack:
            PSE[i] = stack[-1]
        stack.append(i)
    
    NSE = [n] * n
    stack = []
    for i in range(n):
        while stack and arr[i] < arr[stack[-1]]:
            temp = stack.pop()
            NSE[temp] = i
        stack.append(i)
    
    maxArea = 0
    for i in range(n):
        left = PSE[i]
        right = NSE[i]
        width = right - left - 1
        area = width * arr[i]
        maxArea = max(maxArea, area)
    
    return maxArea

def solve(n, m, matrix):
    # Build height matrix (histogram for each row)
    mat = [[0] * m for i in range(n)]
    
    for i in range(n):
        for j in range(m):
            if matrix[i][j] == "1":
                if i == 0:
                    mat[i][j] = 1
                else:
                    # Add height from previous row
                    mat[i][j] = mat[i-1][j] + 1
    
    # Apply histogram algorithm on each row
    ans = 0
    for i in range(n):
        temp = better(m, mat[i])
        ans = max(ans, temp)
    
    return ans
```

```python
matrix = [["1","0","1","0","0"],
          ["1","0","1","1","1"],
          ["1","1","1","1","1"],
          ["1","0","0","1","0"]]
n, m = len(matrix), len(matrix[0])
print(solve(n, m, matrix))
```

---

## 12. Next Greater Node in Linked List

### Problem
For each node in linked list, find the value of the next greater node.

**Examples:**
- Input: `[2,1,5]` → Output: `[5,5,0]`
- Input: `[2,7,4,3,5]` → Output: `[7,0,5,5,0]`

### Intuition
Convert linked list to array, then apply standard Next Greater Element algorithm using monotonic stack.

**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def solve(head):
    # Convert linked list to array
    arr = []
    while head:
        arr.append(head.val)
        head = head.next
    
    # Apply NGE algorithm
    n = len(arr)
    NGE = [0] * n
    stack = []
    
    for i in range(n):
        # Pop smaller elements (they found their NGE)
        while stack and arr[i] > arr[stack[-1]]:
            temp = stack.pop()
            NGE[temp] = arr[i]
        
        stack.append(i)
    
    return NGE
```

---

## 13. Remove Nodes from Linked List

### Problem
Remove nodes that have a greater value anywhere to the right.

**Examples:**
- Input: `[5,2,13,3,8]` → Output: `[13,8]`
- Input: `[1,1,1,1]` → Output: `[1,1,1,1]`

### Intuition
Keep only nodes that are part of a decreasing sequence from right to left. Use monotonic decreasing stack: only keep nodes where no greater node exists to the right.

**Time Complexity:** O(n) | **Space Complexity:** O(n)

```python
def solve(head):
    current = head
    stack = []
    
    # Build monotonic decreasing stack
    while current:
        # Pop nodes with smaller values
        while stack and current.val > stack[-1].val:
            stack.pop()
        
        stack.append(current)
        current = current.next
    
    # Rebuild linked list from stack
    dummy = ListNode()
    current = dummy
    for node in stack:
        current.next = node
        current = current.next
    
    return dummy.next
```

---

## 14. Steps to Make Array Non-decreasing

### Problem
Count steps to make array non-decreasing. In each step, remove all elements nums[i] where nums[i-1] > nums[i].

**Examples:**
- Input: `[5,3,4,4,7,3,6,11,8,5,11]` → Output: `3`
- Input: `[4,5,7,7,13]` → Output: `0`

### Intuition
Each element "eats" smaller elements to its right. Track how many steps each element takes to eat all smaller elements. The answer is the maximum steps any element takes.

**Key Insight:** When element `arr[i]` eats element at `stack[-1]`, it inherits the eating time: `maxEat[i] = max(maxEat[i] + 1, maxEat[stack[-1]])`. This is because eating happens in parallel - arr[i] must wait for stack[-1] to finish eating before it can continue.

### Brute Force Approach
**Time Complexity:** O(n²) in worst case | **Space Complexity:** O(n)

```python
def bruteForce(n, arr):
    steps = 0
    
    for i in range(n):
        found = False
        temp = arr.copy()
        n = len(arr)
        
        # Remove elements where arr[j] < arr[j-1]
        for j in range(1, n):
            if arr[j] < arr[j-1]:
                temp[j] = None
                found = True
        
        # Filter out removed elements
        arr = [k for k in temp if k != None]
        
        if not found:
            break
        
        steps += 1
    
    return steps
```

### Optimized Approach (Monotonic Stack)
**Time Complexity:** O(n) | **Space Complexity:** O(n)

**Formula Explanation with Example:**

Consider `arr = [5,3,4,4,7,3,6,11,8,5,11]`:

**Step-by-step trace:**
1. i=10 (val=11): stack=[], maxEat[10]=0
2. i=9 (val=5): 5<11, maxEat[9]=max(0+1, 0)=1, stack=[10]
3. i=8 (val=8): 8>5, pop 9. maxEat[8]=max(0+1, 1)=1, stack=[10]
4. i=7 (val=11): 11>=8, pop 8. maxEat[7]=max(0+1, 1)=1, stack=[10]
5. i=6 (val=6): 6<11, maxEat[6]=max(0+1, 1)=1, stack=[10,7]
6. i=5 (val=3): 3<6, maxEat[5]=max(0+1, 1)=1, stack=[10,7,6]
7. i=4 (val=7): 7>3, pop 5. 7>6, pop 6. maxEat[4]=max(1+1, 1)=2, stack=[10,7]
8. i=3 (val=4): 4<7, maxEat[3]=max(0+1, 2)=2, stack=[10,7,4]
9. i=2 (val=4): 4>=4, pop 3. maxEat[2]=max(0+1, 2)=2, stack=[10,7,4]
10. i=1 (val=3): 3<4, maxEat[1]=max(0+1, 2)=2, stack=[10,7,4,2]
11. i=0 (val=5): 5>3, pop 1. 5>4, pop 2. maxEat[0]=max(1+1, 2)=2, then max(2+1, 2)=3

Maximum = 3 ✓

```python
def optimized(n, arr):
    stack = []
    maxEat = [0] * n  # Steps needed to "eat" all smaller elements to the right
    maxi = 0
    
    # Traverse from right to left
    for i in range(n - 1, -1, -1):
        # Pop elements that current element can "eat"
        while stack and arr[i] > arr[stack[-1]]:
            index = stack.pop()
            # Inherit eating time from popped element
            maxEat[i] = max(maxEat[i] + 1, maxEat[index])
        
        stack.append(i)
        maxi = max(maxi, maxEat[i])
    
    return maxi
```

```python
arr = [5, 3, 4, 4, 7, 3, 6, 11, 8, 5, 11]
n = len(arr)
print(bruteForce(n, arr))
print(optimized(n, arr))
```

---

## Summary of Techniques

### Pattern Recognition Guide

| Problem Type | Key Indicator | Approach |
|-------------|---------------|----------|
| **Next/Previous Greater/Smaller** | Need to find closest element with relation | Monotonic stack |
| **Circular Array** | Array wraps around | Iterate 2×n with modulo |
| **Distance/Days Count** | Count positions between elements | Store indices, calculate difference |
| **Remove K Elements** | Make result smallest/largest | Monotonic stack, greedily remove |
| **Lexicographical Order** | Keep only necessary characters | Track last position + visited |
| **Pattern Finding (132)** | Find specific number relationships | Stack + track minimums |
| **Subarray Contributions** | Each element contributes to multiple subarrays | PSE + NSE, multiply counts |
| **Rectangle Problems** | Find max area with constraints | Convert to histogram |
| **Eating/Removal Simulation** | Elements consume others over time | Track inherited time |

### Time Complexity Summary

- **Monotonic Stack Operations**: O(n) - each element pushed/popped once
- **Brute Force Approaches**: Usually O(n²) or O(n³)
- **Space Complexity**: Typically O(n) for stack storage

### Key Takeaways

1. **Monotonic Stack = O(n) solution** for next/previous greater/smaller problems
2. **Each element is pushed and popped at most once** - that's why it's O(n)!
3. **For circular arrays**: Iterate 2×n times using modulo
4. **For contribution counting**: Use (left_count) × (right_count) formula
5. **For optimization**: Try to reuse previous computations
6. **Stack stores indices, not values** (usually) - for distance calculations
7. **Traverse direction matters**: Right-to-left for different perspectives
