# Partition DP Problems - Revision Notes

## 📑 Index

1. [Matrix Chain Multiplication](#1-matrix-chain-multiplication)
2. [Minimum Cost to Cut a Stick](#2-minimum-cost-to-cut-a-stick)
3. [Burst Balloons](#3-burst-balloons)
4. [Palindrome Partitioning II](#4-palindrome-partitioning-ii)
5. [Partition Array for Maximum Sum](#5-partition-array-for-maximum-sum)
6. [Problem Comparison Table](#-problem-comparison-table)

---

## 1. Matrix Chain Multiplication

### 📝 Problem Description
Given dimensions of matrices in an array, find the minimum number of scalar multiplications needed to multiply the chain of matrices. Matrix at position i has dimensions arr[i-1] × arr[i].

### 🧪 Sample Test Cases

**Example:**
```
Input: arr = [10, 20, 30, 40, 50]
Output: 38000
Explanation: Matrices are 10×20, 20×30, 30×40, 40×50
Optimal: ((A₁A₂)A₃)A₄ = 38,000 operations
```

### 💡 Approach 1: Recursion

**Intuition:**
- Try every possible partition point k between start and end
- Multiply left subchain, right subchain, then combine them
- Cost to multiply two matrices (m×n) and (n×p) = m×n×p
- Take minimum of all possible partitions

**Code:**
```python
def solve(arr, start, end):
    # Base case: only 1 matrix (start+1 == end means adjacent indices)
    if start + 1 == end:
        return 0
    
    mini = float('inf')
    
    # Try every partition point k
    for k in range(start + 1, end):
        # Cost to multiply left subchain [start...k]
        leftPart = solve(arr, start, k)
        # Cost to multiply right subchain [k...end]
        rightPart = solve(arr, k, end)
        # Cost to multiply two resulting matrices
        operations = arr[start] * arr[k] * arr[end]
        # Total cost
        total = leftPart + rightPart + operations
        mini = min(mini, total)
    
    return mini
```

**Complexity:**
- **Time:** O(2^n) - Exponential
- **Space:** O(n) - Recursion stack

### 💡 Approach 2: Memoization

**Intuition:** Cache results for (start, end) pairs to avoid recomputation

**Code:**
```python
def solve_memo(arr, start, end, memo):
    # Base case
    if start + 1 == end:
        return 0
    
    # Return cached result
    if memo[start][end] is not None:
        return memo[start][end]
    
    mini = float('inf')
    
    for k in range(start + 1, end):
        leftPart = solve_memo(arr, start, k, memo)
        rightPart = solve_memo(arr, k, end, memo)
        operations = arr[start] * arr[k] * arr[end]
        total = leftPart + rightPart + operations
        mini = min(mini, total)
    
    # Cache the result
    memo[start][end] = mini
    return memo[start][end]
```

**Complexity:**
- **Time:** O(n³) - O(n²) states × O(n) for loop
- **Space:** O(n²)

### 💡 Approach 3: Tabulation

**Intuition:** Build solutions for increasing chain lengths

**Code:**
```python
def tabulation(n, arr):
    # DP table: dp[start][end]
    dp = [[0]*n for _ in range(n)]
    
    # Fill by increasing chain length
    for length in range(2, n):  # length = end - start
        for start in range(n - length):
            end = start + length
            
            mini = float('inf')
            # Try all partition points
            for k in range(start + 1, end):
                leftPart = dp[start][k]
                rightPart = dp[k][end]
                operations = arr[start] * arr[k] * arr[end]
                total = leftPart + rightPart + operations
                mini = min(mini, total)
            
            dp[start][end] = mini
    
    # Answer: full chain from 0 to n-1
    return dp[0][n-1]
```

**Complexity:**
- **Time:** O(n³)
- **Space:** O(n²)

---

## 2. Minimum Cost to Cut a Stick

### 📝 Problem Description
Given a stick of length n and cut positions, find minimum total cost to make all cuts. Cost of a cut = length of stick being cut.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: n = 7, cuts = [1, 3, 4, 5]
Output: 16
Explanation: Order [3, 5, 1, 4] gives cost 7+4+3+2 = 16
```

**Example 2:**
```
Input: n = 9, cuts = [5, 6, 1, 4, 2]
Output: 22
```

### 💡 Approach 1: Recursion

**Intuition:**
- Sort cuts and add boundaries (0 and n)
- Similar to MCM: try cutting at each position k
- Cost of cutting = current stick length (arr[end] - arr[start])
- Find minimum total cost

**Code:**
```python
def solve(arr, start, end):
    # Base case: no cuts between start and end
    if start + 1 == end:
        return 0
    
    mini = float('inf')
    
    # Try cutting at each position k
    for k in range(start + 1, end):
        # Cost of cuts in left part
        leftPart = solve(arr, start, k)
        # Cost of cuts in right part
        rightPart = solve(arr, k, end)
        # Cost of current cut = stick length
        cost = arr[end] - arr[start]
        total = leftPart + rightPart + cost
        mini = min(mini, total)
    
    return mini
```

**Complexity:**
- **Time:** O(2^c) where c = number of cuts
- **Space:** O(c)

### 💡 Approach 2: Memoization

**Code:**
```python
def solve_memo(arr, start, end, memo):
    if start + 1 == end:
        return 0
    
    # Check cache
    if memo[start][end] is not None:
        return memo[start][end]
    
    mini = float('inf')
    
    for k in range(start + 1, end):
        leftPart = solve_memo(arr, start, k, memo)
        rightPart = solve_memo(arr, k, end, memo)
        # Cost = length of current stick
        cost = arr[end] - arr[start]
        total = leftPart + rightPart + cost
        mini = min(mini, total)
    
    memo[start][end] = mini
    return memo[start][end]
```

**Complexity:**
- **Time:** O(c³)
- **Space:** O(c²)

### 💡 Approach 3: Tabulation

**Code:**
```python
def tabulation(n, arr):
    dp = [[0]*n for _ in range(n)]
    
    # Build for increasing lengths
    for length in range(2, n):
        for start in range(n - length):
            end = start + length
            
            mini = float('inf')
            for k in range(start + 1, end):
                leftPart = dp[start][k]
                rightPart = dp[k][end]
                # Current stick length
                cost = arr[end] - arr[start]
                total = leftPart + rightPart + cost
                mini = min(mini, total)
            
            dp[start][end] = mini
    
    return dp[0][n-1]

# Usage
length = 7
arr = [5, 3, 1, 2]
arr.sort()  # Sort cuts
arr = [0] + arr + [length]  # Add boundaries
```

**Complexity:**
- **Time:** O(c³)
- **Space:** O(c²)

---

## 3. Burst Balloons

### 📝 Problem Description
Burst n balloons to maximize coins. Bursting balloon i gives coins = nums[i-1] × nums[i] × nums[i+1]. Out-of-bounds treated as 1.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [3, 1, 5, 8]
Output: 167
Explanation: [3,1,5] → [3,5] → [3] → [] gives 3×1×5 + 3×5×8 + 1×3×8 + 1×8×1 = 167
```

**Example 2:**
```
Input: nums = [1, 5]
Output: 10
```

### 💡 Approach 1: Recursion

**Intuition:**
- Think backwards: which balloon to burst LAST in range [start, end]
- If k is burst last, its neighbors are start and end
- Points = arr[start] × arr[k] × arr[end]
- Maximize total points

**Code:**
```python
def solve(arr, start, end):
    # Base case: no balloons between start and end
    if start + 1 == end:
        return 0
    
    maxi = 0
    
    # Try bursting each balloon k as the LAST one in this range
    for k in range(start + 1, end):
        # Points from left subrange (all burst before k)
        leftPart = solve(arr, start, k)
        # Points from right subrange (all burst before k)
        rightPart = solve(arr, k, end)
        # Points from bursting k last (neighbors are start and end)
        points = arr[start] * arr[k] * arr[end]
        total = leftPart + rightPart + points
        maxi = max(maxi, total)
    
    return maxi
```

**Complexity:**
- **Time:** O(2^n)
- **Space:** O(n)

### 💡 Approach 2: Memoization

**Code:**
```python
def solve_memo(arr, start, end, memo):
    if start + 1 == end:
        return 0
    
    # Return cached value
    if memo[start][end] is not None:
        return memo[start][end]
    
    maxi = 0
    
    for k in range(start + 1, end):
        leftPart = solve_memo(arr, start, k, memo)
        rightPart = solve_memo(arr, k, end, memo)
        # Burst k last when only start and end remain as neighbors
        points = arr[start] * arr[k] * arr[end]
        total = leftPart + rightPart + points
        maxi = max(maxi, total)
    
    memo[start][end] = maxi
    return memo[start][end]
```

**Complexity:**
- **Time:** O(n³)
- **Space:** O(n²)

### 💡 Approach 3: Tabulation

**Code:**
```python
def tabulation(n, arr):
    dp = [[0]*n for _ in range(n)]
    
    # Build for increasing lengths
    for length in range(2, n):
        for start in range(n - length):
            end = start + length
            
            maxi = 0
            # Try each k as last balloon to burst
            for k in range(start + 1, end):
                leftPart = dp[start][k]
                rightPart = dp[k][end]
                points = arr[start] * arr[k] * arr[end]
                total = leftPart + rightPart + points
                maxi = max(maxi, total)
            
            dp[start][end] = maxi
    
    return dp[0][n-1]

# Usage
arr = [3, 1, 5, 8]
arr = [1] + arr + [1]  # Add boundary balloons
```

**Complexity:**
- **Time:** O(n³)
- **Space:** O(n²)

---

## 4. Palindrome Partitioning II

### 📝 Problem Description
Partition string into palindromes with minimum number of cuts.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: s = "aab"
Output: 1
Explanation: ["aa", "b"] requires 1 cut
```

**Example 2:**
```
Input: s = "abaaba"
Output: 0
Explanation: Entire string is palindrome
```

### 💡 Approach 1: Recursion

**Intuition:**
- From index, try making palindrome partitions of different lengths
- If substring [index...k] is palindrome, make a cut and recurse for [k+1...n]
- Count minimum cuts needed
- Subtract 1 from result (counts partitions, not cuts)

**Code:**
```python
def isPalindrome(s, start, end):
    # Check if substring is palindrome
    while start < end:
        if s[start] != s[end]:
            return False
        start += 1
        end -= 1
    return True

def solve(n, s, index):
    # Base case: reached end of string
    if index == n:
        return 0
    
    mini = float('inf')
    
    # Try making partition at each position k
    for k in range(index, n):
        if isPalindrome(s, index, k):
            # If [index...k] is palindrome, make a cut
            cuts = 1 + solve(n, s, k + 1)
            mini = min(mini, cuts)
    
    return mini
```

**Complexity:**
- **Time:** O(2^n)
- **Space:** O(n)

### 💡 Approach 2: Memoization

**Code:**
```python
def solve_memo(n, s, index, memo):
    # Base case
    if index == n:
        return 0
    
    # Check cache
    if memo[index] is not None:
        return memo[index]
    
    mini = float('inf')
    
    for k in range(index, n):
        if isPalindrome(s, index, k):
            # Count this partition
            cuts = 1 + solve_memo(n, s, k + 1, memo)
            mini = min(mini, cuts)
    
    memo[index] = mini
    return memo[index]
```

**Complexity:**
- **Time:** O(n² × n) = O(n³) - O(n) states × O(n) loop × O(n) palindrome check
- **Space:** O(n)

### 💡 Approach 3: Tabulation

**Code:**
```python
def tabulation(n, s):
    # dp[index] = min partitions needed from index to end
    dp = [None]*(n + 1)
    dp[n] = 0  # Base case
    
    # Fill from right to left
    for index in range(n - 1, -1, -1):
        mini = float('inf')
        
        # Try each partition ending at k
        for k in range(index, n):
            if isPalindrome(s, index, k):
                length = 1 + dp[k + 1]
                mini = min(mini, length)
        
        dp[index] = mini
    
    return dp[0]

# Usage: subtract 1 to get cuts (not partitions)
print(tabulation(n, s) - 1)
```

**Complexity:**
- **Time:** O(n³)
- **Space:** O(n)

---

## 5. Partition Array for Maximum Sum

### 📝 Problem Description
Partition array into subarrays of length ≤ k. Replace each element with subarray's max. Maximize total sum.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [1, 15, 7, 9, 2, 5, 10], k = 3
Output: 84
Explanation: [1,15,7]→15×3, [9]→9×1, [2,5,10]→10×3 = 45+9+30 = 84
```

**Example 2:**
```
Input: arr = [2, 2, 2, 2], k = 2
Output: 8
```

### 💡 Approach 1: Recursion

**Intuition:**
- From index, try creating subarrays of length 1 to k
- For each subarray, replace all with maximum value
- Sum = max × length
- Maximize total sum

**Code:**
```python
def solve(n, arr, subLen, index):
    # Base case: processed all elements
    if index == n:
        return 0
    
    maxi = 0  # Track max element in current subarray
    maxSum = 0
    
    # Try subarrays of different lengths (1 to subLen)
    for k in range(index, min(index + subLen, n)):
        # Update max in current subarray
        maxi = max(maxi, arr[k])
        # Length of current subarray
        length = k - index + 1
        # Sum: max × length + best sum from remaining
        totalSum = maxi * length + solve(n, arr, subLen, k + 1)
        maxSum = max(maxSum, totalSum)
    
    return maxSum
```

**Complexity:**
- **Time:** O(k^n) - k choices at each position
- **Space:** O(n)

### 💡 Approach 2: Memoization

**Code:**
```python
def solve_memo(n, arr, subLen, index, memo):
    # Base case
    if index == n:
        return 0
    
    # Check cache
    if memo[index] is not None:
        return memo[index]
    
    maxi = 0
    maxSum = 0
    
    # Try different subarray lengths
    for k in range(index, min(index + subLen, n)):
        maxi = max(maxi, arr[k])
        length = k - index + 1
        # Recursively solve for rest
        totalSum = maxi * length + solve_memo(n, arr, subLen, k + 1, memo)
        maxSum = max(maxSum, totalSum)
    
    memo[index] = maxSum
    return memo[index]
```

**Complexity:**
- **Time:** O(n × k)
- **Space:** O(n)

### 💡 Approach 3: Tabulation

**Code:**
```python
def tabulation(n, arr, subLen):
    # dp[index] = max sum from index to end
    dp = [None]*(n + 1)
    dp[n] = 0  # Base case
    
    # Fill from right to left
    for index in range(n - 1, -1, -1):
        maxi = 0
        maxSum = 0
        
        # Try subarrays ending at k
        for k in range(index, n):
            length = k - index + 1
            # Skip if length exceeds limit
            if length > subLen:
                continue
            
            # Update max in subarray
            maxi = max(maxi, arr[k])
            # Calculate sum
            totalSum = maxi * length + dp[k + 1]
            maxSum = max(maxSum, totalSum)
        
        dp[index] = maxSum
    
    return dp[0]
```

**Complexity:**
- **Time:** O(n × k)
- **Space:** O(n)

---

## 📊 Problem Comparison Table

| Problem | Pattern | Partition Type | Goal | Time | Space |
|---------|---------|----------------|------|------|-------|
| **1. MCM** | Range DP | Try all splits | Minimize ops | O(n³) | O(n²) |
| **2. Cut Stick** | Range DP | Try all cuts | Minimize cost | O(c³) | O(c²) |
| **3. Burst Balloons** | Range DP | Last to burst | Maximize points | O(n³) | O(n²) |
| **4. Palindrome Part** | Linear DP | Palindromes | Minimize cuts | O(n³) | O(n) |
| **5. Max Sum** | Linear DP | Fixed length | Maximize sum | O(n×k) | O(n) |

---

## 🎯 Quick Selection Guide

### By DP Type
- **Range DP (start, end)**: MCM, Cut Stick, Burst Balloons
- **Linear DP (index)**: Palindrome Partitioning, Max Sum

### By Goal
- **Minimize**: MCM (operations), Cut Stick (cost), Palindrome (cuts)
- **Maximize**: Burst Balloons (points), Max Sum (sum)

### Key Patterns
1. **Range DP**: Try partition point k between start and end
2. **Linear DP**: Try different partition lengths from index

---

## 🔑 Common Patterns

### 1. **Range DP Template (MCM Pattern)**
```python
def solve(arr, start, end):
    if start + 1 == end:  # Base: no elements between
        return 0
    
    result = float('inf')  # or 0 for max
    
    for k in range(start + 1, end):
        left = solve(arr, start, k)
        right = solve(arr, k, end)
        cost = calculate_cost(start, k, end)
        result = optimize(result, left + right + cost)
    
    return result
```

### 2. **Linear DP Template (Partition Pattern)**
```python
def solve(arr, index):
    if index == n:  # Base: reached end
        return 0
    
    result = float('inf')  # or 0 for max
    
    for k in range(index, min(index + maxLen, n)):
        # Process subarray [index...k]
        cost = calculate_cost(index, k)
        result = optimize(result, cost + solve(arr, k + 1))
    
    return result
```

### 3. **Tabulation by Length**
```python
# For Range DP
for length in range(2, n):  # Increasing lengths
    for start in range(n - length):
        end = start + length
        # Fill dp[start][end]
```

---

## 💭 Key Insights

### Problem-Specific Tricks

1. **MCM**: 
   - Array represents matrix dimensions
   - arr[i-1] × arr[i] gives dimensions of matrix i

2. **Cut Stick**: 
   - Sort cuts first
   - Add boundaries: [0] + cuts + [n]
   - Cost = length of current stick

3. **Burst Balloons**:
   - Think backwards: which to burst LAST
   - Add boundary 1s: [1] + nums + [1]
   - Last balloon's neighbors are fixed (start, end)

4. **Palindrome Partitioning**:
   - Result counts partitions, subtract 1 for cuts
   - Can optimize with precomputed palindrome table

5. **Max Sum**:
   - Only depends on current index (1D DP)
   - Try subarrays of length 1 to k

### Common Mistakes
- ❌ Forgetting base case: `start + 1 == end` (no elements between)
- ❌ Not adding boundaries in Burst Balloons and Cut Stick
- ❌ Confusing "last to burst" vs "first to burst" in Burst Balloons
- ❌ Returning partitions instead of cuts in Palindrome Partitioning
- ❌ Not limiting subarray length in Max Sum problem

### Optimization Tips
- ✅ Use memoization for overlapping subproblems
- ✅ Build tabulation by increasing lengths
- ✅ Precompute helper functions (like isPalindrome)
- ✅ For Range DP, space optimization is difficult (need 2D table)
- ✅ For Linear DP, can use 1D array
