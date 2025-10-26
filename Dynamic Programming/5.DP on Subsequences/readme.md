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

# Partition Equal Subset Sum

## Problem Statement

Given an array `arr` of `n` integers, return `true` if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal, else return `false`.

### Examples

**Example 1:**
```
Input: arr = [1, 10, 21, 10]
Output: True
Explanation: The array can be partitioned as [1, 10, 10] and [21].
```

**Example 2:**
```
Input: arr = [1, 2, 3, 5]
Output: False
Explanation: The array cannot be partitioned into equal sum subsets.
```

---

## Problem Definition

We are given an array and need to check whether we can divide it into two subsequences such that their sums are equal.

### Key Insight

This problem can be reduced to the **Subset Sum Problem**!

Let's say:
- `totalSum = sum(arr)`
- `half = totalSum // 2`

**Observations**:
1. If we can partition the array into two equal sum subsets, each subset must have sum = `totalSum / 2`
2. If we find **one** subset with sum = `half`, the remaining elements automatically form the other subset with sum = `half`
3. If `totalSum` is **odd**, it's impossible to partition into two equal sums

**Therefore**: Check if there exists a subset with sum equal to `half` of total sum.

---

## Problem Reduction

```
Original Problem: Partition array into two equal sum subsets
                         ↓
Reduced Problem: Find if subset exists with sum = totalSum/2
                         ↓
                  Subset Sum Problem!
```

### Why This Works

For `arr = [1, 10, 21, 10]`:
- `totalSum = 42`
- `half = 21`
- If we find subset `[21]` with sum = 21
- Remaining elements `[1, 10, 10]` automatically have sum = 42 - 21 = 21 ✓

---

## Edge Case: Odd Total Sum

**Important Check**: If `totalSum` is odd, return `False` immediately.

```python
if totalSum & 1:  # Bitwise check if odd
    return False
```

**Why?** 
- To partition into two equal sums, each must be `totalSum / 2`
- If `totalSum` is odd, `totalSum / 2` is not an integer
- Example: `arr = [1, 2, 3, 5]`, `totalSum = 11`, cannot divide 11 into two equal integers

---

## Solution: Space Optimized Subset Sum

Since this is a direct application of Subset Sum, we use the space-optimized approach.

```python
def optimized(n, arr, targetSum):
    # Two arrays to track current and previous states
    dp_curr = [False]*(targetSum+1)
    dp_prev = [False]*(targetSum+1)
    
    # Base case: target = 0 is always achievable (empty subset)
    dp_prev[0] = True
    dp_curr[0] = True
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = True
    
    # Fill the arrays for each element
    for index in range(1, n):
        for target in range(1, targetSum+1):
            # Include current element (if it doesn't exceed target)
            if arr[index] <= target:
                include = dp_prev[target-arr[index]]
            else:
                include = False
            
            # Exclude current element
            exclude = dp_prev[target]
            
            # Current state is True if either include or exclude works
            dp_curr[target] = include or exclude
        
        # Move current to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[targetSum]
```

**Time Complexity**: O(n × targetSum) = O(n × sum/2) = O(n × sum)  
**Space Complexity**: O(targetSum) = O(sum)

---

## Complete Solution

```python
def optimized(n, arr, targetSum):
    dp_curr = [False]*(targetSum+1)
    dp_prev = [False]*(targetSum+1)
    dp_prev[0] = True
    dp_curr[0] = True
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = True
    for index in range(1, n):
        for target in range(1, targetSum+1):
            if arr[index] <= target:
                include = dp_prev[target-arr[index]]
            else:
                include = False
            exclude = dp_prev[target]
            dp_curr[target] = include or exclude
        dp_prev = dp_curr.copy()
    return dp_prev[targetSum]

# Main logic
n = int(input())
arr = list(map(int, input().split()))
totalSum = sum(arr)

# Check if totalSum is odd
if totalSum & 1:
    print(False)
else:
    half = totalSum // 2
    print(optimized(n, arr, half))
```

---

## Example Walkthrough

### Example 1: `arr = [1, 10, 21, 10]`

1. Calculate `totalSum = 1 + 10 + 21 + 10 = 42`
2. Check if odd: `42 & 1 = 0` (even) ✓
3. Calculate `half = 42 // 2 = 21`
4. Find if subset with sum = 21 exists
5. **Result**: `True` (subset `[21]` has sum 21)

### Example 2: `arr = [1, 2, 3, 5]`

1. Calculate `totalSum = 1 + 2 + 3 + 5 = 11`
2. Check if odd: `11 & 1 = 1` (odd) ✗
3. **Result**: `False` (cannot partition odd sum into two equal parts)

---

## Tabulation Table Example

For `arr = [1, 5, 5]` and `target = 5` (half of totalSum = 11... wait, 11 is odd!)

Let's use `arr = [2, 3, 5]` and `target = 5` (half of totalSum = 10):

| index/target | 0 | 1 | 2 | 3 | 4 | 5 |
|--------------|---|---|---|---|---|---|
| 0 (arr[0]=2) | T | F | T | F | F | F |
| 1 (arr[1]=3) | T | F | T | T | F | T |
| 2 (arr[2]=5) | T | F | T | T | F | T |

**Explanation**:
- Row 0: Can make sum 0 (empty) and sum 2 (using element 2)
- Row 1: Can make 0, 2, 3 (using element 3), 5 (using 2+3)
- Row 2: Can make 0, 2, 3, 5 (existing sums; element 5 alone also makes 5)
- **Result**: `dp[2][5] = True` ✓

---

## Complexity Analysis

| Metric | Complexity |
|--------|------------|
| **Time** | O(n × sum) |
| **Space** | O(sum) |

Where:
- `n` = length of array
- `sum` = total sum of array elements
- We process each element once and check all possible targets up to `sum/2`

---

# Partition a Set into Two Subsets with Minimum Absolute Sum Difference

## Problem Statement

Given an array `arr` of `n` integers, partition the array into two subsets such that the absolute difference between their sums is minimized.

### Examples

**Example 1:**
```
Input: arr = [1, 7, 14, 5]
Output: 1
Explanation: The array can be partitioned as [1, 7, 5] and [14], with an absolute difference of 1.
```

**Example 2:**
```
Input: arr = [3, 1, 6, 2, 2]
Output: 0
Explanation: The array can be partitioned as [3, 2, 2] and [6, 1], with an absolute difference of 0.
```

---

## Problem Definition

We are given an array and need to split it into two subsequences such that the difference in their sums is **minimum**.

### Key Insight

This problem builds upon the **Subset Sum Problem**!

**Observation from Subset Sum**:
- In the Subset Sum tabulation approach, we check for each index whether a subsequence sum exists for all values from 1 to target
- The **last row** of the DP table shows which subsequence sums are achievable for the entire array
- From this row, we can extract **all possible subsequence sums**

**How to use this**:
1. Find all possible sums that can be formed using array elements (using Subset Sum DP)
2. For each possible sum `S1`, the other subset has sum `S2 = total - S1`
3. Calculate difference: `|S1 - S2|`
4. Find the minimum difference across all valid partitions

---

## Mathematical Formulation

Let:
- `total = sum(arr)` (total sum of array)
- `S1` = sum of first subset
- `S2` = sum of second subset

We know: `S1 + S2 = total`

Therefore: `S2 = total - S1`

**Difference**: 
```
|S1 - S2| = |S1 - (total - S1)| = |2×S1 - total|
```

**Goal**: Minimize `|2×S1 - total|` where `S1` is an achievable subset sum.

---

## Optimization Insight

To minimize `|S1 - S2|`:
- We want `S1` and `S2` to be as close as possible
- Ideally, `S1 ≈ total/2` and `S2 ≈ total/2`
- We should check subset sums from `0` to `total/2` (no need to check beyond, as it's symmetric)

**Why?**
- If `S1 > total/2`, then `S2 = total - S1 < total/2`
- The pair `(S1, S2)` and `(S2, S1)` give the same difference
- So we only need to consider `S1 ≤ total/2`

---

## Approach

### Step 1: Find All Possible Subset Sums

Use the Subset Sum DP approach to generate the last row, which tells us which sums are achievable.

### Step 2: Iterate Through Possible Sums

For each achievable sum from `1` to `total`:
- Calculate `current_sum = target_sum`
- Calculate `other_sum = total - current_sum`
- Calculate `diff = |current_sum - other_sum|`
- Track minimum difference

---

## Solution

```python
def optimized(n, arr, targetSum):
    # Two arrays for space-optimized DP
    dp_curr = [False]*(targetSum+1)
    dp_prev = [False]*(targetSum+1)
    
    # Base case: sum 0 is always achievable
    dp_prev[0] = True
    dp_curr[0] = True
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = True
    
    # Fill DP arrays for each element
    for index in range(1, n):
        for target in range(1, targetSum+1):
            # Include current element
            if arr[index] <= target:
                include = dp_prev[target-arr[index]]
            else:
                include = False
            
            # Exclude current element
            exclude = dp_prev[target]
            
            # Update current state
            dp_curr[target] = include or exclude
        
        # Move current to previous for next iteration
        dp_prev = dp_curr.copy()
    
    # Return last row showing all achievable sums
    return dp_prev
```

### Main Logic

```python
arr = [1, 7, 14, 5]
n = len(arr)
total = sum(arr)

# Get all achievable subset sums
last_row = optimized(n, arr, total)

# Initialize minimum difference as total (worst case)
min_diff = total

# Check all possible subset sums
for target_sum in range(1, total):
    if last_row[target_sum]:  # If this sum is achievable
        current_sum = target_sum
        other_sum = total - current_sum
        diff = abs(current_sum - other_sum)
        min_diff = min(min_diff, diff)

print(min_diff)
```

**Time Complexity**: O(n × total)  
**Space Complexity**: O(total)

---

## Example Walkthrough

### Example 1: `arr = [1, 7, 14, 5]`

**Step 1**: Calculate `total = 1 + 7 + 14 + 5 = 27`

**Step 2**: Run Subset Sum DP to find achievable sums

`last_row` (showing True/False for each sum):
```
sum:  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27
      T  T  F  F  F  T  T  T  T  F  F  F  T  T  T  T  F  F  T  T  T  T  T  F  F  F  T  T
```

**Step 3**: Find minimum difference

| Subset Sum (S1) | Other Sum (S2) | Difference |
|-----------------|----------------|------------|
| 1 | 26 | 25 |
| 5 | 22 | 17 |
| 6 | 21 | 15 |
| 7 | 20 | 13 |
| 8 | 19 | 11 |
| 12 | 15 | 3 |
| 13 | 14 | **1** ← minimum |
| 14 | 13 | 1 |

**Result**: Minimum difference = **1**

Partition: `[1, 7, 5] = 13` and `[14] = 14`

---

### Example 2: `arr = [3, 1, 6, 2, 2]`

**Step 1**: Calculate `total = 3 + 1 + 6 + 2 + 2 = 14`

**Step 2**: Run Subset Sum DP

`last_row`:
```
sum:  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14
      T  T  T  T  T  T  T  T  T  T  T  T  T  T  T
```

(All sums from 0 to 14 are achievable!)

**Step 3**: Find minimum difference

When `S1 = 7`:
- `S2 = 14 - 7 = 7`
- `diff = |7 - 7| = 0`

**Result**: Minimum difference = **0**

Partition: `[3, 2, 2] = 7` and `[6, 1] = 7`

---

## Visualization: DP Table

For `arr = [1, 7, 14, 5]` and `total = 27`:

| index/sum | 0 | 1 | 5 | 6 | 7 | 8 | 12 | 13 | 14 | ... |
|-----------|---|---|---|---|---|---|----|----|----|-----|
| 0 (1) | T | T | F | F | F | F | F | F | F | ... |
| 1 (7) | T | T | F | F | T | T | F | F | F | ... |
| 2 (14) | T | T | F | F | T | T | F | F | T | ... |
| 3 (5) | T | T | T | T | T | T | T | T | T | ... |

**Last Row Interpretation**:
- `True` at position `i` means we can form a subset with sum = `i`
- We check each achievable sum to find the one that minimizes the partition difference

---

## Why Check Range `1` to `total`?

```python
for target_sum in range(1, total):
```

- We skip `0` because an empty subset doesn't help (other subset would be the entire array)
- We check up to `total-1` because if one subset has sum = `total`, the other is empty
- For each achievable sum, we calculate the minimum difference

**Optimization**: We could check only up to `total//2` since partitions are symmetric:
```python
for target_sum in range(1, (total//2) + 1):
    if last_row[target_sum]:
        diff = total - 2 * target_sum
        min_diff = min(min_diff, diff)
```

---

## Complete Code

```python
def optimized(n, arr, targetSum):
    dp_curr = [False]*(targetSum+1)
    dp_prev = [False]*(targetSum+1)
    dp_prev[0] = True
    dp_curr[0] = True
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = True
    for index in range(1, n):
        for target in range(1, targetSum+1):
            if arr[index] <= target:
                include = dp_prev[target-arr[index]]
            else:
                include = False
            exclude = dp_prev[target]
            dp_curr[target] = include or exclude
        dp_prev = dp_curr.copy()
    return dp_prev

arr = [1, 7, 14, 5]
n = len(arr)
total = sum(arr)

# Get all achievable subset sums
last_row = optimized(n, arr, total)

# Find minimum difference
min_diff = total
for target_sum in range(1, total):
    if last_row[target_sum]:
        current_sum = target_sum
        other_sum = total - current_sum
        diff = abs(current_sum - other_sum)
        min_diff = min(min_diff, diff)

print(min_diff)
```

---

## Complexity Analysis

| Metric | Complexity |
|--------|------------|
| **Time** | O(n × total) |
| **Space** | O(total) |

Where:
- `n` = length of array
- `total` = sum of all array elements
- DP computation: O(n × total)
- Finding minimum: O(total)
- **Overall**: O(n × total)

---

# Count Subsets with Sum K

## Problem Statement

Given an array `arr` of `n` integers and an integer `K`, count the number of subsets of the given array that have a sum equal to `K`. Return the result modulo (10⁹ + 7).

### Examples

**Example 1:**
```
Input: arr = [2, 3, 5, 16, 8, 10], K = 10
Output: 3
Explanation: The subsets are [2, 8], [10], and [2, 3, 5].
```

**Example 2:**
```
Input: arr = [1, 2, 3, 4, 5], K = 5
Output: 3
Explanation: The subsets are [5], [2, 3], and [1, 4].
```

---

## Problem Definition

We are given an array and need to find the **count** of different subsequences whose sum equals `K`.

### Programmatic Representation

- **Function Definition**: `f(index, target)` → number of subsequences from index 0 to `index` whose sum equals `target`
- **Goal**: Find `f(n-1, K)`, where `n = len(arr)` and `K` is the target sum

---

## Base Cases

1. **If target = 0**: We found a valid way (empty subsequence)
   ```
   f(index, 0) = 1
   ```

2. **If index = 0**: Array contains only one element
   - If that element equals target: 1 way
   - Otherwise: 0 ways
   ```
   f(0, target) = 1 if arr[0] == target else 0
   ```

---

## Recurrence Relation

We use the **pick and not-pick** technique for subsequence generation.

Since we need the **count** of subsequences, we **add** the counts:

1. **Include** current element: `f(index-1, target-arr[index])`
2. **Exclude** current element: `f(index-1, target)`

### Formula:
```
f(index, target) = f(index-1, target-arr[index]) + f(index-1, target)
```

**Condition**: We can only include the current element if `arr[index] <= target`

---

## Difference from Subset Sum Problem

| Aspect | Subset Sum (Exists?) | Count Subsets |
|--------|---------------------|---------------|
| **Return Type** | Boolean (True/False) | Integer (count) |
| **Combination** | `include OR exclude` | `include + exclude` |
| **DP Initialization** | `False` | `0` |
| **Base Case (target=0)** | `True` | `1` |

---

## Recursion Tree

For `arr = [1, 2, 3]` and `target = 3`:

```
                          f(2, 3)
                         /        \
                   (include 3)   (exclude 3)
                      /              \
                  f(1, 0)          f(1, 3)
                    |              /     \
                    1         f(0,1)   f(0,3)
                                |        |
                                0        0

                  f(1, 0) = 1
                  f(0, 1) = 1 (arr[0]=1 equals target)
                  f(0, 3) = 0
                  
                  f(1, 3) = 1 + 0 = 1
                  f(2, 0) = 1
                  f(2, 3) = 1 + 1 = 2

Wait, let's trace more carefully...

                          f(2, 3)
                         /        \
                  f(1, 0)          f(1, 3)
                    |              /     \
                    1         f(0,1)   f(0,3)
                                |        |
                                1        0
                  
                  f(1, 3) = 1 + 0 = 1  (subset [1,2])
                  f(1, 0) = 1  (empty subset, but wait...)
                  f(2, 3) = 1 + 1 = 2

Actual answer: 2 subsets → [3] and [1, 2]
```

---

## Solution Approaches

### 1. Recursive Solution

```python
def solve(n, arr, index, target):
    # Base case: target achieved (found one way)
    if target == 0:
        return 1
    
    # Base case: first element
    if index == 0:
        return 1 if arr[0] == target else 0
    
    # Include current element (if possible)
    if arr[index] <= target:
        include = solve(n, arr, index-1, target-arr[index])
    else:
        include = 0
    
    # Exclude current element
    exclude = solve(n, arr, index-1, target)
    
    # Return total count
    return include + exclude
```

**Time Complexity**: O(2^n)  
**Space Complexity**: O(n) - Recursion stack

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion**:
- Added memo table `memo[index][target]` to cache counts
- Check memo before computing
- Store count in memo after computing
- Changed from Boolean to Integer storage

```python
def solve_memo(n, arr, index, target, memo):
    # Return cached count if available
    if memo[index][target] != None:
        return memo[index][target]
    
    if target == 0:
        return 1
    
    if index == 0:
        return 1 if arr[0] == target else 0
    
    if arr[index] <= target:
        include = solve_memo(n, arr, index-1, target-arr[index], memo)
    else:
        include = 0
    
    exclude = solve_memo(n, arr, index-1, target, memo)
    
    # Cache and return count
    memo[index][target] = include + exclude
    return memo[index][target]
```

**Time Complexity**: O(n × target)  
**Space Complexity**: O(n × target) + O(n) recursion stack

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization**:
- Eliminated recursion with iterative approach
- Changed DP table initialization from `False` to `0`
- Base case: `dp[index][0] = 1` (one way to make sum 0)
- Changed operation from `OR` to `+` (addition)

```python
def tabulation(n, arr, targetSum):
    # Create DP table initialized with 0
    dp = [[0]*(targetSum+1) for _ in range(n)]
    
    # Base case: target = 0 has one way (empty subset)
    for index in range(n):
        dp[index][0] = 1
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp[0][arr[0]] = 1
    
    # Fill DP table
    for index in range(1, n):
        for target in range(1, targetSum+1):
            if arr[index] <= target:
                include = dp[index-1][target-arr[index]]
            else:
                include = 0
            
            exclude = dp[index-1][target]
            
            # Add counts instead of OR
            dp[index][target] = include + exclude
    
    return dp[n-1][targetSum]
```

**Time Complexity**: O(n × target)  
**Space Complexity**: O(n × target)

#### Tabulation Table Example

For `arr = [1, 2, 3]` and `target = 3`:

| index/target | 0 | 1 | 2 | 3 |
|--------------|---|---|---|---|
| 0 (arr[0]=1) | 1 | 1 | 0 | 0 |
| 1 (arr[1]=2) | 1 | 1 | 1 | 1 |
| 2 (arr[2]=3) | 1 | 1 | 1 | 2 |

**Explanation**:
- **Row 0**: Can make sum 0 (1 way: empty) and sum 1 (1 way: [1])
- **Row 1**: 
  - Sum 0: 1 way (empty)
  - Sum 1: 1 way ([1])
  - Sum 2: 1 way ([2])
  - Sum 3: 1 way ([1,2])
- **Row 2**: 
  - Sum 0: 1 way (empty)
  - Sum 1: 1 way ([1])
  - Sum 2: 1 way ([2])
  - Sum 3: 2 ways ([3] and [1,2])

**Answer**: `dp[2][3] = 2`

---

### 4. Space Optimized Solution

**Conversion from Tabulation**:
- Reduced 2D array to two 1D arrays: `dp_prev` and `dp_curr`
- Only store previous and current row
- Copy `dp_curr` to `dp_prev` after each iteration

```python
def optimized(n, arr, targetSum):
    # Two arrays for space optimization
    dp_curr = [0]*(targetSum+1)
    dp_prev = [0]*(targetSum+1)
    
    # Base case: target = 0
    dp_prev[0] = 1
    dp_curr[0] = 1
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = 1
    
    # Fill arrays
    for index in range(1, n):
        for target in range(1, targetSum+1):
            if arr[index] <= target:
                include = dp_prev[target-arr[index]]
            else:
                include = 0
            
            exclude = dp_prev[target]
            dp_curr[target] = include + exclude
        
        # Copy current to previous
        dp_prev = dp_curr.copy()
    
    return dp_prev[targetSum]
```

**Time Complexity**: O(n × target)  
**Space Complexity**: O(target)

---

## Complete Code

```python
def solve(n, arr, index, target):
    if target == 0:
        return 1
    if index == 0:
        return 1 if arr[0] == target else 0
    if arr[index] <= target:
        include = solve(n, arr, index-1, target-arr[index])
    else:
        include = 0
    exclude = solve(n, arr, index-1, target)
    return include + exclude

def solve_memo(n, arr, index, target, memo):
    if memo[index][target] != None:
        return memo[index][target]
    if target == 0:
        return 1
    if index == 0:
        return 1 if arr[0] == target else 0
    if arr[index] <= target:
        include = solve_memo(n, arr, index-1, target-arr[index], memo)
    else:
        include = 0
    exclude = solve_memo(n, arr, index-1, target, memo)
    memo[index][target] = include + exclude
    return memo[index][target]

def tabulation(n, arr, targetSum):
    dp = [[0]*(targetSum+1) for _ in range(n)]
    for index in range(n):
        dp[index][0] = 1
    if arr[0] <= targetSum:
        dp[0][arr[0]] = 1
    for index in range(1, n):
        for target in range(1, targetSum+1):
            if arr[index] <= target:
                include = dp[index-1][target-arr[index]]
            else:
                include = 0
            exclude = dp[index-1][target]
            dp[index][target] = include + exclude
    return dp[n-1][targetSum]

def optimized(n, arr, targetSum):
    dp_curr = [0]*(targetSum+1)
    dp_prev = [0]*(targetSum+1)
    dp_prev[0] = 1
    dp_curr[0] = 1
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = 1
    for index in range(1, n):
        for target in range(1, targetSum+1):
            if arr[index] <= target:
                include = dp_prev[target-arr[index]]
            else:
                include = 0
            exclude = dp_prev[target]
            dp_curr[target] = include + exclude
        dp_prev = dp_curr.copy()
    return dp_prev[targetSum]

# Test
arr = [1, 2, 7, 3]
n = len(arr)
target = 3
MOD = 10**9 + 7

print(solve(n, arr, n-1, target) % MOD)
print(solve_memo(n, arr, n-1, target, [[None]*(target+1) for _ in range(n)]) % MOD)
print(tabulation(n, arr, target) % MOD)
print(optimized(n, arr, target) % MOD)
```

---

## Complexity Summary

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n × target) | O(n × target) + O(n) |
| Tabulation | O(n × target) | O(n × target) |
| Space Optimized | O(n × target) | O(target) |

---

## Why Modulo 10⁹ + 7?

For large arrays, the count can become extremely large. Taking modulo ensures:
- The result fits within standard integer limits
- Prevents overflow errors
- Standard practice in competitive programming

```python
MOD = 10**9 + 7
result = solve(n, arr, n-1, target) % MOD
```

---

# Target Sum with +/- Assignment

## Problem Statement

Given an array `arr` of `n` non-negative integers and a target difference `diff`, count the number of ways to assign `+` or `-` signs to each element such that the resulting sum equals `diff`.

Alternatively: Partition the array into two subsets `S1` (positive) and `S2` (negative) such that `S1 - S2 = diff`.

### Examples

**Example 1:**
```
Input: arr = [1, 1, 2, 3], diff = 1
Output: 3
Explanation: 
  +1 +1 +2 -3 = 1
  +1 -1 +2 -3 = 1  (Wait, this is -1)
  -1 +1 +2 -3 = 1  (Wait, this is -1)
  +1 +1 -2 +3 = 1  (This is 3)
  
Let me recalculate:
  Total = 7, diff = 1
  S1 - S2 = 1 and S1 + S2 = 7
  => S1 = 4, S2 = 3
  
  Subsets with sum 4: [1,3], [1,1,2]
  Actually need to count carefully...
```

**Example 2:**
```
Input: arr = [1, 2, 3], diff = 0
Output: 2
Explanation: Partitions where S1 - S2 = 0 (equal sums):
  S1 = [3], S2 = [1, 2] → 3 - 3 = 0 ✓
  S1 = [1, 2], S2 = [3] → 3 - 3 = 0 ✓
```

---

## Problem Reduction

This problem reduces to **Count Subsets with Sum K**!

### Mathematical Derivation

Let:
- `S1` = sum of elements assigned `+` sign
- `S2` = sum of elements assigned `-` sign
- `total` = sum of all array elements

**Given Constraints**:
1. `S1 - S2 = diff` (target difference)
2. `S1 + S2 = total` (all elements used)

**Solving for S2**:
```
From (1): S1 = S2 + diff
Substitute in (2): (S2 + diff) + S2 = total
                   2×S2 + diff = total
                   S2 = (total - diff) / 2
```

**Therefore**: Count subsets with sum = `(total - diff) / 2`

---

## Edge Cases to Handle

### 1. Invalid Cases

**Case 1**: If `total - diff < 0`
- This means `S2 < 0`, which is impossible
- Return `0`

**Case 2**: If `(total - diff)` is odd
- `S2 = (total - diff) / 2` must be an integer
- If odd, no valid partition exists
- Return `0`

```python
if (total - diff) < 0 or (total - diff) & 1:
    return 0
```

### 2. Zeros in Array - Critical Case!

When the array contains zeros, special handling is needed for the base case.

**Why?**
- If `target = 0` and array has zeros, each zero can be included or excluded
- If we have `k` zeros, there are `2^k` ways to achieve sum 0

**Example**: `arr = [0, 0, 1]`, `target = 1`
- Subsets with sum 1: `[1]`, `[0, 1]`, `[0, 1]`, `[0, 0, 1]`
- But wait, `[0, 1]` and `[0, 1]` are the same!
- Actually: We can include/exclude zeros independently
  - `[1]` ✓
  - `[0, 1]` (first zero) ✓
  - `[0, 1]` (second zero) ✓
  - `[0, 0, 1]` (both zeros) ✓
- Total: 4 ways = `2^2 × 1` (2 zeros, 1 way without zeros)

**Problem**: Standard DP doesn't distinguish between multiple zeros at index 0!

---

## Handling Zeros - The Fix

### Standard Base Case (No Zeros)
```python
dp_prev[0] = 1  # One way to make sum 0: empty subset
```

### Modified Base Case (With Zeros)

**If first element is 0**:
```python
if arr[0] == 0:
    dp_prev[0] = 2  # Include or exclude the zero
else:
    dp_prev[0] = 1  # Just empty subset
```

**More Generally**:
```python
if targetSum == 0:
    dp_prev[0] = 2  # Special case when target itself is 0
else:
    dp_prev[0] = 1  # Normal case
```

**For first element**:
```python
if arr[0] <= targetSum and arr[0] != 0:
    dp_prev[arr[0]] = 1
```
- We only set `dp_prev[arr[0]]` if `arr[0] != 0`
- If `arr[0] = 0`, it's already handled by `dp_prev[0] = 2`

---

## Solution with Zero Handling

```python
def optimized(n, arr, targetSum):
    dp_curr = [0]*(targetSum+1)
    dp_prev = [0]*(targetSum+1)
    
    # Special handling for target = 0 and zeros in array
    if targetSum == 0:
        dp_prev[0] = 2  # Include or exclude first element (if it's 0)
    else:
        dp_prev[0] = 1  # Normal case: empty subset
    
    # First element handling (avoid double counting if arr[0] = 0)
    if arr[0] <= targetSum and arr[0] != 0:
        dp_prev[arr[0]] = 1
    
    # Fill DP arrays
    for index in range(1, n):
        dp_curr = [0]*(targetSum+1)  # Reset current array
        
        for target in range(targetSum+1):
            # Include current element
            if arr[index] <= target:
                include = dp_prev[target-arr[index]]
            else:
                include = 0
            
            # Exclude current element
            exclude = dp_prev[target]
            
            # Total count
            dp_curr[target] = include + exclude
        
        dp_prev = dp_curr.copy()
    
    return dp_prev[targetSum]
```

---

## Complete Code

```python
def optimized(n, arr, targetSum):
    dp_curr = [0]*(targetSum+1)
    dp_prev = [0]*(targetSum+1)
    
    if targetSum == 0:
        dp_prev[0] = 2
    else:
        dp_prev[0] = 1
    
    if arr[0] <= targetSum and arr[0] != 0:
        dp_prev[arr[0]] = 1
    
    for index in range(1, n):
        dp_curr = [0]*(targetSum+1)
        for target in range(targetSum+1):
            if arr[index] <= target:
                include = dp_prev[target-arr[index]]
            else:
                include = 0
            exclude = dp_prev[target]
            dp_curr[target] = include + exclude
        dp_prev = dp_curr.copy()
    
    return dp_prev[targetSum]

# Main logic
arr = [1, 1, 2, 3]
n = len(arr)
diff = 1
total = sum(arr)

# Check for invalid cases
if (total - diff) < 0 or (total - diff) & 1:
    print(0)
else:
    s2 = (total - diff) // 2
    print(optimized(n, arr, s2))
```

---

## Example Walkthrough

### Example 1: `arr = [1, 1, 2, 3]`, `diff = 1`

**Step 1**: Calculate `total = 7`

**Step 2**: Check validity
- `total - diff = 7 - 1 = 6` (≥ 0) ✓
- `6 & 1 = 0` (even) ✓

**Step 3**: Calculate `s2 = 6 // 2 = 3`

**Step 4**: Count subsets with sum = 3
- `[3]` ✓
- `[1, 2]` ✓
- `[1, 2]` (using second 1) ✓

**Result**: 3 ways

**Verification**:
- `S1 = 4, S2 = 3`: `[1, 3]` and `[1, 2]` → `4 - 3 = 1` ✓
- `S1 = 4, S2 = 3`: `[1, 3]` and `[1, 2]` → `4 - 3 = 1` ✓
- `S1 = 4, S2 = 3`: `[1, 1, 2]` and `[3]` → `4 - 3 = 1` ✓

---

### Example 2: `arr = [0, 0, 1]`, `diff = 1`

**Step 1**: Calculate `total = 1`

**Step 2**: Check validity
- `total - diff = 1 - 1 = 0` (≥ 0) ✓
- `0 & 1 = 0` (even) ✓

**Step 3**: Calculate `s2 = 0 // 2 = 0`

**Step 4**: Count subsets with sum = 0 (THIS IS WHERE ZERO HANDLING MATTERS!)

Without zero handling:
- Only `[]` (empty subset) → 1 way ✗

With zero handling:
- `[]` ✓
- `[0]` (first) ✓
- `[0]` (second) ✓
- `[0, 0]` ✓

**Result**: 4 ways = `2^2` (two zeros)

**Verification**:
- All 4 ways assign `[1]` to `S1` and `[]`, `[0]`, `[0]`, `[0,0]` to `S2`
- Each gives `1 - 0 = 1` ✓

---

## Why Reset `dp_curr` in Loop?

```python
for index in range(1, n):
    dp_curr = [0]*(targetSum+1)  # Reset current array
```

**Reason**: Prevents accumulation from previous iterations
- Without reset, values would keep adding up
- Each iteration should start fresh based on `dp_prev`

---

## Comparison: With vs Without Zero Handling

### Test Case: `arr = [0, 0, 0, 0, 1]`, `target = 1`

| Method | Base Case | Result | Correct? |
|--------|-----------|--------|----------|
| Standard | `dp_prev[0] = 1` | 1 | ✗ |
| With Zero Handling | `dp_prev[0] = 2` (if target=0) | 16 | ✓ |

**Explanation**:
- 4 zeros can be arranged in `2^4 = 16` ways
- Each arrangement combined with `[1]` gives sum = 1
- Standard approach misses the multiple ways zeros can be included/excluded

---

## Complexity Analysis

| Metric | Complexity |
|--------|------------|
| **Time** | O(n × target) |
| **Space** | O(target) |

Where:
- `n` = length of array
- `target = (total - diff) / 2`

---

## Key Takeaways

1. **Problem Reduction**: Target sum assignment reduces to counting subsets
2. **Mathematical Derivation**: `S2 = (total - diff) / 2`
3. **Edge Cases**: Check for negative or odd `(total - diff)`
4. **Zero Handling**: Critical for arrays containing zeros
   - Use `dp_prev[0] = 2` when `targetSum = 0`
   - Avoid double-counting first element if it's 0
5. **Array Reset**: Reset `dp_curr` in each iteration to prevent accumulation

---
