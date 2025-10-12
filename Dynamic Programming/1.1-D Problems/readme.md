
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
