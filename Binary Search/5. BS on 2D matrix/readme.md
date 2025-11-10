# Find Row with Maximum 1's

## Problem Description

Given a non-empty grid `mat` consisting of only 0s and 1s, where all the rows are sorted in ascending order, find the index of the row with the maximum number of ones.

**Rules:**
- If two rows have the same number of ones, return the row with the smaller index
- If no 1 exists in the matrix, return -1
- All rows are sorted in ascending order (0s before 1s)

**Notes on constraints:**
- The sorted property of rows is crucial and allows for binary search optimization
- For large matrices, an O(n*m) approach may be too slow; consider O(n*log(m)) solutions

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: mat = [[1, 1, 1], [0, 0, 1], [0, 0, 0]]
Output: 0
```
**Explanation:** Row 0 has 3 ones, row 1 has 1 one, and row 2 has 0 ones. Row 0 has the maximum count, so we return index 0.

**Test Case 2:**
```
Input: mat = [[0, 0], [0, 0]]
Output: -1
```
**Explanation:** The matrix does not contain any 1. Since no ones exist, we return -1 as specified.

**Test Case 3:**
```
Input: mat = [[0, 1, 1], [0, 1, 1], [1, 1, 1]]
Output: 2
```
**Explanation:** Row 0 has 2 ones, row 1 has 2 ones, and row 2 has 3 ones. Row 2 has the maximum count, so we return index 2.

**Test Case 4:**
```
Input: mat = [[0, 0, 1], [0, 0, 1]]
Output: 0
```
**Explanation:** Both rows have 1 one each. When counts are equal, we return the smaller index, which is 0.

## Approaches

### Approach 1: Brute Force - Count All Ones

**Summary:** Iterate through each row and count all ones by checking every element.

**Intuition**

The simplest approach is to traverse each row completely and count the number of ones in each row. We keep track of the maximum count seen so far and the corresponding row index. This approach doesn't utilize the sorted property of rows but is straightforward to implement.

**Approach Steps**

1. Initialize `maxiIndex` to -1 (for the case when no ones exist) and `maxiCnt` to 0
2. For each row in the matrix:
   - Initialize a counter `cnt` to 0
   - Traverse all elements in the current row
   - Increment `cnt` for each 1 encountered
   - If `cnt` is greater than `maxiCnt`, update `maxiCnt` and `maxiIndex`
3. Return `maxiIndex`

**Example**

Let's trace through `mat = [[0, 1, 1], [1, 1, 1], [0, 0, 1]]`:

**Initial state:** `maxiIndex = -1`, `maxiCnt = 0`

**Row 0:** `[0, 1, 1]`
- Count ones: 0 (skip), 1 (cnt=1), 1 (cnt=2)
- `cnt = 2 > maxiCnt = 0` → Update: `maxiCnt = 2`, `maxiIndex = 0`

**Row 1:** `[1, 1, 1]`
- Count ones: 1 (cnt=1), 1 (cnt=2), 1 (cnt=3)
- `cnt = 3 > maxiCnt = 2` → Update: `maxiCnt = 3`, `maxiIndex = 1`

**Row 2:** `[0, 0, 1]`
- Count ones: 0 (skip), 0 (skip), 1 (cnt=1)
- `cnt = 1 < maxiCnt = 3` → No update

**Result:** Return `maxiIndex = 1`

**Code**

```python
def solve(matrix):
    n, m = len(matrix), len(matrix[0])
    maxiIndex = -1  # Store the index of row with maximum ones
    maxiCnt = 0     # Store the maximum count of ones
    
    # Iterate through each row
    for i in range(n):
        cnt = 0
        # Count ones in current row
        for j in range(m):
            if(matrix[i][j] == 1):
                cnt += 1
        
        # Update if current row has more ones
        if(cnt > maxiCnt):
            maxiCnt = cnt
            maxiIndex = i
    
    return maxiIndex
```

**Time Complexity**

O(n * m) — We traverse all n rows and for each row, we check all m columns. This results in checking every element in the matrix exactly once.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables `maxiIndex`, `maxiCnt`, `cnt`, and loop counters.

---

### Approach 2: Binary Search - Optimized Count

**Summary:** Use binary search to count ones in each sorted row, leveraging the fact that rows are sorted.

**Intuition**

Since each row is sorted in ascending order (all 0s come before all 1s), we can use binary search to find the first occurrence of 1 in each row. Once we find the first 1, all elements to the right are also 1s. This allows us to count ones in O(log m) time instead of O(m) time per row.

**Approach Steps**

1. Create a helper function `cntOnes` that uses binary search to count ones in a sorted array:
   - Search for the leftmost position where 1 appears
   - Return the count as (total elements - first position of 1)
2. Initialize `maxiIndex` to -1 and `maxiCnt` to 0
3. For each row in the matrix:
   - Use `cntOnes` to count ones in O(log m) time
   - If count is greater than `maxiCnt`, update both `maxiCnt` and `maxiIndex`
4. Return `maxiIndex`

**Example**

Let's trace through `mat = [[0, 0, 1, 1], [0, 1, 1, 1], [0, 0, 0, 1]]`:

**Row 0:** `[0, 0, 1, 1]`
- Binary search for first 1:
  - `low=0, high=3, mid=1` → arr[1]=0 → move right: `low=2`
  - `low=2, high=3, mid=2` → arr[2]=1 → found! `ans=4-2=2`
- Count = 2, Update: `maxiCnt = 2`, `maxiIndex = 0`

**Row 1:** `[0, 1, 1, 1]`
- Binary search for first 1:
  - `low=0, high=3, mid=1` → arr[1]=1 → found! `ans=4-1=3`
  - Continue searching left: `high=0, mid=0` → arr[0]=0 → `low=1`
  - Final: `ans=3`
- Count = 3 > 2, Update: `maxiCnt = 3`, `maxiIndex = 1`

**Row 2:** `[0, 0, 0, 1]`
- Binary search for first 1:
  - Find first 1 at index 3 → `ans=4-3=1`
- Count = 1 < 3, No update

**Result:** Return `maxiIndex = 1`

**Code**

```python
def cntOnes(n, arr):
    low, high = 0, n - 1
    ans = 0  # Store count of ones
    
    # Binary search to find the first occurrence of 1
    while low <= high:
        mid = (low + high) // 2
        if(arr[mid] == 1):
            ans = n - mid  # All elements from mid to end are 1
            high = mid - 1  # Search in left half for earlier occurrence
        else:
            low = mid + 1  # Search in right half
    
    return ans

def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    maxiIndex = -1  # Store the index of row with maximum ones
    maxiCnt = 0     # Store the maximum count of ones
    
    # Iterate through each row
    for i in range(n):
        # Count ones using binary search
        cnt = cntOnes(m, matrix[i])
        
        # Update if current row has more ones
        if(cnt > maxiCnt):
            maxiCnt = cnt
            maxiIndex = i
    
    return maxiIndex
```

**Time Complexity**

O(n * log m) — We iterate through n rows, and for each row, we perform binary search which takes O(log m) time. This is significantly better than O(n * m) when m is large.


**Space Complexity**

O(1) — We only use a constant amount of extra space for variables and the binary search doesn't use any additional data structures.

---

# Search in a 2D Matrix

## Problem Description

Given a 2-D array `mat` where the elements of each row are sorted in non-decreasing order, and the first element of a row is greater than the last element of the previous row (if it exists), and an integer `target`, determine if the target exists in the given `mat` or not.

**Properties of the matrix:**
- Each row is sorted in non-decreasing order
- The first element of each row is greater than the last element of the previous row
- This means the entire matrix can be viewed as a sorted array when read row by row

**Notes on constraints:**
- The special property (first element of row > last element of previous row) allows treating the 2D matrix as a flattened sorted 1D array
- For large matrices (e.g., n*m = 10^6), an O(n*m) linear search may be too slow; an O(log(n*m)) binary search solution is preferred

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: mat = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]], target = 8
Output: True
```
**Explanation:** The target 8 exists in the matrix at position (1, 3) - row index 1, column index 3. Therefore, we return True.

**Test Case 2:**
```
Input: mat = [[1, 2, 4], [6, 7, 8], [9, 10, 34]], target = 78
Output: False
```
**Explanation:** The target 78 does not exist anywhere in the matrix. The largest element is 34, which is less than 78. Therefore, we return False.

**Test Case 3:**
```
Input: mat = [[1, 3, 5], [7, 9, 11]], target = 1
Output: True
```
**Explanation:** The target 1 exists at position (0, 0) - the first element of the matrix. We return True.

**Test Case 4:**
```
Input: mat = [[10, 20, 30]], target = 25
Output: False
```
**Explanation:** The matrix has only one row [10, 20, 30]. The target 25 is not present (it would fall between 20 and 30). We return False.

## Approaches

### Approach 1: Brute Force - Linear Search

**Summary:** Iterate through every element in the matrix and check if it equals the target.

**Intuition**

The simplest approach is to check every element in the 2D matrix one by one. We use two nested loops to traverse all rows and columns, comparing each element with the target. If we find a match, we return True immediately. If we finish checking all elements without finding the target, we return False. This approach doesn't utilize the sorted property of the matrix.

**Approach Steps**

1. Get the dimensions of the matrix: `n` rows and `m` columns
2. Use a nested loop to traverse each element:
   - Outer loop iterates through rows (0 to n-1)
   - Inner loop iterates through columns (0 to m-1)
3. For each element at position (i, j):
   - If `matrix[i][j]` equals `target`, return True
4. If the loops complete without finding the target, return False

**Example**

Let's trace through `mat = [[1, 3, 5], [7, 9, 11], [13, 15, 17]]`, `target = 9`:

**Row 0:** Check [1, 3, 5]
- Element (0,0): 1 ≠ 9
- Element (0,1): 3 ≠ 9
- Element (0,2): 5 ≠ 9

**Row 1:** Check [7, 9, 11]
- Element (1,0): 7 ≠ 9
- Element (1,1): 9 = 9 ✓ → **Return True**

We found the target at position (1, 1), so we immediately return True without checking the remaining elements.

**Code**

```python
def solve(matrix, target):
    n, m = len(matrix), len(matrix[0])
    
    # Iterate through each row
    for i in range(n):
        # Iterate through each column
        for j in range(m):
            # Check if current element matches target
            if(matrix[i][j] == target):
                return True 
    
    # Target not found in entire matrix
    return False
```

**Time Complexity**

O(n * m) — We potentially traverse all n rows and m columns, checking every element in the matrix exactly once in the worst case (when target doesn't exist or is at the last position).

**Space Complexity**

O(1) — We only use a constant amount of extra space for loop variables and no additional data structures.

---

### Approach 2: Binary Search - Treat as Flattened Sorted Array

**Summary:** Use binary search on the 2D matrix by treating it as a virtually flattened 1D sorted array.

**Intuition**

Since each row is sorted and the first element of each row is greater than the last element of the previous row, the entire matrix can be conceptually viewed as a single sorted 1D array. Instead of actually flattening the array (which would take O(n*m) space and time), we can use index mapping to perform binary search directly on the 2D structure. We map a 1D index to 2D coordinates: `row = index // m` and `col = index % m`.

**Approach Steps**

1. Get matrix dimensions: `n` rows and `m` columns
2. Treat the matrix as a virtual array of length `n * m`
3. Set binary search bounds: `low = 0`, `high = n * m - 1`
4. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - Convert 1D index to 2D coordinates: `row = mid // m`, `col = mid % m`
   - Compare `matrix[row][col]` with `target`:
     - If equal: return True
     - If less than target: search right half (set `low = mid + 1`)
     - If greater than target: search left half (set `high = mid - 1`)
5. If loop ends without finding target, return False

**Example**

Let's trace through `mat = [[1, 3, 5, 7], [10, 11, 16, 20], [23, 30, 34, 60]]`, `target = 16`:

**Matrix details:** n=3, m=4, total elements=12

**Iteration 1:**
- `low=0, high=11, mid=5`
- Convert to 2D: `row=5//4=1, col=5%4=1`
- `matrix[1][1] = 11 < 16` → Search right: `low=6`

**Iteration 2:**
- `low=6, high=11, mid=8`
- Convert to 2D: `row=8//4=2, col=8%4=0`
- `matrix[2][0] = 23 > 16` → Search left: `high=7`

**Iteration 3:**
- `low=6, high=7, mid=6`
- Convert to 2D: `row=6//4=1, col=6%4=2`
- `matrix[1][2] = 16 = 16` ✓ → **Return True**

We found the target in just 3 comparisons instead of potentially checking up to 12 elements.

**Code**

```python
import math

def optimized(matrix, target):
    n, m = len(matrix), len(matrix[0])
    
    # Binary search on virtual 1D array
    low, high = 0, n * m - 1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Convert 1D index to 2D coordinates
        row = mid // m
        col = mid % m
        
        # Compare middle element with target
        if(matrix[row][col] == target):
            return True 
        elif(matrix[row][col] < target):
            low = mid + 1  # Search in right half
        else:
            high = mid - 1  # Search in left half
    
    # Target not found
    return False
```

**Time Complexity**

O(log(n * m)) — We perform binary search on a virtual array of size n*m. Each iteration reduces the search space by half, resulting in logarithmic time complexity. This is equivalent to O(log n + log m).

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables (low, high, mid, row, col). No additional data structures are created, and we don't actually flatten the matrix.

---
