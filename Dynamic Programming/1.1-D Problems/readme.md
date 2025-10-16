
# Fibonacci Number

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


# Climbing Stairs with K Steps

## Problem Statement

Given a number of stairs and a value **k**, starting from the **0th stair** we need to climb to the **Nth stair**. 

At a time we can climb **1 or 2 or 3 ... or k steps**. We need to return the **total number of distinct ways** to reach from 0th to Nth stair.

### Examples:

**Example 1:**
```
Input: n = 2, k = 2
Output: 2
Explanation: There are two ways to climb to the top.
1) 1 step + 1 step
2) 2 steps
```

**Example 2:**
```
Input: n = 3, k = 3
Output: 4
Explanation: There are four ways to climb to the top.
1) 1 step + 1 step + 1 step
2) 1 step + 2 steps
3) 2 steps + 1 step
4) 3 steps
```

---

## Step 1: Define The Problem

- We have **n steps** in a staircase
- We start from the **0th step**
- We need to reach the **nth step**
- At each move, we can jump **1 or 2 or 3 ... or k steps** (up to k steps)
- We need to find the **total number of different ways** to reach the nth step

---

## Step 2: Represent the Problem Programmatically

- **Starting point:** 0th step
- **Target:** nth step
- **Choices at each step:** We can jump 1, 2, 3, ..., or k steps
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
- From step 0, we can take 1 step to reach step 1 (minimum k value is 1)
- There is only **1 way**
- **f(1) = 1**

**Base cases summary:**
```
f(0) = 1
f(1) = 1
```

---

## Step 4: Finding The Recurrence Relation

### Case 1: When k = 2

To reach the **3rd step**, we can jump either **1 or 2 steps** at a time.

To reach step 3, we can come from:
1. **Step 2** by taking **1 step** → f(3-1) = f(2)
2. **Step 1** by taking **2 steps** → f(3-2) = f(1)

**Total ways:** f(3) = f(2) + f(1)

**Recurrence Relation:** f(n) = f(n-1) + f(n-2)

---

### Case 2: When k = 3

To reach the **3rd step**, we can jump **1 or 2 or 3 steps** at a time.

To reach step 3, we can come from:
1. **Step 2** by taking **1 step** → f(3-1) = f(2)
2. **Step 1** by taking **2 steps** → f(3-2) = f(1)
3. **Step 0** by taking **3 steps** → f(3-3) = f(0)

**Calculation:**
- f(0) = 1
- f(1) = 1
- f(2) = f(1) + f(0) = 1 + 1 = 2
- f(3) = f(2) + f(1) + f(0) = 2 + 1 + 1 = **4**

**Recurrence Relation:** f(n) = f(n-1) + f(n-2) + f(n-3)

---

### General Case: For Any K Steps

To reach the **nth step** with **k choices**, we can come from any of the previous k steps:
- From step (n-1) by taking 1 step
- From step (n-2) by taking 2 steps
- From step (n-3) by taking 3 steps
- ...
- From step (n-k) by taking k steps

### ✅ General Recurrence Relation:
```
f(n) = f(n-1) + f(n-2) + f(n-3) + ... + f(n-k)
```

**Important:** We must ensure that (n-i) ≥ 0, because f(-1) is meaningless.

**In code form:**
```python
total = 0
for i in range(1, k+1):
    if (n-i) >= 0:
        total += f(n-i)
```

---

## Recursion Tree Visualization

### Medium Example: n = 4, k = 3

```
                                f(4)
                    /            |           \
                   /             |            \
                f(3)           f(2)          f(1)
              /  |  \         /    \           |
             /   |   \       /      \          |
          f(2) f(1) f(0)  f(1)    f(0)        1
         /  \   |    |     |        |
        /    \  |    |     |        |
     f(1)  f(0) 1    1     1        1
      |      |
      |      |
      1      1
```

### Analysis:

**Total Function Calls:** Multiple redundant calculations

**Redundant Computations:**
- f(3) is calculated **1 time**
- f(2) is calculated **2 times**
- f(1) is calculated **5 times**
- f(0) is calculated **4 times**

### Result Calculation:
```
f(0) = 1
f(1) = 1
f(2) = f(1) + f(0) = 1 + 1 = 2
f(3) = f(2) + f(1) + f(0) = 2 + 1 + 1 = 4
f(4) = f(3) + f(2) + f(1) = 4 + 2 + 1 = 7

Answer: 7 ways to reach step 4 with k=3
```

### Key Observation:
As k increases, the branching factor of the recursion tree increases, leading to even more redundant calculations. Dynamic Programming becomes essential!

---

## Solution 1: Recursive Approach (Naive)

This is the direct implementation of our recurrence relation with a loop to handle k choices.

### Code:
```python
def solve(n, k):
    # Base cases
    if n <= 1:
        return 1
    
    # Try all possible jumps (1 to k steps)
    ways = 0
    for i in range(1, k + 1):
        if (n - i) >= 0:
            ways += solve(n - i, k)
    
    return ways

# Driver Code
n = int(input())
k = int(input())
print(solve(n, k))
```

### How It Works:
For each step n, we try all possible jumps from 1 to k:
- Jump 1 step: Add ways from (n-1)
- Jump 2 steps: Add ways from (n-2)
- ...
- Jump k steps: Add ways from (n-k)

### Complexity:
- **Time Complexity:** O(k^n) - Exponential with branching factor k
- **Space Complexity:** O(n) - Recursion stack depth

### Pros & Cons:
✅ Simple and intuitive
✅ Directly follows the recurrence relation
✅ Easy to understand the logic
❌ Extremely slow for large inputs
❌ Many redundant calculations
❌ Worse than k=2 case due to higher branching factor

---

## Solution 2: Memoization (Top-Down DP)

### The Idea:
Store computed results to avoid recalculating the same subproblem. The key insight is that for a given n, the number of ways is always the same regardless of how we reached it.

### How We Convert from Recursion:
1. **Keep the recursive structure** - Same loop logic
2. **Add memo array** - Initialize with -1
3. **Check before computing** - If memo[n] != -1, return it
4. **Store after computing** - Save result in memo[n]

### Code:
```python
def solve_memo(n, k, memo):
    # CHECK: Have we already computed this?
    if memo[n] != -1:
        return memo[n]
    
    # Base cases (same as recursive)
    if n <= 1:
        return 1
    
    # Try all possible jumps (same loop as recursive)
    ways = 0
    for i in range(1, k + 1):
        if (n - i) >= 0:
            ways += solve_memo(n - i, k, memo)
    
    # STORE: Save result before returning
    memo[n] = ways
    return memo[n]

# Driver Code
n = int(input())
k = int(input())
memo = [-1] * (n + 1)
print(solve_memo(n, k, memo))
```

### What Changed:
```
Recursive:                          Memoization:
─────────────────────────          ──────────────────────────────────
def solve(n, k):                   def solve_memo(n, k, memo):
                                       if memo[n] != -1:        ← Check cache
                                           return memo[n]
    if n <= 1:                         if n <= 1:
        return 1                           return 1
    ways = 0                           ways = 0
    for i in range(1, k+1):            for i in range(1, k+1):
        if (n-i) >= 0:                     if (n-i) >= 0:
            ways += solve(n-i, k)              ways += solve_memo(n-i, k, memo)
                                       memo[n] = ways           ← Store result
    return ways                        return memo[n]
```

### How It Works (Example: n=4, k=3):
```
Call f(4):
  → Try jump 1: Need f(3) - NOT in memo, calculate it
    → f(3) tries jumps, eventually computes = 4
    → STORE memo[3] = 4
  → Try jump 2: Need f(2) - NOT in memo, calculate it
    → f(2) = 2, STORE memo[2] = 2
  → Try jump 3: Need f(1) - base case, return 1
  → f(4) = 4 + 2 + 1 = 7
  → STORE memo[4] = 7

Any future call to f(3), f(2), etc. returns immediately from memo!
```

### Complexity:
- **Time Complexity:** O(n × k) - n subproblems, each tries k jumps
- **Space Complexity:** O(n) - Memo array + recursion stack

### Pros & Cons:
✅ Much faster than naive recursion
✅ Each subproblem computed only once
✅ Easy to convert from recursive solution
✅ Handles any value of k efficiently
❌ Still uses recursion stack
❌ Uses O(n) extra space

---

## Solution 3: Tabulation (Bottom-Up DP)

### The Idea:
Build the solution iteratively from base cases upward, eliminating recursion completely. For each step i, we look back at the previous k steps and sum their ways.

### How We Convert from Memoization:
1. **Remove recursion** - Use nested loops instead
2. **Create DP array** - Same as memo array
3. **Fill base cases first** - dp[0] = 1, dp[1] = 1
4. **Outer loop:** Iterate from 2 to n (building each step)
5. **Inner loop:** For each step i, try all k possible jumps backwards

### Code:
```python
def tabulation(n, k):
    # Handle edge case
    if n <= 1:
        return 1
    
    # Create DP array
    dp = [-1] * (n + 1)
    
    # Fill base cases
    dp[0] = 1
    dp[1] = 1
    
    # Build up from 2 to n
    for i in range(2, n + 1):
        ways = 0
        # Try all possible previous steps (j steps back)
        for j in range(1, k + 1):
            if (i - j) >= 0:
                ways += dp[i - j]
        dp[i] = ways
    
    return dp[n]

# Driver Code
n = int(input())
k = int(input())
print(tabulation(n, k))
```

### What Changed:
```
Memoization:                        Tabulation:
─────────────────────────────      ──────────────────────────────────
def solve_memo(n, k, memo):        def tabulation(n, k):
    if memo[n] != -1:                  if n <= 1:
        return memo[n]                     return 1
    if n <= 1:                         dp = [-1] * (n + 1)
        return 1                       dp[0] = 1  ← Fill base cases
    ways = 0                           dp[1] = 1
    for i in range(1, k+1):            for i in range(2, n+1):  ← Outer loop
        if (n-i) >= 0:                     ways = 0
            ways += solve_memo(...)        for j in range(1, k+1):  ← Inner loop
    memo[n] = ways                             if (i-j) >= 0:
    return memo[n]                                 ways += dp[i-j]
                                           dp[i] = ways
                                       return dp[n]
```

### How It Works (Example: n=4, k=3):
```
Step 1: Initialize dp array
        dp = [-1, -1, -1, -1, -1]

Step 2: Fill base cases
        dp = [1, 1, -1, -1, -1]
             ↑  ↑
           f(0) f(1)

Step 3: Build iteratively

i=2: Try jumps back: 1 step (dp[1]) and 2 steps (dp[0])
     ways = dp[2-1] + dp[2-2] = dp[1] + dp[0] = 1 + 1 = 2
     dp = [1, 1, 2, -1, -1]

i=3: Try jumps back: 1, 2, 3 steps
     ways = dp[3-1] + dp[3-2] + dp[3-3]
          = dp[2] + dp[1] + dp[0]
          = 2 + 1 + 1 = 4
     dp = [1, 1, 2, 4, -1]

i=4: Try jumps back: 1, 2, 3 steps
     ways = dp[4-1] + dp[4-2] + dp[4-3]
          = dp[3] + dp[2] + dp[1]
          = 4 + 2 + 1 = 7
     dp = [1, 1, 2, 4, 7]

Return dp[4] = 7
```

### Visual Flow:
```
Building dp[i]:
              Look back k steps
                    ↓
    [dp[i-3]] [dp[i-2]] [dp[i-1]] [dp[i]=?]
        ↓         ↓         ↓
        └─────────┴─────────┘
               Sum these
```

### Complexity:
- **Time Complexity:** O(n × k) - For each of n steps, we check k previous steps
- **Space Complexity:** O(n) - DP array only

### Pros & Cons:
✅ Fast and efficient
✅ No recursion stack overhead
✅ Clear iterative logic
✅ Easy to debug and trace
❌ Uses O(n) space
❌ Nested loops might seem complex initially

---

## Solution 4: Space Optimization

### The Idea:
Looking at the recurrence relation, to calculate dp[i], we only need the **last k values** (dp[i-1], dp[i-2], ..., dp[i-k]). We don't need the entire array!

### Why It's Not Preferred:
While we can optimize space by keeping only the last k values in a sliding window (like a queue or deque), this approach:
- **Increases code complexity** significantly
- **Doesn't improve time complexity** (still O(n × k))
- **Space savings are marginal** when k is small
- **Makes the code harder to maintain and understand**

### Conceptual Approach:
```python
# Conceptual - Not implemented in detail
# Use a deque or circular buffer of size k
# Maintain sliding window of last k values
# Update and shift values as we progress

from collections import deque

def optimized(n, k):
    if n <= 1:
        return 1
    
    # Keep last k values in a deque
    window = deque([1, 1])  # Start with f(0) and f(1)
    
    for i in range(2, n + 1):
        # Sum all values in current window
        current = sum(window)
        
        # Add current to window
        window.append(current)
        
        # Remove oldest if window exceeds k size
        if len(window) > k:
            window.popleft()
    
    return window[-1]
```

### Why Tabulation is Preferred:
1. **Simpler code** - Easier to understand and maintain
2. **Space is O(n) anyway** - Not a bottleneck for most problems
3. **Clearer logic** - Direct array access is more readable
4. **Better debugging** - Can inspect entire dp array
5. **Standard approach** - More recognizable pattern

### Complexity (if implemented):
- **Time Complexity:** O(n × k) - Same as tabulation
- **Space Complexity:** O(k) - Only keep last k values

---

## Summary of Transformations

### Recursion → Memoization:
- **Add memo array** initialized with -1
- **Check memo[n]** before computing
- **Store result** in memo[n] after computing
- **Same recursive logic** with caching

### Memoization → Tabulation:
- **Remove recursion** - use nested loops
- **Outer loop (i):** Iterate from 2 to n (build each step)
- **Inner loop (j):** Try all k jumps backward
- **Bottom-up building:** Start from base cases

### Tabulation → Space Optimization:
- **Identify dependency:** Only need last k values
- **Use sliding window:** Deque or circular buffer
- **Trade-off:** Increased complexity for marginal space savings
- **Generally not preferred** for this problem

---

## Comparison with k=2 Case

### Similarities:
- Same base cases: f(0) = 1, f(1) = 1
- Same DP transformation pattern
- Both use overlapping subproblems

### Differences:
| Aspect | k=2 (Fixed) | k=Variable |
|--------|-------------|------------|
| Recurrence | f(n) = f(n-1) + f(n-2) | f(n) = Σf(n-i) for i=1 to k |
| Branching Factor | 2 (constant) | k (variable) |
| Time (Recursive) | O(2^n) | O(k^n) |
| Time (DP) | O(n) | O(n × k) |
| Loop Needed | No | Yes (inner loop) |

---

## Complete Working Code

```python
# Solution 1: Recursive
def solve(n, k):
    if n <= 1:
        return 1
    ways = 0
    for i in range(1, k + 1):
        if (n - i) >= 0:
            ways += solve(n - i, k)
    return ways

# Solution 2: Memoization
def solve_memo(n, k, memo):
    if memo[n] != -1:
        return memo[n]
    if n <= 1:
        return 1
    ways = 0
    for i in range(1, k + 1):
        if (n - i) >= 0:
            ways += solve_memo(n - i, k, memo)
    memo[n] = ways
    return memo[n]

# Solution 3: Tabulation
def tabulation(n, k):
    if n <= 1:
        return 1
    dp = [-1] * (n + 1)
    dp[0] = 1
    dp[1] = 1
    for i in range(2, n + 1):
        ways = 0
        for j in range(1, k + 1):
            if (i - j) >= 0:
                ways += dp[i - j]
        dp[i] = ways
    return dp[n]

# Driver Code
n = int(input())
k = int(input())
print("Recursive:", solve(n, k))
print("Memoization:", solve_memo(n, k, [-1] * (n + 1)))
print("Tabulation:", tabulation(n, k))
```

---


# Frog Jump

## Problem Statement

A frog wants to climb a staircase with `n` steps. Given an integer array `heights`, where `heights[i]` contains the height of the `i-th` step.

To jump from the `i-th` step to the `j-th` step, the frog requires `abs(heights[i] - heights[j])` energy, where `abs()` denotes the absolute difference. The frog can jump from any step either **one or two steps**, provided it exists.

**Return the minimum amount of energy required by the frog to go from the 0th step to the (n-1)th step.**

---

## Examples

### Example 1
**Input:** `heights = [2, 1, 3, 5, 4]`  
**Output:** `2`

**Explanation:**
- 0th step → 2nd step = abs(2 - 3) = 1
- 2nd step → 4th step = abs(3 - 4) = 1
- **Total = 1 + 1 = 2**

### Example 2
**Input:** `heights = [7, 5, 1, 2, 6]`  
**Output:** `9`

**Explanation:**
- 0th step → 1st step = abs(7 - 5) = 2
- 1st step → 3rd step = abs(5 - 2) = 3
- 3rd step → 4th step = abs(2 - 6) = 4
- **Total = 2 + 3 + 4 = 9**

---

## Approach

### Step 1: Define the Problem

- Frog is at the **0th step** and needs to reach the **(n-1)th step**
- The frog can jump **1 or 2 steps** at a time
- Every jump from step `i` to step `j` costs `abs(heights[i] - heights[j])` energy
- We need to find the **minimum total energy** required

### Step 2: Represent the Problem Programmatically

- We need to track the minimum energy required to reach each step from 0 to n-1
- This suggests using a **1D array** where `dp[i]` represents minimum energy to reach step `i`

### Step 3: Finding Base Cases

**Base Case 1:** Already at step 0
```
f(0) = 0  // No energy needed, we're already here
```

**Base Case 2:** Reaching step 1
```
f(1) = abs(heights[0] - heights[1])  // Only one way: jump from 0 to 1
```

### Step 4: Finding the Recurrence Relation

To reach step `n`, we can come from either:
- **Step (n-1):** Energy = `f(n-1) + abs(heights[n-1] - heights[n])`
- **Step (n-2):** Energy = `f(n-2) + abs(heights[n-2] - heights[n])`

We choose the minimum of these two options:

```
f(n) = min(
    f(n-1) + abs(heights[n-1] - heights[n]),
    f(n-2) + abs(heights[n-2] - heights[n])
)
```

---

## Recursion Tree

Let's visualize the recursion tree for `heights = [7, 5, 1, 2, 6]` (medium example):

```
                                solve(4)
                                   |
                    +--------------+--------------+
                    |                             |
                solve(3)                      solve(2)
              [7,5,1,2,6]                   [7,5,1,2,6]
            +abs(2-6)=4                   +abs(1-6)=5
                    |                             |
          +---------+---------+         +---------+---------+
          |                   |         |                   |
      solve(2)            solve(1)  solve(1)            solve(0)
    [7,5,1,2]           [7,5,1,2] [7,5,1]             [7,5,1]
    +abs(1-2)=1         +abs(5-2)=3 +abs(5-1)=4       +abs(7-1)=6
          |                   |         |                   |
    +-----+-----+       +-----+---+   +-----+---+           0
    |           |       |         |   |         |
solve(1)    solve(0) solve(0)   X solve(0)   X
[7,5,1]     [7,5,1]  [7,5]          [7,5]
+abs(5-1)=4 +abs(7-1)=6  0              0
    |           |
    +-----+     0
    |     |
solve(0) X
[7,5]
    |
    0

Legend:
- X means base case where n < 0 (invalid, not computed)
- Numbers in brackets show the subarray being considered
- +abs(...) shows the energy cost being added at that step
```

**Notice the overlapping subproblems:**
- `solve(2)` is computed multiple times
- `solve(1)` is computed multiple times
- `solve(0)` is computed multiple times

This is why we need **memoization** to avoid redundant calculations!

---

## Solution Implementations

### 1. Recursive Solution (Top-Down)

**Time Complexity:** O(2^n) - Exponential due to overlapping subproblems  
**Space Complexity:** O(n) - Recursion stack depth

```python
def solve(n, arr):
    # Base cases
    if n == 0:
        return 0
    if n == 1:
        return abs(arr[0] - arr[1])
    
    # Try jumping from n-1 to n
    oneStep = solve(n-1, arr) + abs(arr[n] - arr[n-1])
    
    # Try jumping from n-2 to n
    twoSteps = solve(n-2, arr) + abs(arr[n] - arr[n-2])
    
    # Return minimum of both options
    return min(oneStep, twoSteps)
```

**Problem:** Many subproblems are computed repeatedly (see recursion tree above).

---

### 2. Memoization (Top-Down DP)

**Converting Recursion → Memoization:**
- **Add a memo array:** Create an array `memo` of size `n` initialized with `-1`
- **Check before computing:** Before solving `f(n)`, check if `memo[n]` is already computed
- **Store after computing:** After computing `f(n)`, store the result in `memo[n]`
- **Same recurrence relation:** The logic remains identical to recursion

**Time Complexity:** O(n) - Each subproblem computed once  
**Space Complexity:** O(n) - Memo array + O(n) recursion stack

```python
def solve_memo(n, arr, memo):
    # Check if already computed
    if memo[n] != -1:
        return memo[n]
    
    # Base cases
    if n == 0:
        return 0
    if n == 1:
        return abs(arr[0] - arr[1])
    
    # Compute and store result
    oneStep = solve_memo(n-1, arr, memo) + abs(arr[n] - arr[n-1])
    twoSteps = solve_memo(n-2, arr, memo) + abs(arr[n] - arr[n-2])
    memo[n] = min(oneStep, twoSteps)
    
    return memo[n]
```

**How it helps:** Each state `f(n)` is computed only once and reused, eliminating redundant recursive calls.

---

### 3. Tabulation (Bottom-Up DP)

**Converting Memoization → Tabulation:**
- **Eliminate recursion:** Replace recursive calls with iterative loops
- **Bottom-up order:** Start from base cases and build up to the final answer
- **Same array concept:** Use `dp` array instead of `memo`, but fill it iteratively
- **Direction change:** Instead of `f(n) → f(n-1) → f(n-2)`, we go `f(0) → f(1) → ... → f(n)`

**Time Complexity:** O(n) - Single loop through all steps  
**Space Complexity:** O(n) - DP array only (no recursion stack)

```python
def tabulation(n, arr):
    if n == 1:
        return 0
    
    # Create DP array
    dp = [-1] * n
    
    # Fill base cases
    dp[0] = 0
    dp[1] = abs(arr[0] - arr[1])
    
    # Fill remaining states from bottom to top
    for i in range(2, n):
        oneStep = dp[i-1] + abs(arr[i] - arr[i-1])
        twoSteps = dp[i-2] + abs(arr[i] - arr[i-2])
        dp[i] = min(oneStep, twoSteps)
    
    return dp[n-1]
```

**How it helps:** No recursion overhead, and we process states in a natural forward order.

---

### 4. Space Optimized (Bottom-Up with O(1) Space)

**Converting Tabulation → Space Optimized:**
- **Identify dependency:** At step `i`, we only need `dp[i-1]` and `dp[i-2]`
- **Use variables:** Instead of an entire array, maintain only two variables: `prev` and `prev2`
- **Update pattern:** After computing current, shift values: `prev2 = prev`, `prev = current`
- **Same logic:** The recurrence relation and iteration order remain unchanged

**Time Complexity:** O(n) - Single loop  
**Space Complexity:** O(1) - Only two variables

```python
def optimized(n, arr):
    if n == 1:
        return 0
    
    # Initialize with base cases
    prev2 = 0  # dp[0]
    prev = abs(arr[0] - arr[1])  # dp[1]
    
    # Compute for remaining steps
    for i in range(2, n):
        oneStep = prev + abs(arr[i] - arr[i-1])
        twoSteps = prev2 + abs(arr[i] - arr[i-2])
        current = min(oneStep, twoSteps)
        
        # Shift values for next iteration
        prev2 = prev
        prev = current
    
    return prev
```

**How it helps:** Achieves the same O(n) time complexity but uses only constant extra space.

---

## Complete Code

```python
def solve(n, arr):
    if n == 0:
        return 0
    if n == 1:
        return abs(arr[0] - arr[1])
    oneStep = solve(n-1, arr) + abs(arr[n] - arr[n-1])
    twoSteps = solve(n-2, arr) + abs(arr[n] - arr[n-2])
    return min(oneStep, twoSteps)

def solve_memo(n, arr, memo):
    if memo[n] != -1:
        return memo[n]
    if n == 0:
        return 0
    if n == 1:
        return abs(arr[0] - arr[1])
    oneStep = solve_memo(n-1, arr, memo) + abs(arr[n] - arr[n-1])
    twoSteps = solve_memo(n-2, arr, memo) + abs(arr[n] - arr[n-2])
    memo[n] = min(oneStep, twoSteps)
    return memo[n]

def tabulation(n, arr):
    if n == 1:
        return 0
    dp = [-1] * n
    dp[0] = 0
    dp[1] = abs(arr[0] - arr[1])
    for i in range(2, n):
        oneStep = dp[i-1] + abs(arr[i] - arr[i-1])
        twoSteps = dp[i-2] + abs(arr[i] - arr[i-2])
        dp[i] = min(oneStep, twoSteps)
    return dp[n-1]

def optimized(n, arr):
    if n == 1:
        return 0
    prev2 = 0
    prev = abs(arr[0] - arr[1])
    for i in range(2, n):
        oneStep = prev + abs(arr[i] - arr[i-1])
        twoSteps = prev2 + abs(arr[i] - arr[i-2])
        current = min(oneStep, twoSteps)
        prev2 = prev
        prev = current
    return prev

# Test
arr = [7, 5, 1, 2, 6]
n = len(arr)
print(solve(n-1, arr))              # Output: 9
print(solve_memo(n-1, arr, [-1]*n)) # Output: 9
print(tabulation(n, arr))            # Output: 9
print(optimized(n, arr))             # Output: 9
```

---

# Frog Jump with K Distances

## Problem Statement

A frog wants to climb a staircase with **n steps**. Given an integer array **heights**, where **heights[i]** contains the height of the ith step, and an integer **k**.

To jump from the **ith step** to the **jth step**, the frog requires **abs(heights[i] - heights[j])** energy, where abs() denotes the absolute difference.

The frog can jump from the **ith step** to any step in the range **[i + 1, i + k]**, provided it exists.

**Return** the **minimum amount of energy** required by the frog to go from the **0th step** to the **(n-1)th step**.

### Examples:

**Example 1:**
```
Input: heights = [10, 5, 20, 0, 15], k = 2
Output: 15
Explanation:
0th step → 2nd step, cost = abs(10 - 20) = 10
2nd step → 4th step, cost = abs(20 - 15) = 5
Total cost = 10 + 5 = 15
```

**Example 2:**
```
Input: heights = [15, 4, 1, 14, 15], k = 3
Output: 2
Explanation:
0th step → 2nd step, cost = abs(15 - 1) = 14 (rejected)
0th step → 3rd step, cost = abs(15 - 14) = 1 ✓
3rd step → 4th step, cost = abs(14 - 15) = 1 ✓
Total cost = 1 + 1 = 2
```

---

## Step 1: Define The Problem

- A frog starts at the **0th step**
- The frog needs to reach the **(n-1)th step**
- Each step has a **height** value
- The frog can jump forward **1 to k steps** at a time
- **Energy cost** for jumping from step i to step j = **abs(heights[i] - heights[j])**
- We need to find the **minimum total energy** required to reach the end

**Key Difference from Previous Problem:**
- Previous: Count number of ways (addition)
- Current: Find minimum energy (minimization with cost calculation)

---

## Step 2: Represent the Problem Programmatically

- **Starting point:** 0th step (index 0)
- **Target:** (n-1)th step (last index)
- **Choices at each step:** Jump 1, 2, 3, ..., or k steps forward
- **Cost calculation:** abs(heights[current] - heights[next])
- **Data structure:** 1-D array of size **n** (since we use 0-based indexing)
- **Function notation:** f(i) = minimum energy required to reach step i from step 0

---

## Step 3: Finding Base Cases

**Case 1: At step 0**
- We start at step 0, so no energy is required
- **f(0) = 0**

**Case 2: At step 1**
- We can only come from step 0
- Energy required = abs(heights[0] - heights[1])
- **f(1) = abs(heights[0] - heights[1])**

**Base cases summary:**
```
f(0) = 0
f(1) = abs(heights[0] - heights[1])
```

---

## Step 4: Finding The Recurrence Relation

To reach the **nth step**, the frog can come from any of the previous k steps:
- From step **(n-1)** with cost: f(n-1) + abs(heights[n-1] - heights[n])
- From step **(n-2)** with cost: f(n-2) + abs(heights[n-2] - heights[n])
- From step **(n-3)** with cost: f(n-3) + abs(heights[n-3] - heights[n])
- ...
- From step **(n-k)** with cost: f(n-k) + abs(heights[n-k] - heights[n])

Since we want the **minimum energy**, we take the minimum of all these options.

### ✅ Recurrence Relation:
```
f(n) = min(
    f(n-1) + abs(heights[n-1] - heights[n]),
    f(n-2) + abs(heights[n-2] - heights[n]),
    f(n-3) + abs(heights[n-3] - heights[n]),
    ...
    f(n-k) + abs(heights[n-k] - heights[n])
)
```

**In code form:**
```python
mini = float('inf')
for i in range(1, k+1):
    if (n-i) >= 0:
        cost = f(n-i) + abs(heights[n] - heights[n-i])
        mini = min(mini, cost)
return mini
```

---

## Recursion Tree Visualization

### Example: heights = [10, 5, 20, 0, 15], k = 2

```
                            f(4)
                          /      \
                         /        \
                    f(3)            f(2)
                   /    \          /    \
                  /      \        /      \
              f(2)      f(1)   f(1)     f(0)
             /   \       |      |         |
            /     \      |      |         |
         f(1)    f(0)    5     5          0
          |       |
          |       |
          5       0

Cost calculations at each node:
f(0) = 0 (base case)
f(1) = abs(10-5) = 5 (base case)

f(2): min(
    f(1) + abs(5-20) = 5 + 15 = 20,
    f(0) + abs(10-20) = 0 + 10 = 10
) = 10

f(3): min(
    f(2) + abs(20-0) = 10 + 20 = 30,
    f(1) + abs(5-0) = 5 + 5 = 10
) = 10

f(4): min(
    f(3) + abs(0-15) = 10 + 15 = 25,
    f(2) + abs(20-15) = 10 + 5 = 15
) = 15

Answer: 15
```

### Analysis:

**Redundant Computations:**
- f(3) is calculated **1 time**
- f(2) is calculated **2 times**
- f(1) is calculated **3 times**
- f(0) is calculated **2 times**

**Path taken:** 0 → 2 → 4 (cost = 10 + 5 = 15)

### Key Observation:
Similar to previous problems, we have overlapping subproblems. The difference here is that we're computing **minimum cost** instead of counting ways.

---

## Solution 1: Recursive Approach (Naive)

This is the direct implementation of our recurrence relation.

### Code:
```python
def solve(n, heights, k):
    # Base cases
    if n == 0:
        return 0
    if n == 1:
        return abs(heights[0] - heights[1])
    
    # Try all possible jumps (from previous k steps)
    mini = float('inf')
    for i in range(1, k + 1):
        if (n - i) >= 0:
            # Cost from (n-i)th step to nth step
            cost = solve(n - i, heights, k) + abs(heights[n] - heights[n - i])
            mini = min(mini, cost)
    
    return mini

# Driver Code
heights = [10, 5, 20, 0, 15]
n = len(heights)
k = 2
print(solve(n - 1, heights, k))
```

### How It Works:
For reaching step n:
1. Try all possible previous steps (n-1, n-2, ..., n-k)
2. For each previous step, calculate: minimum energy to reach that step + jump cost
3. Return the minimum among all options

### Complexity:
- **Time Complexity:** O(k^n) - Exponential with branching factor k
- **Space Complexity:** O(n) - Recursion stack depth

### Pros & Cons:
✅ Simple and intuitive
✅ Directly follows the problem statement
✅ Clear logic with min selection
❌ Extremely slow for large inputs
❌ Many redundant calculations
❌ Impractical for even moderate n values

---

## Solution 2: Memoization (Top-Down DP)

### The Idea:
Store the minimum energy required to reach each step. Once computed, reuse it instead of recalculating.

### How We Convert from Recursion:
1. **Keep the recursive structure** - Same min-finding logic
2. **Add memo array** - Store computed results
3. **Check before computing** - If memo[n] != -1, return it
4. **Store after computing** - Save the minimum in memo[n]

### Code:
```python
def solve_memo(n, heights, k, memo):
    # CHECK: Already computed?
    if memo[n] != -1:
        return memo[n]
    
    # Base cases (same as recursive)
    if n == 0:
        return 0
    if n == 1:
        return abs(heights[0] - heights[1])
    
    # Try all possible jumps (same loop as recursive)
    mini = float('inf')
    for i in range(1, k + 1):
        if (n - i) >= 0:
            cost = solve_memo(n - i, heights, k, memo) + abs(heights[n] - heights[n - i])
            mini = min(mini, cost)
    
    # STORE: Save result before returning
    memo[n] = mini
    return memo[n]

# Driver Code
heights = [10, 5, 20, 0, 15]
n = len(heights)
k = 2
memo = [-1] * n
print(solve_memo(n - 1, heights, k, memo))
```

### What Changed:
```
Recursive:                              Memoization:
─────────────────────────────          ──────────────────────────────────────
def solve(n, heights, k):             def solve_memo(n, heights, k, memo):
                                           if memo[n] != -1:        ← Check cache
                                               return memo[n]
    if n == 0:                             if n == 0:
        return 0                               return 0
    if n == 1:                             if n == 1:
        return abs(heights[0]-heights[1])      return abs(heights[0]-heights[1])
    mini = float('inf')                    mini = float('inf')
    for i in range(1, k+1):                for i in range(1, k+1):
        if (n-i) >= 0:                         if (n-i) >= 0:
            cost = solve(n-i, heights, k)          cost = solve_memo(n-i, heights, k, memo)
                   + abs(heights[n]-heights[n-i])         + abs(heights[n]-heights[n-i])
            mini = min(mini, cost)                 mini = min(mini, cost)
                                           memo[n] = mini           ← Store result
    return mini                            return memo[n]
```

### How It Works (Example: heights=[10,5,20,0,15], k=2):
```
Call f(4):
  → Try from step 3: Need f(3) - NOT in memo
    → f(3) tries steps 2 and 1
      → Need f(2) - NOT in memo, compute = 10, STORE memo[2]=10
      → Need f(1) - base case = 5
      → f(3) = min(10+20, 5+5) = 10, STORE memo[3]=10
  → Try from step 2: Need f(2) - FOUND in memo[2]=10! No recalculation!
  → f(4) = min(10+15, 10+5) = 15, STORE memo[4]=15

Each step's minimum energy is computed only once!
```

### Complexity:
- **Time Complexity:** O(n × k) - n subproblems, each tries k jumps
- **Space Complexity:** O(n) - Memo array + recursion stack

### Pros & Cons:
✅ Much faster than naive recursion
✅ Each subproblem computed only once
✅ Easy to convert from recursive solution
✅ Efficient for any k value
❌ Still uses recursion stack
❌ Uses O(n) extra space

---

## Solution 3: Tabulation (Bottom-Up DP)

### The Idea:
Build the solution iteratively from the start (step 0) to the end (step n-1). For each step, calculate the minimum energy by looking back at the previous k steps.

### How We Convert from Memoization:
1. **Remove recursion** - Use nested loops
2. **Create DP array** - dp[i] = minimum energy to reach step i
3. **Fill base cases** - dp[0] = 0, dp[1] = abs(heights[0] - heights[1])
4. **Outer loop:** Iterate from step 2 to n-1
5. **Inner loop:** For each step i, try all k possible previous steps

### Code:
```python
def tabulation(n, heights, k):
    # Handle edge case
    if n == 1:
        return 0
    
    # Create DP array
    dp = [-1] * n
    
    # Fill base cases
    dp[0] = 0
    dp[1] = abs(heights[0] - heights[1])
    
    # Build up from step 2 to n-1
    for i in range(2, n):
        mini = float('inf')
        
        # Try all possible previous steps (within k range)
        for j in range(1, k + 1):
            if (i - j) >= 0:
                cost = dp[i - j] + abs(heights[i] - heights[i - j])
                mini = min(mini, cost)
        
        dp[i] = mini
    
    return dp[n - 1]

# Driver Code
heights = [10, 5, 20, 0, 15]
n = len(heights)
k = 2
print(tabulation(n, heights, k))
```

### What Changed:
```
Memoization:                            Tabulation:
─────────────────────────────────      ──────────────────────────────────────
def solve_memo(n, heights, k, memo):   def tabulation(n, heights, k):
    if memo[n] != -1:                      if n == 1:
        return memo[n]                         return 0
    if n == 0:                             dp = [-1] * n
        return 0                           dp[0] = 0  ← Fill base cases
    if n == 1:                             dp[1] = abs(heights[0]-heights[1])
        return abs(...)                    for i in range(2, n):  ← Outer loop
    mini = float('inf')                        mini = float('inf')
    for i in range(1, k+1):                    for j in range(1, k+1):  ← Inner loop
        if (n-i) >= 0:                             if (i-j) >= 0:
            cost = solve_memo(...)                     cost = dp[i-j] + abs(...)
            mini = min(mini, cost)                     mini = min(mini, cost)
    memo[n] = mini                             dp[i] = mini
    return memo[n]                         return dp[n-1]
```

### How It Works (Example: heights=[10,5,20,0,15], k=2):
```
Step 1: Initialize
        dp = [-1, -1, -1, -1, -1]

Step 2: Fill base cases
        dp = [0, 5, -1, -1, -1]
             ↑  ↑
          step0 step1

Step 3: Build iteratively

i=2: Try previous steps within k=2 range
     From step 1: dp[1] + abs(heights[2]-heights[1]) = 5 + abs(20-5) = 5 + 15 = 20
     From step 0: dp[0] + abs(heights[2]-heights[0]) = 0 + abs(20-10) = 0 + 10 = 10
     dp[2] = min(20, 10) = 10
     dp = [0, 5, 10, -1, -1]

i=3: Try previous steps within k=2 range
     From step 2: dp[2] + abs(heights[3]-heights[2]) = 10 + abs(0-20) = 10 + 20 = 30
     From step 1: dp[1] + abs(heights[3]-heights[1]) = 5 + abs(0-5) = 5 + 5 = 10
     dp[3] = min(30, 10) = 10
     dp = [0, 5, 10, 10, -1]

i=4: Try previous steps within k=2 range
     From step 3: dp[3] + abs(heights[4]-heights[3]) = 10 + abs(15-0) = 10 + 15 = 25
     From step 2: dp[2] + abs(heights[4]-heights[2]) = 10 + abs(15-20) = 10 + 5 = 15
     dp[4] = min(25, 15) = 15
     dp = [0, 5, 10, 10, 15]

Return dp[4] = 15
```

### Visual Flow:
```
For each step i, look back at previous k steps:

        [i-k] ... [i-2] [i-1] [i=?]
          ↓         ↓     ↓
          └─────────┴─────┘
      Try all paths, choose minimum cost
```

### Complexity:
- **Time Complexity:** O(n × k) - For each of n steps, check k previous steps
- **Space Complexity:** O(n) - DP array only

### Pros & Cons:
✅ Fast and efficient
✅ No recursion stack overhead
✅ Clear iterative logic
✅ Easy to trace and debug
✅ Can see the entire dp array progression
❌ Uses O(n) space

---

## Solution 4: Space Optimization

### The Idea:
To calculate dp[i], we only need the values of the **last k steps** (dp[i-1], dp[i-2], ..., dp[i-k]). We could use a sliding window approach.

### Why It's Not Preferred:

**Reasons:**
1. **Increased Code Complexity** - Managing a sliding window (deque/circular buffer) is more complex
2. **No Time Improvement** - Still O(n × k), same as tabulation
3. **Marginal Space Savings** - Only saves from O(n) to O(k)
4. **When k is small** (common case), the savings are minimal
5. **Harder to Debug** - Can't inspect full dp array
6. **Less Readable** - The logic becomes obscured

### Conceptual Approach:
```python
from collections import deque

def optimized(n, heights, k):
    if n == 1:
        return 0
    
    # Keep last k values in a deque
    window = deque()
    window.append((0, 0))  # (index, min_energy)
    window.append((1, abs(heights[0] - heights[1])))
    
    for i in range(2, n):
        mini = float('inf')
        
        # Look at elements in window (last k steps)
        for idx, energy in window:
            if i - idx <= k:
                cost = energy + abs(heights[i] - heights[idx])
                mini = min(mini, cost)
        
        # Add current step to window
        window.append((i, mini))
        
        # Remove elements outside k range
        while window and i - window[0][0] > k:
            window.popleft()
    
    return window[-1][1]
```

### Trade-offs:
| Aspect | Tabulation | Space Optimized |
|--------|-----------|-----------------|
| Time | O(n × k) | O(n × k) |
| Space | O(n) | O(k) |
| Complexity | Simple | Complex |
| Debugging | Easy | Difficult |
| Readability | High | Low |

**Recommendation:** Use **Tabulation** for this problem. The space savings don't justify the added complexity.

---



## Complete Working Code

```python
# Solution 1: Recursive
def solve(n, heights, k):
    if n == 0:
        return 0
    if n == 1:
        return abs(heights[0] - heights[1])
    mini = float('inf')
    for i in range(1, k + 1):
        if (n - i) >= 0:
            cost = solve(n - i, heights, k) + abs(heights[n] - heights[n - i])
            mini = min(mini, cost)
    return mini

# Solution 2: Memoization
def solve_memo(n, heights, k, memo):
    if memo[n] != -1:
        return memo[n]
    if n == 0:
        return 0
    if n == 1:
        return abs(heights[0] - heights[1])
    mini = float('inf')
    for i in range(1, k + 1):
        if (n - i) >= 0:
            cost = solve_memo(n - i, heights, k, memo) + abs(heights[n] - heights[n - i])
            mini = min(mini, cost)
    memo[n] = mini
    return memo[n]

# Solution 3: Tabulation
def tabulation(n, heights, k):
    if n == 1:
        return 0
    dp = [-1] * n
    dp[0] = 0
    dp[1] = abs(heights[0] - heights[1])
    for i in range(2, n):
        mini = float('inf')
        for j in range(1, k + 1):
            if (i - j) >= 0:
                cost = dp[i - j] + abs(heights[i] - heights[i - j])
                mini = min(mini, cost)
        dp[i] = mini
    return dp[n - 1]

# Driver Code
heights = [10, 5, 20, 0, 15]
n = len(heights)
k = 2
print("Recursive:", solve(n - 1, heights, k))
print("Memoization:", solve_memo(n - 1, heights, k, [-1] * n))
print("Tabulation:", tabulation(n, heights, k))
```

---
# Maximum Sum of Non-Adjacent Elements

## Problem Statement

Given an integer array **nums** of size **n**. Return the **maximum sum possible** using the elements of nums such that **no two elements taken are adjacent** in nums.

**Note:** A subsequence of an array is a list with elements of the array where some elements are deleted (or not deleted at all) and the elements should be in the same order in the subsequence as in the array.

### Examples:

**Example 1:**
```
Input: nums = [1, 2, 4]
Output: 5
Explanation:
[1, 2, 4] → Take elements at index 0 and 2
Maximum sum = 1 + 4 = 5
```

**Example 2:**
```
Input: nums = [2, 1, 4, 9]
Output: 11
Explanation:
[2, 1, 4, 9] → Take elements at index 0 and 2
Maximum sum = 2 + 9 = 11
```

**Example 3:**
```
Input: nums = [1, 2, 3, 4]
Output: 6
Explanation:
[1, 2, 3, 4] → Take elements at index 1 and 3
Maximum sum = 2 + 4 = 6
```

---

## Step 1: Define The Problem

- Given an array of **N positive integers**
- We need to find the **maximum sum** of elements
- **Constraint:** No two selected elements should be adjacent to each other in the array

**Example:** For arr = [1, 2, 3, 4]

**Valid non-adjacent subsequences:**
- [1, 3] → sum = 4
- [1, 4] → sum = 5
- [2, 4] → sum = 6 ✓ (maximum)
- [1] → sum = 1
- [2] → sum = 2
- [3] → sum = 3
- [4] → sum = 4

**Invalid subsequence:**
- [1, 2] → Adjacent elements
- [2, 3] → Adjacent elements
- [3, 4] → Adjacent elements

---

## Step 2: Represent the Problem Programmatically

- Given an array where each element has an **index**
- **Function notation:** f(index) = maximum sum of non-adjacent elements from index 0 to index
- **Goal:** Find f(n-1), where n = size of array
- This represents the maximum sum we can achieve considering all elements from start to end

---

## Step 3: Finding Base Cases

**Case 1: Array with 1 element**
- Only one element exists, no adjacency issue
- We must take that element
- **f(0) = arr[0]**

**Case 2: Array with 2 elements**
- Both elements are adjacent to each other
- We can only pick one of them
- Pick the maximum of the two
- **f(1) = max(arr[0], arr[1])**

**Base cases summary:**
```
f(0) = arr[0]
f(1) = max(arr[0], arr[1])
```

---

## Step 4: Finding The Recurrence Relation

Let's solve for a small example: **arr = [1, 2, 3]**

**Known base cases:**
- f(0) = arr[0] = 1 (maximum sum at index 0)
- f(1) = max(arr[0], arr[1]) = max(1, 2) = 2 (maximum sum from index 0 to 1)

**Finding f(2)** (maximum sum from index 0 to 2):

At index 2, we have **two choices**:

### Choice 1: Include element at index 2
- If we include arr[2] = 3, we **cannot** include arr[1] (adjacent)
- We can only add the maximum sum from index 0 (which is before the adjacent position)
- **Include = arr[2] + f(0) = 3 + 1 = 4**

### Choice 2: Exclude element at index 2
- If we don't include arr[2], we take the maximum sum up to index 1
- **Exclude = f(1) = 2**

### Final Answer:
- f(2) = max(Include, Exclude) = max(4, 2) = **4**

### ✅ Recurrence Relation:
```
f(n) = max(
    arr[n] + f(n-2),    // Include current element
    f(n-1)              // Exclude current element
)
```

**Breaking it down:**
- **Include current element:** arr[n] + f(n-2)
  - Add current element value
  - Add maximum sum from 2 positions back (skip adjacent)
  
- **Exclude current element:** f(n-1)
  - Don't add current element
  - Take maximum sum up to previous index

**Final:** Take the maximum of both choices

---

## Recursion Tree Visualization

### Example: arr = [2, 1, 4, 9]

```
                                f(3)
                          /              \
                         /                \
                    Include 9           Exclude 9
                   arr[3]+f(1)             f(2)
                      /                  /      \
                     /                  /        \
                  f(1)              Include 4   Exclude 4
                 /    \            arr[2]+f(0)    f(1)
                /      \              /           /   \
           Include 1  Exclude 1     f(0)     Include 1  Exclude 1
          arr[1]+f(-1)  f(0)        |        arr[1]+f(-1) f(0)
              |          |           |           |          |
              1          2           2           1          2

Calculations:
f(0) = arr[0] = 2 (base case)
f(1) = max(arr[0], arr[1]) = max(2, 1) = 2 (base case)

f(2) = max(
    arr[2] + f(0) = 4 + 2 = 6,   // Include 4
    f(1) = 2                      // Exclude 4
) = 6

f(3) = max(
    arr[3] + f(1) = 9 + 2 = 11,  // Include 9
    f(2) = 6                      // Exclude 9
) = 11

Answer: 11
```

### Analysis:

**Redundant Computations:**
- f(2) is calculated **1 time**
- f(1) is calculated **3 times**
- f(0) is calculated **2 times**

**Optimal path:** Take elements at index 0 and 3 → [2, 9] → sum = 11

### Key Observation:
Each subproblem (f(i)) is computed multiple times in the recursion tree, making it inefficient. Dynamic Programming eliminates this redundancy.

---

## Solution 1: Recursive Approach (Naive)

This is the direct implementation of our recurrence relation with two choices at each step.

### Code:
```python
def solve(n, arr):
    # Base cases
    if n == 0:
        return arr[0]
    if n == 1:
        return max(arr[0], arr[1])
    
    # Choice 1: Include current element
    include = arr[n] + solve(n - 2, arr)
    
    # Choice 2: Exclude current element
    exclude = solve(n - 1, arr)
    
    # Return maximum of both choices
    return max(include, exclude)

# Driver Code
arr = [2, 1, 4, 9]
n = len(arr)
print(solve(n - 1, arr))
```

### How It Works:
For each index n:
1. **Include option:** Take current element + maximum from (n-2)
2. **Exclude option:** Take maximum from (n-1)
3. Return the larger of the two

### Complexity:
- **Time Complexity:** O(2^n) - Binary tree recursion
- **Space Complexity:** O(n) - Recursion stack depth

### Pros & Cons:
✅ Simple and intuitive
✅ Directly models the problem as "include or exclude"
✅ Clear two-choice logic
❌ Extremely slow for large inputs
❌ Many redundant calculations
❌ Exponential time complexity

---

## Solution 2: Memoization (Top-Down DP)

### The Idea:
Cache the maximum sum for each index. Once computed for an index, reuse it instead of recalculating.

### How We Convert from Recursion:
1. **Keep the recursive structure** - Same include/exclude logic
2. **Add memo array** - Store computed maximum sums
3. **Check before computing** - If memo[n] != -1, return it
4. **Store after computing** - Save the maximum in memo[n]

### Code:
```python
def solve_memo(n, arr, memo):
    # CHECK: Already computed?
    if memo[n] != -1:
        return memo[n]
    
    # Base cases (same as recursive)
    if n == 0:
        return arr[0]
    if n == 1:
        return max(arr[0], arr[1])
    
    # Choice 1: Include current element (same as recursive)
    include = arr[n] + solve_memo(n - 2, arr, memo)
    
    # Choice 2: Exclude current element (same as recursive)
    exclude = solve_memo(n - 1, arr, memo)
    
    # STORE: Save result before returning
    memo[n] = max(include, exclude)
    return memo[n]

# Driver Code
arr = [2, 1, 4, 9]
n = len(arr)
memo = [-1] * n
print(solve_memo(n - 1, arr, memo))
```

### What Changed:
```
Recursive:                          Memoization:
───────────────────────            ────────────────────────────────
def solve(n, arr):                 def solve_memo(n, arr, memo):
                                       if memo[n] != -1:        ← Check cache
                                           return memo[n]
    if n == 0:                         if n == 0:
        return arr[0]                      return arr[0]
    if n == 1:                         if n == 1:
        return max(arr[0], arr[1])         return max(arr[0], arr[1])
    include = arr[n] + solve(n-2)      include = arr[n] + solve_memo(n-2, memo)
    exclude = solve(n-1)               exclude = solve_memo(n-1, memo)
                                       memo[n] = max(include, exclude)  ← Store
    return max(include, exclude)       return memo[n]
```

### How It Works (Example: arr=[2,1,4,9]):
```
Call f(3):
  → Try include: Need f(1)
    → f(1) = max(2, 1) = 2 (base case)
    → include = 9 + 2 = 11
  → Try exclude: Need f(2) - NOT in memo
    → f(2) tries:
      → include: arr[2] + f(0) = 4 + 2 = 6
      → exclude: f(1) = 2 (already computed!)
      → f(2) = max(6, 2) = 6, STORE memo[2]=6
    → exclude = 6
  → f(3) = max(11, 6) = 11, STORE memo[3]=11

Each index's maximum sum is computed only once!
```

### Complexity:
- **Time Complexity:** O(n) - Each subproblem computed once
- **Space Complexity:** O(n) - Memo array + recursion stack

### Pros & Cons:
✅ Much faster than naive recursion
✅ Each subproblem computed only once
✅ Easy to convert from recursive solution
✅ Maintains the include/exclude logic clarity
❌ Still uses recursion stack
❌ Uses O(n) extra space

---

## Solution 3: Tabulation (Bottom-Up DP)

### The Idea:
Build the solution iteratively from the start (index 0) to the end (index n-1). For each index, calculate the maximum sum by considering include/exclude choices.

### How We Convert from Memoization:
1. **Remove recursion** - Use a loop instead
2. **Create DP array** - dp[i] = maximum sum from index 0 to i
3. **Fill base cases** - dp[0] = arr[0], dp[1] = max(arr[0], arr[1])
4. **Loop from index 2 to n-1** - Apply recurrence relation
5. **Each step uses previously computed values** - No backward calls

### Code:
```python
def tabulation(n, arr):
    # Handle edge case
    if n == 1:
        return arr[0]
    
    # Create DP array
    dp = [0] * n
    
    # Fill base cases
    dp[0] = arr[0]
    dp[1] = max(arr[0], arr[1])
    
    # Build up from index 2 to n-1
    for i in range(2, n):
        # Choice 1: Include current element
        include = arr[i] + dp[i - 2]
        
        # Choice 2: Exclude current element
        exclude = dp[i - 1]
        
        # Store maximum of both choices
        dp[i] = max(include, exclude)
    
    return dp[n - 1]

# Driver Code
arr = [2, 1, 4, 9]
n = len(arr)
print(tabulation(n, arr))
```

### What Changed:
```
Memoization:                        Tabulation:
──────────────────────────         ──────────────────────────────────
def solve_memo(n, arr, memo):      def tabulation(n, arr):
    if memo[n] != -1:                  if n == 1:
        return memo[n]                     return arr[0]
    if n == 0:                         dp = [0] * n
        return arr[0]                  dp[0] = arr[0]         ← Fill base
    if n == 1:                         dp[1] = max(arr[0], arr[1])
        return max(arr[0], arr[1])     for i in range(2, n):  ← Loop
    include = arr[n] + solve_memo          include = arr[i] + dp[i-2]
    exclude = solve_memo(n-1)              exclude = dp[i-1]
    memo[n] = max(include, exclude)        dp[i] = max(include, exclude)
    return memo[n]                     return dp[n-1]
```

### How It Works (Example: arr=[2,1,4,9]):
```
Step 1: Initialize
        dp = [0, 0, 0, 0]

Step 2: Fill base cases
        dp = [2, 2, 0, 0]
             ↑  ↑
          f(0) f(1)
        f(0) = arr[0] = 2
        f(1) = max(arr[0], arr[1]) = max(2, 1) = 2

Step 3: Build iteratively

i=2: 
  include = arr[2] + dp[0] = 4 + 2 = 6
  exclude = dp[1] = 2
  dp[2] = max(6, 2) = 6
  dp = [2, 2, 6, 0]
  
  Interpretation: Max sum from index 0 to 2 is 6 (take arr[0] and arr[2])

i=3:
  include = arr[3] + dp[1] = 9 + 2 = 11
  exclude = dp[2] = 6
  dp[3] = max(11, 6) = 11
  dp = [2, 2, 6, 11]
  
  Interpretation: Max sum from index 0 to 3 is 11 (take arr[0] and arr[3])

Return dp[3] = 11
```

### Visual Representation:
```
Array:  [2, 1, 4, 9]
Index:   0  1  2  3

Building dp array:
dp[0] = 2               (take element 0)
dp[1] = max(2, 1) = 2   (take element 0)
dp[2] = max(4+2, 2) = 6 (take elements 0,2)
dp[3] = max(9+2, 6) = 11 (take elements 0,3)

At each index i, we decide:
  - Include arr[i]: Add to dp[i-2] (skip adjacent)
  - Exclude arr[i]: Take dp[i-1] (best so far)
```

### Complexity:
- **Time Complexity:** O(n) - Single loop through array
- **Space Complexity:** O(n) - DP array only

### Pros & Cons:
✅ Fast and efficient
✅ No recursion stack overhead
✅ Clear iterative logic
✅ Easy to trace and debug
✅ Can see the entire dp array progression
❌ Uses O(n) space

---

## Solution 4: Space Optimized (Best Solution)

### The Idea:
Looking at the recurrence relation, to calculate dp[i], we only need:
- **dp[i-1]** (exclude current)
- **dp[i-2]** (include current)

We don't need the entire array! Just keep track of the last two values.

### How We Convert from Tabulation:
1. **Identify what we actually need** - Only last 2 values
2. **Replace array with 2 variables** - prev2 (i-2) and prev (i-1)
3. **Update variables in each iteration** - Shift values forward
4. **Same loop logic** - Just different storage

### Code:
```python
def optimized(n, arr):
    # Handle edge case
    if n == 1:
        return arr[0]
    
    # Initialize with base cases
    prev2 = arr[0]                    # dp[0]
    prev = max(arr[0], arr[1])        # dp[1]
    
    # Build up from index 2 to n-1
    for i in range(2, n):
        # Choice 1: Include current element
        include = arr[i] + prev2
        
        # Choice 2: Exclude current element
        exclude = prev
        
        # Current maximum
        current = max(include, exclude)
        
        # Shift values for next iteration
        prev2 = prev
        prev = current
    
    return prev

# Driver Code
arr = [2, 1, 4, 9]
n = len(arr)
print(optimized(n, arr))
```

### What Changed:
```
Tabulation:                         Space Optimized:
───────────────────────            ────────────────────────────────
def tabulation(n, arr):            def optimized(n, arr):
    if n == 1:                         if n == 1:
        return arr[0]                      return arr[0]
    dp = [0] * n                       prev2 = arr[0]        ← Just 2 vars
    dp[0] = arr[0]                     prev = max(arr[0], arr[1])
    dp[1] = max(arr[0], arr[1])        for i in range(2, n):
    for i in range(2, n):                  include = arr[i] + prev2
        include = arr[i] + dp[i-2]         exclude = prev
        exclude = dp[i-1]                  current = max(include, exclude)
        dp[i] = max(include, exclude)      prev2 = prev       ← Shift values
    return dp[n-1]                         prev = current
                                       return prev
```

### How It Works (Example: arr=[2,1,4,9]):
```
Initial state:
  prev2 = arr[0] = 2
  prev = max(arr[0], arr[1]) = max(2, 1) = 2

i=2:
  include = arr[2] + prev2 = 4 + 2 = 6
  exclude = prev = 2
  current = max(6, 2) = 6
  Shift: prev2 = 2 → prev2 = prev = 2
         prev = 2 → prev = current = 6
  State: prev2=2, prev=6

i=3:
  include = arr[3] + prev2 = 9 + 2 = 11
  exclude = prev = 6
  current = max(11, 6) = 11
  Shift: prev2 = 6 → prev2 = prev = 6
         prev = 6 → prev = current = 11
  State: prev2=6, prev=11

Return prev = 11
```

### Visual Representation of Sliding Window:
```
Array: [2, 1, 4, 9]

Initial:
  [prev2=2] [prev=2] [?] [?]
     ↑        ↑
   dp[0]    dp[1]

i=2:
  [  2  ] [prev2=2] [prev=6] [?]
                      ↑
              current calculated

i=3:
  [  2  ] [  2  ] [prev2=6] [prev=11]
                               ↑
                      current calculated

Only keep the "sliding window" of last 2 values!
```

### Complexity:
- **Time Complexity:** O(n) - Single loop
- **Space Complexity:** O(1) - Only 2-3 variables!

### Pros & Cons:
✅ Optimal time complexity
✅ Minimal space usage - constant space!
✅ No recursion
✅ No extra arrays
✅ Same logic as tabulation
✅ **Best solution overall**

---

## Summary of Transformations

### Recursion → Memoization:
- **Add memo array** to cache maximum sums
- **Check memo[n]** before computing
- **Store result** in memo[n] after finding max of include/exclude
- **Same two-choice logic** preserved

### Memoization → Tabulation:
- **Remove recursion** - use loop from 2 to n-1
- **Fill base cases first** - dp[0] and dp[1]
- **Apply same recurrence** - include/exclude at each step
- **Bottom-up building** - each dp[i] uses dp[i-1] and dp[i-2]

### Tabulation → Space Optimization:
- **Identify dependency** - only need last 2 values
- **Replace array with 2 variables** - prev2 and prev
- **Shift values in each iteration** - maintain sliding window
- **Same include/exclude logic** - just different storage

---

## Key Insights

### Problem Pattern:
This is a classic **"Pick or Not Pick"** DP pattern:
- At each element, we have two choices
- Picking has a constraint (no adjacent)
- We want to maximize/minimize some value

### Similar Problems:
- House Robber
- Delete and Earn
- Paint House
- Maximum Product Subarray (with modifications)

### Core Concept:
```
Decision at each index:
  ┌─────────────────┐
  │  Include arr[i] │ → Add arr[i] + dp[i-2]
  │       OR        │
  │  Exclude arr[i] │ → Take dp[i-1]
  └─────────────────┘
         Take MAX
```

---

## Complete Working Code

```python
# Solution 1: Recursive
def solve(n, arr):
    if n == 0:
        return arr[0]
    if n == 1:
        return max(arr[0], arr[1])
    include = arr[n] + solve(n - 2, arr)
    exclude = solve(n - 1, arr)
    return max(include, exclude)

# Solution 2: Memoization
def solve_memo(n, arr, memo):
    if memo[n] != -1:
        return memo[n]
    if n == 0:
        return arr[0]
    if n == 1:
        return max(arr[0], arr[1])
    include = arr[n] + solve_memo(n - 2, arr, memo)
    exclude = solve_memo(n - 1, arr, memo)
    memo[n] = max(include, exclude)
    return memo[n]

# Solution 3: Tabulation
def tabulation(n, arr):
    if n == 1:
        return arr[0]
    dp = [0] * n
    dp[0] = arr[0]
    dp[1] = max(arr[0], arr[1])
    for i in range(2, n):
        include = arr[i] + dp[i - 2]
        exclude = dp[i - 1]
        dp[i] = max(include, exclude)
    return dp[n - 1]

# Solution 4: Space Optimized
def optimized(n, arr):
    if n == 1:
        return arr[0]
    prev2 = arr[0]
    prev = max(arr[0], arr[1])
    for i in range(2, n):
        include = arr[i] + prev2
        exclude = prev
        current = max(include, exclude)
        prev2 = prev
        prev = current
    return prev

# Driver Code
arr = [2, 1, 4, 9]
n = len(arr)
print("Recursive:", solve(n - 1, arr))
print("Memoization:", solve_memo(n - 1, arr, [-1] * n))
print("Tabulation:", tabulation(n, arr))
print("Optimized:", optimized(n, arr))
```

---

# House Robber (Circular Street)

## Problem Statement

A robber is targeting to rob houses from a street. Each house has security measures that **alert the police when two adjacent houses are robbed**. 

The houses are arranged in a **circular manner**, thus the **first and last houses are adjacent to each other**.

Given an integer array **money**, where **money[i]** represents the amount of money that can be looted from the **(i+1)th house**. 

Return the **maximum amount of money** that the robber can loot without alerting the police.

### Examples:

**Example 1:**
```
Input: money = [2, 1, 4, 9]
Output: 10
Explanation:
[2, 1, 4, 9] → Rob houses at index 1 and 3
Maximum loot = 1 + 9 = 10
Note: We cannot loot the 1st (index 0) and 4th (index 3) houses together
because they are adjacent in circular arrangement.
```

**Example 2:**
```
Input: money = [1, 5, 2, 1, 6]
Output: 11
Explanation:
[1, 5, 2, 1, 6] → Rob houses at index 1 and 4
Maximum loot = 5 + 6 = 11
```

---

## Step 1: Understanding the Circular Constraint

### Previous Problem vs Current Problem:

**Previous (Linear Street):**
```
Houses: [2, 1, 4, 9]
Layout: 2 — 1 — 4 — 9
        ↑               (no connection)
```
- First and last houses are NOT adjacent
- We can rob both house 0 and house 3 if we skip middle houses

**Current (Circular Street):**
```
Houses: [2, 1, 4, 9]
Layout:     2 — 1
            |     |
            9 — 4
```
- First and last houses ARE adjacent (connected in a circle)
- We **CANNOT** rob both house 0 and house 3 together

### Key Insight:
Due to the circular arrangement, **we cannot consider the first and last elements simultaneously** in our answer.

This gives us two scenarios:
1. **Include first house** → Cannot include last house
2. **Include last house** → Cannot include first house

---

## Step 2: Breaking Down the Problem

Since we can't include both first and last houses, we solve two separate subproblems:

### Scenario 1: Exclude the Last House
- Consider houses: **[0, 1, 2, ..., n-2]** (exclude house n-1)
- This means we can potentially include the first house
- Array becomes: `arr[0:n-1]` or `money[:-1]`
- Find maximum sum of non-adjacent elements in this array
- Call this answer **ans1**

### Scenario 2: Exclude the First House
- Consider houses: **[1, 2, 3, ..., n-1]** (exclude house 0)
- This means we can potentially include the last house
- Array becomes: `arr[1:n]` or `money[1:]`
- Find maximum sum of non-adjacent elements in this array
- Call this answer **ans2**

### Final Answer:
```
Maximum money = max(ans1, ans2)
```

We take the maximum of both scenarios because one of them will give us the optimal answer.

---

## Step 3: Visual Representation

### Example: money = [2, 1, 4, 9]

**Original Circular Arrangement:**
```
        2 — 1
        |     |
        9 — 4
        
First (2) and Last (9) are adjacent!
```

**Scenario 1: Exclude Last House**
```
Linear Array: [2, 1, 4]
              
Can rob: 2 and 4 → Sum = 6
```

**Scenario 2: Exclude First House**
```
Linear Array: [1, 4, 9]
              
Can rob: 1 and 9 → Sum = 10 ✓
```

**Final Answer:** max(6, 10) = **10**

---

## Step 4: The Core Algorithm (from Previous Problem)

We use the **Maximum Sum of Non-Adjacent Elements** algorithm from the previous problem for each scenario.

### Optimized Space Solution:
```python
def optimized(n, arr):
    # Handle edge case
    if n == 1:
        return arr[0]
    
    # Initialize with base cases
    prev2 = arr[0]                    # dp[0]
    prev = max(arr[0], arr[1])        # dp[1]
    
    # Build up from index 2 to n-1
    for i in range(2, n):
        # Include current house
        include = arr[i] + prev2
        
        # Exclude current house
        exclude = prev
        
        # Take maximum
        current = max(include, exclude)
        
        # Shift values for next iteration
        prev2 = prev
        prev = current
    
    return prev
```

This function finds the maximum sum of non-adjacent elements in a **linear array**.

---

## Complete Solution

### Code:
```python
def optimized(n, arr):
    """
    Find maximum sum of non-adjacent elements in a linear array
    """
    if n == 1:
        return arr[0]
    
    prev2 = arr[0]
    prev = max(arr[0], arr[1])
    
    for i in range(2, n):
        include = arr[i] + prev2
        exclude = prev
        current = max(include, exclude)
        prev2 = prev
        prev = current
    
    return prev

class Solution:
    def houseRobber(self, money):
        arr = money
        n = len(arr)
        
        # Edge case: Only one house
        if n == 1:
            return arr[0]
        
        # Scenario 1: Exclude last house (can include first)
        ans1 = optimized(n - 1, arr[:n-1])  # arr[0] to arr[n-2]
        
        # Scenario 2: Exclude first house (can include last)
        ans2 = optimized(n - 1, arr[1:])    # arr[1] to arr[n-1]
        
        # Return maximum of both scenarios
        return max(ans1, ans2)

# Driver Code
solution = Solution()
money = [2, 1, 4, 9]
print(solution.houseRobber(money))
```

---

## Step-by-Step Execution

### Example: money = [2, 1, 4, 9]

**Step 1: Check edge case**
- n = 4, so we proceed

**Step 2: Scenario 1 - Exclude last house**
```
Array: [2, 1, 4] (exclude 9)
n = 3

Execution of optimized(3, [2, 1, 4]):
  Initial:
    prev2 = arr[0] = 2
    prev = max(arr[0], arr[1]) = max(2, 1) = 2
  
  i=2:
    include = arr[2] + prev2 = 4 + 2 = 6
    exclude = prev = 2
    current = max(6, 2) = 6
    prev2 = 2, prev = 6
  
  Return prev = 6

ans1 = 6 (rob houses at index 0 and 2: [2, 4])
```

**Step 3: Scenario 2 - Exclude first house**
```
Array: [1, 4, 9] (exclude 2)
n = 3

Execution of optimized(3, [1, 4, 9]):
  Initial:
    prev2 = arr[0] = 1
    prev = max(arr[0], arr[1]) = max(1, 4) = 4
  
  i=2:
    include = arr[2] + prev2 = 9 + 1 = 10
    exclude = prev = 4
    current = max(10, 4) = 10
    prev2 = 4, prev = 10
  
  Return prev = 10

ans2 = 10 (rob houses at index 0 and 2 of this array: [1, 9])
           (which are index 1 and 3 in original array)
```

**Step 4: Final answer**
```
max(ans1, ans2) = max(6, 10) = 10
```

---

## Another Example: money = [1, 5, 2, 1, 6]

**Circular Arrangement:**
```
        1 — 5
        |     |
        6 — 1
          |
          2
```

**Scenario 1: Exclude last house (6)**
```
Array: [1, 5, 2, 1]

Optimal: Rob houses at index 1 and 3 → [5, 1] → Sum = 6
ans1 = 6
```

**Scenario 2: Exclude first house (1)**
```
Array: [5, 2, 1, 6]

Optimal: Rob houses at index 0 and 3 → [5, 6] → Sum = 11 ✓
ans2 = 11
```

**Final Answer:** max(6, 11) = **11**

This corresponds to robbing houses at index 1 and 4 in the original array: [5, 6]

---

## Why This Approach Works

### Correctness Proof:

**Claim:** The optimal solution either:
1. Does NOT include the last house, OR
2. Does NOT include the first house

**Proof:**
- In a circular arrangement, the first and last houses are adjacent
- If we include BOTH first and last houses, the alarm triggers (adjacent houses)
- Therefore, at least ONE of them must be excluded
- By trying both scenarios and taking the maximum, we cover all valid possibilities

**Coverage:**
- **ans1** covers all valid solutions that exclude the last house
- **ans2** covers all valid solutions that exclude the first house
- **max(ans1, ans2)** gives us the optimal solution

---

## Complexity Analysis

### Time Complexity:
- **Scenario 1:** O(n) - Process array of size n-1
- **Scenario 2:** O(n) - Process array of size n-1
- **Total:** O(n) + O(n) = **O(n)**

### Space Complexity:
- Each call to `optimized()` uses O(1) space (only variables)
- Array slicing creates new arrays: O(n) space
- **Total:** **O(n)** due to array slicing

**Note:** If we avoid array slicing by using index ranges, we can achieve O(1) space.

---

## Space-Optimized Version (No Array Slicing)

```python
def optimized_range(arr, start, end):
    """
    Find maximum sum of non-adjacent elements from start to end index
    """
    n = end - start + 1
    
    if n == 1:
        return arr[start]
    
    prev2 = arr[start]
    prev = max(arr[start], arr[start + 1])
    
    for i in range(start + 2, end + 1):
        include = arr[i] + prev2
        exclude = prev
        current = max(include, exclude)
        prev2 = prev
        prev = current
    
    return prev

class Solution:
    def houseRobber(self, money):
        n = len(money)
        
        # Edge case: Only one house
        if n == 1:
            return money[0]
        
        # Scenario 1: Exclude last house (indices 0 to n-2)
        ans1 = optimized_range(money, 0, n - 2)
        
        # Scenario 2: Exclude first house (indices 1 to n-1)
        ans2 = optimized_range(money, 1, n - 1)
        
        return max(ans1, ans2)
```

**Space Complexity:** O(1) - No extra arrays created!

---

## Comparison with Linear House Robber

| Aspect | Linear Street | Circular Street |
|--------|---------------|-----------------|
| **Constraint** | No first-last adjacency | First and last are adjacent |
| **Approach** | Single pass DP | Two passes with different arrays |
| **Scenarios** | One | Two (exclude first OR last) |
| **Time** | O(n) | O(n) |
| **Space** | O(1) | O(n) with slicing, O(1) without |
| **Complexity** | Simpler | Builds on linear solution |

---

## Key Takeaways

### Problem Pattern:
This is an extension of the **"Maximum Sum of Non-Adjacent Elements"** pattern with a circular constraint.

### Solution Strategy:
1. **Identify the constraint** - First and last are adjacent in circular arrangement
2. **Break into subproblems** - Two scenarios (exclude first OR exclude last)
3. **Reuse existing solution** - Apply linear house robber algorithm to each scenario
4. **Combine results** - Take maximum of both scenarios

### Core Insight:
```
Circular Problem → Two Linear Subproblems

Original: [a₀, a₁, a₂, ..., aₙ₋₁]  (circular)
           ↓
Subproblem 1: [a₀, a₁, a₂, ..., aₙ₋₂]  (exclude last)
Subproblem 2: [a₁, a₂, a₃, ..., aₙ₋₁]  (exclude first)
           ↓
Answer: max(solve(Subproblem 1), solve(Subproblem 2))
```

---

## Related Problems

### Similar Patterns:
- House Robber I (Linear street) - Base problem
- House Robber III (Binary tree) - Different structure
- Delete and Earn
- Maximum Sum Circular Subarray (different constraint)

### Key Difference:
- **Linear:** Direct application of include/exclude DP
- **Circular:** Break circle by considering two scenarios

---

## Complete Working Code

```python
def optimized(n, arr):
    """
    Find maximum sum of non-adjacent elements in a linear array
    """
    if n == 1:
        return arr[0]
    
    prev2 = arr[0]
    prev = max(arr[0], arr[1])
    
    for i in range(2, n):
        include = arr[i] + prev2
        exclude = prev
        current = max(include, exclude)
        prev2 = prev
        prev = current
    
    return prev

class Solution:
    def houseRobber(self, money):
        arr = money
        n = len(arr)
        
        # Edge case: Only one house
        if n == 1:
            return arr[0]
        
        # Scenario 1: Exclude last house (can include first)
        ans1 = optimized(n - 1, arr[:n-1])  # Houses 0 to n-2
        
        # Scenario 2: Exclude first house (can include last)
        ans2 = optimized(n - 1, arr[1:])    # Houses 1 to n-1
        
        # Return maximum of both scenarios
        return max(ans1, ans2)

# Driver Code
solution = Solution()

# Test Case 1
money1 = [2, 1, 4, 9]
print(f"Input: {money1}")
print(f"Output: {solution.houseRobber(money1)}")
print()

# Test Case 2
money2 = [1, 5, 2, 1, 6]
print(f"Input: {money2}")
print(f"Output: {solution.houseRobber(money2)}")
```

---


