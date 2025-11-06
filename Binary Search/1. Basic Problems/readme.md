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

---

# Search Insert Position

## Problem Description

Given a sorted array of `nums` consisting of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

**Notes on constraints:** The array contains distinct integers and is sorted, making binary search the optimal approach for O(log n) time complexity.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: nums = [1, 3, 5, 6], target = 5
Output: 2
```
**Explanation:** The target value 5 is found at index 2 in the sorted array.

**Test Case 2:**
```
Input: nums = [1, 3, 5, 6], target = 2
Output: 1
```
**Explanation:** The target value 2 is not found in the array. However, it should be inserted at index 1 to maintain the sorted order (between 1 and 3).

**Test Case 3:**
```
Input: nums = [1, 3, 5, 6], target = 7
Output: 4
```
**Explanation:** The target value 7 is larger than all elements, so it should be inserted at the end (index 4).

## Approaches

### Approach 1: Linear Search

**Summary:** Iterate through the array to find the first position where the target can be inserted.

**Intuition**

We traverse the array from left to right and find the first element that is greater than or equal to the target. This position is where the target either exists or should be inserted to maintain sorted order.

**Approach Steps**

1. Iterate through each index from 0 to n-1
2. At each index, check if the element is >= target
3. If found, return the current index (either the target exists here or should be inserted here)
4. If the loop completes without finding such an element, return n (insert at the end)

**Example**

Consider `nums = [1, 3, 5, 6]` and `target = 2`:
- Index 0: arr[0] = 1 < 2, continue
- Index 1: arr[1] = 3 >= 2, return 1 (target should be inserted at index 1)

**Code**

```python
def solve(n, arr, target):
    # Iterate through each index
    for index in range(n):
        # Check if current element is >= target
        if(arr[index] >= target):
            return index
    # Target is larger than all elements, insert at end
    return n
```

**Time Complexity**

O(n) — We potentially visit every element in the array once in the worst case.

**Space Complexity**

O(1) — We only use a constant amount of extra space.

### Approach 2: Binary Search

**Summary:** Use binary search to efficiently find the insertion position by leveraging the sorted property.

**Intuition**

This problem is essentially finding the lower bound of the target (the smallest index where arr[index] >= target). We use binary search to efficiently narrow down the search space. We maintain an `index` variable that stores the potential insertion position, updating it whenever we find an element >= target.

**Approach Steps**

1. Initialize `low = 0`, `high = n-1`, and `index = n` (default insertion position at end)
2. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - If `arr[mid] == target`, store `mid` in `index` and search left (`high = mid - 1`) to ensure we find the exact position
   - If `arr[mid] > target`, store `mid` in `index` and search left (`high = mid - 1`)
   - If `arr[mid] < target`, search right (`low = mid + 1`)
3. Return `index`

**Example**

Consider `nums = [1, 3, 5, 6]` and `target = 2`:
- Iteration 1: low=0, high=3, mid=1, arr[1]=3 > 2, index=1, search left (high=0)
- Iteration 2: low=0, high=0, mid=0, arr[0]=1 < 2, search right (low=1)
- Loop exits, return index=1

**Code**

```python
def optimized(n, arr, target):
    # Initialize search boundaries and result index
    low, high = 0, n-1
    index = n  # Default: insert at end
    while low <= high:
        # Find middle index
        mid = (low + high) // 2
        # If equal, this is the position, but search left to confirm
        if(arr[mid] == target):
            index = mid
            high = mid - 1
        # If greater, this could be insertion point, search left
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

# Floor and Ceil in Sorted Array

## Problem Description

Given a sorted array `nums` and an integer `x`, find the floor and ceil of `x` in `nums`. The floor of `x` is the largest element in the array which is smaller than or equal to `x`. The ceiling of `x` is the smallest element in the array greater than or equal to `x`. If no floor or ceil exists, output -1.

**Notes on constraints:** The array is sorted, enabling efficient binary search to find both floor and ceil independently in O(log n) time.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: nums = [3, 4, 4, 7, 8, 10], x = 5
Output: [4, 7]
```
**Explanation:** The floor of 5 is 4 (largest element <= 5), and the ceiling of 5 is 7 (smallest element >= 5).

**Test Case 2:**
```
Input: nums = [3, 4, 4, 7, 8, 10], x = 8
Output: [8, 8]
```
**Explanation:** The floor of 8 is 8 (8 exists in array and is <= 8), and the ceiling of 8 is also 8 (8 exists and is >= 8).

**Test Case 3:**
```
Input: nums = [3, 4, 4, 7, 8, 10], x = 2
Output: [-1, 3]
```
**Explanation:** No element is <= 2, so floor is -1. The ceiling is 3 (smallest element >= 2).

## Approaches

### Approach 1: Linear Search

**Summary:** Iterate through the array to find floor and ceil using two separate passes.

**Intuition**

For the floor, we traverse the array and keep updating our answer with elements <= target until we encounter an element > target. For the ceil, we find the first element >= target. Both operations can be done with linear scans.

**Approach Steps**

1. Initialize `floor = -1` and `ceil = -1`
2. For finding floor: Iterate through the array and update `floor` with each element <= target until we find an element > target (then break)
3. For finding ceil: Iterate through the array and set `ceil` to the first element >= target (then break)
4. Return `[floor, ceil]`

**Example**

Consider `nums = [3, 4, 4, 7, 8, 10]` and `x = 5`:

**Finding Floor:**
- Index 0: arr[0] = 3 <= 5, floor = 3
- Index 1: arr[1] = 4 <= 5, floor = 4
- Index 2: arr[2] = 4 <= 5, floor = 4
- Index 3: arr[3] = 7 > 5, break
- Floor = 4

**Finding Ceil:**
- Index 0: arr[0] = 3 < 5, continue
- Index 1: arr[1] = 4 < 5, continue
- Index 2: arr[2] = 4 < 5, continue
- Index 3: arr[3] = 7 >= 5, ceil = 7, break
- Ceil = 7

Result: [4, 7]

**Code**

```python
def solve(n, arr, target):
    floor = -1
    ceil = -1
    # Find floor: largest element <= target
    for index in range(n):
        if(arr[index] <= target):
            floor = arr[index]
        else:
            break
    # Find ceil: smallest element >= target
    for index in range(n):
        if(arr[index] >= target):
            ceil = arr[index]
            break
    return [floor, ceil]
```

**Time Complexity**

O(n) — We make two linear passes through the array in the worst case.

**Space Complexity**

O(1) — We only use a constant amount of extra space.

### Approach 2: Binary Search

**Summary:** Use two independent binary searches to find floor and ceil efficiently.

**Intuition**

We can use binary search twice: once to find the floor (largest element <= target) and once to find the ceil (smallest element >= target). For floor, we search for the rightmost position where elements are <= target. For ceil, we search for the leftmost position where elements are >= target.

**Approach Steps**

1. **Finding Floor:**
   - Initialize `low = 0`, `high = n-1`, and `floor = -1`
   - While `low <= high`, calculate `mid`
   - If `arr[mid] == target`, set `floor = arr[mid]` and break (or continue searching right)
   - If `arr[mid] > target`, search left (`high = mid - 1`)
   - If `arr[mid] < target`, set `floor = arr[mid]` and search right (`low = mid + 1`)

2. **Finding Ceil:**
   - Initialize `low = 0`, `high = n-1`, and `ceil = -1`
   - While `low <= high`, calculate `mid`
   - If `arr[mid] == target`, set `ceil = arr[mid]` and break
   - If `arr[mid] > target`, set `ceil = arr[mid]` and search left (`high = mid - 1`)
   - If `arr[mid] < target`, search right (`low = mid + 1`)

3. Return `[floor, ceil]`

**Example**

Consider `nums = [3, 4, 4, 7, 8, 10]` and `x = 5`:

**Finding Floor (largest <= 5):**
- Iteration 1: low=0, high=5, mid=2, arr[2]=4 < 5, floor=4, search right (low=3)
- Iteration 2: low=3, high=5, mid=4, arr[4]=8 > 5, search left (high=3)
- Iteration 3: low=3, high=3, mid=3, arr[3]=7 > 5, search left (high=2)
- Loop exits, floor=4

**Finding Ceil (smallest >= 5):**
- Iteration 1: low=0, high=5, mid=2, arr[2]=4 < 5, search right (low=3)
- Iteration 2: low=3, high=5, mid=4, arr[4]=8 > 5, ceil=8, search left (high=3)
- Iteration 3: low=3, high=3, mid=3, arr[3]=7 > 5, ceil=7, search left (high=2)
- Loop exits, ceil=7

Result: [4, 7]

**Code**

```python
def optimized(n, arr, target):
    # Find floor: largest element <= target
    low, high = 0, n-1
    floor = -1
    while low <= high:
        mid = (low + high) // 2
        # If equal, this is the floor
        if(arr[mid] == target):
            floor = arr[mid]
            low = mid + 1
            break
        # If greater, search left
        elif(arr[mid] > target):
            high = mid - 1
        # If smaller, update floor and search right
        else:
            floor = arr[mid]
            low = mid + 1

    # Find ceil: smallest element >= target
    low, high = 0, n-1
    ceil = -1
    while low <= high:
        mid = (low + high) // 2
        # If equal, this is the ceil
        if(arr[mid] == target):
            ceil = arr[mid]
            low = mid + 1
            break
        # If greater, update ceil and search left
        elif(arr[mid] > target):
            ceil = arr[mid]
            high = mid - 1
        # If smaller, search right
        else:
            low = mid + 1
        
    return [floor, ceil]
```

**Time Complexity**

O(log n) — We perform two independent binary searches, each taking O(log n) time.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

---

# First and Last Occurrence

## Problem Description

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given target value. If the target is not found in the array, return [-1, -1].

**Notes on constraints:** The array is sorted in non-decreasing order (duplicates allowed), making binary search optimal for finding both first and last occurrences efficiently.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: nums = [5, 7, 7, 8, 8, 10], target = 8
Output: [3, 4]
```
**Explanation:** The target is 8, and it appears at indices 3 and 4. The first occurrence is at index 3 and the last occurrence is at index 4.

**Test Case 2:**
```
Input: nums = [5, 7, 7, 8, 8, 10], target = 6
Output: [-1, -1]
```
**Explanation:** The target is 6, which is not present in the array. Therefore, we return [-1, -1].

**Test Case 3:**
```
Input: nums = [5, 7, 7, 8, 8, 10], target = 7
Output: [1, 2]
```
**Explanation:** The target is 7, appearing at indices 1 and 2.

## Approaches

### Approach 1: Linear Search (Two Passes)

**Summary:** Make two separate passes to find the first and last occurrences.

**Intuition**

We traverse the array twice. In the first pass, we find the first occurrence by breaking as soon as we find the target. In the second pass, we continue through the entire array, updating the last occurrence every time we find the target.

**Approach Steps**

1. Initialize `first = -1` and `last = -1`
2. **First pass:** Iterate through the array and set `first` to the current index when we first encounter the target, then break
3. **Second pass:** Iterate through the array and update `last` to the current index every time we encounter the target
4. Return `[first, last]`

**Example**

Consider `nums = [5, 7, 7, 8, 8, 10]` and `target = 8`:

**First Pass (finding first):**
- Indices 0-2: Not equal to 8
- Index 3: arr[3] = 8, first = 3, break

**Second Pass (finding last):**
- Indices 0-2: Not equal to 8
- Index 3: arr[3] = 8, last = 3
- Index 4: arr[4] = 8, last = 4
- Indices 5: Not equal to 8

Result: [3, 4]

**Code**

```python
def solve(n, arr, target):
    first = -1
    # Find first occurrence
    for index in range(n):
        if(arr[index] == target):
            first = index
            break
    last = -1
    # Find last occurrence
    for index in range(n):
        if(arr[index] == target):
            last = index
            
    return [first, last]
```

**Time Complexity**

O(n) — We make two passes through the array, each taking O(n) time.

**Space Complexity**

O(1) — We only use a constant amount of extra space.

### Approach 2: Linear Search (Single Pass)

**Summary:** Find both first and last occurrences in a single pass through the array.

**Intuition**

Instead of two separate passes, we can find both occurrences in one pass. We set `first` only when we encounter the target for the first time (when `first == -1`), and we update `last` every time we encounter the target.

**Approach Steps**

1. Initialize `first = -1` and `last = -1`
2. Iterate through the array once
3. If current element equals target and `first == -1`, set `first = index`
4. If current element equals target, update `last = index`
5. Return `[first, last]`

**Example**

Consider `nums = [5, 7, 7, 8, 8, 10]` and `target = 8`:
- Indices 0-2: Not equal to 8
- Index 3: arr[3] = 8 and first = -1, so first = 3, last = 3
- Index 4: arr[4] = 8, last = 4
- Index 5: Not equal to 8

Result: [3, 4]

**Code**

```python
def solve_2(n, arr, target):
    first = -1
    last = -1
    # Find both in single pass
    for index in range(n):
        # Set first only once
        if(arr[index] == target and first == -1):
            first = index
        # Update last every time we see target
        if(arr[index] == target):
            last = index
            
    return [first, last]
```

**Time Complexity**

O(n) — We make a single pass through the array.

**Space Complexity**

O(1) — We only use a constant amount of extra space.

### Approach 3: Binary Search

**Summary:** Use two binary searches to find first and last occurrences independently.

**Intuition**

We can use binary search twice: once to find the first (leftmost) occurrence and once to find the last (rightmost) occurrence. For the first occurrence, when we find the target, we continue searching left to find an earlier occurrence. For the last occurrence, when we find the target, we continue searching right to find a later occurrence.

**Approach Steps**

1. **Finding First Occurrence:**
   - Initialize `low = 0`, `high = n-1`, and `first = -1`
   - While `low <= high`, calculate `mid`
   - If `arr[mid] == target`, set `first = mid` and search left (`high = mid - 1`)
   - If `arr[mid] > target`, search left (`high = mid - 1`)
   - If `arr[mid] < target`, search right (`low = mid + 1`)
   - If `first == -1`, return `[-1, -1]` (target not found)

2. **Finding Last Occurrence:**
   - Initialize `low = 0`, `high = n-1`, and `last = -1`
   - While `low <= high`, calculate `mid`
   - If `arr[mid] == target`, set `last = mid` and search right (`low = mid + 1`)
   - If `arr[mid] > target`, search left (`high = mid - 1`)
   - If `arr[mid] < target`, search right (`low = mid + 1`)

3. Return `[first, last]`

**Example**

Consider `nums = [5, 7, 7, 8, 8, 10]` and `target = 8`:

**Finding First:**
- Iteration 1: low=0, high=5, mid=2, arr[2]=7 < 8, search right (low=3)
- Iteration 2: low=3, high=5, mid=4, arr[4]=8 = 8, first=4, search left (high=3)
- Iteration 3: low=3, high=3, mid=3, arr[3]=8 = 8, first=3, search left (high=2)
- Loop exits, first=3

**Finding Last:**
- Iteration 1: low=0, high=5, mid=2, arr[2]=7 < 8, search right (low=3)
- Iteration 2: low=3, high=5, mid=4, arr[4]=8 = 8, last=4, search right (low=5)
- Iteration 3: low=5, high=5, mid=5, arr[5]=10 > 8, search left (high=4)
- Loop exits, last=4

Result: [3, 4]

**Code**

```python
def optimized(n, arr, target):
    # Find first occurrence
    low, high = 0, n-1
    first = -1
    while low <= high:
        mid = (low + high) // 2
        # If equal, update first and search left for earlier occurrence
        if(arr[mid] == target):
            first = mid
            high = mid - 1
        # If greater, search left
        elif(arr[mid] > target):
            high = mid - 1
        # If smaller, search right
        else:
            low = mid + 1
    # If target not found, return [-1, -1]
    if(first == -1):
        return [-1, -1]

    # Find last occurrence
    low, high = 0, n-1
    last = -1
    while low <= high:
        mid = (low + high) // 2
        # If equal, update last and search right for later occurrence
        if(arr[mid] == target):
            last = mid
            low = mid + 1
        # If greater, search left
        elif(arr[mid] > target):
            high = mid - 1
        # If smaller, search right
        else:
            low = mid + 1
        
    return [first, last]
```

**Time Complexity**

O(log n) — We perform two independent binary searches, each taking O(log n) time.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

---
