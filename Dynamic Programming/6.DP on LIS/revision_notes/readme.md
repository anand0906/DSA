1. [Longest Increasing Subsequence (LIS)](#1-longest-increasing-subsequence-lis)
2. [LIS using Binary Search](#2-lis-using-binary-search)
3. [Print Longest Increasing Subsequence](#3-print-longest-increasing-subsequence)
4. [Largest Divisible Subset](#4-largest-divisible-subset)
5. [Longest String Chain](#5-longest-string-chain)
6. [Longest Bitonic Subsequence](#6-longest-bitonic-subsequence)
7. [Number of Longest Increasing Subsequences](#7-number-of-longest-increasing-subsequences)
8. [Comparison Table](#-comparison-table)

---

## 1. Longest Increasing Subsequence (LIS)

### Problem Description
Given an integer array `nums`, return the length of the longest strictly increasing subsequence. A subsequence is derived from an array by deleting some or no elements without changing the order of the remaining elements.

### Examples
```
Input: nums = [10, 9, 2, 5, 3, 7, 101, 18]
Output: 4
Explanation: LIS is [2, 3, 7, 101]

Input: nums = [0, 1, 0, 3, 2, 3]
Output: 4
Explanation: LIS is [0, 1, 2, 3]
```

### Approach 1: Recursion
**Intuition:** At each index, we have two choices - include the current element if it's greater than the last picked element, or exclude it. Try both and take maximum.

```python
def solve(n, arr, index, last_index):
    if index == n:
        return 0
    
    include = 0
    if last_index == -1 or arr[index] > arr[last_index]:
        include = 1 + solve(n, arr, index + 1, index)
    exclude = solve(n, arr, index + 1, last_index)
    return max(include, exclude)
```
- **Time Complexity:** O(2^n)
- **Space Complexity:** O(n) - recursion stack

### Approach 2: Memoization
**Intuition:** Cache the results of overlapping subproblems using a 2D memo table indexed by current position and last picked index.

```python
def solve_memo(n, arr, index, last_index, memo):
    if memo[index][last_index + 1] != None:
        return memo[index][last_index + 1]
    if index == n:
        return 0
    include = 0
    if last_index == -1 or arr[index] > arr[last_index]:
        include = 1 + solve_memo(n, arr, index + 1, index, memo)
    exclude = solve_memo(n, arr, index + 1, last_index, memo)
    memo[index][last_index + 1] = max(include, exclude)
    return memo[index][last_index + 1]
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n²)

### Approach 3: Tabulation
**Intuition:** Build solution bottom-up using a 2D DP table where `dp[index][last_index]` represents the LIS length from index onwards with last_index as the previously picked element.

```python
def tabulation(n, arr):
    dp = [[None] * (n + 1) for _ in range(n + 1)]
    for last_index in range(n + 1):
        dp[n][last_index] = 0
    for index in range(n - 1, -1, -1):
        for last_index in range(n - 1, -2, -1):
            include = 0
            if last_index == -1 or arr[index] > arr[last_index]:
                include = 1 + dp[index + 1][index + 1]
            exclude = dp[index + 1][last_index + 1]
            dp[index][last_index + 1] = max(include, exclude)
    return dp[0][0]
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n²)

### Approach 4: Space Optimized
**Intuition:** Since we only need the next row to compute the current row, we can use two 1D arrays instead of a 2D table.

```python
def optimized(n, arr):
    dp_curr = [None] * (n + 1)
    dp_next = [None] * (n + 1)
    for last_index in range(n + 1):
        dp_next[last_index] = 0
    for index in range(n - 1, -1, -1):
        for last_index in range(n - 1, -2, -1):
            include = 0
            if last_index == -1 or arr[index] > arr[last_index]:
                include = 1 + dp_next[index + 1]
            exclude = dp_next[last_index + 1]
            dp_curr[last_index + 1] = max(include, exclude)
        dp_next = dp_curr.copy()
    return dp_next[0]
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n)

### Approach 5: Algorithmic (Best DP)
**Intuition:** For each element, check all previous elements. If current element is greater, we can extend that subsequence. `dp[i]` stores the length of LIS ending at index i.

```python
def algorithmic(n, arr):
    dp = [1] * (n + 1)
    for index in range(n):
        for last_index in range(index):
            if arr[index] > arr[last_index]:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    return max(dp)
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n)

---

## 2. LIS using Binary Search

### Problem Description
Same as problem 1, but optimized using binary search for better time complexity.

### Examples
```
Input: nums = [10, 9, 2, 5, 3, 7, 101, 18]
Output: 4

Input: nums = [0, 1, 0, 3, 2, 3]
Output: 4
```

### Approach: Binary Search + Greedy
**Intuition:** Maintain an array `temp` where we build potential LIS. For each element, if it's larger than the last element in `temp`, append it. Otherwise, find the smallest element in `temp` that is >= current element and replace it. This keeps `temp` as small as possible for future extensions.

```python
import bisect

def solve(n, arr):
    temp = [arr[0]]
    for index in range(1, n):
        if arr[index] > temp[-1]:
            temp.append(arr[index])
        else:
            lower_bound = bisect.bisect_left(temp, arr[index])
            temp[lower_bound] = arr[index]
    return len(temp)
```
- **Time Complexity:** O(n log n)
- **Space Complexity:** O(n)

---

## 3. Print Longest Increasing Subsequence

### Problem Description
Return the actual Longest Increasing Subsequence that is index-wise lexicographically smallest.

### Examples
```
Input: arr = [10, 22, 9, 33, 21, 50, 41, 60, 80]
Output: [10, 22, 33, 50, 60, 80]

Input: arr = [1, 3, 2, 4, 6, 5]
Output: [1, 3, 4, 6]
Explanation: [1, 3, 4, 6] is lexicographically smaller than [1, 2, 4, 6]
```

### Approach: DP with Parent Tracking
**Intuition:** Use the same DP approach as LIS, but additionally maintain a parent array to track which element was picked before the current one. Backtrack from the element with maximum LIS length to reconstruct the sequence.

```python
def solve(n, arr):
    dp = [1] * n
    parent = {}
    for index in range(n):
        parent[index] = index
        for last_index in range(index):
            if arr[index] > arr[last_index]:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
                    parent[index] = last_index
    
    maxLength = 0
    maxIndex = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
            maxIndex = i
    
    temp = []
    index = maxIndex
    while parent[index] != index:
        temp.append(arr[index])
        index = parent[index]
    temp.append(arr[index])
    ans = temp[::-1]
    return ans
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n)

---

## 4. Largest Divisible Subset

### Problem Description
Find the largest subset where every pair (a, b) satisfies `a % b == 0` or `b % a == 0`.

### Examples
```
Input: nums = [3, 5, 10, 20]
Output: [5, 10, 20]

Input: nums = [16, 8, 2, 4, 32]
Output: [2, 4, 8, 16, 32]
```

### Approach 1: Length Only
**Intuition:** Sort the array first. After sorting, if `arr[i] % arr[j] == 0`, then all elements in the subset ending at j will also divide arr[i]. This transforms into an LIS-like problem with divisibility condition.

```python
def lengthOfLargestSubset(n, arr):
    arr.sort()
    dp = [1] * n
    for index in range(n):
        for last_index in range(index):
            if arr[index] % arr[last_index] == 0:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    maxLength = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
    return maxLength
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n)

### Approach 2: Print Subset
**Intuition:** Same as above but track parent pointers to reconstruct the actual subset.

```python
def printLargestSubset(n, arr):
    arr.sort()
    dp = [1] * n
    parent = {}
    for index in range(n):
        parent[index] = index
        for last_index in range(index):
            if arr[index] % arr[last_index] == 0:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
                    parent[index] = last_index
    
    maxLength = 0
    maxIndex = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
            maxIndex = i
    
    temp = []
    index = maxIndex
    while parent[index] != index:
        temp.append(arr[index])
        index = parent[index]
    temp.append(arr[index])
    ans = temp[::-1]
    return ans
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n)

---

## 5. Longest String Chain

### Problem Description
A word chain is formed when each word is a predecessor of the next (can insert exactly one letter). Find the length of the longest possible word chain.

### Examples
```
Input: words = ["a", "ab", "abc", "abcd", "abcde"]
Output: 5

Input: words = ["dog", "dogs", "dots", "dot", "d", "do"]
Output: 4
Explanation: Chain is ["d", "do", "dot", "dots"]
```

### Helper Function: Compare Words
```python
def compare(word1, word2):
    n, m = len(word1), len(word2)
    if n != m + 1:
        return False
    i, j = 0, 0
    while i < n:
        if j < m and word1[i] == word2[j]:
            i += 1
            j += 1
        else:
            i += 1
    if i == n and j == m:
        return True
    return False
```

### Approach 1: Length Only
**Intuition:** Sort words by length. For each word, check all previous shorter words. If current word can be formed by adding one character to a previous word, extend that chain.

```python
def lengthOfLongestChain(n, arr):
    arr.sort(key=len)
    dp = [1] * n
    for index in range(n):
        for last_index in range(index):
            if compare(arr[index], arr[last_index]):
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    maxLength = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
    return maxLength
```
- **Time Complexity:** O(n² × L) where L is max word length
- **Space Complexity:** O(n)

### Approach 2: Print Chain
**Intuition:** Track parent pointers to reconstruct the actual chain.

```python
def printLongestChain(n, arr):
    arr.sort(key=len)
    dp = [1] * n
    parent = {}
    for index in range(n):
        parent[index] = index
        for last_index in range(index):
            if compare(arr[index], arr[last_index]):
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
                    parent[index] = last_index
    
    maxLength = 0
    maxIndex = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
            maxIndex = i
    
    temp = []
    index = maxIndex
    while parent[index] != index:
        temp.append(arr[index])
        index = parent[index]
    temp.append(arr[index])
    ans = temp[::-1]
    return ans
```
- **Time Complexity:** O(n² × L)
- **Space Complexity:** O(n)

---

## 6. Longest Bitonic Subsequence

### Problem Description
Find the length of the longest bitonic sequence (first increases, then decreases). The sequence doesn't have to be contiguous.

### Examples
```
Input: arr = [5, 1, 4, 2, 3, 6, 8, 7]
Output: 6
Explanation: Bitonic sequence is [1, 2, 3, 6, 8, 7]

Input: arr = [10, 20, 30, 40, 50, 40, 30, 20]
Output: 8
Explanation: Entire array is bitonic
```

### Approach: Two LIS Arrays
**Intuition:** For each position, calculate the LIS ending at that position (increasing part) and LIS starting from that position in reverse (decreasing part). The bitonic length at position i is `increasing[i] + decreasing[i] - 1`.

```python
def longestIncreasing(n, arr):
    dp = [1] * n
    for index in range(n):
        for last_index in range(index):
            if arr[index] > arr[last_index]:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
    return dp

def solve(n, arr):
    increasing = longestIncreasing(n, arr)
    temp = longestIncreasing(n, arr[::-1])
    decreasing = temp[::-1]
    maxi = 0
    for i in range(n):
        length = increasing[i] + decreasing[i] - 1
        maxi = max(maxi, length)
    return maxi
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n)

---

## 7. Number of Longest Increasing Subsequences

### Problem Description
Find the count of all Longest Increasing Subsequences in the array.

### Examples
```
Input: nums = [1, 3, 5, 4, 7]
Output: 2
Explanation: Two LIS: [1, 3, 4, 7] and [1, 3, 5, 7]

Input: nums = [2, 2, 2, 2, 2]
Output: 5
Explanation: Each element forms an LIS of length 1
```

### Approach: DP with Count Array
**Intuition:** Along with `dp[i]` for LIS length, maintain `ways[i]` to count how many ways we can form that length. When we find a longer sequence, copy the count. When we find equal length, add the counts.

```python
def solve(n, arr):
    dp = [1] * n
    ways = [1] * n
    for index in range(n):
        for last_index in range(index):
            if arr[index] > arr[last_index]:
                length = 1 + dp[last_index]
                if length > dp[index]:
                    dp[index] = length
                    ways[index] = ways[last_index]
                elif length == dp[index]:
                    ways[index] += ways[last_index]
    
    maxLength = 0
    for i in range(n):
        if dp[i] > maxLength:
            maxLength = dp[i]
    
    total = 0
    for i in range(n):
        if dp[i] == maxLength:
            total += ways[i]
    return total
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n)

---

## 📊 Comparison Table

| Problem | Key Difference | Time Complexity | Space Complexity | Special Technique |
|---------|---------------|-----------------|------------------|-------------------|
| **LIS** | Basic problem | O(n²) | O(n) | Standard DP |
| **LIS Binary Search** | Optimized approach | O(n log n) | O(n) | Binary search + greedy |
| **Print LIS** | Need actual sequence | O(n²) | O(n) | Parent tracking |
| **Divisible Subset** | Divisibility condition | O(n²) | O(n) | Sort first, then DP |
| **String Chain** | String predecessor | O(n² × L) | O(n) | Sort by length, compare function |
| **Bitonic Subsequence** | Increase then decrease | O(n²) | O(n) | Two LIS (forward + backward) |
| **Count LIS** | Count all LIS | O(n²) | O(n) | Additional count array |

### Key Patterns
- **Standard LIS:** Check if current > previous, extend subsequence
- **With Printing:** Add parent tracking to backtrack the sequence
- **Binary Search:** Use when only length is needed, not the sequence
- **Bitonic:** Combine two LIS calculations (forward and backward)
- **Counting:** Maintain additional count array for combinatorics
- **Modified Conditions:** Replace `arr[i] > arr[j]` with problem-specific condition (divisibility, string chain, etc.)

### When to Use Which Approach
- **O(n²) DP:** When you need the actual sequence or count variations
- **O(n log n) Binary Search:** When only length matters and optimal time is needed
- **Parent Tracking:** When printing/reconstructing the actual subsequence
- **Sorting:** Problems with divisibility or length-based relationships
