
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
