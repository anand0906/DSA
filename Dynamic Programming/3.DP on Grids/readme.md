
# Grid Unique Paths

## Problem Statement

Given two integers **m** and **n**, representing the number of **rows** and **columns** of a 2D grid.

Return the **number of unique ways** to go from the **top-left cell (0, 0)** to the **bottom-right cell (m-1, n-1)**.

**Movement rule:** You can only move **right** or **down** from any cell.

### Examples:

**Example 1:**
```
Input: m = 3, n = 2
Output: 3

Grid:
[S] [ ]
[ ] [ ]
[ ] [E]

Unique paths:
1) Right → Down → Down
2) Down → Right → Down
3) Down → Down → Right
```

**Example 2:**
```
Input: m = 2, n = 4
Output: 4

Grid:
[S] [ ] [ ] [ ]
[ ] [ ] [ ] [E]

Unique paths:
1) Down → Right → Right → Right
2) Right → Down → Right → Right
3) Right → Right → Down → Right
4) Right → Right → Right → Down
```

**Example 3:**
```
Input: m = 3, n = 3
Output: 6

Grid:
[S] [ ] [ ]
[ ] [ ] [ ]
[ ] [ ] [E]

All paths require 2 right moves and 2 down moves in different orders
```

---

## Step 1: Define The Problem

- Given **m** (rows) and **n** (columns)
- This represents a grid of size **m × n**
- **Start:** Top-left corner at position **(0, 0)**
- **End:** Bottom-right corner at position **(m-1, n-1)**
- **Allowed moves:** Right (column + 1) or Down (row + 1)
- **Goal:** Count the number of unique paths from start to end

---

## Step 2: Represent the Problem Programmatically

### Grid Representation:
- Use 2D indices: **(row, col)** or **(i, j)**
- Start: **(0, 0)**
- End: **(m-1, n-1)**

### Function Definition:
- **f(row, col):** Number of unique ways to reach cell (row, col) from (0, 0)
- **Goal:** Find f(m-1, n-1)

### State Variables:
- **row:** Current row position (0 to m-1)
- **col:** Current column position (0 to n-1)

---

## Step 3: Finding Base Cases

### Case 1: Starting Cell
- At position **(0, 0)**, we're already at the start
- There's **1 way** to be at the starting position (stay there)
- **f(0, 0) = 1**

### Case 2: Out of Bounds
- If row < 0 or col < 0, we've gone outside the grid
- There are **0 ways** to reach an invalid position
- **f(row, col) = 0 if row < 0 or col < 0**

### Case 3: First Row (row = 0)
- Can only move right (no down moves possible)
- All cells in first row have exactly **1 way** to reach them
- **f(0, j) = 1 for all j**

### Case 4: First Column (col = 0)
- Can only move down (no right moves possible)
- All cells in first column have exactly **1 way** to reach them
- **f(i, 0) = 1 for all i**

**Base cases summary:**
```
f(0, 0) = 1           (starting position)
f(row, col) = 0       if row < 0 or col < 0
f(0, j) = 1           for all j ≥ 0
f(i, 0) = 1           for all i ≥ 0
```

---

## Step 4: Finding The Recurrence Relation

### Understanding Path Count:

To reach any cell **(row, col)**, we can come from:
1. **Left cell:** (row, col-1) by moving **right**
2. **Top cell:** (row-1, col) by moving **down**

Since these are the **only two ways** to reach (row, col), the total number of paths is the **sum** of paths to these two cells.

### Example: m = 2, n = 2

```
Grid:
[1] [ ]
[ ] [?]

Finding f(1, 1):
```

**From left (1, 0):**
- To reach (1, 0), we can only go down from (0, 0)
- f(1, 0) = 1

**From top (0, 1):**
- To reach (0, 1), we can only go right from (0, 0)
- f(0, 1) = 1

**Total paths to (1, 1):**
- f(1, 1) = f(1, 0) + f(0, 1) = 1 + 1 = **2**

### ✅ Recurrence Relation:
```
f(row, col) = f(row-1, col) + f(row, col-1)
```

**In words:**
- Paths from top + Paths from left = Total paths

---

## Visual Understanding

### Grid Visualization (m = 3, n = 3)

**Grid with path counts:**
```
      col 0   col 1   col 2
row 0   1       1       1
row 1   1       2       3
row 2   1       3       6

Explanation:
(0,0) = 1 (base case)
(0,1) = 1 (only from left)
(0,2) = 1 (only from left)
(1,0) = 1 (only from top)
(1,1) = (1,0) + (0,1) = 1 + 1 = 2
(1,2) = (1,1) + (0,2) = 2 + 1 = 3
(2,0) = 1 (only from top)
(2,1) = (2,0) + (1,1) = 1 + 2 = 3
(2,2) = (2,1) + (1,2) = 3 + 3 = 6 ✓
```

**Visual paths flow:**
```
1  →  1  →  1
↓     ↓     ↓
1  →  2  →  3
↓     ↓     ↓
1  →  3  →  6

Each cell = sum of top + left
```

---

## Recursion Tree Visualization

### Example: m = 3, n = 3 (finding f(2, 2))

```
                              f(2,2)
                            /        \
                           /          \
                      f(1,2)          f(2,1)
                     /      \        /      \
                    /        \      /        \
                f(0,2)    f(1,1)  f(1,1)   f(2,0)
                  |       /    \   /   \      |
                  |      /      \ /     \     |
                  1   f(0,1) f(1,0) f(1,0) f(0,1)  1
                       |      |     |      |
                       |      |     |      |
                       1      1     1      1

Calculations:
f(0,2) = 1 (base case)
f(1,0) = 1 (base case)
f(0,1) = 1 (base case)
f(2,0) = 1 (base case)

f(1,1) = f(0,1) + f(1,0) = 1 + 1 = 2
f(1,2) = f(0,2) + f(1,1) = 1 + 2 = 3
f(2,1) = f(1,1) + f(2,0) = 2 + 1 = 3
f(2,2) = f(1,2) + f(2,1) = 3 + 3 = 6
```

### Key Observation:
- f(1,1) is computed **multiple times** (appears twice in tree)
- Many redundant calculations → Need DP!

---

## Solution 1: Recursive Approach (Naive)

### Code:
```python
def solve(row, col):
    # Base case: Reached starting position
    if row == 0 and col == 0:
        return 1
    
    # Base case: Out of bounds
    if row < 0 or col < 0:
        return 0
    
    # Recursive calls: paths from top + paths from left
    top = solve(row - 1, col)
    left = solve(row, col - 1)
    
    return top + left

# Driver Code
m = 3
n = 3
print(solve(m - 1, n - 1))
```

### How It Works:
1. Start from (m-1, n-1) - the target cell
2. Recursively explore paths from top and left
3. When reaching (0, 0), return 1 (found a valid path)
4. When going out of bounds, return 0 (invalid path)
5. Sum all valid paths

### Complexity:
- **Time Complexity:** O(2^(m+n)) - Exponential (binary tree)
- **Space Complexity:** O(m + n) - Recursion stack depth

### Pros & Cons:
✅ Simple and intuitive
✅ Directly models the problem
✅ Clear base cases
❌ Extremely slow for large grids
❌ Many redundant calculations
❌ Exponential time complexity

---

## Solution 2: Memoization (Top-Down DP)

### The Idea:
Cache the result for each (row, col) pair. Since there are m × n cells, we have m × n unique states.

### How We Convert from Recursion:
1. **Add 2D memo array** - `memo[m][n]` initialized with -1
2. **Check before computing** - If `memo[row][col] != -1`, return it
3. **Store after computing** - Save result in `memo[row][col]`
4. **Same recursive logic** - Just add caching

### Code:
```python
def solve_memo(row, col, memo):
    # CHECK: Already computed?
    if memo[row][col] != -1:
        return memo[row][col]
    
    # Base case: Starting position
    if row == 0 and col == 0:
        return 1
    
    # Base case: Out of bounds
    if row < 0 or col < 0:
        return 0
    
    # Recursive calls with memoization
    top = solve_memo(row - 1, col, memo)
    left = solve_memo(row, col - 1, memo)
    
    # STORE: Save result
    memo[row][col] = top + left
    return memo[row][col]

# Driver Code
m = 3
n = 3
memo = [[-1] * n for _ in range(m)]
print(solve_memo(m - 1, n - 1, memo))
```

### What Changed:
```
Recursive:                          Memoization:
─────────────────────              ──────────────────────────────
def solve(row, col):               def solve_memo(row, col, memo):
                                       if memo[row][col] != -1:  ← Check
                                           return memo[row][col]
    if row == 0 and col == 0:          if row == 0 and col == 0:
        return 1                           return 1
    if row < 0 or col < 0:             if row < 0 or col < 0:
        return 0                           return 0
    top = solve(row-1, col)            top = solve_memo(row-1, col, memo)
    left = solve(row, col-1)           left = solve_memo(row, col-1, memo)
                                       memo[row][col] = top + left  ← Store
    return top + left                  return memo[row][col]
```

### How It Works (Example: m=3, n=3):
```
Computing f(2,2):
  → Need f(1,2): Compute = 3, STORE memo[1][2] = 3
  → Need f(2,1): 
    → Need f(1,1): Compute = 2, STORE memo[1][1] = 2
    → Need f(2,0): Base case = 1
    → f(2,1) = 2 + 1 = 3, STORE memo[2][1] = 3
  → f(2,2) = 3 + 3 = 6, STORE memo[2][2] = 6

Each cell computed only once! Future lookups return immediately.
```

### Complexity:
- **Time Complexity:** O(m × n) - Compute each cell once
- **Space Complexity:** O(m × n) - Memo array + O(m+n) recursion stack

### Pros & Cons:
✅ Much faster than recursion
✅ Each cell computed only once
✅ Easy to convert from recursive
✅ Handles large grids efficiently
❌ Still uses recursion stack
❌ 2D memo array

---

## Solution 3: Tabulation (Bottom-Up DP)

### The Idea:
Build the solution iteratively from (0,0) to (m-1, n-1). Fill the DP table row by row, using previously computed values.

### How We Convert from Memoization:
1. **Create 2D DP array** - `dp[m][n]`
2. **Fill base case** - dp[0][0] = 1
3. **Fill first row** - All cells = 1 (can only come from left)
4. **Fill first column** - All cells = 1 (can only come from top)
5. **Fill remaining cells** - dp[i][j] = dp[i-1][j] + dp[i][j-1]
6. **Return** - dp[m-1][n-1]

### Code:
```python
def tabulation(m, n):
    # Create DP array
    dp = [[0] * n for _ in range(m)]
    
    # Base case: Starting position
    dp[0][0] = 1
    
    # Fill the DP table
    for i in range(m):
        for j in range(n):
            # Skip the starting position (already filled)
            if i == 0 and j == 0:
                continue
            
            top = 0
            left = 0
            
            # Check if top cell exists
            if i - 1 >= 0:
                top = dp[i - 1][j]
            
            # Check if left cell exists
            if j - 1 >= 0:
                left = dp[i][j - 1]
            
            # Sum of paths from top and left
            dp[i][j] = top + left
    
    # Return result for bottom-right cell
    return dp[m - 1][n - 1]

# Driver Code
m = 3
n = 3
print(tabulation(m, n))
```

### What Changed:
```
Memoization (Recursive):           Tabulation (Iterative):
────────────────────────           ─────────────────────────────
def solve_memo(row, col, memo):    def tabulation(m, n):
    if memo[row][col] != -1:           dp = [[0]*n for _ in range(m)]
        return memo[row][col]          dp[0][0] = 1
    if row == 0 and col == 0:          for i in range(m):      ← Loops
        return 1                           for j in range(n):
    if row < 0 or col < 0:                     if i==0 and j==0:
        return 0                                   continue
    top = solve_memo(row-1, col)               top, left = 0, 0
    left = solve_memo(row, col-1)              if i-1 >= 0:
    memo[row][col] = top + left                    top = dp[i-1][j]
    return memo[row][col]                      if j-1 >= 0:
                                                   left = dp[i][j-1]
                                               dp[i][j] = top + left
                                       return dp[m-1][n-1]
```

### How It Works (Example: m=3, n=3):

**Step 1: Initialize**
```
dp = [[0, 0, 0],
      [0, 0, 0],
      [0, 0, 0]]
```

**Step 2: Fill base case**
```
dp[0][0] = 1

dp = [[1, 0, 0],
      [0, 0, 0],
      [0, 0, 0]]
```

**Step 3: Fill row 0**
```
i=0, j=1: top=0 (i-1<0), left=dp[0][0]=1 → dp[0][1] = 1
i=0, j=2: top=0 (i-1<0), left=dp[0][1]=1 → dp[0][2] = 1

dp = [[1, 1, 1],
      [0, 0, 0],
      [0, 0, 0]]
```

**Step 4: Fill row 1**
```
i=1, j=0: top=dp[0][0]=1, left=0 (j-1<0) → dp[1][0] = 1
i=1, j=1: top=dp[0][1]=1, left=dp[1][0]=1 → dp[1][1] = 2
i=1, j=2: top=dp[0][2]=1, left=dp[1][1]=2 → dp[1][2] = 3

dp = [[1, 1, 1],
      [1, 2, 3],
      [0, 0, 0]]
```

**Step 5: Fill row 2**
```
i=2, j=0: top=dp[1][0]=1, left=0 (j-1<0) → dp[2][0] = 1
i=2, j=1: top=dp[1][1]=2, left=dp[2][0]=1 → dp[2][1] = 3
i=2, j=2: top=dp[1][2]=3, left=dp[2][1]=3 → dp[2][2] = 6

dp = [[1, 1, 1],
      [1, 2, 3],
      [1, 3, 6]]
```

**Step 6: Return**
```
dp[2][2] = 6
```

### Visual Flow:
```
Start: (0,0) = 1

→ Fill first row (can only come from left):
  1 → 1 → 1

→ Fill first column (can only come from top):
  1
  ↓
  1
  ↓
  1

→ Fill remaining (sum of top + left):
  1   1   1
  1   2   3
  1   3   6
```

### Complexity:
- **Time Complexity:** O(m × n) - Visit each cell once
- **Space Complexity:** O(m × n) - DP array only

### Pros & Cons:
✅ No recursion overhead
✅ Clear iterative logic
✅ Easy to trace and debug
✅ Can visualize entire DP table
❌ Uses O(m × n) space

---

## Solution 4: Space Optimized

### The Idea:
To compute dp[i][j], we only need:
- **dp[i-1][j]** (top) - from previous row
- **dp[i][j-1]** (left) - from current row

We don't need the entire 2D array! Just keep two 1D arrays: previous row and current row.

### How We Convert from Tabulation:
1. **Use two 1D arrays** - `dp_prev[n]` and `dp_curr[n]`
2. **Initialize dp_prev** - First row values
3. **For each row** - Compute dp_curr using dp_prev and dp_curr itself
4. **After each row** - Copy dp_curr to dp_prev
5. **Return** - dp_prev[n-1]

### Code:
```python
def optimized(m, n):
    # Arrays for previous and current row
    dp_prev = [0] * n
    dp_curr = [0] * n
    
    # Initialize: first cell of first row
    dp_prev[0] = 1
    dp_curr[0] = 1
    
    # Fill the grid row by row
    for i in range(m):
        for j in range(n):
            # Skip starting position
            if i == 0 and j == 0:
                continue
            
            top = 0
            left = 0
            
            # Top: from previous row
            if i - 1 >= 0:
                top = dp_prev[j]
            
            # Left: from current row (already computed)
            if j - 1 >= 0:
                left = dp_curr[j - 1]
            
            dp_curr[j] = top + left
        
        # Copy current to previous for next iteration
        dp_prev = dp_curr.copy()
    
    # Return result
    return dp_prev[n - 1]

# Driver Code
m = 3
n = 3
print(optimized(m, n))
```

### What Changed:
```
Tabulation:                         Space Optimized:
───────────────────────            ─────────────────────────────
dp = [[0]*n for _ in range(m)]     dp_prev = [0] * n
                                   dp_curr = [0] * n
dp[0][0] = 1                       dp_prev[0] = 1
                                   dp_curr[0] = 1

for i in range(m):                 for i in range(m):
    for j in range(n):                 for j in range(n):
        if i==0 and j==0:                  if i==0 and j==0:
            continue                           continue
        top, left = 0, 0                   top, left = 0, 0
        if i-1 >= 0:                       if i-1 >= 0:
            top = dp[i-1][j]                   top = dp_prev[j]  ← From prev row
        if j-1 >= 0:                       if j-1 >= 0:
            left = dp[i][j-1]                  left = dp_curr[j-1]  ← From curr row
        dp[i][j] = top + left              dp_curr[j] = top + left
                                       dp_prev = dp_curr.copy()  ← Copy!

return dp[m-1][n-1]                return dp_prev[n-1]
```

### How It Works (Example: m=3, n=3):

**Initial:**
```
dp_prev = [1, 0, 0]
dp_curr = [1, 0, 0]
```

**Row 0:**
```
j=1: top=0, left=dp_curr[0]=1 → dp_curr[1] = 1
j=2: top=0, left=dp_curr[1]=1 → dp_curr[2] = 1
dp_curr = [1, 1, 1]
Copy: dp_prev = [1, 1, 1]
```

**Row 1:**
```
j=0: dp_curr[0] = 1 (already set)
j=1: top=dp_prev[1]=1, left=dp_curr[0]=1 → dp_curr[1] = 2
j=2: top=dp_prev[2]=1, left=dp_curr[1]=2 → dp_curr[2] = 3
dp_curr = [1, 2, 3]
Copy: dp_prev = [1, 2, 3]
```

**Row 2:**
```
j=0: dp_curr[0] = 1 (already set)
j=1: top=dp_prev[1]=2, left=dp_curr[0]=1 → dp_curr[1] = 3
j=2: top=dp_prev[2]=3, left=dp_curr[1]=3 → dp_curr[2] = 6
dp_curr = [1, 3, 6]
Copy: dp_prev = [1, 3, 6]
```

**Return:** dp_prev[2] = 6

### Visual Representation:
```
Only keep 2 rows at a time:

Processing Row 1:
  dp_prev: [1, 1, 1]  ← Row 0 (for top values)
  dp_curr: [1, 2, 3]  ← Row 1 (being computed)

Processing Row 2:
  dp_prev: [1, 2, 3]  ← Row 1 (for top values)
  dp_curr: [1, 3, 6]  ← Row 2 (being computed)
```

### Complexity:
- **Time Complexity:** O(m × n) - Same as tabulation
- **Space Complexity:** O(n) - Only two 1D arrays!

### Pros & Cons:
✅ Optimal space usage
✅ Reduces from O(m×n) to O(n)
✅ Same time complexity
✅ **Best solution for large grids**

---

## Summary of Transformations

### Recursion → Memoization:
- **Add 2D memo array** `[m][n]`
- **Check memo** before computing
- **Store result** after computing
- **Same recursive structure** with caching

### Memoization → Tabulation:
- **Remove recursion** - use nested loops
- **Fill base cases first** - dp[0][0] = 1
- **Build bottom-up** - row by row, left to right
- **Each cell uses** - top and left neighbors

### Tabulation → Space Optimization:
- **Reduce to 1D arrays** - only need previous row
- **Use two arrays** - dp_prev and dp_curr
- **Update dp_curr** - using dp_prev (top) and dp_curr (left)
- **Copy after each row** - dp_prev = dp_curr

---

## Key Insights

### Problem Pattern:
This is a **"Grid Path Counting"** problem:
- 2D grid navigation
- Limited movement directions (right, down)
- Count all possible paths
- Classic DP on grids

### Mathematical Insight:
The answer is actually the **binomial coefficient**:
```
C(m+n-2, m-1) = C(m+n-2, n-1)
```
Because we need exactly (m-1) down moves and (n-1) right moves!

### Why DP Works Here:
- **Overlapping subproblems:** Same cells computed multiple times
- **Optimal substructure:** Paths to (i,j) = Paths to (i-1,j) + Paths to (i,j-1)
- **2D state space:** (row, col) uniquely identifies each subproblem

---

## Complete Working Code

```python
# Solution 1: Recursive
def solve(row, col):
    if row == 0 and col == 0:
        return 1
    if row < 0 or col < 0:
        return 0
    top = solve(row - 1, col)
    left = solve(row, col - 1)
    return top + left

# Solution 2: Memoization
def solve_memo(row, col, memo):
    if memo[row][col] != -1:
        return memo[row][col]
    if row == 0 and col == 0:
        return 1
    if row < 0 or col < 0:
        return 0
    top = solve_memo(row - 1, col, memo)
    left = solve_memo(row, col - 1, memo)
    memo[row][col] = top + left
    return memo[row][col]

# Solution 3: Tabulation
def tabulation(m, n):
    dp = [[0] * n for _ in range(m)]
    dp[0][0] = 1
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                continue
            top, left = 0, 0
            if i - 1 >= 0:
                top = dp[i - 1][j]
            if j - 1 >= 0:
                left = dp[i][j - 1]
            dp[i][j] = top + left
    return dp[m - 1][n - 1]

# Solution 4: Space Optimized
def optimized(m, n):
    dp_prev = [0] * n
    dp_curr = [0] * n
    dp_prev[0] = 1
    dp_curr[0] = 1
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                continue
            top, left = 0, 0
            if i - 1 >= 0:
                top = dp_prev[j]
            if j - 1 >= 0:
                left = dp_curr[j - 1]
            dp_curr[j] = top + left
        dp_prev = dp_curr.copy()
    return dp_prev[n - 1]

# Driver Code
m = 3
n = 3
print("Recursive:", solve(m - 1, n - 1))
print("Memoization:", solve_memo(m - 1, n - 1, [[-1] * n for _ in range(m)]))
print("Tabulation:", tabulation(m, n))
print("Optimized:", optimized(m, n))
```

---
