
# 1.Fibonacci Number

## Problem Definition

### What are Fibonacci Numbers?

The Fibonacci numbers are a sequence of integers: **0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, ...**

Each number is the sum of the two preceding ones, starting from 0 and 1.

**Mathematically:** F(n) = F(n-1) + F(n-2), where F(0) = 0 and F(1) = 1

**Task:** Given n, calculate F(n)

---

## Understanding the Problem

### Step 1: Define The Problem

The Fibonacci sequence is an integer sequence starting with **0, 1**. Each upcoming number is obtained by adding the previous two numbers.

**Example:**
- Start: 0, 1
- Next: 0 + 1 = **1**
- Next: 1 + 1 = **2**
- Next: 1 + 2 = **3**
- Next: 2 + 3 = **5**

**Series:** 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55...

---

### Step 2: Represent the Problem Programmatically

- **Input:** Integer `n` (0-based index)
- **Output:** The nth Fibonacci number
- **Note:** Since it's 0-based indexing, we need to find (n+1) numbers including zero

**Examples:**
```
Input: n = 0  →  Output: 0
Input: n = 1  →  Output: 1
Input: n = 2  →  Output: 1
Input: n = 3  →  Output: 2
Input: n = 4  →  Output: 3
Input: n = 5  →  Output: 5
Input: n = 6  →  Output: 8
```

---

### Step 3: Finding Base Cases

We know the first few Fibonacci numbers:

```
f(0) = 0
f(1) = 1
f(2) = 1
```

These are our **base cases** that stop the recursion.

---

### Step 4: Finding The Recurrence Relation

Let's represent the problem as a mathematical function **f(n)**, where n represents the nth Fibonacci number.

**Known values:**
- f(0) = 0
- f(1) = 1
- f(2) = 1

**Deriving the pattern:**
- f(3) = f(2) + f(1) = 1 + 1 = **2**
- f(4) = f(3) + f(2) = 2 + 1 = **3**
- f(5) = f(4) + f(3) = 3 + 2 = **5**

**Observation:** Each number is the sum of the previous two numbers.

### ✅ Recurrence Relation:
```
f(n) = f(n-1) + f(n-2)
```

---

## Recursion Tree Visualization

### Medium Example: Calculate F(5)

```
                                    f(5)
                                   /    \
                                  /      \
                               f(4)      f(3)
                              /   \      /   \
                             /     \    /     \
                          f(3)    f(2) f(2)  f(1)
                         /   \     |    |      |
                        /     \    |    |      |
                     f(2)    f(1)  1    1      1
                    /   \     |
                   /     \    |
                f(1)    f(0)  1
                 |       |
                 |       |
                 1       0
```

### Analysis of Recursion Tree:

**Tree Height:** 5 (depth of recursion)

**Total Function Calls:** 15 calls for F(5)

**Redundant Computations:**
- f(3) is calculated **2 times**
- f(2) is calculated **3 times**
- f(1) is calculated **5 times**
- f(0) is calculated **3 times**

**This redundancy leads to exponential time complexity!**

### Call Count Breakdown:
```
f(5) → 1 call
f(4) → 1 call
f(3) → 2 calls
f(2) → 3 calls
f(1) → 5 calls
f(0) → 3 calls
─────────────
Total: 15 calls
```

### Key Observation:
The same subproblems are solved multiple times, which makes the naive recursive approach inefficient. This is where **Dynamic Programming** comes to the rescue!

---

## All Solutions with Complexity Analysis

### Solution 1: Recursive Approach (Naive)

#### Code:
```python
def recursive(n):
    if n <= 1:
        return n
    if n == 2:
        return 1
    
    prev = recursive(n - 1)
    prev2 = recursive(n - 2)
    answer = prev + prev2
    return answer

# Driver Code
n = int(input())
print(recursive(n))
```

#### Complexity:
- **Time Complexity:** O(2^n) - Exponential (very slow for large n)
- **Space Complexity:** O(n) - Recursion stack depth

#### Pros & Cons:
✅ Simple and intuitive
❌ Extremely slow for large inputs
❌ Many redundant calculations

---

### Solution 2: Memoization (Top-Down DP)

#### Code:
```python
def memoization(n, memo):
    # Check if already computed
    if memo[n] != -1:
        return memo[n]
    
    # Base cases
    if n <= 1:
        return n
    
    # Recursive calls
    a = memoization(n - 1, memo)
    b = memoization(n - 2, memo)
    answer = a + b
    
    # Store in memo
    memo[n] = answer
    return memo[n]

# Driver Code
n = int(input())
memo = [-1] * (n + 1)  # Initialize array with -1
print(memoization(n, memo))
```

#### Complexity:
- **Time Complexity:** O(n) - Each subproblem computed once
- **Space Complexity:** O(n) - Memo array + recursion stack

#### Pros & Cons:
✅ Much faster than naive recursion
✅ Avoids redundant calculations
❌ Uses extra space for memoization
❌ Still uses recursion stack

---

### Solution 3: Tabulation (Bottom-Up DP)

#### Code:
```python
def tabulation(n):
    # Handle edge cases
    if n <= 1:
        return n
    if n == 2:
        return 1
    
    # Create DP array
    dp = [0] * (n + 1)
    
    # Fill base cases
    dp[0] = 0
    dp[1] = 1
    dp[2] = 1
    
    # Fill the DP table
    for i in range(3, n + 1):
        prev = dp[i - 1]
        prev2 = dp[i - 2]
        answer = prev + prev2
        dp[i] = answer
    
    return dp[n]

# Driver Code
n = int(input())
print(tabulation(n))
```

#### Complexity:
- **Time Complexity:** O(n) - Single loop
- **Space Complexity:** O(n) - DP array

#### Pros & Cons:
✅ Fast and efficient
✅ No recursion stack overhead
✅ Iterative approach
❌ Uses O(n) space

---

### Solution 4: Space Optimized (Best Solution)

#### Code:
```python
def optimization(n):
    # Handle edge cases
    if n <= 1:
        return n
    if n == 2:
        return 1
    
    # Only store last two values
    prev = 1   # f(2)
    prev2 = 1  # f(1)
    
    for i in range(3, n + 1):
        answer = prev + prev2
        prev2 = prev
        prev = answer
    
    return prev

# Driver Code
n = int(input())
print(optimization(n))
```

#### Complexity:
- **Time Complexity:** O(n) - Single loop
- **Space Complexity:** O(1) - Only two variables

#### Pros & Cons:
✅ Optimal time complexity
✅ Minimal space usage
✅ No recursion
✅ **Best solution overall**

---

# Climbing Stairs

## Problem Statement

Given an integer **n**, there is a staircase with **n steps**, starting from the **0th step**.

Determine the **number of unique ways** to reach the **nth step**, given that each move can be either **1 or 2 steps** at a time.

### Examples:

**Example 1:**
```
Input: n = 2
Output: 2
Explanation:
There are 2 unique ways to climb to the 2nd step:
1) 1 step + 1 step
2) 2 steps
```

**Example 2:**
```
Input: n = 3
Output: 3
Explanation:
There are 3 unique ways to climb to the 3rd step:
1) 1 step + 1 step + 1 step
2) 2 steps + 1 step
3) 1 step + 2 steps
```

---

## Step 1: Define The Problem

- We have **n steps** in a staircase
- We start from the **0th step**
- We need to reach the **nth step**
- At each move, we can jump either **1 step** or **2 steps**
- We need to find the **total number of different ways** to reach the nth step

---

## Step 2: Represent the Problem Programmatically

- **Starting point:** 0th step
- **Target:** nth step
- **Data structure:** We might need a 1-D array of size **(n+1)** to include the 0th step
- **Index representation:** 0 to n represents the step number we are at currently
- **Function notation:** f(n) represents the number of ways to reach the nth step

---

## Step 3: Finding Base Cases

Let's analyze small cases:

**Case 1: Reach step 0**
- We are already at step 0, so there is only **1 way** (stay there)
- **f(0) = 1**

**Case 2: Reach step 1**
- From step 0, we can take 1 step to reach step 1
- There is only **1 way**
- **f(1) = 1**

**Case 3: Reach step 2**
- Way 1: Take 1 step + 1 step (0→1→2)
- Way 2: Take 2 steps (0→2)
- There are **2 ways**
- **f(2) = 2**

**Base cases summary:**
```
f(0) = 1
f(1) = 1
f(2) = 2
```

---

## Step 4: Finding The Recurrence Relation

Let's think about reaching the **nth step**.

To reach step **n**, we can come from:
1. **Step (n-1)** by taking **1 step**
2. **Step (n-2)** by taking **2 steps**

Since we can only take 1 or 2 steps at a time, these are the **only two possibilities**.

**Key insight:** 
- Total ways to reach step n = Ways to reach step (n-1) + Ways to reach step (n-2)

**Example for step 3:**
- We know f(1) = 1 and f(2) = 2
- Therefore, f(3) = f(2) + f(1) = 2 + 1 = **3**

### ✅ Recurrence Relation:
```
f(n) = f(n-1) + f(n-2)
```

---

## Recursion Tree Visualization

### Medium Example: Calculate ways to reach Step 5

```
                                    f(5)
                                   /    \
                                  /      \
                               f(4)      f(3)
                              /   \      /   \
                             /     \    /     \
                          f(3)    f(2) f(2)  f(1)
                         /   \     |    |      |
                        /     \    |    |      |
                     f(2)    f(1)  2    2      1
                    /   \     |
                   /     \    |
                f(1)    f(0)  1
                 |       |
                 |       |
                 1       1
```

### Analysis of Recursion Tree:

**Tree Height:** 5 (depth of recursion)

**Total Function Calls:** 15 calls for f(5)

**Redundant Computations:**
- f(4) is calculated **1 time**
- f(3) is calculated **2 times**
- f(2) is calculated **3 times**
- f(1) is calculated **5 times**
- f(0) is calculated **3 times**

### Call Count Breakdown:
```
f(5) → 1 call
f(4) → 1 call
f(3) → 2 calls
f(2) → 3 calls
f(1) → 5 calls
f(0) → 3 calls
─────────────
Total: 15 calls
```

### Result for f(5):
```
f(0) = 1
f(1) = 1
f(2) = f(1) + f(0) = 1 + 1 = 2
f(3) = f(2) + f(1) = 2 + 1 = 3
f(4) = f(3) + f(2) = 3 + 2 = 5
f(5) = f(4) + f(3) = 5 + 3 = 8

Answer: 8 ways to reach step 5
```

### Key Observation:
Multiple subproblems are being solved repeatedly, leading to **exponential time complexity**. This inefficiency can be eliminated using **Dynamic Programming**.

---

## Solution 1: Recursive Approach (Naive)

This is the direct implementation of our recurrence relation.

### Code:
```python
def solve(n):
    # Base cases
    if n <= 1:
        return 1
    
    # Recursive calls
    oneStep = solve(n - 1)
    twoSteps = solve(n - 2)
    
    return oneStep + twoSteps

# Driver Code
n = int(input())
print(solve(n))
```

### Complexity:
- **Time Complexity:** O(2^n) - Exponential
- **Space Complexity:** O(n) - Recursion stack depth

### Pros & Cons:
✅ Simple and intuitive
✅ Directly follows the recurrence relation
❌ Extremely slow for large inputs
❌ Many redundant calculations (as seen in recursion tree)

### Why It's Slow:
Looking at the recursion tree, we calculate f(3) twice, f(2) three times, f(1) five times! This exponential growth makes it impractical for large n.

---

## Solution 2: Memoization (Top-Down DP)

### The Idea:
Instead of recalculating the same subproblem multiple times, we **store the result the first time we calculate it** and **reuse it** whenever needed.

### How We Convert from Recursion:
1. **Keep the recursive structure** - Don't change the logic
2. **Add a storage array (memo)** - Initialize with -1 to indicate "not computed yet"
3. **Before computing** - Check if we already have the answer in memo
4. **After computing** - Store the result in memo before returning

### Code:
```python
def solve(n, memo):
    # CHECK: Have we already computed this?
    if memo[n] != -1:
        return memo[n]
    
    # Base cases (same as recursive)
    if n <= 1:
        return 1
    
    # Recursive calls (same as recursive)
    oneStep = solve(n - 1, memo)
    twoSteps = solve(n - 2, memo)
    
    # STORE: Save result before returning
    memo[n] = oneStep + twoSteps
    return memo[n]

# Driver Code
n = int(input())
memo = [-1] * (n + 1)  # Initialize array with -1
print(solve(n, memo))
```

### What Changed:
```
Recursive:                 Memoization:
─────────────────         ──────────────────────────
def solve(n):             def solve(n, memo):
                              if memo[n] != -1:      ← Check cache
                                  return memo[n]
    if n <= 1:                if n <= 1:
        return 1                  return 1
    oneStep = solve(n-1)      oneStep = solve(n-1, memo)
    twoSteps = solve(n-2)     twoSteps = solve(n-2, memo)
                              memo[n] = oneStep + twoSteps  ← Store result
    return oneStep+twoSteps   return memo[n]
```

### How It Works (Example for n=5):
```
Call f(5):
  → Need f(4) - NOT in memo, calculate it
  → Need f(3) - NOT in memo, calculate it
  → Need f(2) - NOT in memo, calculate it
  → Need f(1) - base case, return 1
  → Need f(0) - base case, return 1
  → f(2) = 2, STORE in memo[2]
  → f(3) = 3, STORE in memo[3]
  → Need f(2) again - FOUND in memo[2], return 2 immediately!
  → f(4) = 5, STORE in memo[4]
  → Need f(3) again - FOUND in memo[3], return 3 immediately!
  → f(5) = 8, STORE in memo[5]
```

**Key Benefit:** Each subproblem is computed only once!

### Complexity:
- **Time Complexity:** O(n) - Each subproblem computed once
- **Space Complexity:** O(n) - Memo array + recursion stack

### Pros & Cons:
✅ Much faster than naive recursion
✅ Avoids redundant calculations
✅ Easy to convert from recursive solution
✅ Each f(i) computed exactly once
❌ Still uses recursion stack
❌ Uses O(n) extra space for memo array

---

## Solution 3: Tabulation (Bottom-Up DP)

### The Idea:
Instead of computing top-down (from n to base cases), we compute **bottom-up** (from base cases to n). This eliminates recursion entirely!

### How We Convert from Memoization:
1. **Remove recursion** - Use a loop instead
2. **Create DP array** - Same size as memo array
3. **Fill base cases first** - Start with known values
4. **Build up iteratively** - Use the recurrence relation in a loop
5. **Each step uses previously computed values** - Just like memoization, but in order

### Code:
```python
def tabulation(n):
    # Handle edge case
    if n <= 1:
        return 1
    
    # Create DP array (like memo, but we fill it ourselves)
    dp = [-1] * (n + 1)
    
    # Fill base cases FIRST
    dp[0] = 1
    dp[1] = 1
    
    # Build up from small to large
    for i in range(2, n + 1):
        # Use the SAME recurrence relation
        oneStep = dp[i - 1]
        twoSteps = dp[i - 2]
        dp[i] = oneStep + twoSteps
    
    return dp[n]

# Driver Code
n = int(input())
print(tabulation(n))
```

### What Changed:
```
Memoization (Top-Down):          Tabulation (Bottom-Up):
───────────────────────          ───────────────────────
def solve(n, memo):              def tabulation(n):
    if memo[n] != -1:                if n <= 1:
        return memo[n]                   return 1
    if n <= 1:                       dp = [-1] * (n + 1)
        return 1                     dp[0] = 1  ← Fill base cases
    oneStep = solve(n-1, memo)       dp[1] = 1
    twoSteps = solve(n-2, memo)      for i in range(2, n+1):  ← Loop instead of recursion
    memo[n] = oneStep + twoSteps         dp[i] = dp[i-1] + dp[i-2]
    return memo[n]                   return dp[n]
```

### How It Works (Example for n=5):
```
Step 1: Create dp array of size 6: [-1, -1, -1, -1, -1, -1]

Step 2: Fill base cases:
        dp = [1, 1, -1, -1, -1, -1]
             ↑  ↑
           f(0) f(1)

Step 3: Loop from i=2 to i=5:
        i=2: dp[2] = dp[1] + dp[0] = 1 + 1 = 2
             dp = [1, 1, 2, -1, -1, -1]
        
        i=3: dp[3] = dp[2] + dp[1] = 2 + 1 = 3
             dp = [1, 1, 2, 3, -1, -1]
        
        i=4: dp[4] = dp[3] + dp[2] = 3 + 2 = 5
             dp = [1, 1, 2, 3, 5, -1]
        
        i=5: dp[5] = dp[4] + dp[3] = 5 + 3 = 8
             dp = [1, 1, 2, 3, 5, 8]

Step 4: Return dp[5] = 8
```

**Key Benefit:** No recursion overhead, straightforward iteration!

### Complexity:
- **Time Complexity:** O(n) - Single loop
- **Space Complexity:** O(n) - DP array only (no recursion stack!)

### Pros & Cons:
✅ Fast and efficient
✅ No recursion stack overhead
✅ Iterative approach - easier to understand flow
✅ More cache-friendly (sequential memory access)
❌ Still uses O(n) space for DP array

---

## Solution 4: Space Optimized

### The Idea:
Looking at the recurrence relation `f(n) = f(n-1) + f(n-2)`, we notice that to calculate f(n), we only need the **last 2 values**. We don't need the entire array!

### How We Convert from Tabulation:
1. **Identify what we actually need** - Only last 2 values (prev and prev2)
2. **Replace array with variables** - Use 2 variables instead of array
3. **Update variables in each iteration** - Shift values: prev2 → prev, current → prev2
4. **Same loop logic** - Just different storage

### Code:
```python
def optimized(n):
    # Handle edge case
    if n <= 1:
        return 1
    
    # Only store last two values
    prev2 = 1  # f(0)
    prev = 1   # f(1)
    
    for i in range(2, n + 1):
        # Calculate current using last two
        current = prev + prev2
        
        # Shift values for next iteration
        prev2 = prev
        prev = current
    
    return prev

# Driver Code
n = int(input())
print(optimized(n))
```

### What Changed:
```
Tabulation:                      Space Optimized:
──────────────────              ─────────────────────
def tabulation(n):              def optimized(n):
    if n <= 1:                      if n <= 1:
        return 1                        return 1
    dp = [-1] * (n + 1)             prev2 = 1  ← Just 2 variables!
    dp[0] = 1                       prev = 1
    dp[1] = 1                       for i in range(2, n+1):
    for i in range(2, n+1):             current = prev + prev2
        dp[i] = dp[i-1] + dp[i-2]       prev2 = prev  ← Shift values
    return dp[n]                        prev = current
                                    return prev
```

### Complexity:
- **Time Complexity:** O(n) - Single loop
- **Space Complexity:** O(1) - Only 2-3 variables!

### Pros & Cons:
✅ Optimal time complexity
✅ Minimal space usage - constant space!
✅ No recursion
✅ No extra arrays
✅ **Best solution overall**

---

## Complete Working Code

```python
# Solution 1: Recursive
def solve(n):
    if n <= 1:
        return 1
    oneStep = solve(n - 1)
    twoSteps = solve(n - 2)
    return oneStep + twoSteps

# Solution 2: Memoization
def solve_memo(n, memo):
    if memo[n] != -1:
        return memo[n]
    if n <= 1:
        return 1
    oneStep = solve_memo(n - 1, memo)
    twoSteps = solve_memo(n - 2, memo)
    memo[n] = oneStep + twoSteps
    return memo[n]

# Solution 3: Tabulation
def tabulation(n):
    if n <= 1:
        return 1
    dp = [-1] * (n + 1)
    dp[0] = 1
    dp[1] = 1
    for i in range(2, n + 1):
        oneStep = dp[i - 1]
        twoSteps = dp[i - 2]
        dp[i] = oneStep + twoSteps
    return dp[n]

# Solution 4: Space Optimized
def optimized(n):
    if n <= 1:
        return 1
    prev2 = 1
    prev = 1
    for i in range(2, n + 1):
        current = prev + prev2
        prev2 = prev
        prev = current
    return prev

# Driver Code
n = int(input())
print("Recursive:", solve(n))
print("Memoization:", solve_memo(n, [-1] * (n + 1)))
print("Tabulation:", tabulation(n))
print("Optimized:", optimized(n))
```

---
