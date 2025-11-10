# Find Peak Element

## Problem Description

Given an array `arr` of integers, a **peak element** is defined as an element that is greater than both of its neighbors.

Formally, if `arr[i]` is a peak element, then `arr[i - 1] < arr[i]` and `arr[i + 1] < arr[i]`.

Your task is to find the **index (0-based)** of a peak element in the array. If there are multiple peak numbers, return the index of **any** peak number.

If the returned index corresponds to a valid peak, output `true`; otherwise, output `false`.

**Notes on constraints:**

* The array can have multiple peaks, and the goal is to find any one.
* Efficient solutions should aim for **O(log n)** time complexity for large arrays.

## Sample Test Cases With Explanation

### Test Case 1

**Input:** `arr = [1, 2, 3, 4, 5, 6, 7, 8, 5, 1]`

**Output:** `7`

**Explanation:** The element at index `7` (value `8`) is greater than its neighbors (`7` and `5`). Hence, it is a peak.

---

### Test Case 2

**Input:** `arr = [1, 2, 1, 3, 5, 6, 4]`

**Output:** `1`

**Explanation:** There are two peaks — elements at indices `1` (value `2`) and `5` (value `6`). Returning either index is valid.

---

## Approaches

### Approach 1: Linear Scan

**Summary:** Brute-force approach that checks each element and compares it with its neighbors.

**Intuition:**
We can find a peak by simply scanning through the array. For each element, if it is greater than its neighbors, it qualifies as a peak. Edge cases (first and last elements) must be handled separately.

**Approach Steps:**

1. If the array has only one element, return index `0`.
2. If the first element is greater than the second, return index `0`.
3. If the last element is greater than the second last, return the last index.
4. For each element from index `1` to `n-2`, check if it is greater than both its neighbors.
5. Return the index of the first such element found.
6. If no peak is found, return `-1` (though there should always be one).

**Example:**
For `arr = [1, 3, 2, 4, 1]`:

1. `1` < `3` → not a peak.
2. `3` > `1` and `3` > `2` → peak at index `1`.
3. The function returns `1`.

**Code:**

```python
def solve(n, arr):
    # If only one element, it is the peak
    if n == 1:
        return 0

    # Check if the first element is a peak
    if arr[0] > arr[1]:
        return 0

    # Check if the last element is a peak
    if arr[n - 1] > arr[n - 2]:
        return n - 1

    # Check the rest of the elements
    for i in range(1, n - 1):
        # Element is greater than both neighbors
        if arr[i] > arr[i + 1] and arr[i] > arr[i - 1]:
            return i

    # If no peak found
    return -1
```

**Time Complexity:** O(n) — Each element is checked once.

**Space Complexity:** O(1) — Uses constant extra space.

---

### Approach 2: Binary Search (Optimized)

**Summary:** Divide-and-conquer approach using binary search for faster performance.

**Intuition:**
If an element is smaller than its right neighbor, there must be a peak on the right side. If it’s smaller than its left neighbor, there must be a peak on the left. Using this logic, we can apply binary search to narrow down the region containing a peak.

**Approach Steps:**

1. Handle base cases for arrays of size 1, or if the first/last element is a peak.
2. Initialize `low = 1` and `high = n - 2`.
3. While `low <= high`, find the middle index `mid`.
4. If `arr[mid]` is greater than both neighbors, return `mid`.
5. If `arr[mid]` is greater than its left neighbor, move right (`low = mid + 1`).
6. Otherwise, move left (`high = mid - 1`).
7. If no peak is found, return `-1`.

**Example:**
For `arr = [1, 2, 3, 1]`:

1. `low = 1`, `high = 2`, `mid = 1` → `arr[mid] = 2` < `arr[mid+1] = 3` → move right.
2. `low = 2`, `mid = 2` → `arr[mid] = 3` > `arr[mid-1] = 2` and `arr[mid] > arr[mid+1] = 1` → peak at index `2`.

**Code:**

```python
def optimized(n, arr):
    # If only one element, it is the peak
    if n == 1:
        return 0

    # Check if the first element is a peak
    if arr[0] > arr[1]:
        return 0

    # Check if the last element is a peak
    if arr[n - 1] > arr[n - 2]:
        return n - 1

    # Binary search between 1 and n-2
    low, high = 1, n - 2

    while low <= high:
        mid = (low + high) // 2

        # Check if mid is a peak
        if arr[mid] > arr[mid - 1] and arr[mid] > arr[mid + 1]:
            return mid

        # If mid is part of ascending sequence, move right
        elif arr[mid] > arr[mid - 1]:
            low = mid + 1

        # Otherwise, move left
        else:
            high = mid - 1

    # If no peak found
    return -1
```

**Time Complexity:** O(log n) — Binary search divides the array each iteration.

**Space Complexity:** O(1) — Only a few variables are used.

---

---

# Median of 2 Sorted Arrays

## Problem Description

Given two sorted arrays `arr1` and `arr2` of sizes `m` and `n` respectively, return the **median** of the two sorted arrays.

The **median** is defined as the middle value of a sorted list of numbers. If the total number of elements is even, the median is the **average of the two middle elements**.

**Notes on constraints:**

* Both arrays are sorted.
* The total size `m + n` can be large, so efficient solutions should aim for **O(log(min(m, n)))**.

## Sample Test Cases With Explanation

### Test Case 1

**Input:** `arr1 = [2, 4, 6]`, `arr2 = [1, 3, 5]`

**Output:** `3.5`

**Explanation:** After merging, we get `[1, 2, 3, 4, 5, 6]`. Since the length is even, the median is the average of the two middle elements: `(3 + 4) / 2 = 3.5`.

---

### Test Case 2

**Input:** `arr1 = [2, 4, 6]`, `arr2 = [1, 3]`

**Output:** `3.0`

**Explanation:** After merging, we get `[1, 2, 3, 4, 6]`. The middle element is `3`, so the median is `3.0`.

---

## Approaches

### Approach 1: Merge and Find Median

**Summary:** Brute-force merging both arrays and finding the median.

**Intuition:**
Since both arrays are sorted, we can merge them into one sorted array and directly compute the median.

**Approach Steps:**

1. Initialize two pointers `left` and `right` for both arrays.
2. Compare elements from both arrays and append the smaller one to a new merged list.
3. Continue merging until all elements are processed.
4. Compute the median based on the merged list length.

**Example:**
For `arr1 = [1, 3]` and `arr2 = [2, 4]`:

1. Merged array → `[1, 2, 3, 4]`.
2. Middle elements → `2` and `3`.
3. Median = `(2 + 3) / 2 = 2.5`.

**Code:**

```python
def solve(arr1, arr2):
    n1 = len(arr1)
    n2 = len(arr2)
    n = n1 + n2
    left, right = 0, 0
    arr = []

    # Merge both sorted arrays
    while left < n1 and right < n2:
        if arr1[left] < arr2[right]:
            arr.append(arr1[left])
            left += 1
        else:
            arr.append(arr2[right])
            right += 1

    # Append remaining elements from arr1
    while left < n1:
        arr.append(arr1[left])
        left += 1

    # Append remaining elements from arr2
    while right < n2:
        arr.append(arr2[right])
        right += 1

    # Find median
    mid = n // 2
    if n % 2 == 1:
        return arr[mid]
    else:
        return (arr[mid] + arr[mid - 1]) / 2
```

**Time Complexity:** O(m + n) — Full merge of both arrays.

**Space Complexity:** O(m + n) — Extra space for the merged array.

---

### Approach 2: Partial Merge Until Median

**Summary:** Merges only until the median point without storing all elements.

**Intuition:**
We don’t need the entire merged array — just the elements around the median position. We can keep track of the last two merged elements to calculate the median.

**Approach Steps:**

1. Initialize pointers for both arrays.
2. Keep merging until the combined index reaches the median positions.
3. Track the two middle elements (`mid1`, `mid2`).
4. Calculate and return the median.

**Example:**
For `arr1 = [2, 4, 6]`, `arr2 = [1, 3, 5]`:

1. As we merge, we reach indices `2` and `3` with values `3` and `4`.
2. Median = `(3 + 4) / 2 = 3.5`.

**Code:**

```python
def solve2(arr1, arr2):
    n1 = len(arr1)
    n2 = len(arr2)
    n = n1 + n2
    mid1 = (n1 + n2) // 2
    mid2 = mid1 - 1
    midEle1, midEle2 = -1, -1
    index = -1
    left, right = 0, 0

    # Merge until reaching median indices
    while left < n1 and right < n2:
        if arr1[left] < arr2[right]:
            index += 1
            if index == mid1:
                midEle1 = arr1[left]
            if index == mid2:
                midEle2 = arr1[left]
            left += 1
        else:
            index += 1
            if index == mid1:
                midEle1 = arr2[right]
            if index == mid2:
                midEle2 = arr2[right]
            right += 1

    # Process remaining elements in arr1
    while left < n1:
        index += 1
        if index == mid1:
            midEle1 = arr1[left]
        if index == mid2:
            midEle2 = arr1[left]
        left += 1

    # Process remaining elements in arr2
    while right < n2:
        index += 1
        if index == mid1:
            midEle1 = arr2[right]
        if index == mid2:
            midEle2 = arr2[right]
        right += 1

    # Return median based on parity
    if n % 2 == 1:
        return midEle1
    else:
        return (midEle1 + midEle2) / 2
```

**Time Complexity:** O(m + n) — Only iterates until median indices.

**Space Complexity:** O(1) — No extra array created.

---

### Approach 3: Binary Search Partition (Optimized)

**Summary:** Uses binary search to find the correct partition without merging.

**Intuition:**
We divide both arrays such that all elements on the left are smaller than all on the right. Once partitioned correctly, we can calculate the median directly.

**Approach Steps:**

1. Ensure `arr1` is the smaller array.
2. Perform binary search on `arr1` to find the partition point.
3. For each partition, calculate boundaries: `left1`, `left2`, `right1`, `right2`.
4. If valid (`left1 <= right2` and `left2 <= right1`), compute the median.
5. Otherwise, adjust search range accordingly.

**Example:**
For `arr1 = [1, 3]`, `arr2 = [2, 4, 5]`:

1. After partition, left halves = `[1, 2]`, right halves = `[3, 4, 5]`.
2. Median = `3`.

**Code:**

```python
import math

def optimized(arr1, arr2):
    n1, n2 = len(arr1), len(arr2)

    # Ensure arr1 is the smaller array
    if n1 > n2:
        return optimized(arr2, arr1)

    n = n1 + n2
    length = math.ceil(n / 2)
    low, high = 0, n1

    while low <= high:
        mid = (low + high) // 2
        mid1 = mid
        mid2 = length - mid1

        # Determine partition boundaries
        left1 = arr1[mid1 - 1] if mid1 != 0 else float('-inf')
        left2 = arr2[mid2 - 1] if mid2 != 0 else float('-inf')
        right1 = arr1[mid1] if mid1 != n1 else float('inf')
        right2 = arr2[mid2] if mid2 != n2 else float('inf')

        # Check for correct partition
        if left1 <= right2 and left2 <= right1:
            if n % 2 == 1:
                return max(left1, left2)
            else:
                midEle1 = max(left1, left2)
                midEle2 = min(right1, right2)
                return (midEle1 + midEle2) / 2

        # Adjust binary search boundaries
        elif left1 > right2:
            high = mid - 1
        else:
            low = mid + 1

    return -1
```

**Time Complexity:** O(log(min(m, n))) — Binary search on the smaller array.

**Space Complexity:** O(1) — Constant extra space used.

---

# Kth Element of 2 Sorted Arrays

## Problem Description

Given two sorted arrays `a` and `b` of sizes `m` and `n` respectively, find the **kth element** in the merged sorted array.

You do not need to merge the arrays completely — the goal is to efficiently determine the element that would appear in position `k` if both arrays were combined and sorted.

**Notes on constraints:**

* Both arrays are sorted in ascending order.
* `1 ≤ k ≤ m + n`.
* Efficient solutions should target **O(log(min(m, n)))**.

## Sample Test Cases With Explanation

### Test Case 1

**Input:** `a = [2, 3, 6, 7, 9]`, `b = [1, 4, 8, 10]`, `k = 5`

**Output:** `6`

**Explanation:** The merged array is `[1, 2, 3, 4, 6, 7, 8, 9, 10]`. The 5th element is `6`.

---

### Test Case 2

**Input:** `a = [100, 112, 256, 349, 770]`, `b = [72, 86, 113, 119, 265, 445, 892]`, `k = 7`

**Output:** `256`

**Explanation:** After merging, we get `[72, 86, 100, 112, 113, 119, 256, 265, 349, 445, 770, 892]`. The 7th element is `256`.

---

## Approaches

### Approach 1: Binary Search Partition (Optimized)

**Summary:** Uses binary search to efficiently locate the kth element without merging the arrays.

**Intuition:**
Similar to finding the median of two sorted arrays, we can partition both arrays such that the total number of elements on the left equals `k`. The largest value among the left partitions gives the kth element.

**Approach Steps:**

1. Ensure that `arr1` is the smaller array for simplicity.
2. Perform binary search on `arr1` to find how many elements should be taken from it (`mid1`).
3. Determine how many elements must come from `arr2` (`mid2 = k - mid1`).
4. Compare boundary elements of both partitions (`left1`, `left2`, `right1`, `right2`).
5. If partition is valid (`left1 <= right2` and `left2 <= right1`), the kth element is `max(left1, left2)`.
6. Adjust search space if not valid.

**Example:**
For `a = [2, 3, 6, 7, 9]`, `b = [1, 4, 8, 10]`, and `k = 5`:

1. Possible partitions → take 3 elements from `a` and 2 from `b`.
2. Left partitions: `[2, 3, 6]` and `[1, 4]` → largest on left = `6`.
3. Right partitions: `[7, 9]` and `[8, 10]` → smallest on right = `7`.
4. Since `6 ≤ 7`, the partition is valid, and `6` is the kth element.

**Code:**

```python
import math

def optimized(arr1, arr2, k):
    n1, n2 = len(arr1), len(arr2)

    # Ensure arr1 is smaller for simpler partitioning
    if n1 > n2:
        return optimized(arr2, arr1, k)

    n = n1 + n2
    length = k

    # Binary search boundaries
    low, high = max(0, k - n2), min(n1, k)

    while low <= high:
        mid = (low + high) >> 1
        mid1 = mid
        mid2 = length - mid1

        # Partition boundaries
        left1 = arr1[mid1 - 1] if mid1 > 0 else float('-inf')
        left2 = arr2[mid2 - 1] if mid2 > 0 else float('-inf')
        right1 = arr1[mid1] if mid1 < n1 else float('inf')
        right2 = arr2[mid2] if mid2 < n2 else float('inf')

        # Valid partition found
        if left1 <= right2 and left2 <= right1:
            return max(left1, left2)

        # Too many elements taken from arr1, move left
        elif left1 > right2:
            high = mid - 1

        # Too few elements taken from arr1, move right
        else:
            low = mid + 1

    # If no valid partition found (should not happen for valid k)
    return -1
```

**Time Complexity:** O(log(min(m, n))) — Binary search on the smaller array.

**Space Complexity:** O(1) — Constant auxiliary space.

---
