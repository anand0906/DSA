# Ninja's Training

## Problem Statement

Ninja is planning an **N days-long training schedule**. Each day, he can perform any one of these **three activities**:
1. **Running** (Activity 0)
2. **Fighting Practice** (Activity 1)
3. **Learning New Moves** (Activity 2)

Each activity has some **merit points** on each day. As Ninja has to improve all his skills, he **can't do the same activity in two consecutive days**.

You are given a **2D array** of size **N × 3** called **POINTS** with the points corresponding to each day and activity.

**Task:** Calculate the **maximum number of merit points** that Ninja can earn.

### Examples:

**Example 1:**
```
Input: points = [[1, 2, 5], [3, 1, 1], [3, 3, 3]]
Output: 11

Explanation:
Day 0: Choose Activity 2 (Learning) → 5 points
Day 1: Choose Activity 0 (Running) → 3 points
Day 2: Choose Activity 0 (Fighting) → 3 points
Total: 5 + 3 + 3 = 11
```

**Example 2:**
```
Input: points = [[10, 40, 70], [20, 50, 80], [30, 60, 90]]
Output: 210

Explanation:
Day 0: Choose Activity 2 (Learning) → 70 points
Day 1: Choose Activity 1 (Fighting) → 50 points
Day 2: Choose Activity 2 (Learning) → 90 points
Total: 70 + 50 + 90 = 210
```

**Example 3:**
```
Input: points = [[18, 11, 19], [4, 13, 7], [1, 8, 13]]
Output: 45

Explanation:
Day 0: Choose Activity 2 → 19 points
Day 1: Choose Activity 1 → 13 points
Day 2: Choose Activity 2 → 13 points
Total: 19 + 13 + 13 = 45
```

---

## Step 1: Define The Problem

- Ninja has **N days** of training
- Each day has **3 activities**, each with specific merit points
- **Constraint:** Cannot do the same activity on consecutive days
- **Goal:** Maximize total merit points earned across all N days

**Key Insight:** The activity chosen on a particular day affects which activities can be chosen the next day.

---

## Step 2: Represent the Problem Programmatically

### Input Representation:
- **2D Matrix:** `points[n][3]`
  - Rows (n): Represent days (0 to n-1)
  - Columns (3): Represent activities (0, 1, 2)
  - `points[i][j]`: Merit points for activity j on day i

### Function Definition:
- **f(day, last_activity):** Maximum points from day 0 to day, where last_activity was performed on day

### State Variables:
1. **day:** Current day (0 to n-1)
2. **last_activity:** The activity performed on the current day (0, 1, 2, or -1 for no constraint)

### Goal:
Find **f(n-1, -1)** where:
- n-1: Last day
- -1: No previous activity constraint (can choose any activity on last day)

---

## Step 3: Finding Base Cases

### Case 1: First Day (day = 0)

When we're at the first day, we need to consider what activity was "performed" on the previous day (represented by `last_activity`).

**If last_activity = -1** (no constraint - only one day):
- We can choose any activity
- Return maximum of all three activities
- `f(0, -1) = max(points[0][0], points[0][1], points[0][2])`

**If last_activity = 0** (Activity 0 was done on day 1, we're looking back to day 0):
- Can't choose Activity 0 on day 0
- Can choose Activity 1 or 2
- `f(0, 0) = max(points[0][1], points[0][2])`

**If last_activity = 1**:
- Can't choose Activity 1
- `f(0, 1) = max(points[0][0], points[0][2])`

**If last_activity = 2**:
- Can't choose Activity 2
- `f(0, 2) = max(points[0][0], points[0][1])`

### General Base Case Formula:
```python
if day == 0:
    maxi = 0
    for activity in range(3):
        if activity != last_activity:
            maxi = max(maxi, points[0][activity])
    return maxi
```

This works for any number of activities (not just 3).

---

## Step 4: Finding The Recurrence Relation

For any day > 0, to find the maximum points:

### For each possible activity on the current day:
1. Check if it's different from last_activity (no consecutive constraint)
2. If valid:
   - Current points = points[day][activity]
   - Previous maximum = f(day-1, activity) (activity becomes the "last" for previous day)
   - Total = Current points + Previous maximum
3. Take the maximum across all valid activities

### ✅ Recurrence Relation:
```python
f(day, last_activity) = max(
    points[day][activity] + f(day-1, activity)
    for all activity in range(3)
    where activity != last_activity
)
```

**In words:**
- For each valid activity on current day
- Add its points to the maximum obtainable from previous day
- Where that activity becomes the constraint for the previous day
- Return the maximum among all valid choices

---

## Recursion Tree Visualization

### Example: points = [[1, 2, 5], [3, 1, 1], [3, 3, 3]]

```
                            f(2, -1)
                  /            |            \
                 /             |             \
           Act0:3+f(1,0)  Act1:3+f(1,1)  Act2:3+f(1,2)
              /                 |                \
             /                  |                 \
        f(1,0)              f(1,1)             f(1,2)
        /    \              /    \             /    \
       /      \            /      \           /      \
  Act1:1+f(0,1) Act2:1+f(0,2)  Act0:3+f(0,0) Act2:1+f(0,2)  Act0:3+f(0,0) Act1:1+f(0,1)
      |           |              |           |              |           |
      |           |              |           |              |           |
   f(0,1)      f(0,2)         f(0,0)      f(0,2)         f(0,0)      f(0,1)
      |           |              |           |              |           |
   max(1,5)=5  max(1,2)=2    max(2,5)=5  max(1,2)=2    max(2,5)=5  max(1,5)=5
      ↓           ↓              ↓           ↓              ↓           ↓
      6           3              8           3              8           6

f(1,0) = max(6, 3) = 6
f(1,1) = max(8, 3) = 8
f(1,2) = max(8, 6) = 8

f(2,-1) = max(3+6, 3+8, 3+8) = max(9, 11, 11) = 11
```

### Optimal Path:
- Day 0: Activity 2 (5 points)
- Day 1: Activity 0 (3 points)
- Day 2: Activity 1 or 2 (3 points)
- Total: 5 + 3 + 3 = **11**

### Key Observation:
Multiple overlapping subproblems (f(0,0), f(0,1), f(0,2) computed multiple times).

---

## Solution 1: Recursive Approach (Naive)

### Code:
```python
def solve(points, day, last_activity):
    # Base case: First day
    if day == 0:
        maxi = 0
        for activity in range(3):
            if activity != last_activity:
                maxi = max(maxi, points[day][activity])
        return maxi
    
    # Try all activities on current day
    max_points = 0
    for activity in range(3):
        if activity != last_activity:
            # Current activity points + max from previous days
            current_points = points[day][activity] + solve(points, day - 1, activity)
            max_points = max(max_points, current_points)
    
    return max_points

# Driver Code
points = [[1, 2, 5], [3, 1, 1], [3, 3, 3]]
n = len(points)
print(solve(points, n - 1, -1))
```

### How It Works:
1. Start from last day with no constraint (last_activity = -1)
2. For each valid activity on current day:
   - Add its points
   - Recursively find max points from previous day with current activity as constraint
3. Return maximum across all valid choices

### Complexity:
- **Time Complexity:** O(3^n) - Each state branches into 2-3 choices
- **Space Complexity:** O(n) - Recursion stack depth

### Pros & Cons:
✅ Simple and intuitive
✅ Directly models the problem
✅ Clear constraint handling
❌ Extremely slow for large n
❌ Many redundant calculations

---

## Solution 2: Memoization (Top-Down DP)

### The Idea:
Cache results for each (day, last_activity) pair. Since there are n days and 3 possible last activities (0, 1, 2), we have n × 3 unique states.

### How We Convert from Recursion:
1. **Add 2D memo array** - `memo[n][3]` initialized with -1
2. **Check before computing** - If `memo[day][last_activity] != -1`, return it
3. **Store after computing** - Save result in `memo[day][last_activity]`
4. **Handle base case** - Still need to check `day == 0`

**Note:** We use last_activity as index (0, 1, 2), but when calling initially with -1, we don't cache that since it's not part of our state space.

### Code:
```python
def solve_memo(points, day, last_activity, memo):
    # CHECK: Already computed?
    if memo[day][last_activity] != -1:
        return memo[day][last_activity]
    
    # Base case: First day
    if day == 0:
        maxi = 0
        for activity in range(3):
            if activity != last_activity:
                maxi = max(maxi, points[day][activity])
        memo[day][last_activity] = maxi
        return maxi
    
    # Try all activities on current day
    max_points = 0
    for activity in range(3):
        if activity != last_activity:
            current_points = points[day][activity] + solve_memo(points, day - 1, activity, memo)
            max_points = max(max_points, current_points)
    
    # STORE: Save result
    memo[day][last_activity] = max_points
    return memo[day][last_activity]

# Driver Code
points = [[1, 2, 5], [3, 1, 1], [3, 3, 3]]
n = len(points)
memo = [[-1] * 3 for _ in range(n)]
print(solve_memo(points, n - 1, -1, memo))
```

### What Changed:
```
Recursive:                              Memoization:
──────────────────────────             ─────────────────────────────────
def solve(points, day, last):         def solve_memo(points, day, last, memo):
                                           if memo[day][last] != -1:  ← Check
                                               return memo[day][last]
    if day == 0:                           if day == 0:
        maxi = 0                               maxi = 0
        for activity in range(3):              for activity in range(3):
            if activity != last:                   if activity != last:
                maxi = max(...)                        maxi = max(...)
        return maxi                            memo[day][last] = maxi  ← Store
                                               return maxi
    max_points = 0                         max_points = 0
    for activity in range(3):              for activity in range(3):
        if activity != last:                   if activity != last:
            current = points[day][act]             current = points[day][act]
                    + solve(...)                           + solve_memo(...)
            max_points = max(...)                  max_points = max(...)
                                           memo[day][last] = max_points  ← Store
    return max_points                      return memo[day][last]
```

### Complexity:
- **Time Complexity:** O(n × 3 × 3) = O(9n) = O(n)
  - n days × 3 last_activity states × 3 activity choices
- **Space Complexity:** O(n × 3) + O(n) = O(n)
  - Memo array + recursion stack

### Pros & Cons:
✅ Much faster than recursion
✅ Each state computed only once
✅ Easy to convert from recursive
❌ Still uses recursion stack
❌ 2D memo array

---

## Solution 3: Tabulation (Bottom-Up DP)

### The Idea:
Build solution iteratively from day 0 to day n-1. For each day and each possible last_activity, compute the maximum points.

### How We Convert from Memoization:
1. **Create 2D DP array** - `dp[n][3]`
2. **Fill base cases** - All possibilities for day 0
3. **Outer loop** - Iterate through days (1 to n-1)
4. **Middle loop** - For each last_activity (0, 1, 2)
5. **Inner loop** - Try all activities on current day
6. **Final answer** - Maximum of `dp[n-1][0]`, `dp[n-1][1]`, `dp[n-1][2]`

### Code:
```python
def tabulation(points):
    n = len(points)
    
    # Create DP array
    dp = [[0] * 3 for _ in range(n)]
    
    # Fill base cases for day 0
    for last_activity in range(3):
        maxi = 0
        for activity in range(3):
            if activity != last_activity:
                maxi = max(maxi, points[0][activity])
        dp[0][last_activity] = maxi
    
    # Fill DP table for days 1 to n-1
    for day in range(1, n):
        for last_activity in range(3):
            max_points = 0
            for activity in range(3):
                if activity != last_activity:
                    current_points = points[day][activity] + dp[day - 1][activity]
                    max_points = max(max_points, current_points)
            dp[day][last_activity] = max_points
    
    # Return maximum from last day (any activity could be last)
    return max(dp[n - 1])

# Driver Code
points = [[1, 2, 5], [3, 1, 1], [3, 3, 3]]
print(tabulation(points))
```

### How It Works (Example: [[1,2,5], [3,1,1], [3,3,3]]):

**Step 1: Initialize DP array**
```
dp = [[0, 0, 0],
      [0, 0, 0],
      [0, 0, 0]]
```

**Step 2: Fill base cases (day 0)**
```
For last_activity = 0: Can choose activity 1 or 2 → max(2, 5) = 5
For last_activity = 1: Can choose activity 0 or 2 → max(1, 5) = 5
For last_activity = 2: Can choose activity 0 or 1 → max(1, 2) = 2

dp[0] = [5, 5, 2]
```

**Step 3: Fill day 1**
```
For last_activity = 0:
  Activity 1: points[1][1] + dp[0][1] = 1 + 5 = 6
  Activity 2: points[1][2] + dp[0][2] = 1 + 2 = 3
  max = 6

For last_activity = 1:
  Activity 0: points[1][0] + dp[0][0] = 3 + 5 = 8
  Activity 2: points[1][2] + dp[0][2] = 1 + 2 = 3
  max = 8

For last_activity = 2:
  Activity 0: points[1][0] + dp[0][0] = 3 + 5 = 8
  Activity 1: points[1][1] + dp[0][1] = 1 + 5 = 6
  max = 8

dp[1] = [6, 8, 8]
```

**Step 4: Fill day 2**
```
For last_activity = 0:
  Activity 1: points[2][1] + dp[1][1] = 3 + 8 = 11
  Activity 2: points[2][2] + dp[1][2] = 3 + 8 = 11
  max = 11

For last_activity = 1:
  Activity 0: points[2][0] + dp[1][0] = 3 + 6 = 9
  Activity 2: points[2][2] + dp[1][2] = 3 + 8 = 11
  max = 11

For last_activity = 2:
  Activity 0: points[2][0] + dp[1][0] = 3 + 6 = 9
  Activity 1: points[2][1] + dp[1][1] = 3 + 8 = 11
  max = 11

dp[2] = [11, 11, 11]
```

**Step 5: Return answer**
```
max(dp[2]) = max(11, 11, 11) = 11
```

### DP Table Interpretation:
- `dp[day][last_activity]` = Maximum points from day 0 to day if last_activity was performed on day

### Complexity:
- **Time Complexity:** O(n × 3 × 3) = O(n)
- **Space Complexity:** O(n × 3) = O(n)

### Pros & Cons:
✅ No recursion overhead
✅ Clear iterative logic
✅ Easy to trace
✅ Can see full DP table
❌ Uses O(n) space

---

## Solution 4: Space Optimized

### The Idea:
To compute `dp[day]`, we only need `dp[day-1]`. We don't need the entire 2D array! Just keep two 1D arrays: current and previous.

### How We Convert from Tabulation:
1. **Use two 1D arrays** - `dp_prev[3]` and `dp_curr[3]`
2. **Fill dp_prev with day 0** - Base cases
3. **For each day** - Compute dp_curr using dp_prev
4. **After each day** - Copy dp_curr to dp_prev
5. **Final answer** - Maximum of dp_prev

### Code:
```python
def optimized(points):
    n = len(points)
    
    # Arrays for previous and current day
    dp_prev = [0] * 3
    dp_curr = [0] * 3
    
    # Fill base cases for day 0
    for last_activity in range(3):
        maxi = 0
        for activity in range(3):
            if activity != last_activity:
                maxi = max(maxi, points[0][activity])
        dp_prev[last_activity] = maxi
    
    # Build for days 1 to n-1
    for day in range(1, n):
        for last_activity in range(3):
            max_points = 0
            for activity in range(3):
                if activity != last_activity:
                    current_points = points[day][activity] + dp_prev[activity]
                    max_points = max(max_points, current_points)
            dp_curr[last_activity] = max_points
        
        # Copy current to previous for next iteration
        dp_prev = dp_curr.copy()
    
    # Return maximum from last computed day
    return max(dp_prev)

# Driver Code
points = [[1, 2, 5], [3, 1, 1], [3, 3, 3]]
print(optimized(points))
```

### What Changed:
```
Tabulation:                             Space Optimized:
───────────────────────────            ─────────────────────────────────
dp = [[0]*3 for _ in range(n)]         dp_prev = [0] * 3
                                       dp_curr = [0] * 3

for last in range(3):                  for last in range(3):
    ...                                    ...
    dp[0][last] = maxi                     dp_prev[last] = maxi

for day in range(1, n):                for day in range(1, n):
    for last in range(3):                  for last in range(3):
        ...                                    ...
        dp[day][last] = max_points             dp_curr[last] = max_points
                                           dp_prev = dp_curr.copy()  ← Copy!

return max(dp[n-1])                    return max(dp_prev)
```

### Visual Representation:
```
Day 0: Fill dp_prev = [5, 5, 2]

Day 1: 
  Compute dp_curr = [6, 8, 8] using dp_prev
  Copy: dp_prev = [6, 8, 8]

Day 2:
  Compute dp_curr = [11, 11, 11] using dp_prev
  Copy: dp_prev = [11, 11, 11]

Return max(dp_prev) = 11
```

### Complexity:
- **Time Complexity:** O(n × 3 × 3) = O(n)
- **Space Complexity:** O(3 + 3) = O(1) - Constant space!

### Pros & Cons:
✅ Optimal space usage
✅ Same time complexity
✅ Constant space
✅ **Best solution overall**

---

## Summary of Transformations

### Recursion → Memoization:
- **Add 2D memo array** `[n][3]`
- **Check memo** before computing
- **Store result** after computing
- **Handle 2 parameters** (day, last_activity)

### Memoization → Tabulation:
- **Remove recursion** - nested loops
- **Fill base cases first** - day 0 for all last_activity
- **Build bottom-up** - day 1 to n-1
- **Triple nested loops** - day, last_activity, activity

### Tabulation → Space Optimization:
- **Reduce to 1D arrays** - only need previous day
- **Use two arrays** - dp_prev and dp_curr
- **Copy after each day** - dp_prev = dp_curr
- **Same logic** - just different storage

---

## Key Insights

### Problem Pattern:
This is a **"Choice with Constraint"** DP pattern:
- Multiple choices at each step (3 activities)
- Previous choice constrains current choice (no consecutive)
- Maximize sum across all steps

### State Design:
```
State: (day, last_activity)
- day: Current position in sequence
- last_activity: Constraint from previous step
- Value: Maximum points achievable
```

### Why 2D DP?
- We need to track **where we are** (day)
- AND what **constraint we have** (last_activity)
- This requires 2 dimensions in our state space

---

## Complete Working Code

```python
# Solution 1: Recursive
def solve(points, day, last_activity):
    if day == 0:
        maxi = 0
        for activity in range(3):
            if activity != last_activity:
                maxi = max(maxi, points[day][activity])
        return maxi
    max_points = 0
    for activity in range(3):
        if activity != last_activity:
            current_points = points[day][activity] + solve(points, day - 1, activity)
            max_points = max(max_points, current_points)
    return max_points

# Solution 2: Memoization
def solve_memo(points, day, last_activity, memo):
    if memo[day][last_activity] != -1:
        return memo[day][last_activity]
    if day == 0:
        maxi = 0
        for activity in range(3):
            if activity != last_activity:
                maxi = max(maxi, points[day][activity])
        memo[day][last_activity] = maxi
        return maxi
    max_points = 0
    for activity in range(3):
        if activity != last_activity:
            current_points = points[day][activity] + solve_memo(points, day - 1, activity, memo)
            max_points = max(max_points, current_points)
    memo[day][last_activity] = max_points
    return memo[day][last_activity]

# Solution 3: Tabulation
def tabulation(points):
    n = len(points)
    dp = [[0] * 3 for _ in range(n)]
    for last_activity in range(3):
        maxi = 0
        for activity in range(3):
            if activity != last_activity:
                maxi = max(maxi, points[0][activity])
        dp[0][last_activity] = maxi
    for day in range(1, n):
        for last_activity in range(3):
            max_points = 0
            for activity in range(3):
                if activity != last_activity:
                    current_points = points[day][activity] + dp[day - 1][activity]
                    max_points = max(max_points, current_points)
            dp[day][last_activity] = max_points
    return max(dp[n - 1])

# Solution 4: Space Optimized
def optimized(points):
    n = len(points)
    dp_prev = [0] * 3
    dp_curr = [0] * 3
    for last_activity in range(3):
        maxi = 0
        for activity in range(3):
            if activity != last_activity:
                maxi = max(maxi, points[0][activity])
        dp_prev[last_activity] = maxi
    for day in range(1, n):
        for last_activity in range(3):
            max_points = 0
            for activity in range(3):
                if activity != last_activity:
                    current_points = points[day][activity] + dp_prev[activity]
                    max_points = max(max_points, current_points)
            dp_curr[last_activity] = max_points
        dp_prev = dp_curr.copy()
    return max(dp_prev)

# Driver Code
points = [[1, 2, 5], [3, 1, 1], [3, 3, 3]]
n = len(points)
print("Recursive:", solve(points, n - 1, -1))
print("Memoization:", solve_memo(points, n - 1, -1, [[-1] * 3 for _ in range(n)]))
print("Tabulation:", tabulation(points))
print("Optimized:", optimized(points))
```

---

**Happy Coding! 🚀**
