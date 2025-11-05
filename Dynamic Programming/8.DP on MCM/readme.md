# Matrix Chain Multiplication

## Matrix Multiplication Basics

### How Matrix Multiplication Works

To multiply two matrices A and B:
- Matrix A has dimensions: **p × q**
- Matrix B has dimensions: **q × r**
- Result matrix C has dimensions: **p × r**
- Number of scalar multiplications needed: **p × q × r**

**Simple Example:**

```
Matrix A (2×3)          Matrix B (3×2)          Result C (2×2)
┌       ┐              ┌       ┐              ┌           ┐
│ 1 2 3 │              │ 7  8  │              │ 58   64   │
│ 4 5 6 │      ×       │ 9  10 │      =       │ 139  154  │
└       ┘              │ 11 12 │              └           ┘
                       └       ┘
```

**Cost calculation:**  
Dimensions: (2×3) × (3×2) → Number of multiplications = **2 × 3 × 2 = 12**

### Why Order Matters

When multiplying **multiple matrices**, different parenthesization leads to different costs:

**Example:** Three matrices A(10×20), B(20×30), C(30×40)

**Option 1:** (A × B) × C
- Step 1: A × B = 10 × 20 × 30 = **6,000** operations → Result: 10×30
- Step 2: (AB) × C = 10 × 30 × 40 = **12,000** operations
- **Total: 18,000 operations**

**Option 2:** A × (B × C)
- Step 1: B × C = 20 × 30 × 40 = **24,000** operations → Result: 20×40
- Step 2: A × (BC) = 10 × 20 × 40 = **8,000** operations
- **Total: 32,000 operations**

Same result, but **Option 1 is 1.77x faster!** This is what we optimize.

---

## Problem Statement

Given a sequence of matrices, find the most efficient way to multiply these matrices together. The problem is not actually to perform the multiplications, but merely to decide in which order to perform the multiplications to minimize the number of scalar multiplications.

For an array `arr[]` of size `n` representing dimensions of matrices, where `arr[i-1] x arr[i]` represents the dimensions of the i-th matrix, find the minimum number of multiplications needed.

**Example:**
- Input: `arr = [10, 20, 30, 40, 50]`
- This represents 4 matrices: M1(10×20), M2(20×30), M3(30×40), M4(40×50)
- Output: `38000`

---

## Intuition

### The Core Idea

The problem asks: "Where should we place the **last** (outermost) pair of parentheses?"

When we have a chain of matrices to multiply, we need to decide:
- Which two groups should be multiplied **last**?
- This splits our chain into two parts at some partition point `k`

**Example:** For matrices M1, M2, M3, M4

```
We can split at:
├─ k=1: (M1) × (M2 × M3 × M4)
├─ k=2: (M1 × M2) × (M3 × M4)
└─ k=3: (M1 × M2 × M3) × (M4)
```

### Why Try All Partition Points?

We don't know which split gives minimum cost, so we:
1. **Try every possible partition point** `k` between start and end
2. For each `k`:
   - Recursively solve **left part** (start to k)
   - Recursively solve **right part** (k to end)
   - Add cost of multiplying the two results: `arr[start] × arr[k] × arr[end]`
3. **Take the minimum** among all options

### Visual Example

For `arr = [10, 20, 30, 40]` representing M1(10×20), M2(20×30), M3(30×40):

```
Try splitting at k=1:
  Left: M1 (no multiplication needed, cost = 0)
  Right: M2 × M3 (cost = 20×30×40 = 24000)
  Final: Multiply results (cost = 10×20×40 = 8000)
  Total = 0 + 24000 + 8000 = 32000

Try splitting at k=2:
  Left: M1 × M2 (cost = 10×20×30 = 6000)
  Right: M3 (no multiplication needed, cost = 0)
  Final: Multiply results (cost = 10×30×40 = 12000)
  Total = 6000 + 0 + 12000 = 18000 ✓ (minimum!)
```

### Why Dynamic Programming?

Notice that when we split at different `k` values, we end up solving the **same subproblems** multiple times. For example, "cost of multiplying M2×M3" might be needed in multiple splits.

**Solution:** Store results of subproblems (memoization/tabulation) to avoid recomputation!

---

## Recursion Tree Diagram

```
                          solve(0,4)
                              |
        +---------------------+---------------------+
        |                     |                     |
     k=1                   k=2                   k=3
        |                     |                     |
   solve(0,1)            solve(0,2)            solve(0,3)
   solve(1,4)            solve(2,4)            solve(3,4)
        |                     |                     |
     +--+--+              +---+---+            +---+---+
     |     |              |       |            |       |
  s(0,1) s(1,2)       s(0,1)  s(1,2)      s(0,1)  s(1,3)
  s(1,2) s(2,4)       s(1,2)  s(2,3)      s(1,2)  s(2,3)
  ...    ...          s(2,3)  s(3,4)      s(2,3)  s(3,4)
                      ...     ...          ...     ...

Base Case: solve(i, i+1) = 0
```

---

## Recurrence Relation

### Formation of Recurrence Relation

For multiplying matrices from index `start` to `end`:

1. **Base Case:** If `start + 1 == end`, we have only one matrix, so no multiplication is needed → return `0`

2. **Recursive Case:** Try every possible partition point `k` between `start` and `end`:
   - Divide at position `k`: matrices [start...k] and [k...end]
   - Cost = Cost(left part) + Cost(right part) + Cost(multiplying results)
   
3. **Cost of multiplying two result matrices:**
   - Left result has dimensions: `arr[start] x arr[k]`
   - Right result has dimensions: `arr[k] x arr[end]`
   - Multiplication cost: `arr[start] * arr[k] * arr[end]`

### Recurrence Relation:
```
dp[start][end] = min(dp[start][k] + dp[k][end] + arr[start]*arr[k]*arr[end])
                 for all k in range(start+1, end)

Base Case: dp[i][i+1] = 0
```

---

## Solution Approaches

### 1. Recursion (Brute Force)

```python
def solve(arr, start, end):
    # Base case: single matrix needs no multiplication
    if start + 1 == end:
        return 0
    
    mini = float('inf')
    
    # Try every partition point k
    for k in range(start + 1, end):
        # Cost of left subproblem
        leftPart = solve(arr, start, k)
        # Cost of right subproblem
        rightPart = solve(arr, k, end)
        # Cost of multiplying the two resulting matrices
        operations = arr[start] * arr[k] * arr[end]
        # Total cost for this partition
        total = leftPart + rightPart + operations
        # Keep track of minimum
        mini = min(mini, total)
    
    return mini
```

**Time Complexity:** O(2^n) - Exponential due to overlapping subproblems  
**Space Complexity:** O(n) - Recursion stack depth

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion:**
1. Add a `memo` table to store computed results
2. Before computing, check if result already exists in `memo[start][end]`
3. After computing, store result in `memo[start][end]`
4. Initialize memo table with `None` values

```python
def solve_memo(arr, start, end, memo):
    # Base case: single matrix needs no multiplication
    if start + 1 == end:
        return 0
    
    # Return cached result if already computed
    if memo[start][end] != None:
        return memo[start][end]
    
    mini = float('inf')
    
    # Try every partition point k
    for k in range(start + 1, end):
        # Cost of left subproblem (memoized)
        leftPart = solve_memo(arr, start, k, memo)
        # Cost of right subproblem (memoized)
        rightPart = solve_memo(arr, k, end, memo)
        # Cost of multiplying the two resulting matrices
        operations = arr[start] * arr[k] * arr[end]
        # Total cost for this partition
        total = leftPart + rightPart + operations
        # Keep track of minimum
        mini = min(mini, total)
    
    # Store result in memo table
    memo[start][end] = mini
    return memo[start][end]
```

**Time Complexity:** O(n³) - Each subproblem computed once, n² subproblems, each with n iterations  
**Space Complexity:** O(n²) - Memo table + O(n) recursion stack

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization:**
1. Replace recursion with iteration using nested loops
2. Create `dp` table initialized with base cases
3. Build solutions from smaller subproblems to larger ones
4. Iterate by increasing chain length (from 2 to n-1)
5. For each length, iterate through all possible start positions
6. Replace recursive calls with table lookups `dp[start][k]` and `dp[k][end]`

```python
def tabulation(n, arr):
    # Initialize DP table
    dp = [[0] * n for _ in range(n)]
    
    # Base case already handled: dp[i][i+1] = 0
    
    # Iterate by chain length (2 to n-1)
    for length in range(2, n):
        # For each starting position
        for start in range(n - length):
            end = start + length
            mini = float('inf')
            
            # Try every partition point k
            for k in range(start + 1, end):
                # Cost from DP table (already computed)
                leftPart = dp[start][k]
                rightPart = dp[k][end]
                # Cost of multiplying the two resulting matrices
                operations = arr[start] * arr[k] * arr[end]
                # Total cost for this partition
                total = leftPart + rightPart + operations
                # Keep track of minimum
                mini = min(mini, total)
            
            dp[start][end] = mini
    
    # Return the result for entire chain
    return dp[0][n - 1]
```

**Time Complexity:** O(n³) - Three nested loops  
**Space Complexity:** O(n²) - DP table only, no recursion stack

---

## Tabulation DP Table Example

**Input:** `arr = [10, 20, 30, 40, 50]` (4 matrices)

**DP Table Construction:**

| start\end | 0 | 1 | 2 | 3 | 4 |
|-----------|-------|-------|--------|--------|--------|
| **0** | 0 | 0 | 6000 | 18000 | 38000 |
| **1** | - | 0 | 0 | 24000 | 48000 |
| **2** | - | - | 0 | 0 | 60000 |
| **3** | - | - | - | 0 | 0 |
| **4** | - | - | - | - | 0 |

**Step-by-step filling:**

**Length = 2:**
- `dp[0][2]`: Multiply M1×M2 = 10×20×30 = **6000**
- `dp[1][3]`: Multiply M2×M3 = 20×30×40 = **24000**
- `dp[2][4]`: Multiply M3×M4 = 30×40×50 = **60000**

**Length = 3:**
- `dp[0][3]`: Min of:
  - k=1: dp[0][1] + dp[1][3] + 10×20×40 = 0 + 24000 + 8000 = 32000
  - k=2: dp[0][2] + dp[2][3] + 10×30×40 = 6000 + 0 + 12000 = **18000** ✓
- `dp[1][4]`: Min of:
  - k=2: dp[1][2] + dp[2][4] + 20×30×50 = 0 + 60000 + 30000 = 90000
  - k=3: dp[1][3] + dp[3][4] + 20×40×50 = 24000 + 0 + 40000 = **48000** ✓

**Length = 4:**
- `dp[0][4]`: Min of:
  - k=1: dp[0][1] + dp[1][4] + 10×20×50 = 0 + 48000 + 10000 = 58000
  - k=2: dp[0][2] + dp[2][4] + 10×30×50 = 6000 + 60000 + 15000 = 81000
  - k=3: dp[0][3] + dp[3][4] + 10×40×50 = 18000 + 0 + 20000 = **38000** ✓

**Final Answer:** `dp[0][4] = 38000`

---

## Space Optimization

**Note:** For Matrix Chain Multiplication, space optimization below O(n²) is not straightforward because we need to access multiple non-adjacent cells in the DP table (dp[start][k] and dp[k][end] for various k values). Unlike problems like Fibonacci or 0/1 Knapsack where we only need the previous row, this problem requires random access to previously computed values.

Therefore, **O(n²) tabulation is the optimal space complexity** for this problem.

---

## Complete Implementation

```python
# Recursion
def solve(arr, start, end):
    if start + 1 == end:
        return 0
    mini = float('inf')
    for k in range(start + 1, end):
        leftPart = solve(arr, start, k)
        rightPart = solve(arr, k, end)
        operations = arr[start] * arr[k] * arr[end]
        total = leftPart + rightPart + operations
        mini = min(mini, total)
    return mini

# Memoization
def solve_memo(arr, start, end, memo):
    if start + 1 == end:
        return 0
    if memo[start][end] != None:
        return memo[start][end]
    mini = float('inf')
    for k in range(start + 1, end):
        leftPart = solve_memo(arr, start, k, memo)
        rightPart = solve_memo(arr, k, end, memo)
        operations = arr[start] * arr[k] * arr[end]
        total = leftPart + rightPart + operations
        mini = min(mini, total)
    memo[start][end] = mini
    return memo[start][end]

# Tabulation
def tabulation(n, arr):
    dp = [[0] * n for _ in range(n)]
    for length in range(2, n):
        for start in range(n - length):
            end = start + length
            mini = float('inf')
            for k in range(start + 1, end):
                leftPart = dp[start][k]
                rightPart = dp[k][end]
                operations = arr[start] * arr[k] * arr[end]
                total = leftPart + rightPart + operations
                mini = min(mini, total)
            dp[start][end] = mini
    return dp[0][n - 1]

# Test
arr = [10, 20, 30, 40, 50]
n = len(arr)
print(solve(arr, 0, n - 1))  # Output: 38000
memo = [[None] * n for _ in range(n)]
print(solve_memo(arr, 0, n - 1, memo))  # Output: 38000
print(tabulation(n, arr))  # Output: 38000
```

---

## Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n³) | O(n²) + O(n) |
| Tabulation | O(n³) | O(n²) |
| Space Optimized | O(n³) | O(n²) |

- **n³ Time Complexity** comes from: n² subproblems × n iterations per subproblem
- **n² Space Complexity** is optimal for this problem due to the need for random access to DP table values

---
