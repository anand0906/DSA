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
