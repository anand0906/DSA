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

# Search in 2D Matrix - II

## Problem Description

Given a 2D array `matrix` where each row is sorted in ascending order from left to right and each column is sorted in ascending order from top to bottom, write an efficient algorithm to search for a specific integer `target` in the matrix.

**Properties of the matrix:**
- Each row is sorted in ascending order (left to right)
- Each column is sorted in ascending order (top to bottom)
- Unlike the previous problem, the first element of a row is NOT necessarily greater than the last element of the previous row

**Notes on constraints:**
- The matrix has both row-wise and column-wise sorting, but rows are not strictly ordered relative to each other
- For large matrices (e.g., n*m = 10^6), an O(n*m) linear search is inefficient
- Optimal solutions should aim for O(n + m) or O(n log m) time complexity

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: matrix = [[1, 4, 7, 11, 15], [2, 5, 8, 12, 19], [3, 6, 9, 16, 22], [10, 13, 14, 17, 24], [18, 21, 23, 26, 30]], target = 5
Output: True
```
**Explanation:** The target 5 exists in the matrix at position (1, 1) - row index 1, column index 1. Therefore, we return True.

**Test Case 2:**
```
Input: matrix = [[1, 4, 7, 11, 15], [2, 5, 8, 12, 19], [3, 6, 9, 16, 22], [10, 13, 14, 17, 24], [18, 21, 23, 26, 30]], target = 20
Output: False
```
**Explanation:** The target 20 does not exist anywhere in the matrix. If we check all positions, no element equals 20. Therefore, we return False.

**Test Case 3:**
```
Input: matrix = [[1, 3, 5], [2, 4, 6], [7, 8, 9]], target = 4
Output: True
```
**Explanation:** The target 4 is found at position (1, 1). We return True.

**Test Case 4:**
```
Input: matrix = [[-5]], target = -5
Output: True
```
**Explanation:** The matrix contains a single element which matches the target. We return True.

## Approaches

### Approach 1: Brute Force - Linear Search

**Summary:** Iterate through every element in the matrix and check if it equals the target.

**Intuition**

The most straightforward approach is to check every element in the 2D matrix sequentially. We use two nested loops to traverse all rows and columns, comparing each element with the target. If we find a match, we return True immediately. If we finish checking all elements without finding the target, we return False. This approach doesn't utilize any of the sorted properties of the matrix.

**Approach Steps**

1. Get the dimensions of the matrix: `n` rows and `m` columns
2. Use a nested loop to traverse each element:
   - Outer loop iterates through rows (0 to n-1)
   - Inner loop iterates through columns (0 to m-1)
3. For each element at position (i, j):
   - If `matrix[i][j]` equals `target`, return True
4. If the loops complete without finding the target, return False

**Example**

Let's trace through `mat = [[1, 4], [2, 5], [3, 6]]`, `target = 5`:

**Row 0:** Check [1, 4]
- Element (0,0): 1 ≠ 5
- Element (0,1): 4 ≠ 5

**Row 1:** Check [2, 5]
- Element (1,0): 2 ≠ 5
- Element (1,1): 5 = 5 ✓ → **Return True**

We found the target at position (1, 1), so we immediately return True without checking the remaining elements in row 2.

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

O(n * m) — We potentially traverse all n rows and m columns, checking every element in the matrix exactly once in the worst case (when the target doesn't exist or is at the last position).

**Space Complexity**

O(1) — We only use a constant amount of extra space for loop variables and no additional data structures are created.

---

### Approach 2: Binary Search on Each Row

**Summary:** Apply binary search on each row individually, leveraging the fact that each row is sorted.

**Intuition**

Since each row is sorted independently, we can perform binary search on each row to check if the target exists. Instead of checking all m elements in a row linearly (O(m)), binary search allows us to search each row in O(log m) time. We iterate through all n rows and apply binary search on each one. If any row contains the target, we return True.

**Approach Steps**

1. Create a helper function `search` that performs binary search on a sorted array:
   - Use standard binary search with `low` and `high` pointers
   - Return True if target is found, False otherwise
2. Get matrix dimensions: `n` rows and `m` columns
3. For each row in the matrix:
   - Call `search` function on the current row
   - If search returns True, immediately return True
4. If no row contains the target, return False

**Example**

Let's trace through `mat = [[1, 3, 5], [2, 4, 6], [7, 9, 11]]`, `target = 4`:

**Row 0:** Binary search in [1, 3, 5]
- `low=0, high=2, mid=1`: arr[1]=3 < 4 → `low=2`
- `low=2, high=2, mid=2`: arr[2]=5 > 4 → `high=1`
- `low>high` → Not found

**Row 1:** Binary search in [2, 4, 6]
- `low=0, high=2, mid=1`: arr[1]=4 = 4 ✓ → **Return True**

We found the target in row 1 using binary search, which took only 1 comparison instead of potentially 3 linear checks.

**Code**

```python
def search(n, arr, target):
    low, high = 0, n - 1
    
    # Standard binary search
    while low <= high:
        mid = (low + high) // 2
        if(arr[mid] == target):
            return True
        elif(arr[mid] < target):
            low = mid + 1
        else:
            high = mid - 1
    
    return False

def solve2(matrix, target):
    n, m = len(matrix), len(matrix[0])
    
    # Apply binary search on each row
    for i in range(n):
        if(search(m, matrix[i], target)):
            return True 
    
    # Target not found in any row
    return False
```

**Time Complexity**

O(n * log m) — We iterate through n rows, and for each row, we perform binary search which takes O(log m) time. This is better than O(n * m) when m is large.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables in the binary search and main function. No additional data structures are created.

---

### Approach 3: Staircase Search - Optimal Solution

**Summary:** Start from top-right (or bottom-left) corner and eliminate either a row or column in each step.

**Intuition**

This approach leverages both the row-wise and column-wise sorting properties simultaneously. Starting from the top-right corner, we observe that:
- All elements to the left are smaller (row is sorted ascending)
- All elements below are larger (column is sorted ascending)

This gives us a "decision point" where we can eliminate either an entire column (move left if current > target) or an entire row (move down if current < target). We continue until we find the target or run out of bounds. This creates a "staircase" pattern of movement through the matrix.

**Approach Steps**

1. Get matrix dimensions: `n` rows and `m` columns
2. Initialize position at top-right corner: `row = 0`, `col = m - 1`
3. While `row < n` and `col >= 0`:
   - If `matrix[row][col]` equals `target`, return True
   - If `matrix[row][col]` is greater than `target`:
     - All elements below in this column are even larger, so eliminate this column
     - Move left: `col = col - 1`
   - If `matrix[row][col]` is less than `target`:
     - All elements to the left in this row are even smaller, so eliminate this row
     - Move down: `row = row + 1`
4. If we exit the bounds without finding the target, return False

**Example**

Let's trace through `mat = [[1, 4, 7], [2, 5, 8], [3, 6, 9]]`, `target = 5`:

**Initial position:** `row=0, col=2` → `matrix[0][2] = 7`

**Step 1:**
- Current element: 7 > 5
- Eliminate column 2 (all elements below 7 are larger)
- Move left: `col=1`

**Step 2:** `row=0, col=1` → `matrix[0][1] = 4`
- Current element: 4 < 5
- Eliminate row 0 (all elements to the left of 4 are smaller)
- Move down: `row=1`

**Step 3:** `row=1, col=1` → `matrix[1][1] = 5`
- Current element: 5 = 5 ✓ → **Return True**

We found the target in just 3 steps, eliminating entire rows/columns at each step!

**Code**

```python
import math

def optimized(matrix, target):
    n, m = len(matrix), len(matrix[0])
    
    # Start from top-right corner
    row, col = 0, m - 1
    
    # Continue until we go out of bounds
    while row < n and col >= 0:
        if(matrix[row][col] == target):
            return True 
        elif(matrix[row][col] > target):
            col -= 1  # Eliminate current column, move left
        else:
            row += 1  # Eliminate current row, move down
    
    # Target not found
    return False
```

**Time Complexity**

O(n + m) — In the worst case, we traverse from the top-right corner to the bottom-left corner, moving either down or left at each step. We can move down at most n times and left at most m times, giving us a maximum of n + m steps.

**Space Complexity**

O(1) — We only use a constant amount of extra space for the `row` and `col` variables. No additional data structures are created, and we don't use recursion.

---

# Find Peak Element - II

## Problem Description

Given a 0-indexed n x m matrix `mat` where no two adjacent cells are equal, find any peak element `mat[i][j]` and return the array `[i, j]`.

A peak element in a 2D grid is an element that is strictly greater than all of its adjacent neighbours to the left, right, top, and bottom.

**Important properties:**
- No two adjacent cells are equal
- The entire matrix is surrounded by an outer perimeter with the value -1 in each cell (conceptually)
- Multiple peak elements may exist; any valid peak can be returned
- Adjacent neighbors are only in 4 directions: left, right, top, bottom (not diagonal)

**Notes on constraints:**
- For large matrices (e.g., n*m = 10^6), an O(n*m) solution checking every element may be too slow
- Optimal solutions should aim for O(n log m) or O(m log n) time complexity using binary search techniques

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: mat = [[10, 20, 15], [21, 30, 14], [7, 16, 32]]
Output: [1, 1]
```
**Explanation:** The value at index [1, 1] is 30. Its neighbors are: left=21, right=14, top=20, bottom=16. Since 30 > 21, 30 > 14, 30 > 20, and 30 > 16, it is a peak element. Note that [2, 2] with value 32 is also a valid peak.

**Test Case 2:**
```
Input: mat = [[10, 7], [11, 17]]
Output: [1, 1]
```
**Explanation:** The value at index [1, 1] is 17. Its neighbors are: left=11, top=7. Since 17 > 11 and 17 > 7 (and right/bottom are treated as -1), it is a peak element.

**Test Case 3:**
```
Input: mat = [[1, 4], [3, 2]]
Output: [0, 1]
```
**Explanation:** The value at index [0, 1] is 4. Its neighbors are: left=1, bottom=2 (top and right are -1). Since 4 > 1 and 4 > 2, it is a peak element.

**Test Case 4:**
```
Input: mat = [[5]]
Output: [0, 0]
```
**Explanation:** A single element matrix where the element 5 has no actual neighbors (all boundaries are -1), making it a peak by definition.

## Approaches

### Approach 1: Brute Force - Check Every Element

**Summary:** Check every element in the matrix to see if it's greater than all its adjacent neighbors.

**Intuition**

The straightforward approach is to examine each element in the matrix and verify if it satisfies the peak condition by comparing it with all its adjacent neighbors (up, down, left, right). For each element, we check all four possible neighbors, handling boundary cases where neighbors don't exist (treating them as -1). If an element is greater than all its existing neighbors, it's a peak and we return its position.

**Approach Steps**

1. Get matrix dimensions: `n` rows and `m` columns
2. For each cell at position (i, j):
   - Initialize a flag as True (assume it's a peak)
   - Check all 4 adjacent neighbors using direction vectors:
     - Up: (-1, 0), Down: (1, 0), Left: (0, -1), Right: (0, 1)
   - For each valid neighbor (within bounds):
     - If neighbor value > current value, set flag to False and break
   - If flag remains True after checking all neighbors, return [i, j]
3. If no peak is found (shouldn't happen by problem guarantee), return [0, 0]

**Example**

Let's trace through `mat = [[10, 20], [15, 25]]`, finding a peak:

**Position (0, 0):** Value = 10
- Right neighbor (0, 1): 20 > 10 → Not a peak (flag = False)

**Position (0, 1):** Value = 20
- Left neighbor (0, 0): 10 < 20 ✓
- Down neighbor (1, 1): 25 > 20 → Not a peak (flag = False)

**Position (1, 0):** Value = 15
- Up neighbor (0, 0): 10 < 15 ✓
- Right neighbor (1, 1): 25 > 15 → Not a peak (flag = False)

**Position (1, 1):** Value = 25
- Up neighbor (0, 1): 20 < 25 ✓
- Left neighbor (1, 0): 15 < 25 ✓
- All neighbors are smaller → **Peak found! Return [1, 1]**

**Code**

```python
def solve(matrix):
    n, m = len(matrix), len(matrix[0])
    
    # Check every element in the matrix
    for i in range(n):
        for j in range(m):
            flag = True
            
            # Check all 4 adjacent neighbors (up, down, left, right)
            for r, c in zip([-1, 1, 0, 0], [0, 0, -1, 1]):
                row = i + r
                col = j + c
                
                # If neighbor is within bounds and greater than current element
                if(row >= 0 and col >= 0 and row < n and col < m and matrix[row][col] > matrix[i][j]):
                    flag = False
            
            # If current element is greater than all neighbors
            if(flag):
                return [i, j]
    
    return [0, 0]
```

**Time Complexity**

O(n * m) — We potentially check all n*m elements in the matrix, and for each element, we check at most 4 neighbors which is O(1). In the worst case, we examine every element before finding a peak.

**Space Complexity**

O(1) — We only use a constant amount of extra space for loop variables, the flag, and direction vectors. No additional data structures proportional to input size are created.

---

### Approach 2: Binary Search on Columns - Optimal Solution

**Summary:** Use binary search on columns and find the maximum element in each middle column to guide the search.

**Intuition**

This approach uses binary search on the columns of the matrix. For the middle column, we find the row with the maximum element. This element is a candidate for a peak. We then compare it with its left and right neighbors:
- If it's greater than both neighbors, it's a peak
- If the left neighbor is greater, a peak must exist in the left half (columns 0 to mid-1)
- If the right neighbor is greater, a peak must exist in the right half (columns mid+1 to m-1)

This works because if we move towards a higher neighbor, we're guaranteed to find a peak (since boundaries are treated as -1). The maximum element in a column ensures we don't have to worry about top/bottom neighbors being larger.

**Approach Steps**

1. Create a helper function `findmaxIndex` to find the row index of the maximum element in a given column
2. Initialize binary search on columns: `low = 0`, `high = m - 1`
3. While `low <= high`:
   - Calculate middle column: `mid = (low + high) // 2`
   - Find the row with maximum value in this column: `maxInd`
   - Get left and right neighbors (treating out-of-bounds as -1)
   - If `matrix[maxInd][mid]` is greater than both neighbors, return `[maxInd, mid]`
   - Else if left neighbor is greater, search left half: `high = mid - 1`
   - Else search right half: `low = mid + 1`
4. Return [0, 0] if no peak found (shouldn't happen)

**Example**

Let's trace through `mat = [[10, 20, 15], [21, 30, 14], [7, 16, 32]]`:

**Initial:** `low = 0, high = 2`

**Iteration 1:**
- `mid = 1` (middle column: [20, 30, 16])
- Find max in column 1: `maxInd = 1` (value 30)
- Left neighbor: `matrix[1][0] = 21`
- Right neighbor: `matrix[1][2] = 14`
- Check: 30 > 21 ✓ and 30 > 14 ✓
- **Peak found! Return [1, 1]**

The algorithm found the peak in just one iteration by using binary search on columns and finding the maximum element in the middle column.

**Another example:** `mat = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]`:

**Iteration 1:**
- `mid = 1`, column [2, 5, 8], `maxInd = 2` (value 8)
- Left: 7, Right: 9
- 8 < 9, so move right: `low = 2`

**Iteration 2:**
- `mid = 2`, column [3, 6, 9], `maxInd = 2` (value 9)
- Left: 8, Right: -1 (boundary)
- 9 > 8 and 9 > -1 ✓
- **Peak found! Return [2, 2]**

**Code**

```python
def findmaxIndex(n, col, matrix):
    maxIndex = 0
    maxi = 0
    
    # Find the row with maximum element in the given column
    for i in range(n):
        if(matrix[i][col] > maxi):
            maxi = matrix[i][col]
            maxIndex = i
    
    return maxIndex

def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    low, high = 0, m - 1
    
    # Binary search on columns
    while low <= high:
        mid = (low + high) // 2
        
        # Find row with maximum element in middle column
        maxInd = findmaxIndex(n, mid, matrix)
        
        # Get left and right neighbors (treat out-of-bounds as -1)
        left = matrix[maxInd][mid - 1] if(mid - 1 >= 0) else -1
        right = matrix[maxInd][mid + 1] if(mid + 1 < m) else -1
        
        # Check if current element is a peak
        if(matrix[maxInd][mid] > left and matrix[maxInd][mid] > right):
            return [maxInd, mid]
        else:
            # Move towards the greater neighbor
            if(left > matrix[maxInd][mid]):
                high = mid - 1  # Search in left half
            else:
                low = mid + 1  # Search in right half
    
    return [0, 0]
```

**Time Complexity**

O(n log m) — We perform binary search on columns which takes O(log m) iterations. In each iteration, we find the maximum element in a column which takes O(n) time. Therefore, total time is O(n * log m).

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables (low, high, mid, maxInd, left, right). No additional data structures proportional to input size are created, and we don't use recursion.

----

# Matrix Median

## Problem Description

Given a 2D array `matrix` that is row-wise sorted, find the median of the given matrix.

**Important properties:**
- Each row is sorted in non-decreasing order
- The median is the middle element when all elements are arranged in sorted order
- N*M is always odd (guaranteed by constraints), so median is at position (N*M)/2 in 0-indexed sorted array

**Constraints:**
- 1 ≤ N, M ≤ 10^5
- 1 ≤ N*M ≤ 10^6
- 1 ≤ matrix[i][j] ≤ 10^9
- N*M is odd

**Notes on constraints:**
- For large matrices (N*M up to 10^6), an O(N*M log(N*M)) sorting approach may be acceptable but not optimal
- The row-wise sorted property allows for binary search optimizations
- Optimal solution should aim for O(N*M) space avoidance and use O(N log M * log(range)) time

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: matrix = [[1, 4, 9], [2, 5, 6], [3, 7, 8]]
Output: 5
```
**Explanation:** When we flatten and sort the matrix, we get [1, 2, 3, 4, 5, 6, 7, 8, 9]. The total number of elements is 9 (odd). The median is at index 9//2 = 4, which is the element 5.

**Test Case 2:**
```
Input: matrix = [[1, 3, 8], [2, 3, 4], [1, 2, 5]]
Output: 3
```
**Explanation:** When we flatten and sort the matrix, we get [1, 1, 2, 2, 3, 3, 4, 5, 8]. The total number of elements is 9 (odd). The median is at index 9//2 = 4, which is the element 3.

**Test Case 3:**
```
Input: matrix = [[1, 2, 3, 4, 5]]
Output: 3
```
**Explanation:** A single row with 5 elements. The median is at index 5//2 = 2, which is the element 3.

**Test Case 4:**
```
Input: matrix = [[5], [3], [1], [7], [9]]
Output: 5
```
**Explanation:** A single column with 5 elements. When sorted: [1, 3, 5, 7, 9]. The median is at index 5//2 = 2, which is the element 5.

## Approaches

### Approach 1: Brute Force - Flatten and Sort

**Summary:** Flatten the 2D matrix into a 1D array, sort it, and return the middle element.

**Intuition**

The most straightforward approach is to convert the 2D matrix into a 1D array by collecting all elements, then sort this array to find the median. Since the median is the middle element in a sorted sequence and we're guaranteed that N*M is odd, we can directly access the element at position (N*M)//2 after sorting. This approach is simple to implement but doesn't leverage the row-wise sorted property of the matrix.

**Approach Steps**

1. Get matrix dimensions: `n` rows and `m` columns
2. Create an empty array `temp` to store all elements
3. Iterate through each row and column:
   - Append each element to `temp`
4. Sort the `temp` array in ascending order
5. Calculate the median index: `mid = (n * m) // 2`
6. Return `temp[mid]`

**Example**

Let's trace through `mat = [[1, 5, 7], [3, 6, 9], [2, 4, 8]]`:

**Step 1: Flatten the matrix**
- Row 0: Add 1, 5, 7 → temp = [1, 5, 7]
- Row 1: Add 3, 6, 9 → temp = [1, 5, 7, 3, 6, 9]
- Row 2: Add 2, 4, 8 → temp = [1, 5, 7, 3, 6, 9, 2, 4, 8]

**Step 2: Sort the array**
- temp = [1, 2, 3, 4, 5, 6, 7, 8, 9]

**Step 3: Find median**
- Total elements: 9
- Median index: 9 // 2 = 4
- **Median value: temp[4] = 5**

**Code**

```python
def solve(matrix):
    n, m = len(matrix), len(matrix[0])
    temp = []
    
    # Flatten the 2D matrix into 1D array
    for i in range(n):
        for j in range(m):
            temp.append(matrix[i][j])
    
    # Sort the flattened array
    temp.sort()
    
    # Find the median index (middle element)
    mid = (m * n) // 2
    
    return temp[mid]
```

**Time Complexity**

O(N*M log(N*M)) — We first collect all N*M elements in O(N*M) time, then sort them which takes O(N*M log(N*M)) time. The sorting dominates the overall complexity.

**Space Complexity**

O(N*M) — We create a temporary array `temp` to store all N*M elements from the matrix. This additional space is proportional to the total number of elements.

---

### Approach 2: Binary Search on Value Range - Optimal Solution

**Summary:** Use binary search on the range of possible values and count elements smaller than or equal to mid value.

**Intuition**

Instead of actually sorting all elements, we can binary search on the value range (from minimum to maximum element in the matrix). For each candidate value in this range, we count how many elements in the matrix are less than or equal to it. The median is the smallest value that has at least (N*M+1)/2 elements less than or equal to it. We leverage the row-wise sorted property to count elements efficiently using binary search on each row.

**Approach Steps**

1. Create a helper function `findCount` that counts elements ≤ a given value:
   - For each row, use binary search to find the count of elements ≤ value
   - Sum up counts from all rows
2. Find the value range:
   - `low` = minimum of all first column elements (smallest in matrix)
   - `high` = maximum of all last column elements (largest in matrix)
3. Calculate target count: `targetCnt = ceil((n*m)/2)` (number of elements before/at median)
4. Binary search on value range:
   - For `mid` value, count elements ≤ mid
   - If count ≥ targetCnt, this could be median; store it and search left
   - Otherwise, search right
5. Return the found answer

**Example**

Let's trace through `mat = [[1, 3, 5], [2, 6, 9], [3, 6, 9]]`:

**Step 1: Find value range**
- `low` = min(1, 2, 3) = 1
- `high` = max(5, 9, 9) = 9
- Total elements = 9, `targetCnt` = ceil(9/2) = 5

**Step 2: Binary search on values**

**Iteration 1:** `low=1, high=9, mid=5`
- Count elements ≤ 5:
  - Row 0 [1,3,5]: 3 elements ≤ 5
  - Row 1 [2,6,9]: 1 element ≤ 5
  - Row 2 [3,6,9]: 1 element ≤ 5
  - Total: 5 elements
- 5 ≥ 5 → Update ans=5, search left: `high=4`

**Iteration 2:** `low=1, high=4, mid=2`
- Count elements ≤ 2:
  - Row 0: 1 element (only 1)
  - Row 1: 1 element (only 2)
  - Row 2: 0 elements
  - Total: 2 elements
- 2 < 5 → Search right: `low=3`

**Iteration 3:** `low=3, high=4, mid=3`
- Count elements ≤ 3:
  - Row 0: 2 elements (1, 3)
  - Row 1: 1 element (2)
  - Row 2: 1 element (3)
  - Total: 4 elements
- 4 < 5 → Search right: `low=4`

**Iteration 4:** `low=4, high=4, mid=4`
- Count elements ≤ 4:
  - Total: 4 elements
- 4 < 5 → Search right: `low=5`

**Exit:** `low > high`, **Return ans = 5**

**Code**

```python
def findCount(n, m, matrix, element):
    cnt = 0
    
    # For each row, count elements <= element using binary search
    for i in range(n):
        arr = matrix[i]
        low, high = 0, m - 1
        largeIndex = m  # Index of first element > element
        
        # Binary search to find first element > element
        while low <= high:
            mid = (low + high) // 2
            if(matrix[i][mid] > element):
                largeIndex = mid
                high = mid - 1
            else:
                low = mid + 1
        
        # Count of elements <= element in this row
        currentCnt = largeIndex
        cnt += currentCnt
    
    return cnt

import math

def optimized(matrix):
    n, m = len(matrix), len(matrix[0])
    
    # Find the range of values in matrix
    low = float('inf')
    high = float('-inf')
    
    for i in range(n):
        if(matrix[i][0] < low):
            low = matrix[i][0]
        if(matrix[i][m - 1] > high):
            high = matrix[i][m - 1]
    
    # Target: number of elements that should be <= median
    targetCnt = math.ceil((n * m) / 2)
    ans = 0
    
    # Binary search on the value range
    while low <= high:
        mid = (low + high) // 2
        
        # Count elements <= mid
        cnt = findCount(n, m, matrix, mid)
        
        if(cnt >= targetCnt):
            ans = mid  # Potential median, search for smaller
            high = mid - 1
        else:
            low = mid + 1  # Need larger value
    
    return ans
```

**Time Complexity**

O(N * log M * log(range)) — The outer binary search on value range takes O(log(range)) iterations, where range = max_value - min_value (up to 10^9). For each iteration, we call `findCount` which iterates through N rows, and for each row performs binary search in O(log M) time. Therefore, total time is O(N * log M * log(10^9)) ≈ O(N * log M * 30).

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables (low, high, mid, cnt, ans, etc.). No additional data structures proportional to input size are created, making this solution highly space-efficient.

---
