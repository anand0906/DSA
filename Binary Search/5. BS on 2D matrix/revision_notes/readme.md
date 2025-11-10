# Binary Search - 2D Matrix Problems

## Table of Contents
1. [Find Row with Maximum 1's](#1-find-row-with-maximum-1s)
2. [Search in a 2D Matrix](#2-search-in-a-2d-matrix)
3. [Search in 2D Matrix - II](#3-search-in-2d-matrix---ii)
4. [Find Peak Element - II](#4-find-peak-element---ii)
5. [Matrix Median](#5-matrix-median)
6. [Comparison Summary](#comparison-summary)

---

## 1. Find Row with Maximum 1's

### Problem Description
Given a non-empty grid `mat` consisting of only 0s and 1s, where all rows are sorted in ascending order, find the index of the row with the maximum number of ones. If two rows have the same number of ones, return the smaller index. If no 1 exists, return -1.

### Sample Test Cases

```
Input: mat = [[1, 1, 1], [0, 0, 1], [0, 0, 0]]
Output: 0

Input: mat = [[0, 0], [0, 0]]
Output: -1
```

---

### Approach 1: Brute Force

**Intuition:**
Count the number of 1s in each row and track the row with maximum count.

**Code:**
```python
def solve(matrix):
    n, m = len(matrix), len(matrix[0])
    maxiIndex = -1
    maxiCnt = 0
    
    # Check each row
    for i in range(n):
        cnt = 0
        # Count 1s in current row
        for j in range(m):
            if matrix[i][j] == 1:
                cnt += 1
        
        # Update if current row has more 1s
        if cnt > maxiCnt:
            maxiCnt = cnt
            maxiIndex = i
    
    return maxiIndex
```

**Time Complexity:** O(n × m)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search on Each Row

**Intuition:**
Since each row is sorted, use binary search to find the first occurrence of 1 in each row. The count of 1s is `m - firstIndex`.

**Code:**
```python
def cntOnes(n, arr):
    low, high = 0, n-1
    ans = 0
    
    # Find first occurrence of 1
    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == 1:
            ans = n - mid  # Count from first 1 to end
            high = mid - 1  # Search for earlier 1
        else:
            low = mid + 1
    
    return ans

def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    maxiIndex = -1
    maxiCnt = 0
    
    # Use binary search for each row
    for i in range(n):
        cnt = cntOnes(m, matrix[i])
        
        # Update if current row has more 1s
        if cnt > maxiCnt:
            maxiCnt = cnt
            maxiIndex = i
    
    return maxiIndex
```

**Time Complexity:** O(n × log m)  
**Space Complexity:** O(1)

---

## 2. Search in a 2D Matrix

### Problem Description
Given a 2D array `mat` where each row is sorted in non-decreasing order and the first element of each row is greater than the last element of the previous row, determine if a target exists in the matrix.

### Sample Test Cases

```
Input: mat = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]], target = 8
Output: True

Input: mat = [[1, 2, 4], [6, 7, 8], [9, 10, 34]], target = 78
Output: False
```

---

### Approach 1: Linear Search

**Intuition:**
Check every element in the matrix.

**Code:**
```python
def solve(matrix, target):
    n, m = len(matrix), len(matrix[0])
    
    # Check each element
    for i in range(n):
        for j in range(m):
            if matrix[i][j] == target:
                return True
    
    return False
```

**Time Complexity:** O(n × m)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search (Treat as 1D Array)

**Intuition:**
Since rows are sorted and each row starts greater than the previous row's end, treat the 2D matrix as a flattened 1D sorted array. Convert 1D index to 2D coordinates using `row = index // m` and `col = index % m`.

**Code:**
```python
import math

def optimized(matrix, target):
    n, m = len(matrix), len(matrix[0])
    low, high = 0, n*m - 1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Convert 1D index to 2D coordinates
        row = mid // m
        col = mid % m
        
        # Check target
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] < target:
            low = mid + 1
        else:
            high = mid - 1
    
    return False
```

**Time Complexity:** O(log(n × m))  
**Space Complexity:** O(1)

---

## 3. Search in 2D Matrix - II

### Problem Description
Given a 2D array `matrix` where each row is sorted in ascending order from left to right and each column is sorted in ascending order from top to bottom, search for a specific integer `target`.

### Sample Test Cases

```
Input: matrix = [[1, 4, 7, 11, 15], [2, 5, 8, 12, 19], [3, 6, 9, 16, 22], [10, 13, 14, 17, 24], [18, 21, 23, 26, 30]], target = 5
Output: True

Input: matrix = [[1, 4, 7, 11, 15], [2, 5, 8, 12, 19], [3, 6, 9, 16, 22], [10, 13, 14, 17, 24], [18, 21, 23, 26, 30]], target = 20
Output: False
```

---

### Approach 1: Brute Force

**Intuition:**
Check every element in the matrix.

**Code:**
```python
def solve(matrix, target):
    n, m = len(matrix), len(matrix[0])
    
    # Check each element
    for i in range(n):
        for j in range(m):
            if matrix[i][j] == target:
                return True
    
    return False
```

**Time Complexity:** O(n × m)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search on Each Row

**Intuition:**
Apply binary search on each row since rows are sorted.

**Code:**
```python
def search(n, arr, target):
    low, high = 0, n-1
    
    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return True
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    
    return False

def solve2(matrix, target):
    n, m = len(matrix), len(matrix[0])
    
    # Binary search on each row
    for i in range(n):
        if search(m, matrix[i], target):
            return True
    
    return False
```

**Time Complexity:** O(n × log m)  
**Space Complexity:** O(1)

---

### Approach 3: Staircase Search

**Intuition:**
Start from top-right corner. If current element > target, move left (eliminate column). If current element < target, move down (eliminate row). This works because rows and columns are both sorted.

**Code:**
```python
import math

def optimized(matrix, target):
    n, m = len(matrix), len(matrix[0])
    row, col = 0, m-1  # Start from top-right corner
    
    while row < n and col >= 0:
        # Found target
        if matrix[row][col] == target:
            return True
        # Current element > target, move left
        elif matrix[row][col] > target:
            col -= 1
        # Current element < target, move down
        else:
            row += 1
    
    return False
```

**Time Complexity:** O(n + m)  
**Space Complexity:** O(1)

---

## 4. Find Peak Element - II

### Problem Description
Given an n × m matrix `mat` where no two adjacent cells are equal, find any peak element. A peak element is strictly greater than all its adjacent neighbors (left, right, top, bottom). Assume the matrix is surrounded by -1.

### Sample Test Cases

```
Input: mat = [[10, 20, 15], [21, 30, 14], [7, 16, 32]]
Output: [1, 1]

Input: mat = [[10, 7], [11, 17]]
Output: [1, 1]
```

---

### Approach 1: Brute Force

**Intuition:**
Check each element to see if it's greater than all its neighbors.

**Code:**
```python
def solve(matrix):
    n, m = len(matrix), len(matrix[0])
    
    # Check each element
    for i in range(n):
        for j in range(m):
            flag = True
            
            # Check all 4 neighbors
            for r, c in zip([-1, 1, 0, 0], [0, 0, -1, 1]):
                row = i + r
                col = j + c
                
                # If neighbor exists and is greater, not a peak
                if (row >= 0 and col >= 0 and row < n and col < m 
                    and matrix[row][col] > matrix[i][j]):
                    flag = False
            
            # Found a peak
            if flag:
                return [i, j]
    
    return [0, 0]
```

**Time Complexity:** O(n × m)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search on Columns

**Intuition:**
Apply binary search on columns. For the middle column, find the maximum element. If it's greater than its left and right neighbors, it's a peak. Otherwise, move toward the larger neighbor (a peak must exist in that direction).

**Code:**
```python
def findmaxIndex(n, col, matrix):
    maxIndex = 0
    maxi = 0
    
    # Find row with maximum element in given column
    for i in range(n):
        if matrix[i][col] > maxi:
            maxi = matrix[i][col]
            maxIndex = i
    
    return maxIndex

def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    low, high = 0, m-1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Find max element in middle column
        maxInd = findmaxIndex(n, mid, matrix)
        
        # Get left and right neighbors
        left = matrix[maxInd][mid-1] if mid-1 >= 0 else -1
        right = matrix[maxInd][mid+1] if mid+1 < m else -1
        
        # Check if it's a peak
        if matrix[maxInd][mid] > left and matrix[maxInd][mid] > right:
            return [maxInd, mid]
        else:
            # Move toward larger neighbor
            if left > matrix[maxInd][mid]:
                high = mid - 1
            else:
                low = mid + 1
    
    return [0, 0]
```

**Time Complexity:** O(n × log m)  
**Space Complexity:** O(1)

---

## 5. Matrix Median

### Problem Description
Given a 2D array `matrix` that is row-wise sorted, find the median of the entire matrix. The total number of elements (n × m) is always odd.

### Sample Test Cases

```
Input: matrix = [[1, 4, 9], [2, 5, 6], [3, 7, 8]]
Output: 5
Explanation: Sorted array [1, 2, 3, 4, 5, 6, 7, 8, 9], median = 5

Input: matrix = [[1, 3, 8], [2, 3, 4], [1, 2, 5]]
Output: 3
Explanation: Sorted array [1, 1, 2, 2, 3, 3, 4, 5, 8], median = 3
```

---

### Approach 1: Flatten and Sort

**Intuition:**
Store all elements in an array, sort it, and return the middle element.

**Code:**
```python
def solve(matrix):
    n, m = len(matrix), len(matrix[0])
    temp = []
    
    # Flatten the matrix
    for i in range(n):
        for j in range(m):
            temp.append(matrix[i][j])
    
    # Sort and find median
    temp.sort()
    mid = (m * n) // 2
    return temp[mid]
```

**Time Complexity:** O(n × m × log(n × m))  
**Space Complexity:** O(n × m)

---

### Approach 2: Binary Search on Answer

**Intuition:**
The median is the element where exactly ⌈(n×m)/2⌉ elements are ≤ to it. Binary search on the value range [min, max] of the matrix. For each mid value, count how many elements are ≤ mid using binary search on each row.

**Code:**
```python
def findCount(n, m, matrix, element):
    cnt = 0
    
    # Count elements <= element in each row
    for i in range(n):
        arr = matrix[i]
        low, high = 0, m-1
        largeIndex = m
        
        # Find first element > element
        while low <= high:
            mid = (low + high) // 2
            if matrix[i][mid] > element:
                largeIndex = mid
                high = mid - 1
            else:
                low = mid + 1
        
        # Count elements <= element in this row
        currentCnt = largeIndex
        cnt += currentCnt
    
    return cnt

import math

def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    
    # Find min and max in matrix
    low = float('inf')
    high = float('-inf')
    for i in range(n):
        if matrix[i][0] < low:
            low = matrix[i][0]
        if matrix[i][m-1] > high:
            high = matrix[i][m-1]
    
    # Target: at least this many elements should be <= median
    targetCnt = math.ceil((n * m) / 2)
    ans = 0
    
    # Binary search on answer
    while low <= high:
        mid = (low + high) // 2
        
        # Count elements <= mid
        cnt = findCount(n, m, matrix, mid)
        
        # If enough elements <= mid, mid could be median
        if cnt >= targetCnt:
            ans = mid
            high = mid - 1  # Try smaller values
        else:
            low = mid + 1  # Need larger values
    
    return ans
```

**Time Complexity:** O(n × log m × log(max - min))  
**Space Complexity:** O(1)

---

## Comparison Summary

| # | Problem | Brute Force | Optimized Approach | Key Technique |
|---|---------|-------------|-------------------|---------------|
| 1 | Row with Max 1's | O(n×m) / O(1) | O(n×log m) / O(1) | Binary search per row for first 1 |
| 2 | Search in 2D Matrix | O(n×m) / O(1) | O(log(n×m)) / O(1) | Treat as flattened 1D array |
| 3 | Search in 2D Matrix II | O(n×m) / O(1) | O(n+m) / O(1) | Staircase search from corner |
| 4 | Find Peak Element II | O(n×m) / O(1) | O(n×log m) / O(1) | Binary search on columns |
| 5 | Matrix Median | O(n×m×log(n×m)) / O(n×m) | O(n×log m×log(range)) / O(1) | Binary search on answer |

### Quick Reference Pattern Guide

**Row-wise Sorted Matrix:**
- Used for: Problems 1, 5
- Key: Apply binary search on each row independently
- Optimization: Can count/search efficiently per row

**Fully Sorted Matrix (1D style):**
- Used for: Problem 2
- Key: Treat as flattened array with index conversion
- Formula: `row = index // m`, `col = index % m`

**Row & Column Sorted Matrix:**
- Used for: Problem 3
- Key: Staircase search from top-right or bottom-left
- Strategy: Eliminate entire row or column at each step

**2D Peak Finding:**
- Used for: Problem 4
- Key: Binary search on one dimension, linear search on other
- Guarantee: Moving toward larger neighbor ensures peak exists

**Binary Search on Answer (2D):**
- Used for: Problem 5
- Key: Binary search on value range, count elements efficiently
- Application: Finding kth element, median, percentile in matrix

### Key Insights:
1. **Row-wise sorted**: Binary search per row reduces O(n×m) to O(n×log m)
2. **Fully sorted (2D → 1D)**: Can achieve O(log(n×m)) with proper indexing
3. **Staircase search**: Works when both rows and columns are sorted, eliminates row/column at each step
4. **2D peak**: Binary search on columns + find max in column = O(n×log m)
5. **Binary search on answer**: When direct indexing is hard, search on value range and count occurrences
