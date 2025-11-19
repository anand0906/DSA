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

# Binary Search on Answer - Advanced Problems

## Table of Contents
1. [Find Square Root of a Number](#1-find-square-root-of-a-number)
2. [Find Nth Root of a Number](#2-find-nth-root-of-a-number)
3. [Find the Smallest Divisor](#3-find-the-smallest-divisor)
4. [Koko Eating Bananas](#4-koko-eating-bananas)
5. [Minimum Days to Make M Bouquets](#5-minimum-days-to-make-m-bouquets)
6. [Kth Missing Positive Number](#6-kth-missing-positive-number)

---

## 1. Find Square Root of a Number

### 📝 Problem Description
Given a non-negative integer `x`, return the square root of `x` rounded down to the nearest integer. The returned integer should be non-negative as well.

You must not use any built-in exponent function or operator.

For example, do not use `pow(x, 0.5)` in C++ or `x ** 0.5` in Python.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: x = 4
Output: 2

Visual Representation:
√4 = 2.000... → Round down = 2

Explanation: The square root of 4 is 2, so we return 2
```

**Example 2:**
```
Input: x = 8
Output: 2

Visual Representation:
√8 = 2.828... → Round down = 2

Binary Search Process:
Range: 1 to 8
Try 4: 4² = 16 > 8 → search left
Try 2: 2² = 4 < 8 → possible answer, search right
Try 3: 3² = 9 > 8 → search left
Answer: 2
```

**Example 3:**
```
Input: x = 1
Output: 1

Explanation: √1 = 1
```

**Example 4:**
```
Input: x = 0
Output: 0

Explanation: √0 = 0
```

**Example 5:**
```
Input: x = 15
Output: 3

Visual Representation:
√15 = 3.872... → Round down = 3

Testing:
3² = 9 ≤ 15 ✓
4² = 16 > 15 ✗
Answer: 3
```

### ⚙️ Constraints
- `0 <= x <= 2³¹ - 1`

---

## 2. Find Nth Root of a Number

### 📝 Problem Description
You are given two positive integers `n` and `m`. You have to return the `nth` root of `m`, i.e., `m^(1/n)`. If the `nth` root is not an integer, return `-1`.

**Note:** The `nth` root of a number `m` is defined as a number `x` such that `x^n = m`. If no such `x` exists that is an integer, return `-1`.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: n = 3, m = 27
Output: 3

Visual Representation:
∛27 = 3 because 3³ = 27

Verification:
3 × 3 × 3 = 27 ✓
```

**Example 2:**
```
Input: n = 4, m = 69
Output: -1

Visual Representation:
⁴√69 = 2.898... (not an integer)

Testing:
2⁴ = 16 < 69
3⁴ = 81 > 69
No integer x where x⁴ = 69
Answer: -1
```

**Example 3:**
```
Input: n = 2, m = 16
Output: 4

Visual Representation:
√16 = 4 because 4² = 16

Verification:
4 × 4 = 16 ✓
```

**Example 4:**
```
Input: n = 3, m = 64
Output: 4

Visual Representation:
∛64 = 4 because 4³ = 64

Binary Search Process:
Range: 1 to 64
Try 32: 32³ = 32768 > 64 → search left
Try 16: 16³ = 4096 > 64 → search left
Try 8: 8³ = 512 > 64 → search left
Try 4: 4³ = 64 = 64 → Found! ✓
```

**Example 5:**
```
Input: n = 2, m = 9
Output: 3

Explanation: 3² = 9
```

### ⚙️ Constraints
- `1 <= n <= 30`
- `1 <= m <= 10^9`

---

## 3. Find the Smallest Divisor

### 📝 Problem Description
Given an array of integers `nums` and an integer `threshold`, we will choose a positive integer `divisor`, divide all the array by it, and sum the division's result. Find the smallest divisor such that the result mentioned above is less than or equal to `threshold`.

Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: `7/3 = 3` and `10/2 = 5`).

The test cases are generated so that there will be an answer.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [1, 2, 5, 9], threshold = 6
Output: 5

Visual Representation:
Testing divisor = 5:
┌─────────────────────────────────┐
│ Element │ Division │ Ceil Value │
├─────────┼──────────┼────────────┤
│    1    │   1/5    │     1      │
│    2    │   2/5    │     1      │
│    5    │   5/5    │     1      │
│    9    │   9/5    │     2      │
└─────────┴──────────┴────────────┘
Sum = 1 + 1 + 1 + 2 = 5 ≤ 6 ✓

Testing divisor = 4:
Sum = ⌈1/4⌉ + ⌈2/4⌉ + ⌈5/4⌉ + ⌈9/4⌉
    = 1 + 1 + 2 + 3 = 7 > 6 ✗

Smallest divisor = 5
```

**Example 2:**
```
Input: nums = [44, 22, 33, 11, 1], threshold = 5
Output: 44

Visual Representation:
Testing divisor = 44:
┌─────────────────────────────────┐
│ Element │ Division │ Ceil Value │
├─────────┼──────────┼────────────┤
│   44    │  44/44   │     1      │
│   22    │  22/44   │     1      │
│   33    │  33/44   │     1      │
│   11    │  11/44   │     1      │
│    1    │   1/44   │     1      │
└─────────┴──────────┴────────────┘
Sum = 1 + 1 + 1 + 1 + 1 = 5 ≤ 5 ✓

Smallest divisor = 44
```

**Example 3:**
```
Input: nums = [2, 3, 5, 7, 11], threshold = 11
Output: 3

Visual Representation:
Testing divisor = 3:
⌈2/3⌉ + ⌈3/3⌉ + ⌈5/3⌉ + ⌈7/3⌉ + ⌈11/3⌉
= 1 + 1 + 2 + 3 + 4 = 11 ≤ 11 ✓

Testing divisor = 2:
⌈2/2⌉ + ⌈3/2⌉ + ⌈5/2⌉ + ⌈7/2⌉ + ⌈11/2⌉
= 1 + 2 + 3 + 4 + 6 = 16 > 11 ✗

Smallest divisor = 3
```

### ⚙️ Constraints
- `1 <= nums.length <= 5 * 10^4`
- `1 <= nums[i] <= 10^6`
- `nums.length <= threshold <= 10^6`

---

## 4. Koko Eating Bananas

### 📝 Problem Description
Koko loves to eat bananas. There are `n` piles of bananas, the `i-th` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: piles = [3, 6, 7, 11], h = 8
Output: 4

Visual Representation:
Piles:  [3, 6, 7, 11]
Speed k = 4 bananas/hour

Hour-by-hour breakdown:
┌──────┬─────────┬──────────────┬───────────┐
│ Hour │  Pile   │ Bananas Eaten│ Remaining │
├──────┼─────────┼──────────────┼───────────┤
│  1   │ Pile 1  │      3       │     0     │
│  2   │ Pile 2  │      4       │     2     │
│  3   │ Pile 2  │      2       │     0     │
│  4   │ Pile 3  │      4       │     3     │
│  5   │ Pile 3  │      3       │     0     │
│  6   │ Pile 4  │      4       │     7     │
│  7   │ Pile 4  │      4       │     3     │
│  8   │ Pile 4  │      3       │     0     │
└──────┴─────────┴──────────────┴───────────┘

Total hours needed = ⌈3/4⌉ + ⌈6/4⌉ + ⌈7/4⌉ + ⌈11/4⌉
                   = 1 + 2 + 2 + 3 = 8 hours ✓
```

**Example 2:**
```
Input: piles = [30, 11, 23, 4, 20], h = 5
Output: 30

Visual Representation:
Piles:  [30, 11, 23, 4, 20]
Speed k = 30 bananas/hour

Hours needed per pile:
Pile 1: ⌈30/30⌉ = 1 hour
Pile 2: ⌈11/30⌉ = 1 hour
Pile 3: ⌈23/30⌉ = 1 hour
Pile 4: ⌈4/30⌉ = 1 hour
Pile 5: ⌈20/30⌉ = 1 hour
Total: 1 + 1 + 1 + 1 + 1 = 5 hours ✓

Minimum speed = 30 (eat fastest pile in 1 hour)
```

**Example 3:**
```
Input: piles = [30, 11, 23, 4, 20], h = 6
Output: 23

Visual Representation:
Speed k = 23 bananas/hour

Hours needed:
⌈30/23⌉ + ⌈11/23⌉ + ⌈23/23⌉ + ⌈4/23⌉ + ⌈20/23⌉
= 2 + 1 + 1 + 1 + 1 = 6 hours ✓

If k = 22:
⌈30/22⌉ + ⌈11/22⌉ + ⌈23/22⌉ + ⌈4/22⌉ + ⌈20/22⌉
= 2 + 1 + 2 + 1 + 1 = 7 hours ✗

Minimum speed = 23
```

### ⚙️ Constraints
- `1 <= piles.length <= 10^4`
- `piles.length <= h <= 10^9`
- `1 <= piles[i] <= 10^9`

---

## 5. Minimum Days to Make M Bouquets

### 📝 Problem Description
You are given an integer array `bloomDay`, an integer `m` and an integer `k`.

You want to make `m` bouquets. To make a bouquet, you need to use `k` adjacent flowers from the garden.

The garden consists of `n` flowers, the `i-th` flower will bloom in the `bloomDay[i]` and then can be used in exactly one bouquet.

Return the minimum number of days you need to wait to be able to make `m` bouquets from the garden. If it is impossible to make `m` bouquets return `-1`.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: bloomDay = [1, 10, 3, 10, 2], m = 3, k = 1
Output: 3

Visual Representation:
Need: 3 bouquets, each with 1 flower (k=1)

Day 1: [B, _, _, _, _]  → 1 bouquet
Day 2: [B, _, _, _, B]  → 2 bouquets
Day 3: [B, _, B, _, B]  → 3 bouquets ✓

Timeline:
Day 0: [_, _, _, _, _]      (0 bloomed)
Day 1: [B, _, _, _, _]      (1 bloomed)
Day 2: [B, _, _, _, B]      (2 bloomed)
Day 3: [B, _, B, _, B]      (3 bloomed) → 3 bouquets!
Day 10: [B, B, B, B, B]     (5 bloomed)

Minimum days = 3
```

**Example 2:**
```
Input: bloomDay = [1, 10, 3, 10, 2], m = 3, k = 2
Output: -1

Visual Representation:
Need: 3 bouquets, each with 2 ADJACENT flowers (k=2)
Total flowers needed: 3 × 2 = 6
Available flowers: 5

Explanation: Cannot make 3 bouquets with 2 adjacent flowers
because we only have 5 flowers total
Answer: -1
```

**Example 3:**
```
Input: bloomDay = [7, 7, 7, 7, 12, 7, 7], m = 2, k = 3
Output: 12

Visual Representation:
Need: 2 bouquets, each with 3 ADJACENT flowers

Position:  0  1  2  3   4  5  6
bloomDay: [7, 7, 7, 7, 12, 7, 7]

Day 7:  [B, B, B, B, _, B, B]
        └──────┘         
         Bouquet 1 (positions 0-2) ✓
        
Can't make 2nd bouquet because position 4 not bloomed yet

Day 12: [B, B, B, B, B, B, B]
        └──────┘ └──────┘
        Bouquet 1  Bouquet 2 ✓
        (pos 0-2)  (pos 4-6)

Minimum days = 12
```

**Example 4:**
```
Input: bloomDay = [1, 10, 2, 9, 3, 8, 4, 7, 5, 6], m = 4, k = 2
Output: 9

Visual Representation:
Need: 4 bouquets, each with 2 adjacent flowers

Day 9:
Position:  0   1  2  3  4  5  6  7  8  9
bloomDay: [1, 10, 2, 9, 3, 8, 4, 7, 5, 6]
Status:   [B,  _, B, B, B, B, B, B, B, B]
          └──┘ └──┘ └──┘ └──┘
          B1   B2   B3   B4

4 bouquets can be made on day 9 ✓
```

### ⚙️ Constraints
- `bloomDay.length == n`
- `1 <= n <= 10^5`
- `1 <= bloomDay[i] <= 10^9`
- `1 <= m <= 10^6`
- `1 <= k <= n`

---

## 6. Kth Missing Positive Number

### 📝 Problem Description
Given an array `arr` of positive integers sorted in a strictly increasing order, and an integer `k`.

Return the `k-th` positive integer that is missing from this array.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [2, 3, 4, 7, 11], k = 5
Output: 9

Visual Representation:
Array:   [2, 3, 4, 7, 11]
Missing: 1, 5, 6, 8, 9, 10, 12, 13...
         ↑  ↑  ↑  ↑  ↑
         1st 2nd 3rd 4th 5th

Complete sequence analysis:
Position │ Expected │ Actual │ Missing Numbers
─────────┼──────────┼────────┼─────────────────
    0    │    1     │   2    │      1
    1    │    2     │   3    │      -
    2    │    3     │   4    │      -
    3    │    4     │   7    │    5, 6
    4    │    7     │   11   │  8, 9, 10

The 5th missing number is 9
```

**Example 2:**
```
Input: arr = [1, 2, 3, 4], k = 2
Output: 6

Visual Representation:
Array:   [1, 2, 3, 4]
Missing: 5, 6, 7, 8, 9...
         ↑  ↑
         1st 2nd

Explanation: 
All numbers 1-4 are present
5 is the 1st missing number
6 is the 2nd missing number ✓
```

**Example 3:**
```
Input: arr = [2], k = 1
Output: 1

Visual Representation:
Array:   [2]
Missing: 1, 3, 4, 5, 6...
         ↑
         1st

The 1st missing number is 1
```

**Example 4:**
```
Input: arr = [5, 6, 7, 8, 9], k = 9
Output: 14

Visual Representation:
Array starts at 5, so missing: 1, 2, 3, 4 (first 4 missing)
After 9: 10, 11, 12, 13, 14...

Missing numbers:
1, 2, 3, 4, 10, 11, 12, 13, 14...
↑  ↑  ↑  ↑   ↑   ↑   ↑   ↑   ↑
1  2  3  4   5   6   7   8   9

The 9th missing number is 14
```

**Example 5:**
```
Input: arr = [1, 3], k = 1
Output: 2

Visual Representation:
Array:   [1, 3]
Missing: 2, 4, 5, 6...
         ↑
         1st

The 1st missing number is 2
```

### ⚙️ Constraints
- `1 <= arr.length <= 1000`
- `1 <= arr[i] <= 1000`
- `1 <= k <= 1000`
- `arr[i] < arr[j]` for `1 <= i < j <= arr.length`

**Follow up:** Could you solve this problem in less than O(n) complexity?

---


