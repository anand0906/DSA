## Table of Contents
1. [Subset Sum Equals to Target](#1-subset-sum-equals-to-target)
2. [Partition Equal Subset Sum](#2-partition-equal-subset-sum)
3. [Partition with Minimum Absolute Sum Difference](#3-partition-with-minimum-absolute-sum-difference)
4. [Count Subsets with Sum K](#4-count-subsets-with-sum-k)
5. [Count Partitions with Given Difference / Target Sum](#5-count-partitions-with-given-difference--target-sum)
6. [0/1 Knapsack](#6-01-knapsack)
7. [Unbounded Knapsack](#7-unbounded-knapsack)
8. [Rod Cutting Problem](#8-rod-cutting-problem)
9. [Minimum Coins](#9-minimum-coins)
10. [Coin Change II](#10-coin-change-ii)

---

## 1. Subset Sum Equals to Target

### Problem Description
Given an array `arr` of `n` integers and an integer `target`, determine if there is a subset of the given array with a sum equal to the given target.

### Sample Test Cases
**Input:** `arr = [1, 2, 7, 3]`, `target = 6`  
**Output:** `True`  
**Explanation:** There is a subset (1, 2, 3) with sum 6.

**Input:** `arr = [2, 3, 5]`, `target = 6`  
**Output:** `False`  
**Explanation:** There is no subset with sum 6.

---

### Approach 1: Recursion

**Intuition:** For each element, we have two choices: include it in the subset or exclude it. We recursively explore both options and check if we can achieve the target sum.

**Code:**
```python
def solve(n, arr, index, target):
    # Base case: target achieved
    if target == 0:
        return True
    
    # Base case: reached first element
    if index == 0:
        return arr[0] == target
    
    # Include current element if possible
    if arr[index] <= target:
        include = solve(n, arr, index - 1, target - arr[index])
    else:
        include = False
    
    # Exclude current element
    exclude = solve(n, arr, index - 1, target)
    
    # Return True if either choice works
    return include or exclude
```

**Time Complexity:** O(2^n) - Two choices for each element  
**Space Complexity:** O(n) - Recursion stack depth

---

### Approach 2: Memoization

**Intuition:** Store the results of subproblems to avoid recomputation. Use a 2D memo array where `memo[index][target]` stores whether we can achieve the target sum using elements from index 0 to `index`.

**Code:**
```python
def solve_memo(n, arr, index, target, memo):
    # Return cached result if available
    if memo[index][target] != None:
        return memo[index][target]
    
    # Base case: target achieved
    if target == 0:
        return True
    
    # Base case: reached first element
    if index == 0:
        return arr[0] == target
    
    # Include current element if possible
    if arr[index] <= target:
        include = solve_memo(n, arr, index - 1, target - arr[index], memo)
    else:
        include = False
    
    # Exclude current element
    exclude = solve_memo(n, arr, index - 1, target, memo)
    
    # Cache and return result
    memo[index][target] = include or exclude
    return memo[index][target]
```

**Time Complexity:** O(n × target) - Each subproblem computed once  
**Space Complexity:** O(n × target) - Memo array + recursion stack

---

### Approach 3: Tabulation

**Intuition:** Build a 2D DP table bottom-up where `dp[i][j]` represents whether we can achieve sum `j` using first `i` elements.

**Code:**
```python
def tabulation(n, arr, targetSum):
    # Initialize DP table
    dp = [[False] * (targetSum + 1) for _ in range(n)]
    
    # Base case: sum 0 is always achievable (empty subset)
    for index in range(n):
        dp[index][0] = True
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp[0][arr[0]] = True
    
    # Fill the DP table
    for index in range(1, n):
        for target in range(1, targetSum + 1):
            # Include current element if possible
            if arr[index] <= target:
                include = dp[index - 1][target - arr[index]]
            else:
                include = False
            
            # Exclude current element
            exclude = dp[index - 1][target]
            
            # Store result
            dp[index][target] = include or exclude
    
    return dp[n - 1][targetSum]
```

**Time Complexity:** O(n × target)  
**Space Complexity:** O(n × target)

---

### Approach 4: Space Optimized

**Intuition:** Since we only need the previous row to compute the current row, we can use two 1D arrays instead of a 2D table.

**Code:**
```python
def optimized(n, arr, targetSum):
    # Two arrays to track current and previous states
    dp_curr = [False] * (targetSum + 1)
    dp_prev = [False] * (targetSum + 1)
    
    # Base case: sum 0 is always achievable
    dp_prev[0] = True
    dp_curr[0] = True
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = True
    
    # Fill DP arrays
    for index in range(1, n):
        for target in range(1, targetSum + 1):
            # Include current element if possible
            if arr[index] <= target:
                include = dp_prev[target - arr[index]]
            else:
                include = False
            
            # Exclude current element
            exclude = dp_prev[target]
            
            # Store result in current array
            dp_curr[target] = include or exclude
        
        # Move current to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[targetSum]
```

**Time Complexity:** O(n × target)  
**Space Complexity:** O(target)

---

## 2. Partition Equal Subset Sum

### Problem Description
Given an array `arr` of `n` integers, return true if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal, otherwise return false.

### Sample Test Cases
**Input:** `arr = [1, 10, 21, 10]`  
**Output:** `True`  
**Explanation:** The array can be partitioned as [1, 10, 10] and [21].

**Input:** `arr = [1, 2, 3, 5]`  
**Output:** `False`  
**Explanation:** The array cannot be partitioned into equal sum subsets.

---

### Approach: Space Optimized DP

**Intuition:** If the total sum is odd, we can't partition equally. If even, we need to find a subset with sum = totalSum/2. This reduces to the subset sum problem.

**Code:**
```python
def optimized(n, arr, targetSum):
    dp_curr = [False] * (targetSum + 1)
    dp_prev = [False] * (targetSum + 1)
    
    # Base case: sum 0 is always achievable
    dp_prev[0] = True
    dp_curr[0] = True
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = True
    
    # Fill DP arrays
    for index in range(1, n):
        for target in range(1, targetSum + 1):
            # Include current element if possible
            if arr[index] <= target:
                include = dp_prev[target - arr[index]]
            else:
                include = False
            
            # Exclude current element
            exclude = dp_prev[target]
            
            dp_curr[target] = include or exclude
        
        dp_prev = dp_curr.copy()
    
    return dp_prev[targetSum]

# Main logic
n = int(input())
arr = list(map(int, input().split()))
totalSum = sum(arr)

# Check if total sum is odd
if totalSum & 1:
    print(False)
else:
    half = totalSum // 2
    print(optimized(n, arr, half))
```

**Time Complexity:** O(n × sum/2)  
**Space Complexity:** O(sum/2)

---

## 3. Partition with Minimum Absolute Sum Difference

### Problem Description
Given an array `arr` of `n` integers, partition the array into two subsets such that the absolute difference between their sums is minimized.

### Sample Test Cases
**Input:** `arr = [1, 7, 14, 5]`  
**Output:** `1`  
**Explanation:** The array can be partitioned as [1, 7, 5] and [14], with an absolute difference of 1.

**Input:** `arr = [3, 1, 6, 2, 2]`  
**Output:** `0`  
**Explanation:** The array can be partitioned as [3, 2, 2] and [6, 1], with an absolute difference of 0.

---

### Approach: DP with All Possible Sums

**Intuition:** Find all possible subset sums up to the total sum. For each possible sum S1, S2 = total - S1. Minimize |S1 - S2|.

**Code:**
```python
def optimized(n, arr, targetSum):
    dp_curr = [False] * (targetSum + 1)
    dp_prev = [False] * (targetSum + 1)
    
    # Base case: sum 0 is always achievable
    dp_prev[0] = True
    dp_curr[0] = True
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = True
    
    # Fill DP arrays
    for index in range(1, n):
        for target in range(1, targetSum + 1):
            # Include current element if possible
            if arr[index] <= target:
                include = dp_prev[target - arr[index]]
            else:
                include = False
            
            # Exclude current element
            exclude = dp_prev[target]
            
            dp_curr[target] = include or exclude
        
        dp_prev = dp_curr.copy()
    
    return dp_prev

# Main logic
arr = [1, 7, 14, 5]
n = len(arr)
total = sum(arr)

# Get all achievable sums
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

**Time Complexity:** O(n × sum)  
**Space Complexity:** O(sum)

---

## 4. Count Subsets with Sum K

### Problem Description
Given an array `arr` of `n` integers and an integer `K`, count the number of subsets of the given array that have a sum equal to K. Return the result modulo (10^9 + 7).

### Sample Test Cases
**Input:** `arr = [2, 3, 5, 16, 8, 10]`, `K = 10`  
**Output:** `3`  
**Explanation:** The subsets are [2, 8], [10], and [2, 3, 5].

**Input:** `arr = [1, 2, 3, 4, 5]`, `K = 5`  
**Output:** `3`  
**Explanation:** The subsets are [5], [2, 3], and [1, 4].

---

### Approach 1: Recursion

**Intuition:** For each element, count subsets by including it and excluding it. Base case: if target is 0, one subset (empty); if at first element, check if it equals target.

**Code:**
```python
def solve(n, arr, index, target):
    # Base case: target achieved (empty subset counts as 1)
    if target == 0:
        return 1
    
    # Base case: reached first element
    if index == 0:
        return 1 if arr[0] == target else 0
    
    # Include current element if possible
    if arr[index] <= target:
        include = solve(n, arr, index - 1, target - arr[index])
    else:
        include = 0
    
    # Exclude current element
    exclude = solve(n, arr, index - 1, target)
    
    # Return total count
    return include + exclude
```

**Time Complexity:** O(2^n)  
**Space Complexity:** O(n)

---

### Approach 2: Memoization

**Intuition:** Cache the count of subsets for each (index, target) pair to avoid recomputation.

**Code:**
```python
def solve_memo(n, arr, index, target, memo):
    # Return cached result if available
    if memo[index][target] != None:
        return memo[index][target]
    
    # Base case: target achieved
    if target == 0:
        return 1
    
    # Base case: reached first element
    if index == 0:
        return 1 if arr[0] == target else 0
    
    # Include current element if possible
    if arr[index] <= target:
        include = solve_memo(n, arr, index - 1, target - arr[index], memo)
    else:
        include = 0
    
    # Exclude current element
    exclude = solve_memo(n, arr, index - 1, target, memo)
    
    # Cache and return result
    memo[index][target] = include + exclude
    return memo[index][target]
```

**Time Complexity:** O(n × target)  
**Space Complexity:** O(n × target)

---

### Approach 3: Tabulation

**Intuition:** Build a 2D DP table where `dp[i][j]` represents the count of subsets with sum `j` using first `i` elements.

**Code:**
```python
def tabulation(n, arr, targetSum):
    # Initialize DP table
    dp = [[0] * (targetSum + 1) for _ in range(n)]
    
    # Base case: sum 0 has 1 subset (empty subset)
    for index in range(n):
        dp[index][0] = 1
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp[0][arr[0]] = 1
    
    # Fill the DP table
    for index in range(1, n):
        for target in range(1, targetSum + 1):
            # Include current element if possible
            if arr[index] <= target:
                include = dp[index - 1][target - arr[index]]
            else:
                include = False  # Should be 0 for counting
            
            # Exclude current element
            exclude = dp[index - 1][target]
            
            # Sum the counts
            dp[index][target] = include + exclude
    
    return dp[n - 1][targetSum]
```

**Time Complexity:** O(n × target)  
**Space Complexity:** O(n × target)

---

### Approach 4: Space Optimized

**Intuition:** Use two 1D arrays to track counts for previous and current rows.

**Code:**
```python
def optimized(n, arr, targetSum):
    dp_curr = [0] * (targetSum + 1)
    dp_prev = [0] * (targetSum + 1)
    
    # Base case: sum 0 has 1 subset
    dp_prev[0] = 1
    dp_curr[0] = 1
    
    # Base case: first element
    if arr[0] <= targetSum:
        dp_prev[arr[0]] = 1
    
    # Fill DP arrays
    for index in range(1, n):
        for target in range(1, targetSum + 1):
            # Include current element if possible
            if arr[index] <= target:
                include = dp_prev[target - arr[index]]
            else:
                include = 0
            
            # Exclude current element
            exclude = dp_prev[target]
            
            # Sum the counts
            dp_curr[target] = include + exclude
        
        dp_prev = dp_curr.copy()
    
    return dp_prev[targetSum]
```

**Time Complexity:** O(n × target)  
**Space Complexity:** O(target)

---

## 5. Count Partitions with Given Difference / Target Sum

### Problem Description
Given an array `arr` of `n` integers and an integer `diff`, count the number of ways to partition the array into two subsets S1 and S2 such that |S1 - S2| = diff and S1 ≥ S2.

**Alternative Problem (Target Sum):** Build an expression using integers from `nums` where each integer can be prefixed with '+' or '-' to achieve a target sum.

### Sample Test Cases
**Input:** `arr = [1, 1, 2, 3]`, `diff = 1`  
**Output:** `3`  
**Explanation:** The subsets are [1, 2] and [1, 3], [1, 3] and [1, 2], [1, 1, 2] and [3].

**Input:** `nums = [1, 2, 7, 1, 5]`, `target = 4`  
**Output:** `2`  
**Explanation:** +1+2+7-1-5=4 and -1+2+7+1-5=4

---

### Approach: Space Optimized DP with Zero Handling

**Intuition:** 
- S1 - S2 = diff and S1 + S2 = total
- Solving: S2 = (total - diff) / 2
- Find count of subsets with sum = S2
- Handle zeros specially: if arr[0]=0, we have 2 ways (pick or not pick)

**Code:**
```python
MOD = 10**9 + 7

def optimized(n, arr, targetSum):
    dp_prev = [0] * (targetSum + 1)
    dp_curr = [0] * (targetSum + 1)

    # Base case handling for zeros
    if arr[0] == 0:
        # Two options: pick or not pick (both valid)
        dp_prev[0] = 2
    else:
        dp_prev[0] = 1

    # If first element is non-zero and within target
    if arr[0] != 0 and arr[0] <= targetSum:
        dp_prev[arr[0]] = 1

    # Process rest of elements
    for index in range(1, n):
        for target in range(targetSum + 1):
            include = 0
            # Include current element if possible
            if arr[index] <= target:
                include = dp_prev[target - arr[index]]

            # Exclude current element
            exclude = dp_prev[target]
            
            # Sum counts with modulo
            dp_curr[target] = (include + exclude) % MOD

        dp_prev = dp_curr[:]  # Copy for next iteration

    return dp_prev[targetSum] % MOD


class Solution:
    def countPartitions(self, n, diff, arr):
        total = sum(arr)

        # Check validity of partition
        # S2 must be non-negative and integer
        if (total - diff) < 0 or (total - diff) % 2 != 0:
            return 0

        # Calculate target sum for second subset
        s2 = (total - diff) // 2
        return optimized(n, arr, s2)
```

**Time Complexity:** O(n × s2) where s2 = (total - diff) / 2  
**Space Complexity:** O(s2)

---

## 6. 0/1 Knapsack

### Problem Description
Given two integer arrays `val` and `wt` representing values and weights of N items, and an integer W representing knapsack capacity, determine the maximum value achievable by selecting items such that total weight doesn't exceed W. Each item can be picked at most once.

### Sample Test Cases
**Input:** `val = [60, 100, 120]`, `wt = [10, 20, 30]`, `W = 50`  
**Output:** `220`  
**Explanation:** Select items with weights 20 and 30 for value 100 + 120 = 220.

**Input:** `val = [10, 40, 30, 50]`, `wt = [5, 4, 6, 3]`, `W = 10`  
**Output:** `90`  
**Explanation:** Select items with weights 4 and 3 for value 40 + 50 = 90.

---

### Approach 1: Recursion

**Intuition:** For each item, decide to include it (if weight allows) or exclude it. Return maximum value from both choices.

**Code:**
```python
def solve(n, weight, cost, index, capacity):
    # Base case: no capacity left
    if weight == 0:
        return 0
    
    # Base case: first item
    if index == 0:
        if weight[0] <= capacity:
            return cost[0]
        return 0

    # Include current item if possible
    if weight[index] <= capacity:
        include = cost[index] + solve(n, weight, cost, index - 1, capacity - weight[index])
    else:
        include = 0
    
    # Exclude current item
    exclude = solve(n, weight, cost, index - 1, capacity)
    
    # Return maximum value
    return max(include, exclude)
```

**Time Complexity:** O(2^n)  
**Space Complexity:** O(n)

---

### Approach 2: Memoization

**Intuition:** Cache results for each (index, capacity) pair to avoid recomputation.

**Code:**
```python
def solve_memo(n, weight, cost, index, capacity, memo):
    # Return cached result if available
    if memo[index][capacity] != None:
        return memo[index][capacity]
    
    # Base case: no capacity
    if weight == 0:
        return 0
    
    # Base case: first item
    if index == 0:
        if weight[0] <= capacity:
            return cost[0]
        return 0

    # Include current item if possible
    if weight[index] <= capacity:
        include = cost[index] + solve_memo(n, weight, cost, index - 1, capacity - weight[index], memo)
    else:
        include = 0
    
    # Exclude current item
    exclude = solve_memo(n, weight, cost, index - 1, capacity, memo)
    
    # Cache and return result
    memo[index][capacity] = max(include, exclude)
    return memo[index][capacity]
```

**Time Complexity:** O(n × capacity)  
**Space Complexity:** O(n × capacity)

---

### Approach 3: Tabulation

**Intuition:** Build a 2D DP table where `dp[i][w]` represents maximum value using first `i` items with capacity `w`.

**Code:**
```python
def tabulation(n, weight, cost, capacity):
    # Initialize DP table
    dp = [[0] * (capacity + 1) for _ in range(n)]
    
    # Base case: first item
    for wt in range(capacity + 1):
        if weight[0] <= wt:
            dp[0][wt] = cost[0]
    
    # Fill the DP table
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Include current item if possible
            if weight[index] <= cap:
                include = cost[index] + dp[index - 1][cap - weight[index]]
            else:
                include = 0
            
            # Exclude current item
            exclude = dp[index - 1][cap]
            
            # Store maximum value
            dp[index][cap] = max(include, exclude)
    
    return dp[n - 1][capacity]
```

**Time Complexity:** O(n × capacity)  
**Space Complexity:** O(n × capacity)

---

### Approach 4: Space Optimized

**Intuition:** Use two 1D arrays instead of a 2D table since we only need the previous row.

**Code:**
```python
def optimized(n, weight, cost, capacity):
    dp_curr = [0] * (capacity + 1)
    dp_prev = [0] * (capacity + 1)
    
    # Base case: first item
    for wt in range(capacity + 1):
        if weight[0] <= wt:
            dp_prev[wt] = cost[0]
    
    # Fill DP arrays
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Include current item if possible
            if weight[index] <= cap:
                include = cost[index] + dp_prev[cap - weight[index]]
            else:
                include = 0
            
            # Exclude current item
            exclude = dp_prev[cap]
            
            # Store maximum value
            dp_curr[cap] = max(include, exclude)
        
        # Move current to previous
        dp_prev = dp_curr.copy()
    
    return dp_prev[capacity]
```

**Time Complexity:** O(n × capacity)  
**Space Complexity:** O(capacity)

---

## 7. Unbounded Knapsack

### Problem Description
Given two integer arrays `val` and `wt` representing values and weights of N items, and an integer W representing knapsack capacity, determine the maximum value achievable. An infinite supply of each item is available.

### Sample Test Cases
**Input:** `val = [5, 11, 13]`, `wt = [2, 4, 6]`, `W = 10`  
**Output:** `27`  
**Explanation:** Select 2 items with weight 4 and 1 item with weight 2: 11+11+5 = 27.

**Input:** `val = [10, 40, 50, 70]`, `wt = [1, 3, 4, 5]`, `W = 8`  
**Output:** `110`  
**Explanation:** Select items with weights 3 and 5: 40 + 70 = 110.

---

### Approach 1: Recursion

**Intuition:** Unlike 0/1 knapsack, after including an item, we stay at the same index (can pick again). Base case: for first item, take it as many times as capacity allows.

**Code:**
```python
def solve(n, weight, cost, index, capacity):
    # Base case: first item (take as many as possible)
    if index == 0:
        return cost[0] * (capacity // weight[0])
    
    # Include current item if possible (stay at same index)
    if weight[index] <= capacity:
        include = cost[index] + solve(n, weight, cost, index, capacity - weight[index])
    else:
        include = 0
    
    # Exclude current item (move to previous)
    exclude = solve(n, weight, cost, index - 1, capacity)
    
    # Return maximum value
    return max(include, exclude)
```

**Time Complexity:** Exponential  
**Space Complexity:** O(n)

---

### Approach 2: Memoization

**Intuition:** Cache results to avoid recomputation. Same recursion logic with memoization.

**Code:**
```python
def solve_memo(n, weight, cost, index, capacity, memo):
    # Return cached result if available
    if memo[index][capacity] != None:
        return memo[index][capacity]
    
    # Base case: first item
    if index == 0:
        return cost[0] * (capacity // weight[0])

    # Include current item if possible (stay at same index)
    if weight[index] <= capacity:
        include = cost[index] + solve_memo(n, weight, cost, index, capacity - weight[index], memo)
    else:
        include = 0
    
    # Exclude current item
    exclude = solve_memo(n, weight, cost, index - 1, capacity, memo)
    
    # Cache and return result
    memo[index][capacity] = max(include, exclude)
    return memo[index][capacity]
```

**Time Complexity:** O(n × capacity)  
**Space Complexity:** O(n × capacity)

---

### Approach 3: Tabulation

**Intuition:** Build a 2D DP table. Key difference: when including item at index i, look at `dp[i][cap-weight[i]]` (same row) instead of previous row.

**Code:**
```python
def tabulation(n, weight, cost, capacity):
    # Initialize DP table
    dp = [[0] * (capacity + 1) for _ in range(n)]
    
    # Base case: first item (take as many as possible)
    for wt in range(capacity + 1):
        dp[0][wt] = cost[0] * (wt // weight[0])
    
    # Fill the DP table
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Include current item (look at same row)
            if weight[index] <= cap:
                include = cost[index] + dp[index][cap - weight[index]]
            else:
                include = 0
            
            # Exclude current item
            exclude = dp[index - 1][cap]
            
            # Store maximum value
            dp[index][cap] = max(include, exclude)
    
    return dp[n - 1][capacity]
```

**Time Complexity:** O(n × capacity)  
**Space Complexity:** O(n × capacity)

---

### Approach 4: Space Optimized

**Intuition:** Use two 1D arrays. When including, look at current array (since we can reuse items).

**Code:**
```python
def optimized(n, weight, cost, capacity):
    dp_curr = [0] * (capacity + 1)
    dp_prev = [0] * (capacity + 1)
    
    # Base case: first item
    for wt in range(capacity + 1):
        dp_prev[wt] = cost[0] * (wt // weight[0])
    
    # Fill DP arrays
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Include current item (look at current array)
            if weight[index] <= cap:
                include = cost[index] + dp_curr[cap - weight[index]]
            else:
                include = 0
            
            # Exclude current item
            exclude = dp_prev[cap]
            
            # Store maximum value
            dp_curr[cap] = max(include, exclude)
        
        # Move current to previous
        dp_prev = dp_curr.copy()
    
    return dp_prev[capacity]
```

**Time Complexity:** O(n × capacity)  
**Space Complexity:** O(capacity)

---

## 8. Rod Cutting Problem

### Problem Description
Given a rod of length N inches and an array `price[]` where `price[i]` denotes the value of a piece of rod of length i inches (1-based indexing), determine the maximum value obtainable by cutting up the rod and selling the pieces. Make any number of cuts, or none at all, and sell the resulting pieces.

### Sample Test Cases
**Input:** `price = [1, 6, 8, 9, 10, 19, 7, 20]`, `N = 8`  
**Output:** `25`  
**Explanation:** Cut the rod into lengths of 2 and 6 for a total price of 6 + 19 = 25.

**Input:** `price = [1, 5, 8, 9]`, `N = 4`  
**Output:** `10`  
**Explanation:** Cut the rod into lengths of 2 and 2 for a total price of 5 + 5 = 10.

---

### Approach: Unbounded Knapsack Variant

**Intuition:** This is an unbounded knapsack problem where:
- Items = rod pieces of different lengths [1, 2, 3, ..., N]
- Weights = lengths themselves
- Values = prices at those lengths
- Capacity = total rod length N

We can cut the same length multiple times (unbounded).

**Code:**
```python
def solve(n, weight, cost, index, capacity):
    # Base case: first piece (cut as many times as possible)
    if index == 0:
        return cost[0] * (capacity // weight[0])
    
    # Include current piece if possible (stay at same index)
    if weight[index] <= capacity:
        include = cost[index] + solve(n, weight, cost, index, capacity - weight[index])
    else:
        include = 0
    
    # Exclude current piece
    exclude = solve(n, weight, cost, index - 1, capacity)
    
    return max(include, exclude)

def solve_memo(n, weight, cost, index, capacity, memo):
    # Return cached result if available
    if memo[index][capacity] != None:
        return memo[index][capacity]
    
    # Base case: first piece
    if index == 0:
        return cost[0] * (capacity // weight[0])

    # Include current piece if possible
    if weight[index] <= capacity:
        include = cost[index] + solve_memo(n, weight, cost, index, capacity - weight[index], memo)
    else:
        include = 0
    
    # Exclude current piece
    exclude = solve_memo(n, weight, cost, index - 1, capacity, memo)
    
    # Cache and return result
    memo[index][capacity] = max(include, exclude)
    return memo[index][capacity]

def tabulation(n, weight, cost, capacity):
    # Initialize DP table
    dp = [[0] * (capacity + 1) for _ in range(n)]
    
    # Base case: first piece
    for wt in range(capacity + 1):
        dp[0][wt] = cost[0] * (wt // weight[0])
    
    # Fill the DP table
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Include current piece (look at same row)
            if weight[index] <= cap:
                include = cost[index] + dp[index][cap - weight[index]]
            else:
                include = 0
            
            # Exclude current piece
            exclude = dp[index - 1][cap]
            
            dp[index][cap] = max(include, exclude)
    
    return dp[n - 1][capacity]

def optimized(n, weight, cost, capacity):
    dp_curr = [0] * (capacity + 1)
    dp_prev = [0] * (capacity + 1)
    
    # Base case: first piece
    for wt in range(capacity + 1):
        dp_prev[wt] = cost[0] * (wt // weight[0])
    
    # Fill DP arrays
    for index in range(1, n):
        for cap in range(1, capacity + 1):
            # Include current piece (look at current array)
            if weight[index] <= cap:
                include = cost[index] + dp_curr[cap - weight[index]]
            else:
                include = 0
            
            # Exclude current piece
            exclude = dp_prev[cap]
            
            dp_curr[cap] = max(include, exclude)
        
        dp_prev = dp_curr.copy()
    
    return dp_prev[capacity]

# Main logic
price = [1, 6, 8, 9, 10, 19, 7, 20]
n = len(price)
capacity = n
weight = [length for length in range(1, n + 1)]  # Lengths: 1, 2, 3, ..., n
cost = price
```

**Time Complexity:** O(n²)  
**Space Complexity:** O(n) for optimized version

---

## 9. Minimum Coins

### Problem Description
Given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money, return the fewest number of coins needed to make up that amount. If that amount cannot be made up by any combination of coins, return -1. There are infinite numbers of coins of each type.

### Sample Test Cases
**Input:** `coins = [1, 2, 5]`, `amount = 11`  
**Output:** `3`  
**Explanation:** 11 = 5 + 5 + 1. We need 3 coins.

**Input:** `coins = [2, 5]`, `amount = 3`  
**Output:** `-1`  
**Explanation:** It's not possible to make amount 3 with coins 2 and 5.

---

### Approach 1: Recursion

**Intuition:** For each coin, try including it (add 1 to count and reduce amount) or excluding it. Base case: if amount is 0, need 0 coins; if at first coin, check if amount is divisible by it.

**Code:**
```python
def solve(n, coins, index, amount):
    # Base case: amount achieved
    if amount == 0:
        return 0
    
    # Base case: first coin
    if index == 0:
        if amount % coins[0] == 0:
            return amount // coins[0]
        else:
            return float('inf')  # Not possible

    # Include current coin if possible (stay at same index)
    if coins[index] <= amount:
        include = 1 + solve(n, coins, index, amount - coins[index])
    else:
        include = float('inf')
    
    # Exclude current coin
    exclude = solve(n, coins, index - 1, amount)
    
    # Return minimum coins needed
    return min(include, exclude)
```

**Time Complexity:** Exponential  
**Space Complexity:** O(n)

---

### Approach 2: Memoization

**Intuition:** Cache results to avoid recomputation. Store minimum coins needed for each (index, amount) pair.

**Code:**
```python
def solve_memo(n, coins, index, amount, memo):
    # Return cached result if available
    if memo[index][amount] != None:
        return memo[index][amount]
    
    # Base case: amount achieved
    if amount == 0:
        return 0
    
    # Base case: first coin
    if index == 0:
        if amount % coins[0] == 0:
            return amount // coins[0]
        else:
            return float('inf')

    # Include current coin if possible
    if coins[index] <= amount:
        include = 1 + solve_memo(n, coins, index, amount - coins[index], memo)
    else:
        include = float('inf')
    
    # Exclude current coin
    exclude = solve_memo(n, coins, index - 1, amount, memo)
    
    # Cache and return result
    memo[index][amount] = min(include, exclude)
    return memo[index][amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(n × amount)

---

### Approach 3: Tabulation

**Intuition:** Build a 2D DP table where `dp[i][j]` represents minimum coins needed to make amount `j` using first `i` coins.

**Code:**
```python
def tabulation(n, coins, amount):
    # Initialize DP table with infinity
    dp = [[float('inf')] * (amount + 1) for _ in range(n)]
    
    # Base case: amount 0 needs 0 coins
    for index in range(n):
        dp[index][0] = 0
    
    # Base case: first coin
    for amt in range(1, amount + 1):
        if amt % coins[0] == 0:
            dp[0][amt] = amt // coins[0]
        else:
            dp[0][amt] = float('inf')

    # Fill the DP table
    for index in range(1, n):
        for amt in range(1, amount + 1):
            # Include current coin (look at same row)
            if coins[index] <= amt:
                include = 1 + dp[index][amt - coins[index]]
            else:
                include = float('inf')
            
            # Exclude current coin
            exclude = dp[index - 1][amt]
            
            # Store minimum
            dp[index][amt] = min(include, exclude)
    
    return dp[n - 1][amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(n × amount)

---

### Approach 4: Space Optimized

**Intuition:** Use two 1D arrays. When including a coin, look at current array (unbounded knapsack).

**Code:**
```python
def optimized(n, coins, amount):
    dp_curr = [float('inf')] * (amount + 1)
    dp_prev = [float('inf')] * (amount + 1)
    
    # Base case: amount 0 needs 0 coins
    for index in range(n):
        dp_prev[0] = 0
        dp_curr[0] = 0
    
    # Base case: first coin
    for amt in range(1, amount + 1):
        if amt % coins[0] == 0:
            dp_prev[amt] = amt // coins[0]
        else:
            dp_prev[amt] = float('inf')

    # Fill DP arrays
    for index in range(1, n):
        for amt in range(1, amount + 1):
            # Include current coin (look at current array)
            if coins[index] <= amt:
                include = 1 + dp_curr[amt - coins[index]]
            else:
                include = float('inf')
            
            # Exclude current coin
            exclude = dp_prev[amt]
            
            # Store minimum
            dp_curr[amt] = min(include, exclude)
        
        dp_prev = dp_curr.copy()
    
    return dp_prev[amount]

# Main logic
amount = 11
coins = [1, 2, 5]
n = len(coins)
ans = optimized(n, coins, amount)

# Check if solution exists
if ans == float('inf'):
    print(-1)
else:
    print(ans)
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(amount)

---

## 10. Coin Change II

### Problem Description
Given an array `coins` of n integers representing coin denominations, find the number of distinct combinations that sum up to a specified amount of money. If it's impossible to achieve the exact amount, return 0. Single coin can be used any number of times. Return answer modulo 10^9+7.

### Sample Test Cases
**Input:** `coins = [2, 4, 10]`, `amount = 10`  
**Output:** `4`  
**Explanation:** The four combinations are:
- 10 = 10
- 10 = 4 + 4 + 2
- 10 = 4 + 2 + 2 + 2
- 10 = 2 + 2 + 2 + 2 + 2

**Input:** `coins = [5]`, `amount = 5`  
**Output:** `1`  
**Explanation:** There is one combination: 5 = 5.

---

### Approach 1: Recursion

**Intuition:** For each coin, count combinations by including it (stay at same index) or excluding it. Base case: amount 0 has 1 way (empty combination); if at first coin, check if amount is divisible.

**Code:**
```python
def solve(n, coins, index, amount):
    # Base case: amount achieved (1 way)
    if amount == 0:
        return 1
    
    # Base case: first coin
    if index == 0:
        if amount % coins[0] == 0:
            return 1
        else:
            return 0

    # Include current coin if possible (stay at same index)
    if coins[index] <= amount:
        include = solve(n, coins, index, amount - coins[index])
    else:
        include = 0
    
    # Exclude current coin
    exclude = solve(n, coins, index - 1, amount)
    
    # Return total combinations
    return include + exclude
```

**Time Complexity:** Exponential  
**Space Complexity:** O(n)

---

### Approach 2: Memoization

**Intuition:** Cache the count of combinations for each (index, amount) pair.

**Code:**
```python
def solve_memo(n, coins, index, amount, memo):
    # Return cached result if available
    if memo[index][amount] != None:
        return memo[index][amount]
    
    # Base case: amount achieved
    if amount == 0:
        return 1
    
    # Base case: first coin
    if index == 0:
        if amount % coins[0] == 0:
            return 1
        else:
            return 0

    # Include current coin if possible
    if coins[index] <= amount:
        include = solve_memo(n, coins, index, amount - coins[index], memo)
    else:
        include = 0
    
    # Exclude current coin
    exclude = solve_memo(n, coins, index - 1, amount, memo)
    
    # Cache and return result
    memo[index][amount] = include + exclude
    return memo[index][amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(n × amount)

---

### Approach 3: Tabulation

**Intuition:** Build a 2D DP table where `dp[i][j]` represents count of combinations to make amount `j` using first `i` coins.

**Code:**
```python
def tabulation(n, coins, amount):
    # Initialize DP table
    dp = [[0] * (amount + 1) for _ in range(n)]
    
    # Base case: amount 0 has 1 way
    for index in range(n):
        dp[index][0] = 1
    
    # Base case: first coin
    for amt in range(1, amount + 1):
        if amt % coins[0] == 0:
            dp[0][amt] = 1
        else:
            dp[0][amt] = 0

    # Fill the DP table
    for index in range(1, n):
        for amt in range(1, amount + 1):
            # Include current coin (look at same row)
            if coins[index] <= amt:
                include = dp[index][amt - coins[index]]
            else:
                include = 0
            
            # Exclude current coin
            exclude = dp[index - 1][amt]
            
            # Sum the combinations
            dp[index][amt] = include + exclude
    
    return dp[n - 1][amount]
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(n × amount)

---

### Approach 4: Space Optimized

**Intuition:** Use two 1D arrays. When including a coin, look at current array (unbounded).

**Code:**
```python
def optimized(n, coins, amount):
    dp_curr = [0] * (amount + 1)
    dp_prev = [0] * (amount + 1)
    
    # Base case: amount 0 has 1 way
    for index in range(n):
        dp_prev[0] = 1
        dp_curr[0] = 1
    
    # Base case: first coin
    for amt in range(1, amount + 1):
        if amt % coins[0] == 0:
            dp_prev[amt] = 1
        else:
            dp_prev[amt] = 0

    # Fill DP arrays
    for index in range(1, n):
        for amt in range(1, amount + 1):
            # Include current coin (look at current array)
            if coins[index] <= amt:
                include = dp_curr[amt - coins[index]]
            else:
                include = 0
            
            # Exclude current coin
            exclude = dp_prev[amt]
            
            # Sum the combinations
            dp_curr[amt] = include + exclude
        
        dp_prev = dp_curr.copy()
    
    return dp_prev[amount]

# Main logic
amount = 10
coins = [2, 4, 10]
n = len(coins)
ans = optimized(n, coins, amount)
print(ans % (10**9 + 7))
```

**Time Complexity:** O(n × amount)  
**Space Complexity:** O(amount)

---

## Summary Comparison Table

| Problem | Type | Key Idea | Return Value | Time Complexity | Space Complexity (Optimized) |
|---------|------|----------|--------------|-----------------|------------------------------|
| **Subset Sum** | 0/1 Subset | Can we achieve target sum? | Boolean | O(n × target) | O(target) |
| **Equal Partition** | 0/1 Subset | Can we split into equal sums? | Boolean | O(n × sum/2) | O(sum/2) |
| **Min Difference Partition** | 0/1 Subset | Find all possible sums, minimize difference | Integer (min diff) | O(n × sum) | O(sum) |
| **Count Subsets with Sum K** | 0/1 Subset | Count ways to achieve sum K | Integer (count) | O(n × K) | O(K) |
| **Count Partitions/Target Sum** | 0/1 Subset | Count ways with given difference | Integer (count) | O(n × s2) | O(s2) |
| **0/1 Knapsack** | 0/1 Selection | Max value with weight constraint | Integer (max value) | O(n × W) | O(W) |
| **Unbounded Knapsack** | Unlimited Use | Max value, items can repeat | Integer (max value) | O(n × W) | O(W) |
| **Rod Cutting** | Unlimited Use | Max value cutting rod | Integer (max value) | O(n²) | O(n) |
| **Minimum Coins** | Unlimited Use | Min coins to make amount | Integer (min count) | O(n × amount) | O(amount) |
| **Coin Change II** | Unlimited Use | Count ways to make amount | Integer (count) | O(n × amount) | O(amount) |

---

## Key Patterns to Remember

### 0/1 Knapsack Pattern (Each Item Used Once)
- **Include:** Move to `index-1` and reduce capacity/target
- **Exclude:** Move to `index-1` with same capacity/target
- Used in: Subset Sum, Equal Partition, Count Subsets, 0/1 Knapsack

### Unbounded Knapsack Pattern (Items Can Repeat)
- **Include:** Stay at `index` (same item can be used again)
- **Exclude:** Move to `index-1`
- Used in: Unbounded Knapsack, Rod Cutting, Min Coins, Coin Change II

### Space Optimization Key Difference
- **0/1:** Look at `dp_prev[target - arr[index]]` (previous row)
- **Unbounded:** Look at `dp_curr[target - arr[index]]` (current row)

### Base Cases
- **Boolean problems:** `target==0` returns `True`
- **Count problems:** `target==0` returns `1` (empty subset)
- **Min problems:** `target==0` returns `0` (no items needed)
- **Max problems:** `target==0` returns `0` (no value)

---

## Quick Revision Tips

1. **Identify the pattern:**
   - Can items be reused? → Unbounded
   - Each item used once? → 0/1

2. **Return type determines base case:**
   - Boolean → True/False
   - Count → 1 for base case
   - Min/Max → 0 or infinity

3. **Space optimization:**
   - Always use two arrays: `dp_curr` and `dp_prev`
   - 0/1: Look at previous array when including
   - Unbounded: Look at current array when including

4. **Handle zeros in counting problems:**
   - If first element is 0, there are 2 ways (pick or not pick)

5. **Common mistakes to avoid:**
   - Forgetting to handle modulo in count problems
   - Using wrong array (curr vs prev) in space optimization
   - Not checking if amount/target is achievable before computing
