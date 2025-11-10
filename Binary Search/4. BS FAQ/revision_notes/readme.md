# Binary Search - Advanced Problems

## Table of Contents
1. [Find Peak Element](#1-find-peak-element)
2. [Median of 2 Sorted Arrays](#2-median-of-2-sorted-arrays)
3. [Kth Element of 2 Sorted Arrays](#3-kth-element-of-2-sorted-arrays)
4. [Comparison Summary](#comparison-summary)

---

## 1. Find Peak Element

### Problem Description
Given an array `arr` of integers, a peak element is defined as an element greater than both of its neighbors. Find the index of any peak element in the array.

Formally, if `arr[i]` is the peak element: `arr[i-1] < arr[i]` and `arr[i+1] < arr[i]`.

### Sample Test Cases

```
Input: arr = [1, 2, 3, 4, 5, 6, 7, 8, 5, 1]
Output: 7

Input: arr = [1, 2, 1, 3, 5, 6, 4]
Output: 1 (or 5, both are valid)
```

---

### Approach 1: Linear Search

**Intuition:**
Check each element to see if it's greater than both neighbors. Handle boundary cases separately.

**Code:**
```python
def solve(n, arr):
    # Edge case: single element
    if n == 1:
        return 0
    
    # Check first element
    if arr[0] > arr[1]:
        return 0
    
    # Check last element
    if arr[n-1] > arr[n-2]:
        return n-1
    
    # Check middle elements
    for i in range(1, n-1):
        if arr[i] > arr[i+1] and arr[i] > arr[i-1]:
            return i
    
    return -1
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search

**Intuition:**
If the element at mid is on an increasing slope (arr[mid] > arr[mid-1]), a peak must exist on the right. If on a decreasing slope, a peak must exist on the left. Move toward the increasing side.

**Code:**
```python
def optimized(n, arr):
    # Edge case: single element
    if n == 1:
        return 0
    
    # Check first element
    if arr[0] > arr[1]:
        return 0
    
    # Check last element
    if arr[n-1] > arr[n-2]:
        return n-1
    
    # Search in range [1, n-2] to avoid boundary checks
    low, high = 1, n-2
    
    while low <= high:
        mid = (low + high) // 2
        
        # Check if mid is a peak
        if arr[mid] > arr[mid-1] and arr[mid] > arr[mid+1]:
            return mid
        # If on increasing slope, peak is on right
        elif arr[mid] > arr[mid-1]:
            low = mid + 1
        # If on decreasing slope, peak is on left
        else:
            high = mid - 1
    
    return -1
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 2. Median of 2 Sorted Arrays

### Problem Description
Given two sorted arrays `arr1` and `arr2` of size m and n respectively, return the median of the two sorted arrays. The median is the middle value of a sorted list. For even-length lists, it's the average of the two middle elements.

### Sample Test Cases

```
Input: arr1 = [2, 4, 6], arr2 = [1, 3, 5]
Output: 3.5
Explanation: Merged array [1, 2, 3, 4, 5, 6], median = (3 + 4) / 2 = 3.5

Input: arr1 = [2, 4, 6], arr2 = [1, 3]
Output: 3.0
Explanation: Merged array [1, 2, 3, 4, 6], median = 3
```

---

### Approach 1: Merge Arrays

**Intuition:**
Merge both arrays into one sorted array, then find the median based on whether the total length is odd or even.

**Code:**
```python
def solve(arr1, arr2):
    n1 = len(arr1)
    n2 = len(arr2)
    n = n1 + n2
    left, right = 0, 0
    arr = []
    
    # Merge two sorted arrays
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
        return (arr[mid] + arr[mid-1]) / 2
```

**Time Complexity:** O(n1 + n2)  
**Space Complexity:** O(n1 + n2)

---

### Approach 2: Find Median During Merge

**Intuition:**
Instead of creating the full merged array, only track the middle elements during the merge process.

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
    
    # Merge and track median positions
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
    
    # Process remaining elements from arr1
    while left < n1:
        index += 1
        if index == mid1:
            midEle1 = arr1[left]
        if index == mid2:
            midEle2 = arr1[left]
        left += 1
    
    # Process remaining elements from arr2
    while right < n2:
        index += 1
        if index == mid1:
            midEle1 = arr2[right]
        if index == mid2:
            midEle2 = arr2[right]
        right += 1
    
    # Return median
    if n % 2 == 1:
        return midEle1
    else:
        return (midEle1 + midEle2) / 2
```

**Time Complexity:** O(n1 + n2)  
**Space Complexity:** O(1)

---

### Approach 3: Binary Search on Partition

**Intuition:**
Use binary search on the smaller array to find the correct partition. The partition divides both arrays such that all elements on the left are ≤ all elements on the right. The median is derived from the elements around the partition.

**Code:**
```python
import math

def optimized(arr1, arr2):
    n1, n2 = len(arr1), len(arr2)
    
    # Ensure arr1 is the smaller array for efficiency
    if n1 > n2:
        return optimized(arr2, arr1)
    
    n = n1 + n2
    length = math.ceil(n / 2)  # Left half size
    low, high = 0, n1
    
    while low <= high:
        mid = (low + high) // 2
        mid1 = mid  # Number of elements from arr1 in left half
        mid2 = length - mid1  # Number of elements from arr2 in left half
        
        # Get boundary elements
        left1 = arr1[mid1-1] if mid1 != 0 else float('-inf')
        left2 = arr2[mid2-1] if mid2 != 0 else float('-inf')
        right1 = arr1[mid1] if mid1 != n1 else float('inf')
        right2 = arr2[mid2] if mid2 != n2 else float('inf')
        
        # Check if valid partition found
        if left1 <= right2 and left2 <= right1:
            # Odd length: return max of left half
            if n % 2 == 1:
                return max(left1, left2)
            # Even length: return average of middle two
            else:
                midEle1 = max(left1, left2)
                midEle2 = min(right1, right2)
                return (midEle1 + midEle2) / 2
        # Too many elements from arr1, move left
        elif left1 > right2:
            high = mid - 1
        # Too few elements from arr1, move right
        else:
            low = mid + 1
    
    return -1
```

**Time Complexity:** O(log(min(n1, n2)))  
**Space Complexity:** O(1)

---

## 3. Kth Element of 2 Sorted Arrays

### Problem Description
Given two sorted arrays `a` and `b` of size m and n respectively, find the kth element of the final sorted array (1-indexed).

### Sample Test Cases

```
Input: a = [2, 3, 6, 7, 9], b = [1, 4, 8, 10], k = 5
Output: 6
Explanation: Merged array [1, 2, 3, 4, 6, 7, 8, 9, 10], 5th element is 6

Input: a = [100, 112, 256, 349, 770], b = [72, 86, 113, 119, 265, 445, 892], k = 7
Output: 256
Explanation: Merged array [72, 86, 100, 112, 113, 119, 256, ...], 7th element is 256
```

---

### Approach: Binary Search on Partition

**Intuition:**
Similar to finding median, but instead of splitting at the middle, we partition to get exactly k elements on the left. The kth element is the maximum of the left partition.

**Code:**
```python
import math

def optimized(arr1, arr2, k):
    n1, n2 = len(arr1), len(arr2)
    
    # Ensure arr1 is the smaller array for efficiency
    if n1 > n2:
        return optimized(arr2, arr1, k)
    
    n = n1 + n2
    length = k  # We need k elements in left partition
    
    # Adjust bounds to ensure valid partitions
    low = max(0, k - n2)  # Minimum elements from arr1
    high = min(n1, k)  # Maximum elements from arr1
    
    while low <= high:
        mid = (low + high) >> 1  # Bit shift for division by 2
        mid1 = mid  # Number of elements from arr1 in left partition
        mid2 = length - mid1  # Number of elements from arr2 in left partition
        
        # Get boundary elements
        left1 = arr1[mid1-1] if mid1 > 0 else float('-inf')
        left2 = arr2[mid2-1] if mid2 > 0 else float('-inf')
        right1 = arr1[mid1] if mid1 < n1 else float('inf')
        right2 = arr2[mid2] if mid2 < n2 else float('inf')
        
        # Check if valid partition found
        if left1 <= right2 and left2 <= right1:
            # Kth element is max of left partition
            return max(left1, left2)
        # Too many elements from arr1, move left
        elif left1 > right2:
            high = mid - 1
        # Too few elements from arr1, move right
        else:
            low = mid + 1
    
    return -1
```

**Time Complexity:** O(log(min(n1, n2)))  
**Space Complexity:** O(1)

---

## Comparison Summary

| # | Problem | Brute Force | Optimized Approach | Key Technique |
|---|---------|-------------|-------------------|---------------|
| 1 | Find Peak Element | O(n) / O(1) | O(log n) / O(1) | Move toward increasing slope |
| 2 | Median of 2 Arrays | O(n1+n2) / O(n1+n2) | O(log(min(n1,n2))) / O(1) | Binary search on partition |
| 3 | Kth Element of 2 Arrays | O(n1+n2) / O(1) | O(log(min(n1,n2))) / O(1) | Binary search on partition |

### Quick Reference Pattern Guide

**Peak Finding Pattern:**
- Used for: Problem 1
- Key: Move in the direction of the slope (toward increasing values)
- Guarantee: At least one peak always exists

**Partition Pattern (2 Arrays):**
- Used for: Problems 2, 3
- Key: Binary search on smaller array to find valid partition
- Valid partition: `left1 ≤ right2` and `left2 ≤ right1`
- Applications: Median, Kth element, percentile calculations

**Key Insights:**
1. **Find Peak Element**: Always move toward the increasing side - a peak is guaranteed to exist in that direction
2. **Median of 2 Arrays**: Partition both arrays so left half has ⌈n/2⌉ elements; median comes from boundary elements
3. **Kth Element**: Similar to median but partition to get exactly k elements on the left; answer is max of left partition
4. **Optimization**: Always binary search on the smaller array to minimize time complexity
