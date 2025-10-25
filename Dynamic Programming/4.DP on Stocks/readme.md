
# Best Time to Buy and Sell Stock (One Transaction)

## 📋 Problem Statement

Given an array `arr` of `n` integers, where `arr[i]` represents the price of the stock on the `i-th` day.

**Objective:** Determine the maximum profit achievable by buying and selling the stock **at most once**.

**Constraints:**
- The stock must be purchased **before** selling it
- Both actions (buy and sell) cannot occur on the same day
- If no profit can be made, return 0

---

## 📊 Examples

### Example 1:
```
Input: arr = [10, 7, 5, 8, 11, 9]
Output: 6
Explanation: Buy on day 3 (price = 5) and sell on day 5 (price = 11)
             Profit = 11 - 5 = 6
```

### Example 2:
```
Input: arr = [5, 4, 3, 2, 1]
Output: 0
Explanation: Prices are continuously decreasing.
             No profitable transaction possible.
```

---

## 🎯 Problem Analysis

### Step 1: Define The Problem
- We have an array of size `n` with stock prices
- We need to buy a stock and then sell it
- **Profit = Selling Price - Buying Price**
- Goal: Find **maximum profit**
- Constraint: Can only sell **after** buying

### Step 2: Programmatic Representation
- Find two elements where:
  1. First element < Second element
  2. Difference between them is maximum
  3. Elements are in order (index of first < index of second)

### Strategy:
- Track the **minimum price** seen so far (best buying opportunity)
- For each price, calculate profit if we sell at current price
- Update maximum profit if current profit is better

---

## 🌳 Recursion Tree

This problem is **not typically solved using recursion** because it has a straightforward **greedy approach**. However, for educational purposes, here's how we could visualize the decision tree:

```
                    solve(arr, 0, minPrice=∞, maxProfit=0)
                                    |
            ┌───────────────────────┴───────────────────────┐
            |                                               |
     Buy at day 0?                                   Skip day 0?
     minPrice = arr[0]                               Continue
            |                                               |
    ┌───────┴───────┐                              ┌────────┴────────┐
    |               |                              |                 |
Sell at day 1?  Continue...                   Buy at day 1?     Continue...
profit = arr[1]-arr[0]                        minPrice = arr[1]
    |               |                              |                 |
   ...             ...                            ...               ...
```

**Note:** The greedy solution is optimal here because we only need one pass through the array.

---

## 🔄 Approach Evolution

### ❌ Why Recursion/DP is NOT needed here:

This is a **greedy problem**, not a dynamic programming problem because:
- We don't have overlapping subproblems
- We don't need to explore multiple states
- A single pass with tracking minimum price gives optimal solution

### ✅ The Greedy Approach (Optimal):

**Recurrence Relation (Conceptual):**
```
maxProfit[i] = max(maxProfit[i-1], arr[i] - minPrice[i-1])
minPrice[i] = min(minPrice[i-1], arr[i])
```

Where:
- `maxProfit[i]` = Maximum profit achievable till day `i`
- `minPrice[i]` = Minimum price seen till day `i`

---

## 💻 Solution Code

```python
def solve(arr):
    """
    Find maximum profit by buying and selling stock once.
    
    Args:
        arr: List of stock prices where arr[i] is price on day i
    
    Returns:
        Maximum profit achievable
    
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    n = len(arr)
    
    # Initialize maximum profit to 0 (no transaction)
    max_profit = 0
    
    # Initialize minimum price with first day's price
    mini_price = arr[0]
    
    # Traverse from day 2 to last day
    for i in range(1, n):
        current_price = arr[i]
        
        # Calculate profit if we sell at current price
        # (having bought at the minimum price seen so far)
        profit = current_price - mini_price
        
        # Update maximum profit if current profit is better
        max_profit = max(max_profit, profit)
        
        # Update minimum price for future calculations
        # This represents the best buying opportunity so far
        mini_price = min(mini_price, current_price)
    
    return max_profit


# Test the solution
arr = [10, 7, 5, 8, 11, 9]
print(solve(arr))  # Output: 6
```

---

## 🔍 Step-by-Step Execution

For `arr = [10, 7, 5, 8, 11, 9]`:

| Day | Price | Mini Price (Before) | Profit | Max Profit | Mini Price (After) |
|-----|-------|---------------------|--------|------------|--------------------|
| 0   | 10    | -                   | -      | 0          | 10                 |
| 1   | 7     | 10                  | -3     | 0          | 7                  |
| 2   | 5     | 7                   | -2     | 0          | 5                  |
| 3   | 8     | 5                   | 3      | 3          | 5                  |
| 4   | 11    | 5                   | 6      | **6**      | 5                  |
| 5   | 9     | 5                   | 4      | 6          | 5                  |

**Result:** Maximum Profit = **6** (Buy at 5, Sell at 11)

---

## ⏱️ Complexity Analysis

### Time Complexity: **O(n)**
- Single traversal through the array
- Each element is visited exactly once
- Constant time operations (comparison, arithmetic) per element

### Space Complexity: **O(1)**
- Only using two variables: `max_profit` and `mini_price`
- No additional data structures used
- Space usage doesn't depend on input size

---

# Best Time to Buy and Sell Stock II (Unlimited Transactions)

## 📋 Problem Statement

Given an array `arr` of `n` integers, where `arr[i]` represents the price of the stock on the `i-th` day.

**Objective:** Determine the maximum profit achievable by buying and selling the stock **any number of times**.

**Constraints:**
- Hold at most **one share** of the stock at any given time
- Stock must be **sold before buying again**
- Buying and selling on the **same day is permitted**

---

## 📊 Examples

### Example 1:
```
Input: arr = [9, 2, 6, 4, 7, 3]
Output: 7
Explanation: 
- Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6 - 2 = 4
- Buy on day 4 (price = 4) and sell on day 5 (price = 7), profit = 7 - 4 = 3
- Total profit = 4 + 3 = 7
```

### Example 2:
```
Input: arr = [2, 3, 4, 5, 6]
Output: 4
Explanation: 
- Buy on day 1 (price = 2) and sell on day 5 (price = 6), profit = 6 - 2 = 4
- Total profit = 4
```

---

## 🎯 Problem Analysis

### Step 1: Define The Problem
- Array represents stock prices over time
- Can buy and sell **any number of times**
- Must buy before selling
- **Profit = Selling Price - Buying Price**
- Goal: Find **total maximum profit**

### Step 2: Represent the Problem Programmatically

**Function Definition:**
```
f(index, buy) → maximum profit from index to n-1
```

**Parameters:**
- `index`: Current day (0 to n-1)
- `buy`: Can we buy or sell?
  - `buy = 0`: Can only **BUY** stock (we don't currently hold stock)
  - `buy = 1`: Can only **SELL** stock (we currently hold stock)

**State Space:** 
- Days: `0` to `n-1`
- Buy status: `0` or `1`
- Total states: `n × 2`

---

## 🌳 Recursion Tree

For `arr = [9, 2, 6]`, starting at `index=0, buy=0`:

```
                            f(0, buy=0)
                            [Can BUY]
                    ┌───────────┴───────────┐
                    │                       │
            BUY at day 0               DON'T BUY at day 0
            profit: -9                 profit: 0
            f(1, buy=1)                f(1, buy=0)
            [Can SELL]                 [Can BUY]
         ┌──────┴──────┐           ┌──────┴──────┐
         │             │           │             │
    SELL at 1    Don't SELL   BUY at 1     Don't BUY
    profit: +2      0         profit: -2       0
    f(2, 0)      f(2, 1)      f(2, 1)       f(2, 0)
         │           │            │             │
        ...         ...          ...           ...
```

**Key Observations:**
- At each state, we have 2 choices (include or exclude)
- `buy=0` means we can BUY (buying decreases profit: `-arr[i]`)
- `buy=1` means we can SELL (selling increases profit: `+arr[i]`)
- After buying, state changes to `buy=1` (can sell)
- After selling, state changes to `buy=0` (can buy)

---

## 📐 Recurrence Relation

### Base Case:
```
if index == n:
    return 0  # No more days, no profit
```

### Recurrence:

**Case 1: When `buy = 0` (Can BUY stock)**
```
include = -arr[index] + f(index+1, 1)   # Buy stock, reduce profit, now can sell
exclude = f(index+1, 0)                 # Skip buying, move to next day
result = max(include, exclude)
```

**Case 2: When `buy = 1` (Can SELL stock)**
```
include = arr[index] + f(index+1, 0)    # Sell stock, increase profit, now can buy
exclude = f(index+1, 1)                 # Skip selling, move to next day
result = max(include, exclude)
```

**Complete Relation:**
```python
if buy == 0:
    return max(-arr[index] + f(index+1, 1), f(index+1, 0))
else:
    return max(arr[index] + f(index+1, 0), f(index+1, 1))
```

---

## 🔄 Solution Evolution

### Step 3: Base Cases
```python
if day == n:
    return 0  # No more stocks available, profit = 0
```

### Step 4: Finding The Recurrence Relation

**When `buy = 0` (Can BUY):**
- **Include:** Buy at current price → `-arr[day] + f(day+1, 1)`
- **Exclude:** Don't buy, try next day → `f(day+1, 0)`

**When `buy = 1` (Can SELL):**
- **Include:** Sell at current price → `arr[day] + f(day+1, 0)`
- **Exclude:** Don't sell, try next day → `f(day+1, 1)`

Return maximum of both choices.

---

## 💻 Solution Approaches

### 1️⃣ Recursive Solution

```python
def solve(n, arr, day, buy):
    """
    Recursive solution to find maximum profit.
    
    Args:
        n: Total number of days
        arr: Stock prices array
        day: Current day index
        buy: 0 (can buy) or 1 (can sell)
    
    Returns:
        Maximum profit from current day to end
    """
    # Base case: No more days left
    if day == n:
        return 0
    
    # If buy=0, we can BUY stock
    if buy == 0:
        include = -arr[day] + solve(n, arr, day+1, 1)  # Buy stock
        exclude = solve(n, arr, day+1, 0)              # Don't buy
    # If buy=1, we can SELL stock
    else:
        include = arr[day] + solve(n, arr, day+1, 0)   # Sell stock
        exclude = solve(n, arr, day+1, 1)              # Don't sell
    
    # Return maximum profit
    return max(include, exclude)
```

**Time Complexity:** O(2^n)  
**Space Complexity:** O(n) - Recursion stack depth

---

### 2️⃣ Memoization Solution

**Conversion Steps (Recursion → Memoization):**
1. Identify changing parameters: `day` and `buy`
2. Create DP table: `memo[n+1][2]` (day ranges 0 to n, buy is 0 or 1)
3. Initialize with `-1` to mark uncomputed states
4. Before computing, check if already computed
5. Store result before returning

```python
def solve_memo(n, arr, day, buy, memo):
    """
    Memoization solution to find maximum profit.
    
    Args:
        n: Total number of days
        arr: Stock prices array
        day: Current day index
        buy: 0 (can buy) or 1 (can sell)
        memo: DP table to store computed results
    
    Returns:
        Maximum profit from current day to end
    """
    # Check if already computed
    if memo[day][buy] != -1:
        return memo[day][buy]
    
    # Base case: No more days left
    if day == n:
        return 0
    
    # If buy=0, we can BUY stock
    if buy == 0:
        include = -arr[day] + solve_memo(n, arr, day+1, 1, memo)
        exclude = solve_memo(n, arr, day+1, 0, memo)
    # If buy=1, we can SELL stock
    else:
        include = arr[day] + solve_memo(n, arr, day+1, 0, memo)
        exclude = solve_memo(n, arr, day+1, 1, memo)
    
    # Store and return result
    memo[day][buy] = max(include, exclude)
    return memo[day][buy]
```

**Time Complexity:** O(n × 2) = O(n)  
**Space Complexity:** O(n) + O(n × 2) = O(n) - Stack + DP table

---

### 3️⃣ Tabulation Solution

**Conversion Steps (Memoization → Tabulation):**
1. Create DP table: `dp[n+1][2]`
2. Initialize base case: `dp[n][0] = 0, dp[n][1] = 0`
3. Convert recursion direction: Fill from `n-1` to `0` (bottom-up)
4. Replace recursive calls with DP table lookups
5. Return `dp[0][0]` (start at day 0 with buy option)

```python
def tabulation(n, arr):
    """
    Tabulation (bottom-up DP) solution to find maximum profit.
    
    Args:
        n: Total number of days
        arr: Stock prices array
    
    Returns:
        Maximum profit from all transactions
    """
    # Create DP table: dp[day][buy]
    dp = [[-1]*2 for _ in range(n+1)]
    
    # Base case: At day n, no profit possible
    dp[n][0] = 0
    dp[n][1] = 0
    
    # Fill table from day n-1 to 0 (bottom-up)
    for day in range(n-1, -1, -1):
        for buy in range(2):
            # If buy=0, we can BUY stock
            if buy == 0:
                include = -arr[day] + dp[day+1][1]  # Buy stock
                exclude = dp[day+1][0]              # Don't buy
            # If buy=1, we can SELL stock
            else:
                include = arr[day] + dp[day+1][0]   # Sell stock
                exclude = dp[day+1][1]              # Don't sell
            
            # Store maximum profit for current state
            dp[day][buy] = max(include, exclude)
    
    # Return result: start at day 0 with option to buy
    return dp[0][0]
```

**Time Complexity:** O(n × 2) = O(n)  
**Space Complexity:** O(n × 2) = O(n)

---

### 4️⃣ Space Optimized Solution

**Conversion Steps (Tabulation → Space Optimized):**
1. Observe dependency: `dp[day]` only depends on `dp[day+1]`
2. Replace 2D array with two 1D arrays: `dp_next[2]` and `dp_curr[2]`
3. Use `dp_next` for next day's values
4. Use `dp_curr` to compute current day's values
5. After each iteration, copy `dp_curr` to `dp_next`
6. Return `dp_next[0]`

```python
def optimized(n, arr):
    """
    Space optimized solution to find maximum profit.
    
    Args:
        n: Total number of days
        arr: Stock prices array
    
    Returns:
        Maximum profit from all transactions
    """
    # Use two arrays instead of 2D table
    dp_next = [-1] * 2  # Stores next day's values
    dp_curr = [-1] * 2  # Stores current day's values
    
    # Base case: At day n, no profit possible
    dp_next[0] = 0
    dp_next[1] = 0
    
    # Fill from day n-1 to 0
    for day in range(n-1, -1, -1):
        for buy in range(2):
            # If buy=0, we can BUY stock
            if buy == 0:
                include = -arr[day] + dp_next[1]  # Buy stock
                exclude = dp_next[0]              # Don't buy
            # If buy=1, we can SELL stock
            else:
                include = arr[day] + dp_next[0]   # Sell stock
                exclude = dp_next[1]              # Don't sell
            
            # Store maximum profit for current state
            dp_curr[buy] = max(include, exclude)
        
        # Move current to next for next iteration
        dp_next = dp_curr.copy()
    
    # Return result: start at day 0 with option to buy
    return dp_next[0]
```

**Time Complexity:** O(n × 2) = O(n)  
**Space Complexity:** O(2) = O(1)

---

## 🧪 Test Code

```python
arr = [9, 2, 6, 4, 7, 3]
n = len(arr)

# Recursive Solution
print(solve(n, arr, 0, 0))  # Output: 7

# Memoization Solution
memo = [[-1]*2 for _ in range(n+1)]
print(solve_memo(n, arr, 0, 0, memo))  # Output: 7

# Tabulation Solution
print(tabulation(n, arr))  # Output: 7

# Space Optimized Solution
print(optimized(n, arr))  # Output: 7
```

---

## 🔍 Step-by-Step Execution

For `arr = [9, 2, 6, 4, 7, 3]`:

### Tabulation DP Table:

| Day | buy=0 (Can BUY) | buy=1 (Can SELL) |
|-----|-----------------|------------------|
| 6   | 0               | 0                |
| 5   | 0               | 3                |
| 4   | 3               | 3                |
| 3   | 3               | 2                |
| 2   | 4               | 2                |
| 1   | 7               | 6                |
| 0   | **7**           | 6                |

**Final Answer:** `dp[0][0] = 7`

---

## ⏱️ Complexity Analysis

### 1. Recursive Solution
- **Time Complexity:** O(2^n)
  - Each state has 2 choices (include/exclude)
  - Total recursive calls ≈ 2^n
- **Space Complexity:** O(n)
  - Recursion stack depth = n (maximum call depth)

### 2. Memoization Solution
- **Time Complexity:** O(n × 2) = **O(n)**
  - Total unique states = n days × 2 buy options = 2n
  - Each state computed once
- **Space Complexity:** O(n) + O(n × 2) = **O(n)**
  - Recursion stack: O(n)
  - DP table: O(n × 2) = O(n)

### 3. Tabulation Solution
- **Time Complexity:** O(n × 2) = **O(n)**
  - Two nested loops: n iterations × 2 iterations
- **Space Complexity:** O(n × 2) = **O(n)**
  - DP table of size (n+1) × 2

### 4. Space Optimized Solution
- **Time Complexity:** O(n × 2) = **O(n)**
  - Same as tabulation
- **Space Complexity:** O(2) = **O(1)**
  - Only two arrays of size 2
  - No recursion stack

---

## 📊 Complexity Comparison Table

| Approach          | Time Complexity | Space Complexity | Notes                          |
|-------------------|-----------------|------------------|--------------------------------|
| Recursion         | O(2^n)          | O(n)             | Exponential, very slow         |
| Memoization       | O(n)            | O(n)             | Top-down DP, uses recursion    |
| Tabulation        | O(n)            | O(n)             | Bottom-up DP, iterative        |
| Space Optimized   | O(n)            | O(1)             | Most efficient, constant space |

---

# Best Time to Buy and Sell Stock III & IV (At Most K Transactions)

## 📋 Problem Statement

Given an array `arr` of `n` integers, where `arr[i]` represents the price of the stock on the `i-th` day.

**Objective:** Determine the maximum profit achievable by completing **at most k transactions** in total.

**Constraints:**
- Hold at most **one share** of the stock at any given time
- Stock must be **sold before buying again**
- Buying and selling on the **same day is permitted**
- **Note:** One transaction = One buy + One sell

---

## 📊 Examples

### Example 1: k = 2
```
Input: arr = [3, 2, 6, 5, 0, 3], k = 2
Output: 7
Explanation: 
- Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6 - 2 = 4
- Buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3 - 0 = 3
- Total profit = 4 + 3 = 7
```

### Example 2: k = 3
```
Input: arr = [1, 2, 4, 2, 5, 7, 2, 4, 9, 0], k = 3
Output: 15
Explanation: 
- Buy on day 1 (price = 1) and sell on day 3 (price = 4), profit = 4 - 1 = 3
- Buy on day 4 (price = 2) and sell on day 6 (price = 7), profit = 7 - 2 = 5
- Buy on day 7 (price = 2) and sell on day 9 (price = 9), profit = 9 - 2 = 7
- Total profit = 3 + 5 + 7 = 15
```

---

## 🎯 Problem Analysis

### Step 1: Define The Problem
- Array represents stock prices over time
- Can buy and sell **at most k times**
- Must buy before selling
- **Profit = Selling Price - Buying Price**
- Goal: Find **total maximum profit**

### Step 2: Represent the Problem Programmatically

**Function Definition:**
```
f(index, buy, cap) → maximum profit from index to n-1 with cap transactions left
```

**Parameters:**
- `index`: Current day (0 to n-1)
- `buy`: Can we buy or sell?
  - `buy = 0`: Can only **BUY** stock (we don't currently hold stock)
  - `buy = 1`: Can only **SELL** stock (we currently hold stock)
- `cap`: Number of transactions **remaining** (we can still sell `cap` times)

**State Space:** 
- Days: `0` to `n-1`
- Buy status: `0` or `1`
- Capacity: `0` to `k`
- Total states: `n × 2 × (k+1)`

---

## 🌳 Recursion Tree

For `arr = [3, 2, 6]`, `k = 2`, starting at `index=0, buy=0, cap=2`:

```
                            f(0, buy=0, cap=2)
                            [Can BUY, 2 trans left]
                    ┌───────────┴───────────┐
                    │                       │
            BUY at day 0               DON'T BUY at day 0
            profit: -3                 profit: 0
            f(1, 1, 2)                 f(1, 0, 2)
            [Can SELL, 2 left]         [Can BUY, 2 left]
         ┌──────┴──────┐           ┌──────┴──────┐
         │             │           │             │
    SELL at 1    Don't SELL   BUY at 1     Don't BUY
    profit: +2   profit: 0    profit: -2   profit: 0
    f(2,0,1)     f(2,1,2)     f(2,1,2)     f(2,0,2)
    [1 trans]    [2 trans]    [2 trans]    [2 trans]
         │           │            │             │
        ...         ...          ...           ...
```

**Key Observations:**
- At each state, we have 2 choices (include or exclude)
- `buy=0` means we can BUY (buying decreases profit: `-arr[i]`)
- `buy=1` means we can SELL (selling increases profit: `+arr[i]`)
- **Capacity decreases only when we SELL** (complete a transaction)
- After buying, state changes to `buy=1` (can sell)
- After selling, state changes to `buy=0` AND `cap=cap-1`

---

## 📐 Recurrence Relation

### Step 3: Base Cases
```
if index == n:
    return 0  # No more days, no profit

if cap == 0:
    return 0  # No transactions left, no profit
```

### Step 4: Finding The Recurrence Relation

**Case 1: When `buy = 0` (Can BUY stock)**
```
include = -arr[index] + f(index+1, 1, cap)   # Buy stock, reduce profit, now can sell
exclude = f(index+1, 0, cap)                 # Skip buying, move to next day
result = max(include, exclude)
```

**Case 2: When `buy = 1` (Can SELL stock)**
```
include = arr[index] + f(index+1, 0, cap-1)  # Sell stock, increase profit, reduce cap
exclude = f(index+1, 1, cap)                 # Skip selling, move to next day
result = max(include, exclude)
```

**Complete Relation:**
```python
if buy == 0:
    return max(-arr[index] + f(index+1, 1, cap), f(index+1, 0, cap))
else:
    return max(arr[index] + f(index+1, 0, cap-1), f(index+1, 1, cap))
```

**Key Difference from Stock II:**
- We track `cap` (capacity/transactions remaining)
- `cap` decreases by 1 when we **sell** (complete a transaction)
- Base case includes `cap == 0` (no more transactions allowed)

---

## 💻 Solution Approaches

### 1️⃣ Recursive Solution

```python
def solve(n, arr, day, buy, cap):
    """
    Recursive solution to find maximum profit with at most k transactions.
    
    Args:
        n: Total number of days
        arr: Stock prices array
        day: Current day index
        buy: 0 (can buy) or 1 (can sell)
        cap: Number of transactions remaining
    
    Returns:
        Maximum profit from current day to end
    """
    # Base case: No more days or no transactions left
    if day == n or cap == 0:
        return 0
    
    # If buy=0, we can BUY stock
    if buy == 0:
        include = -arr[day] + solve(n, arr, day+1, 1, cap)  # Buy stock
        exclude = solve(n, arr, day+1, 0, cap)              # Don't buy
    # If buy=1, we can SELL stock
    else:
        include = arr[day] + solve(n, arr, day+1, 0, cap-1) # Sell stock, reduce cap
        exclude = solve(n, arr, day+1, 1, cap)              # Don't sell
    
    # Return maximum profit
    return max(include, exclude)
```

**Time Complexity:** O(2^n)  
**Space Complexity:** O(n) - Recursion stack depth

---

### 2️⃣ Memoization Solution

**Conversion Steps (Recursion → Memoization):**
1. Identify changing parameters: `day`, `buy`, and `cap`
2. Create 3D DP table: `memo[n+1][2][k+1]`
3. Initialize with `-1` to mark uncomputed states
4. Before computing, check if already computed
5. Store result before returning

```python
def solve_memo(n, arr, day, buy, cap, memo):
    """
    Memoization solution to find maximum profit with at most k transactions.
    
    Args:
        n: Total number of days
        arr: Stock prices array
        day: Current day index
        buy: 0 (can buy) or 1 (can sell)
        cap: Number of transactions remaining
        memo: 3D DP table to store computed results
    
    Returns:
        Maximum profit from current day to end
    """
    # Check if already computed
    if memo[day][buy][cap] != -1:
        return memo[day][buy][cap]
    
    # Base case: No more days or no transactions left
    if day == n or cap == 0:
        return 0
    
    # If buy=0, we can BUY stock
    if buy == 0:
        include = -arr[day] + solve_memo(n, arr, day+1, 1, cap, memo)
        exclude = solve_memo(n, arr, day+1, 0, cap, memo)
    # If buy=1, we can SELL stock
    else:
        include = arr[day] + solve_memo(n, arr, day+1, 0, cap-1, memo)
        exclude = solve_memo(n, arr, day+1, 1, cap, memo)
    
    # Store and return result
    memo[day][buy][cap] = max(include, exclude)
    return memo[day][buy][cap]
```

**Time Complexity:** O(n × 2 × k) = O(n × k)  
**Space Complexity:** O(n) + O(n × 2 × k) = O(n × k) - Stack + DP table

---

### 3️⃣ Tabulation Solution

**Conversion Steps (Memoization → Tabulation):**
1. Create 3D DP table: `dp[n+1][2][k+1]`
2. Initialize base cases:
   - `dp[n][0][cap] = 0` and `dp[n][1][cap] = 0` for all cap
   - `dp[day][0][0] = 0` and `dp[day][1][0] = 0` for all day
3. Convert recursion direction: Fill from `n-1` to `0` (bottom-up)
4. Replace recursive calls with DP table lookups
5. Return `dp[0][0][k]` (start at day 0, can buy, k transactions left)

```python
def tabulation(n, arr, k):
    """
    Tabulation (bottom-up DP) solution to find maximum profit.
    
    Args:
        n: Total number of days
        arr: Stock prices array
        k: Maximum number of transactions allowed
    
    Returns:
        Maximum profit from all transactions
    """
    # Edge cases
    if n == 1 or k == 0:
        return 0
    
    # Create 3D DP table: dp[day][buy][cap]
    dp = [[[-1]*(k+1) for buy in range(2)] for day in range(n+1)]
    
    # Base case 1: At day n, no profit possible for any capacity
    for cap in range(k+1):
        dp[n][0][cap] = 0
        dp[n][1][cap] = 0
    
    # Base case 2: With 0 capacity, no profit possible for any day
    for day in range(1, n+1):
        dp[day][0][0] = 0
        dp[day][1][0] = 0
    
    # Fill table from day n-1 to 0 (bottom-up)
    for day in range(n-1, -1, -1):
        for buy in range(2):
            for cap in range(1, k+1):  # Start from 1 as cap=0 is base case
                # If buy=0, we can BUY stock
                if buy == 0:
                    include = -arr[day] + dp[day+1][1][cap]  # Buy stock
                    exclude = dp[day+1][0][cap]              # Don't buy
                # If buy=1, we can SELL stock
                else:
                    include = arr[day] + dp[day+1][0][cap-1] # Sell, reduce cap
                    exclude = dp[day+1][1][cap]              # Don't sell
                
                # Store maximum profit for current state
                dp[day][buy][cap] = max(include, exclude)
    
    # Return result: start at day 0, can buy, k transactions left
    return dp[0][0][k]
```

**Time Complexity:** O(n × 2 × k) = O(n × k)  
**Space Complexity:** O(n × 2 × k) = O(n × k)

---

### 4️⃣ Space Optimized Solution

**Conversion Steps (Tabulation → Space Optimized):**
1. Observe dependency: `dp[day]` only depends on `dp[day+1]`
2. Replace 3D array with two 2D arrays: `dp_next[2][k+1]` and `dp_curr[2][k+1]`
3. Use `dp_next` for next day's values
4. Use `dp_curr` to compute current day's values
5. After each iteration, copy `dp_curr` to `dp_next`
6. Return `dp_next[0][k]`

```python
def optimized(n, arr, k):
    """
    Space optimized solution to find maximum profit.
    
    Args:
        n: Total number of days
        arr: Stock prices array
        k: Maximum number of transactions allowed
    
    Returns:
        Maximum profit from all transactions
    """
    # Edge cases
    if n == 1 or k == 0:
        return 0
    
    # Use two 2D arrays instead of 3D table
    dp_next = [[-1]*(k+1) for buy in range(2)]  # Stores next day's values
    dp_curr = [[-1]*(k+1) for buy in range(2)]  # Stores current day's values
    
    # Base case 1: At day n, no profit possible for any capacity
    for cap in range(k+1):
        dp_next[0][cap] = 0
        dp_next[1][cap] = 0
    
    # Base case 2: With 0 capacity, no profit possible
    dp_curr[0][0] = 0
    dp_curr[1][0] = 0
    
    # Fill from day n-1 to 0
    for day in range(n-1, -1, -1):
        for buy in range(2):
            for cap in range(1, k+1):  # Start from 1 as cap=0 is base case
                # If buy=0, we can BUY stock
                if buy == 0:
                    include = -arr[day] + dp_next[1][cap]    # Buy stock
                    exclude = dp_next[0][cap]                # Don't buy
                # If buy=1, we can SELL stock
                else:
                    include = arr[day] + dp_next[0][cap-1]   # Sell, reduce cap
                    exclude = dp_next[1][cap]                # Don't sell
                
                # Store maximum profit for current state
                dp_curr[buy][cap] = max(include, exclude)
        
        # Move current to next for next iteration
        dp_next = dp_curr.copy()
    
    # Return result: start at day 0, can buy, k transactions left
    return dp_next[0][k]
```

**Time Complexity:** O(n × 2 × k) = O(n × k)  
**Space Complexity:** O(2 × k) = O(k)

---

## 🧪 Test Code

```python
# Test Case 1: k = 2
arr = [3, 2, 6, 5, 0, 3]
n = len(arr)
k = 2

# Recursive Solution
print(solve(n, arr, 0, 0, k))  # Output: 7

# Memoization Solution
memo = [[[-1]*(k+1) for buy in range(2)] for day in range(n+1)]
print(solve_memo(n, arr, 0, 0, k, memo))  # Output: 7

# Tabulation Solution
print(tabulation(n, arr, k))  # Output: 7

# Space Optimized Solution
print(optimized(n, arr, k))  # Output: 7
```

---

## 🔍 Step-by-Step Execution

For `arr = [3, 2, 6, 5, 0, 3]`, `k = 2`:

### Partial DP Table (day=0, all states):

| Day | buy | cap | Value | Explanation                              |
|-----|-----|-----|-------|------------------------------------------|
| 0   | 0   | 0   | 0     | No transactions left                     |
| 0   | 0   | 1   | 3     | Max profit with 1 transaction remaining |
| 0   | 0   | 2   | **7** | Max profit with 2 transactions remaining |
| 0   | 1   | 0   | 0     | No transactions left                     |
| 0   | 1   | 1   | 4     | Can sell once more                       |
| 0   | 1   | 2   | 7     | Can sell twice more                      |

**Final Answer:** `dp[0][0][2] = 7`

**Transaction Breakdown:**
- Transaction 1: Buy at 2, Sell at 6 → Profit = 4
- Transaction 2: Buy at 0, Sell at 3 → Profit = 3
- Total = 7

---

## ⏱️ Complexity Analysis

### 1. Recursive Solution
- **Time Complexity:** O(2^n)
  - Each state has 2 choices (include/exclude)
  - Without memoization, exponential time
- **Space Complexity:** O(n)
  - Recursion stack depth = n (maximum call depth)

### 2. Memoization Solution
- **Time Complexity:** O(n × 2 × k) = **O(n × k)**
  - Total unique states = n days × 2 buy options × k capacities
  - Each state computed once
- **Space Complexity:** O(n) + O(n × 2 × k) = **O(n × k)**
  - Recursion stack: O(n)
  - 3D DP table: O(n × 2 × k) = O(n × k)

### 3. Tabulation Solution
- **Time Complexity:** O(n × 2 × k) = **O(n × k)**
  - Three nested loops: n iterations × 2 iterations × k iterations
- **Space Complexity:** O(n × 2 × k) = **O(n × k)**
  - 3D DP table of size (n+1) × 2 × (k+1)

### 4. Space Optimized Solution
- **Time Complexity:** O(n × 2 × k) = **O(n × k)**
  - Same as tabulation
- **Space Complexity:** O(2 × k) = **O(k)**
  - Two 2D arrays of size 2 × (k+1)
  - No recursion stack

---

## 📊 Complexity Comparison Table

| Approach          | Time Complexity | Space Complexity | Notes                             |
|-------------------|-----------------|------------------|-----------------------------------|
| Recursion         | O(2^n)          | O(n)             | Exponential, very slow            |
| Memoization       | O(n × k)        | O(n × k)         | Top-down DP, uses recursion       |
| Tabulation        | O(n × k)        | O(n × k)         | Bottom-up DP, iterative           |
| Space Optimized   | O(n × k)        | O(k)             | Most space efficient              |

---

## 🔄 Conversion Summary

### Recursion → Memoization
- Add 3D memo table `[n+1][2][k+1]`
- Check memo before computation
- Store result in memo before returning

### Memoization → Tabulation
- Convert top-down to bottom-up
- Initialize base cases explicitly
- Loop from `n-1` to `0`
- Replace recursive calls with table lookups

### Tabulation → Space Optimized
- Observe that `dp[day]` only depends on `dp[day+1]`
- Use two 2D arrays instead of 3D table
- `dp_next` stores next day values
- `dp_curr` computes current day values
- Copy `dp_curr` to `dp_next` after each day

---
