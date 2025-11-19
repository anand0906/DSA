# Binary Search Problems - Comprehensive Guide

## Table of Contents
1. [Search in Sorted Array (Binary Search)](#1-search-in-sorted-array-binary-search)
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

---

## 1. Search in Sorted Array (Binary Search)

### 📝 Problem Description
Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [-1, 0, 3, 5, 9, 12], target = 9
Output: 4

Visual Representation:
Index:  0   1   2   3   4   5
Array: [-1, 0,  3,  5,  9, 12]
                        ↑
                     target = 9 found at index 4

Binary Search Process:
Step 1: left = 0, right = 5, mid = 2
        nums[2] = 3 < 9 → search right half
        
Step 2: left = 3, right = 5, mid = 4
        nums[4] = 9 == 9 → Found! Return 4
```

**Example 2:**
```
Input: nums = [-1, 0, 3, 5, 9, 12], target = 2
Output: -1

Explanation: 2 does not exist in nums, so return -1

Binary Search Process:
Step 1: left = 0, right = 5, mid = 2
        nums[2] = 3 > 2 → search left half
        
Step 2: left = 0, right = 1, mid = 0
        nums[0] = -1 < 2 → search right half
        
Step 3: left = 1, right = 1, mid = 1
        nums[1] = 0 < 2 → search right half
        
Step 4: left = 2, right = 1 (left > right) → Not found! Return -1
```

### ⚙️ Constraints
- `1 <= nums.length <= 10^4`
- `-10^4 < nums[i], target < 10^4`
- All the integers in `nums` are **unique**
- `nums` is sorted in ascending order

---

## 2. Lower Bound

### 📝 Problem Description
Given a sorted array of `N` integers and an integer `x`, find the lower bound of `x`.

The lower bound is the smallest index `ind` such that `arr[ind] >= x`. If no such index exists (i.e., all elements are smaller than `x`), return `N` (the size of the array).

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [1, 2, 3, 3, 5, 8, 8, 10, 10, 11], x = 3
Output: 2

Visual Representation:
Index:  0  1  2  3  4  5  6   7   8   9
Array: [1, 2, 3, 3, 5, 8, 8, 10, 10, 11]
              ↑
         Lower bound of 3 is at index 2
         (First position where arr[i] >= 3)
```

**Example 2:**
```
Input: arr = [1, 2, 3, 3, 7, 8, 9, 10, 10, 11], x = 5
Output: 4

Visual Representation:
Index:  0  1  2  3  4  5  6   7   8   9
Array: [1, 2, 3, 3, 7, 8, 9, 10, 10, 11]
                    ↑
         Lower bound of 5 is at index 4
         (First position where arr[i] >= 5, which is 7)
```

**Example 3:**
```
Input: arr = [1, 2, 3, 4, 5], x = 7
Output: 5

Explanation: All elements are smaller than 7, so return N = 5
```

### ⚙️ Constraints
- `1 <= N <= 10^5`
- `1 <= arr[i] <= 10^9`
- `1 <= x <= 10^9`
- Array is sorted in non-decreasing order

---

## 3. Upper Bound

### 📝 Problem Description
Given a sorted array of `N` integers and an integer `x`, find the upper bound of `x`.

The upper bound is the smallest index `ind` such that `arr[ind] > x`. If no such index exists (i.e., all elements are less than or equal to `x`), return `N` (the size of the array).

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [1, 2, 3, 3, 5, 8, 8, 10, 10, 11], x = 3
Output: 4

Visual Representation:
Index:  0  1  2  3  4  5  6   7   8   9
Array: [1, 2, 3, 3, 5, 8, 8, 10, 10, 11]
                    ↑
         Upper bound of 3 is at index 4
         (First position where arr[i] > 3, which is 5)
```

**Example 2:**
```
Input: arr = [1, 2, 3, 3, 7, 8, 9, 10, 10, 11], x = 5
Output: 4

Visual Representation:
Index:  0  1  2  3  4  5  6   7   8   9
Array: [1, 2, 3, 3, 7, 8, 9, 10, 10, 11]
                    ↑
         Upper bound of 5 is at index 4
         (First position where arr[i] > 5, which is 7)
```

**Example 3:**
```
Input: arr = [1, 2, 3, 4, 5], x = 5
Output: 5

Explanation: All elements are <= 5, so return N = 5
```

### ⚙️ Constraints
- `1 <= N <= 10^5`
- `1 <= arr[i] <= 10^9`
- `1 <= x <= 10^9`
- Array is sorted in non-decreasing order

---

## 4. Search Insert Position

### 📝 Problem Description
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [1, 3, 5, 6], target = 5
Output: 2

Visual Representation:
Index:  0  1  2  3
Array: [1, 3, 5, 6]
              ↑
         Target 5 found at index 2
```

**Example 2:**
```
Input: nums = [1, 3, 5, 6], target = 2
Output: 1

Visual Representation:
Index:  0  1  2  3
Array: [1, 3, 5, 6]
           ↑
         Target 2 should be inserted at index 1
         Result: [1, 2, 3, 5, 6]
```

**Example 3:**
```
Input: nums = [1, 3, 5, 6], target = 7
Output: 4

Visual Representation:
Index:  0  1  2  3  4
Array: [1, 3, 5, 6, _]
                    ↑
         Target 7 should be inserted at index 4
         Result: [1, 3, 5, 6, 7]
```

### ⚙️ Constraints
- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` contains **distinct** values sorted in **ascending** order
- `-10^4 <= target <= 10^4`

---

## 5. Floor and Ceil in Sorted Array

### 📝 Problem Description
Given a sorted array `arr[]` of size `N` without duplicates, and given a value `x`. Find the floor and ceiling of `x` in the array.

- **Floor of x**: The largest element in the array which is smaller than or equal to `x`.
- **Ceil of x**: The smallest element in the array which is greater than or equal to `x`.

If floor or ceil doesn't exist, return `-1`.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [1, 2, 8, 10, 10, 12, 19], x = 5
Output: floor = 2, ceil = 8

Visual Representation:
Index:  0  1  2   3   4   5   6
Array: [1, 2, 8, 10, 10, 12, 19]
           ↑  ↑
        Floor Ceil
        
Floor: Largest element <= 5 is 2
Ceil:  Smallest element >= 5 is 8
```

**Example 2:**
```
Input: arr = [1, 2, 8, 10, 10, 12, 19], x = 10
Output: floor = 10, ceil = 10

Visual Representation:
Index:  0  1  2   3   4   5   6
Array: [1, 2, 8, 10, 10, 12, 19]
                   ↑
              Floor & Ceil
        
When x exists in array, floor = ceil = x
```

**Example 3:**
```
Input: arr = [5, 6, 8, 9, 10], x = 3
Output: floor = -1, ceil = 5

Explanation: No element <= 3, so floor = -1
             Smallest element >= 3 is 5
```

### ⚙️ Constraints
- `1 <= N <= 10^5`
- `1 <= arr[i] <= 10^9`
- `1 <= x <= 10^9`
- Array is sorted in ascending order with distinct elements

---

## 6. First and Last Occurrence

### 📝 Problem Description
Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [5, 7, 7, 8, 8, 10], target = 8
Output: [3, 4]

Visual Representation:
Index:  0  1  2  3  4   5
Array: [5, 7, 7, 8, 8, 10]
                   ↑  ↑
              First Last
              
First occurrence of 8 is at index 3
Last occurrence of 8 is at index 4
```

**Example 2:**
```
Input: nums = [5, 7, 7, 8, 8, 10], target = 6
Output: [-1, -1]

Explanation: 6 is not present in the array
```

**Example 3:**
```
Input: nums = [], target = 0
Output: [-1, -1]

Explanation: Empty array, target not found
```

**Example 4:**
```
Input: nums = [1], target = 1
Output: [0, 0]

Visual Representation:
Index:  0
Array: [1]
        ↑
    First & Last
```

### ⚙️ Constraints
- `0 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- `nums` is a non-decreasing array
- `-10^9 <= target <= 10^9`

---

## 7. Search in Rotated Sorted Array - I

### 📝 Problem Description
There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**).

For example, `[0, 1, 2, 4, 5, 6, 7]` might be rotated at pivot index `3` and become `[4, 5, 6, 7, 0, 1, 2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [4, 5, 6, 7, 0, 1, 2], target = 0
Output: 4

Visual Representation:
Original: [0, 1, 2, 4, 5, 6, 7]
Rotated:  [4, 5, 6, 7, 0, 1, 2]
Index:     0  1  2  3  4  5  6
                        ↑
                    target = 0

Process:
- Array is rotated at index 4
- Left part [4,5,6,7] is sorted
- Right part [0,1,2] is sorted
- Target 0 found at index 4
```

**Example 2:**
```
Input: nums = [4, 5, 6, 7, 0, 1, 2], target = 3
Output: -1

Explanation: 3 is not in the array
```

**Example 3:**
```
Input: nums = [1], target = 0
Output: -1
```

**Example 4:**
```
Input: nums = [4, 5, 6, 7, 0, 1, 2], target = 5
Output: 1

Visual Representation:
Index:  0  1  2  3  4  5  6
Array: [4, 5, 6, 7, 0, 1, 2]
           ↑
       target = 5 at index 1
```

### ⚙️ Constraints
- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- All values of `nums` are **unique**
- `nums` is an ascending array that is possibly rotated
- `-10^4 <= target <= 10^4`

---

## 8. Search in Rotated Sorted Array - II

### 📝 Problem Description
There is an integer array `nums` sorted in non-decreasing order (not necessarily with **distinct** values).

Before being passed to your function, `nums` is **rotated** at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**).

For example, `[0, 1, 2, 4, 4, 4, 5, 6, 6, 7]` might be rotated at pivot index `5` and become `[4, 5, 6, 6, 7, 0, 1, 2, 4, 4]`.

Given the array `nums` **after** the rotation and an integer `target`, return `true` if `target` is in `nums`, or `false` if it is not in `nums`.

You must decrease the overall operation steps as much as possible.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [2, 5, 6, 0, 0, 1, 2], target = 0
Output: true

Visual Representation:
Index:  0  1  2  3  4  5  6
Array: [2, 5, 6, 0, 0, 1, 2]
                   ↑  ↑
               target = 0 found
```

**Example 2:**
```
Input: nums = [2, 5, 6, 0, 0, 1, 2], target = 3
Output: false

Explanation: 3 is not present in the array
```

**Example 3:**
```
Input: nums = [1, 0, 1, 1, 1], target = 0
Output: true

Visual Representation:
Index:  0  1  2  3  4
Array: [1, 0, 1, 1, 1]
           ↑
       target = 0 found at index 1

Note: Duplicates make it harder to determine which half is sorted
```

### ⚙️ Constraints
- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- `nums` is guaranteed to be rotated at some pivot
- `-10^4 <= target <= 10^4`

**Follow up:** This problem is similar to Search in Rotated Sorted Array I, but `nums` may contain **duplicates**. Would this affect the runtime complexity? How and why?

---

## 9. Find Minimum in Rotated Sorted Array

### 📝 Problem Description
Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times.

For example, the array `nums = [0, 1, 2, 4, 5, 6, 7]` might become:
- `[4, 5, 6, 7, 0, 1, 2]` if it was rotated `4` times
- `[0, 1, 2, 4, 5, 6, 7]` if it was rotated `7` times

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of **unique** elements, return the minimum element of this array.

You must write an algorithm that runs in `O(log n)` time.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [3, 4, 5, 1, 2]
Output: 1

Visual Representation:
Original: [1, 2, 3, 4, 5]
Rotated:  [3, 4, 5, 1, 2]
Index:     0  1  2  3  4
                      ↑
                  Minimum = 1

The array was rotated 3 times
Minimum element is 1 at index 3
```

**Example 2:**
```
Input: nums = [4, 5, 6, 7, 0, 1, 2]
Output: 0

Visual Representation:
Index:  0  1  2  3  4  5  6
Array: [4, 5, 6, 7, 0, 1, 2]
                    ↑
                Minimum = 0

Minimum element is 0 at index 4
```

**Example 3:**
```
Input: nums = [11, 13, 15, 17]
Output: 11

Visual Representation:
Index:  0   1   2   3
Array: [11, 13, 15, 17]
        ↑
    Minimum = 11

Array is not rotated (or rotated n times)
First element is the minimum
```

### ⚙️ Constraints
- `n == nums.length`
- `1 <= n <= 5000`
- `-5000 <= nums[i] <= 5000`
- All the integers of `nums` are **unique**
- `nums` is sorted and rotated between `1` and `n` times

---

## 10. Find Out How Many Times Array is Rotated

### 📝 Problem Description
Given an ascending sorted rotated array `arr` of distinct integers of size `N`. The array is right-rotated `K` times. Find the value of `K`.

**Note:** Right rotation means shifting elements to the right, where the last element moves to the first position.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [15, 18, 2, 3, 6, 12]
Output: 2

Visual Representation:
Original: [2, 3, 6, 12, 15, 18]
After 1:  [18, 2, 3, 6, 12, 15]
After 2:  [15, 18, 2, 3, 6, 12]  ← Current state
Index:     0   1  2  3  4   5
                  ↑
            Minimum at index 2

The array is rotated 2 times
Minimum element's index = number of rotations
```

**Example 2:**
```
Input: arr = [7, 9, 11, 12, 5]
Output: 4

Visual Representation:
Original: [5, 7, 9, 11, 12]
After 1:  [12, 5, 7, 9, 11]
After 2:  [11, 12, 5, 7, 9]
After 3:  [9, 11, 12, 5, 7]
After 4:  [7, 9, 11, 12, 5]  ← Current state
Index:     0  1   2   3  4
                          ↑
                   Minimum at index 4

The array is rotated 4 times
```

**Example 3:**
```
Input: arr = [1, 2, 3, 4, 5]
Output: 0

Explanation: Array is not rotated (or rotated 5 times = 0 effective rotations)
```

### ⚙️ Constraints
- `1 <= N <= 10^5`
- `1 <= arr[i] <= 10^9`
- All elements are distinct
- Array is sorted and rotated

---

## 11. Single Element in Sorted Array

### 📝 Problem Description
You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Return the single element that appears only once.

Your solution must run in `O(log n)` time and `O(1)` space.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [1, 1, 2, 3, 3, 4, 4, 8, 8]
Output: 2

Visual Representation:
Index:  0  1  2  3  4  5  6  7  8
Array: [1, 1, 2, 3, 3, 4, 4, 8, 8]
        ↑  ↑  ↑  ↑  ↑  ↑  ↑  ↑  ↑
       pair  single  pair  pair  pair

Element 2 appears only once at index 2
```

**Example 2:**
```
Input: nums = [3, 3, 7, 7, 10, 11, 11]
Output: 10

Visual Representation:
Index:  0  1  2  3   4   5   6
Array: [3, 3, 7, 7, 10, 11, 11]
        ↑  ↑  ↑  ↑   ↑   ↑   ↑
       pair  pair  single  pair

Element 10 appears only once at index 4
```

**Example 3:**
```
Input: nums = [1]
Output: 1

Explanation: Single element array
```

**Pattern Observation:**
```
Before the single element:
- Pairs start at even indices: (0,1), (2,3), (4,5)...
- First occurrence at even index, second at odd index

After the single element:
- Pairs start at odd indices: (5,6), (7,8)...
- First occurrence at odd index, second at even index

Example: [1, 1, 2, 3, 3, 4, 4]
          0  1  2  3  4  5  6
         even odd (single) odd even odd even
         
This pattern helps identify which half contains the single element
```

### ⚙️ Constraints
- `1 <= nums.length <= 10^5`
- `0 <= nums[i] <= 10^5`
- `nums.length` is always odd
- Every element appears exactly twice except for one element which appears once

---
