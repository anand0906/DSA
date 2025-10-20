
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

# Minimum Path Sum

## Problem Statement

Given an M×N matrix of integers, find a path from the top-left corner to the bottom-right corner such that the sum of values along the path is minimized.

At every cell, we can move in only two directions: **right** or **bottom**.

**Example:**
```
Input: matrix = [
  [1, 2, 10, 4],
  [100, 3, 2, 1],
  [1, 1, 20, 2],
  [1, 2, 2, 1]
]

Output: 14

Explanation: 
Path: 1 → 2 → 3 → 2 → 1 → 2 → 2 → 1
Route: (0,0) → (0,1) → (1,1) → (1,2) → (1,3) → (2,3) → (3,3)
Sum = 1 + 2 + 3 + 2 + 1 + 2 + 2 + 1 = 14
```

---

## Step 1: Define the Problem

Given a matrix of size m×n where:
- **m** = number of rows
- **n** = number of columns

We need to:
- Start from top-left corner: `matrix[0][0]`
- Reach bottom-right corner: `matrix[m-1][n-1]`
- Move only **right** or **down**
- Find the path with **minimum sum** of all cell values along the path

---

## Step 2: Represent the Problem Programmatically

We can use a 2D array to represent the matrix of size m×n.

Define: **f(row, col)** = minimum sum to reach `matrix[row][col]` from `matrix[0][0]`

**Goal:** Find `f(m-1, n-1)`

---

## Step 3: Finding Base Cases

1. **Starting position:** `f(0, 0) = matrix[0][0]` (we start here)
2. **Out of bounds:** `f(row<0, col<0) = ∞` (invalid path, use infinity since we're finding minimum)

---

## Step 4: Finding the Recurrence Relation

To reach `(row, col)`, we can come from:
- **Top:** `(row-1, col)` - moving down
- **Left:** `(row, col-1)` - moving right

**Recurrence Relation:**
```
f(row, col) = matrix[row][col] + min(f(row-1, col), f(row, col-1))
```

---

## Recursion Tree Example

Let's visualize for a 3×3 matrix:
```
Matrix:
[1, 3, 1]
[1, 5, 1]
[4, 2, 1]

Recursion Tree for f(2,2):
                              f(2,2)
                            /        \
                       f(1,2)        f(2,1)
                      /      \      /      \
                 f(0,2)   f(1,1) f(1,1)  f(2,0)
                /    \    /    \  /   \   /    \
           f(-1,2) f(0,1) f(0,1) f(1,0) f(1,0) f(2,-1)
             |     /   \   /   \  /   \   /   \    |
            [∞] f(-1,1) f(0,0) ... ... ... ... [∞]
                  |       |
                 [∞]     [1]

Values computed:
f(0,0) = 1
f(0,1) = 1 + 3 = 4
f(0,2) = 4 + 1 = 5
f(1,0) = 1 + 1 = 2
f(1,1) = 5 + min(f(0,1), f(1,0)) = 5 + min(4, 2) = 7
f(1,2) = 1 + min(f(0,2), f(1,1)) = 1 + min(5, 7) = 6
f(2,0) = 2 + 1 = 3  (only from top, left is ∞)
f(2,1) = 2 + min(f(1,1), f(2,0)) = 2 + min(7, 3) = 5
f(2,2) = 1 + min(f(1,2), f(2,1)) = 1 + min(6, 5) = 6

Legend:
- [∞] = out of bounds (returns infinity)
- [1] = base case at (0,0)
```

**Observations:**
- Many subproblems are repeated: `f(1,1)`, `f(0,1)`, `f(1,0)` appear multiple times
- Exponential time complexity due to overlapping subproblems
- Time Complexity: **O(2^(m+n))**

---

## Step 5: Recursive Solution

```python
def solve(matrix, row, col):
    # Base case: starting position
    if row == 0 and col == 0:
        return matrix[row][col]
    
    # Out of bounds: return infinity
    if row < 0 or col < 0:
        return float('inf')
    
    # Recursive calls
    top = matrix[row][col] + solve(matrix, row-1, col)
    left = matrix[row][col] + solve(matrix, row, col-1)
    
    return min(top, left)

# Usage: solve(matrix, m-1, n-1)
```

**Complexity:**
- **Time:** O(2^(m+n)) - Exponential, explores all possible paths
- **Space:** O(m+n) - Recursion stack depth

---

## Step 6: Memoization (Top-Down DP)

**Optimization Strategy:** Store results of computed subproblems in a 2D array to avoid redundant calculations.

**How we convert Recursion → Memoization:**
1. Create a 2D array `memo` initialized with `-1` to indicate uncomputed states
2. Before computing `f(row, col)`, check if `memo[row][col]` already has a result
3. If yes, return the stored value immediately
4. If no, compute the result, store it in `memo[row][col]`, then return it
5. This eliminates the repeated calculations seen in the recursion tree

**Example:** In the recursion tree, `f(1,1)` is computed multiple times. With memoization, it's computed once and reused.

```python
def solve_memo(matrix, row, col, memo):
    # Base case: starting position
    if row == 0 and col == 0:
        return matrix[row][col]
    
    # Out of bounds
    if row < 0 or col < 0:
        return float('inf')
    
    # Check if already computed
    if memo[row][col] != -1:
        return memo[row][col]
    
    # Compute and store
    top = matrix[row][col] + solve_memo(matrix, row-1, col, memo)
    left = matrix[row][col] + solve_memo(matrix, row, col-1, memo)
    memo[row][col] = min(top, left)
    
    return memo[row][col]

# Usage:
# n, m = len(matrix), len(matrix[0])
# memo = [[-1]*m for _ in range(n)]
# result = solve_memo(matrix, n-1, m-1, memo)
```

**Complexity:**
- **Time:** O(m×n) - Each cell computed once
- **Space:** O(m+n) + O(m×n) - Recursion stack + memoization array

**Why this is better:** Each subproblem `f(row, col)` is solved exactly once and its result is stored. Any future call to the same subproblem retrieves the result in O(1) time.

---

## Step 7: Tabulation (Bottom-Up DP)

**Optimization Strategy:** Build solution iteratively from base cases to eliminate recursion overhead.

**How we convert Memoization → Tabulation:**
1. Replace recursive calls with nested loops that iterate through all states
2. Start from the base case `dp[0][0] = matrix[0][0]`
3. Fill the DP table row by row, left to right
4. Each cell `dp[i][j]` depends only on already computed cells: `dp[i-1][j]` (top) and `dp[i][j-1]` (left)
5. This follows natural dependency order: we compute smaller subproblems before larger ones
6. No recursion means no call stack overhead

```python
def tabulation(matrix):
    n, m = len(matrix), len(matrix[0])
    
    # Create DP table
    dp = [[float('inf')]*m for _ in range(n)]
    
    # Base case
    dp[0][0] = matrix[0][0]
    
    # Fill DP table
    for row in range(n):
        for col in range(m):
            # Skip base case
            if row == 0 and col == 0:
                continue
            
            top, left = float('inf'), float('inf')
            
            # Get value from top cell
            if row-1 >= 0:
                top = matrix[row][col] + dp[row-1][col]
            
            # Get value from left cell
            if col-1 >= 0:
                left = matrix[row][col] + dp[row][col-1]
            
            dp[row][col] = min(top, left)
    
    return dp[n-1][m-1]
```

**Complexity:**
- **Time:** O(m×n) - Single pass through the matrix
- **Space:** O(m×n) - DP table

**Why this is better:** 
- No recursion stack overhead
- Iterative approach is more efficient and easier to optimize further
- Better cache locality for large matrices

---

## Step 8: Space Optimization

**Optimization Strategy:** Observe that we only need the previous row to compute the current row.

**How we convert Tabulation → Space Optimized:**
1. Analyze dependencies in tabulation: `dp[i][j]` only needs `dp[i-1][j]` (previous row) and `dp[i][j-1]` (current row, previous column)
2. We don't need the entire 2D array - just two 1D arrays:
   - `dp_prev`: stores values from the previous row
   - `dp_curr`: stores values being computed for the current row
3. As we process each cell left to right in a row:
   - `dp_curr[col-1]` gives us the left value (already computed in current row)
   - `dp_prev[col]` gives us the top value (from previous row)
4. After completing a row, copy `dp_curr` to `dp_prev` for the next iteration
5. This reduces space from O(m×n) to O(n)

**Visualization:**
```
Processing row i:
dp_prev: [values from row i-1]
dp_curr: [computing row i, left to right]

At each cell (i, j):
- top = dp_prev[j]  ← from previous row
- left = dp_curr[j-1]  ← from current row
```

```python
def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    
    # Two arrays for previous and current row
    dp_curr = [float('inf')] * m
    dp_prev = [float('inf')] * m
    
    # Base case
    dp_curr[0] = matrix[0][0]
    dp_prev[0] = matrix[0][0]
    
    for row in range(n):
        for col in range(m):
            # Skip base case
            if row == 0 and col == 0:
                continue
            
            top, left = float('inf'), float('inf')
            
            # Get value from top (previous row)
            if row-1 >= 0:
                top = matrix[row][col] + dp_prev[col]
            
            # Get value from left (current row)
            if col-1 >= 0:
                left = matrix[row][col] + dp_curr[col-1]
            
            dp_curr[col] = min(top, left)
        
        # Move current row to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[m-1]
```

**Complexity:**
- **Time:** O(m×n) - Same iteration pattern
- **Space:** O(n) - Only two 1D arrays of size n

**Why this is better:** Achieves the same time complexity with minimal space usage. Critical for large matrices where memory is a constraint.

---

## Summary of Optimization Journey

| Approach | Time | Space | Key Improvement |
|----------|------|-------|-----------------|
| **Recursion** | O(2^(m+n)) | O(m+n) | Basic solution, explores all paths with many redundant calculations |
| **Memoization** | O(m×n) | O(m×n) + O(m+n) | Stores computed results to eliminate redundant subproblem calculations |
| **Tabulation** | O(m×n) | O(m×n) | Removes recursion overhead by building solution iteratively from base case |
| **Space Optimized** | O(m×n) | O(n) | Minimizes memory by keeping only previous row needed for computation |

**Progression Summary:**
1. **Recursion → Memoization:** Add caching to avoid recomputing same subproblems
2. **Memoization → Tabulation:** Replace recursion with iteration to eliminate call stack
3. **Tabulation → Space Optimized:** Identify that only adjacent rows are needed, reduce 2D array to two 1D arrays

---

## Complete Code

```python
def solve(matrix, row, col):
    if row == 0 and col == 0:
        return matrix[row][col]
    if row < 0 or col < 0:
        return float('inf')
    top = matrix[row][col] + solve(matrix, row-1, col)
    left = matrix[row][col] + solve(matrix, row, col-1)
    return min(top, left)

def solve_memo(matrix, row, col, memo):
    if row == 0 and col == 0:
        return matrix[row][col]
    if row < 0 or col < 0:
        return float('inf')
    if memo[row][col] != -1:
        return memo[row][col]
    top = matrix[row][col] + solve_memo(matrix, row-1, col, memo)
    left = matrix[row][col] + solve_memo(matrix, row, col-1, memo)
    memo[row][col] = min(top, left)
    return memo[row][col]

def tabulation(matrix):
    n, m = len(matrix), len(matrix[0])
    dp = [[float('inf')]*m for _ in range(n)]
    dp[0][0] = matrix[0][0]
    for row in range(n):
        for col in range(m):
            if row == 0 and col == 0:
                continue
            top, left = float('inf'), float('inf')
            if row-1 >= 0:
                top = matrix[row][col] + dp[row-1][col]
            if col-1 >= 0:
                left = matrix[row][col] + dp[row][col-1]
            dp[row][col] = min(top, left)
    return dp[n-1][m-1]

def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    dp_curr = [float('inf')]*m
    dp_prev = [float('inf')]*m
    dp_curr[0] = matrix[0][0]
    dp_prev[0] = matrix[0][0]
    for row in range(n):
        for col in range(m):
            if row == 0 and col == 0:
                continue
            top, left = float('inf'), float('inf')
            if row-1 >= 0:
                top = matrix[row][col] + dp_prev[col]
            if col-1 >= 0:
                left = matrix[row][col] + dp_curr[col-1]
            dp_curr[col] = min(top, left)
        dp_prev = dp_curr.copy()
    return dp_prev[m-1]

# Test
matrix = [
    [1, 2, 10, 4],
    [100, 3, 2, 1],
    [1, 1, 20, 2],
    [1, 2, 2, 1]
]

n, m = len(matrix), len(matrix[0])
print(solve(matrix, n-1, m-1))  # 14
print(solve_memo(matrix, n-1, m-1, [[-1]*m for _ in range(n)]))  # 14
print(tabulation(matrix))  # 14
print(optimized(matrix))  # 14
```

---

## Usage

```python
# Define your matrix
matrix = [
    [1, 2, 10, 4],
    [100, 3, 2, 1],
    [1, 1, 20, 2],
    [1, 2, 2, 1]
]

# Choose the approach based on constraints
result = optimized(matrix)  # Recommended for most cases
print(f"Minimum path sum: {result}")
```


---

# Minimum Falling Path Sum

## Problem Statement

Given a 2D array called matrix consisting of integer values, return the minimum path sum that can be obtained by starting at **any cell in the first row** and ending at **any cell in the last row**.

Movement is allowed only to three directions from the current cell:
- **Bottom** (directly down)
- **Bottom-left** (diagonally down-left)
- **Bottom-right** (diagonally down-right)

**Example 1:**
```
Input: matrix = [[1, 2, 10, 4], 
                 [100, 3, 2, 1], 
                 [1, 1, 20, 2], 
                 [1, 2, 2, 1]]

Output: 6

Explanation:
Optimal route: Start at 1st cell of 1st row → bottom-right → bottom → bottom-left
Path: 1 → 3 → 1 → 1
Sum: 1 + 3 + 1 + 1 = 6
```

**Example 2:**
```
Input: matrix = [[1, 4, 3, 1], 
                 [2, 3, -1, -1], 
                 [1, 1, -1, 8]]

Output: -1

Explanation:
Optimal route: Start at 4th cell of 1st row → bottom-left → bottom
Path: 1 → -1 → -1
Sum: 1 + (-1) + (-1) = -1
```

---

## Step 1: Define the Problem

Given a matrix of size m×n where:
- **m** = number of rows
- **n** = number of columns

We need to:
- Start from **any cell in the first row**: `matrix[0][j]` where `0 ≤ j < n`
- Reach **any cell in the last row**: `matrix[m-1][j]` where `0 ≤ j < n`
- Move only in three directions: **bottom**, **bottom-left**, or **bottom-right**
- Find the path with **minimum sum** of all cell values along the path

**Key Difference from other path problems:** Multiple starting and ending points!

---

## Step 2: Represent the Problem Programmatically

We can use a 2D array to represent the matrix of size m×n.

Define: **f(row, col)** = minimum path sum from `matrix[row][col]` to any cell in the last row

**Goal:** Find `min(f(0, 0), f(0, 1), ..., f(0, n-1))` - minimum among all starting points in first row

---

## Step 3: Finding Base Cases

**Last row:** If we're at the last row, the minimum path sum is just the value at that cell.
```
f(m-1, j) = matrix[m-1][j]  for all j
```

**Out of bounds:** If row or column is out of bounds, return infinity (invalid path)
```
f(row, col) = ∞  if row < 0 or col < 0 or row ≥ m or col ≥ n
```

---

## Step 4: Finding the Recurrence Relation

To reach `(row, col)`, we can come from three cells in the previous row:
- **Top:** `(row-1, col)` - moving down
- **Top-left:** `(row-1, col-1)` - moving diagonally down-right
- **Top-right:** `(row-1, col+1)` - moving diagonally down-left

**Recurrence Relation:**
```
f(row, col) = matrix[row][col] + min(
    f(row-1, col),      // from top
    f(row-1, col-1),    // from top-left
    f(row-1, col+1)     // from top-right
)
```

---

## Recursion Tree Example

Let's visualize for a 3×3 matrix starting from position (2,1):
```
Matrix:
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]

Recursion Tree for f(2,1) = finding min path from (2,1) to first row:
                              f(2,1)
                          /     |     \
                    f(1,0)   f(1,1)   f(1,2)
                   /  |  \   /  |  \   /  |  \
              f(0,*)         ...        f(0,*)
              
Detailed expansion of f(1,1):
                           f(1,1)
                        /    |    \
                   f(0,0) f(0,1) f(0,2)
                    [1]    [2]    [3]

Computing f(2,1):
- Path via f(1,0): 8 + (4 + min(∞, 1, 2)) = 8 + 5 = 13
- Path via f(1,1): 8 + (5 + min(1, 2, 3)) = 8 + 6 = 14
- Path via f(1,2): 8 + (6 + min(2, 3, ∞)) = 8 + 8 = 16

f(2,1) = min(13, 14, 16) = 13

Legend:
- [value] = base case (first row)
- ∞ = out of bounds
```

**Observations:**
- Multiple paths lead to the same subproblem
- Each cell in first row is computed multiple times
- Must try all starting positions in first row for final answer
- Time Complexity: **O(3^(m×n))** - exponential with three choices per cell

---

## Step 5: Recursive Solution

```python
def solve(matrix, row, col):
    n, m = len(matrix), len(matrix[0])
    
    # Out of bounds
    if row < 0 or col < 0 or row >= n or col >= m:
        return float('inf')
    
    # Base case: reached first row
    if row == 0:
        return matrix[row][col]
    
    # Recursive calls for three possible previous cells
    top = matrix[row][col] + solve(matrix, row-1, col)
    top_left = matrix[row][col] + solve(matrix, row-1, col-1)
    top_right = matrix[row][col] + solve(matrix, row-1, col+1)
    
    return min(top, top_left, top_right)

# Find minimum among all ending positions in last row
n, m = len(matrix), len(matrix[0])
ans = float('inf')
for i in range(m):
    ans = min(ans, solve(matrix, n-1, i))
print(ans)
```

**Complexity:**
- **Time:** O(3^(m×n)) - Exponential, three recursive calls per cell
- **Space:** O(m) - Recursion stack depth

---

## Step 6: Memoization (Top-Down DP)

**Optimization Strategy:** Store results of computed subproblems in a 2D array to avoid redundant calculations.

**How we convert Recursion → Memoization:**
1. Create a 2D array `memo` initialized with `None` (or `-1`) to indicate uncomputed states
2. Before computing `f(row, col)`, check if `memo[row][col]` is already computed
3. If yes, return the stored value immediately (O(1) lookup)
4. If no, compute the result recursively, store it in `memo[row][col]`, then return it
5. This ensures each subproblem is solved exactly once
6. We still need to try all starting positions in the last row

**Example:** In the recursion tree, cells like `f(1,1)` and `f(0,1)` are computed multiple times. With memoization, each is computed once and reused.

```python
def solve_memo(matrix, row, col, memo):
    n, m = len(matrix), len(matrix[0])
    
    # Out of bounds
    if row < 0 or col < 0 or row >= n or col >= m:
        return float('inf')
    
    # Base case: reached first row
    if row == 0:
        return matrix[row][col]
    
    # Check if already computed
    if memo[row][col] is not None:
        return memo[row][col]
    
    # Compute and store
    top = matrix[row][col] + solve_memo(matrix, row-1, col, memo)
    top_left = matrix[row][col] + solve_memo(matrix, row-1, col-1, memo)
    top_right = matrix[row][col] + solve_memo(matrix, row-1, col+1, memo)
    memo[row][col] = min(top, top_left, top_right)
    
    return memo[row][col]

# Find minimum among all ending positions
n, m = len(matrix), len(matrix[0])
memo = [[None]*m for _ in range(n)]
ans = float('inf')
for i in range(m):
    ans = min(ans, solve_memo(matrix, n-1, i, memo))
print(ans)
```

**Complexity:**
- **Time:** O(m×n) - Each cell computed once, try m ending positions
- **Space:** O(m×n) + O(m) - Memoization array + recursion stack

**Why this is better:** Each subproblem `f(row, col)` is solved exactly once. When multiple paths converge to the same cell, we retrieve the stored result instead of recomputing.

---

## Step 7: Tabulation (Bottom-Up DP)

**Optimization Strategy:** Build solution iteratively from base cases to eliminate recursion overhead.

**How we convert Memoization → Tabulation:**
1. Replace recursion with nested loops that iterate from base case to final answer
2. Start from the first row (base cases): `dp[0][j] = matrix[0][j]` for all columns
3. Fill the DP table row by row, moving downward (top to bottom)
4. Each cell `dp[i][j]` depends only on already computed cells in the previous row:
   - `dp[i-1][j]` (directly above)
   - `dp[i-1][j-1]` (top-left diagonal)
   - `dp[i-1][j+1]` (top-right diagonal)
5. Direction: Process rows from 1 to n-1, and within each row, process columns from 0 to m-1
6. Final answer: `min(dp[n-1])` - minimum value in the last row
7. No recursion means no call stack overhead and we naturally handle all starting positions

```python
def tabulation(matrix):
    n, m = len(matrix), len(matrix[0])
    
    # Create DP table
    dp = [[0]*m for _ in range(n)]
    
    # Base case: first row
    for col in range(m):
        dp[0][col] = matrix[0][col]
    
    # Fill DP table row by row
    for row in range(1, n):
        for col in range(m):
            top, top_left, top_right = float('inf'), float('inf'), float('inf')
            
            # From directly above
            if row-1 >= 0:
                top = matrix[row][col] + dp[row-1][col]
            
            # From top-left diagonal
            if row-1 >= 0 and col-1 >= 0:
                top_left = matrix[row][col] + dp[row-1][col-1]
            
            # From top-right diagonal
            if row-1 >= 0 and col+1 < m:
                top_right = matrix[row][col] + dp[row-1][col+1]
            
            dp[row][col] = min(top_left, top, top_right)
    
    # Return minimum value in last row
    return min(dp[n-1])
```

**Complexity:**
- **Time:** O(m×n) - Single pass through the matrix
- **Space:** O(m×n) - DP table

**Why this is better:** 
- No recursion stack overhead
- Processes all starting positions naturally by computing entire last row
- More intuitive to understand the flow from top to bottom
- Better cache locality for large matrices

---

## Step 8: Space Optimization

**Optimization Strategy:** Observe that we only need the previous row to compute the current row.

**How we convert Tabulation → Space Optimized:**
1. Analyze dependencies: `dp[i][j]` only needs values from `dp[i-1][j-1]`, `dp[i-1][j]`, and `dp[i-1][j+1]`
2. All three dependencies are from the previous row only
3. We don't need the entire 2D array - just two 1D arrays:
   - `dp_prev`: stores values from the previous row (row i-1)
   - `dp_curr`: stores values being computed for the current row (row i)
4. As we process each cell in the current row:
   - `dp_prev[col-1]` gives us the top-left diagonal value
   - `dp_prev[col]` gives us the top value
   - `dp_prev[col+1]` gives us the top-right diagonal value
5. After completing a row, copy `dp_curr` to `dp_prev` for the next iteration
6. This reduces space from O(m×n) to O(m)

**Visualization:**
```
Processing row i:
dp_prev: [val0, val1, val2, val3, ...]  ← row i-1
dp_curr: [computing row i...]           ← row i

At cell (i, j):
- top_left = dp_prev[j-1]
- top = dp_prev[j]
- top_right = dp_prev[j+1]
```

```python
def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    
    # Two arrays for previous and current row
    dp_curr = [0] * m
    dp_prev = [0] * m
    
    # Base case: first row
    for col in range(m):
        dp_prev[col] = matrix[0][col]
    
    # Process each row
    for row in range(1, n):
        for col in range(m):
            top, top_left, top_right = float('inf'), float('inf'), float('inf')
            
            # From directly above
            if row-1 >= 0:
                top = matrix[row][col] + dp_prev[col]
            
            # From top-left diagonal
            if row-1 >= 0 and col-1 >= 0:
                top_left = matrix[row][col] + dp_prev[col-1]
            
            # From top-right diagonal
            if row-1 >= 0 and col+1 < m:
                top_right = matrix[row][col] + dp_prev[col+1]
            
            dp_curr[col] = min(top_left, top, top_right)
        
        # Move current row to previous for next iteration
        dp_prev = dp_curr.copy()
    
    # Return minimum value in last row
    return min(dp_prev)
```

**Complexity:**
- **Time:** O(m×n) - Same iteration pattern
- **Space:** O(m) - Only two 1D arrays of size m

**Why this is better:** Achieves the same time complexity with minimal space usage (reduced from O(m×n) to O(m)). Critical for large matrices where memory is a constraint.

---

## Summary of Optimization Journey

| Approach | Time | Space | Key Improvement |
|----------|------|-------|-----------------|
| **Recursion** | O(3^(m×n)) | O(m) | Basic solution with three choices per cell, massive redundant calculations |
| **Memoization** | O(m×n) | O(m×n) + O(m) | Caches results to eliminate redundant subproblem calculations |
| **Tabulation** | O(m×n) | O(m×n) | Removes recursion overhead, builds solution bottom-up naturally |
| **Space Optimized** | O(m×n) | O(m) | Minimizes memory by keeping only previous row needed for computation |

**Progression Summary:**
1. **Recursion → Memoization:** Add memoization array to store computed results, avoiding recalculation of same subproblems
2. **Memoization → Tabulation:** Replace recursion with iteration, process from first row to last row, naturally handles all paths
3. **Tabulation → Space Optimized:** Identify that only previous row is needed, reduce 2D array to two 1D arrays

---

## Complete Code

```python
def solve(matrix, row, col):
    n, m = len(matrix), len(matrix[0])
    if row < 0 or col < 0 or row >= n or col >= m:
        return float('inf')
    if row == 0:
        return matrix[row][col]
    top = matrix[row][col] + solve(matrix, row-1, col)
    top_left = matrix[row][col] + solve(matrix, row-1, col-1)
    top_right = matrix[row][col] + solve(matrix, row-1, col+1)
    return min(top, top_left, top_right)

def solve_memo(matrix, row, col, memo):
    n, m = len(matrix), len(matrix[0])
    if row < 0 or col < 0 or row >= n or col >= m:
        return float('inf')
    if row == 0:
        return matrix[row][col]
    if memo[row][col] is not None:
        return memo[row][col]
    top = matrix[row][col] + solve_memo(matrix, row-1, col, memo)
    top_left = matrix[row][col] + solve_memo(matrix, row-1, col-1, memo)
    top_right = matrix[row][col] + solve_memo(matrix, row-1, col+1, memo)
    memo[row][col] = min(top, top_left, top_right)
    return memo[row][col]

def tabulation(matrix):
    n, m = len(matrix), len(matrix[0])
    dp = [[0]*m for _ in range(n)]
    for col in range(m):
        dp[0][col] = matrix[0][col]
    for row in range(1, n):
        for col in range(m):
            top, top_left, top_right = float('inf'), float('inf'), float('inf')
            if row-1 >= 0:
                top = matrix[row][col] + dp[row-1][col]
            if row-1 >= 0 and col-1 >= 0:
                top_left = matrix[row][col] + dp[row-1][col-1]
            if row-1 >= 0 and col+1 < m:
                top_right = matrix[row][col] + dp[row-1][col+1]
            dp[row][col] = min(top_left, top, top_right)
    return min(dp[n-1])

def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    dp_curr = [0]*m
    dp_prev = [0]*m
    for col in range(m):
        dp_prev[col] = matrix[0][col]
    for row in range(1, n):
        for col in range(m):
            top, top_left, top_right = float('inf'), float('inf'), float('inf')
            if row-1 >= 0:
                top = matrix[row][col] + dp_prev[col]
            if row-1 >= 0 and col-1 >= 0:
                top_left = matrix[row][col] + dp_prev[col-1]
            if row-1 >= 0 and col+1 < m:
                top_right = matrix[row][col] + dp_prev[col+1]
            dp_curr[col] = min(top_left, top, top_right)
        dp_prev = dp_curr.copy()
    return min(dp_prev)

# Test
matrix = [[1, 2, 10, 4], 
          [100, 3, 2, 1], 
          [1, 1, 20, 2], 
          [1, 2, 2, 1]]

n, m = len(matrix), len(matrix[0])

# Recursion
ans = float('inf')
for i in range(m):
    ans = min(ans, solve(matrix, n-1, i))
print(ans)  # 6

# Memoization
ans = float('inf')
for i in range(m):
    ans = min(ans, solve_memo(matrix, n-1, i, [[None]*m for _ in range(n)]))
print(ans)  # 6

# Tabulation
print(tabulation(matrix))  # 6

# Space Optimized
print(optimized(matrix))  # 6
```

---

## Usage

```python
# Define your matrix
matrix = [[1, 2, 10, 4], 
          [100, 3, 2, 1], 
          [1, 1, 20, 2], 
          [1, 2, 2, 1]]

# Choose the approach based on constraints
result = optimized(matrix)  # Recommended for most cases
print(f"Minimum falling path sum: {result}")
```

---

## Key Differences from Similar Problems

1. **Multiple Starting Points:** Unlike "Minimum Path Sum" which has a fixed start at (0,0), this problem allows starting from any cell in the first row
2. **Three Movement Directions:** Can move bottom, bottom-left, or bottom-right (diagonal movements allowed)
3. **Multiple Ending Points:** Can end at any cell in the last row, need to find minimum among all possibilities
4. **Solution Approach:** In tabulation, we naturally compute all paths by filling the entire DP table and take minimum of last row


---

# Triangle - Minimum Path Sum

## Problem Statement

Given a 2D integer array named **triangle** with n rows where:
- The first row has 1 element
- Each succeeding row has one more element than the row above it
- This forms a triangular structure

Return the minimum falling path sum from the first row to the last row.

Movement is allowed only to two directions from the current cell:
- **Bottom** (same column index)
- **Bottom-right** (column index + 1)

**Example 1:**
```
Input: triangle = [[1], 
                   [1, 2], 
                   [1, 2, 4]]

Output: 3

Explanation:
Triangle visualization:
    1
   1 2
  1 2 4

Optimal route: Start at 1st row → bottom → bottom
Path: 1 → 1 → 1
Sum: 1 + 1 + 1 = 3
```

**Example 2:**
```
Input: triangle = [[1], 
                   [4, 7], 
                   [4, 10, 50], 
                   [-50, 5, 6, -100]]

Output: -42

Explanation:
Triangle visualization:
      1
     4  7
   4 10 50
-50  5  6 -100

Optimal route: Start at 1st row → bottom-right → bottom-right → bottom-right
Path: 1 → 7 → 50 → -100
Sum: 1 + 7 + 50 + (-100) = -42
```

---

## Step 1: Define the Problem

Given a triangular array where:
- **Row 0** has 1 element
- **Row i** has (i+1) elements
- **n** = total number of rows

We need to:
- Start from the **top** of the triangle: `triangle[0][0]`
- Reach **any cell in the last row**: `triangle[n-1][j]` where `0 ≤ j < n`
- Move only **bottom** or **bottom-right**
- Find the path with **minimum sum** of all cell values along the path

**Unique Structure:** This is a triangular grid, not a rectangular matrix!

---

## Step 2: Represent the Problem Programmatically

We use a 2D array where row i has (i+1) elements.

Define: **f(row, col)** = minimum path sum from `triangle[row][col]` to the top of the triangle `(0, 0)`

**Goal:** Find `min(f(n-1, 0), f(n-1, 1), ..., f(n-1, n-1))` - minimum among all ending positions in the last row

**Alternative approach:** We can also think top-down from `(0, 0)` to last row.

---

## Step 3: Finding Base Cases

**Top of triangle:** If we're at the first row (the apex), the minimum path sum is just that value.
```
f(0, 0) = triangle[0][0]
```

**Out of bounds:** If row or column is invalid for the triangular structure:
```
f(row, col) = ∞  if row < 0 or col < 0 or col > row
```

**Key constraint:** In row i, valid column indices are 0 to i (inclusive).

---

## Step 4: Finding the Recurrence Relation

To reach `(row, col)` from the top, we can come from two cells in the previous row:
- **Top:** `(row-1, col)` - moving bottom (same column)
- **Top-left:** `(row-1, col-1)` - moving bottom-right

**Why these two?**
- If we're at column `col` in row `row`, we came from either:
  - Column `col` in row `row-1` (moved down)
  - Column `col-1` in row `row-1` (moved diagonally down-right)

**Recurrence Relation:**
```
f(row, col) = triangle[row][col] + min(
    f(row-1, col),      // from top (same column)
    f(row-1, col-1)     // from top-left (diagonal)
)
```

---

## Recursion Tree Example

Let's visualize for the triangle `[[1], [1, 2], [1, 2, 4]]`:

```
Triangle Structure:
    1       (0,0)
   1 2      (1,0) (1,1)
  1 2 4     (2,0) (2,1) (2,2)

Recursion Tree for f(2,1) = finding min path from (2,1) to apex:
                        f(2,1)
                       /      \
                  f(1,1)      f(1,0)
                 /     \      /     \
            f(0,1)  f(0,0) f(0,0) f(-1,0)
              |       |       |       |
             [∞]     [1]     [1]     [∞]

Computing values:
f(0,0) = 1 (base case)

f(1,0) = 1 + min(f(0,0), f(0,-1)) = 1 + min(1, ∞) = 2
f(1,1) = 2 + min(f(0,1), f(0,0)) = 2 + min(∞, 1) = 3

f(2,0) = 1 + min(f(1,0), f(1,-1)) = 1 + min(2, ∞) = 3
f(2,1) = 2 + min(f(1,1), f(1,0)) = 2 + min(3, 2) = 4
f(2,2) = 4 + min(f(1,2), f(1,1)) = 4 + min(∞, 3) = 7

Final answer: min(f(2,0), f(2,1), f(2,2)) = min(3, 4, 7) = 3

Legend:
- [value] = base case or computed value
- [∞] = out of bounds
- Notice f(0,0) and f(1,0) are computed multiple times!
```

**Observations:**
- Multiple paths lead to the same subproblem
- Cells like `f(0,0)` and `f(1,0)` are computed multiple times
- Must try all ending positions in the last row
- Time Complexity: **O(2^n)** - exponential with two choices per cell

---

## Step 5: Recursive Solution

```python
def solve(matrix, row, col):
    n, m = len(matrix), len(matrix[0])
    
    # Out of bounds (invalid for triangular structure)
    if row < 0 or col < 0 or col > row:
        return float('inf')
    
    # Base case: reached the apex
    if row == 0 and col == 0:
        return matrix[row][col]
    
    # Recursive calls
    top = matrix[row][col] + solve(matrix, row-1, col)
    top_left = matrix[row][col] + solve(matrix, row-1, col-1)
    
    return min(top, top_left)

# Find minimum among all ending positions in last row
n, m = len(matrix), len(matrix[-1])
ans = float('inf')
for i in range(m):
    ans = min(ans, solve(matrix, n-1, i))
print(ans)
```

**Complexity:**
- **Time:** O(2^n) - Exponential, two recursive calls per cell
- **Space:** O(n) - Recursion stack depth (maximum depth is n)

---

## Step 6: Memoization (Top-Down DP)

**Optimization Strategy:** Store results of computed subproblems in a 2D array to avoid redundant calculations.

**How we convert Recursion → Memoization:**
1. Create a 2D array `memo` where `memo[i]` has length `(i+1)` to match triangle structure
2. Initialize with `None` to indicate uncomputed states
3. Before computing `f(row, col)`, check if `memo[row][col]` is already computed
4. If yes, return the stored value immediately (O(1) lookup)
5. If no, compute recursively, store in `memo[row][col]`, then return it
6. This ensures each subproblem `(row, col)` is solved exactly once
7. We still try all ending positions in the last row

**Example:** In the recursion tree above, `f(0,0)` appears twice and `f(1,0)` appears twice. With memoization, each is computed once and the result is reused.

```python
def solve_memo(matrix, row, col, memo):
    n, m = len(matrix), len(matrix[0])
    
    # Out of bounds
    if row < 0 or col < 0 or col > row:
        return float('inf')
    
    # Base case: reached the apex
    if row == 0 and col == 0:
        return matrix[row][col]
    
    # Check if already computed
    if memo[row][col] is not None:
        return memo[row][col]
    
    # Compute and store
    top = matrix[row][col] + solve_memo(matrix, row-1, col, memo)
    top_left = matrix[row][col] + solve_memo(matrix, row-1, col-1, memo)
    memo[row][col] = min(top, top_left)
    
    return memo[row][col]

# Find minimum among all ending positions
n, m = len(matrix), len(matrix[-1])
memo = [[None]*(i+1) for i in range(n)]  # Triangular structure
ans = float('inf')
for i in range(m):
    ans = min(ans, solve_memo(matrix, n-1, i, memo))
print(ans)
```

**Complexity:**
- **Time:** O(n²) - Each of the n(n+1)/2 cells computed once
- **Space:** O(n²) + O(n) - Memoization array (triangular) + recursion stack

**Why this is better:** Each subproblem `f(row, col)` is solved exactly once. When multiple paths converge to the same cell (like `f(0,0)` in our example), we retrieve the stored result instead of recomputing.

---

## Step 7: Tabulation (Bottom-Up DP)

**Optimization Strategy:** Build solution iteratively from base cases to eliminate recursion overhead.

**How we convert Memoization → Tabulation:**
1. Replace recursion with nested loops that iterate from base case to final answer
2. Start from the apex (base case): `dp[0][0] = triangle[0][0]`
3. Fill the DP table row by row, moving downward (top to bottom)
4. Each cell `dp[i][j]` depends only on already computed cells in the previous row:
   - `dp[i-1][j]` (directly above, same column)
   - `dp[i-1][j-1]` (top-left diagonal)
5. Direction: Process rows from 1 to n-1, and within each row, process columns from 0 to row index
6. Each row has a different number of columns (triangular structure)
7. Final answer: `min(dp[n-1])` - minimum value in the last row
8. No recursion means no call stack overhead and naturally handles all paths

```python
def tabulation(matrix):
    n, m = len(matrix), len(matrix[0])
    
    # Create DP table with triangular structure
    dp = [[0]*(i+1) for i in range(n)]
    
    # Base case: apex of triangle
    dp[0][0] = matrix[0][0]
    
    # Fill DP table row by row
    for row in range(1, n):
        for col in range(row + 1):  # row + 1 elements in this row
            top, top_left = float('inf'), float('inf')
            
            # From directly above (same column)
            if row-1 >= 0 and col <= (row-1):
                top = matrix[row][col] + dp[row-1][col]
            
            # From top-left diagonal
            if row-1 >= 0 and col-1 >= 0 and (col-1) <= (row-1):
                top_left = matrix[row][col] + dp[row-1][col-1]
            
            dp[row][col] = min(top_left, top)
    
    # Return minimum value in last row
    return min(dp[n-1])
```

**Complexity:**
- **Time:** O(n²) - Process n(n+1)/2 cells in the triangle
- **Space:** O(n²) - DP table with triangular structure

**Why this is better:** 
- No recursion stack overhead
- Processes all ending positions naturally by computing entire last row
- More intuitive to understand the top-to-bottom flow
- Better cache locality

---

## Step 8: Space Optimization

**Optimization Strategy:** Observe that we only need the previous row to compute the current row.

**How we convert Tabulation → Space Optimized:**
1. Analyze dependencies: `dp[i][j]` only needs `dp[i-1][j]` and `dp[i-1][j-1]`
2. Both dependencies are from the previous row only
3. We don't need the entire 2D array - just two 1D arrays:
   - `dp_prev`: stores values from the previous row (row i-1)
   - `dp_curr`: stores values being computed for the current row (row i)
4. As we process each cell in the current row:
   - `dp_prev[col]` gives us the top value (same column)
   - `dp_prev[col-1]` gives us the top-left diagonal value
5. After completing a row, copy `dp_curr` to `dp_prev` for the next iteration
6. Array size can be the maximum row length (last row has n elements)
7. This reduces space from O(n²) to O(n)

**Visualization:**
```
Processing row i (which has i+1 elements):
dp_prev: [val0, val1, ..., val_i-1, ...]  ← row i-1
dp_curr: [computing row i...]              ← row i

At cell (i, j):
- top = dp_prev[j]      ← from same column
- top_left = dp_prev[j-1]  ← from diagonal
```

```python
def optimized(matrix):
    n, m = len(matrix), len(matrix[-1])
    
    # Two arrays for previous and current row
    dp_curr = [0] * m
    dp_prev = [0] * m
    
    # Base case: apex
    dp_prev[0] = matrix[0][0]
    
    # Process each row
    for row in range(1, n):
        for col in range(row + 1):  # row + 1 elements in this row
            top, top_left = float('inf'), float('inf')
            
            # From directly above (same column)
            if row-1 >= 0 and col <= row-1:
                top = matrix[row][col] + dp_prev[col]
            
            # From top-left diagonal
            if row-1 >= 0 and col-1 <= row-1 and col-1 >= 0:
                top_left = matrix[row][col] + dp_prev[col-1]
            
            dp_curr[col] = min(top_left, top)
        
        # Move current row to previous for next iteration
        dp_prev = dp_curr.copy()
    
    # Return minimum value in last row
    return min(dp_prev)
```

**Complexity:**
- **Time:** O(n²) - Same iteration pattern
- **Space:** O(n) - Only two 1D arrays of size n (last row length)

**Why this is better:** Achieves the same time complexity with minimal space usage. Space reduced from O(n²) to O(n), which is significant for large triangles.

---

## Summary of Optimization Journey

| Approach | Time | Space | Key Improvement |
|----------|------|-------|-----------------|
| **Recursion** | O(2^n) | O(n) | Basic solution with two choices per cell, massive redundant calculations |
| **Memoization** | O(n²) | O(n²) + O(n) | Caches results to eliminate redundant subproblem calculations |
| **Tabulation** | O(n²) | O(n²) | Removes recursion overhead, builds solution top-to-bottom naturally |
| **Space Optimized** | O(n²) | O(n) | Minimizes memory by keeping only previous row needed |

**Progression Summary:**
1. **Recursion → Memoization:** Add memoization array (triangular structure) to store computed results, avoiding recalculation
2. **Memoization → Tabulation:** Replace recursion with iteration from apex to base, naturally handles all ending positions
3. **Tabulation → Space Optimized:** Identify that only previous row is needed, reduce triangular 2D array to two 1D arrays

---

## Complete Code

```python
def solve(matrix, row, col):
    n, m = len(matrix), len(matrix[0])
    if row < 0 or col < 0 or col > row:
        return float('inf')
    if row == 0 and col == 0:
        return matrix[row][col]
    top = matrix[row][col] + solve(matrix, row-1, col)
    top_left = matrix[row][col] + solve(matrix, row-1, col-1)
    return min(top, top_left)

def solve_memo(matrix, row, col, memo):
    n, m = len(matrix), len(matrix[0])
    if row < 0 or col < 0 or col > row:
        return float('inf')
    if row == 0 and col == 0:
        return matrix[row][col]
    if memo[row][col] is not None:
        return memo[row][col]
    top = matrix[row][col] + solve_memo(matrix, row-1, col, memo)
    top_left = matrix[row][col] + solve_memo(matrix, row-1, col-1, memo)
    memo[row][col] = min(top, top_left)
    return memo[row][col]

def tabulation(matrix):
    n, m = len(matrix), len(matrix[0])
    dp = [[0]*(i+1) for i in range(n)]
    dp[0][0] = matrix[0][0]
    for row in range(1, n):
        for col in range(row + 1):
            top, top_left = float('inf'), float('inf')
            if row-1 >= 0 and col <= (row-1):
                top = matrix[row][col] + dp[row-1][col]
            if row-1 >= 0 and col-1 >= 0 and (col-1) <= (row-1):
                top_left = matrix[row][col] + dp[row-1][col-1]
            dp[row][col] = min(top_left, top)
    return min(dp[n-1])

def optimized(matrix):
    n, m = len(matrix), len(matrix[-1])
    dp_curr = [0]*m
    dp_prev = [0]*m
    dp_prev[0] = matrix[0][0]
    for row in range(1, n):
        for col in range(row + 1):
            top, top_left = float('inf'), float('inf')
            if row-1 >= 0 and col <= row-1:
                top = matrix[row][col] + dp_prev[col]
            if row-1 >= 0 and col-1 <= row-1 and col-1 >= 0:
                top_left = matrix[row][col] + dp_prev[col-1]
            dp_curr[col] = min(top_left, top)
        dp_prev = dp_curr.copy()
    return min(dp_prev)

# Test Example 1
matrix = [[1], [1, 2], [1, 2, 4]]
n, m = len(matrix), len(matrix[-1])

# Recursion
ans = float('inf')
for i in range(m):
    ans = min(ans, solve(matrix, n-1, i))
print(ans)  # 3

# Memoization
ans = float('inf')
for i in range(m):
    ans = min(ans, solve_memo(matrix, n-1, i, [[None]*(i+1) for i in range(n)]))
print(ans)  # 3

# Tabulation
print(tabulation(matrix))  # 3

# Space Optimized
print(optimized(matrix))  # 3

# Test Example 2
matrix2 = [[1], [4, 7], [4, 10, 50], [-50, 5, 6, -100]]
print(optimized(matrix2))  # -42
```

---

## Usage

```python
# Define your triangle
triangle = [[1], 
            [1, 2], 
            [1, 2, 4]]

# Choose the approach based on constraints
result = optimized(triangle)  # Recommended for most cases
print(f"Minimum path sum: {result}")
```

---

## Key Differences from Similar Problems

1. **Triangular Structure:** Unlike rectangular grids, row i has exactly (i+1) elements
2. **Fixed Starting Point:** Must start at the apex `(0, 0)`, unlike "Minimum Falling Path Sum" which allows any starting cell
3. **Two Movement Directions:** Can only move bottom or bottom-right (not three directions like falling path)
4. **Multiple Ending Points:** Can end at any cell in the last row
5. **Column Constraint:** Valid column indices at row i are 0 to i (must satisfy `col ≤ row`)
6. **Space Structure:** DP arrays need to handle variable row lengths, but can be optimized to use maximum length (last row)


---

# Cherry Pickup II

## Problem Statement

Given an `n x m` 2D integer array `matrix` where `matrix[i][j]` represents the number of cherries at cell `(i, j)`.

Two robots collect cherries:
- **Robot 1**: Starts at top-left `(0, 0)`
- **Robot 2**: Starts at top-right `(0, m-1)`

**Movement Rules:**
- From `(i, j)`, robots can move to `(i+1, j-1)`, `(i+1, j)`, or `(i+1, j+1)`
- Each robot collects cherries from cells it passes through
- If both robots are in the same cell, cherries are collected only once
- Both robots must reach the bottom row

**Goal:** Maximize total cherries collected by both robots.

---

## Examples

### Example 1
```
Input: matrix = [[2, 1, 3],
                 [4, 2, 5],
                 [1, 6, 2],
                 [7, 2, 8]]

Output: 37

Robot 1 path: (0,0)→(1,0)→(2,1)→(3,0) = 2+4+6+7 = 19
Robot 2 path: (0,2)→(1,2)→(2,2)→(3,2) = 3+5+2+8 = 18
Total = 37
```

### Example 2
```
Input: matrix = [[1, 4, 4, 1],
                 [1, 2, 2, 1],
                 [5, 6, 10, 11],
                 [8, 1, 1, 1]]

Output: 32

Robot 1 path: (0,0)→(1,1)→(2,1)→(3,0) = 1+2+6+8 = 17
Robot 2 path: (0,3)→(1,2)→(2,3)→(3,3) = 1+2+11+1 = 15
Total = 32
```

---

## Recursion Tree (Medium Example)

For matrix:
```
[[3, 1, 1],
 [2, 5, 1],
 [1, 5, 5]]
```

```
                    solve(0, 0, 2)
                    [3 + 1 = 4]
                         |
        +----------------+----------------+
        |                |                |
   solve(1,0,1)    solve(1,0,2)    solve(1,1,2)
   [2 + 5 = 7]     [2 + 1 = 3]     [5 + 1 = 6]
        |                |                |
    +---+---+        +---+---+        +---+---+
    |   |   |        |   |   |        |   |   |
  (2,0,0)(2,0,1)(2,0,2) ...          ...
   [1]  [1+5=6] [1+5=6]

Final: solve(0,0,2) tries all 9 combinations of moves
Each state: (row, col1, col2) explores 9 possibilities
Tree depth: n (number of rows)
Branches per node: 9 (3 moves for robot1 × 3 moves for robot2)
```

**Key Observations:**
- State space: `O(n × m × m)` unique states
- Without memoization: `O(9^n)` time complexity
- Many overlapping subproblems - same `(row, col1, col2)` computed multiple times

---

## Solution Approaches

## 1. Recursion (Brute Force)

**Approach:** Try all possible paths for both robots simultaneously.

**How it works:**
- Start from row 0 with robots at columns 0 and m-1
- At each step, try all 9 combinations of moves (3 for robot1 × 3 for robot2)
- Collect cherries from current position (once if same cell, twice if different)
- Recursively solve for next row
- Base case: reached bottom row

```python
def solve(row, col1, col2):
    # Base case: out of bounds
    if row >= n or col1 < 0 or col1 >= m or col2 < 0 or col2 >= m:
        return float('-inf')
    
    # Base case: reached last row
    if row == n - 1:
        if col1 == col2:
            return matrix[row][col1]
        else:
            return matrix[row][col1] + matrix[row][col2]
    
    max_points = float('-inf')
    
    # Try all 9 combinations of moves
    for i in range(-1, 2):  # Robot 1: -1, 0, 1
        for j in range(-1, 2):  # Robot 2: -1, 0, 1
            if col1 == col2:
                collected = matrix[row][col1] + solve(row + 1, col1 + i, col2 + j)
            else:
                collected = matrix[row][col1] + matrix[row][col2] + solve(row + 1, col1 + i, col2 + j)
            
            max_points = max(collected, max_points)
    
    return max_points

# Usage
matrix = [[2, 1, 3], [4, 2, 5], [1, 6, 2], [7, 2, 8]]
n, m = len(matrix), len(matrix[0])
print(solve(0, 0, m - 1))  # Output: 37
```

**Complexity:**
- Time: `O(9^n)` - exponential
- Space: `O(n)` - recursion stack

---

## 2. Memoization (Top-Down DP)

**Why memoization?** The recursion computes the same `(row, col1, col2)` state multiple times.

**Optimization:** Store computed results in a 3D array to avoid recomputation.

**Key Changes from Recursion:**
1. Added `memo` array of size `[n][m][m]`
2. Before computing, check if result already exists in memo
3. After computing, store result in memo
4. Same recursive logic, but each state computed only once

```python
def solve_memo(row, col1, col2, memo):
    # Base case: out of bounds
    if row >= n or col1 < 0 or col1 >= m or col2 < 0 or col2 >= m:
        return float('-inf')
    
    # Base case: reached last row
    if row == n - 1:
        if col1 == col2:
            return matrix[row][col1]
        else:
            return matrix[row][col1] + matrix[row][col2]
    
    # Check if already computed
    if memo[row][col1][col2] is not None:
        return memo[row][col1][col2]
    
    max_points = float('-inf')
    
    # Try all 9 combinations
    for i in range(-1, 2):
        for j in range(-1, 2):
            if col1 == col2:
                collected = matrix[row][col1] + solve_memo(row + 1, col1 + i, col2 + j, memo)
            else:
                collected = matrix[row][col1] + matrix[row][col2] + solve_memo(row + 1, col1 + i, col2 + j, memo)
            
            max_points = max(collected, max_points)
    
    # Store result
    memo[row][col1][col2] = max_points
    return max_points

# Usage
memo = [[[None] * m for _ in range(m)] for _ in range(n)]
print(solve_memo(0, 0, m - 1, memo))  # Output: 37
```

**Complexity:**
- Time: `O(n × m × m × 9)` = `O(n × m²)` - each state computed once
- Space: `O(n × m × m)` - memo array + `O(n)` recursion stack

---

## 3. Tabulation (Bottom-Up DP)

**Why tabulation?** Eliminate recursion overhead and stack space.

**Key Transformation:**
1. **Direction change:** Recursion goes top→bottom, tabulation goes bottom→top
2. **Initialize base case:** Start with last row (what recursion returns at base case)
3. **Fill table iteratively:** For each row from `n-2` to `0`, compute using already-filled next row
4. **No recursion:** Pure iterative approach using loops

**Step-by-step conversion:**
- Recursion base case (`row == n-1`) → Initialize `dp[n-1][j1][j2]`
- Recursive call `solve(row+1, ...)` → Access `dp[i+1][...]`
- Return from function → Store in `dp[i][j1][j2]`

```python
def tabulation(n, m, matrix):
    # Create 3D DP table
    dp = [[[0] * m for _ in range(m)] for _ in range(n)]
    
    # Initialize base case: last row
    for j1 in range(m):
        for j2 in range(m):
            if j1 == j2:
                dp[n - 1][j1][j2] = matrix[n - 1][j1]
            else:
                dp[n - 1][j1][j2] = matrix[n - 1][j1] + matrix[n - 1][j2]
    
    # Fill table from bottom to top
    for i in range(n - 2, -1, -1):
        for j1 in range(m):
            for j2 in range(m):
                maxi = float('-inf')
                
                # Try all 9 combinations
                for di in range(-1, 2):
                    for dj in range(-1, 2):
                        ans = 0
                        
                        if j1 == j2:
                            ans = matrix[i][j1]
                        else:
                            ans = matrix[i][j1] + matrix[i][j2]
                        
                        # Check if valid move
                        if (j1 + di < 0 or j1 + di >= m) or (j2 + dj < 0 or j2 + dj >= m):
                            ans += -1e9  # Invalid move
                        else:
                            ans += dp[i + 1][j1 + di][j2 + dj]
                        
                        maxi = max(ans, maxi)
                
                dp[i][j1][j2] = maxi
    
    # Answer is at starting position
    return dp[0][0][m - 1]

# Usage
print(tabulation(n, m, matrix))  # Output: 37
```

**Complexity:**
- Time: `O(n × m × m × 9)` = `O(n × m²)`
- Space: `O(n × m × m)` - only DP table, no recursion stack

---

## 4. Space Optimization

**Why optimize?** We only need the previous row to compute the current row.

**Key Insight:** 
- `dp[i][j1][j2]` only depends on `dp[i+1][...]`
- Don't need entire 3D array, just two 2D arrays
- `front` = previous row (i+1), `cur` = current row (i)

**Transformation:**
1. Replace `dp[n][m][m]` with two 2D arrays: `front[m][m]` and `cur[m][m]`
2. `dp[i+1][...]` becomes `front[...]`
3. `dp[i][...]` becomes `cur[...]`
4. After processing each row, copy `cur` to `front`

```python
import copy

def optimized(n, m, matrix):
    # Two 2D arrays instead of 3D
    front = [[0] * m for _ in range(m)]
    cur = [[0] * m for _ in range(m)]
    
    # Initialize base case: last row
    for j1 in range(m):
        for j2 in range(m):
            if j1 == j2:
                front[j1][j2] = matrix[n - 1][j1]
            else:
                front[j1][j2] = matrix[n - 1][j1] + matrix[n - 1][j2]
    
    # Fill from bottom to top
    for i in range(n - 2, -1, -1):
        for j1 in range(m):
            for j2 in range(m):
                maxi = float('-inf')
                
                # Try all 9 combinations
                for di in [-1, 0, 1]:
                    for dj in [-1, 0, 1]:
                        if 0 <= j1 + di < m and 0 <= j2 + dj < m:
                            ans = matrix[i][j1]
                            if j1 != j2:
                                ans += matrix[i][j2]
                            ans += front[j1 + di][j2 + dj]
                        else:
                            ans = -1e9  # Invalid move
                        
                        maxi = max(maxi, ans)
                
                cur[j1][j2] = maxi
        
        # Move current to front for next iteration
        front = copy.deepcopy(cur)
    
    return front[0][m - 1]

# Usage
print(optimized(n, m, matrix))  # Output: 37
```

**Complexity:**
- Time: `O(n × m × m × 9)` = `O(n × m²)`
- Space: `O(m × m)` = `O(m²)` - only two 2D arrays

---

## Complexity Comparison

| Approach | Time | Space | Stack Space |
|----------|------|-------|-------------|
| Recursion | O(9^n) | O(n) | O(n) |
| Memoization | O(n×m²) | O(n×m²) | O(n) |
| Tabulation | O(n×m²) | O(n×m²) | O(1) |
| Optimized | O(n×m²) | O(m²) | O(1) |

---

## Complete Working Code

```python
def solve(row, col1, col2):
    if row >= n or col1 < 0 or col1 >= m or col2 < 0 or col2 >= m:
        return float('-inf')
    if row == n - 1:
        if col1 == col2:
            return matrix[row][col1]
        else:
            return matrix[row][col1] + matrix[row][col2]
    max_points = float('-inf')
    for i in range(-1, 2):
        for j in range(-1, 2):
            if col1 == col2:
                collected = matrix[row][col1] + solve(row + 1, col1 + i, col2 + j)
            else:
                collected = matrix[row][col1] + matrix[row][col2] + solve(row + 1, col1 + i, col2 + j)
            max_points = max(collected, max_points)
    return max_points


def solve_memo(row, col1, col2, memo):
    if row >= n or col1 < 0 or col1 >= m or col2 < 0 or col2 >= m:
        return float('-inf')
    if row == n - 1:
        if col1 == col2:
            return matrix[row][col1]
        else:
            return matrix[row][col1] + matrix[row][col2]
    if memo[row][col1][col2] is not None:
        return memo[row][col1][col2]
    max_points = float('-inf')
    for i in range(-1, 2):
        for j in range(-1, 2):
            if col1 == col2:
                collected = matrix[row][col1] + solve_memo(row + 1, col1 + i, col2 + j, memo)
            else:
                collected = matrix[row][col1] + matrix[row][col2] + solve_memo(row + 1, col1 + i, col2 + j, memo)
            max_points = max(collected, max_points)
    memo[row][col1][col2] = max_points
    return max_points


def tabulation(n, m, matrix):
    dp = [[[0] * m for _ in range(m)] for _ in range(n)]
    for j1 in range(m):
        for j2 in range(m):
            if j1 == j2:
                dp[n - 1][j1][j2] = matrix[n - 1][j1]
            else:
                dp[n - 1][j1][j2] = matrix[n - 1][j1] + matrix[n - 1][j2]
    for i in range(n - 2, -1, -1):
        for j1 in range(m):
            for j2 in range(m):
                maxi = float('-inf')
                for di in range(-1, 2):
                    for dj in range(-1, 2):
                        ans = 0
                        if j1 == j2:
                            ans = matrix[i][j1]
                        else:
                            ans = matrix[i][j1] + matrix[i][j2]
                        if (j1 + di < 0 or j1 + di >= m) or (j2 + dj < 0 or j2 + dj >= m):
                            ans += -1e9
                        else:
                            ans += dp[i + 1][j1 + di][j2 + dj]
                        maxi = max(ans, maxi)
                dp[i][j1][j2] = maxi
    return dp[0][0][m - 1]


import copy

def optimized(n, m, matrix):
    front = [[0] * m for _ in range(m)]
    cur = [[0] * m for _ in range(m)]
    for j1 in range(m):
        for j2 in range(m):
            if j1 == j2:
                front[j1][j2] = matrix[n - 1][j1]
            else:
                front[j1][j2] = matrix[n - 1][j1] + matrix[n - 1][j2]
    for i in range(n - 2, -1, -1):
        for j1 in range(m):
            for j2 in range(m):
                maxi = float('-inf')
                for di in [-1, 0, 1]:
                    for dj in [-1, 0, 1]:
                        if 0 <= j1 + di < m and 0 <= j2 + dj < m:
                            ans = matrix[i][j1]
                            if j1 != j2:
                                ans += matrix[i][j2]
                            ans += front[j1 + di][j2 + dj]
                        else:
                            ans = -1e9
                        maxi = max(maxi, ans)
                cur[j1][j2] = maxi
        front = copy.deepcopy(cur)
    return front[0][m - 1]


# Test
matrix = [[2, 1, 3], [4, 2, 5], [1, 6, 2], [7, 2, 8]]
n, m = len(matrix), len(matrix[0])
print("Recursion:", solve(0, 0, m - 1))
memo = [[[None] * m for _ in range(m)] for _ in range(n)]
print("Memoization:", solve_memo(0, 0, m - 1, memo))
print("Tabulation:", tabulation(n, m, matrix))
print("Optimized:", optimized(n, m, matrix))
```

**Output:**
```
Recursion: 37
Memoization: 37
Tabulation: 37
Optimized: 37
```
