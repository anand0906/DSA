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

# Count Partitions with Given Difference

## Problem Statement

Given an array `arr` of `n` integers and an integer `diff`, count the number of ways to partition the array into two subsets `S1` and `S2` such that:
- `|S1 - S2| = diff` where `S1 ≥ S2`
- `|S1|` and `|S2|` represent the **sum** of subsets `S1` and `S2` respectively

Return the result modulo (10⁹ + 7).

**Note**: A partition means that the union of `S1` and `S2` is the original array, and no element is left out or used twice — every element belongs to exactly one of the two subsets.

### Examples

**Example 1:**
```
Input: arr = [1, 1, 2, 3], diff = 1
Output: 3
Explanation: The subsets are [1, 2] and [1, 3], [1, 3] and [1, 2], [1, 1, 2] and [3].
```

**Example 2:**
```
Input: arr = [1, 2, 3, 4], diff = 2
Output: 2
Explanation: The subsets are [1, 3] and [2, 4], [1, 2, 3] and [4].
```

---

## Problem Definition

We need to partition the array into two subsets such that the absolute difference of their sums equals `diff`.

Since `S1 ≥ S2`, the problem is equivalent to: `S1 - S2 = diff`

---

## Problem Reduction to Subset Sum

This problem reduces to the **Count Subsets with Sum K** problem!

### Mathematical Derivation

Given:
- `S1` = sum of first subset (larger)
- `S2` = sum of second subset (smaller)
- `total` = sum of all array elements
- `S1 ≥ S2` (given constraint)

**Constraints**:
1. `S1 - S2 = diff` (target difference)
2. `S1 + S2 = total` (all elements must be used)

**Solving for S2**:
```
From equation (1): S1 = S2 + diff

Substitute in equation (2):
(S2 + diff) + S2 = total
2×S2 + diff = total
2×S2 = total - diff
S2 = (total - diff) / 2
```

**Also**:
```
S1 = total - S2 = total - (total - diff)/2 = (total + diff) / 2
```

**Conclusion**: Count subsets with sum = `(total - diff) / 2`

---

## Why This Reduction Works

**Key Insight**:
- Once we choose elements for subset `S2`, the remaining elements automatically form `S1`
- We don't need to count both `S1` and `S2` separately
- Counting subsets with sum `S2` gives us all valid partitions

**Example**: `arr = [1, 1, 2, 3]`, `diff = 1`
- `total = 7`
- `S2 = (7 - 1) / 2 = 3`
- Subsets with sum 3: `{3}`, `{1, 2}` (first 1), `{1, 2}` (second 1)
- Count = 3 ✓

For each subset with sum 3, the remaining elements have sum `7 - 3 = 4`, giving difference `4 - 3 = 1` ✓

---

## Base Cases

Before applying the subset sum algorithm, check for invalid cases:

### Invalid Case 1: Negative Sum
```python
if (total - diff) < 0:
    return 0
```
**Reason**: `S2 = (total - diff) / 2` cannot be negative

**Example**: `arr = [1, 2]`, `diff = 5`
- `total = 3`, `S2 = (3 - 5) / 2 = -1` (impossible)

### Invalid Case 2: Odd Result
```python
if (total - diff) % 2 != 0:
    return 0
```
**Reason**: `S2` must be an integer

**Example**: `arr = [1, 2, 3]`, `diff = 2`
- `total = 6`, `S2 = (6 - 2) / 2 = 2` ✓ (even, valid)

**Example**: `arr = [1, 2]`, `diff = 2`
- `total = 3`, `S2 = (3 - 2) / 2 = 0.5` (not an integer, invalid)

---

## Recurrence Relation

Once we have `targetSum = (total - diff) / 2`, we apply the subset sum counting logic:

```
f(index, target) = f(index-1, target-arr[index]) + f(index-1, target)
```

Where:
- `f(index-1, target-arr[index])` → **Include** current element (if `arr[index] ≤ target`)
- `f(index-1, target)` → **Exclude** current element

**Base Cases for Subset Sum**:
1. `f(index, 0) = 1` (empty subset)
2. `f(0, target) = 1` if `arr[0] == target`, else `0`

---

## Handling Zeros in Array

**Critical Edge Case**: When array contains zeros, special handling is needed!

### Why Zeros Need Special Treatment

- A zero doesn't change the sum, so it can be placed in either subset
- If first element is 0, there are **2 ways** to achieve sum 0:
  1. Empty subset (don't pick the zero)
  2. Subset with just the zero (pick the zero)

**Example**: `arr = [0, 0, 1, 2]`, `diff = 1`
- `total = 3`, `S2 = (3 - 1) / 2 = 1`
- Subsets with sum 1: `{1}`, `{0, 1}`, `{0, 1}`, `{0, 0, 1}`
- Each zero can independently go into either subset
- With 2 zeros and 1 non-zero subset, we have `2² × 1 = 4` ways

### Base Case for Zeros

```python
if arr[0] == 0:
    dp_prev[0] = 2  # Two ways: pick or not pick the zero
else:
    dp_prev[0] = 1  # One way: empty subset
```

```python
if arr[0] != 0 and arr[0] <= targetSum:
    dp_prev[arr[0]] = 1  # Only mark if non-zero to avoid double-counting
```

---

## Solution: Space Optimized DP

```python
MOD = 10**9 + 7

def countSubsetsWithSum(n, arr, targetSum):
    dp_prev = [0] * (targetSum + 1)
    dp_curr = [0] * (targetSum + 1)

    # Base case: Handle first element and zeros
    if arr[0] == 0:
        dp_prev[0] = 2  # Include or exclude the zero
    else:
        dp_prev[0] = 1  # Only empty subset gives sum 0

    # If first element is non-zero and within target
    if arr[0] != 0 and arr[0] <= targetSum:
        dp_prev[arr[0]] = 1

    # Process remaining elements
    for index in range(1, n):
        for target in range(targetSum + 1):
            # Include current element
            include = 0
            if arr[index] <= target:
                include = dp_prev[target - arr[index]]

            # Exclude current element
            exclude = dp_prev[target]
            
            # Total count with modulo
            dp_curr[target] = (include + exclude) % MOD

        # Copy for next iteration
        dp_prev = dp_curr[:]

    return dp_prev[targetSum] % MOD
```

**Time Complexity**: O(n × targetSum)  
**Space Complexity**: O(targetSum)

---

## Complete Solution

```python
MOD = 10**9 + 7

def countSubsetsWithSum(n, arr, targetSum):
    dp_prev = [0] * (targetSum + 1)
    dp_curr = [0] * (targetSum + 1)

    # Base case
    if arr[0] == 0:
        dp_prev[0] = 2
    else:
        dp_prev[0] = 1

    if arr[0] != 0 and arr[0] <= targetSum:
        dp_prev[arr[0]] = 1

    # Fill DP
    for index in range(1, n):
        for target in range(targetSum + 1):
            include = 0
            if arr[index] <= target:
                include = dp_prev[target - arr[index]]

            exclude = dp_prev[target]
            dp_curr[target] = (include + exclude) % MOD

        dp_prev = dp_curr[:]

    return dp_prev[targetSum] % MOD


class Solution:
    def countPartitions(self, n, diff, arr):
        total = sum(arr)

        # Check validity
        if (total - diff) < 0 or (total - diff) % 2 != 0:
            return 0

        s2 = (total - diff) // 2
        return countSubsetsWithSum(n, arr, s2)


# Test cases
sol = Solution()
print(sol.countPartitions(4, 1, [1, 1, 2, 3]))  # Output: 3
print(sol.countPartitions(4, 2, [1, 2, 3, 4]))  # Output: 2
```

---

## Example Walkthrough

### Example 1: `arr = [1, 1, 2, 3]`, `diff = 1`

**Step 1**: Calculate total sum
```
total = 1 + 1 + 2 + 3 = 7
```

**Step 2**: Check validity
```
(total - diff) = 7 - 1 = 6 ≥ 0 ✓
6 % 2 = 0 (even) ✓
```

**Step 3**: Calculate target
```
S2 = (total - diff) / 2 = (7 - 1) / 2 = 3
```

**Step 4**: Count subsets with sum = 3

Build DP table:

| index/target | 0 | 1 | 2 | 3 |
|--------------|---|---|---|---|
| 0 (val=1) | 1 | 1 | 0 | 0 |
| 1 (val=1) | 1 | 2 | 1 | 1 |
| 2 (val=2) | 1 | 2 | 2 | 2 |
| 3 (val=3) | 1 | 2 | 2 | **3** |

**Subsets with sum 3**:
1. `{3}` → Remaining: `{1, 1, 2}` with sum 4 → `4 - 3 = 1` ✓
2. `{1, 2}` (first 1) → Remaining: `{1, 3}` with sum 4 → `4 - 3 = 1` ✓
3. `{1, 2}` (second 1) → Remaining: `{1, 3}` with sum 4 → `4 - 3 = 1` ✓

**Answer**: 3

---

### Example 2: `arr = [1, 2, 3, 4]`, `diff = 2`

**Step 1**: Calculate total sum
```
total = 1 + 2 + 3 + 4 = 10
```

**Step 2**: Check validity
```
(total - diff) = 10 - 2 = 8 ≥ 0 ✓
8 % 2 = 0 (even) ✓
```

**Step 3**: Calculate target
```
S2 = (total - diff) / 2 = (10 - 2) / 2 = 4
```

**Step 4**: Count subsets with sum = 4

Build DP table:

| index/target | 0 | 1 | 2 | 3 | 4 |
|--------------|---|---|---|---|---|
| 0 (val=1) | 1 | 1 | 0 | 0 | 0 |
| 1 (val=2) | 1 | 1 | 1 | 1 | 0 |
| 2 (val=3) | 1 | 1 | 1 | 2 | 1 |
| 3 (val=4) | 1 | 1 | 1 | 2 | **2** |

**Subsets with sum 4**:
1. `{1, 3}` → Remaining: `{2, 4}` with sum 6 → `6 - 4 = 2` ✓
2. `{4}` → Remaining: `{1, 2, 3}` with sum 6 → `6 - 4 = 2` ✓

**Answer**: 2

---

### Example 3: `arr = [0, 0, 1]`, `diff = 1` (Zero Handling)

**Step 1**: Calculate total sum
```
total = 0 + 0 + 1 = 1
```

**Step 2**: Check validity
```
(total - diff) = 1 - 1 = 0 ≥ 0 ✓
0 % 2 = 0 (even) ✓
```

**Step 3**: Calculate target
```
S2 = (total - diff) / 2 = (1 - 1) / 2 = 0
```

**Step 4**: Count subsets with sum = 0

**Key Point**: First element is 0, so `dp_prev[0] = 2`

Build DP table:

| index/target | 0 |
|--------------|---|
| 0 (val=0) | 2 |
| 1 (val=0) | 4 |
| 2 (val=1) | 4 |

**Explanation**:
- Row 0: `dp_prev[0] = 2` (pick or not pick first zero)
- Row 1: For each way in row 0, we can pick or not pick second zero → `2 × 2 = 4`
- Row 2: Element 1 doesn't contribute to sum 0, so count remains 4

**Subsets with sum 0**:
1. `{}` → Remaining: `{0, 0, 1}` with sum 1 → `1 - 0 = 1` ✓
2. `{0}` (first) → Remaining: `{0, 1}` with sum 1 → `1 - 0 = 1` ✓
3. `{0}` (second) → Remaining: `{0, 1}` with sum 1 → `1 - 0 = 1` ✓
4. `{0, 0}` → Remaining: `{1}` with sum 1 → `1 - 0 = 1` ✓

**Answer**: 4 = 2² (two zeros)

---

## Complexity Analysis

| Operation | Complexity |
|-----------|------------|
| **Time** | O(n × S2) = O(n × sum) |
| **Space** | O(S2) = O(sum) |

Where:
- `n` = length of array
- `S2 = (total - diff) / 2`
- `sum` = total sum of array

---

## Key Insights

1. **Problem Reduction**: 
   - Partition with difference → Count subsets with specific sum
   - Formula: `S2 = (total - diff) / 2`

2. **Validity Checks**:
   - `(total - diff) < 0` → Impossible (negative sum)
   - `(total - diff) % 2 != 0` → Impossible (non-integer sum)

3. **Zero Handling**:
   - If `arr[0] = 0`: `dp_prev[0] = 2` (two ways)
   - If `arr[0] ≠ 0`: `dp_prev[0] = 1` (one way)
   - Check `arr[0] != 0` before marking `dp_prev[arr[0]]`

4. **Space Optimization**:
   - Use two 1D arrays instead of 2D table
   - Proper copying: `dp_prev = dp_curr[:]`

5. **Modulo Operation**:
   - Apply during computation: `(include + exclude) % MOD`
   - Prevents integer overflow for large counts

---

# Target Sum

## Problem Statement

Given an array `nums` of `n` integers and an integer `target`, build an expression using the integers from `nums` where each integer can be prefixed with either a `+` or `-` sign.

The goal is to achieve the target sum by evaluating all possible combinations of these signs.

Determine the number of ways to achieve the target sum and return your answer modulo (10⁹ + 7).

### Examples

**Example 1:**
```
Input: nums = [1, 2, 7, 1, 5], target = 4
Output: 2
Explanation: There are 2 ways to assign symbols to make the sum of nums be target 4.
  +1 + 2 + 7 - 1 - 5 = 4
  -1 + 2 + 7 + 1 - 5 = 4
```

**Example 2:**
```
Input: nums = [1], target = 1
Output: 1
Explanation: There is only one way to assign symbols to make the sum of nums be target 1.
  +1 = 1
```

**Example 3:**
```
Input: nums = [1, 1, 1, 1, 1], target = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
  +1 + 1 + 1 + 1 - 1 = 3
  +1 + 1 + 1 - 1 + 1 = 3
  +1 + 1 - 1 + 1 + 1 = 3
  +1 - 1 + 1 + 1 + 1 = 3
  -1 + 1 + 1 + 1 + 1 = 3
```

---

## Why This Is The Same As "Count Partitions with Given Difference"

This problem is **mathematically identical** to the "Count Partitions with Given Difference" problem!

### The Connection

**Target Sum Problem**:
- Assign `+` or `-` to each element
- Elements with `+` contribute positively
- Elements with `-` contribute negatively
- Goal: `(sum of + elements) - (sum of - elements) = target`

**Partition with Difference Problem**:
- Partition array into two subsets `S1` and `S2`
- Goal: `S1 - S2 = diff`

### They're The Same Thing!

Let's say:
- `Positive` = sum of all elements with `+` sign
- `Negative` = sum of all elements with `-` sign

Then:
- `Positive - Negative = target`
- `Positive + Negative = total` (sum of all elements)

This is **exactly** the same as:
- `S1 - S2 = diff`
- `S1 + S2 = total`

**Mapping**:
- Elements with `+` sign → belong to subset `S1`
- Elements with `-` sign → belong to subset `S2`
- `target` in Target Sum = `diff` in Partition problem

### Visual Example

For `nums = [1, 2, 7, 1, 5]`, `target = 4`:

**As Target Sum**:
```
+1 + 2 + 7 - 1 - 5 = 4
Positive group: {1, 2, 7} = 10
Negative group: {1, 5} = 6
Result: 10 - 6 = 4 ✓
```

**As Partition Problem** (diff = 4):
```
S1 = {1, 2, 7} = 10
S2 = {1, 5} = 6
Difference: 10 - 6 = 4 ✓
```

**They're identical!**

---

## Solution Approach

Use the **exact same solution** as "Count Partitions with Given Difference":

1. Calculate `total = sum(nums)`
2. Check if `(total - target) < 0` or `(total - target)` is odd → return 0
3. Calculate `S2 = (total - target) / 2`
4. Count subsets with sum = `S2` using DP

The code is identical, just replace `diff` with `target`!

---

## Test Cases

```python
# Test Case 1
nums = [1, 2, 7, 1, 5]
target = 4
# Output: 2

# Test Case 2
nums = [1]
target = 1
# Output: 1

# Test Case 3
nums = [1, 1, 1, 1, 1]
target = 3
# Output: 5

# Test Case 4
nums = [0, 0, 0, 0, 0, 0, 1]
target = 1
# Output: 64 (2^6 = 64 ways to distribute 6 zeros)

# Test Case 5
nums = [100]
target = -200
# Output: 0 (impossible)
```

---


# 0/1 Knapsack Problem

## Problem Statement

Given two integer arrays, `val` and `wt`, each of size N, which represent the values and weights of N items respectively, and an integer W representing the maximum capacity of a knapsack, determine the maximum value achievable by selecting a subset of the items such that the total weight of the selected items does not exceed the knapsack capacity W.

Each item can either be picked in its entirety or not picked at all (0-1 property). The goal is to maximize the sum of the values of the selected items while keeping the total weight within the knapsack's capacity.

### Examples

**Example 1:**
```
Input: val = [60, 100, 120], wt = [10, 20, 30], W = 50
Output: 220
Explanation: Select items with weights 20 and 30 for a total value of 100 + 120 = 220.
```

**Example 2:**
```
Input: val = [10, 40, 30, 50], wt = [5, 4, 6, 3], W = 10
Output: 90
Explanation: Select items with weights 4 and 3 for a total value of 40 + 50 = 90.
```

---

## Step-by-Step Approach

### Step 1: Define The Problem

We have a knapsack with a maximum weight capacity and n items, where each item has a weight and value. We can include items in the bag such that we don't exceed the bag's weight capacity while maximizing the total value.

### Step 2: Represent the Problem Programmatically

Since we need to check all possible ways to select items and find the maximum value, we use the **pick or not-pick technique**.

**Function Definition:**
```
f(index, capacity) → Maximum value that can be obtained using items from index 0 to index with at most 'capacity' weight
```

### Step 3: Base Cases

1. **If we've processed all items (index < 0):** Return 0
2. **If we're at the first item (index == 0):**
   - If `weight[0] <= capacity`, return `cost[0]`
   - Otherwise, return 0

### Step 4: Recurrence Relation

For each item at index, we have two choices:

1. **Include the item:** `include = cost[index] + f(index-1, capacity - weight[index])`
   - Only possible if `weight[index] <= capacity`
2. **Exclude the item:** `exclude = f(index-1, capacity)`

**Recurrence:** `f(index, capacity) = max(include, exclude)`

---

## Recursion Tree Diagram

```
Example: val=[60,100,120], wt=[10,20,30], W=50

                        f(2, 50)
                       /        \
              (include)          (exclude)
            120+f(1,20)          f(1,50)
               /    \            /     \
         100+f(0,-10) f(0,20) 100+f(0,30) f(0,50)
              |         |         |          |
              0        60        60         60
            (invalid)

Result Paths:
- f(2,50) → include item 2 → f(1,20) → exclude item 1 → f(0,20) → include item 0 = 120+60 = 180
- f(2,50) → include item 2 → f(1,20) → include item 1 → invalid (weight exceeded)
- f(2,50) → exclude item 2 → f(1,50) → include item 1 → f(0,30) → include item 0 = 100+60 = 160
- f(2,50) → exclude item 2 → f(1,50) → exclude item 1 → f(0,50) → include item 0 = 60

Maximum = 220 comes from including items 1 and 2 (weights 20+30=50, values 100+120=220)
Note: The tree shows partial exploration; complete tree would show path yielding 220.
```

---

## Solutions

### 1. Recursive Solution

```python
def solve(n, weight, cost, index, capacity):
    # Base case: first item
    if index == 0:
        if weight[0] <= capacity:
            return cost[0]
        return 0
    
    # Option 1: Include current item (if weight allows)
    if weight[index] <= capacity:
        include = cost[index] + solve(n, weight, cost, index-1, capacity - weight[index])
    else:
        include = 0
    
    # Option 2: Exclude current item
    exclude = solve(n, weight, cost, index-1, capacity)
    
    return max(include, exclude)
```

**Time Complexity:** O(2^n) - Each item has 2 choices  
**Space Complexity:** O(n) - Recursion stack depth

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion → Memoization:**

1. Create a memoization table `memo[index][capacity]` initialized with `None`
2. Before computing, check if `memo[index][capacity]` already has a value
3. If yes, return the cached value
4. If no, compute the result and store it in `memo[index][capacity]` before returning

```python
def solve_memo(n, weight, cost, index, capacity, memo):
    # Check if already computed
    if memo[index][capacity] != None:
        return memo[index][capacity]
    
    # Base case: first item
    if index == 0:
        if weight[0] <= capacity:
            return cost[0]
        return 0
    
    # Option 1: Include current item (if weight allows)
    if weight[index] <= capacity:
        include = cost[index] + solve_memo(n, weight, cost, index-1, capacity - weight[index], memo)
    else:
        include = 0
    
    # Option 2: Exclude current item
    exclude = solve_memo(n, weight, cost, index-1, capacity, memo)
    
    # Store result in memo table
    memo[index][capacity] = max(include, exclude)
    return memo[index][capacity]
```

**Time Complexity:** O(n × W) - Each state computed once  
**Space Complexity:** O(n × W) - Memoization table + O(n) recursion stack

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization → Tabulation:**

1. Replace recursion with iteration
2. Create a DP table `dp[index][capacity]` where `dp[i][c]` represents the maximum value using items 0 to i with capacity c
3. Fill base cases first (index 0)
4. Iterate through indices and capacities, filling the table using the recurrence relation
5. Replace recursive calls with table lookups: `dp[index-1][...]`

```python
def tabulation(n, weight, cost, capacity):
    # Create DP table
    dp = [[0] * (capacity + 1) for _ in range(n)]
    
    # Base case: first item (index 0)
    for wt in range(capacity + 1):
        if weight[0] <= wt:
            dp[0][wt] = cost[0]
    
    # Fill table for remaining items
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Option 1: Include current item
            if weight[index] <= cap:
                include = cost[index] + dp[index-1][cap - weight[index]]
            else:
                include = 0
            
            # Option 2: Exclude current item
            exclude = dp[index-1][cap]
            
            dp[index][cap] = max(include, exclude)
    
    return dp[n-1][capacity]
```

**Time Complexity:** O(n × W)  
**Space Complexity:** O(n × W) - DP table

---

### Tabulation Example

**Input:** val = [60, 100, 120], wt = [10, 20, 30], W = 50

**DP Table:**

| Index\Cap | 0 | 10 | 20 | 30 | 40 | 50 |
|-----------|---|----|----|----|----|-----|
| 0 (wt=10, val=60) | 0 | 60 | 60 | 60 | 60 | 60 |
| 1 (wt=20, val=100) | 0 | 60 | 100 | 160 | 160 | 160 |
| 2 (wt=30, val=120) | 0 | 60 | 100 | 160 | 180 | **220** |

**Explanation:**
- **Row 0:** Only item 0 available (wt=10, val=60). If capacity ≥ 10, we can take it.
- **Row 1:** Items 0-1 available. At cap=20, we can take item 1 (val=100). At cap=30+, we can take both items (60+100=160).
- **Row 2:** All items available. At cap=50, we can take items 1 and 2 (wt=20+30=50, val=100+120=**220**).

---

### 4. Space Optimized Solution

**Conversion from Tabulation → Space Optimization:**

1. Observe that `dp[index][cap]` only depends on `dp[index-1][...]`
2. Replace 2D table with two 1D arrays: `dp_prev` (previous row) and `dp_curr` (current row)
3. After computing each row, copy `dp_curr` to `dp_prev`
4. This reduces space from O(n × W) to O(W)

```python
def optimized(n, weight, cost, capacity):
    # Two arrays instead of 2D table
    dp_curr = [0] * (capacity + 1)
    dp_prev = [0] * (capacity + 1)
    
    # Base case: first item
    for wt in range(capacity + 1):
        if weight[0] <= wt:
            dp_prev[wt] = cost[0]
    
    # Fill for remaining items
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Option 1: Include current item
            if weight[index] <= cap:
                include = cost[index] + dp_prev[cap - weight[index]]
            else:
                include = 0
            
            # Option 2: Exclude current item
            exclude = dp_prev[cap]
            
            dp_curr[cap] = max(include, exclude)
        
        # Move current row to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[capacity]
```

**Time Complexity:** O(n × W)  
**Space Complexity:** O(W) - Only two 1D arrays

---

## Usage

```python
capacity = 50
weight = [10, 20, 30]
cost = [60, 100, 120]
n = len(weight)

print(solve(n, weight, cost, n-1, capacity))  # Recursion

memo = [[None] * (capacity + 1) for _ in range(n)]
print(solve_memo(n, weight, cost, n-1, capacity, memo))  # Memoization

print(tabulation(n, weight, cost, capacity))  # Tabulation

print(optimized(n, weight, cost, capacity))  # Space Optimized
```

**Output:** 220

---

## Complexity Summary

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n × W) | O(n × W) + O(n) |
| Tabulation | O(n × W) | O(n × W) |
| Space Optimized | O(n × W) | O(W) |

Where n = number of items, W = knapsack capacity

---

# Rod Cutting Problem

## Problem Statement

Given a rod of length N inches and an array price[] where price[i] denotes the value of a piece of rod of length i inches (1-based indexing). Determine the maximum value obtainable by cutting up the rod and selling the pieces. Make any number of cuts, or none at all, and sell the resulting pieces.

### Examples

**Example 1:**
```
Input: price = [1, 6, 8, 9, 10, 19, 7, 20], N = 8
Output: 25
Explanation: Cut the rod into lengths of 2 and 6 for a total price of 6 + 19 = 25.
```

**Example 2:**
```
Input: price = [1, 5, 8, 9], N = 4
Output: 10
Explanation: Cut the rod into lengths of 2 and 2 for a total price of 5 + 5 = 10.
```

---

## How Rod Cutting is Similar to Unbounded Knapsack

The Rod Cutting problem is **exactly the Unbounded Knapsack problem** with a clever transformation. Once you understand this mapping, you can directly apply the unbounded knapsack solution!

### Problem Mapping

| Rod Cutting Concept | Unbounded Knapsack Equivalent |
|---------------------|-------------------------------|
| Rod length (N) | Knapsack capacity (W) |
| Piece lengths [1, 2, 3, ..., N] | Item weights [1, 2, 3, ..., N] |
| Price array [price[0], price[1], ...] | Item values [val[0], val[1], ...] |
| Cut the rod into pieces | Select items to fill knapsack |
| Maximize total price | Maximize total value |
| Can use same length multiple times | Can use same item multiple times (unbounded) |

### Transformation

```python
# Rod Cutting Input
price = [1, 6, 8, 9, 10, 19, 7, 20]
N = 8  # Total rod length

# Transform to Unbounded Knapsack Input
capacity = N  # Rod length becomes knapsack capacity
weight = [1, 2, 3, 4, 5, 6, 7, 8]  # Possible piece lengths
cost = price  # Prices become item values

# Now solve as unbounded knapsack!
```

### Why It's Unbounded Knapsack

1. **Same piece can be cut multiple times:** Just like unbounded knapsack where you can pick the same item multiple times, you can cut multiple pieces of the same length.
   - Example: Rod of length 8 can be cut into four pieces of length 2: [2, 2, 2, 2]

2. **Maximize value within capacity:** 
   - Rod Cutting: Maximize price while total cut lengths ≤ N
   - Unbounded Knapsack: Maximize value while total weight ≤ W

3. **Decision at each step:**
   - Rod Cutting: "Should I cut a piece of length i?"
   - Unbounded Knapsack: "Should I pick item i?"

### Example Walkthrough

**Rod Cutting:**
- Rod length N = 8
- price = [1, 6, 8, 9, 10, 19, 7, 20]
- Question: How to cut rod of length 8 to maximize price?

**Converted to Unbounded Knapsack:**
- Capacity W = 8
- weights = [1, 2, 3, 4, 5, 6, 7, 8]
- values = [1, 6, 8, 9, 10, 19, 7, 20]
- Question: Which items to pick (can repeat) to maximize value with weight ≤ 8?

**Solution:**
- Cut pieces of length 2 and 6 (or pick items with weight 2 and 6)
- Total length: 2 + 6 = 8 ✓
- Total price: 6 + 19 = 25 ✓

---

## Solutions

### Setup: Transform Rod Cutting to Unbounded Knapsack

```python
# Rod Cutting Input
price = [1, 6, 8, 9, 10, 19, 7, 20]
n = len(price)

# Transform to Unbounded Knapsack
capacity = n  # Total rod length
weight = [length for length in range(1, n+1)]  # [1, 2, 3, 4, 5, 6, 7, 8]
cost = price  # Same as price array
```

---

### 1. Recursive Solution

```python
def solve(n, weight, cost, index, capacity):
    # Base case: only first piece length available
    if index == 0:
        return cost[0] * (capacity // weight[0])
    
    # Option 1: Cut a piece of current length (if fits)
    if weight[index] <= capacity:
        # Stay at same index to allow multiple cuts of same length
        include = cost[index] + solve(n, weight, cost, index, capacity - weight[index])
    else:
        include = 0
    
    # Option 2: Don't cut this length
    exclude = solve(n, weight, cost, index-1, capacity)
    
    return max(include, exclude)
```

**Time Complexity:** O(2^n)  
**Space Complexity:** O(n)

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion → Memoization:**

Add a memo table to cache results and avoid recomputation of overlapping subproblems.

```python
def solve_memo(n, weight, cost, index, capacity, memo):
    # Return cached result if available
    if memo[index][capacity] != None:
        return memo[index][capacity]
    
    # Base case: only first piece length available
    if index == 0:
        return cost[0] * (capacity // weight[0])
    
    # Option 1: Cut a piece of current length (if fits)
    if weight[index] <= capacity:
        # Stay at same index to allow multiple cuts of same length
        include = cost[index] + solve_memo(n, weight, cost, index, capacity - weight[index], memo)
    else:
        include = 0
    
    # Option 2: Don't cut this length
    exclude = solve_memo(n, weight, cost, index-1, capacity, memo)
    
    # Cache and return result
    memo[index][capacity] = max(include, exclude)
    return memo[index][capacity]
```

**Time Complexity:** O(n × N)  
**Space Complexity:** O(n × N) + O(n)

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization → Tabulation:**

Replace recursion with iterative DP table filling.

```python
def tabulation(n, weight, cost, capacity):
    # Create DP table
    dp = [[0] * (capacity + 1) for _ in range(n)]
    
    # Base case: only first piece length available
    for wt in range(capacity + 1):
        dp[0][wt] = cost[0] * (wt // weight[0])
    
    # Fill table for remaining piece lengths
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Option 1: Cut a piece of current length
            if weight[index] <= cap:
                # Use dp[index] (same row) to allow multiple cuts
                include = cost[index] + dp[index][cap - weight[index]]
            else:
                include = 0
            
            # Option 2: Don't cut this length
            exclude = dp[index-1][cap]
            
            dp[index][cap] = max(include, exclude)
    
    return dp[n-1][capacity]
```

**Time Complexity:** O(n × N)  
**Space Complexity:** O(n × N)

---

### Tabulation Example

**Input:** price = [1, 5, 8, 9], N = 4

**DP Table:**

| Index\Length | 0 | 1 | 2 | 3 | 4 |
|--------------|---|---|---|---|---|
| 0 (len=1, price=1) | 0 | 1 | 2 | 3 | 4 |
| 1 (len=2, price=5) | 0 | 1 | 5 | 6 | 10 |
| 2 (len=3, price=8) | 0 | 1 | 5 | 8 | 10 |
| 3 (len=4, price=9) | 0 | 1 | 5 | 8 | **10** |

**Explanation:**
- **Row 0:** Only cuts of length 1 available. Rod of length 4 → cut 4 pieces of length 1 → 1×4 = 4
- **Row 1:** Cuts of length 1 and 2 available.
  - Length 4: Best is 2 cuts of length 2 → 5+5 = **10**
- **Row 2:** Cuts of length 1, 2, 3 available.
  - Length 4: Still best is 2 cuts of length 2 → **10** (better than 8+1=9)
- **Row 3:** All cuts available.
  - Length 4: Best remains **10** (2 cuts of length 2)

---

### 4. Space Optimized Solution

**Conversion from Tabulation → Space Optimization:**

Use two 1D arrays instead of 2D table since each row only depends on the previous row.

```python
def optimized(n, weight, cost, capacity):
    # Two arrays instead of 2D table
    dp_curr = [0] * (capacity + 1)
    dp_prev = [0] * (capacity + 1)
    
    # Base case: only first piece length available
    for wt in range(capacity + 1):
        dp_prev[wt] = cost[0] * (wt // weight[0])
    
    # Fill for remaining piece lengths
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Option 1: Cut a piece of current length
            if weight[index] <= cap:
                # Use dp_curr (current row) for multiple cuts
                include = cost[index] + dp_curr[cap - weight[index]]
            else:
                include = 0
            
            # Option 2: Don't cut this length
            exclude = dp_prev[cap]
            
            dp_curr[cap] = max(include, exclude)
        
        # Move current to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[capacity]
```

**Time Complexity:** O(n × N)  
**Space Complexity:** O(N)

---

## Usage

```python
price = [1, 6, 8, 9, 10, 19, 7, 20]
n = len(price)
capacity = n
weight = [length for length in range(1, n+1)]
cost = price

print(solve(n, weight, cost, n-1, capacity))  # Recursion

memo = [[None] * (capacity + 1) for _ in range(n)]
print(solve_memo(n, weight, cost, n-1, capacity, memo))  # Memoization

print(tabulation(n, weight, cost, capacity))  # Tabulation

print(optimized(n, weight, cost, capacity))  # Space Optimized
```

**Output:** 25

---

## Key Insights: Rod Cutting vs Unbounded Knapsack

### Similarities

1. **Both allow unlimited picks:** 
   - Rod Cutting: Cut same length multiple times
   - Unbounded Knapsack: Pick same item multiple times

2. **Both use same index in include case:**
   - `f(index, capacity - weight[index])` not `f(index-1, ...)`

3. **Both have same recurrence relation:**
   - `f(index, capacity) = max(cost[index] + f(index, capacity - weight[index]), f(index-1, capacity))`

4. **Both have same base case pattern:**
   - `f(0, capacity) = cost[0] × (capacity // weight[0])`

### The Only Difference

**Rod Cutting** has a special constraint: `weight = [1, 2, 3, ..., N]`
- The "items" (piece lengths) are always sequential: 1, 2, 3, up to N
- But algorithmically, it's solved exactly like unbounded knapsack!

### Visual Comparison

```
Rod Cutting Problem:
┌─────────────────────────┐
│   Rod of length N       │
│   Cut into pieces       │
│   [1, 2, 3, ..., N]     │
│   Maximize total price  │
└─────────────────────────┘
           ↓
      Transform
           ↓
┌─────────────────────────┐
│   Knapsack capacity N   │
│   Items with weights    │
│   [1, 2, 3, ..., N]     │
│   Maximize total value  │
└─────────────────────────┘
Unbounded Knapsack Problem
```

---

## Complexity Summary

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n × N) | O(n × N) + O(n) |
| Tabulation | O(n × N) | O(n × N) |
| Space Optimized | O(n × N) | O(N) |

Where n = number of different piece lengths (= N), N = total rod length

---

# Minimum Coins Problem

## Problem Statement

Given an integer array of coins representing coins of different denominations and an integer amount representing a total amount of money. Return the fewest number of coins that are needed to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

**Note:** There are infinite numbers of coins of each type (unbounded).

### Examples

**Example 1:**
```
Input: coins = [1, 2, 5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1. We need 3 coins to make up the amount 11.
```

**Example 2:**
```
Input: coins = [2, 5], amount = 3
Output: -1
Explanation: It's not possible to make amount 3 with coins 2 and 5.
```

---

## Step-by-Step Approach

### Step 1: Define The Problem

We have a target sum and an array of coin denominations. We need to make the target sum using the **minimum number of coins**. Each coin can be used multiple times (unbounded).

### Step 2: Represent the Problem Programmatically

Since we need to check all possible ways and find the minimum count, we use the **pick or not-pick technique**.

**Key Difference from Unbounded Knapsack:** Instead of maximizing value, we're **minimizing count**.

**Function Definition:**
```
f(index, amount) → Minimum number of coins needed to make 'amount' using coins from index 0 to index
```

### Step 3: Base Cases

1. **If amount is 0:** No coins needed
   - `f(index, 0) = 0`

2. **If we're at the first coin (index == 0):**
   - If `amount % coins[0] == 0`, we need `amount // coins[0]` coins
   - Otherwise, impossible to make the amount: `return infinity`
   - `f(0, amount) = amount // coins[0]` if divisible, else `∞`

### Step 4: Recurrence Relation

For each coin at index, we have two choices:

1. **Include the coin:** `include = 1 + f(index, amount - coins[index])`
   - Add 1 to count (we took one coin)
   - Stay at same index (unbounded - can use same coin again)
   - Only possible if `coins[index] <= amount`

2. **Exclude the coin:** `exclude = f(index-1, amount)`
   - Move to previous coin

**Recurrence:** `f(index, amount) = min(include, exclude)`

**Key Differences from Unbounded Knapsack:**
- Use `min()` instead of `max()` (minimizing count vs maximizing value)
- Add `1` instead of `cost[index]` (counting coins vs summing values)
- Use `infinity` for impossible cases instead of `0`

---

## Recursion Tree Diagram

```
Example: coins=[1,2,5], amount=11

                        f(2, 11)
                       /        \
              (include)          (exclude)
            1+f(2,6)             f(1,11)
               /    \            /     \
         1+f(2,1)  f(1,6)   1+f(1,9)  f(0,11)
            /  \      |         |         |
      1+f(2,-4) ...  ...       ...       11
          |                              (11 coins
         ∞                               of value 1)
      (invalid)

Optimal Path:
f(2,11) → include coin 5 → f(2,6) → include coin 5 → f(2,1) → exclude → f(1,1) → include coin 1 → f(1,0) = 0
Result: 1 + 1 + 1 = 3 coins (5 + 5 + 1)
```

---

## Solutions

### 1. Recursive Solution

```python
def solve(n, coins, index, amount):
    # Base case: amount is 0, no coins needed
    if amount == 0:
        return 0
    
    # Base case: only first coin available
    if index == 0:
        if amount % coins[0] == 0:
            return amount // coins[0]
        else:
            return float('inf')  # Impossible to make amount
    
    # Option 1: Include current coin (if it fits)
    if coins[index] <= amount:
        # Add 1 to count and stay at same index
        include = 1 + solve(n, coins, index, amount - coins[index])
    else:
        include = float('inf')
    
    # Option 2: Exclude current coin
    exclude = solve(n, coins, index-1, amount)
    
    # Return minimum
    return min(include, exclude)
```

**Time Complexity:** O(2^n) in worst case  
**Space Complexity:** O(n) - Recursion stack depth

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion → Memoization:**

1. Create a memoization table `memo[index][amount]` initialized with `None`
2. Before computing, check if result already exists
3. If yes, return cached value
4. If no, compute, cache, and return

```python
def solve_memo(n, coins, index, amount, memo):
    # Return cached result if available
    if memo[index][amount] != None:
        return memo[index][amount]
    
    # Base case: amount is 0, no coins needed
    if amount == 0:
        return 0
    
    # Base case: only first coin available
    if index == 0:
        if amount % coins[0] == 0:
            return amount // coins[0]
        else:
            return float('inf')
    
    # Option 1: Include current coin (if it fits)
    if coins[index] <= amount:
        # Add 1 to count and stay at same index
        include = 1 + solve_memo(n, coins, index, amount - coins[index], memo)
    else:
        include = float('inf')
    
    # Option 2: Exclude current coin
    exclude = solve_memo(n, coins, index-1, amount, memo)
    
    # Cache and return minimum
    memo[index][amount] = min(include, exclude)
    return memo[index][amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(n × amount) + O(n)

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization → Tabulation:**

1. Replace recursion with iteration
2. Create DP table `dp[index][amount]` initialized with `infinity`
3. Fill base cases first
4. Iterate through indices and amounts
5. Use table lookups instead of recursive calls

```python
def tabulation(n, coins, amount):
    # Create DP table initialized with infinity
    dp = [[float('inf')] * (amount + 1) for _ in range(n)]
    
    # Base case: amount = 0, need 0 coins
    for index in range(n):
        dp[index][0] = 0
    
    # Base case: only first coin available
    for amt in range(1, amount + 1):
        if amt % coins[0] == 0:
            dp[0][amt] = amt // coins[0]
        else:
            dp[0][amt] = float('inf')
    
    # Fill table for remaining coins
    for index in range(1, n):
        for amt in range(1, amount + 1):
            # Option 1: Include current coin
            if coins[index] <= amt:
                # Use dp[index] (same row) for unbounded property
                include = 1 + dp[index][amt - coins[index]]
            else:
                include = float('inf')
            
            # Option 2: Exclude current coin
            exclude = dp[index-1][amt]
            
            dp[index][amt] = min(include, exclude)
    
    return dp[n-1][amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(n × amount)

---

### Tabulation Example

**Input:** coins = [1, 2, 5], amount = 11

**DP Table:**

| Index\Amount | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|--------------|---|---|---|---|---|---|---|---|---|---|----|----|
| 0 (coin=1) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| 1 (coin=2) | 0 | 1 | 1 | 2 | 2 | 3 | 3 | 4 | 4 | 5 | 5 | 6 |
| 2 (coin=5) | 0 | 1 | 1 | 2 | 2 | 1 | 2 | 2 | 3 | 3 | 2 | **3** |

**Explanation:**
- **Row 0 (coin=1):** Can make any amount using coins of 1. Amount 11 needs 11 coins.
- **Row 1 (coins=1,2):** Can use coin 2 to reduce count.
  - Amount 11: Use 5 coins of 2 and 1 coin of 1 → 6 coins
- **Row 2 (coins=1,2,5):** Can use coin 5 to further reduce count.
  - Amount 11: Use 2 coins of 5 and 1 coin of 1 → **3 coins** (5+5+1)

---

### 4. Space Optimized Solution

**Conversion from Tabulation → Space Optimization:**

1. Observe that `dp[index][amt]` depends on:
   - `dp[index][amt - coins[index]]` (same row, earlier column)
   - `dp[index-1][amt]` (previous row, same column)

2. Use two 1D arrays: `dp_prev` and `dp_curr`

```python
def optimized(n, coins, amount):
    # Two arrays instead of 2D table
    dp_curr = [float('inf')] * (amount + 1)
    dp_prev = [float('inf')] * (amount + 1)
    
    # Base case: amount = 0
    for index in range(n):
        dp_prev[0] = 0
        dp_curr[0] = 0
    
    # Base case: only first coin available
    for amt in range(1, amount + 1):
        if amt % coins[0] == 0:
            dp_prev[amt] = amt // coins[0]
        else:
            dp_prev[amt] = float('inf')
    
    # Fill for remaining coins
    for index in range(1, n):
        for amt in range(1, amount + 1):
            # Option 1: Include current coin
            if coins[index] <= amt:
                # Use dp_curr (current row) for unbounded property
                include = 1 + dp_curr[amt - coins[index]]
            else:
                include = float('inf')
            
            # Option 2: Exclude current coin
            exclude = dp_prev[amt]
            
            dp_curr[amt] = min(include, exclude)
        
        # Move current to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(amount)

---

## Usage

```python
amount = 11
coins = [1, 2, 5]
n = len(coins)

# Recursion
ans = solve(n, coins, n-1, amount)

# Memoization
memo = [[None] * (amount + 1) for _ in range(n)]
ans = solve_memo(n, coins, n-1, amount, memo)

# Tabulation
ans = tabulation(n, coins, amount)

# Space Optimized
ans = optimized(n, coins, amount)

# Handle impossible case
if ans == float('inf'):
    print(-1)
else:
    print(ans)
```

**Output:** 3

---

## Comparison with Unbounded Knapsack

| Aspect | Unbounded Knapsack | Minimum Coins |
|--------|-------------------|---------------|
| **Goal** | Maximize value | Minimize count |
| **Optimization Function** | `max(include, exclude)` | `min(include, exclude)` |
| **Include Formula** | `cost[index] + f(...)` | `1 + f(...)` |
| **Impossible Case Value** | 0 | `infinity` |
| **Base Case (index=0)** | `cost[0] × (capacity // weight[0])` | `amount // coins[0]` if divisible, else `∞` |
| **Unbounded Property** | Both use `f(index, ...)` | Both use `f(index, ...)` |

### Structural Similarity

Both problems share the same structure:
- **Unbounded property:** Can pick same item/coin multiple times
- **Recurrence uses same index:** `f(index, remaining)` not `f(index-1, remaining)`
- **Two choices:** Include or exclude current item/coin

The only differences are:
- **Objective:** Maximize vs minimize
- **What we're adding:** Value vs count (1)
- **Invalid state representation:** 0 vs infinity

---

## Complexity Summary

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n × amount) | O(n × amount) + O(n) |
| Tabulation | O(n × amount) | O(n × amount) |
| Space Optimized | O(n × amount) | O(amount) |

Where n = number of coin denominations, amount = target amount

---

# Coin Change II Problem

## Problem Statement

Given an array coins of n integers representing coin denominations, find the number of distinct combinations that sum up to a specified amount of money. If it's impossible to achieve the exact amount with any combination of coins, return 0.

**Note:** 
- A single coin can be used any number of times (unbounded)
- Return answer modulo 10^9 + 7

### Examples

**Example 1:**
```
Input: coins = [2, 4, 10], amount = 10
Output: 4
Explanation: The four combinations are:
10 = 10
10 = 4 + 4 + 2
10 = 4 + 2 + 2 + 2
10 = 2 + 2 + 2 + 2 + 2
```

**Example 2:**
```
Input: coins = [5], amount = 5
Output: 1
Explanation: There is one combination: 5 = 5.
```

---

## Step-by-Step Approach

### Step 1: Define The Problem

We have a target sum and an array of coin denominations. We need to find the **total number of ways** to make the target sum. Each coin can be used multiple times (unbounded).

### Step 2: Represent the Problem Programmatically

Since we need to count all possible ways, we use the **pick or not-pick technique**.

**Function Definition:**
```
f(index, amount) → Number of ways to make 'amount' using coins from index 0 to index
```

### Step 3: Base Cases

1. **If amount is 0:** There's exactly one way - use no coins
   - `f(index, 0) = 1`

2. **If we're at the first coin (index == 0):**
   - If `amount % coins[0] == 0`, there's exactly 1 way (use coin 0 multiple times)
   - Otherwise, 0 ways
   - `f(0, amount) = 1` if divisible, else `0`

### Step 4: Recurrence Relation

For each coin at index, we have two choices:

1. **Include the coin:** `include = f(index, amount - coins[index])`
   - Stay at same index (unbounded - can use same coin again)
   - Only possible if `coins[index] <= amount`

2. **Exclude the coin:** `exclude = f(index-1, amount)`
   - Move to previous coin

**Recurrence:** `f(index, amount) = include + exclude`

**Key Differences from Other Coin Problems:**
- **Coin Change II:** Use `+` (count all ways)
- **Minimum Coins:** Use `min()` (find minimum count)
- **Unbounded Knapsack:** Use `max()` (maximize value)

---

## Recursion Tree Diagram

```
Example: coins=[2,4,10], amount=10

                        f(2, 10)
                       /        \
              (include)          (exclude)
              f(2,0)             f(1,10)
                |                /     \
                1           f(1,6)    f(0,10)
                            /   \        |
                      f(1,2)  f(0,6)     1
                       /  \      |    (10=2+2+2+2+2)
                 f(1,0) f(0,2)   0
                   |      |
                   1      1
               (10=4+4+2) (10=2+4+4 or 10=4+2+4)

Total combinations found: 4
1. 10 = 10
2. 10 = 4 + 4 + 2
3. 10 = 4 + 2 + 2 + 2
4. 10 = 2 + 2 + 2 + 2 + 2
```

---

## Solutions

### 1. Recursive Solution

```python
def solve(n, coins, index, amount):
    # Base case: amount is 0, one way (use no coins)
    if amount == 0:
        return 1
    
    # Base case: only first coin available
    if index == 0:
        if amount % coins[0] == 0:
            return 1  # One way: use coin 0 multiple times
        else:
            return 0  # No way to make amount
    
    # Option 1: Include current coin (if it fits)
    if coins[index] <= amount:
        # Stay at same index to allow multiple uses
        include = solve(n, coins, index, amount - coins[index])
    else:
        include = 0
    
    # Option 2: Exclude current coin
    exclude = solve(n, coins, index-1, amount)
    
    # Return sum of both options
    return include + exclude
```

**Time Complexity:** O(2^n) in worst case  
**Space Complexity:** O(n) - Recursion stack depth

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion → Memoization:**

1. Create a memoization table `memo[index][amount]` initialized with `None`
2. Before computing, check if result already exists
3. If yes, return cached value
4. If no, compute, cache, and return

```python
def solve_memo(n, coins, index, amount, memo):
    # Return cached result if available
    if memo[index][amount] != None:
        return memo[index][amount]
    
    # Base case: amount is 0, one way (use no coins)
    if amount == 0:
        return 1
    
    # Base case: only first coin available
    if index == 0:
        if amount % coins[0] == 0:
            return 1
        else:
            return 0
    
    # Option 1: Include current coin (if it fits)
    if coins[index] <= amount:
        # Stay at same index to allow multiple uses
        include = solve_memo(n, coins, index, amount - coins[index], memo)
    else:
        include = 0
    
    # Option 2: Exclude current coin
    exclude = solve_memo(n, coins, index-1, amount, memo)
    
    # Cache and return sum
    memo[index][amount] = include + exclude
    return memo[index][amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(n × amount) + O(n)

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization → Tabulation:**

1. Replace recursion with iteration
2. Create DP table `dp[index][amount]` initialized with 0
3. Fill base cases first
4. Iterate through indices and amounts
5. Use table lookups instead of recursive calls

```python
def tabulation(n, coins, amount):
    # Create DP table initialized with 0
    dp = [[0] * (amount + 1) for _ in range(n)]
    
    # Base case: amount = 0, one way
    for index in range(n):
        dp[index][0] = 1
    
    # Base case: only first coin available
    for amt in range(1, amount + 1):
        if amt % coins[0] == 0:
            dp[0][amt] = 1
        else:
            dp[0][amt] = 0
    
    # Fill table for remaining coins
    for index in range(1, n):
        for amt in range(1, amount + 1):
            # Option 1: Include current coin
            if coins[index] <= amt:
                # Use dp[index] (same row) for unbounded property
                include = dp[index][amt - coins[index]]
            else:
                include = 0
            
            # Option 2: Exclude current coin
            exclude = dp[index-1][amt]
            
            # Sum both options
            dp[index][amt] = include + exclude
    
    return dp[n-1][amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(n × amount)

---

### Tabulation Example

**Input:** coins = [2, 4, 10], amount = 10

**DP Table:**

| Index\Amount | 0 | 2 | 4 | 6 | 8 | 10 |
|--------------|---|---|---|---|---|-----|
| 0 (coin=2) | 1 | 1 | 1 | 1 | 1 | 1 |
| 1 (coin=4) | 1 | 1 | 2 | 2 | 3 | 3 |
| 2 (coin=10) | 1 | 1 | 2 | 2 | 3 | **4** |

**Explanation:**
- **Row 0 (coin=2):** Can make any even amount using coin 2. One way for each.
  - Amount 10: [2+2+2+2+2] = 1 way
  
- **Row 1 (coins=2,4):** Can combine coins 2 and 4.
  - Amount 4: [4] or [2+2] = 2 ways
  - Amount 8: [4+4], [4+2+2], [2+2+2+2] = 3 ways
  - Amount 10: [4+4+2], [4+2+2+2], [2+2+2+2+2] = 3 ways
  
- **Row 2 (coins=2,4,10):** Add coin 10.
  - Amount 10: Previous 3 ways + [10] = **4 ways**
    1. 10
    2. 4 + 4 + 2
    3. 4 + 2 + 2 + 2
    4. 2 + 2 + 2 + 2 + 2

---

### 4. Space Optimized Solution

**Conversion from Tabulation → Space Optimization:**

1. Observe that `dp[index][amt]` depends on:
   - `dp[index][amt - coins[index]]` (same row, earlier column)
   - `dp[index-1][amt]` (previous row, same column)

2. Use two 1D arrays: `dp_prev` and `dp_curr`

```python
def optimized(n, coins, amount):
    # Two arrays instead of 2D table
    dp_curr = [0] * (amount + 1)
    dp_prev = [0] * (amount + 1)
    
    # Base case: amount = 0
    for index in range(n):
        dp_prev[0] = 1
        dp_curr[0] = 1
    
    # Base case: only first coin available
    for amt in range(1, amount + 1):
        if amt % coins[0] == 0:
            dp_prev[amt] = 1
        else:
            dp_prev[amt] = 0
    
    # Fill for remaining coins
    for index in range(1, n):
        for amt in range(1, amount + 1):
            # Option 1: Include current coin
            if coins[index] <= amt:
                # Use dp_curr (current row) for unbounded property
                include = dp_curr[amt - coins[index]]
            else:
                include = 0
            
            # Option 2: Exclude current coin
            exclude = dp_prev[amt]
            
            # Sum both options
            dp_curr[amt] = include + exclude
        
        # Move current to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(amount)

---

## Usage

```python
amount = 10
coins = [2, 4, 10]
n = len(coins)

# Recursion
ans = solve(n, coins, n-1, amount)

# Memoization
memo = [[None] * (amount + 1) for _ in range(n)]
ans = solve_memo(n, coins, n-1, amount, memo)

# Tabulation
ans = tabulation(n, coins, amount)

# Space Optimized
ans = optimized(n, coins, amount)

# Apply modulo
print(ans % (10**9 + 7))
```

**Output:** 4

---

## Comparison with Related Problems

| Problem | Goal | Combination Function | Include Formula | Impossible Value |
|---------|------|---------------------|-----------------|------------------|
| **Coin Change II** | Count ways | `include + exclude` | `f(index, amt - coin[i])` | 0 |
| **Minimum Coins** | Minimize count | `min(include, exclude)` | `1 + f(index, amt - coin[i])` | ∞ |
| **Unbounded Knapsack** | Maximize value | `max(include, exclude)` | `val[i] + f(index, cap - wt[i])` | 0 |

### Structural Similarities

All three problems share:
1. **Unbounded property:** Can pick same item/coin multiple times
2. **Same index on include:** `f(index, ...)` not `f(index-1, ...)`
3. **Two choices per state:** Include or exclude
4. **Similar base cases:** Special handling for index 0 and capacity/amount 0

### Key Differences

| Aspect | Coin Change II |
|--------|---------------|
| **What we're counting** | Number of ways/combinations |
| **Aggregation** | Addition (`+`) |
| **Base case (amount=0)** | 1 (one way: use no coins) |
| **Base case (index=0)** | 1 if divisible, else 0 |
| **Invalid state** | 0 (no ways) |

---

## Why We Use Addition (+)

In Coin Change II, we're counting **all possible combinations**:

```
For coins = [2, 4] and amount = 6:

Using coin 4:
- Take coin 4: f(1, 2) → ways to make 2 with coins [2,4]
  - Ways: [4+2] = 1 way

Not using coin 4:
- Skip coin 4: f(0, 6) → ways to make 6 with coins [2]
  - Ways: [2+2+2] = 1 way

Total ways = 1 + 1 = 2 ways
```

We **add** because both choices give us distinct, valid combinations.

Compare with:
- **Minimum Coins:** Use `min()` because we want the best (smallest) count
- **Unbounded Knapsack:** Use `max()` because we want the best (largest) value

---

## Complexity Summary

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n × amount) | O(n × amount) + O(n) |
| Tabulation | O(n × amount) | O(n × amount) |
| Space Optimized | O(n × amount) | O(amount) |

Where n = number of coin denominations, amount = target amount
