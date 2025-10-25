# Stock Trading Problems - Revision Notes

## 📑 Index

1. [Best Time to Buy and Sell Stock (One Transaction)](#1-best-time-to-buy-and-sell-stock-one-transaction)
2. [Best Time to Buy and Sell Stock II (Unlimited Transactions)](#2-best-time-to-buy-and-sell-stock-ii-unlimited-transactions)
3. [Best Time to Buy and Sell Stock III & IV (At Most K Transactions)](#3-best-time-to-buy-and-sell-stock-iii--iv-at-most-k-transactions)
4. [Best Time to Buy and Sell Stock with Transaction Fee](#4-best-time-to-buy-and-sell-stock-with-transaction-fee)
5. [Buy and Sell Stocks with Cooldown](#5-buy-and-sell-stocks-with-cooldown)
6. [Problem Comparison Table](#-problem-comparison-table)

---

## 1. Best Time to Buy and Sell Stock (One Transaction)

### 📝 Problem Description
Find maximum profit by buying and selling stock **at most once**. Must buy before selling, and both actions cannot occur on the same day.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [10, 7, 5, 8, 11, 9]
Output: 6
Explanation: Buy on day 3 (price = 5) and sell on day 5 (price = 11), profit = 11 - 5 = 6
```

**Example 2:**
```
Input: arr = [5, 4, 3, 2, 1]
Output: 0
Explanation: No transactions made, profit = 0
```

### 💡 Approach: Single Pass (Greedy)

**Intuition:**
- Track the minimum price seen so far
- For each day, calculate profit if we sell today
- Keep track of maximum profit

**Code:**
```python
def solve(arr):
    n = len(arr)
    max_profit = 0
    mini_price = arr[0]  # Track minimum price seen so far
    
    for i in range(1, n):
        current_price = arr[i]
        # Calculate profit if we sell today
        profit = current_price - mini_price
        # Update maximum profit found
        max_profit = max(max_profit, profit)
        # Update minimum price for future transactions
        mini_price = min(mini_price, current_price)
    
    return max_profit
```

**Complexity:**
- **Time:** O(n) - Single pass through array
- **Space:** O(1) - Only variables used

---

## 2. Best Time to Buy and Sell Stock II (Unlimited Transactions)

### 📝 Problem Description
Find maximum profit with **unlimited transactions**. Can only hold at most one share at a time. Must sell before buying again. Can buy and sell on the same day.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [9, 2, 6, 4, 7, 3]
Output: 7
Explanation: Buy at 2, sell at 6 (profit = 4). Buy at 4, sell at 7 (profit = 3). Total = 7
```

**Example 2:**
```
Input: arr = [2, 3, 4, 5, 6]
Output: 4
Explanation: Buy at 2, sell at 6, profit = 4
```

### 💡 Approach 1: Recursion

**Intuition:**
- At each day, we have 2 states: can buy (buy=0) or can sell (buy=1)
- If can buy: choose to buy (pay price, move to sell state) or skip
- If can sell: choose to sell (get price, move to buy state) or skip

**Code:**
```python
def solve(n, arr, day, buy):
    # Base case: no more days
    if day == n:
        return 0
    
    if buy == 0:  # Can buy
        # Option 1: Buy today (pay arr[day], move to sell state)
        include = -arr[day] + solve(n, arr, day+1, 1)
        # Option 2: Skip buying today
        exclude = solve(n, arr, day+1, 0)
    else:  # Can sell
        # Option 1: Sell today (get arr[day], move to buy state)
        include = arr[day] + solve(n, arr, day+1, 0)
        # Option 2: Skip selling today
        exclude = solve(n, arr, day+1, 1)
    
    # Return maximum of both options
    return max(include, exclude)
```

**Complexity:**
- **Time:** O(2^n) - Exponential
- **Space:** O(n) - Recursion stack

### 💡 Approach 2: Memoization

**Intuition:** Cache results for (day, buy) states to avoid recomputation

**Code:**
```python
def solve_memo(n, arr, day, buy, memo):
    # Check if already computed
    if memo[day][buy] != -1:
        return memo[day][buy]
    
    # Base case
    if day == n:
        return 0
    
    if buy == 0:
        # Buy or skip
        include = -arr[day] + solve_memo(n, arr, day+1, 1, memo)
        exclude = solve_memo(n, arr, day+1, 0, memo)
    else:
        # Sell or skip
        include = arr[day] + solve_memo(n, arr, day+1, 0, memo)
        exclude = solve_memo(n, arr, day+1, 1, memo)
    
    # Store result in memo before returning
    memo[day][buy] = max(include, exclude)
    return memo[day][buy]
```

**Complexity:**
- **Time:** O(n × 2) = O(n)
- **Space:** O(n × 2) = O(n)

### 💡 Approach 3: Tabulation (Bottom-Up DP)

**Intuition:** Build solution from base case (day n) to day 0

**Code:**
```python
def tabulation(n, arr):
    # Create DP table: dp[day][buy]
    dp = [[-1]*2 for _ in range(n+1)]
    # Base case: day n, no profit possible
    dp[n][0] = 0
    dp[n][1] = 0
    
    # Fill table from bottom to top (day n-1 to 0)
    for day in range(n-1, -1, -1):
        for buy in range(2):
            if buy == 0:  # Can buy
                include = -arr[day] + dp[day+1][1]
                exclude = dp[day+1][0]
            else:  # Can sell
                include = arr[day] + dp[day+1][0]
                exclude = dp[day+1][1]
            # Store maximum profit for this state
            dp[day][buy] = max(include, exclude)
    
    # Answer: start at day 0 in buy state
    return dp[0][0]
```

**Complexity:**
- **Time:** O(n × 2) = O(n)
- **Space:** O(n × 2) = O(n)

### 💡 Approach 4: Space Optimized

**Intuition:** Only need current and next day states

**Code:**
```python
def optimized(n, arr):
    # Only need next day's values
    dp_next = [-1]*2
    dp_curr = [-1]*2
    # Base case
    dp_next[0] = 0
    dp_next[1] = 0
    
    for day in range(n-1, -1, -1):
        for buy in range(2):
            if buy == 0:
                include = -arr[day] + dp_next[1]
                exclude = dp_next[0]
            else:
                include = arr[day] + dp_next[0]
                exclude = dp_next[1]
            dp_curr[buy] = max(include, exclude)
        # Move current to next for next iteration
        dp_next = dp_curr.copy()
    
    return dp_next[0]
```

**Complexity:**
- **Time:** O(n)
- **Space:** O(1) - Only two arrays of size 2

---

## 3. Best Time to Buy and Sell Stock III & IV (At Most K Transactions)

### 📝 Problem Description
Find maximum profit with **at most K transactions**. Can only hold one share at a time. Must sell before buying again. Can buy and sell on the same day.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [3, 2, 6, 5, 0, 3], k = 2
Output: 7
Explanation: Buy at 2, sell at 6 (profit = 4). Buy at 0, sell at 3 (profit = 3). Total = 7
```

**Example 2:**
```
Input: arr = [1, 2, 4, 2, 5, 7, 2, 4, 9, 0], k = 3
Output: 15
Explanation: Three transactions with profits 3 + 5 + 7 = 15
```

### 💡 Approach 1: Recursion

**Intuition:**
- Track remaining transactions (cap)
- Transaction completes when we sell
- States: (day, buy, cap)

**Code:**
```python
def solve(n, arr, day, buy, cap):
    # Base case: no more days or no transactions left
    if day == n or cap == 0:
        return 0
    
    if buy == 0:  # Can buy
        # Buy today or skip
        include = -arr[day] + solve(n, arr, day+1, 1, cap)
        exclude = solve(n, arr, day+1, 0, cap)
    else:  # Can sell
        # Sell today (transaction completes, reduce cap) or skip
        include = arr[day] + solve(n, arr, day+1, 0, cap-1)  # cap-1: transaction done
        exclude = solve(n, arr, day+1, 1, cap)
    
    return max(include, exclude)
```

**Complexity:**
- **Time:** O(2^n) - Exponential
- **Space:** O(n) - Recursion stack

### 💡 Approach 2: Memoization

**Intuition:** Cache (day, buy, cap) states

**Code:**
```python
def solve_memo(n, arr, day, buy, cap, memo):
    # Return cached result if available
    if memo[day][buy][cap] != -1:
        return memo[day][buy][cap]
    
    # Base case
    if day == n or cap == 0:
        return 0
    
    if buy == 0:
        include = -arr[day] + solve_memo(n, arr, day+1, 1, cap, memo)
        exclude = solve_memo(n, arr, day+1, 0, cap, memo)
    else:
        # Reduce cap when completing a transaction (selling)
        include = arr[day] + solve_memo(n, arr, day+1, 0, cap-1, memo)
        exclude = solve_memo(n, arr, day+1, 1, cap, memo)
    
    # Cache and return result
    memo[day][buy][cap] = max(include, exclude)
    return memo[day][buy][cap]
```

**Complexity:**
- **Time:** O(n × 2 × k)
- **Space:** O(n × 2 × k)

### 💡 Approach 3: Tabulation

**Intuition:** Build DP table from base cases

**Code:**
```python
def tabulation(n, arr, k):
    if n == 1 or k == 0:
        return 0
    
    # DP table: dp[day][buy][cap]
    dp = [[[-1]*(k+1) for _ in range(2)] for _ in range(n+1)]
    
    # Base case: day == n (no days left)
    for cap in range(k+1):
        dp[n][0][cap] = 0
        dp[n][1][cap] = 0
    
    # Base case: cap == 0 (no transactions left)
    for day in range(1, n+1):
        dp[day][0][0] = 0
        dp[day][1][0] = 0
    
    # Fill table from bottom to top
    for day in range(n-1, -1, -1):
        for buy in range(2):
            for cap in range(1, k+1):
                if buy == 0:
                    include = -arr[day] + dp[day+1][1][cap]
                    exclude = dp[day+1][0][cap]
                else:
                    # Transaction completes on sell
                    include = arr[day] + dp[day+1][0][cap-1]
                    exclude = dp[day+1][1][cap]
                dp[day][buy][cap] = max(include, exclude)
    
    # Start at day 0, can buy, with k transactions
    return dp[0][0][k]
```

**Complexity:**
- **Time:** O(n × 2 × k)
- **Space:** O(n × 2 × k)

### 💡 Approach 4: Space Optimized

**Intuition:** Use only current and next day arrays

**Code:**
```python
def optimized(n, arr, k):
    if n == 1 or k == 0:
        return 0
    
    # Only track next and current day states
    dp_next = [[-1]*(k+1) for _ in range(2)]
    dp_curr = [[-1]*(k+1) for _ in range(2)]
    
    # Base cases
    for cap in range(k+1):
        dp_next[0][cap] = 0
        dp_next[1][cap] = 0
    
    dp_curr[0][0] = 0
    dp_curr[1][0] = 0
    
    for day in range(n-1, -1, -1):
        for buy in range(2):
            for cap in range(1, k+1):
                if buy == 0:
                    include = -arr[day] + dp_next[1][cap]
                    exclude = dp_next[0][cap]
                else:
                    # Decrement cap on sell
                    include = arr[day] + dp_next[0][cap-1]
                    exclude = dp_next[1][cap]
                dp_curr[buy][cap] = max(include, exclude)
        # Shift: current becomes next for next iteration
        dp_next = dp_curr.copy()
    
    return dp_next[0][k]
```

**Complexity:**
- **Time:** O(n × 2 × k)
- **Space:** O(2 × k) = O(k)

---

## 4. Best Time to Buy and Sell Stock with Transaction Fee

### 📝 Problem Description
Find maximum profit with **unlimited transactions** but with a **transaction fee** applied on each sell. Can only hold one share at a time.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [1, 3, 4, 0, 2], fee = 1
Output: 3
Explanation: Buy at 1, sell at 4 (profit = 3-1 = 2). Buy at 0, sell at 2 (profit = 2-1 = 1). Total = 3
```

**Example 2:**
```
Input: arr = [1, 3, 2, 8, 4, 9], fee = 2
Output: 8
Explanation: Buy at 1, sell at 8 (profit = 7-2 = 5). Buy at 4, sell at 9 (profit = 5-2 = 3). Total = 8
```

### 💡 Approach 1: Recursion

**Intuition:**
- Similar to unlimited transactions
- Deduct fee when selling

**Code:**
```python
def solve(n, arr, fee, day, buy):
    # Base case
    if day == n:
        return 0
    
    if buy == 0:  # Can buy
        include = -arr[day] + solve(n, arr, fee, day+1, 1)
        exclude = solve(n, arr, fee, day+1, 0)
    else:  # Can sell
        # Deduct fee when selling
        include = arr[day] - fee + solve(n, arr, fee, day+1, 0)
        exclude = solve(n, arr, fee, day+1, 1)
    
    return max(include, exclude)
```

**Complexity:**
- **Time:** O(2^n)
- **Space:** O(n)

### 💡 Approach 2: Memoization

**Code:**
```python
def solve_memo(n, arr, fee, day, buy, memo):
    # Return cached value
    if memo[day][buy] != -1:
        return memo[day][buy]
    
    if day == n:
        return 0
    
    if buy == 0:
        include = -arr[day] + solve_memo(n, arr, fee, day+1, 1, memo)
        exclude = solve_memo(n, arr, fee, day+1, 0, memo)
    else:
        # Fee applied on sell
        include = arr[day] - fee + solve_memo(n, arr, fee, day+1, 0, memo)
        exclude = solve_memo(n, arr, fee, day+1, 1, memo)
    
    # Cache result
    memo[day][buy] = max(include, exclude)
    return memo[day][buy]
```

**Complexity:**
- **Time:** O(n)
- **Space:** O(n)

### 💡 Approach 3: Tabulation

**Code:**
```python
def tabulation(n, arr, fee):
    # DP table
    dp = [[-1]*2 for _ in range(n+1)]
    # Base case
    dp[n][0] = 0
    dp[n][1] = 0
    
    for day in range(n-1, -1, -1):
        for buy in range(2):
            if buy == 0:
                include = -arr[day] + dp[day+1][1]
                exclude = dp[day+1][0]
            else:
                # Subtract fee on sell
                include = arr[day] - fee + dp[day+1][0]
                exclude = dp[day+1][1]
            dp[day][buy] = max(include, exclude)
    
    return dp[0][0]
```

**Complexity:**
- **Time:** O(n)
- **Space:** O(n)

### 💡 Approach 4: Space Optimized

**Code:**
```python
def optimized(n, arr, fee):
    # Space optimized: only track two states
    dp_next = [-1]*2
    dp_curr = [-1]*2
    dp_next[0] = 0
    dp_next[1] = 0
    
    for day in range(n-1, -1, -1):
        for buy in range(2):
            if buy == 0:
                include = -arr[day] + dp_next[1]
                exclude = dp_next[0]
            else:
                # Fee deducted on sell
                include = arr[day] - fee + dp_next[0]
                exclude = dp_next[1]
            dp_curr[buy] = max(include, exclude)
        dp_next = dp_curr.copy()
    
    return dp_next[0]
```

**Complexity:**
- **Time:** O(n)
- **Space:** O(1)

---

## 5. Buy and Sell Stocks with Cooldown

### 📝 Problem Description
Find maximum profit with **unlimited transactions** but with a **cooldown period**. Cannot buy stock on the very next day after selling (1-day cooldown).

### 🧪 Sample Test Cases

**Example:**
```
Input: arr = [9, 2, 6, 4, 7, 3]
Output: 7
Explanation: Multiple buy-sell cycles with cooldown considered
```

### 💡 Approach 1: Recursion

**Intuition:**
- When selling, skip next day (day+2) due to cooldown
- When buying, proceed normally (day+1)

**Code:**
```python
def solve(n, arr, day, buy):
    # Base case: day out of bounds
    if day >= n:
        return 0
    
    if buy == 0:  # Can buy
        include = -arr[day] + solve(n, arr, day+1, 1)
        exclude = solve(n, arr, day+1, 0)
    else:  # Can sell
        # After selling, skip next day (cooldown) -> day+2
        include = arr[day] + solve(n, arr, day+2, 0)
        exclude = solve(n, arr, day+1, 1)
    
    return max(include, exclude)
```

**Complexity:**
- **Time:** O(2^n)
- **Space:** O(n)

### 💡 Approach 2: Memoization

**Code:**
```python
def solve_memo(n, arr, day, buy, memo):
    # Check cache
    if memo[day][buy] != -1:
        return memo[day][buy]
    
    if day >= n:
        return 0
    
    if buy == 0:
        include = -arr[day] + solve_memo(n, arr, day+1, 1, memo)
        exclude = solve_memo(n, arr, day+1, 0, memo)
    else:
        # Cooldown: jump to day+2 after selling
        include = arr[day] + solve_memo(n, arr, day+2, 0, memo)
        exclude = solve_memo(n, arr, day+1, 1, memo)
    
    # Store result
    memo[day][buy] = max(include, exclude)
    return memo[day][buy]
```

**Complexity:**
- **Time:** O(n)
- **Space:** O(n)

### 💡 Approach 3: Tabulation

**Intuition:** Need n+2 size array to handle day+2 transitions

**Code:**
```python
def tabulation(n, arr):
    # Need n+2 size to handle day+2 transitions
    dp = [[-1]*2 for _ in range(n+2)]
    # Base cases
    dp[n][0] = 0
    dp[n][1] = 0
    dp[n+1][0] = 0
    dp[n+1][1] = 0
    
    for day in range(n-1, -1, -1):
        for buy in range(2):
            if buy == 0:
                include = -arr[day] + dp[day+1][1]
                exclude = dp[day+1][0]
            else:
                # Cooldown: use day+2 state
                include = arr[day] + dp[day+2][0]
                exclude = dp[day+1][1]
            dp[day][buy] = max(include, exclude)
    
    return dp[0][0]
```
                exclude = dp[day+1][1]
            dp[day][buy] = max(include, exclude)
    
    return dp[0][0]
```

**Complexity:**
- **Time:** O(n)
- **Space:** O(n)

### 💡 Approach 4: Space Optimized

**Intuition:** Track current, next, and next+2 states

**Code:**
```python
def optimized(n, arr):
    dp_next = [-1]*2
    dp_next2 = [-1]*2
    dp_curr = [-1]*2
    dp_next[0] = 0
    dp_next[1] = 0
    dp_next2[0] = 0
    dp_next2[1] = 0
    
    for day in range(n-1, -1, -1):
        for buy in range(2):
            if buy == 0:
                include = -arr[day] + dp_next[1]
                exclude = dp_next[0]
            else:
                include = arr[day] + dp_next2[0]
                exclude = dp_next[1]
            dp_curr[buy] = max(include, exclude)
        dp_next2 = dp_next.copy()
        dp_next = dp_curr.copy()
    
    return dp_next[0]
```

**Complexity:**
- **Time:** O(n)
- **Space:** O(1)

---

## 📊 Problem Comparison Table

| Problem | Transactions | Constraint | Best Time | Best Space | Key Difference |
|---------|--------------|------------|-----------|------------|----------------|
| **1. Single Transaction** | 1 | Buy once, sell once | O(n) | O(1) | Greedy approach, track min price |
| **2. Unlimited Transactions** | ∞ | No limit | O(n) | O(1) | Standard DP with buy/sell states |
| **3. K Transactions** | K | At most K | O(n×k) | O(k) | Add transaction count dimension |
| **4. Transaction Fee** | ∞ | Fee on sell | O(n) | O(1) | Deduct fee when selling |
| **5. Cooldown** | ∞ | 1-day cooldown | O(n) | O(1) | Jump day+2 on sell |

### 🎯 Quick Selection Guide

- **Need simplest solution?** → Problem 1 (Single Transaction)
- **Want maximum profit, no restrictions?** → Problem 2 (Unlimited)
- **Have transaction limit?** → Problem 3 (K Transactions)
- **Pay fees per trade?** → Problem 4 (Transaction Fee)
- **Need wait time between trades?** → Problem 5 (Cooldown)

### 🔑 Common Patterns

1. **State Definition**: `(day, buy, [cap])` where buy=0 (can buy) or buy=1 (can sell)
2. **Base Cases**: day >= n or cap == 0 returns 0
3. **Transitions**: 
   - Buy: `-price + next_state(sell)`
   - Sell: `+price + next_state(buy)`
4. **Optimization Path**: Recursion → Memoization → Tabulation → Space Optimized

### 💭 Key Insights

- **Buy state (0)**: You can buy stock, paying the price
- **Sell state (1)**: You can sell stock, receiving the price
- **Transaction completes**: When you SELL (important for counting)
- **Space optimization**: Most problems only need O(1) space in final form
