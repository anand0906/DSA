# Binary Search - Revision Notes

## Table of Contents
1. [Search X in Sorted Array](#1-search-x-in-sorted-array)
2. [Lower Bound](#2-lower-bound)
3. [Upper Bound](#3-upper-bound)
4. [Search Insert Position](#4-search-insert-position)
5. [Floor and Ceil in Sorted Array](#5-floor-and-ceil-in-sorted-array)
6. [First and Last Occurrence](#6-first-and-last-occurrence)
7. [Search in Rotated Sorted Array - I](#7-search-in-rotated-sorted-array---i)
8. [Search in Rotated Sorted Array - II](#8-search-in-rotated-sorted-array---ii)
9. [Find Minimum in Rotated Sorted Array](#9-find-minimum-in-rotated-sorted-array)
10. [Find Out How Many Times Array is Rotated](#10-find-out-how-many-times-array-is-rotated)
11. [Single Element in Sorted Array](#11-single-element-in-sorted-array)
12. [Comparison Summary](#comparison-summary)

---

## 1. Search X in Sorted Array

### Problem Description
Given a sorted array of integers `nums` with 0-based indexing, find the index of a specified target integer. If the target is found in the array, return its index. If the target is not found, return -1.

### Sample Test Cases

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1

Input: nums = [-1,0,3,5,9,12], target = -1
Output: 0
```

---

### Approach 1: Linear Search

**Intuition:**
Iterate through the array and check each element until we find the target.

**Code:**
```python
def solve(n, arr, target):
    # Check each element one by one
    for index in range(n):
        if arr[index] == target:
            return index
    # Target not found
    return -1
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search

**Intuition:**
Since the array is sorted, use binary search to efficiently locate the target by repeatedly dividing the search space in half.

**Code:**
```python
def optimized(n, arr, target):
    low, high = 0, n-1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Target found at mid
        if arr[mid] == target:
            return mid
        # Target is in left half
        elif arr[mid] > target:
            high = mid - 1
        # Target is in right half
        else:
            low = mid + 1
    
    # Target not found
    return -1
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 2. Lower Bound

### Problem Description
Given a sorted array `nums` and an integer `x`, find the lower bound of `x`. The lower bound is the first and smallest index where the value at that index is greater than or equal to `x`. If no such index exists, return the size of the array.

### Sample Test Cases

```
Input: nums = [1,2,2,3], x = 2
Output: 1

Input: nums = [3,5,8,15,19], x = 9
Output: 3
```

---

### Approach 1: Linear Search

**Intuition:**
Iterate through the array and return the first index where the element is greater than or equal to the target.

**Code:**
```python
def solve(n, arr, target):
    # Find first element >= target
    for index in range(n):
        if arr[index] >= target:
            return index
    # No element >= target, return array size
    return n
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search

**Intuition:**
Use binary search to find the smallest index where the element is greater than or equal to the target. Keep updating the answer whenever we find a valid candidate.

**Code:**
```python
def optimized(n, arr, target):
    low, high = 0, n-1
    index = n  # Initialize to n (no valid index case)
    
    while low <= high:
        mid = (low + high) // 2
        
        # If exact match, this could be lower bound
        if arr[mid] == target:
            index = mid
            high = mid - 1  # Search left for smaller index
        # If mid element > target, it's a candidate
        elif arr[mid] > target:
            index = mid
            high = mid - 1  # Search left for smaller index
        # If mid element < target, search right
        else:
            low = mid + 1
    
    return index
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 3. Upper Bound

### Problem Description
Given a sorted array `nums` and an integer `x`, find the upper bound of `x`. The upper bound is the smallest index where the value is strictly greater than `x`. If no such index exists, return the size of the array.

### Sample Test Cases

```
Input: nums = [1,2,2,3], x = 2
Output: 3

Input: nums = [3,5,8,15,19], x = 9
Output: 3
```

---

### Approach 1: Linear Search

**Intuition:**
Iterate through the array and return the first index where the element is strictly greater than the target.

**Code:**
```python
def solve(n, arr, target):
    # Find first element > target
    for index in range(n):
        if arr[index] > target:
            return index
    # No element > target, return array size
    return n
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search

**Intuition:**
Use binary search to find the smallest index where the element is strictly greater than the target.

**Code:**
```python
def optimized(n, arr, target):
    low, high = 0, n-1
    index = n  # Initialize to n (no valid index case)
    
    while low <= high:
        mid = (low + high) // 2
        
        # If exact match, search right for upper bound
        if arr[mid] == target:
            low = mid + 1
        # If mid element > target, it's a candidate
        elif arr[mid] > target:
            index = mid
            high = mid - 1  # Search left for smaller index
        # If mid element < target, search right
        else:
            low = mid + 1
    
    return index
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 4. Search Insert Position

### Problem Description
Given a sorted array `nums` consisting of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

### Sample Test Cases

```
Input: nums = [1, 3, 5, 6], target = 5
Output: 2

Input: nums = [1, 3, 5, 6], target = 2
Output: 1
```

---

### Approach 1: Linear Search

**Intuition:**
Find the first position where the element is greater than or equal to the target (same as lower bound).

**Code:**
```python
def solve(n, arr, target):
    # Find first position where element >= target
    for index in range(n):
        if arr[index] >= target:
            return index
    # Insert at end if all elements < target
    return n
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search

**Intuition:**
This problem is identical to finding the lower bound. Use binary search to find the insertion position.

**Code:**
```python
def optimized(n, arr, target):
    low, high = 0, n-1
    index = n  # Initialize to n (insert at end case)
    
    while low <= high:
        mid = (low + high) // 2
        
        # If exact match found
        if arr[mid] == target:
            index = mid
            high = mid - 1
        # If mid element > target, potential insertion point
        elif arr[mid] > target:
            index = mid
            high = mid - 1
        # If mid element < target, search right
        else:
            low = mid + 1
    
    return index
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 5. Floor and Ceil in Sorted Array

### Problem Description
Given a sorted array `nums` and an integer `x`, find the floor and ceil of `x`. The floor is the largest element ≤ x. The ceiling is the smallest element ≥ x. If no floor or ceil exists, output -1.

### Sample Test Cases

```
Input: nums = [3, 4, 4, 7, 8, 10], x = 5
Output: 4 7

Input: nums = [3, 4, 4, 7, 8, 10], x = 8
Output: 8 8
```

---

### Approach 1: Linear Search

**Intuition:**
Iterate once to find the floor (largest element ≤ x) and once to find the ceil (smallest element ≥ x).

**Code:**
```python
def solve(n, arr, target):
    floor = -1
    ceil = -1
    
    # Find floor: largest element <= target
    for index in range(n):
        if arr[index] <= target:
            floor = arr[index]
        else:
            break
    
    # Find ceil: smallest element >= target
    for index in range(n):
        if arr[index] >= target:
            ceil = arr[index]
            break
    
    return [floor, ceil]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search

**Intuition:**
Use two separate binary searches: one to find the floor (largest ≤ x) and one to find the ceil (smallest ≥ x).

**Code:**
```python
def optimized(n, arr, target):
    # Find floor using binary search
    low, high = 0, n-1
    floor = -1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Exact match is both floor and can break
        if arr[mid] == target:
            floor = arr[mid]
            low = mid + 1
            break
        # If mid > target, search left
        elif arr[mid] > target:
            high = mid - 1
        # If mid <= target, it's a candidate for floor
        else:
            floor = arr[mid]
            low = mid + 1
    
    # Find ceil using binary search
    low, high = 0, n-1
    ceil = -1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Exact match is ceil and can break
        if arr[mid] == target:
            ceil = arr[mid]
            low = mid + 1
            break
        # If mid > target, it's a candidate for ceil
        elif arr[mid] > target:
            ceil = arr[mid]
            high = mid - 1
        # If mid < target, search right
        else:
            low = mid + 1
    
    return [floor, ceil]
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 6. First and Last Occurrence

### Problem Description
Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given target value. If the target is not found in the array, return [-1, -1].

### Sample Test Cases

```
Input: nums = [5, 7, 7, 8, 8, 10], target = 8
Output: [3, 4]

Input: nums = [5, 7, 7, 8, 8, 10], target = 6
Output: [-1, -1]
```

---

### Approach 1: Linear Search (Two Passes)

**Intuition:**
Find the first occurrence by iterating from the start, then find the last occurrence by continuing to iterate.

**Code:**
```python
def solve(n, arr, target):
    # Find first occurrence
    first = -1
    for index in range(n):
        if arr[index] == target:
            first = index
            break
    
    # Find last occurrence
    last = -1
    for index in range(n):
        if arr[index] == target:
            last = index
    
    return [first, last]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Linear Search (Single Pass)

**Intuition:**
Find both first and last occurrence in a single pass by tracking when we see the target for the first time and updating the last occurrence continuously.

**Code:**
```python
def solve_2(n, arr, target):
    first = -1
    last = -1
    
    for index in range(n):
        # Record first occurrence
        if arr[index] == target and first == -1:
            first = index
        # Keep updating last occurrence
        if arr[index] == target:
            last = index
    
    return [first, last]
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 3: Binary Search

**Intuition:**
Use two binary searches: one to find the first occurrence (search left when target is found) and one to find the last occurrence (search right when target is found).

**Code:**
```python
def optimized(n, arr, target):
    # Find first occurrence
    low, high = 0, n-1
    first = -1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Found target, but search left for first occurrence
        if arr[mid] == target:
            first = mid
            high = mid - 1
        elif arr[mid] > target:
            high = mid - 1
        else:
            low = mid + 1
    
    # If target not found, return [-1, -1]
    if first == -1:
        return [-1, -1]
    
    # Find last occurrence
    low, high = 0, n-1
    last = -1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Found target, but search right for last occurrence
        if arr[mid] == target:
            last = mid
            low = mid + 1
        elif arr[mid] > target:
            high = mid - 1
        else:
            low = mid + 1
    
    return [first, last]
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 7. Search in Rotated Sorted Array - I

### Problem Description
Given an integer array `nums` sorted in ascending order (with distinct values) and a target value `k`. The array is rotated at some unknown pivot point. Find the index at which `k` is present, or return -1 if not present.

### Sample Test Cases

```
Input: nums = [4, 5, 6, 7, 0, 1, 2], k = 0
Output: 4

Input: nums = [4, 5, 6, 7, 0, 1, 2], k = 3
Output: -1
```

---

### Approach 1: Linear Search

**Intuition:**
Simply iterate through the array to find the target.

**Code:**
```python
def solve(n, arr, target):
    # Check each element
    for index in range(n):
        if arr[index] == target:
            return index
    return -1
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search

**Intuition:**
One half of the array will always be sorted. Identify which half is sorted, then check if the target lies in that sorted half. If yes, search there; otherwise, search the other half.

**Code:**
```python
def optimized(n, arr, target):
    low, high = 0, n-1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Target found
        if arr[mid] == target:
            return mid
        else:
            # Check if left half is sorted
            if arr[low] <= arr[mid]:
                # Check if target is in sorted left half
                if arr[low] <= target <= arr[mid]:
                    high = mid - 1
                else:
                    low = mid + 1
            # Right half is sorted
            else:
                # Check if target is in sorted right half
                if arr[mid] <= target <= arr[high]:
                    low = mid + 1
                else:
                    high = mid - 1
    
    return -1
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 8. Search in Rotated Sorted Array - II

### Problem Description
Given an integer array `nums` sorted in ascending order (may contain duplicates) and a target value `k`. The array is rotated at some unknown pivot point. Return True if `k` is present, otherwise return False.

### Sample Test Cases

```
Input: nums = [7, 8, 1, 2, 3, 3, 3, 4, 5, 6], k = 3
Output: True

Input: nums = [7, 8, 1, 2, 3, 3, 3, 4, 5, 6], k = 10
Output: False
```

---

### Approach 1: Linear Search

**Intuition:**
Iterate through the array to find the target.

**Code:**
```python
def solve(n, arr, target):
    # Check each element
    for index in range(n):
        if arr[index] == target:
            return True
    return False
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search (with Duplicate Handling)

**Intuition:**
Similar to Rotated Array I, but when we encounter duplicates at low, mid, and high, we can't determine which half is sorted. In this case, shrink the search space by moving low and high pointers.

**Code:**
```python
def optimized(n, arr, target):
    low, high = 0, n-1
    
    while low <= high:
        mid = (low + high) // 2
        
        # Target found
        if arr[mid] == target:
            return True
        else:
            # Handle duplicates: can't determine sorted half
            if arr[low] == arr[mid] == arr[high]:
                low += 1
                high -= 1
            # Check if left half is sorted
            elif arr[low] <= arr[mid]:
                # Check if target is in sorted left half
                if arr[low] <= target <= arr[mid]:
                    high = mid - 1
                else:
                    low = mid + 1
            # Right half is sorted
            else:
                # Check if target is in sorted right half
                if arr[mid] <= target <= arr[high]:
                    low = mid + 1
                else:
                    high = mid - 1
    
    return False
```

**Time Complexity:** O(log n) average case, O(n) worst case (all duplicates)  
**Space Complexity:** O(1)

---

## 9. Find Minimum in Rotated Sorted Array

### Problem Description
Given an integer array `nums` of size N, sorted in ascending order with distinct values, then rotated an unknown number of times. Find the minimum element in the array.

### Sample Test Cases

```
Input: nums = [4, 5, 6, 7, 0, 1, 2, 3]
Output: 0

Input: nums = [3, 4, 5, 1, 2]
Output: 1
```

---

### Approach 1: Linear Search

**Intuition:**
Iterate through the array to find the minimum element.

**Code:**
```python
def solve(n, arr):
    mini = float('inf')
    
    # Find minimum by checking each element
    for index in range(n):
        if arr[index] < mini:
            mini = arr[index]
    
    return mini
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search

**Intuition:**
The minimum element is in the unsorted half. Identify which half is sorted, take its minimum, and search the other half for a potentially smaller minimum.

**Code:**
```python
def optimized(n, arr):
    low, high = 0, n-1
    mini = float('inf')
    
    while low <= high:
        mid = (low + high) // 2
        
        # If left half is sorted
        if arr[low] <= arr[mid]:
            # Take minimum from sorted left half
            mini = min(mini, arr[low])
            # Search in unsorted right half
            low = mid + 1
        # Right half is sorted
        else:
            # Take minimum from sorted right half
            mini = min(mini, arr[mid])
            # Search in unsorted left half
            high = mid - 1
    
    return mini
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 10. Find Out How Many Times Array is Rotated

### Problem Description
Given an integer array `nums` of size n, sorted in ascending order with distinct values. The array has been right rotated an unknown number of times. Determine the number of rotations performed.

### Sample Test Cases

```
Input: nums = [4, 5, 6, 7, 0, 1, 2, 3]
Output: 4

Input: nums = [3, 4, 5, 1, 2]
Output: 3
```

---

### Approach 1: Linear Search

**Intuition:**
The number of rotations equals the index of the minimum element.

**Code:**
```python
def solve(n, arr):
    mini = float('inf')
    minIndex = 0
    
    # Find index of minimum element
    for index in range(n):
        if arr[index] < mini:
            mini = arr[index]
            minIndex = index
    
    return minIndex
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 2: Binary Search

**Intuition:**
Find the index of the minimum element using binary search. The minimum element's index represents the number of rotations.

**Code:**
```python
def optimized(n, arr):
    low, high = 0, n-1
    mini = float('inf')
    minIndex = 0
    
    while low <= high:
        mid = (low + high) // 2
        
        # If left half is sorted
        if arr[low] <= arr[mid]:
            # Check if left element is minimum
            if arr[low] < mini:
                mini = arr[low]
                minIndex = low
            # Search unsorted right half
            low = mid + 1
        # Right half is sorted
        else:
            # Check if mid element is minimum
            if arr[mid] < mini:
                mini = arr[mid]
                minIndex = mid
            # Search unsorted left half
            high = mid - 1
    
    return minIndex
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 11. Single Element in Sorted Array

### Problem Description
Given an array `nums` sorted in non-decreasing order where every number except one appears twice. Find the single number.

### Sample Test Cases

```
Input: nums = [1, 1, 2, 2, 3, 3, 4, 5, 5, 6, 6]
Output: 4

Input: nums = [1, 1, 3, 5, 5]
Output: 3
```

---

### Approach 1: Hash Map

**Intuition:**
Count the frequency of each element and return the one that appears once.

**Code:**
```python
def solve(n, arr):
    count = {}
    
    # Count frequency of each element
    for index in range(n):
        if arr[index] in count:
            count[arr[index]] += 1
        else:
            count[arr[index]] = 1
    
    # Find element with count 1
    for element, cnt in count.items():
        if cnt == 1:
            return element
    
    return -1
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

### Approach 2: XOR

**Intuition:**
XOR of two same numbers is 0. XOR of all elements will cancel out duplicates, leaving only the single element.

**Code:**
```python
def solve_2(n, arr):
    xor = arr[0]
    
    # XOR all elements: duplicates cancel out
    for index in range(1, n):
        xor ^= arr[index]
    
    return xor
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### Approach 3: Binary Search

**Intuition:**
In the array before the single element, pairs start at even indices (0, 2, 4...). After the single element, pairs start at odd indices. Use this property to binary search for the single element.

**Code:**
```python
def optimized(n, arr):
    # Edge cases: single element or at boundaries
    if n == 1:
        return arr[0]
    if arr[0] != arr[1]:
        return arr[0]
    if arr[n-1] != arr[n-2]:
        return arr[n-1]
    
    # Search in range [1, n-2] to avoid boundary checks
    low, high = 1, n-2
    
    while low <= high:
        mid = (low + high) // 2
        
        # Check if mid is the single element
        if arr[mid] != arr[mid-1] and arr[mid] != arr[mid+1]:
            return arr[mid]
        else:
            # If in left half (pairs at even indices)
            if (mid % 2 == 0 and arr[mid] == arr[mid+1]) or \
               (mid % 2 == 1 and arr[mid] == arr[mid-1]):
                # Single element is in right half
                low = mid + 1
            # If in right half (pairs at odd indices)
            else:
                # Single element is in left half
                high = mid - 1
    
    return -1
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## Comparison Summary

| # | Problem | Linear Search | Optimized Approach | Key Technique |
|---|---------|---------------|-------------------|---------------|
| 1 | Search in Sorted Array | O(n) / O(1) | O(log n) / O(1) | Standard Binary Search |
| 2 | Lower Bound | O(n) / O(1) | O(log n) / O(1) | Binary Search + Store candidate |
| 3 | Upper Bound | O(n) / O(1) | O(log n) / O(1) | Binary Search + Store candidate |
| 4 | Search Insert Position | O(n) / O(1) | O(log n) / O(1) | Same as Lower Bound |
| 5 | Floor and Ceil | O(n) / O(1) | O(log n) / O(1) | Two Binary Searches |
| 6 | First and Last Occurrence | O(n) / O(1) | O(log n) / O(1) | Two Binary Searches (left & right) |
| 7 | Rotated Array I | O(n) / O(1) | O(log n) / O(1) | Identify sorted half |
| 8 | Rotated Array II | O(n) / O(1) | O(log n) avg / O(1) | Handle duplicates by shrinking |
| 9 | Min in Rotated Array | O(n) / O(1) | O(log n) / O(1) | Search unsorted half |
| 10 | Count Rotations | O(n) / O(1) | O(log n) / O(1) | Find min element index |
| 11 | Single Element | O(n) / O(n) | O(log n) / O(1) | Pair pattern analysis |

### Quick Reference Pattern Guide

**Standard Binary Search Pattern:**
- Used for: Problems 1, 2, 3, 4
- Key: Compare mid with target, adjust search space

**Modified Binary Search Pattern:**
- Used for: Problems 5, 6
- Key: Multiple binary searches or store candidates

**Rotated Array Pattern:**
- Used for: Problems 7, 8, 9, 10
- Key: Identify which half is sorted, make decision based on that

**Index Pattern Analysis:**
- Used for: Problem 11
- Key: Analyze index properties (even/odd) to determine search direction
