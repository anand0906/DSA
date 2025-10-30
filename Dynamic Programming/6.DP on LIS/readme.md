# Longest Increasing Subsequence

## Problem Statement

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3, 6, 2, 7]` is a subsequence of `[0, 3, 1, 6, 2, 2, 7]`.

### Examples

**Example 1:**
```
Input: nums = [10, 9, 2, 5, 3, 7, 101, 18]
Output: 4
Explanation: The longest increasing subsequence is [2, 3, 7, 101], length = 4
```

**Example 2:**
```
Input: nums = [0, 1, 0, 3, 2, 3]
Output: 4
Explanation: The longest increasing subsequence is [0, 1, 2, 3], length = 4
```

---

## Problem Analysis

### Step 1: Define the Problem
- Given an array of length `n` with some numbers
- Find subsequences whose elements are in strictly increasing order
- Return the length of the longest such subsequence

### Step 2: Represent the Problem Programmatically
Define the function:
```
f(index, last_index) → Length of LIS from index to end, where last_index is the previous picked element
```

We need to find `f(0, -1)`, where `-1` indicates no previous element has been picked.

### Step 3: Base Case
```
if index == n:
    return 0  # No more elements to process
```

### Step 4: Recurrence Relation

At each index, we have two choices:

1. **Exclude** the current element: Move to the next index without picking current element
   ```
   exclude = f(index+1, last_index)
   ```

2. **Include** the current element: Only if it's greater than the last picked element
   ```
   if last_index == -1 OR arr[index] > arr[last_index]:
       include = 1 + f(index+1, index)
   ```

The answer is the maximum of both choices:
```
return max(include, exclude)
```

---

## Recursion Tree

For array `[10, 9, 2, 5]`:

```
                        f(0, -1)
                       /        \
                      /          \
              f(1, 0)              f(1, -1)
             /      \              /       \
            /        \            /         \
       f(2, 1)    f(2, 0)    f(2, 1)      f(2, -1)
       /    \      /   \      /   \        /     \
      /      \    /     \    /     \      /       \
   f(3,2) f(3,1) ...   ... ...    ... f(3,2)    f(3,-1)
     |      |                            |          |
     0      0                            0          0
```

**Legend:**
- `f(index, last_index)` represents the state
- Left branch: Include current element
- Right branch: Exclude current element

---

## Approach 1: Recursion

```python
def solve(n, arr, index, last_index):
    # Base case: reached end of array
    if index == n:
        return 0
    
    # Option 1: Include current element (if valid)
    include = 0
    if last_index == -1 or arr[index] > arr[last_index]:
        include = 1 + solve(n, arr, index+1, index)
    
    # Option 2: Exclude current element
    exclude = solve(n, arr, index+1, last_index)
    
    return max(include, exclude)
```

**Time Complexity:** O(2^n) - Two choices at each index  
**Space Complexity:** O(n) - Recursion stack depth

---

## Approach 2: Memoization (Top-Down DP)

### Conversion: Recursion → Memoization

**Changes made:**
1. Added `memo` parameter: 2D array of size `[n+1][n+1]`
2. Shifted `last_index` by 1: Since `-1` cannot be an array index, we use `last_index+1` for indexing
3. Check if subproblem is already solved before computing
4. Store the result before returning

```python
def solve_memo(n, arr, index, last_index, memo):
    # Check if already computed
    if memo[index][last_index+1] != None:
        return memo[index][last_index+1]
    
    # Base case
    if index == n:
        return 0
    
    # Include current element (if valid)
    include = 0
    if last_index == -1 or arr[index] > arr[last_index]:
        include = 1 + solve_memo(n, arr, index+1, index, memo)
    
    # Exclude current element
    exclude = solve_memo(n, arr, index+1, last_index, memo)
    
    # Store and return result
    memo[index][last_index+1] = max(include, exclude)
    return memo[index][last_index+1]
```

**Time Complexity:** O(n²) - We compute each state once  
**Space Complexity:** O(n²) - Memoization table + O(n) recursion stack

---

## Approach 3: Tabulation (Bottom-Up DP)

### Conversion: Memoization → Tabulation

**Changes made:**
1. Eliminated recursion by using iterative loops
2. Filled the DP table in reverse order (from `n` to `0`)
3. `last_index` ranges from `n-1` to `-1` (mapped to `n` to `0` in array)
4. Base case is explicitly set: `dp[n][last_index] = 0` for all `last_index`

```python
def tabulation(n, arr):
    # Create DP table
    dp = [[None]*(n+1) for _ in range(n+1)]
    
    # Base case: when index = n
    for last_index in range(n+1):
        dp[n][last_index] = 0
    
    # Fill table from bottom to top
    for index in range(n-1, -1, -1):
        for last_index in range(n-1, -2, -1):  # -2 because we go till -1
            # Include current element (if valid)
            include = 0
            if last_index == -1 or arr[index] > arr[last_index]:
                include = 1 + dp[index+1][index+1]
            
            # Exclude current element
            exclude = dp[index+1][last_index+1]
            
            dp[index][last_index+1] = max(include, exclude)
    
    return dp[0][0]  # f(0, -1) mapped to dp[0][0]
```

### Tabulation Table Example

For array `[10, 9, 2]`:

| index\last_index | -1 | 0 | 1 | 2 | 3 |
|------------------|----|----|----|----|---|
| **3 (base)**     | 0  | 0  | 0  | 0  | 0 |
| **2**            | 1  | 1  | 1  | 0  | - |
| **1**            | 1  | 1  | 1  | -  | - |
| **0**            | 1  | 1  | -  | -  | - |

- `dp[0][0]` = `f(0, -1)` = 1 (answer)
- Each cell represents the LIS length starting from that index with that last_index

**Time Complexity:** O(n²) - Two nested loops  
**Space Complexity:** O(n²) - DP table

---

## Approach 4: Space Optimization

### Conversion: Tabulation → Space Optimization

**Observation:** At any index, we only need values from `index+1` row.

**Changes made:**
1. Instead of 2D array, use two 1D arrays: `dp_curr` and `dp_next`
2. `dp_next` stores results for `index+1`
3. After computing `dp_curr`, copy it to `dp_next` for next iteration

```python
def optimized(n, arr):
    dp_curr = [None]*(n+1)
    dp_next = [None]*(n+1)
    
    # Base case
    for last_index in range(n+1):
        dp_next[last_index] = 0
    
    # Fill from bottom to top
    for index in range(n-1, -1, -1):
        for last_index in range(n-1, -2, -1):
            # Include current element (if valid)
            include = 0
            if last_index == -1 or arr[index] > arr[last_index]:
                include = 1 + dp_next[index+1]
            
            # Exclude current element
            exclude = dp_next[last_index+1]
            
            dp_curr[last_index+1] = max(include, exclude)
        
        dp_next = dp_curr.copy()
    
    return dp_next[0]
```

**Time Complexity:** O(n²)  
**Space Complexity:** O(n) - Two 1D arrays

---

## Approach 5: Algorithmic (Optimal)

### Different Approach - Using Previous Answers

Instead of tracking `(index, last_index)`, we maintain a DP array where `dp[i]` = length of LIS ending at index `i`.

**Algorithm:**
1. Initialize `dp[i] = 1` for all indices (each element is LIS of length 1)
2. For each index, check all previous indices
3. If `arr[index] > arr[last_index]`, we can extend that subsequence
4. Update `dp[index]` with the maximum length found

```python
def algorithmic(n, arr):
    # dp[i] = length of LIS ending at index i
    dp = [1]*(n+1)
    
    for index in range(n):
        for last_index in range(index):
            # Can we extend the subsequence ending at last_index?
            if arr[index] > arr[last_index]:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    
    return max(dp)  # Maximum among all ending positions
```

**Example for `[10, 9, 2, 5]`:**

| Index | Element | dp[i] | Explanation |
|-------|---------|-------|-------------|
| 0     | 10      | 1     | First element |
| 1     | 9       | 1     | 9 < 10, cannot extend |
| 2     | 2       | 1     | 2 < all previous |
| 3     | 5       | 2     | 5 > 2, extend from index 2: dp[3] = 1 + dp[2] = 2 |

**Time Complexity:** O(n²)  
**Space Complexity:** O(n)

---

## Complete Code

```python
def solve(n, arr, index, last_index):
    if index == n:
        return 0
    
    include = 0
    if last_index == -1 or arr[index] > arr[last_index]:
        include = 1 + solve(n, arr, index+1, index)
    exclude = solve(n, arr, index+1, last_index)
    return max(include, exclude)

def solve_memo(n, arr, index, last_index, memo):
    if memo[index][last_index+1] != None:
        return memo[index][last_index+1]
    if index == n:
        return 0
    include = 0
    if last_index == -1 or arr[index] > arr[last_index]:
        include = 1 + solve_memo(n, arr, index+1, index, memo)
    exclude = solve_memo(n, arr, index+1, last_index, memo)
    memo[index][last_index+1] = max(include, exclude)
    return memo[index][last_index+1]

def tabulation(n, arr):
    dp = [[None]*(n+1) for _ in range(n+1)]
    for last_index in range(n+1):
        dp[n][last_index] = 0
    for index in range(n-1, -1, -1):
        for last_index in range(n-1, -2, -1):
            include = 0
            if last_index == -1 or arr[index] > arr[last_index]:
                include = 1 + dp[index+1][index+1]
            exclude = dp[index+1][last_index+1]
            dp[index][last_index+1] = max(include, exclude)
    return dp[0][0]

def optimized(n, arr):
    dp_curr = [None]*(n+1)
    dp_next = [None]*(n+1)
    for last_index in range(n+1):
        dp_next[last_index] = 0
    for index in range(n-1, -1, -1):
        for last_index in range(n-1, -2, -1):
            include = 0
            if last_index == -1 or arr[index] > arr[last_index]:
                include = 1 + dp_next[index+1]
            exclude = dp_next[last_index+1]
            dp_curr[last_index+1] = max(include, exclude)
        dp_next = dp_curr.copy()
    return dp_next[0]

def algorithmic(n, arr):
    dp = [1]*(n+1)
    for index in range(n):
        for last_index in range(index):
            if arr[index] > arr[last_index]:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    return max(dp)

# Test
arr = [10, 9, 2, 5, 3, 7, 101, 18]
n = len(arr)
print(solve(n, arr, 0, -1))
memo = [[None]*(n+1) for _ in range(n+1)]
print(solve_memo(n, arr, 0, -1, memo))
print(tabulation(n, arr))
print(optimized(n, arr))
print(algorithmic(n, arr))
```

---

## Complexity Summary

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n²) | O(n²) |
| Tabulation | O(n²) | O(n²) |
| Space Optimized | O(n²) | O(n) |
| Algorithmic | O(n²) | O(n) |

---

# Longest Increasing Subsequence - Binary Search Approach

## Problem Statement

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3, 6, 2, 7]` is a subsequence of `[0, 3, 1, 6, 2, 2, 7]`.

### Examples

**Example 1:**
```
Input: nums = [10, 9, 2, 5, 3, 7, 101, 18]
Output: 4
Explanation: The longest increasing subsequence is [2, 3, 7, 101], length = 4
```

**Example 2:**
```
Input: nums = [0, 1, 0, 3, 2, 3]
Output: 4
Explanation: The longest increasing subsequence is [0, 1, 2, 3], length = 4
```

---

## The Intuition Behind Binary Search Approach

### Core Idea

Instead of exploring all possible subsequences, we maintain a smart auxiliary array `temp` where:

**`temp[i]` = the smallest tail element of all increasing subsequences of length `i+1`**

### Why "Smallest Tail"?

Because a smaller tail element gives us **more opportunities** to extend the subsequence with future elements.

**Example:**
- Subsequence 1: `[2, 5]` (tail = 5)
- Subsequence 2: `[2, 3]` (tail = 3)

Both have length 2, but subsequence 2 with tail `3` is better because more future elements (like 4, 5, 6...) can extend it.

---

## Algorithm Steps

```python
import bisect

def solve(n, arr):
    temp = [arr[0]]  # Initialize with first element
    
    for index in range(1, n):
        if arr[index] > temp[-1]:
            # Current element is larger than all elements in temp
            # Extend the longest subsequence
            temp.append(arr[index])
        else:
            # Find the position where arr[index] should be placed
            # Replace the element at that position to maintain smallest tails
            lower_bound = bisect.bisect_left(temp, arr[index])
            temp[lower_bound] = arr[index]
    
    return len(temp)
```

### Two Cases:

1. **If `arr[index] > temp[-1]`:** 
   - Current element is larger than all elements in `temp`
   - We can extend the longest subsequence
   - **Action:** Append to `temp`

2. **If `arr[index] <= temp[-1]`:**
   - Current element cannot extend the longest subsequence
   - But it might help create a better (smaller tail) subsequence of some length
   - **Action:** Find the first element in `temp` that is >= `arr[index]` and replace it

---

## Visual Walkthrough

Let's trace through `arr = [10, 9, 2, 5, 3, 7, 101, 18]`

### Step-by-Step Execution:

```
Initial: temp = [10]
```

| Step | Element | Comparison | Action | temp | Explanation |
|------|---------|------------|--------|------|-------------|
| 1 | 9 | 9 < 10 | Replace | `[9]` | 9 is better tail for length-1 subsequence |
| 2 | 2 | 2 < 9 | Replace | `[2]` | 2 is even better tail for length-1 |
| 3 | 5 | 5 > 2 | Append | `[2, 5]` | Extend: now we have length-2 subsequence |
| 4 | 3 | 3 < 5 | Replace | `[2, 3]` | 3 is better tail than 5 for length-2 |
| 5 | 7 | 7 > 3 | Append | `[2, 3, 7]` | Extend: now we have length-3 subsequence |
| 6 | 101 | 101 > 7 | Append | `[2, 3, 7, 101]` | Extend: now we have length-4 subsequence |
| 7 | 18 | 18 < 101 | Replace | `[2, 3, 7, 18]` | 18 is better tail than 101 for length-4 |

**Final Answer:** `len(temp) = 4`

---

## Why This Works: Detailed Explanation

### Key Insight 1: Length Preservation

The length of `temp` always equals the length of the longest increasing subsequence found so far.

### Key Insight 2: Maintaining Invariant

At any point, `temp` is sorted in increasing order and represents:
- `temp[0]` = smallest tail for LIS of length 1
- `temp[1]` = smallest tail for LIS of length 2
- `temp[2]` = smallest tail for LIS of length 3
- And so on...

### Key Insight 3: Replacement Strategy

When we replace `temp[i]` with a smaller value:
- We're saying: "I found a better (smaller tail) subsequence of length `i+1`"
- This doesn't change the current LIS length
- But it **increases the chances** of extending subsequences in the future

### Example to Understand Replacement

Consider array: `[3, 5, 6, 2, 4]`

```
After [3, 5, 6]: temp = [3, 5, 6], LIS length = 3

Process 2:
- 2 < 6, so we can't extend
- But we replace temp[0] = 3 with 2
- temp = [2, 5, 6]
- Why? Because [2] is a better starting point than [3]

Process 4:
- 4 < 6, so we can't extend to length 4
- But 4 > 2, so we can create length-2 subsequence [2, 4]
- Replace temp[1] = 5 with 4
- temp = [2, 4, 6]
- This gives us subsequence [2, 4, 6] which is valid!
```

Without the replacement of 3→2, we couldn't have built [2, 4, 6].

---

## Why Binary Search?

Since `temp` is always sorted, we can use **binary search** to find the position for replacement in O(log n) time instead of O(n).

`bisect.bisect_left(temp, arr[index])` finds the **leftmost position** where `arr[index]` can be inserted to keep `temp` sorted.

---

## Important Notes

### What temp IS:
- ✅ A tool to find the **length** of LIS
- ✅ Always maintains sorted order
- ✅ Represents smallest tails for each length

### What temp IS NOT:
- ❌ NOT necessarily the actual LIS
- ❌ NOT all elements form a valid subsequence

**Example:** For `[10, 9, 2, 5, 3, 7, 101, 18]`
- Final `temp = [2, 3, 7, 18]`
- Actual LIS = `[2, 3, 7, 101]` or `[2, 3, 7, 18]`
- Both have length 4, which is what we need!

---

## Complete Code

```python
import bisect

def solve(n, arr):
    temp = [arr[0]]
    
    for index in range(1, n):
        if arr[index] > temp[-1]:
            # Extend the longest subsequence
            temp.append(arr[index])
        else:
            # Find position and replace for better tail
            lower_bound = bisect.bisect_left(temp, arr[index])
            temp[lower_bound] = arr[index]
    
    return len(temp)

# Test
arr = [10, 9, 2, 5, 3, 7, 101, 18]
n = len(arr)
print(solve(n, arr))  # Output: 4
```

---

## Complexity Analysis

**Time Complexity:** O(n log n)
- We iterate through array once: O(n)
- For each element, binary search takes: O(log n)
- Total: O(n log n)

**Space Complexity:** O(n)
- In worst case, `temp` can have all elements (e.g., already sorted array)

---

## Proof of Correctness

### Claim: Length of `temp` = Length of LIS

**Proof by Induction:**

**Base Case:** After first element, `temp = [arr[0]]`, length = 1 ✓

**Inductive Step:** Assume after processing first `k` elements, `len(temp)` = LIS length for those elements.

When processing `k+1`-th element:

1. **If `arr[k+1] > temp[-1]`:**
   - We can extend the LIS by 1
   - We append, so `len(temp)` increases by 1 ✓

2. **If `arr[k+1] <= temp[-1]`:**
   - Cannot extend current LIS
   - We replace some element, `len(temp)` stays same ✓
   - But we maintain better tails for future extensions

Therefore, `len(temp)` always equals LIS length.

---

## Comparison with Other Approaches

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n²) | O(n²) |
| Tabulation | O(n²) | O(n²) |
| **Binary Search** | **O(n log n)** | **O(n)** |

---

# Print Longest Increasing Subsequence

## Problem Statement

Given an array of n integers `arr`, return the Longest Increasing Subsequence (LIS) that is **Index-wise Lexicographically Smallest**.

The Longest Increasing Subsequence (LIS) is the longest subsequence where all elements are in strictly increasing order.

### Index-wise Lexicographically Smallest

A subsequence A1 is Index-wise Lexicographically Smaller than another subsequence A2 if, at the first position where A1 and A2 differ, the element in A1 appears **earlier in the array** than the corresponding element in A2.

### Examples

**Example 1:**
```
Input: arr = [10, 22, 9, 33, 21, 50, 41, 60, 80]
Output: [10, 22, 33, 50, 60, 80]
Explanation: The LIS is [10, 22, 33, 50, 60, 80] and it is lexicographically smallest.
```

**Example 2:**
```
Input: arr = [1, 3, 2, 4, 6, 5]
Output: [1, 3, 4, 6]
Explanation: Possible LIS sequences are [1, 3, 4, 6] and [1, 2, 4, 6]. 
Since [1, 3, 4, 6] is Index-wise Lexicographically Smaller, it is the result.
```

---

## Approach: DP with Parent Tracking

### Core Idea

We already know how to find the **length** of LIS using dynamic programming. Now, to **print** the actual subsequence, we need to track the path.

### Key Modifications:

1. **Track the parent**: For each index, store which previous index contributed to its LIS
2. **Find the ending index**: Find the index with maximum LIS length
3. **Backtrack**: Follow the parent pointers to reconstruct the sequence

---

## Algorithm Explanation

### Step 1: DP Array and Parent Tracking

```python
dp[i] = length of LIS ending at index i
parent[i] = the previous index in the LIS ending at i
```

Initially:
- `dp[i] = 1` (each element is a subsequence of length 1)
- `parent[i] = i` (pointing to itself, meaning no parent)

### Step 2: Fill DP and Parent Arrays

For each index `i`, check all previous indices `j`:
- If `arr[i] > arr[j]`, we can extend the subsequence ending at `j`
- If `1 + dp[j] > dp[i]`, update:
  - `dp[i] = 1 + dp[j]`
  - `parent[i] = j` (track that we came from index `j`)

### Step 3: Find the Ending Index

Find the index with maximum LIS length:
```python
maxIndex = index with maximum dp[i]
```

### Step 4: Backtrack to Construct LIS

Starting from `maxIndex`, follow parent pointers until we reach a node that points to itself:
```python
while parent[index] != index:
    add arr[index] to result
    index = parent[index]
add arr[index] to result  # Add the starting element
```

Reverse the result to get the correct order.

---

## Visual Example

Let's trace through `arr = [10, 9, 2, 5, 3, 7, 101, 18]`

### Initial State:
```
dp     = [1, 1, 1, 1, 1, 1, 1, 1]
parent = [0, 1, 2, 3, 4, 5, 6, 7]  (each points to itself)
```

### Step-by-Step Execution:

| Index | Element | Check Previous | Update | dp | parent |
|-------|---------|----------------|--------|----|----|
| 0 | 10 | - | - | [1, ...] | [0, ...] |
| 1 | 9 | 9 < 10 | No update | [1, 1, ...] | [0, 1, ...] |
| 2 | 2 | 2 < all | No update | [1, 1, 1, ...] | [0, 1, 2, ...] |
| 3 | 5 | 5 > 2 | dp[3]=2, parent[3]=2 | [1, 1, 1, 2, ...] | [0, 1, 2, 2, ...] |
| 4 | 3 | 3 > 2 | dp[4]=2, parent[4]=2 | [1, 1, 1, 2, 2, ...] | [0, 1, 2, 2, 2, ...] |
| 5 | 7 | 7 > 5 | dp[5]=3, parent[5]=3 | [1, 1, 1, 2, 2, 3, ...] | [0, 1, 2, 2, 2, 3, ...] |
| 6 | 101 | 101 > 7 | dp[6]=4, parent[6]=5 | [1, 1, 1, 2, 2, 3, 4, ...] | [0, 1, 2, 2, 2, 3, 5, ...] |
| 7 | 18 | 18 > 7 | dp[7]=4, parent[7]=5 | [1, 1, 1, 2, 2, 3, 4, 4] | [0, 1, 2, 2, 2, 3, 5, 5] |

### Final State:
```
dp     = [1, 1, 1, 2, 2, 3, 4, 4]
parent = [0, 1, 2, 2, 2, 3, 5, 5]
```

**Maximum length = 4** (at index 6 or 7)

**maxIndex = 6** (first occurrence of maximum)

### Backtracking from Index 6:

```
Start at index 6 (101):
  - temp = [101]
  - parent[6] = 5, move to index 5

At index 5 (7):
  - temp = [101, 7]
  - parent[5] = 3, move to index 3

At index 3 (5):
  - temp = [101, 7, 5]
  - parent[3] = 2, move to index 2

At index 2 (2):
  - temp = [101, 7, 5, 2]
  - parent[2] = 2, STOP (points to itself)

Reverse temp: [2, 5, 7, 101]
```

**Result:** `[2, 5, 7, 101]`

---

## Visual Parent Pointer Chain

```
Array:   [10,  9,  2,  5,  3,  7, 101, 18]
Index:    0    1   2   3   4   5   6    7

LIS ending at index 6 (101):
    6 → 5 → 3 → 2
  (101)(7) (5) (2)
  
Path: 2 → 5 → 7 → 101
```

---

## Complete Code with Comments

```python
def solve(n, arr):
    # dp[i] = length of LIS ending at index i
    dp = [1] * n
    
    # parent[i] = previous index in the LIS ending at i
    parent = {}
    
    # Fill dp and parent arrays
    for index in range(n):
        parent[index] = index  # Initially, each element points to itself
        
        for last_index in range(index):
            # Can we extend the subsequence ending at last_index?
            if arr[index] > arr[last_index]:
                length = 1 + dp[last_index]
                
                # Update if we found a longer subsequence
                if length > dp[index]:
                    dp[index] = length
                    parent[index] = last_index  # Track the parent
    
    # Find the index with maximum LIS length
    maxLength = 0
    maxIndex = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
            maxIndex = i
    
    # Backtrack to construct the LIS
    temp = []
    index = maxIndex
    
    # Follow parent pointers until we reach the starting element
    while parent[index] != index:
        temp.append(arr[index])
        index = parent[index]
    
    # Add the starting element
    temp.append(arr[index])
    
    # Reverse to get correct order
    ans = temp[::-1]
    return ans

# Test
arr = [10, 9, 2, 5, 3, 7, 101, 18]
n = len(arr)
print(solve(n, arr))  # Output: [2, 5, 7, 101]
```

---

## Why This Gives Lexicographically Smallest LIS

### Key Observation:

When we iterate from left to right (index 0 to n-1), and update `dp[i]` only when we find a **strictly better** length (`length > dp[i]`), we automatically ensure that:

1. We pick the **earliest possible** starting element
2. At each step, we extend from the **earliest possible** previous element

This greedy left-to-right approach naturally produces the index-wise lexicographically smallest LIS.

### Example: `[1, 3, 2, 4, 6, 5]`

```
When we reach index 3 (element 4):
- We can extend from index 1 (element 3): gives [1, 3, 4]
- We can extend from index 2 (element 2): gives [1, 2, 4]

Since we scan left to right, we first check index 1, then index 2.
Both give length 3, but we update only when length > dp[3].
After checking index 1, dp[3] = 3, so we DON'T update when checking index 2.

Result: We keep the path through index 1, giving us [1, 3, 4, 6]
```

---

## Detailed Trace for Example 2

`arr = [1, 3, 2, 4, 6, 5]`

| Index | Element | Process | dp | parent |
|-------|---------|---------|----|----|
| 0 | 1 | Base | [1, ...] | [0, ...] |
| 1 | 3 | 3 > 1, extend from 0 | [1, 2, ...] | [0, 0, ...] |
| 2 | 2 | 2 > 1, extend from 0 | [1, 2, 2, ...] | [0, 0, 0, ...] |
| 3 | 4 | 4 > 3, extend from 1 (dp=3)<br>4 > 2, extend from 2 (dp=3, NO update) | [1, 2, 2, 3, ...] | [0, 0, 0, 1, ...] |
| 4 | 6 | 6 > 4, extend from 3 | [1, 2, 2, 3, 4, ...] | [0, 0, 0, 1, 3, ...] |
| 5 | 5 | 5 > 4, extend from 3 (dp=4, NO update) | [1, 2, 2, 3, 4, 4] | [0, 0, 0, 1, 3, 3] |

**maxIndex = 4** (first occurrence of max length 4)

**Backtrack from index 4:**
```
4 → 3 → 1 → 0
(6) (4) (3) (1)

Result: [1, 3, 4, 6]
```

---

## Complexity Analysis

**Time Complexity:** O(n²)
- Two nested loops: outer loop runs n times, inner loop runs up to n times
- Finding max index: O(n)
- Backtracking: O(n) in worst case

**Space Complexity:** O(n)
- `dp` array: O(n)
- `parent` dictionary: O(n)
- `temp` array: O(n)

---
# Largest Divisible Subset

## Problem Statement

Given an array `nums` of positive integers, the task is to find the **largest subset** such that every pair `(a, b)` of elements in the subset satisfies `a % b == 0` or `b % a == 0`.

Return the subset in any order. If there are multiple solutions, return any one of them.

### Examples

**Example 1:**
```
Input: nums = [3, 5, 10, 20]
Output: [5, 10, 20]
Explanation: The subset [5, 10, 20] satisfies the divisibility condition:
- 10 % 5 == 0 
- 20 % 10 == 0
```

**Example 2:**
```
Input: nums = [16, 8, 2, 4, 32]
Output: [2, 4, 8, 16, 32]
Explanation: The entire array forms a divisible subset:
- 4 % 2 == 0
- 8 % 4 == 0
- 16 % 8 == 0
- 32 % 16 == 0
```

---

## Key Concepts

### Subset vs Subsequence

- **Subsequence:** Elements must follow the order of the original array
- **Subset:** No constraint on the order of elements (order doesn't matter)

### Divisible Subset

A divisible subset is one where if we pick any two elements `i` and `j`, then either:
- `arr[i] % arr[j] == 0`, OR
- `arr[j] % arr[i] == 0`

**Example:** `[16, 8, 4]` is a divisible subset because:
- 16 % 8 == 0
- 8 % 4 == 0
- 16 % 4 == 0

---

## The Key Insight

### Why Sorting Helps

If we sort the array, and element `a` divides element `b`, and `b` divides element `c`, then **`a` automatically divides `c`**.

**Mathematical Property:**
```
If a | b and b | c, then a | c (transitivity of divisibility)
```

**Example:**
```
Sorted: [2, 4, 8, 16]

- 4 % 2 == 0 (4 is divisible by 2)
- 8 % 4 == 0 (8 is divisible by 4)
- Therefore, 8 % 2 == 0 automatically! (transitivity)
```

### Transformation to LIS-like Problem

After sorting:
- We only need to check if `arr[i] % arr[j] == 0` where `j < i`
- We don't need to check all pairs explicitly
- This becomes similar to Longest Increasing Subsequence, but with divisibility condition

---

## Approach 1: Find Length of Largest Subset

### Algorithm

1. **Sort the array** to leverage transitivity
2. Use DP where `dp[i]` = length of largest divisible subset ending at index `i`
3. For each index, check all previous indices
4. If `arr[index] % arr[last_index] == 0`, we can extend the subset

### Code

```python
def lengthOfLargestSubset(n, arr):
    # Sort to leverage transitivity property
    arr.sort()
    
    # dp[i] = length of largest divisible subset ending at index i
    dp = [1] * n
    
    for index in range(n):
        for last_index in range(index):
            # Check if current element is divisible by previous element
            if arr[index] % arr[last_index] == 0:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    
    # Find maximum length
    maxLength = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
    
    return maxLength
```

---

## Visual Example: Length Calculation

Let's trace `arr = [3, 5, 10, 20]`

### After Sorting: `[3, 5, 10, 20]`

| Index | Element | Check Previous | Divisibility | Update | dp |
|-------|---------|----------------|--------------|--------|-----|
| 0 | 3 | - | - | Base | [1, ...] |
| 1 | 5 | 5 % 3 = 2 | ❌ Not divisible | No update | [1, 1, ...] |
| 2 | 10 | 10 % 3 = 1<br>10 % 5 = 0 | ❌<br>✅ | dp[2] = 1 + dp[1] = 2 | [1, 1, 2, ...] |
| 3 | 20 | 20 % 3 = 2<br>20 % 5 = 0<br>20 % 10 = 0 | ❌<br>✅ (dp=2)<br>✅ (dp=3) | dp[3] = 1 + dp[2] = 3 | [1, 1, 2, 3] |

**Final:** `dp = [1, 1, 2, 3]`  
**Maximum Length = 3**

### Why 20 % 10 gives better result?

```
From index 1 (5): dp[1] = 1, so extending gives length = 2
From index 2 (10): dp[2] = 2, so extending gives length = 3 ✓ Better!
```

---

## Approach 2: Print the Largest Subset

### Algorithm Enhancement

Same as finding length, but also track **parent pointers** to reconstruct the subset.

### Code

```python
def printLargestSubset(n, arr):
    # Sort the array
    arr.sort()
    
    # dp[i] = length of largest divisible subset ending at index i
    dp = [1] * n
    
    # parent[i] = previous index in the largest subset ending at i
    parent = {}
    
    for index in range(n):
        parent[index] = index  # Initially points to itself
        
        for last_index in range(index):
            # Check divisibility
            if arr[index] % arr[last_index] == 0:
                length = 1 + dp[last_index]
                
                # Update if we found a longer subset
                if length > dp[index]:
                    dp[index] = length
                    parent[index] = last_index  # Track parent
    
    # Find index with maximum length
    maxLength = 0
    maxIndex = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
            maxIndex = i
    
    # Backtrack to construct the subset
    temp = []
    index = maxIndex
    
    while parent[index] != index:
        temp.append(arr[index])
        index = parent[index]
    
    temp.append(arr[index])  # Add the starting element
    
    # Reverse to get correct order
    ans = temp[::-1]
    return ans
```

---

## Detailed Trace: Print Subset

### Array: `[3, 5, 10, 20]`

After sorting: `[3, 5, 10, 20]`

### Step-by-Step Execution:

**Index 0 (3):**
```
dp[0] = 1
parent[0] = 0
```

**Index 1 (5):**
```
Check index 0: 5 % 3 = 2 ❌
dp[1] = 1
parent[1] = 1
```

**Index 2 (10):**
```
Check index 0: 10 % 3 = 1 ❌
Check index 1: 10 % 5 = 0 ✅
  - length = 1 + dp[1] = 2
  - dp[2] = 2
  - parent[2] = 1
```

**Index 3 (20):**
```
Check index 0: 20 % 3 = 2 ❌
Check index 1: 20 % 5 = 0 ✅
  - length = 1 + dp[1] = 2
  - dp[3] = 2
  - parent[3] = 1
Check index 2: 20 % 10 = 0 ✅
  - length = 1 + dp[2] = 3
  - dp[3] = 3 (updated!)
  - parent[3] = 2 (updated!)
```

### Final State:
```
dp     = [1, 1, 2, 3]
parent = [0, 1, 1, 2]
```

**maxLength = 3 at maxIndex = 3**

### Backtracking from Index 3:

```
Start at index 3 (20):
  - temp = [20]
  - parent[3] = 2, move to index 2

At index 2 (10):
  - temp = [20, 10]
  - parent[2] = 1, move to index 1

At index 1 (5):
  - temp = [20, 10, 5]
  - parent[1] = 1, STOP (self-pointing)

Reverse: [5, 10, 20]
```

**Result:** `[5, 10, 20]`

---

## Visual Parent Pointer Chain

```
Array:   [3,  5, 10, 20]
Index:    0   1   2   3

Parent:   0   1   1   2
          ↓   ↓   ↑   ↑
          3   5 ←─10←─20

Largest subset ending at index 3:
  3 → 2 → 1
 (20)(10)(5)

Result: [5, 10, 20]
```

---

## Example 2 Trace

### Array: `[16, 8, 2, 4, 32]`

**After Sorting:** `[2, 4, 8, 16, 32]`

| Index | Element | Divisibility Checks | dp | parent |
|-------|---------|---------------------|----|----|
| 0 | 2 | - | 1 | 0 |
| 1 | 4 | 4 % 2 = 0 ✅ | 2 | 0 |
| 2 | 8 | 8 % 2 = 0 ✅<br>8 % 4 = 0 ✅ (better) | 3 | 1 |
| 3 | 16 | 16 % 8 = 0 ✅ (best) | 4 | 2 |
| 4 | 32 | 32 % 16 = 0 ✅ (best) | 5 | 3 |

**Final:**
```
dp     = [1, 2, 3, 4, 5]
parent = [0, 0, 1, 2, 3]
```

**Backtrack from index 4:**
```
4 → 3 → 2 → 1 → 0
(32)(16)(8)(4)(2)

Result: [2, 4, 8, 16, 32]
```

---

## Why Sorting is Critical

### Without Sorting:

Array: `[10, 5, 20, 3]`

```
- At index 2 (20): We check 10 and 5
- 20 % 10 = 0 ✅
- But we don't know that 10 % 5 = 0
- We might miss the chain [5, 10, 20]
```

### With Sorting:

Array: `[3, 5, 10, 20]`

```
- At index 2 (10): We know 10 % 5 = 0
- At index 3 (20): We know 20 % 10 = 0
- Transitivity guarantees 20 % 5 = 0
- We correctly build [5, 10, 20]
```

---

## Mathematical Proof: Why This Works

### Claim: If array is sorted and we build chains using divisibility, the result is valid.

**Proof:**

1. **Base case:** Single element is always a valid divisible subset ✓

2. **Inductive step:** Assume we have a valid subset ending at index `j`: `[a₁, a₂, ..., aⱼ]`
   - All pairs in this subset satisfy divisibility
   - Array is sorted, so `a₁ < a₂ < ... < aⱼ`

3. **Adding element at index `i` (i > j):**
   - We check: `arr[i] % arr[j] == 0`
   - By transitivity: if `arr[j] % arr[k] == 0` for any `k < j`, then `arr[i] % arr[k] == 0`
   - Therefore, adding `arr[i]` maintains the divisibility property ✓

---

## Complete Code

```python
def lengthOfLargestSubset(n, arr):
    arr.sort()
    dp = [1] * n
    
    for index in range(n):
        for last_index in range(index):
            if arr[index] % arr[last_index] == 0:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    
    maxLength = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
    
    return maxLength

def printLargestSubset(n, arr):
    arr.sort()
    dp = [1] * n
    parent = {}
    
    for index in range(n):
        parent[index] = index
        for last_index in range(index):
            if arr[index] % arr[last_index] == 0:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
                    parent[index] = last_index
    
    maxLength = 0
    maxIndex = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
            maxIndex = i
    
    temp = []
    index = maxIndex
    while parent[index] != index:
        temp.append(arr[index])
        index = parent[index]
    temp.append(arr[index])
    
    ans = temp[::-1]
    return ans

# Test
arr = [3, 5, 10, 20]
n = len(arr)
print(lengthOfLargestSubset(n, arr))    # Output: 3
print(printLargestSubset(n, arr))       # Output: [5, 10, 20]
```

---

## Complexity Analysis

**Time Complexity:** O(n² + n log n)
- Sorting: O(n log n)
- Two nested loops: O(n²)
- Finding max and backtracking: O(n)
- **Overall: O(n²)**

**Space Complexity:** O(n)
- `dp` array: O(n)
- `parent` dictionary: O(n)
- `temp` array: O(n) in worst case

---

## Comparison with LIS

| Aspect | LIS | Largest Divisible Subset |
|--------|-----|--------------------------|
| **Order matters** | Yes (subsequence) | No (subset) |
| **Preprocessing** | Not required | Sort array |
| **Condition** | `arr[i] > arr[j]` | `arr[i] % arr[j] == 0` |
| **Property used** | Ordering | Divisibility transitivity |
| **DP Definition** | Same structure | Same structure |

---

# Longest String Chain

## Problem Statement

You are given an array of words where each word consists of lowercase English letters.

**wordA** is a **predecessor** of **wordB** if and only if we can insert exactly one letter anywhere in wordA without changing the order of the other characters to make it equal to wordB.

- For example, `"abc"` is a predecessor of `"abac"`, while `"cba"` is not a predecessor of `"bcad"`.

A **word chain** is a sequence of words `[word1, word2, ..., wordk]` with `k >= 1`, where word1 is a predecessor of word2, word2 is a predecessor of word3, and so on.

Return the length of the longest possible word chain with words chosen from the given list of words.

### Examples

**Example 1:**
```
Input: words = ["a", "ab", "abc", "abcd", "abcde"]
Output: 5
Explanation: The longest chain is ["a", "ab", "abc", "abcd", "abcde"].
Each word in the chain is formed by adding exactly one character to the previous word.
```

**Example 2:**
```
Input: words = ["dog", "dogs", "dots", "dot", "d", "do"]
Output: 4
Explanation: The longest chain is ["d", "do", "dot", "dots"].
Each word is formed by inserting one character into the previous word.
```

---

## Understanding the Problem

### Key Points:

1. A string chain is a **subsequence** of the words array
2. Every element (except the first) can be formed by **inserting exactly one character** into the previous element
3. The character can be inserted at **any position**
4. The first element can be any string from the array

### Predecessor Relationship Examples:

```
✅ "abc" → "abac"  (insert 'a' at position 2)
✅ "abc" → "abcd"  (insert 'd' at end)
✅ "dog" → "dogs"  (insert 's' at end)
✅ "dot" → "dots"  (insert 's' at end)

❌ "cba" → "bcad"  (not a valid predecessor, order changes)
❌ "ab" → "cd"     (completely different)
❌ "abc" → "abc"   (no character added)
```

---

## The Key Insight

### Similarity to Longest Increasing Subsequence (LIS)

This problem is similar to LIS, but instead of comparing numeric values, we:
1. **Sort by length** - Shorter words must come before longer words
2. **Check predecessor relationship** - Instead of `arr[i] > arr[j]`, we check if `arr[i]` can be formed by adding one character to `arr[j]`

### Why Sort by Length?

If word A is a predecessor of word B, then:
```
len(B) = len(A) + 1
```

By sorting by length, we ensure that potential predecessors always come before their successors.

---

## Checking Predecessor Relationship

### The Compare Function

We need to check if `word1` can be formed by inserting exactly one character into `word2`.

**Requirements:**
1. `len(word1) == len(word2) + 1` (exactly one character more)
2. We can match all characters of `word2` with characters in `word1` in order
3. Exactly one character in `word1` is "extra"

### Algorithm:

```python
def compare(word1, word2):
    n, m = len(word1), len(word2)
    
    # word1 must be exactly 1 character longer
    if n != m + 1:
        return False
    
    i, j = 0, 0
    
    while i < n:
        if j < m and word1[i] == word2[j]:
            # Characters match, move both pointers
            i += 1
            j += 1
        else:
            # Character in word1 is extra, skip it
            i += 1
    
    # Check if we matched all characters of word2
    if i == n and j == m:
        return True
    
    return False
```

### Visual Example:

Comparing `"abac"` with `"abc"`:

```
word1 = "abac"  (length 4)
word2 = "abc"   (length 3)

Step 1: i=0, j=0
  word1[0] = 'a', word2[0] = 'a' ✓ Match
  i=1, j=1

Step 2: i=1, j=1
  word1[1] = 'b', word2[1] = 'b' ✓ Match
  i=2, j=2

Step 3: i=2, j=2
  word1[2] = 'a', word2[2] = 'c' ✗ No match
  Skip word1[2], i=3, j=2

Step 4: i=3, j=2
  word1[3] = 'c', word2[2] = 'c' ✓ Match
  i=4, j=3

Result: i==n (4==4) and j==m (3==3) → True ✓
```

---

## Approach 1: Find Length of Longest Chain

### Algorithm

1. **Sort by length** - Ensures predecessors come before successors
2. Use DP where `dp[i]` = length of longest chain ending at word `i`
3. For each word, check all previous words
4. If previous word is a predecessor, extend the chain

### Code

```python
def compare(word1, word2):
    n, m = len(word1), len(word2)
    if n != m + 1:
        return False
    i, j = 0, 0
    while i < n:
        if j < m and word1[i] == word2[j]:
            i += 1
            j += 1
        else:
            i += 1
    if i == n and j == m:
        return True
    return False

def lengthOfLongestChain(n, arr):
    # Sort by length to ensure predecessors come first
    arr.sort(key=len)
    
    # dp[i] = length of longest chain ending at index i
    dp = [1] * n
    
    for index in range(n):
        for last_index in range(index):
            # Check if arr[last_index] is a predecessor of arr[index]
            if compare(arr[index], arr[last_index]):
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    
    # Find maximum length
    maxLength = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
    
    return maxLength
```

---

## Visual Trace: Length Calculation

### Example: `words = ["a", "ab", "abc", "abcd", "abcde"]`

**After sorting by length:** Already sorted

| Index | Word | Check Previous | Predecessor? | Update | dp |
|-------|------|----------------|--------------|--------|-----|
| 0 | "a" | - | - | Base | [1, ...] |
| 1 | "ab" | compare("ab", "a") | ✅ Yes | dp[1] = 1+1 = 2 | [1, 2, ...] |
| 2 | "abc" | compare("abc", "a")<br>compare("abc", "ab") | ❌ No (length)<br>✅ Yes | dp[2] = 1+2 = 3 | [1, 2, 3, ...] |
| 3 | "abcd" | compare("abcd", "abc") | ✅ Yes (best) | dp[3] = 1+3 = 4 | [1, 2, 3, 4, ...] |
| 4 | "abcde" | compare("abcde", "abcd") | ✅ Yes (best) | dp[4] = 1+4 = 5 | [1, 2, 3, 4, 5] |

**Final:** `dp = [1, 2, 3, 4, 5]`  
**Maximum Length = 5**

---

## Approach 2: Print the Longest Chain

### Algorithm Enhancement

Add parent tracking to reconstruct the actual chain.

### Code

```python
def printLongestChain(n, arr):
    # Sort by length first
    arr.sort(key=len)
    
    # dp[i] = length of longest chain ending at index i
    dp = [1] * n
    
    # parent[i] = previous index in the chain ending at i
    parent = {}
    
    for index in range(n):
        parent[index] = index  # Initially points to itself
        
        for last_index in range(index):
            # Check if arr[last_index] is a predecessor
            if compare(arr[index], arr[last_index]):
                length = 1 + dp[last_index]
                
                # Update if we found a longer chain
                if length > dp[index]:
                    dp[index] = length
                    parent[index] = last_index  # Track parent
    
    # Find index with maximum length
    maxLength = 0
    maxIndex = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
            maxIndex = i
    
    # Backtrack to construct the chain
    temp = []
    index = maxIndex
    
    while parent[index] != index:
        temp.append(arr[index])
        index = parent[index]
    
    temp.append(arr[index])  # Add starting word
    
    # Reverse to get correct order
    ans = temp[::-1]
    return ans
```

---

## Detailed Trace: Print Chain

### Example: `words = ["dog", "dogs", "dots", "dot", "d", "do"]`

**After sorting by length:** `["d", "do", "dog", "dot", "dogs", "dots"]`

### Step-by-Step Execution:

**Index 0 ("d"):**
```
dp[0] = 1
parent[0] = 0
```

**Index 1 ("do"):**
```
Check "d": compare("do", "d") = True ✅
  length = 1 + 1 = 2
  dp[1] = 2, parent[1] = 0
```

**Index 2 ("dog"):**
```
Check "d": compare("dog", "d") = False ❌ (length diff = 2)
Check "do": compare("dog", "do") = True ✅
  length = 1 + 2 = 3
  dp[2] = 3, parent[2] = 1
```

**Index 3 ("dot"):**
```
Check "d": compare("dot", "d") = False ❌
Check "do": compare("dot", "do") = True ✅
  length = 1 + 2 = 3
  dp[3] = 3, parent[3] = 1
```

**Index 4 ("dogs"):**
```
Check "d", "do": False (length diff = 3, 2)
Check "dog": compare("dogs", "dog") = True ✅
  length = 1 + 3 = 4
  dp[4] = 4, parent[4] = 2
Check "dot": compare("dogs", "dot") = False
```

**Index 5 ("dots"):**
```
Check "d", "do": False
Check "dog": compare("dots", "dog") = False ❌
Check "dot": compare("dots", "dot") = True ✅
  length = 1 + 3 = 4
  dp[5] = 4, parent[5] = 3
Check "dogs": compare("dots", "dogs") = False
```

### Final State:
```
Words:  ["d", "do", "dog", "dot", "dogs", "dots"]
Index:    0    1     2     3      4       5
dp:      [1,   2,    3,    3,     4,      4]
parent:  [0,   0,    1,    1,     2,      3]
```

**maxLength = 4 at maxIndex = 4** (first occurrence)

### Backtracking from Index 4:

```
Start at index 4 ("dogs"):
  temp = ["dogs"]
  parent[4] = 2, move to index 2

At index 2 ("dog"):
  temp = ["dogs", "dog"]
  parent[2] = 1, move to index 1

At index 1 ("do"):
  temp = ["dogs", "dog", "do"]
  parent[1] = 0, move to index 0

At index 0 ("d"):
  temp = ["dogs", "dog", "do", "d"]
  parent[0] = 0, STOP

Reverse: ["d", "do", "dog", "dogs"]
```

**Result:** `["d", "do", "dog", "dogs"]`

---

## Visual Parent Pointer Chain

```
Words:   ["d", "do", "dog", "dot", "dogs", "dots"]
Index:     0    1     2     3      4       5

Parent Chain for "dogs" (index 4):
  4 → 2 → 1 → 0
(dogs)(dog)(do)(d)

Result: ["d", "do", "dog", "dogs"]
```

---

## Why Compare Function Works

### Case 1: Valid Predecessor
```
word1 = "dogs"
word2 = "dog"

i=0, j=0: 'd'=='d' → match, i=1, j=1
i=1, j=1: 'o'=='o' → match, i=2, j=2
i=2, j=2: 'g'=='g' → match, i=3, j=3
i=3, j=3: j<m is False, skip 's', i=4

Final: i==4, j==3 → True ✅
```

### Case 2: Invalid - Different Characters
```
word1 = "dots"
word2 = "dog"

i=0, j=0: 'd'=='d' → match, i=1, j=1
i=1, j=1: 'o'=='o' → match, i=2, j=2
i=2, j=2: 't'!='g' → skip 't', i=3, j=2
i=3, j=2: 's'!='g' → skip 's', i=4, j=2

Final: i==4, j==2 (not 3) → False ❌
```

### Case 3: Invalid - Length Difference
```
word1 = "abcd"
word2 = "ab"

len(word1) = 4, len(word2) = 2
Difference = 2 (not 1) → False ❌
```

---

## Important Notes

### About Sorting

The code shows two sorts:
```python
arr.sort(key=len)  # Sort by length
arr.sort()         # Sort lexicographically
```

**Note:** The second sort `arr.sort()` is actually **not necessary** for correctness. However, it can help achieve lexicographically smallest chain when there are multiple valid chains of same length.

**Recommendation:** For just finding the longest chain, only `arr.sort(key=len)` is needed.

---

## Complete Code

```python
def compare(word1, word2):
    n, m = len(word1), len(word2)
    if n != m + 1:
        return False
    i, j = 0, 0
    while i < n:
        if j < m and word1[i] == word2[j]:
            i += 1
            j += 1
        else:
            i += 1
    if i == n and j == m:
        return True
    return False

def lengthOfLongestChain(n, arr):
    arr.sort(key=len)
    dp = [1] * n
    
    for index in range(n):
        for last_index in range(index):
            if compare(arr[index], arr[last_index]):
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    
    maxLength = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
    
    return maxLength

def printLongestChain(n, arr):
    arr.sort(key=len)
    dp = [1] * n
    parent = {}
    
    for index in range(n):
        parent[index] = index
        for last_index in range(index):
            if compare(arr[index], arr[last_index]):
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
                    parent[index] = last_index
    
    maxLength = 0
    maxIndex = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
            maxIndex = i
    
    temp = []
    index = maxIndex
    while parent[index] != index:
        temp.append(arr[index])
        index = parent[index]
    temp.append(arr[index])
    
    ans = temp[::-1]
    return ans

# Test
arr = ["a", "ab", "abc", "abcd", "abcde"]
n = len(arr)
print(lengthOfLongestChain(n, arr))    # Output: 5
print(printLongestChain(n, arr))       # Output: ['a', 'ab', 'abc', 'abcd', 'abcde']
```

---

## Complexity Analysis

**Time Complexity:** O(n² × L)
- Sorting: O(n log n)
- Two nested loops: O(n²)
- Compare function in each iteration: O(L) where L is average word length
- **Overall: O(n² × L)**

**Space Complexity:** O(n)
- `dp` array: O(n)
- `parent` dictionary: O(n)
- `temp` array: O(n) in worst case

---

## Comparison with Related Problems

| Problem | Sorting | Condition | Key Difference |
|---------|---------|-----------|----------------|
| **LIS** | Optional | `arr[i] > arr[j]` | Numeric comparison |
| **Divisible Subset** | Required | `arr[i] % arr[j] == 0` | Mathematical property |
| **String Chain** | Required (by length) | One character insertion | String manipulation |

---
