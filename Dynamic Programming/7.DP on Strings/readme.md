# Longest Common Subsequence (LCS)

## Problem Statement

Given two strings `str1` and `str2`, find the length of their longest common subsequence.

A **subsequence** is a sequence that appears in the same relative order but not necessarily contiguous. A **common subsequence** of two strings is a subsequence that is common to both strings.

### Examples

**Example 1:**
```
Input: str1 = "bdefg", str2 = "bfg"
Output: 3
Explanation: The longest common subsequence is "bfg", which has a length of 3.
```

**Example 2:**
```
Input: str1 = "mnop", str2 = "mnq"
Output: 2
Explanation: The longest common subsequence is "mn", which has a length of 2.
```

---

## Problem Understanding

Given two strings `s1` and `s2` with lengths `n1` and `n2`:
- `s1` has a set of subsequences
- `s2` has a set of subsequences
- We need to find the **common subsequence** present in both strings
- There may be multiple common subsequences
- We need to find the **length of the longest** common subsequence

**Example:**
```
s1 = "abc"
s2 = "ac"

Subsequences of "abc": [a, b, c, ab, bc, ac, abc]
Subsequences of "ac": [a, c, ac]

Common subsequences: [a, c, ac]
Longest common subsequence: "ac"
Length of longest: 2
```

---

## Problem Representation

Both strings can be represented by indices: `0...n1-1` and `0...n2-1`

Let `f(index1, index2)` represent the length of the longest common subsequence in:
- `s1[0...index1]` 
- `s2[0...index2]`

---

## Base Cases

If either string has no characters, there are no subsequences, hence no common subsequence.

```
f(index1 < 0 OR index2 < 0) = 0
```

---

## Recurrence Relation

Since we need the length of the common subsequence in two strings, we compare characters of both strings one by one. Since the subsequence must maintain order, we move in the same direction while iterating.

### Case 1: Characters Match
If both characters match, we found a common character:
```
if s1[index1] == s2[index2]:
    f(index1, index2) = 1 + f(index1-1, index2-1)
```

### Case 2: Characters Don't Match
We explore two possibilities:
1. Exclude the last character of `s1` and compare again: `f(index1-1, index2)`
2. Exclude the last character of `s2` and compare again: `f(index1, index2-1)`

Pick the longer subsequence:
```
if s1[index1] != s2[index2]:
    f(index1, index2) = max(f(index1-1, index2), f(index1, index2-1))
```

### Complete Recurrence Relation
```python
if s1[i] == s2[j]:
    LCS[i][j] = 1 + LCS[i-1][j-1]
else:
    LCS[i][j] = max(LCS[i-1][j], LCS[i][j-1])
```

---

## Recursion Tree

For `s1 = "abc"` and `s2 = "ac"`:

```
                          f(2,1) [c vs c]
                               |
                          1 + f(1,0)
                               |
                          f(1,0) [b vs a]
                          /          \
                    f(0,0)          f(1,-1)
                [a vs a]              |
                    |                 0
               1 + f(-1,-1)
                    |
                    0
                    
Result: 1 + 1 = 2
```

**Explanation:**
- At `f(2,1)`: `s1[2]='c'` matches `s2[1]='c'` → `1 + f(1,0)`
- At `f(1,0)`: `s1[1]='b'` doesn't match `s2[0]='a'` → `max(f(0,0), f(1,-1))`
- At `f(0,0)`: `s1[0]='a'` matches `s2[0]='a'` → `1 + f(-1,-1) = 1`
- At `f(1,-1)`: Base case → `0`
- Result: `max(1, 0) = 1`, then final answer: `1 + 1 = 2`

---

## Solution Approaches

### 1. Recursion (Top-Down)

```python
def solve(s1, s2, index1, index2):
    # Base case: if either string is exhausted
    if index1 < 0 or index2 < 0:
        return 0

    # If characters match, include this character
    if s1[index1] == s2[index2]:
        return 1 + solve(s1, s2, index1-1, index2-1)
    else:
        # If characters don't match, try both possibilities
        exclude1 = solve(s1, s2, index1-1, index2)
        exclude2 = solve(s1, s2, index1, index2-1)
        return max(exclude1, exclude2)
```

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion:**
1. Identify overlapping subproblems (same `index1` and `index2` are computed multiple times)
2. Create a 2D memoization table `memo[index1][index2]`
3. Before computing, check if the result is already stored
4. After computing, store the result in the memo table

```python
def solve_memo(s1, s2, index1, index2, memo):
    # Base case: if either string is exhausted
    if index1 < 0 or index2 < 0:
        return 0
    
    # Check if already computed
    if memo[index1][index2] != None:
        return memo[index1][index2]
    
    # If characters match
    if s1[index1] == s2[index2]:
        memo[index1][index2] = 1 + solve_memo(s1, s2, index1-1, index2-1, memo)
    else:
        # If characters don't match
        exclude1 = solve_memo(s1, s2, index1-1, index2, memo)
        exclude2 = solve_memo(s1, s2, index1, index2-1, memo)
        memo[index1][index2] = max(exclude1, exclude2)
    
    return memo[index1][index2]
```

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization:**
1. Convert recursive calls to iterative loops
2. Use 1-based indexing to handle base cases (empty strings at index 0)
3. Fill the DP table from smallest subproblems to largest
4. Initialize base cases: `dp[i][0] = 0` and `dp[0][j] = 0`

```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    # Create DP table with 1-based indexing
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base case: empty s1
    for index1 in range(n1+1):
        dp[index1][0] = 0
    
    # Base case: empty s2
    for index2 in range(n2+1):
        dp[0][index2] = 0

    # Fill the DP table
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp[index1][index2] = 1 + dp[index1-1][index2-1]
            else:
                exclude1 = dp[index1-1][index2]
                exclude2 = dp[index1][index2-1]
                dp[index1][index2] = max(exclude1, exclude2)
    
    return dp[n1][n2]
```

#### Tabulation Example

For `s1 = "bfg"` and `s2 = "bg"`:

|     | ε | b | g |
|-----|---|---|---|
| **ε** | 0 | 0 | 0 |
| **b** | 0 | 1 | 1 |
| **f** | 0 | 1 | 1 |
| **g** | 0 | 1 | 2 |

**Step-by-step filling:**
- `dp[0][*]` and `dp[*][0]` are 0 (empty string)
- `dp[1][1]`: `s1[0]='b'` == `s2[0]='b'` → `1 + dp[0][0] = 1`
- `dp[1][2]`: `s1[0]='b'` != `s2[1]='g'` → `max(dp[0][2], dp[1][1]) = 1`
- `dp[2][1]`: `s1[1]='f'` != `s2[0]='b'` → `max(dp[1][1], dp[2][0]) = 1`
- `dp[2][2]`: `s1[1]='f'` != `s2[1]='g'` → `max(dp[1][2], dp[2][1]) = 1`
- `dp[3][1]`: `s1[2]='g'` != `s2[0]='b'` → `max(dp[2][1], dp[3][0]) = 1`
- `dp[3][2]`: `s1[2]='g'` == `s2[1]='g'` → `1 + dp[2][1] = 2`

**Result:** `dp[3][2] = 2`

---

### 4. Space Optimized (Bottom-Up DP)

**Conversion from Tabulation:**
1. Observe that we only need the previous row (`dp[index1-1]`) and current row (`dp[index1]`)
2. Use two 1D arrays instead of a 2D array
3. `dp_prev` stores the previous row
4. `dp_curr` stores the current row being computed
5. After each row, copy `dp_curr` to `dp_prev`

```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    # Use two arrays instead of 2D table
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)

    # Initialize base cases
    dp_curr[0] = 0
    dp_prev[0] = 0
    for index2 in range(n2+1):
        dp_prev[index2] = 0

    # Fill row by row
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp_curr[index2] = 1 + dp_prev[index2-1]
            else:
                exclude1 = dp_prev[index2]
                exclude2 = dp_curr[index2-1]
                dp_curr[index2] = max(exclude1, exclude2)
        # Move current row to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[n2]
```

---

## Complete Code

```python
def solve(s1, s2, index1, index2):
    if index1 < 0 or index2 < 0:
        return 0

    if s1[index1] == s2[index2]:
        return 1 + solve(s1, s2, index1-1, index2-1)
    else:
        exclude1 = solve(s1, s2, index1-1, index2)
        exclude2 = solve(s1, s2, index1, index2-1)
        return max(exclude1, exclude2)

def solve_memo(s1, s2, index1, index2, memo):
    if index1 < 0 or index2 < 0:
        return 0
    if memo[index1][index2] != None:
        return memo[index1][index2]
    if s1[index1] == s2[index2]:
        memo[index1][index2] = 1 + solve_memo(s1, s2, index1-1, index2-1, memo)
    else:
        exclude1 = solve_memo(s1, s2, index1-1, index2, memo)
        exclude2 = solve_memo(s1, s2, index1, index2-1, memo)
        memo[index1][index2] = max(exclude1, exclude2)
    return memo[index1][index2]

def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    for index1 in range(n1+1):
        dp[index1][0] = 0
    for index2 in range(n2+1):
        dp[0][index2] = 0

    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp[index1][index2] = 1 + dp[index1-1][index2-1]
            else:
                exclude1 = dp[index1-1][index2]
                exclude2 = dp[index1][index2-1]
                dp[index1][index2] = max(exclude1, exclude2)
    return dp[n1][n2]

def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)

    dp_curr[0] = 0
    dp_prev[0] = 0
    for index2 in range(n2+1):
        dp_prev[index2] = 0

    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp_curr[index2] = 1 + dp_prev[index2-1]
            else:
                exclude1 = dp_prev[index2]
                exclude2 = dp_curr[index2-1]
                dp_curr[index2] = max(exclude1, exclude2)
        dp_prev = dp_curr.copy()
    return dp_prev[n2]

# Test
s1, s2 = "bdefg", "bfg"
n1, n2 = len(s1), len(s2)
print(solve(s1, s2, n1-1, n2-1))
memo = [[None]*n2 for _ in range(n1)]
print(solve_memo(s1, s2, n1-1, n2-1, memo))
print(tabulation(s1, s2))
print(optimized(s1, s2))
```

---

## Complexity Analysis

### Time Complexity

| Approach | Time Complexity | Explanation |
|----------|----------------|-------------|
| **Recursion** | O(2^(n1+n2)) | Each call branches into two recursive calls, creating exponential growth |
| **Memoization** | O(n1 × n2) | Each subproblem (index1, index2) is computed once and stored |
| **Tabulation** | O(n1 × n2) | Two nested loops iterate through all combinations |
| **Space Optimized** | O(n1 × n2) | Time complexity remains the same as tabulation |

### Space Complexity

| Approach | Space Complexity | Explanation |
|----------|-----------------|-------------|
| **Recursion** | O(n1 + n2) | Recursion stack depth is at most n1 + n2 |
| **Memoization** | O(n1 × n2) + O(n1 + n2) | 2D memo table + recursion stack |
| **Tabulation** | O(n1 × n2) | 2D DP table of size (n1+1) × (n2+1) |
| **Space Optimized** | O(n2) | Only two 1D arrays of size (n2+1) |

---

# Print Longest Common Subsequence (LCS)

## Problem Statement

Given two strings `str1` and `str2`, print the longest common subsequence of the two strings.

A **subsequence** of a string is a list of characters of the string where zero or more characters are deleted and they should be in the same order in the subsequence as in the original string.

### Examples

**Example 1:**
```
Input: str1 = "abcd", str2 = "bdef"
Output: "bd"
Explanation: LCS of two strings is "bd".
```

**Example 2:**
```
Input: str1 = "apple", str2 = "waffle"
Output: "ale"
Explanation: LCS of two strings is "ale".
```

---

## Approach

To find and print the Longest Common Subsequence, we follow a two-step process:

### Step 1: Build the DP Table

First, we need to create a 2D DP table where each cell at position `(i, j)` stores the length of the LCS between the prefixes of the two strings up to indices `i` and `j`.

We use the same approach as finding the length of LCS (tabulation method).

### Step 2: Backtrack to Reconstruct the LCS

Once the DP table is completely filled, we reconstruct the actual LCS string by backtracking:

1. **Start from the bottom-right corner** `(n, m)` of the table, where `n` and `m` are the lengths of the two strings.

2. **Compare characters** at positions `i-1` and `j-1`:
   - **If they match**: This character is part of the LCS
     - Add this character to the result
     - Move diagonally up-left to `(i-1, j-1)`
   
   - **If they don't match**: Move in the direction of the larger DP value
     - If `dp[i-1][j] > dp[i][j-1]`: Move up to `(i-1, j)`
     - Otherwise: Move left to `(i, j-1)`

3. **Continue** until you reach the top row or leftmost column (`i == 0` or `j == 0`)

4. **Reverse** the collected characters since reconstruction starts from the end

---

## Backtracking Visualization

For `s1 = "abcd"` and `s2 = "bdef"`:

### DP Table:
|     | ε | b | d | e | f |
|-----|---|---|---|---|---|
| **ε** | 0 | 0 | 0 | 0 | 0 |
| **a** | 0 | 0 | 0 | 0 | 0 |
| **b** | 0 | 1 | 1 | 1 | 1 |
| **c** | 0 | 1 | 1 | 1 | 1 |
| **d** | 0 | 1 | 2 | 2 | 2 |

### Backtracking Path:
```
Start at (4, 4): dp[4][4] = 2
├─ s1[3]='d' == s2[3]='f'? NO
├─ dp[3][4]=1 vs dp[4][3]=2 → Move left to (4, 3)
│
At (4, 3): dp[4][3] = 2
├─ s1[3]='d' == s2[2]='e'? NO
├─ dp[3][3]=1 vs dp[4][2]=2 → Move left to (4, 2)
│
At (4, 2): dp[4][2] = 2
├─ s1[3]='d' == s2[1]='d'? YES → Add 'd'
├─ Move diagonally to (3, 1)
│
At (3, 1): dp[3][1] = 1
├─ s1[2]='c' == s2[0]='b'? NO
├─ dp[2][1]=1 vs dp[3][0]=0 → Move up to (2, 1)
│
At (2, 1): dp[2][1] = 1
├─ s1[1]='b' == s2[0]='b'? YES → Add 'b'
├─ Move diagonally to (1, 0)
│
At (1, 0): STOP (reached boundary)

Collected characters (backwards): ['d', 'b']
Reversed result: "bd"
```

---

## Algorithm Steps

1. **Build DP Table**:
   - Use tabulation to fill the 2D DP table
   - `dp[i][j]` = length of LCS of `s1[0...i-1]` and `s2[0...j-1]`

2. **Initialize Backtracking**:
   - Start from `index1 = n1`, `index2 = n2`
   - Create empty result list

3. **Backtrack Loop** (while both indices > 0):
   - **If characters match**: 
     - Add character to result
     - Move diagonally: `index1--`, `index2--`
   - **If characters don't match**:
     - Compare `dp[index1-1][index2]` and `dp[index1][index2-1]`
     - Move towards the larger value

4. **Reverse and Return**:
   - Reverse the collected characters
   - Join to form the final LCS string

---

## Code Implementation

```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    # Create DP table with 1-based indexing
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base case: empty s1
    for index1 in range(n1+1):
        dp[index1][0] = 0
    
    # Base case: empty s2
    for index2 in range(n2+1):
        dp[0][index2] = 0
    
    # Fill the DP table
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp[index1][index2] = 1 + dp[index1-1][index2-1]
            else:
                exclude1 = dp[index1-1][index2]
                exclude2 = dp[index1][index2-1]
                dp[index1][index2] = max(exclude1, exclude2)
    
    return dp

def solve(s1, s2):
    n1, n2 = len(s1), len(s2)
    
    # Step 1: Build the DP table
    dp = tabulation(s1, s2)
    
    # Step 2: Backtrack to find the LCS string
    ans = []
    index1, index2 = n1, n2
    
    while index1 > 0 and index2 > 0:
        # If characters match, it's part of LCS
        if s1[index1-1] == s2[index2-1]:
            ans.append(s1[index1-1])
            index1 -= 1
            index2 -= 1
        else:
            # Move in direction of larger DP value
            if dp[index1-1][index2] > dp[index1][index2-1]:
                index1 -= 1
            else:
                index2 -= 1
    
    # Reverse the result as we built it backwards
    ans = "".join(ans[::-1])
    return ans

# Test
s1, s2 = "bdefg", "bfg"
print(solve(s1, s2))
```

---

## Detailed Example Walkthrough

Let's trace through `s1 = "bdefg"` and `s2 = "bfg"`:

### Step 1: DP Table Construction

|     | ε | b | f | g |
|-----|---|---|---|---|
| **ε** | 0 | 0 | 0 | 0 |
| **b** | 0 | 1 | 1 | 1 |
| **d** | 0 | 1 | 1 | 1 |
| **e** | 0 | 1 | 1 | 1 |
| **f** | 0 | 1 | 2 | 2 |
| **g** | 0 | 1 | 2 | 3 |

LCS Length: `dp[5][3] = 3`

### Step 2: Backtracking

| Step | Position | s1[i-1] | s2[j-1] | Match? | Action | Collected |
|------|----------|---------|---------|--------|--------|-----------|
| 1 | (5, 3) | g | g | ✓ | Add 'g', go to (4,2) | ['g'] |
| 2 | (4, 3) | f | g | ✗ | dp[3][3]=1 < dp[4][2]=2, go to (4,2) | ['g'] |
| 3 | (4, 2) | f | f | ✓ | Add 'f', go to (3,1) | ['g', 'f'] |
| 4 | (3, 1) | e | b | ✗ | dp[2][1]=1 = dp[3][0]=0, go to (2,1) | ['g', 'f'] |
| 5 | (2, 1) | d | b | ✗ | dp[1][1]=1 > dp[2][0]=0, go to (1,1) | ['g', 'f'] |
| 6 | (1, 1) | b | b | ✓ | Add 'b', go to (0,0) | ['g', 'f', 'b'] |
| 7 | (0, 0) | - | - | - | Stop | ['g', 'f', 'b'] |

**Collected (backwards)**: ['g', 'f', 'b']  
**Reversed**: "bfg"  
**Output**: "bfg"

---

## Why Backtracking Works

The DP table stores the optimal solution at each step. When we backtrack:

1. **Matching characters** must be part of the LCS because:
   - They contribute to the optimal length
   - They maintain the relative order in both strings

2. **Moving towards larger values** ensures we follow the path that led to the optimal solution:
   - If `dp[i-1][j]` is larger, the optimal solution came from excluding `s1[i-1]`
   - If `dp[i][j-1]` is larger, the optimal solution came from excluding `s2[j-1]`

3. **Building backwards** is natural because we start from the final result and work our way to the beginning

---

## Complexity Analysis

### Time Complexity: **O(n1 × n2)**
- Building the DP table: O(n1 × n2)
- Backtracking: O(n1 + n2) in the worst case
- Total: O(n1 × n2)

### Space Complexity: **O(n1 × n2)**
- DP table: O(n1 × n2)
- Result string: O(min(n1, n2)) - at most the length of the shorter string
- Total: O(n1 × n2)

---

## Alternative: Space-Optimized with Reconstruction

While we can optimize the DP table to O(n2) space for finding the length, reconstructing the actual LCS string requires the full 2D table. This is because we need to trace back through multiple cells to determine which characters are part of the LCS.

**Note**: If only the length is needed, space optimization is possible. For printing the actual subsequence, the full DP table is necessary.

---

# Longest Common Substring

## Problem Statement

Given two strings `str1` and `str2`, find the length of their longest common substring.

A **substring** is a contiguous sequence of characters within a string.

### Examples

**Example 1:**
```
Input: str1 = "abcde", str2 = "abfce"
Output: 2
Explanation: The longest common substring is "ab", which has a length of 2.
```

**Example 2:**
```
Input: str1 = "abcdxyz", str2 = "xyzabcd"
Output: 4
Explanation: The longest common substring is "abcd", which has a length of 4.
```

---

## Problem Understanding

Given two strings `s1` and `s2`:
- `s1` has a set of substrings
- `s2` has a set of substrings
- We need to find **common substrings** in both sets
- Return the **length of the longest** common substring

**Key Difference from Longest Common Subsequence (LCS):**
- **Subsequence**: Characters can be non-contiguous (order matters)
- **Substring**: Characters must be contiguous (adjacent)

**Example:**
```
s1 = "abcde"
s2 = "abfce"

Substrings of s1: [a, b, c, d, e, ab, bc, cd, de, abc, bcd, cde, abcd, bcde, abcde]
Substrings of s2: [a, b, f, c, e, ab, bf, fc, ce, abf, bfc, fce, abfc, bfce, abfce]

Common substrings: [a, b, c, e, ab]
Longest common substring: "ab"
Length: 2
```

---

## Problem Representation

Both strings can be represented by indices: `0...n1-1` and `0...n2-1`

Let `f(index1, index2)` represent the length of the longest common substring **ending at** `s1[index1]` and `s2[index2]`.

**Important**: Unlike LCS (subsequence), this function represents the length of the common substring that **must end at these specific positions**.

---

## Base Cases

If either string has no characters, there are no substrings.

```
f(index1 < 0 OR index2 < 0) = 0
```

---

## Recurrence Relation

Since we need the length of a common **contiguous** substring, we compare characters one by one.

### Case 1: Characters Match
If both characters match, they extend the current substring:
```
if s1[index1] == s2[index2]:
    f(index1, index2) = 1 + f(index1-1, index2-1)
```

### Case 2: Characters Don't Match
If characters don't match, the substring **ends here**, so the length becomes 0:
```
if s1[index1] != s2[index2]:
    f(index1, index2) = 0
```

**Note**: We don't have the `max(f(index1-1, index2), f(index1, index2-1))` option like in LCS because substrings must be contiguous.

### Complete Recurrence Relation
```python
if s1[i] == s2[j]:
    dp[i][j] = 1 + dp[i-1][j-1]
else:
    dp[i][j] = 0
```

### Tracking the Maximum
Since `dp[i][j]` only stores the substring length ending at position `(i, j)`, we need to track the maximum value across all cells:
```python
ans = max(ans, dp[i][j])
```

---

## Key Differences from LCS (Subsequence)

| Aspect | LCS (Subsequence) | LCS (Substring) |
|--------|-------------------|-----------------|
| **Contiguity** | Non-contiguous (can skip characters) | Contiguous (adjacent characters only) |
| **DP Meaning** | `dp[i][j]` = LCS length in prefixes `s1[0..i]` and `s2[0..j]` | `dp[i][j]` = substring length **ending at** `s1[i]` and `s2[j]` |
| **Mismatch Case** | `max(dp[i-1][j], dp[i][j-1])` | `0` (reset) |
| **Final Answer** | `dp[n1][n2]` (bottom-right corner) | `max(all dp values)` |

---

## Visualization

For `s1 = "abcde"` and `s2 = "abfce"`:

### DP Table:
|     | ε | a | b | f | c | e |
|-----|---|---|---|---|---|---|
| **ε** | 0 | 0 | 0 | 0 | 0 | 0 |
| **a** | 0 | **1** | 0 | 0 | 0 | 0 |
| **b** | 0 | 0 | **2** | 0 | 0 | 0 |
| **c** | 0 | 0 | 0 | 0 | 1 | 0 |
| **d** | 0 | 0 | 0 | 0 | 0 | 0 |
| **e** | 0 | 0 | 0 | 0 | 0 | 1 |

**Explanation:**
- At `(1,1)`: 'a' == 'a' → `1 + dp[0][0] = 1`
- At `(2,2)`: 'b' == 'b' → `1 + dp[1][1] = 2` ✓ **Maximum!**
- At `(2,3)`: 'b' != 'f' → `0` (substring breaks)
- At `(3,4)`: 'c' == 'c' → `1 + dp[2][3] = 1`

**Maximum value**: `2`  
**Longest common substring**: "ab"

---

## Solution Approaches

### 1. Tabulation (Bottom-Up DP)

```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    # Create DP table with 1-based indexing
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base case: empty s1
    for index1 in range(n1+1):
        dp[index1][0] = 0
    
    # Base case: empty s2
    for index2 in range(n2+1):
        dp[0][index2] = 0
    
    ans = 0  # Track maximum substring length
    
    # Fill the DP table
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # Characters match: extend the substring
                dp[index1][index2] = 1 + dp[index1-1][index2-1]
                ans = max(ans, dp[index1][index2])
            else:
                # Characters don't match: substring breaks
                dp[index1][index2] = 0
    
    return ans
```

---

### 2. Space Optimized (Bottom-Up DP)

**Optimization Strategy:**
- We only need the previous row (`dp[index1-1]`) to compute the current row
- Use two 1D arrays instead of a 2D array
- `dp_prev` stores the previous row
- `dp_curr` stores the current row being computed

```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    # Use two arrays instead of 2D table
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    
    # Initialize base cases
    dp_curr[0] = 0
    dp_prev[0] = 0
    for index2 in range(n2+1):
        dp_prev[index2] = 0
    
    ans = 0  # Track maximum substring length
    
    # Fill row by row
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # Characters match: extend the substring
                dp_curr[index2] = 1 + dp_prev[index2-1]
                ans = max(ans, dp_curr[index2])
            else:
                # Characters don't match: substring breaks
                dp_curr[index2] = 0
        # Move current row to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return ans
```

---

## Detailed Example Walkthrough

Let's trace through `s1 = "abcdxyz"` and `s2 = "xyzabcd"`:

### DP Table Construction:

|     | ε | x | y | z | a | b | c | d |
|-----|---|---|---|---|---|---|---|---|
| **ε** | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| **a** | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| **b** | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 0 |
| **c** | 0 | 0 | 0 | 0 | 0 | 0 | 3 | 0 |
| **d** | 0 | 0 | 0 | 0 | 0 | 0 | 0 | **4** |
| **x** | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| **y** | 0 | 0 | 2 | 0 | 0 | 0 | 0 | 0 |
| **z** | 0 | 0 | 0 | 3 | 0 | 0 | 0 | 0 |

**Step-by-step filling (key cells):**
1. `dp[1][4]`: 'a' == 'a' → `1 + dp[0][3] = 1`
2. `dp[2][5]`: 'b' == 'b' → `1 + dp[1][4] = 2`
3. `dp[3][6]`: 'c' == 'c' → `1 + dp[2][5] = 3`
4. `dp[4][7]`: 'd' == 'd' → `1 + dp[3][6] = 4` ✓ **Maximum!**
5. `dp[5][1]`: 'x' == 'x' → `1 + dp[4][0] = 1`
6. `dp[6][2]`: 'y' == 'y' → `1 + dp[5][1] = 2`
7. `dp[7][3]`: 'z' == 'z' → `1 + dp[6][2] = 3`

**Maximum value**: `4`  
**Longest common substring**: "abcd"

---

## Why Recursion/Memoization Are Not Shown

Unlike the LCS subsequence problem, the substring problem doesn't naturally lend itself to top-down recursion because:

1. **DP state meaning**: `dp[i][j]` represents substring **ending at** position `(i, j)`, not substring **within** prefixes `s1[0..i]` and `s2[0..j]`

2. **No optimal substructure for the main problem**: When characters don't match, we reset to 0 rather than exploring subproblems

3. **Answer location**: The answer isn't at `dp[n1][n2]` but is the maximum across all cells

4. **Natural iteration**: The problem is naturally solved by iterating through all positions and tracking the maximum, making tabulation the most intuitive approach

---

## Complete Code

```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    for index1 in range(n1+1):
        dp[index1][0] = 0
    for index2 in range(n2+1):
        dp[0][index2] = 0
    ans = 0
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp[index1][index2] = 1 + dp[index1-1][index2-1]
                ans = max(ans, dp[index1][index2])
            else:
                dp[index1][index2] = 0
    return ans

def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    dp_curr[0] = 0
    dp_prev[0] = 0
    for index2 in range(n2+1):
        dp_prev[index2] = 0
    ans = 0
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp_curr[index2] = 1 + dp_prev[index2-1]
                ans = max(ans, dp_curr[index2])
            else:
                dp_curr[index2] = 0
        dp_prev = dp_curr.copy()
    return ans

# Test
s1, s2 = "bdefg", "bfg"
print(tabulation(s1, s2))  # Output: 2 (substring "fg")
print(optimized(s1, s2))   # Output: 2
```

---

## Complexity Analysis

### Time Complexity: **O(n1 × n2)**
- Two nested loops iterate through all combinations of indices
- Each cell computation takes O(1) time
- Tracking maximum also takes O(1) time

### Space Complexity

| Approach | Space Complexity | Explanation |
|----------|-----------------|-------------|
| **Tabulation** | O(n1 × n2) | 2D DP table of size (n1+1) × (n2+1) |
| **Space Optimized** | O(n2) | Two 1D arrays of size (n2+1) |

---

## Comparison: Subsequence vs Substring

| Feature | Longest Common Subsequence | Longest Common Substring |
|---------|---------------------------|-------------------------|
| **Definition** | Non-contiguous sequence | Contiguous sequence |
| **DP State** | LCS in prefixes up to (i, j) | Substring ending at (i, j) |
| **Match Case** | `1 + dp[i-1][j-1]` | `1 + dp[i-1][j-1]` |
| **Mismatch Case** | `max(dp[i-1][j], dp[i][j-1])` | `0` |
| **Answer Location** | `dp[n1][n2]` | `max(all dp values)` |
| **Example** | "abc" and "ac" → "ac" (length 2) | "abc" and "ac" → "a" or "c" (length 1) |

---

# Longest Palindromic Subsequence

## Problem Description

Given a string, find the **length of the longest palindromic subsequence**.

A palindrome reads the same backward as forward. A subsequence can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

### Examples

**Input:** s = "eeeme"
**Output:** 4
**Explanation:** The longest palindromic subsequence is "eeee".

**Input:** s = "annb"
**Output:** 2
**Explanation:** The longest palindromic subsequence is "nn".

---

## Intuition Behind the Solution

The idea is simple. A palindrome reads the same from both directions. So, if we reverse the string and find the **Longest Common Subsequence (LCS)** between the original and reversed strings, we get the length of the longest palindromic subsequence.

Example:

```
Original: e e e m e
Reversed: e m e e e
```

The LCS between these two strings is `e e e e`, which is also the longest palindromic subsequence.

Thus, the problem reduces to finding the LCS between `s` and `reverse(s)`.

---

## Recurrence Relation

Let `f(i, j)` represent the length of the longest palindromic subsequence in substring `s[i...j]`.

* If `s[i] == s[j]`:
  `f(i, j) = 1 + f(i + 1, j - 1)`
* Else:
  `f(i, j) = max(f(i + 1, j), f(i, j - 1))`

**Base Case:**
If `i > j`, return 0.
If `i == j`, return 1.

---

## Recursion Tree Example (s = "aba")

```
f(0,2)
├── s[0]==s[2] → 1 + f(1,1)
│   └── f(1,1)=1
└── Total = 2
```

---

## Step-by-Step Conversion

### 1. Recursion → Memoization

We store intermediate results in a 2D array `dp[i][j]` to avoid recomputation.

### 2. Memoization → Tabulation

We convert the top-down recursion into a bottom-up iterative table.

### 3. Tabulation → Space Optimization

We observe that only the previous and current rows are required, so we use two 1D arrays instead of a full 2D matrix.

---

## Code

```python
def optimized(s1,s2):
    n1,n2=len(s1),len(s2)
    dp_curr=[None]*(n2+1)
    dp_prev=[None]*(n2+1)
    dp_curr[0]=0
    dp_prev[0]=0
    for index2 in range(n2+1):
        dp_prev[index2]=0
    for index1 in range(1,n1+1):
        for index2 in range(1,n2+1):
            if(s1[index1-1]==s2[index2-1]):
                dp_curr[index2]=1+dp_prev[index2-1]
            else:
                exclude1=dp_prev[index2]
                exclude2=dp_curr[index2-1]
                dp_curr[index2]=max(exclude1,exclude2)
        dp_prev=dp_curr.copy()
    return dp_prev[n2]

def solve(s):
    ans=optimized(s,s[::-1])
    return ans

print(solve("eeeme"))
```

---

## Time and Space Complexity

* **Time Complexity:** O(N²)
* **Space Complexity:** O(N)


---

# Minimum Insertions to Make String Palindrome

### Problem Description

Given a string `s`, find the **minimum number of insertions** needed to make it a palindrome. You can insert characters at any position in the string.

### Examples

**Input:** s = "abcaa"
**Output:** 2
**Explanation:** Insert 'c' and 'b' to make "abcacba".

**Input:** s = "ba"
**Output:** 1
**Explanation:** Insert 'a' at the beginning to make "aba".

---

### Intuition

* To form a palindrome, the string should read the same forward and backward.
* The **Longest Palindromic Subsequence (LPS)** tells us the largest part of the string that is already a palindrome.
* We only need to insert characters for the remaining unmatched parts.
* Formula:
  `Minimum Insertions = len(s) - LPS(s)`
* LPS can be found using **Longest Common Subsequence (LCS)** of `s` and its reverse.

---

### Code

```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None] * (n2 + 1)
    dp_prev = [None] * (n2 + 1)

    dp_curr[0] = 0
    dp_prev[0] = 0
    for index2 in range(n2 + 1):
        dp_prev[index2] = 0

    for index1 in range(1, n1 + 1):
        for index2 in range(1, n2 + 1):
            if s1[index1 - 1] == s2[index2 - 1]:
                dp_curr[index2] = 1 + dp_prev[index2 - 1]
            else:
                exclude1 = dp_prev[index2]
                exclude2 = dp_curr[index2 - 1]
                dp_curr[index2] = max(exclude1, exclude2)
        dp_prev = dp_curr.copy()
    return dp_prev[n2]

def solve(s):
    lcsLength = optimized(s, s[::-1])
    ans = len(s) - lcsLength
    return ans
```

---

### Example Dry Run

**Input:** s = "ba"
`s[::-1] = "ab"`

| s1 | s2 | LCS Table Result |
| -- | -- | ---------------- |
| b  | a  | 0                |
| b  | b  | 1                |
| a  | a  | 1                |
| a  | b  | 1                |

LCS = 1 → Minimum insertions = 2 - 1 = **1**

---

# Minimum Insertions or Deletions to Convert String A to B

### Problem Description

Given two strings `str1` and `str2`, find the **minimum number of insertions and deletions** required to transform `str1` into `str2`.

You can insert or delete characters at any position.

### Examples

**Example 1:**
Input: `str1 = "kitten"`, `str2 = "sitting"`
Output: `5`
Explanation:
Delete `k`, insert `s`, then insert `i` and `g` at appropriate positions → "sitting".

**Example 2:**
Input: `str1 = "flaw"`, `str2 = "lawn"`
Output: `2`
Explanation: Delete `f` and insert `n` → "lawn".

---

### Intuition Behind Solution

* The **Longest Common Subsequence (LCS)** between `str1` and `str2` represents the part that already matches in both strings.
* You only need to **delete** the extra characters in `str1` and **insert** the missing characters to make it match `str2`.
* Hence:

  * Deletions = `len(str1) - LCS`
  * Insertions = `len(str2) - LCS`
  * Total operations = Deletions + Insertions

---

### Code (Optimized using Space Optimization)

```python
def optimized(s1,s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None] * (n2 + 1)
    dp_prev = [None] * (n2 + 1)

    dp_curr[0] = 0
    dp_prev[0] = 0
    for index2 in range(n2 + 1):
        dp_prev[index2] = 0

    for index1 in range(1, n1 + 1):
        for index2 in range(1, n2 + 1):
            if s1[index1 - 1] == s2[index2 - 1]:
                dp_curr[index2] = 1 + dp_prev[index2 - 1]
            else:
                exclude1 = dp_prev[index2]
                exclude2 = dp_curr[index2 - 1]
                dp_curr[index2] = max(exclude1, exclude2)
        dp_prev = dp_curr.copy()
    return dp_prev[n2]

def solve(s1, s2):
    lcsLength = optimized(s1, s2)
    deletions = len(s1) - lcsLength
    insertions = len(s2) - lcsLength
    return deletions + insertions

class Solution:
    def minOperations(self, str1, str2):
        return solve(str1, str2)
```

---

### Intuitive Example

Let `str1 = "heap"` and `str2 = "pea"`.

* LCS(`heap`, `pea`) = `ea` (length = 2)
* Deletions = `4 - 2 = 2` (remove `h`, `p`)
* Insertions = `3 - 2 = 1` (insert `p` at start)
* Total = 3 operations.

---

### Recurrence Relation

```
f(i, j) = 1 + f(i-1, j-1) if s1[i-1] == s2[j-1]
else f(i, j) = max(f(i-1, j), f(i, j-1))
```

* Base case: `f(0, j) = 0`, `f(i, 0) = 0`

---

### Recursion Tree Example (s1 = "ab", s2 = "acb")

```
f(2,3)
├── s1[1] == s2[2] → 1 + f(1,2)
│   └── f(1,2) → max(f(0,2), f(1,1))
│       └── f(1,1)=1
└── Total LCS length = 2
```

---

### Time and Space Complexity

* **Time Complexity:** O(n1 × n2)
* **Space Complexity:** O(n2) after optimization (since only previous and current rows are stored)
---

### Complexity

* **Time Complexity:** O(n²)
* **Space Complexity:** O(n) using 1D DP array optimization

---

# Shortest Common Supersequence

## Problem Statement

Given two strings `str1` and `str2`, find the shortest string that contains both `str1` and `str2` as subsequences.

### Examples

**Example 1:**
- Input: `str1 = "mno"`, `str2 = "nop"`
- Output: `"mnop"`
- Explanation: "mnop" contains "mno" (positions 0,1,2) and "nop" (positions 1,2,3)

**Example 2:**
- Input: `str1 = "dynamic"`, `str2 = "program"`
- Output: `"dynprogramic"`
- Explanation: All characters from both strings are included with minimal overlap

## Intuition

### Core Idea

The shortest common supersequence combines two strings by merging their common parts. Think of it as:

**Total length = len(str1) + len(str2) - len(LCS)**

Where LCS is the Longest Common Subsequence - the characters that appear in both strings in the same order.

### Simple Example

Consider `str1 = "ab"` and `str2 = "ac"`:

1. LCS = "a" (length 1)
2. Supersequence length = 2 + 2 - 1 = 3
3. Result: "abc" (merge at common 'a', then add unique 'b' and 'c')

### Step-by-Step Process

The algorithm works in two phases:

**Phase 1: Find LCS using Dynamic Programming**
- Build a table where `dp[i][j]` = length of LCS between first `i` characters of `str1` and first `j` characters of `str2`
- When characters match: `dp[i][j] = 1 + dp[i-1][j-1]`
- When they don't match: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`

**Phase 2: Backtrack to Build Supersequence**
- Start from `dp[n1][n2]` and move backwards
- When characters match: add character once (shared in both strings)
- When they don't match: add the character from the string that gave the higher LCS value
- Add remaining characters from either string

## Why It Works

The algorithm ensures correctness through these principles:

1. **Matching characters** appear once in the result (no duplication of common subsequence)
2. **Non-matching characters** are added based on which string contributed to the optimal LCS
3. **Remaining characters** from either string are appended at the end
4. The backtracking ensures both strings remain as subsequences in the final result

## Implementation

```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[0] * (n2 + 1) for _ in range(n1 + 1)]
    
    # Build LCS table
    for index1 in range(1, n1 + 1):
        for index2 in range(1, n2 + 1):
            if s1[index1 - 1] == s2[index2 - 1]:
                dp[index1][index2] = 1 + dp[index1 - 1][index2 - 1]
            else:
                dp[index1][index2] = max(dp[index1 - 1][index2], 
                                         dp[index1][index2 - 1])
    return dp

def solve(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = tabulation(s1, s2)
    
    index1, index2 = n1, n2
    ans = []
    
    # Backtrack to build supersequence
    while index1 > 0 and index2 > 0:
        if s1[index1 - 1] == s2[index2 - 1]:
            # Characters match - add once
            ans.append(s1[index1 - 1])
            index1 -= 1
            index2 -= 1
        else:
            # Add character from string with higher LCS contribution
            if dp[index1 - 1][index2] > dp[index1][index2 - 1]:
                ans.append(s1[index1 - 1])
                index1 -= 1
            else:
                ans.append(s2[index2 - 1])
                index2 -= 1
    
    # Add remaining characters
    while index1 > 0:
        ans.append(s1[index1 - 1])
        index1 -= 1
    
    while index2 > 0:
        ans.append(s2[index2 - 1])
        index2 -= 1
    
    return "".join(ans[::-1])

# Test
print(solve("brute", "groot"))  # Output: "bgruoote"
```

## Complexity Analysis

### Time Complexity
$$ O(n_1 \times n_2) $$

- Building the LCS table: $$O(n_1 \times n_2)$$
- Backtracking: $$O(n_1 + n_2)$$
- Overall: $$O(n_1 \times n_2)$$

### Space Complexity
$$ O(n_1 \times n_2) $$

- DP table storage: $$O(n_1 \times n_2)$$
- Result string: $$O(n_1 + n_2)$$
- Overall: $$O(n_1 \times n_2)$$

## Dry Run Example

**Input:** `str1 = "brute"`, `str2 = "groot"`

### LCS Table (partial)

|   | ε | g | r | o | o | t |
|---|---|---|---|---|---|---|
| ε | 0 | 0 | 0 | 0 | 0 | 0 |
| b | 0 | 0 | 0 | 0 | 0 | 0 |
| r | 0 | 0 | 1 | 1 | 1 | 1 |
| u | 0 | 0 | 1 | 1 | 1 | 1 |
| t | 0 | 0 | 1 | 1 | 1 | 2 |
| e | 0 | 0 | 1 | 1 | 1 | 2 |

### Backtracking Path

1. Compare 'e' and 't': different → take 'e' (from str1)
2. Compare 't' and 't': same → take 't'
3. Compare 'u' and 'o': different → take 'o' (from str2)
4. Compare 'u' and 'o': different → take 'o' (from str2)
5. Compare 'u' and 'r': different → take 'u' (from str1)
6. Compare 'r' and 'r': same → take 'r'
7. Add remaining 'b' and 'g'

**Result:** "bgruoote"

---

# Distinct Subsequences

## Problem Statement

Given two strings `s` and `t`, return the number of distinct subsequences of `s` that equal `t`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde" while "aec" is not.

The task is to count how many different ways we can form `t` from `s` by deleting some (or no) characters from `s`.

Return the result modulo 10⁹+7.

### Examples

**Example 1:**
```
Input: s = "axbxax", t = "axa"
Output: 2
Explanation: In the string "axbxax", there are two distinct subsequences "axa":
(a)(x)bx(a)x  → indices [0,1,4]
(a)xb(x)(a)x  → indices [0,3,4]
```

**Example 2:**
```
Input: s = "babgbag", t = "bag"
Output: 5
Explanation: In the string "babgbag", there are five distinct subsequences "bag":
(ba)(b)(g)bag  → indices [0,1,2]
(ba)(b)g(ba)(g) → indices [0,1,6]
(bab)(g)(ba)g  → indices [0,1,3]
(ba)b(g)(ba)(g) → indices [0,5,6]
(babg)(ba)(g)  → indices [2,5,6]
```

---

## Problem Understanding

Given two strings `s1` and `s2`:
- We need to find the **number of times** `s2` appears as a subsequence in `s1`
- Each occurrence uses a different combination of character indices from `s1`
- Characters must maintain their relative order

**Example:**
```
s1 = "axbxax"
s2 = "axa"

Way 1: s1[0]='a', s1[1]='x', s1[4]='a' → "axa"
Way 2: s1[0]='a', s1[3]='x', s1[4]='a' → "axa"

Total ways: 2
```

---

## Problem Representation

Both strings can be represented by indices: `0...n1-1` and `0...n2-1`

Let `f(index1, index2)` represent the count of ways `s2[0...index2]` appears as a subsequence in `s1[0...index1]`.

---

## Base Cases

### Case 1: All characters of `t` are matched
If we've successfully matched all characters of `s2` (reached the end), we found one valid way:
```
f(index1, index2 < 0) = 1
```

### Case 2: String `s` is exhausted but `t` is not
If `s1` ends but `s2` still has characters remaining, we cannot form `s2`:
```
f(index1 < 0, index2) = 0
```

---

## Recurrence Relation

To check the existence of `s2` in `s1`, we compare characters of `s2` with characters of `s1` in order.

### Case 1: Characters Match
When characters match at current positions, we have **two possibilities**:

1. **Use this character match**: Move both pointers back to find remaining characters
   - `f(index1-1, index2-1)`

2. **Don't use this character match**: Look for `s2` in the remaining part of `s1`
   - `f(index1-1, index2)`
   - This accounts for `s2` appearing multiple times

Since we want the **total count**, we **add** both possibilities:
```
if s1[index1] == s2[index2]:
    f(index1, index2) = f(index1-1, index2) + f(index1-1, index2-1)
```

### Case 2: Characters Don't Match
If characters don't match, skip the current character in `s1` and continue searching:
```
if s1[index1] != s2[index2]:
    f(index1, index2) = f(index1-1, index2)
```

### Complete Recurrence Relation
```python
if s1[index1] == s2[index2]:
    return f(index1-1, index2) + f(index1-1, index2-1)
else:
    return f(index1-1, index2)
```

---

## Recursion Tree

For `s1 = "aba"` and `s2 = "ab"`:

```
                          f(2,1) [a vs b]
                               |
                         f(1,1) [b vs b] ✓
                        /              \
              f(0,1) [a vs b]      f(0,0) [a vs a] ✓
                    |                    /        \
                f(-1,1)            f(-1,0)    f(-1,-1)
                    |                  |           |
                    0                  0           1
                    
Path 1: f(2,1) → f(1,1) → f(0,0) → f(-1,-1) = 1 [using indices 0,1]
Path 2: None (f(0,1) fails as 'a' != 'b')

Total: 1
```

**Explanation:**
- At `f(2,1)`: `s1[2]='a'` doesn't match `s2[1]='b'` → `f(1,1)`
- At `f(1,1)`: `s1[1]='b'` matches `s2[1]='b'` → `f(0,1) + f(0,0)`
- At `f(0,0)`: `s1[0]='a'` matches `s2[0]='a'` → `f(-1,0) + f(-1,-1)`
- `f(-1,-1) = 1` (all of `s2` matched)
- Result: `0 + (0 + 1) = 1`

---

## Solution Approaches

### 1. Recursion (Top-Down)

```python
def solve(s1, s2, index1, index2):
    # Base case: all characters of s2 are matched
    if index2 < 0:
        return 1
    
    # Base case: s1 is exhausted but s2 is not
    if index1 < 0:
        return 0
    
    # If characters match
    if s1[index1] == s2[index2]:
        # Way 1: Don't use this match, look for s2 in remaining s1
        way1 = solve(s1, s2, index1-1, index2)
        # Way 2: Use this match, find remaining characters
        way2 = solve(s1, s2, index1-1, index2-1)
        return way1 + way2
    else:
        # Characters don't match, skip current character in s1
        return solve(s1, s2, index1-1, index2)
```

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion:**
1. Identify overlapping subproblems (same `index1` and `index2` are computed multiple times)
2. Create a 2D memoization table `memo[index1][index2]`
3. Before computing, check if the result is already stored
4. After computing, store the result in the memo table

```python
def solve_memo(s1, s2, index1, index2, memo):
    # Base case: all characters of s2 are matched
    if index2 < 0:
        return 1
    
    # Base case: s1 is exhausted but s2 is not
    if index1 < 0:
        return 0
    
    # Check if already computed
    if memo[index1][index2] != None:
        return memo[index1][index2]
    
    # If characters match
    if s1[index1] == s2[index2]:
        way1 = solve_memo(s1, s2, index1-1, index2, memo)
        way2 = solve_memo(s1, s2, index1-1, index2-1, memo)
        memo[index1][index2] = way1 + way2
    else:
        # Characters don't match
        memo[index1][index2] = solve_memo(s1, s2, index1-1, index2, memo)
    
    return memo[index1][index2]
```

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization:**
1. Convert recursive calls to iterative loops
2. Use 1-based indexing to handle base cases
3. Fill the DP table from smallest subproblems to largest
4. Initialize base cases:
   - `dp[i][0] = 1` (empty `s2` can be formed in one way)
   - `dp[0][j] = 0` for `j > 0` (cannot form non-empty `s2` from empty `s1`)

```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    # Create DP table with 1-based indexing
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base case: empty s2 can be formed in one way (delete all)
    for index1 in range(n1+1):
        dp[index1][0] = 1
    
    # Base case: cannot form non-empty s2 from empty s1
    for index2 in range(n2+1):
        dp[0][index2] = 0
    
    # Exception: empty s1 and empty s2
    dp[0][0] = 1
    
    # Fill the DP table
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # Characters match: add both possibilities
                way1 = dp[index1-1][index2]      # Don't use this match
                way2 = dp[index1-1][index2-1]    # Use this match
                dp[index1][index2] = way1 + way2
            else:
                # Characters don't match: skip current character in s1
                dp[index1][index2] = dp[index1-1][index2]
    
    return dp[n1][n2]
```

#### Tabulation Example

For `s1 = "aba"` and `s2 = "ab"`:

|     | ε | a | b |
|-----|---|---|---|
| **ε** | 1 | 0 | 0 |
| **a** | 1 | 1 | 0 |
| **b** | 1 | 1 | 1 |
| **a** | 1 | 2 | 1 |

**Step-by-step filling:**
- `dp[*][0]` = 1 (empty `s2`)
- `dp[0][*]` = 0 (empty `s1`, except `dp[0][0] = 1`)
- `dp[1][1]`: `s1[0]='a'` == `s2[0]='a'` → `dp[0][1] + dp[0][0] = 0 + 1 = 1`
- `dp[1][2]`: `s1[0]='a'` != `s2[1]='b'` → `dp[0][2] = 0`
- `dp[2][1]`: `s1[1]='b'` != `s2[0]='a'` → `dp[1][1] = 1`
- `dp[2][2]`: `s1[1]='b'` == `s2[1]='b'` → `dp[1][2] + dp[1][1] = 0 + 1 = 1`
- `dp[3][1]`: `s1[2]='a'` == `s2[0]='a'` → `dp[2][1] + dp[2][0] = 1 + 1 = 2`
- `dp[3][2]`: `s1[2]='a'` != `s2[1]='b'` → `dp[2][2] = 1`

**Result:** `dp[3][2] = 1`

---

### 4. Space Optimized (Bottom-Up DP)

**Conversion from Tabulation:**
1. Observe that we only need the previous row (`dp[index1-1]`) to compute the current row
2. Use two 1D arrays instead of a 2D array
3. `dp_prev` stores the previous row
4. `dp_curr` stores the current row being computed
5. After each row, copy `dp_curr` to `dp_prev`

```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    # Use two arrays instead of 2D table
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    
    # Initialize base cases
    for index2 in range(n2+1):
        dp_prev[index2] = 0
    dp_prev[0] = 1  # Empty s2 can be formed in one way
    dp_curr[0] = 1
    
    # Fill row by row
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                way1 = dp_prev[index2]       # Don't use this match
                way2 = dp_prev[index2-1]     # Use this match
                dp_curr[index2] = way1 + way2
            else:
                dp_curr[index2] = dp_prev[index2]
        # Move current row to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[n2]
```

---

## Detailed Example Walkthrough

Let's trace through `s1 = "babgbag"` and `s2 = "bag"`:

### DP Table Construction:

|     | ε | b | a | g |
|-----|---|---|---|---|
| **ε** | 1 | 0 | 0 | 0 |
| **b** | 1 | 1 | 0 | 0 |
| **a** | 1 | 1 | 1 | 0 |
| **b** | 1 | 2 | 1 | 0 |
| **g** | 1 | 2 | 1 | 1 |
| **b** | 1 | 3 | 1 | 1 |
| **a** | 1 | 3 | 4 | 1 |
| **g** | 1 | 3 | 4 | 5 |

**Key computations:**
1. `dp[1][1]`: 'b'=='b' → `0 + 1 = 1`
2. `dp[2][2]`: 'a'=='a' → `1 + 0 = 1`
3. `dp[3][1]`: 'b'=='b' → `1 + 1 = 2`
4. `dp[4][3]`: 'g'=='g' → `0 + 1 = 1`
5. `dp[5][1]`: 'b'=='b' → `2 + 1 = 3`
6. `dp[6][2]`: 'a'=='a' → `1 + 3 = 4`
7. `dp[7][3]`: 'g'=='g' → `1 + 4 = 5` ✓

**Result:** `5` distinct ways

**The 5 subsequences:**
1. `b[0]a[1]g[3]` → "bag"
2. `b[0]a[1]g[6]` → "bag"
3. `b[2]a[1]g[3]` → "bag"
4. `b[2]a[5]g[6]` → "bag"
5. `b[4]a[5]g[6]` → "bag"

---

## Why Add Both Ways?

When characters match, we add both possibilities because:

1. **Way 1** (`f(index1-1, index2)`): We might want to skip this match and use a different occurrence of the same character later in `s1`. This accounts for multiple occurrences.

2. **Way 2** (`f(index1-1, index2-1)`): We use this match and look for the remaining characters.

**Example:** For `s1 = "aba"` and `s2 = "a"`:
- Both `s1[0]` and `s1[2]` are 'a'
- If we only took one way, we'd miss one of the valid subsequences
- By adding both, we count all possible ways

---

## Complete Code

```python
def solve(s1, s2, index1, index2):
    if index2 < 0:
        return 1
    if index1 < 0:
        return 0
    if s1[index1] == s2[index2]:
        way1 = solve(s1, s2, index1-1, index2)
        way2 = solve(s1, s2, index1-1, index2-1)
        return way1 + way2
    else:
        return solve(s1, s2, index1-1, index2)

def solve_memo(s1, s2, index1, index2, memo):
    if index2 < 0:
        return 1
    if index1 < 0:
        return 0
    if memo[index1][index2] != None:
        return memo[index1][index2]
    if s1[index1] == s2[index2]:
        way1 = solve_memo(s1, s2, index1-1, index2, memo)
        way2 = solve_memo(s1, s2, index1-1, index2-1, memo)
        memo[index1][index2] = way1 + way2
    else:
        memo[index1][index2] = solve_memo(s1, s2, index1-1, index2, memo)
    return memo[index1][index2]

def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    for index1 in range(n1+1):
        dp[index1][0] = 1
    for index2 in range(n2+1):
        dp[0][index2] = 0
    dp[0][0] = 1
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                way1 = dp[index1-1][index2]
                way2 = dp[index1-1][index2-1]
                dp[index1][index2] = way1 + way2
            else:
                dp[index1][index2] = dp[index1-1][index2]
    return dp[n1][n2]

def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    for index2 in range(n2+1):
        dp_prev[index2] = 0
    dp_prev[0] = 1
    dp_curr[0] = 1
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                way1 = dp_prev[index2]
                way2 = dp_prev[index2-1]
                dp_curr[index2] = way1 + way2
            else:
                dp_curr[index2] = dp_prev[index2]
        dp_prev = dp_curr.copy()
    return dp_prev[n2]

# Test
s1, s2 = "babgbag", "bag"
n1, n2 = len(s1), len(s2)
print(solve(s1, s2, n1-1, n2-1))
memo = [[None]*(n2) for _ in range(n1)]
print(solve_memo(s1, s2, n1-1, n2-1, memo))
print(tabulation(s1, s2))
print(optimized(s1, s2))
```

---

## Complexity Analysis

### Time Complexity

| Approach | Time Complexity | Explanation |
|----------|----------------|-------------|
| **Recursion** | O(2^(n1+n2)) | Each call can branch into two recursive calls |
| **Memoization** | O(n1 × n2) | Each subproblem is computed once |
| **Tabulation** | O(n1 × n2) | Two nested loops iterate through all combinations |
| **Space Optimized** | O(n1 × n2) | Time complexity remains the same |

### Space Complexity

| Approach | Space Complexity | Explanation |
|----------|-----------------|-------------|
| **Recursion** | O(n1 + n2) | Recursion stack depth |
| **Memoization** | O(n1 × n2) + O(n1 + n2) | 2D memo table + recursion stack |
| **Tabulation** | O(n1 × n2) | 2D DP table |
| **Space Optimized** | O(n2) | Two 1D arrays of size (n2+1) |

---

# Edit Distance

## Problem Statement

Given two strings `start` and `target`, you need to determine the minimum number of operations required to convert the string `start` into the string `target`. 

The operations you can use are:
1. **Insert a character**: Add any single character at any position in the string
2. **Delete a character**: Remove any single character from the string
3. **Replace a character**: Change any single character in the string to another character

The goal is to transform `start` into `target` using the fewest number of these operations.

### Examples

**Example 1:**
```
Input: start = "planet", target = "plan"
Output: 2

Explanation: 
To transform "planet" into "plan", the following operations are required:
1. Delete the character 'e': "planet" -> "plat"
2. Delete the character 't': "plat" -> "plan"
Thus, a total of 2 operations are needed.
```

**Example 2:**
```
Input: start = "abcdefg", target = "azced"
Output: 4

Explanation: 
To transform "abcdefg" into "azced", the following operations are required:
1. Replace 'b' with 'z': "abcdefg" -> "azcdefg"
2. Delete 'd': "azcdefg" -> "azcefg"
3. Delete 'f': "azcefg" -> "azceg"
4. Replace 'g' with 'd': "azceg" -> "azced"
Thus, a total of 4 operations are needed.
```

---

## Problem Understanding

Given two strings `s1` and `s2` with lengths `n1` and `n2`:
- We need to convert `s1` to `s2`
- We can perform **insert**, **delete**, or **replace** operations
- We need to find the **minimum number** of operations
- At a time, we can perform only **one action** on **one character**

**Example:**
```
s1 = "horse"
s2 = "ros"

Operation 1: Replace 'h' with 'r': "horse" -> "rorse"
Operation 2: Delete 'r': "rorse" -> "rose"
Operation 3: Delete 'e': "rose" -> "ros"

Minimum operations: 3
```

---

## Problem Representation

Both strings can be represented by indices: `0...n1-1` and `0...n2-1`

Let `f(index1, index2)` represent the minimum number of operations to convert `s1[0...index1]` to `s2[0...index2]`.

---

## Base Cases

### Case 1: `s2` is empty, `s1` has characters
If `s2` becomes empty and `s1` still has characters, we need to **delete** all remaining characters in `s1`:
```
f(index1, index2 < 0) = index1 + 1
```
The `+1` accounts for 0-based indexing.

### Case 2: `s1` is empty, `s2` has characters
If `s1` becomes empty and `s2` still has characters, we need to **insert** all remaining characters from `s2`:
```
f(index1 < 0, index2) = index2 + 1
```

**Example:**
- `s1 = "abc"`, `s2 = ""` → Need 3 deletions
- `s1 = ""`, `s2 = "xyz"` → Need 3 insertions

---

## Recurrence Relation

To make `s1 == s2`, we compare each character in `s1` and `s2`.

### Case 1: Characters Match
If both characters are equal, no operation is needed:
```
if s1[index1] == s2[index2]:
    f(index1, index2) = f(index1-1, index2-1)
```

### Case 2: Characters Don't Match
If characters are different, we have **three options**:

1. **Replace**: Replace `s1[index1]` with `s2[index2]`, then move both pointers
   - Cost: `1 + f(index1-1, index2-1)`

2. **Delete**: Delete `s1[index1]` and try matching the next character in `s1`
   - Cost: `1 + f(index1-1, index2)`

3. **Insert**: Insert `s2[index2]` into `s1` at the current position
   - After insertion, `s2[index2]` is matched, so we move `index2` backward
   - `index1` stays the same as we're still comparing the same character
   - Cost: `1 + f(index1, index2-1)`

We take the **minimum** of all three options:
```
if s1[index1] != s2[index2]:
    f(index1, index2) = min(
        1 + f(index1-1, index2-1),  # Replace
        1 + f(index1-1, index2),    # Delete
        1 + f(index1, index2-1)     # Insert
    )
```

---

## Understanding Insert Operation

The insert operation can be confusing. Here's a detailed explanation:

**Current state**: Comparing `s1[index1]` with `s2[index2]`

**Insert operation**: Insert `s2[index2]` into `s1` at position `index1+1`
- After insertion, the new character matches `s2[index2]`
- So we've successfully matched `s2[index2]`
- Move to `s2[index2-1]` to match the next character
- `s1[index1]` is still there, so `index1` remains the same

**Example:**
```
s1 = "ab", s2 = "abc"
Comparing: s1[1]='b' with s2[2]='c'

Insert 'c': "ab" -> "abc"
Now we've matched s2[2]='c'
Continue comparing: s1[1]='b' with s2[1]='b'
```

---

## Recursion Tree

For `s1 = "ab"` and `s2 = "ac"`:

```
                    f(1,1) [b vs c]
                         |
                  min of 3 options
           /              |              \
    1+f(0,0)         1+f(0,1)        1+f(1,0)
   [Replace]         [Delete]        [Insert]
        |                |                |
    1+f(0,0)         1+f(0,1)        1+f(1,0)
   [a vs a]         [a vs ac]       [ab vs a]
        |                |                |
    1+0=1            1+1=2           1+f(0,0)
                                         |
                                     1+f(0,0)
                                    [a vs a]
                                         |
                                     1+0=1

Result: min(1, 2, 1) = 1
```

**Explanation:**
- At `f(1,1)`: 'b' != 'c' → Try all 3 operations
- Replace: `1 + f(0,0)` → Characters match → `1 + 0 = 1`
- Delete: `1 + f(0,1)` → Need 1 more operation → `1 + 1 = 2`
- Insert: `1 + f(1,0)` → Eventually resolves to → `1`
- Minimum: `1`

---

## Solution Approaches

### 1. Recursion (Top-Down)

```python
def solve(s1, s2, index1, index2):
    # Base case: s1 is empty, need to insert all of s2
    if index1 < 0:
        return index2 + 1
    
    # Base case: s2 is empty, need to delete all of s1
    if index2 < 0:
        return index1 + 1
    
    # If characters match, no operation needed
    if s1[index1] == s2[index2]:
        return solve(s1, s2, index1-1, index2-1)
    else:
        # Try all three operations and take minimum
        replace = 1 + solve(s1, s2, index1-1, index2-1)
        insert = 1 + solve(s1, s2, index1, index2-1)
        delete = 1 + solve(s1, s2, index1-1, index2)
        return min(replace, insert, delete)
```

---

### 2. Memoization (Top-Down DP)

**Conversion from Recursion:**
1. Identify overlapping subproblems (same `index1` and `index2` are computed multiple times)
2. Create a 2D memoization table `memo[index1][index2]`
3. Before computing, check if the result is already stored
4. After computing, store the result in the memo table

```python
def solve_memo(s1, s2, index1, index2, memo):
    # Base case: s1 is empty
    if index1 < 0:
        return index2 + 1
    
    # Base case: s2 is empty
    if index2 < 0:
        return index1 + 1
    
    # Check if already computed
    if memo[index1][index2] != None:
        return memo[index1][index2]
    
    # If characters match
    if s1[index1] == s2[index2]:
        memo[index1][index2] = solve_memo(s1, s2, index1-1, index2-1, memo)
    else:
        # Try all three operations
        replace = 1 + solve_memo(s1, s2, index1-1, index2-1, memo)
        insert = 1 + solve_memo(s1, s2, index1, index2-1, memo)
        delete = 1 + solve_memo(s1, s2, index1-1, index2, memo)
        memo[index1][index2] = min(replace, insert, delete)
    
    return memo[index1][index2]
```

---

### 3. Tabulation (Bottom-Up DP)

**Conversion from Memoization:**
1. Convert recursive calls to iterative loops
2. Use 1-based indexing to handle base cases
3. Fill the DP table from smallest subproblems to largest
4. Initialize base cases:
   - `dp[i][0] = i` (delete all characters from `s1`)
   - `dp[0][j] = j` (insert all characters from `s2`)

```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    # Create DP table with 1-based indexing
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base case: s2 is empty, delete all from s1
    for index1 in range(n1+1):
        dp[index1][0] = index1
    
    # Base case: s1 is empty, insert all from s2
    for index2 in range(n2+1):
        dp[0][index2] = index2
    
    # Fill the DP table
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # Characters match, no operation needed
                dp[index1][index2] = dp[index1-1][index2-1]
            else:
                # Try all three operations and take minimum
                replace = 1 + dp[index1-1][index2-1]
                insert = 1 + dp[index1][index2-1]
                delete = 1 + dp[index1-1][index2]
                dp[index1][index2] = min(replace, insert, delete)
    
    return dp[n1][n2]
```

#### Tabulation Example

For `s1 = "horse"` and `s2 = "ros"`:

|     | ε | r | o | s |
|-----|---|---|---|---|
| **ε** | 0 | 1 | 2 | 3 |
| **h** | 1 | 1 | 2 | 3 |
| **o** | 2 | 2 | 1 | 2 |
| **r** | 3 | 2 | 2 | 2 |
| **s** | 4 | 3 | 3 | 2 |
| **e** | 5 | 4 | 4 | 3 |

**Step-by-step filling:**
- Row 0 and Column 0 are initialized with indices
- `dp[1][1]`: 'h' != 'r' → `min(1+0, 1+1, 1+1) = 1` (replace)
- `dp[2][2]`: 'o' == 'o' → `dp[1][1] = 1`
- `dp[3][1]`: 'r' == 'r' → `dp[2][0] = 2`
- `dp[3][2]`: 'r' != 'o' → `min(1+1, 1+2, 1+2) = 2`
- `dp[4][3]`: 's' == 's' → `dp[3][2] = 2`
- `dp[5][3]`: 'e' != 's' → `min(1+2, 1+2, 1+4) = 3`

**Result:** `dp[5][3] = 3`

---

### 4. Space Optimized (Bottom-Up DP)

**Conversion from Tabulation:**
1. Observe that we only need the previous row (`dp[index1-1]`) to compute the current row
2. Use two 1D arrays instead of a 2D array
3. `dp_prev` stores the previous row
4. `dp_curr` stores the current row being computed
5. **Important**: Set `dp_curr[0] = index1` at the start of each iteration

```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    # Use two arrays instead of 2D table
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    
    # Initialize base case for s1 being empty
    dp_curr[0] = 0
    for index2 in range(n2+1):
        dp_prev[index2] = index2
    
    # Fill row by row
    for index1 in range(1, n1+1):
        # Set the first column value for this row
        dp_curr[0] = index1
        
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # Characters match
                dp_curr[index2] = dp_prev[index2-1]
            else:
                # Try all three operations
                replace = 1 + dp_prev[index2-1]
                insert = 1 + dp_curr[index2-1]
                delete = 1 + dp_prev[index2]
                dp_curr[index2] = min(replace, insert, delete)
        
        # Move current row to previous for next iteration
        dp_prev = dp_curr.copy()
    
    return dp_prev[n2]
```

---

## Detailed Example Walkthrough

Let's trace through `s1 = "abc"` and `s2 = "ac"`:

### DP Table Construction:

|     | ε | a | c |
|-----|---|---|---|
| **ε** | 0 | 1 | 2 |
| **a** | 1 | 0 | 1 |
| **b** | 2 | 1 | 1 |
| **c** | 3 | 2 | 1 |

**Step-by-step filling:**
1. Initialize: Row 0 = [0, 1, 2], Column 0 = [0, 1, 2, 3]
2. `dp[1][1]`: 'a' == 'a' → `dp[0][0] = 0`
3. `dp[1][2]`: 'a' != 'c' → `min(1+1, 1+0, 1+2) = 1` (insert 'c')
4. `dp[2][1]`: 'b' != 'a' → `min(1+0, 1+1, 1+1) = 1` (delete 'b')
5. `dp[2][2]`: 'b' != 'c' → `min(1+0, 1+1, 1+1) = 1` (replace or delete+insert)
6. `dp[3][1]`: 'c' != 'a' → `min(1+1, 1+1, 1+2) = 2`
7. `dp[3][2]`: 'c' == 'c' → `dp[2][1] = 1`

**Result:** `dp[3][2] = 1` (delete 'b')

**Operations:**
- Delete 'b' from "abc" → "ac" ✓

---

## Complete Code

```python
def solve(s1, s2, index1, index2):
    if index1 < 0:
        return index2 + 1
    if index2 < 0:
        return index1 + 1
    if s1[index1] == s2[index2]:
        return solve(s1, s2, index1-1, index2-1)
    else:
        replace = 1 + solve(s1, s2, index1-1, index2-1)
        insert = 1 + solve(s1, s2, index1, index2-1)
        delete = 1 + solve(s1, s2, index1-1, index2)
        return min(replace, insert, delete)
    
def solve_memo(s1, s2, index1, index2, memo):
    if index1 < 0:
        return index2 + 1
    if index2 < 0:
        return index1 + 1
    if memo[index1][index2] != None:
        return memo[index1][index2]
    if s1[index1] == s2[index2]:
        memo[index1][index2] = solve_memo(s1, s2, index1-1, index2-1, memo)
    else:
        replace = 1 + solve_memo(s1, s2, index1-1, index2-1, memo)
        insert = 1 + solve_memo(s1, s2, index1, index2-1, memo)
        delete = 1 + solve_memo(s1, s2, index1-1, index2, memo)
        memo[index1][index2] = min(replace, insert, delete)
    return memo[index1][index2]
    
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    for index1 in range(n1+1):
        dp[index1][0] = index1
    for index2 in range(n2+1):
        dp[0][index2] = index2
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp[index1][index2] = dp[index1-1][index2-1]
            else:
                replace = 1 + dp[index1-1][index2-1]
                insert = 1 + dp[index1][index2-1]
                delete = 1 + dp[index1-1][index2]
                dp[index1][index2] = min(replace, insert, delete)
    return dp[n1][n2]

def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    dp_curr[0] = 0
    for index2 in range(n2+1):
        dp_prev[index2] = index2
    for index1 in range(1, n1+1):
        dp_curr[0] = index1
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp_curr[index2] = dp_prev[index2-1]
            else:
                replace = 1 + dp_prev[index2-1]
                insert = 1 + dp_curr[index2-1]
                delete = 1 + dp_prev[index2]
                dp_curr[index2] = min(replace, insert, delete)
        dp_prev = dp_curr.copy()
    return dp_prev[n2]

# Test
s1, s2 = "babgbag", "bag"
n1, n2 = len(s1), len(s2)
print(solve(s1, s2, n1-1, n2-1))
memo = [[None]*(n2) for _ in range(n1)]
print(solve_memo(s1, s2, n1-1, n2-1, memo))
print(tabulation(s1, s2))
print(optimized(s1, s2))
```

---

## Complexity Analysis

### Time Complexity

| Approach | Time Complexity | Explanation |
|----------|----------------|-------------|
| **Recursion** | O(3^(n1+n2)) | Each call can branch into three recursive calls |
| **Memoization** | O(n1 × n2) | Each subproblem is computed once |
| **Tabulation** | O(n1 × n2) | Two nested loops iterate through all combinations |
| **Space Optimized** | O(n1 × n2) | Time complexity remains the same |

### Space Complexity

| Approach | Space Complexity | Explanation |
|----------|-----------------|-------------|
| **Recursion** | O(n1 + n2) | Recursion stack depth |
| **Memoization** | O(n1 × n2) + O(n1 + n2) | 2D memo table + recursion stack |
| **Tabulation** | O(n1 × n2) | 2D DP table |
| **Space Optimized** | O(n2) | Two 1D arrays of size (n2+1) |

---

## Applications

The Edit Distance algorithm (also known as Levenshtein Distance) has many real-world applications:

1. **Spell Checking**: Finding the closest correctly spelled word
2. **DNA Sequencing**: Comparing genetic sequences
3. **Plagiarism Detection**: Measuring similarity between texts
4. **Speech Recognition**: Matching spoken words to dictionary
5. **Version Control**: Computing diffs between file versions

---
