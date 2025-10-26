# Subset Sum Problem

## Problem Statement

Given an array `arr` of `n` integers and an integer `target`, determine if there is a subset of the given array with a sum equal to the given target.

### Examples

**Example 1:**
```
Input: arr = [1, 2, 7, 3], target = 6
Output: True
Explanation: There is a subset (1, 2, 3) with sum 6.
```

**Example 2:**
```
Input: arr = [2, 3, 5], target = 6
Output: False
Explanation: There is no subset with sum 6.
```

---

## Problem Definition

We are given an array and a value `k`. We need to check whether there is a subsequence of the array with sum equal to `k`.

### Programmatic Representation

- **Input**: Array `arr` (0-based indexing) and target value `k`
- **Output**: `True` or `False`
- **Function Definition**: `f(index, target)` → checks whether target sum subsequence is present from index 0 to `index`
- **Goal**: Find `f(n-1, target)`, where `n = len(arr)` and `target = k`

---

## Base Cases

1. **If target = 0**: Answer is always `True` (empty subsequence has sum 0)
   ```
   f(index, 0) = True
   ```

2. **If index = 0**: Array contains only one element
   ```
   f(0, target) = (arr[0] == target)
   ```

---

## Recurrence Relation

For subsequence generation, we have two choices at each index:
1. **Include** the current element: Reduce target and move to the previous index
2. **Exclude** the current element: Keep target same and move to the previous index

### Formula:
```
f(index, target) = f(index-1, target-arr[index]) OR f(index-1, target)
```

Where:
- `f(index-1, target-arr[index])` → Include current element (only if `arr[index] <= target`)
- `f(index-1, target)` → Exclude current element

**Note**: We can only include the current element if `arr[index] <= target`

---

## Recursion Tree

For `arr = [1, 2, 7, 3]` and `target = 6`:

```
                          f(3, 6)
                         /        \
                   (include 3)   (exclude 3)
                      /              \
                  f(2, 3)          f(2, 6)
                  /     \          /     \
            f(1,3)    f(1,-4)  f(1,-1) f(1,6)
            /   \        |        |      /  \
        f(0,2) f(0,3)  False   False  f(0,5) f(0,6)
         |      |                       |      |
       False  False                   False  False
       
(Tree continues with many branches...)

Key observation: Many subproblems like f(1,3), f(2,3) are recalculated multiple times
```

---

## Solution Approaches

### 1. Recursive Solution

```python
def solve(n, arr, index, target):
    # Base case: if target becomes 0, we found a valid subset
    if target == 0:
        return True
    
    # Base case: if we're at the first element
    if index == 0:
        return arr[0] == target
    
    # Try including current element (only if it doesn't exceed target)
    if arr[index] <= target:
        include = solve(n, arr, index-1, target-arr[index])
    else:
        include = False
    
    # Try excluding current element
    exclude = solve(n, arr, index-1, target)
    
    return include or exclude
```

**Time Complexity**: O(2^n) - Each element has 2 choices (include/exclude)  
**Space Complexity**: O(n) - Recursion stack depth

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion**:
- Identified overlapping subproblems: same `(index, target)` combinations are computed multiple times
- Added a 2D memo table: `memo[index][target]` to store results
- Before computing, check if result already exists in memo
- After computing, store result in memo before returning

```python
def solve_memo(n, arr, index, target, memo):
    # Check if already computed
    if memo[index][target] != None:
        return memo[index][target]
    
    if target == 0:
        return True
    
    if index == 0:
        return arr[0] == target
    
    if arr[index] <= target:
        include = solve_memo(n, arr, index-1, target-arr[index], memo)
    else:
        include = False
    
    exclude = solve_memo(n, arr, index-1, target, memo)
    
    # Store result in memo before returning
    memo[index][target] = include or exclude
    return memo[index][target]
```

**Time Complexity**: O(n × target) - Each subproblem computed once  
**Space Complexity**: O(n × target) - Memo table + O(n) recursion stack

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization**:
- Eliminated recursion by using iterative approach
- Created 2D DP table: `dp[index][target]`
- Filled base cases first (target = 0 and index = 0)
- Filled table iteratively from smaller subproblems to larger ones
- Direction: index 0→n-1, target 0→targetSum

```python
def tabulation(n, arr, targetSum):
    # Create DP table
    dp = [[False]*(targetSum+1) for _ in range(n)]
    
    # Base case: target = 0 is always achievable
    for index in range(n):
        dp[index][0] = True
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp[0][arr[0]] = True
    
    # Fill the DP table
    for index in range(1, n):
        for target in range(1, targetSum+1):
            if arr[index] <= target:
                include = dp[index-1][target-arr[index]]
            else:
                include = False
            
            exclude = dp[index-1][target]
            dp[index][target] = include or exclude
    
    return dp[n-1][targetSum]
```

**Time Complexity**: O(n × target)  
**Space Complexity**: O(n × target)

#### Tabulation Table Example

For `arr = [1, 2, 3]` and `target = 4`:

| index/target | 0 | 1 | 2 | 3 | 4 |
|--------------|---|---|---|---|---|
| 0 (arr[0]=1) | T | T | F | F | F |
| 1 (arr[1]=2) | T | T | T | T | F |
| 2 (arr[2]=3) | T | T | T | T | T |

**Explanation**:
- Row 0: Can only make sum 0 (empty) and sum 1 (using element 1)
- Row 1: Can make 0, 1 (using first element), 2 (using second element), 3 (using 1+2)
- Row 2: Can make all sums from 0 to 4 (4 = 1+3)

---

### 4. Space Optimized Solution

**Conversion from Tabulation**:
- Observed that we only need previous row (`dp[index-1]`) to compute current row (`dp[index]`)
- Reduced 2D array to two 1D arrays: `dp_prev` and `dp_curr`
- After processing each index, copy `dp_curr` to `dp_prev`
- Space reduced from O(n × target) to O(target)

```python
def optimized(n, arr, targetSum):
    # Use two arrays instead of 2D table
    dp_curr = [False]*(targetSum+1)
    dp_prev = [False]*(targetSum+1)
    
    # Base case: target = 0
    dp_prev[0] = True
    dp_curr[0] = True
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = True
    
    # Fill the arrays
    for index in range(1, n):
        for target in range(1, targetSum+1):
            if arr[index] <= target:
                include = dp_prev[target-arr[index]]
            else:
                include = False
            
            exclude = dp_prev[target]
            dp_curr[target] = include or exclude
        
        # Copy current to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[targetSum]
```

**Time Complexity**: O(n × target)  
**Space Complexity**: O(target) - Only two 1D arrays

---

## Complete Code

```python
arr = [1, 2, 7, 3]
n = len(arr)
target = 6

# Test all approaches
print(solve(n, arr, n-1, target))
print(solve_memo(n, arr, n-1, target, [[None]*(target+1) for _ in range(n)]))
print(tabulation(n, arr, target))
print(optimized(n, arr, target))
```

**Output**: `True` (for all approaches)

---

## Complexity Summary

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n × target) | O(n × target) |
| Tabulation | O(n × target) | O(n × target) |
| Space Optimized | O(n × target) | O(target) |

---
