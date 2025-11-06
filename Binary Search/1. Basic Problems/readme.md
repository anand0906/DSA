# Search X in Sorted Array

## Problem Description

Given a sorted array of integers `nums` with 0-based indexing, find the index of a specified target integer. If the target is found in the array, return its index. If the target is not found, return -1.

**Notes on constraints:** Since the array is sorted, we can leverage binary search to achieve logarithmic time complexity instead of linear time with a brute-force approach.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
```
**Explanation:** The target integer 9 exists in nums at index 4.

**Test Case 2:**
```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
```
**Explanation:** The target integer 2 does not exist in nums, so we return -1.

**Test Case 3:**
```
Input: nums = [-1,0,3,5,9,12], target = -1
Output: 0
```
**Explanation:** The target integer -1 exists in nums at index 0 (the first position).

## Approaches

### Approach 1: Linear Search

**Summary:** Iterate through the array sequentially to find the target.

**Intuition**

The simplest approach is to check each element in the array one by one until we find the target or reach the end of the array. This doesn't utilize the sorted property of the array.

**Approach Steps**

1. Iterate through each index from 0 to n-1
2. At each index, check if the element equals the target
3. If found, return the current index
4. If the loop completes without finding the target, return -1

**Example**

Consider `nums = [-1, 0, 3, 5, 9, 12]` and `target = 9`:
- Index 0: arr[0] = -1 ≠ 9, continue
- Index 1: arr[1] = 0 ≠ 9, continue
- Index 2: arr[2] = 3 ≠ 9, continue
- Index 3: arr[3] = 5 ≠ 9, continue
- Index 4: arr[4] = 9 = 9, return 4

**Code**

```python
def solve(n, arr, target):
    # Iterate through each index in the array
    for index in range(n):
        # Check if current element matches target
        if(arr[index] == target):
            return index
    # Target not found, return -1
    return -1
```

**Time Complexity**

O(n) — We potentially visit every element in the array once in the worst case.

**Space Complexity**

O(1) — We only use a constant amount of extra space for the index variable.

### Approach 2: Binary Search

**Summary:** Utilize the sorted property to efficiently search by repeatedly halving the search space.

**Intuition**

Since the array is sorted, we can use binary search to find the target much faster. We maintain a search range with `low` and `high` pointers, and check the middle element. Based on whether the middle element is greater than, less than, or equal to the target, we eliminate half of the remaining elements.

**Approach Steps**

1. Initialize `low = 0` and `high = n-1` to represent the search range
2. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - If `arr[mid] == target`, return `mid`
   - If `arr[mid] > target`, search the left half by setting `high = mid - 1`
   - If `arr[mid] < target`, search the right half by setting `low = mid + 1`
3. If the loop exits without finding the target, return -1

**Example**

Consider `nums = [-1, 0, 3, 5, 9, 12]` and `target = 9`:
- Iteration 1: low=0, high=5, mid=2, arr[2]=3 < 9, move right (low=3)
- Iteration 2: low=3, high=5, mid=4, arr[4]=9 = 9, return 4

**Code**

```python
def optimized(n, arr, target):
    # Initialize search boundaries
    low, high = 0, n-1
    while low <= high:
        # Find middle index
        mid = (low + high) // 2
        # Target found at mid
        if(arr[mid] == target):
            return mid
        # Target is in left half
        elif(arr[mid] > target):
            high = mid - 1
        # Target is in right half
        else:
            low = mid + 1
    # Target not found
    return -1
```

**Time Complexity**

O(log n) — We halve the search space in each iteration, resulting in logarithmic time.

**Space Complexity**

O(1) — We only use a constant amount of extra space for the low, high, and mid variables.

---

# Lower Bound

## Problem Description

Given a sorted array of `nums` and an integer `x`, write a program to find the lower bound of `x`. The lower bound algorithm finds the first and smallest index in a sorted array where the value at that index is greater than or equal to a given key (i.e., x). If no such index is found, return the size of the array.

**Notes on constraints:** The array is sorted, making binary search an optimal approach for finding the lower bound efficiently.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: nums = [1,2,2,3], x = 2
Output: 1
```
**Explanation:** Index 1 is the smallest index such that arr[1] >= 2 (arr[1] = 2).

**Test Case 2:**
```
Input: nums = [3,5,8,15,19], x = 9
Output: 3
```
**Explanation:** Index 3 is the smallest index such that arr[3] >= 9 (arr[3] = 15, which is the first element >= 9).

**Test Case 3:**
```
Input: nums = [1,2,3,4], x = 5
Output: 4
```
**Explanation:** No element in the array is >= 5, so we return the size of the array (4).

## Approaches

### Approach 1: Linear Search

**Summary:** Iterate through the array to find the first element >= x.

**Intuition**

The simplest approach is to traverse the array from left to right and return the index of the first element that is greater than or equal to the target. If no such element exists, return the array size.

**Approach Steps**

1. Iterate through each index from 0 to n-1
2. At each index, check if the element is >= target
3. If found, return the current index (this is the lower bound)
4. If the loop completes without finding such an element, return n

**Example**

Consider `nums = [1, 2, 2, 3]` and `x = 2`:
- Index 0: arr[0] = 1 < 2, continue
- Index 1: arr[1] = 2 >= 2, return 1

**Code**

```python
def solve(n, arr, target):
    # Iterate through each index
    for index in range(n):
        # Check if current element is >= target
        if(arr[index] >= target):
            return index
    # No element >= target, return array size
    return n
```

**Time Complexity**

O(n) — We potentially visit every element in the array once in the worst case.

**Space Complexity**

O(1) — We only use a constant amount of extra space.

### Approach 2: Binary Search

**Summary:** Use binary search to efficiently find the smallest index where arr[index] >= x.

**Intuition**

Since the array is sorted, we can use binary search to find the lower bound. We maintain a variable `index` to store the potential answer. Whenever we find an element >= target, we store that index and continue searching in the left half to find a potentially smaller index.

**Approach Steps**

1. Initialize `low = 0`, `high = n-1`, and `index = n` (default if no element >= target)
2. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - If `arr[mid] == target`, store `mid` in `index` and search left (`high = mid - 1`) for a potentially smaller index
   - If `arr[mid] > target`, store `mid` in `index` and search left (`high = mid - 1`)
   - If `arr[mid] < target`, search right (`low = mid + 1`)
3. Return `index`

**Example**

Consider `nums = [1, 2, 2, 3]` and `x = 2`:
- Iteration 1: low=0, high=3, mid=1, arr[1]=2 = 2, index=1, search left (high=0)
- Iteration 2: low=0, high=0, mid=0, arr[0]=1 < 2, search right (low=1)
- Loop exits, return index=1

**Code**

```python
def optimized(n, arr, target):
    # Initialize search boundaries and result index
    low, high = 0, n-1
    index = n  # Default: no element >= target
    while low <= high:
        # Find middle index
        mid = (low + high) // 2
        # If equal, this could be lower bound, search left for smaller index
        if(arr[mid] == target):
            index = mid
            high = mid - 1
        # If greater, this could be lower bound, search left
        elif(arr[mid] > target):
            index = mid
            high = mid - 1
        # If smaller, search right
        else:
            low = mid + 1
    return index
```

**Time Complexity**

O(log n) — Binary search halves the search space in each iteration.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

---

# Upper Bound

## Problem Description

Given a sorted array of `nums` and an integer `x`, write a program to find the upper bound of `x`. The upper bound of `x` is defined as the smallest index `i` such that `nums[i] > x`. If no such index is found, return the size of the array.

**Notes on constraints:** The array is sorted, allowing us to use binary search for an efficient O(log n) solution.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: n = 4, nums = [1,2,2,3], x = 2
Output: 3
```
**Explanation:** Index 3 is the smallest index such that arr[3] > 2 (arr[3] = 3).

**Test Case 2:**
```
Input: n = 5, nums = [3,5,8,15,19], x = 9
Output: 3
```
**Explanation:** Index 3 is the smallest index such that arr[3] > 9 (arr[3] = 15, which is the first element > 9).

**Test Case 3:**
```
Input: n = 4, nums = [1,2,3,4], x = 4
Output: 4
```
**Explanation:** No element in the array is > 4, so we return the size of the array (4).

## Approaches

### Approach 1: Linear Search

**Summary:** Iterate through the array to find the first element > x.

**Intuition**

The straightforward approach is to traverse the array from left to right and return the index of the first element that is strictly greater than the target. If no such element exists, return the array size.

**Approach Steps**

1. Iterate through each index from 0 to n-1
2. At each index, check if the element is > target
3. If found, return the current index (this is the upper bound)
4. If the loop completes without finding such an element, return n

**Example**

Consider `nums = [1, 2, 2, 3]` and `x = 2`:
- Index 0: arr[0] = 1 <= 2, continue
- Index 1: arr[1] = 2 <= 2, continue
- Index 2: arr[2] = 2 <= 2, continue
- Index 3: arr[3] = 3 > 2, return 3

**Code**

```python
def solve(n, arr, target):
    # Iterate through each index
    for index in range(n):
        # Check if current element is > target
        if(arr[index] > target):
            return index
    # No element > target, return array size
    return n
```

**Time Complexity**

O(n) — We potentially visit every element in the array once in the worst case.

**Space Complexity**

O(1) — We only use a constant amount of extra space.

### Approach 2: Binary Search

**Summary:** Use binary search to efficiently find the smallest index where arr[index] > x.

**Intuition**

Since the array is sorted, we can use binary search to find the upper bound. We maintain a variable `index` to store the potential answer. Whenever we find an element > target, we store that index and continue searching in the left half to find a potentially smaller index. When we encounter an element equal to the target, we search the right half.

**Approach Steps**

1. Initialize `low = 0`, `high = n-1`, and `index = n` (default if no element > target)
2. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - If `arr[mid] == target`, search right (`low = mid + 1`) since we need strictly greater
   - If `arr[mid] > target`, store `mid` in `index` and search left (`high = mid - 1`) for a smaller index
   - If `arr[mid] < target`, search right (`low = mid + 1`)
3. Return `index`

**Example**

Consider `nums = [1, 2, 2, 3]` and `x = 2`:
- Iteration 1: low=0, high=3, mid=1, arr[1]=2 = 2, search right (low=2)
- Iteration 2: low=2, high=3, mid=2, arr[2]=2 = 2, search right (low=3)
- Iteration 3: low=3, high=3, mid=3, arr[3]=3 > 2, index=3, search left (high=2)
- Loop exits, return index=3

**Code**

```python
def optimized(n, arr, target):
    # Initialize search boundaries and result index
    low, high = 0, n-1
    index = n  # Default: no element > target
    while low <= high:
        # Find middle index
        mid = (low + high) // 2
        # If equal, search right for strictly greater element
        if(arr[mid] == target):
            low = mid + 1
        # If greater, this could be upper bound, search left for smaller index
        elif(arr[mid] > target):
            index = mid
            high = mid - 1
        # If smaller, search right
        else:
            low = mid + 1
    return index
```

**Time Complexity**

O(log n) — Binary search halves the search space in each iteration.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.
