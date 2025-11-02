# String DP Problems - Revision Notes

## 📑 Index

1. [Longest Common Subsequence (LCS)](#1-longest-common-subsequence-lcs)
2. [Print Longest Common Subsequence](#2-print-longest-common-subsequence)
3. [Longest Common Substring](#3-longest-common-substring)
4. [Longest Palindromic Subsequence](#4-longest-palindromic-subsequence)
5. [Minimum Insertions to Make String Palindrome](#5-minimum-insertions-to-make-string-palindrome)
6. [Shortest Common Supersequence](#6-shortest-common-supersequence)
7. [Distinct Subsequences](#7-distinct-subsequences)
8. [Edit Distance](#8-edit-distance)
9. [Wildcard Matching](#9-wildcard-matching)
10. [Problem Comparison Table](#-problem-comparison-table)

---

## 1. Longest Common Subsequence (LCS)

### 📝 Problem Description
Find the length of the longest common subsequence between two strings. A subsequence maintains relative order but characters need not be contiguous.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: str1 = "bdefg", str2 = "bfg"
Output: 3
Explanation: LCS is "bfg"
```

**Example 2:**
```
Input: str1 = "mnop", str2 = "mnq"
Output: 2
Explanation: LCS is "mn"
```

### 💡 Approach 1: Recursion

**Intuition:**
- Compare characters from end of both strings
- If match: include character, move both pointers back
- If no match: try excluding from either string, take maximum

**Code:**
```python
def solve(s1, s2, index1, index2):
    # Base case: either string exhausted
    if index1 < 0 or index2 < 0:
        return 0
    
    # Characters match: include and move both pointers
    if s1[index1] == s2[index2]:
        return 1 + solve(s1, s2, index1-1, index2-1)
    else:
        # Try excluding from either string
        exclude1 = solve(s1, s2, index1-1, index2)
        exclude2 = solve(s1, s2, index1, index2-1)
        return max(exclude1, exclude2)
```

**Complexity:**
- **Time:** O(2^(n+m)) - Exponential
- **Space:** O(n+m) - Recursion stack

### 💡 Approach 2: Memoization

**Intuition:** Cache results for (index1, index2) pairs

**Code:**
```python
def solve_memo(s1, s2, index1, index2, memo):
    # Base case
    if index1 < 0 or index2 < 0:
        return 0
    
    # Return cached result
    if memo[index1][index2] is not None:
        return memo[index1][index2]
    
    if s1[index1] == s2[index2]:
        # Characters match
        memo[index1][index2] = 1 + solve_memo(s1, s2, index1-1, index2-1, memo)
    else:
        # Try both options
        exclude1 = solve_memo(s1, s2, index1-1, index2, memo)
        exclude2 = solve_memo(s1, s2, index1, index2-1, memo)
        memo[index1][index2] = max(exclude1, exclude2)
    
    return memo[index1][index2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

### 💡 Approach 3: Tabulation

**Intuition:** Build DP table bottom-up with 1-based indexing

**Code:**
```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    # DP table with extra row/column for base case
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base case: empty string has LCS = 0
    for index1 in range(n1+1):
        dp[index1][0] = 0
    for index2 in range(n2+1):
        dp[0][index2] = 0
    
    # Fill table
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            # Use index-1 for string access (1-based indexing)
            if s1[index1-1] == s2[index2-1]:
                dp[index1][index2] = 1 + dp[index1-1][index2-1]
            else:
                exclude1 = dp[index1-1][index2]
                exclude2 = dp[index1][index2-1]
                dp[index1][index2] = max(exclude1, exclude2)
    
    return dp[n1][n2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

### 💡 Approach 4: Space Optimized

**Intuition:** Only need previous and current row

**Code:**
```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    
    # Initialize base cases
    dp_curr[0] = 0
    dp_prev[0] = 0
    for index2 in range(n2+1):
        dp_prev[index2] = 0
    
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # Use dp_prev[index2-1] for diagonal
                dp_curr[index2] = 1 + dp_prev[index2-1]
            else:
                exclude1 = dp_prev[index2]
                exclude2 = dp_curr[index2-1]
                dp_curr[index2] = max(exclude1, exclude2)
        # Move current to previous
        dp_prev = dp_curr.copy()
    
    return dp_prev[n2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(m) - Two arrays

---

## 2. Print Longest Common Subsequence

### 📝 Problem Description
Print the actual longest common subsequence string between two strings.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: str1 = "abcd", str2 = "bdef"
Output: "bd"
```

**Example 2:**
```
Input: str1 = "apple", str2 = "waffle"
Output: "ale"
```

### 💡 Approach: Backtrack from DP Table

**Intuition:**
- First build the DP table (like LCS)
- Backtrack from dp[n1][n2] to dp[0][0]
- If characters match, include in result
- Otherwise, move in direction of larger value

**Code:**
```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base cases
    for index1 in range(n1+1):
        dp[index1][0] = 0
    for index2 in range(n2+1):
        dp[0][index2] = 0
    
    # Build DP table
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
    dp = tabulation(s1, s2)
    ans = []
    index1, index2 = n1, n2
    
    # Backtrack from bottom-right
    while index1 > 0 and index2 > 0:
        if s1[index1-1] == s2[index2-1]:
            # Characters match: part of LCS
            ans.append(s1[index1-1])
            index1 -= 1
            index2 -= 1
        else:
            # Move in direction of larger value
            if dp[index1-1][index2] > dp[index1][index2-1]:
                index1 -= 1
            else:
                index2 -= 1
    
    # Reverse since we built backwards
    ans = "".join(ans[::-1])
    return ans
```

**Complexity:**
- **Time:** O(n × m) - Building DP + O(n+m) backtracking
- **Space:** O(n × m) - DP table

---

## 3. Longest Common Substring

### 📝 Problem Description
Find the length of the longest common **substring** (contiguous sequence) between two strings.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: str1 = "abcde", str2 = "abfce"
Output: 2
Explanation: Longest common substring is "ab"
```

**Example 2:**
```
Input: str1 = "abcdxyz", str2 = "xyzabcd"
Output: 4
Explanation: Longest common substring is "abcd"
```

### 💡 Approach 1: Tabulation

**Intuition:**
- Similar to LCS but substring must be contiguous
- If characters match: extend previous diagonal value
- If no match: reset to 0 (breaks contiguity)
- Track maximum value seen

**Code:**
```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base cases
    for index1 in range(n1+1):
        dp[index1][0] = 0
    for index2 in range(n2+1):
        dp[0][index2] = 0
    
    ans = 0  # Track maximum length
    
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # Extend substring from diagonal
                dp[index1][index2] = 1 + dp[index1-1][index2-1]
                ans = max(ans, dp[index1][index2])
            else:
                # No match: substring breaks
                dp[index1][index2] = 0
    
    return ans
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

### 💡 Approach 2: Space Optimized

**Code:**
```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    
    # Initialize
    dp_curr[0] = 0
    dp_prev[0] = 0
    for index2 in range(n2+1):
        dp_prev[index2] = 0
    
    ans = 0
    
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # Extend from diagonal
                dp_curr[index2] = 1 + dp_prev[index2-1]
                ans = max(ans, dp_curr[index2])
            else:
                # Reset to 0
                dp_curr[index2] = 0
        dp_prev = dp_curr.copy()
    
    return ans
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(m)

---

## 4. Longest Palindromic Subsequence

### 📝 Problem Description
Find the length of the longest palindromic subsequence in a string.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: s = "eeeme"
Output: 4
Explanation: Longest palindromic subsequence is "eeee"
```

**Example 2:**
```
Input: s = "annb"
Output: 2
Explanation: Longest palindromic subsequence is "nn"
```

### 💡 Approach: LCS with Reverse

**Intuition:**
- Palindrome reads same forwards and backwards
- LCS of string and its reverse gives longest palindromic subsequence

**Code:**
```python
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

def solve(s):
    # LCS of string and its reverse
    ans = optimized(s, s[::-1])
    return ans
```

**Complexity:**
- **Time:** O(n²) - String length squared
- **Space:** O(n)

---

## 5. Minimum Insertions to Make String Palindrome

### 📝 Problem Description
Find minimum number of character insertions needed to make a string palindrome.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: s = "abcaa"
Output: 2
Explanation: Insert "cb" to make "abcacba"
```

**Example 2:**
```
Input: s = "ba"
Output: 1
Explanation: Insert "a" at beginning to make "aba"
```

### 💡 Approach: Using LPS

**Intuition:**
- Characters not in longest palindromic subsequence need to be inserted
- Formula: n - LPS(s)

**Code:**
```python
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

def solve(s):
    # Find LPS
    lcsLength = optimized(s, s[::-1])
    # Characters not in LPS need insertion
    ans = len(s) - lcsLength
    return ans
```

**Complexity:**
- **Time:** O(n²)
- **Space:** O(n)

---

## 6. Shortest Common Supersequence

### 📝 Problem Description
Find the shortest string that contains both input strings as subsequences.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: str1 = "mno", str2 = "nop"
Output: "mnop"
```

**Example 2:**
```
Input: str1 = "dynamic", str2 = "program"
Output: "dynprogramic"
```

### 💡 Approach: Backtrack with All Characters

**Intuition:**
- Build DP table for LCS
- Backtrack but include ALL characters:
  - If match: include once
  - If no match: include character from path taken
- Include remaining characters from both strings

**Code:**
```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    for index1 in range(n1+1):
        dp[index1][0] = 0
    for index2 in range(n2+1):
        dp[0][index2] = 0
    
    # Build LCS table
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
    dp = tabulation(s1, s2)
    index1, index2 = n1, n2
    ans = []
    
    while index1 > 0 and index2 > 0:
        if s1[index1-1] == s2[index2-1]:
            # Match: include once
            ans.append(s1[index1-1])
            index1 -= 1
            index2 -= 1
        else:
            # No match: include from path taken
            if dp[index1-1][index2] > dp[index1][index2-1]:
                ans.append(s1[index1-1])
                index1 -= 1
            else:
                ans.append(s2[index2-1])
                index2 -= 1
    
    # Add remaining characters
    while index1 > 0:
        ans.append(s1[index1-1])
        index1 -= 1
    while index2 > 0:
        ans.append(s2[index2-1])
        index2 -= 1
    
    ans = "".join(ans[::-1])
    return ans
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

---

## 7. Distinct Subsequences

### 📝 Problem Description
Count the number of distinct subsequences of string `s` that equal string `t`.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: s = "axbxax", t = "axa"
Output: 2
Explanation: Two ways to form "axa"
```

**Example 2:**
```
Input: s = "babgbag", t = "bag"
Output: 5
```

### 💡 Approach 1: Recursion

**Intuition:**
- If t is empty (index2 < 0): found 1 valid way
- If s is empty but t not (index1 < 0): 0 ways
- If characters match: count both using and not using current character
- If no match: move to next character in s

**Code:**
```python
def solve(s1, s2, index1, index2):
    # Target string exhausted: valid subsequence
    if index2 < 0:
        return 1
    # Source exhausted but target not: invalid
    if index1 < 0:
        return 0
    
    if s1[index1] == s2[index2]:
        # Count both: use this match + don't use this match
        way1 = solve(s1, s2, index1-1, index2)      # Don't use
        way2 = solve(s1, s2, index1-1, index2-1)    # Use
        return way1 + way2
    else:
        # No match: move to next character in s1
        return solve(s1, s2, index1-1, index2)
```

**Complexity:**
- **Time:** O(2^n)
- **Space:** O(n)

### 💡 Approach 2: Memoization

**Code:**
```python
def solve_memo(s1, s2, index1, index2, memo):
    if index2 < 0:
        return 1
    if index1 < 0:
        return 0
    
    # Return cached result
    if memo[index1][index2] is not None:
        return memo[index1][index2]
    
    if s1[index1] == s2[index2]:
        # Two ways: use match or skip
        way1 = solve_memo(s1, s2, index1-1, index2, memo)
        way2 = solve_memo(s1, s2, index1-1, index2-1, memo)
        memo[index1][index2] = way1 + way2
    else:
        # Skip current character in s1
        memo[index1][index2] = solve_memo(s1, s2, index1-1, index2, memo)
    
    return memo[index1][index2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

### 💡 Approach 3: Tabulation

**Code:**
```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base case: empty target, 1 way
    for index1 in range(n1+1):
        dp[index1][0] = 1
    
    # Base case: empty source but non-empty target, 0 ways
    for index2 in range(n2+1):
        dp[0][index2] = 0
    dp[0][0] = 1  # Both empty
    
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # Add: ways using match + ways not using
                way1 = dp[index1-1][index2]
                way2 = dp[index1-1][index2-1]
                dp[index1][index2] = way1 + way2
            else:
                # Only option: skip current in s1
                dp[index1][index2] = dp[index1-1][index2]
    
    return dp[n1][n2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

### 💡 Approach 4: Space Optimized

**Code:**
```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    
    # Base cases
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
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(m)

---

## 8. Edit Distance

### 📝 Problem Description
Find minimum operations (insert, delete, replace) to convert string `start` to `target`.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: start = "planet", target = "plan"
Output: 2
Explanation: Delete 'e' and 't'
```

**Example 2:**
```
Input: start = "abcdefg", target = "azced"
Output: 4
```

### 💡 Approach 1: Recursion

**Intuition:**
- If start exhausted: insert remaining target characters
- If target exhausted: delete remaining start characters
- If match: no operation needed
- If no match: try insert, delete, or replace (take minimum)

**Code:**
```python
def solve(s1, s2, index1, index2):
    # Need to insert remaining characters from s2
    if index1 < 0:
        return index2 + 1
    # Need to delete remaining characters from s1
    if index2 < 0:
        return index1 + 1
    
    # Characters match: no operation
    if s1[index1] == s2[index2]:
        return solve(s1, s2, index1-1, index2-1)
    else:
        # Try all 3 operations
        update = 1 + solve(s1, s2, index1-1, index2-1)  # Replace
        insert = 1 + solve(s1, s2, index1, index2-1)    # Insert
        delete = 1 + solve(s1, s2, index1-1, index2)    # Delete
        return min(update, insert, delete)
```

**Complexity:**
- **Time:** O(3^(n+m))
- **Space:** O(n+m)

### 💡 Approach 2: Memoization

**Code:**
```python
def solve_memo(s1, s2, index1, index2, memo):
    if index1 < 0:
        return index2 + 1
    if index2 < 0:
        return index1 + 1
    
    # Check cache
    if memo[index1][index2] is not None:
        return memo[index1][index2]
    
    if s1[index1] == s2[index2]:
        memo[index1][index2] = solve_memo(s1, s2, index1-1, index2-1, memo)
    else:
        update = 1 + solve_memo(s1, s2, index1-1, index2-1, memo)
        insert = 1 + solve_memo(s1, s2, index1, index2-1, memo)
        delete = 1 + solve_memo(s1, s2, index1-1, index2, memo)
        memo[index1][index2] = min(update, insert, delete)
    
    return memo[index1][index2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

### 💡 Approach 3: Tabulation

**Code:**
```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base cases: converting from/to empty string
    for index1 in range(n1+1):
        dp[index1][0] = index1  # Delete all
    for index2 in range(n2+1):
        dp[0][index2] = index2  # Insert all
    
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                # No operation needed
                dp[index1][index2] = dp[index1-1][index2-1]
            else:
                # Minimum of 3 operations
                update = 1 + dp[index1-1][index2-1]
                insert = 1 + dp[index1][index2-1]
                delete = 1 + dp[index1-1][index2]
                dp[index1][index2] = min(update, insert, delete)
    
    return dp[n1][n2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

### 💡 Approach 4: Space Optimized

**Code:**
```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    
    # Base case for previous row
    dp_curr[0] = 0
    for index2 in range(n2+1):
        dp_prev[index2] = index2
    
    for index1 in range(1, n1+1):
        # Base case for current row
        dp_curr[0] = index1
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1]:
                dp_curr[index2] = dp_prev[index2-1]
            else:
                update = 1 + dp_prev[index2-1]
                insert = 1 + dp_curr[index2-1]
                delete = 1 + dp_prev[index2]
                dp_curr[index2] = min(update, insert, delete)
        dp_prev = dp_curr.copy()
    
    return dp_prev[n2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(m)

---

## 9. Wildcard Matching

### 📝 Problem Description
Match string with pattern containing '?' (any single char) and '*' (any sequence).

### 🧪 Sample Test Cases

**Example 1:**
```
Input: str = "xaylmz", pat = "x?y*z"
Output: true
Explanation: '?' matches 'a', '*' matches "lm"
```

**Example 2:**
```
Input: str = "xyza", pat = "x*z"
Output: false
Explanation: Extra 'a' at end not matched
```

### 💡 Approach 1: Recursion

**Intuition:**
- Both exhausted: match found
- Pattern exhausted but string not: no match
- String exhausted but pattern has only '*': check if all remaining are '*'
- If characters match or pattern is '?': move both
- If pattern is '*': try matching 0 or more characters

**Code:**
```python
def solve(s1, s2, index1, index2):
    # Both exhausted: match
    if index1 < 0 and index2 < 0:
        return True
    # Pattern exhausted but string not: no match
    if index1 < 0 and index2 >= 0:
        return False
    # String exhausted: check if remaining pattern is all '*'
    if index2 < 0 and index1 >= 0:
        return all(s1[i] == '*' for i in range(index1 + 1))
    
    # Characters match or '?'
    if s1[index1] == s2[index2] or s1[index1] == "?":
        return solve(s1, s2, index1-1, index2-1)
    else:
        if s1[index1] == "*":
            # Try: match 0 chars OR match 1+ chars
            exclude = solve(s1, s2, index1-1, index2)    # Match 0
            include = solve(s1, s2, index1, index2-1)    # Match 1+
            return exclude or include
        else:
            # No match possible
            return False
```

**Complexity:**
- **Time:** O(2^(n+m))
- **Space:** O(n+m)

### 💡 Approach 2: Memoization

**Code:**
```python
def solve_memo(s1, s2, index1, index2, memo):
    if index1 < 0 and index2 < 0:
        return True
    if index1 < 0 and index2 > 0:
        return False
    if index2 < 0 and index1 >= 0:
        return all(s1[i] == '*' for i in range(index1 + 1))
    
    # Check cache
    if memo[index1][index2] is not None:
        return memo[index1][index2]
    
    if s1[index1] == s2[index2] or s1[index1] == '?':
        memo[index1][index2] = solve_memo(s1, s2, index1-1, index2-1, memo)
    else:
        if s1[index1] == "*":
            # Try both options
            exclude = solve_memo(s1, s2, index1-1, index2, memo)
            include = solve_memo(s1, s2, index1, index2-1, memo)
            memo[index1][index2] = exclude or include
        else:
            memo[index1][index2] = False
    
    return memo[index1][index2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

### 💡 Approach 3: Tabulation

**Code:**
```python
def tabulation(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp = [[None]*(n2+1) for _ in range(n1+1)]
    
    # Base case: both empty
    dp[0][0] = True
    
    # Base case: pattern empty, string not empty
    for index2 in range(1, n2+1):
        dp[0][index2] = False
    
    # Base case: string empty, pattern has only '*'
    for index1 in range(1, n1+1):
        if s1[index1-1] == "*":
            dp[index1][0] = True
        else:
            break  # First non-'*' breaks the chain
    
    for index1 in range(1, n1+1):
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1] or s1[index1-1] == "?":
                # Match or wildcard
                dp[index1][index2] = dp[index1-1][index2-1]
            else:
                if s1[index1-1] == "*":
                    # Match 0 chars OR match 1+ chars
                    exclude = dp[index1-1][index2]
                    include = dp[index1][index2-1]
                    dp[index1][index2] = exclude or include
                else:
                    dp[index1][index2] = False
    
    return dp[n1][n2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(n × m)

### 💡 Approach 4: Space Optimized

**Code:**
```python
def optimized(s1, s2):
    n1, n2 = len(s1), len(s2)
    dp_curr = [None]*(n2+1)
    dp_prev = [None]*(n2+1)
    
    # Base cases
    for index2 in range(n2+1):
        dp_prev[index2] = False
    dp_prev[0] = True
    
    for index1 in range(1, n1+1):
        # Base case: string empty, check if all '*'
        dp_curr[0] = (s1[index1-1] == "*" and dp_prev[0] == True)
        
        for index2 in range(1, n2+1):
            if s1[index1-1] == s2[index2-1] or s1[index1-1] == "?":
                dp_curr[index2] = dp_prev[index2-1]
            else:
                if s1[index1-1] == "*":
                    exclude = dp_prev[index2]
                    include = dp_curr[index2-1]
                    dp_curr[index2] = exclude or include
                else:
                    dp_curr[index2] = False
        dp_prev = dp_curr.copy()
    
    return dp_prev[n2]
```

**Complexity:**
- **Time:** O(n × m)
- **Space:** O(m)

---

## 📊 Problem Comparison Table

| Problem | Type | Key Insight | Best Time | Best Space |
|---------|------|-------------|-----------|------------|
| **1. LCS** | Subsequence | Match→include, else try both | O(n×m) | O(m) |
| **2. Print LCS** | Backtracking | Backtrack from DP table | O(n×m) | O(n×m) |
| **3. Longest Substring** | Substring | Match→extend, else reset to 0 | O(n×m) | O(m) |
| **4. LPS** | Palindrome | LCS(s, reverse(s)) | O(n²) | O(n) |
| **5. Min Insertions** | Palindrome | n - LPS(s) | O(n²) | O(n) |
| **6. SCS** | Supersequence | Backtrack including all chars | O(n×m) | O(n×m) |
| **7. Distinct Subseq** | Counting | Match→add both ways | O(n×m) | O(m) |
| **8. Edit Distance** | Operations | Min(insert, delete, replace) | O(n×m) | O(m) |
| **9. Wildcard Match** | Pattern Match | Handle '?' and '*' | O(n×m) | O(m) |

---

## 🎯 Quick Selection Guide

### By Problem Type
- **Subsequence problems**: LCS, LPS, Distinct Subsequences
- **Substring problems**: Longest Common Substring
- **Transformation**: Edit Distance, Min Insertions
- **Combination**: Shortest Common Supersequence
- **Pattern Matching**: Wildcard Matching

### By Technique
- **Standard DP**: LCS, Edit Distance
- **DP + Backtracking**: Print LCS, SCS
- **Using LCS**: LPS, Min Insertions, SCS
- **Counting Ways**: Distinct Subsequences
- **Special Characters**: Wildcard Matching

---

## 🔑 Common Patterns

### 1. **Two String DP Template**
```
States: dp[index1][index2]
Base cases:
- index1 < 0: handle remaining of s2
- index2 < 0: handle remaining of s1
Transitions:
- If match: decision based on problem
- If no match: try excluding from one or both
```

### 2. **Index Shifting (0-based vs 1-based)**
- **Recursion**: Use 0-based (index1-1, index2-1)
- **Tabulation**: Use 1-based (extra row/column for base case)
- **Access string**: Always use `s[index-1]` in tabulation

### 3. **Space Optimization**
- Only need previous row → O(m) space
- Use `dp_curr` and `dp_prev` arrays
- Remember to copy: `dp_prev = dp_curr.copy()`

### 4. **Base Case Patterns**

**LCS/Edit Distance:**
```python
dp[i][0] = i  # or 0 depending on problem
dp[0][j] = j  # or 0 depending on problem
```

**Distinct Subsequences:**
```python
dp[i][0] = 1  # Empty target: 1 way
dp[0][j] = 0  # Empty source: 0 ways
```

**Wildcard Matching:**
```python
dp[0][0] = True  # Both empty: match
dp[0][j] = False  # Pattern empty: no match
dp[i][0] = all '*' check
```

---

## 💭 Key Insights

### Problem-Specific Tricks

1. **LCS**: Foundation for many problems (LPS, Min Insertions, SCS)

2. **Print LCS/SCS**: Backtrack based on DP values:
   - Match → move diagonal
   - No match → move to larger value direction

3. **Substring vs Subsequence**:
   - Subsequence: Can skip characters
   - Substring: Must be contiguous (reset to 0 on mismatch)

4. **Palindrome Problems**: Use reverse string with LCS

5. **Distinct Subsequences**: When match, ADD both ways (use + skip)

6. **Edit Distance**: Consider all 3 operations, take minimum

7. **Wildcard**: 
   - '?' matches exactly 1 character
   - '*' can match 0 or more (two recursive calls)

### Optimization Path
```
Recursion (TLE)
    ↓
Memoization (Accepted)
    ↓
Tabulation (Better)
    ↓
Space Optimized (Optimal)
```

### Common Mistakes to Avoid
- ❌ Forgetting to use `index-1` for string access in tabulation
- ❌ Not copying array in space optimization (`dp_prev = dp_curr` instead of `.copy()`)
- ❌ Wrong base cases (especially for distinct subsequences and wildcard)
- ❌ Confusing subsequence with substring logic1 = "mnop", str2 = "mnq"
