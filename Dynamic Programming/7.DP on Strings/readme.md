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
