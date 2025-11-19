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

# Binary Search Optimization Problems

## Table of Contents
1. [Aggressive Cows](#1-aggressive-cows)
2. [Split Array - Largest Sum](#2-split-array---largest-sum)
3. [Book Allocation Problem](#3-book-allocation-problem)
4. [The Painter's Partition Problem](#4-the-painters-partition-problem)
5. [Minimize Max Distance to Gas Station](#5-minimize-max-distance-to-gas-station)

---

## 1. Aggressive Cows

### 📝 Problem Description
You are given an array `stalls[]` of size `n`, representing the positions of `n` stalls along a straight line. You are also given an integer `k`, representing the number of aggressive cows.

The task is to assign stalls to `k` cows such that the **minimum distance** between any two cows is **maximized**. Return the largest minimum distance possible.

**Note:** Cows are aggressive and will fight if placed too close.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: stalls = [1, 2, 4, 8, 9], k = 3
Output: 3

Visual Representation:
Stalls: [1, 2, 4, 8, 9]
        ↓     ↓     ↓
Position: 1  2  3  4  5  6  7  8  9
         [C]    [C]          [C]
         
Cow placement:
- Cow 1 at position 1
- Cow 2 at position 4 (distance = 3)
- Cow 3 at position 8 (distance = 4)

Minimum distance between any two cows = min(3, 4) = 3

Why not distance = 4?
Position: 1  2  3  4  5  6  7  8  9
         [C]                [C]
We can only place 2 cows with distance ≥ 4

Maximum of minimum distance = 3 ✓
```

**Example 2:**
```
Input: stalls = [0, 3, 4, 7, 10, 9], k = 4
Output: 3

Visual Representation:
Sorted stalls: [0, 3, 4, 7, 9, 10]

Position: 0  1  2  3  4  5  6  7  8  9  10
         [C]       [C]       [C]       [C]

Cow placement with minimum distance = 3:
- Cow 1 at position 0
- Cow 2 at position 3 (distance = 3)
- Cow 3 at position 7 (distance = 4)
- Cow 4 at position 10 (distance = 3)

Minimum distance = min(3, 4, 3) = 3 ✓
```

**Example 3:**
```
Input: stalls = [1, 2, 8, 4, 9], k = 3
Output: 3

Visual Representation:
Sorted stalls: [1, 2, 4, 8, 9]

Position: 1  2  3  4  5  6  7  8  9
         [C]       [C]          [C]

Optimal placement:
- Cow 1 at position 1
- Cow 2 at position 4 (distance = 3)
- Cow 3 at position 8 (distance = 4)

Minimum distance = 3
```

**Example 4:**
```
Input: stalls = [10, 1, 2, 7, 5], k = 3
Output: 4

Visual Representation:
Sorted stalls: [1, 2, 5, 7, 10]

Position: 1  2  3  4  5  6  7  8  9  10
         [C]             [C]       [C]

Optimal placement:
- Cow 1 at position 1
- Cow 2 at position 5 (distance = 4)
- Cow 3 at position 10 (distance = 5)

Minimum distance = 4 ✓
```

### ⚙️ Constraints
- `2 <= n <= 10^5`
- `0 <= stalls[i] <= 10^9`
- `2 <= k <= n`

---

## 2. Split Array - Largest Sum

### 📝 Problem Description
Given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.

Return the minimized largest sum of the split.

A **subarray** is a contiguous part of the array.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [7, 2, 5, 10, 8], k = 2
Output: 18

Visual Representation:
Array: [7, 2, 5, 10, 8]

Split into 2 subarrays:
Option 1: [7] | [2, 5, 10, 8]
          sum=7  sum=25
          Largest sum = 25

Option 2: [7, 2] | [5, 10, 8]
          sum=9    sum=23
          Largest sum = 23

Option 3: [7, 2, 5] | [10, 8]
          sum=14      sum=18
          Largest sum = 18 ✓ (Minimum)

Option 4: [7, 2, 5, 10] | [8]
          sum=24          sum=8
          Largest sum = 24

Best split: [7, 2, 5] | [10, 8]
Minimized largest sum = 18
```

**Example 2:**
```
Input: nums = [1, 2, 3, 4, 5], k = 2
Output: 9

Visual Representation:
Array: [1, 2, 3, 4, 5]

Possible splits:
[1] | [2, 3, 4, 5]        → max(1, 14) = 14
[1, 2] | [3, 4, 5]        → max(3, 12) = 12
[1, 2, 3] | [4, 5]        → max(6, 9) = 9 ✓
[1, 2, 3, 4] | [5]        → max(10, 5) = 10

Best split: [1, 2, 3] | [4, 5]
Minimized largest sum = 9
```

**Example 3:**
```
Input: nums = [1, 4, 4], k = 3
Output: 4

Visual Representation:
Array: [1, 4, 4]

Split into 3 subarrays (each element separately):
[1] | [4] | [4]
sum=1 sum=4 sum=4

Largest sum = 4 ✓
```

**Example 4:**
```
Input: nums = [10, 5, 13, 4, 8, 4, 5, 11, 14, 9, 16, 10, 20, 8], k = 8
Output: 25

Visual Representation:
One optimal split:
[10, 5] | [13, 4] | [8, 4] | [5, 11] | [14, 9] | [16] | [10, 20] | [8]
  15       17        12       16        23       16      30        8

Largest sum = 30... but we can do better

Better split:
[10, 5] | [13] | [4, 8, 4] | [5, 11] | [14] | [9, 16] | [10, 20] | [8]
  15      13      16          16        14      25        30        8

After trying all possibilities:
Minimized largest sum = 25
```

### ⚙️ Constraints
- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 10^6`
- `1 <= k <= min(50, nums.length)`

---

## 3. Book Allocation Problem

### 📝 Problem Description
Given an array `arr[]` of size `n` where `arr[i]` represents the number of pages in the `i-th` book. There are `m` students, and the task is to allocate all the books to the students.

**Allocate books in such a way that:**
1. Each student gets at least one book
2. Each book should be allocated to exactly one student
3. Book allocation should be in a contiguous manner
4. The objective is to minimize the maximum number of pages assigned to a student

Return the minimum possible maximum pages a student can get. If it's not possible to allocate all books to `m` students, return `-1`.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [12, 34, 67, 90], m = 2
Output: 113

Visual Representation:
Books: [12, 34, 67, 90]
       ↓   ↓   ↓   ↓
Pages: 12  34  67  90

Allocate to 2 students:

Option 1: [12] | [34, 67, 90]
          12     191
          Max = 191

Option 2: [12, 34] | [67, 90]
          46         157
          Max = 157

Option 3: [12, 34, 67] | [90]
          113            90
          Max = 113 ✓ (Minimum)

Option 4: [12, 34, 67, 90] | []
          Not valid (student 2 gets no book)

Best allocation:
Student 1: Books [12, 34, 67] → 113 pages
Student 2: Book [90] → 90 pages
Minimum of maximum pages = 113
```

**Example 2:**
```
Input: arr = [10, 20, 30, 40], m = 2
Output: 60

Visual Representation:
Books: [10, 20, 30, 40]

Testing different allocations:
[10] | [20, 30, 40]        → max(10, 90) = 90
[10, 20] | [30, 40]        → max(30, 70) = 70
[10, 20, 30] | [40]        → max(60, 40) = 60 ✓

Best allocation:
Student 1: [10, 20, 30] → 60 pages
Student 2: [40] → 40 pages
Answer: 60
```

**Example 3:**
```
Input: arr = [10, 20, 30], m = 1
Output: 60

Visual Representation:
Books: [10, 20, 30]

Only 1 student gets all books:
Student 1: [10, 20, 30] → 60 pages

Answer: 60
```

**Example 4:**
```
Input: arr = [5, 17, 100, 11], m = 4
Output: 100

Visual Representation:
Books: [5, 17, 100, 11]

4 students, 4 books → Each student gets 1 book:
Student 1: [5] → 5 pages
Student 2: [17] → 17 pages
Student 3: [100] → 100 pages
Student 4: [11] → 11 pages

Maximum pages = 100
```

**Example 5:**
```
Input: arr = [10, 20, 30], m = 5
Output: -1

Explanation: 
We have 3 books but 5 students
Cannot allocate at least 1 book to each student
Answer: -1
```

### ⚙️ Constraints
- `1 <= n <= 10^5`
- `1 <= arr[i] <= 10^6`
- `1 <= m <= 10^5`

---

## 4. The Painter's Partition Problem

### 📝 Problem Description
Given an array `arr[]` of size `n` representing the length of `n` different boards and `k` painters available. Each painter takes 1 unit of time to paint 1 unit of the board.

The task is to find the **minimum time** to paint all boards under the constraints that:
1. Each painter can paint only contiguous sections of boards
2. A painter will only start painting another board after completing the current board
3. Multiple painters can work simultaneously

Return the minimum time required to paint all boards.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr = [10, 20, 30, 40], k = 2
Output: 60

Visual Representation:
Boards: [10, 20, 30, 40]
        ↓   ↓   ↓   ↓
Length: 10  20  30  40 units

Allocate to 2 painters:

Option 1: [10] | [20, 30, 40]
          10     90 time units
          Max = 90

Option 2: [10, 20] | [30, 40]
          30         70 time units
          Max = 70

Option 3: [10, 20, 30] | [40]
          60             40 time units
          Max = 60 ✓ (Minimum)

Best allocation:
Painter 1: Boards [10, 20, 30] → 60 time units
Painter 2: Board [40] → 40 time units

Both work simultaneously:
Timeline:
0────────────────────60  (Painter 1 finishes)
0──────────40            (Painter 2 finishes)

Total time = 60 units
```

**Example 2:**
```
Input: arr = [5, 10, 30, 20, 15], k = 3
Output: 35

Visual Representation:
Boards: [5, 10, 30, 20, 15]

Optimal allocation to 3 painters:
[5, 10] | [30] | [20, 15]
  15      30      35

Timeline:
Painter 1: 0────15
Painter 2: 0──────────30
Painter 3: 0──────────────35 (finishes last)

Minimum time = 35 units
```

**Example 3:**
```
Input: arr = [10, 10, 10, 10], k = 2
Output: 20

Visual Representation:
Boards: [10, 10, 10, 10]

Optimal allocation:
[10, 10] | [10, 10]
   20        20

Painter 1: 0────────20
Painter 2: 0────────20

Both finish at same time
Minimum time = 20 units
```

**Example 4:**
```
Input: arr = [1, 2, 3, 4, 5, 6, 7, 8, 9], k = 3
Output: 17

Visual Representation:
Boards: [1, 2, 3, 4, 5, 6, 7, 8, 9]
Sum = 45

Optimal allocation to 3 painters:
[1, 2, 3, 4, 5] | [6, 7] | [8, 9]
      15           13        17

Timeline:
Painter 1: 0────────────15
Painter 2: 0──────────13
Painter 3: 0────────────────17

Minimum time = 17 units
```

**Example 5:**
```
Input: arr = [100, 200, 300, 400], k = 4
Output: 400

Visual Representation:
4 painters, 4 boards → Each painter paints 1 board

[100] | [200] | [300] | [400]

Painter 1: 0──────────100
Painter 2: 0────────────────200
Painter 3: 0──────────────────────300
Painter 4: 0────────────────────────────400

Maximum time taken = 400 units
```

### ⚙️ Constraints
- `1 <= n <= 10^5`
- `1 <= k <= 10^5`
- `1 <= arr[i] <= 10^5`

---

## 5. Minimize Max Distance to Gas Station

### 📝 Problem Description
You are given a sorted array `stations[]` representing the positions of `n` gas stations on a highway. You need to add `k` new gas stations on the highway.

The goal is to minimize the **maximum distance** between any two consecutive gas stations after adding `k` new stations.

Return the minimum possible value of the maximum distance between consecutive gas stations after adding `k` new stations. The answer will be accepted if it's within `10^-6` of the actual answer.

**Note:** You can add gas stations anywhere between existing stations.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: stations = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 9
Output: 0.50

Visual Representation:
Original stations: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Distance between consecutive: all = 1

Adding 9 new stations optimally:
Between each pair, add 1 station at midpoint:

1 ─── 1.5 ─── 2 ─── 2.5 ─── 3 ─── 3.5 ─── 4 ...
    0.5     0.5     0.5     0.5     0.5

New stations at: 1.5, 2.5, 3.5, 4.5, 5.5, 6.5, 7.5, 8.5, 9.5

Maximum distance between consecutive stations = 0.5
```

**Example 2:**
```
Input: stations = [23, 24, 36, 39, 46, 56, 57, 65, 84, 98], k = 1
Output: 14.00

Visual Representation:
Original stations: [23, 24, 36, 39, 46, 56, 57, 65, 84, 98]

Distances between consecutive stations:
23→24: 1
24→36: 12
36→39: 3
39→46: 7
46→56: 10
56→57: 1
57→65: 8
65→84: 19 ← Largest gap
84→98: 14

Add 1 station at position 74.5 (middle of 65 and 84):
65 ────── 74.5 ────── 84
    9.5         9.5

Wait, but 84→98 = 14 is still larger!

Better: Add station in the gap 84→98 at position 91:
84 ────── 91 ────── 98
    7         7

But gap 65→84 = 19 remains largest

Optimal: Add in largest gap (65→84) at 74.5:
New gaps: 65→74.5 = 9.5 and 74.5→84 = 9.5
But 84→98 = 14 becomes the maximum

Maximum distance = 14.00
```

**Example 3:**
```
Input: stations = [1, 13, 17, 23], k = 5
Output: 4.00

Visual Representation:
Original: [1, 13, 17, 23]

Distances:
1→13: 12 (largest)
13→17: 4
17→23: 6

Adding 5 new stations optimally:
Divide the largest gap (1→13) into smaller segments

Add 3 stations in gap 1→13:
1 ─── 4 ─── 7 ─── 10 ─── 13
   3     3      3       3

Add 1 station in gap 17→23:
17 ─── 20 ─── 23
    3       3

Add 1 station in gap 13→17:
13 ─── 15 ─── 17
    2       2

Final gaps: 3, 3, 3, 3, 2, 2, 3, 3
But we need to minimize maximum...

Better distribution:
Add 3 in gap 1→13: [1, 4, 7, 10, 13] → max gap = 3
Add 1 in gap 17→23: [17, 20, 23] → max gap = 3
Add 1 in gap 13→17: [13, 15, 17] → max gap = 2

After optimization: Maximum distance ≈ 4.00
```

**Example 4:**
```
Input: stations = [3, 6, 12, 19, 33, 44, 67, 72, 89, 95], k = 2
Output: 14.00

Visual Representation:
Distances between stations:
3→6: 3
6→12: 6
12→19: 7
19→33: 14
33→44: 11
44→67: 23 ← Largest
67→72: 5
72→89: 17
89→95: 6

Add 2 stations in largest gap (44→67):
44 ──── 51.67 ──── 59.33 ──── 67
     7.67        7.67        7.67

But 72→89 = 17 is now the maximum

Better: Add 1 in 44→67 and 1 in 72→89:
44 ──── 55.5 ──── 67
    11.5       11.5

72 ──── 80.5 ──── 89
    8.5        8.5

Maximum after additions ≈ 14.00
```

### ⚙️ Constraints
- `10 <= stations.length <= 2000`
- `0 <= stations[i] <= 10^8`
- `stations` is sorted in strictly increasing order
- `0 <= k <= 10^6`
- Answers within `10^-6` of the actual answer will be accepted

---

# Binary Search - Peak & Median Problems

## Table of Contents
1. [Find Peak Element](#1-find-peak-element)
2. [Median of 2 Sorted Arrays](#2-median-of-2-sorted-arrays)
3. [Kth Element of 2 Sorted Arrays](#3-kth-element-of-2-sorted-arrays)

---

## 1. Find Peak Element

### 📝 Problem Description
A peak element is an element that is strictly greater than its neighbors.

Given a **0-indexed** integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in `O(log n)` time.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums = [1, 2, 3, 1]
Output: 2

Visual Representation:
Index:  0  1  2  3
Array: [1, 2, 3, 1]
              ↑
           Peak at index 2

Explanation:
- nums[2] = 3 is a peak element
- 3 > 2 (left neighbor)
- 3 > 1 (right neighbor)
```

**Example 2:**
```
Input: nums = [1, 2, 1, 3, 5, 6, 4]
Output: 5 (or 1)

Visual Representation:
Index:  0  1  2  3  4  5  6
Array: [1, 2, 1, 3, 5, 6, 4]
           ↑           ↑
        Peak 1      Peak 2

Two valid peaks:
- Index 1: nums[1] = 2 (2 > 1 and 2 > 1)
- Index 5: nums[5] = 6 (6 > 5 and 6 > 4)

Either answer is acceptable
```

**Example 3:**
```
Input: nums = [1]
Output: 0

Visual Representation:
Index:  0
Array: [1]
        ↑
      Peak

Single element is always a peak
```

**Example 4:**
```
Input: nums = [1, 2, 3, 4, 5]
Output: 4

Visual Representation:
Index:  0  1  2  3  4
Array: [1, 2, 3, 4, 5]
                    ↑
                  Peak

Strictly increasing array
Peak at the last element
nums[4] = 5 > 4 and 5 > -∞ (right boundary)
```

**Example 5:**
```
Input: nums = [5, 4, 3, 2, 1]
Output: 0

Visual Representation:
Index:  0  1  2  3  4
Array: [5, 4, 3, 2, 1]
        ↑
      Peak

Strictly decreasing array
Peak at the first element
nums[0] = 5 > -∞ (left boundary) and 5 > 4
```

**Example 6:**
```
Input: nums = [1, 3, 2, 4, 1]
Output: 1 (or 3)

Visual Representation:
Index:  0  1  2  3  4
Array: [1, 3, 2, 4, 1]
           ↑     ↑
        Peak   Peak

Binary Search Process:
Step 1: mid = 2, nums[2] = 2
        nums[1] = 3 > nums[2] → go left
        
Step 2: mid = 1, nums[1] = 3
        nums[0] = 1 < nums[1] and nums[2] = 2 < nums[1]
        Found peak at index 1!
```

### ⚙️ Constraints
- `1 <= nums.length <= 1000`
- `-2³¹ <= nums[i] <= 2³¹ - 1`
- `nums[i] != nums[i + 1]` for all valid `i`

---

## 2. Median of 2 Sorted Arrays

### 📝 Problem Description
Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: nums1 = [1, 3], nums2 = [2]
Output: 2.00000

Visual Representation:
nums1: [1, 3]
nums2: [2]

Merged array: [1, 2, 3]
                  ↑
              Median = 2

Explanation:
Total elements = 3 (odd)
Median = middle element = 2
```

**Example 2:**
```
Input: nums1 = [1, 2], nums2 = [3, 4]
Output: 2.50000

Visual Representation:
nums1: [1, 2]
nums2: [3, 4]

Merged array: [1, 2, 3, 4]
                  ↑  ↑
              Median = (2 + 3) / 2 = 2.5

Explanation:
Total elements = 4 (even)
Median = average of two middle elements = (2 + 3) / 2 = 2.5
```

**Example 3:**
```
Input: nums1 = [0, 0], nums2 = [0, 0]
Output: 0.00000

Visual Representation:
nums1: [0, 0]
nums2: [0, 0]

Merged array: [0, 0, 0, 0]
                  ↑  ↑
              Median = (0 + 0) / 2 = 0
```

**Example 4:**
```
Input: nums1 = [], nums2 = [1]
Output: 1.00000

Visual Representation:
nums1: []
nums2: [1]

Merged array: [1]
               ↑
           Median = 1
```

**Example 5:**
```
Input: nums1 = [2], nums2 = []
Output: 2.00000

Visual Representation:
nums1: [2]
nums2: []

Merged array: [2]
               ↑
           Median = 2
```

**Example 6:**
```
Input: nums1 = [1, 3, 8, 9, 15], nums2 = [7, 11, 18, 19, 21, 25]
Output: 11.00000

Visual Representation:
nums1: [1, 3, 8, 9, 15]
nums2: [7, 11, 18, 19, 21, 25]

Merged: [1, 3, 7, 8, 9, 11, 15, 18, 19, 21, 25]
                          ↑
                      Median = 11

Total elements = 11 (odd)
Median position = (11 + 1) / 2 = 6th element = 11
```

**Example 7:**
```
Input: nums1 = [1, 2, 3, 4], nums2 = [5, 6, 7, 8]
Output: 4.50000

Visual Representation:
nums1: [1, 2, 3, 4]
nums2: [5, 6, 7, 8]

Merged: [1, 2, 3, 4, 5, 6, 7, 8]
                    ↑  ↑
            Median = (4 + 5) / 2 = 4.5

Partition concept for O(log(m+n)) solution:
nums1: [1, 2, | 3, 4]
nums2: [5, 6, | 7, 8]

Left half: [1, 2, 5, 6]  (4 elements)
Right half: [3, 4, 7, 8] (4 elements)

Max of left = 6, Min of right = 3
But 6 > 3, so this partition is wrong

Correct partition:
nums1: [1, 2, 3, 4 |]
nums2: [| 5, 6, 7, 8]

Left half: [1, 2, 3, 4] (max = 4)
Right half: [5, 6, 7, 8] (min = 5)
4 < 5 ✓

Median = (4 + 5) / 2 = 4.5
```

### ⚙️ Constraints
- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-10⁶ <= nums1[i], nums2[i] <= 10⁶`

---

## 3. Kth Element of 2 Sorted Arrays

### 📝 Problem Description
Given two sorted arrays `arr1[]` and `arr2[]` of sizes `n` and `m` respectively, and an integer `k`, find the element that would be at the `k-th` position in the final sorted array formed by merging the two arrays.

**Note:** The arrays are 0-indexed, but `k` is 1-indexed (i.e., the 1st element is at index 0).

### 🧪 Sample Test Cases

**Example 1:**
```
Input: arr1 = [2, 3, 6, 7, 9], arr2 = [1, 4, 8, 10], k = 5
Output: 6

Visual Representation:
arr1: [2, 3, 6, 7, 9]
arr2: [1, 4, 8, 10]

Merged array: [1, 2, 3, 4, 6, 7, 8, 9, 10]
Position:      1  2  3  4  5  6  7  8   9
                          ↑
                    5th element = 6

Step-by-step merge visualization:
Position 1: min(2, 1) = 1 from arr2
Position 2: min(2, 4) = 2 from arr1
Position 3: min(3, 4) = 3 from arr1
Position 4: min(6, 4) = 4 from arr2
Position 5: min(6, 8) = 6 from arr1 ✓
```

**Example 2:**
```
Input: arr1 = [100, 112, 256, 349, 770], arr2 = [72, 86, 113, 119, 265, 445, 892], k = 7
Output: 256

Visual Representation:
arr1: [100, 112, 256, 349, 770]
arr2: [72, 86, 113, 119, 265, 445, 892]

Merged: [72, 86, 100, 112, 113, 119, 256, 265, 349, 445, 770, 892]
Pos:     1   2    3    4    5    6    7    8    9    10   11   12
                                       ↑
                                 7th element = 256
```

**Example 3:**
```
Input: arr1 = [1, 3, 5], arr2 = [2, 4, 6], k = 4
Output: 4

Visual Representation:
arr1: [1, 3, 5]
arr2: [2, 4, 6]

Merged array: [1, 2, 3, 4, 5, 6]
Position:      1  2  3  4  5  6
                        ↑
                  4th element = 4

Merge process:
[1] < 2  → take 1 (pos 1)
1 taken, [3] vs [2] → take 2 (pos 2)
[3] > 2, now [3] vs [4] → take 3 (pos 3)
3 taken, [5] vs [4] → take 4 (pos 4) ✓
```

**Example 4:**
```
Input: arr1 = [1], arr2 = [2, 3, 4], k = 2
Output: 2

Visual Representation:
arr1: [1]
arr2: [2, 3, 4]

Merged array: [1, 2, 3, 4]
Position:      1  2  3  4
                  ↑
            2nd element = 2
```

**Example 5:**
```
Input: arr1 = [2, 3, 45], arr2 = [4, 6, 7, 8], k = 4
Output: 6

Visual Representation:
arr1: [2, 3, 45]
arr2: [4, 6, 7, 8]

Merged: [2, 3, 4, 6, 7, 8, 45]
Pos:     1  2  3  4  5  6   7
                  ↑
            4th element = 6
```

**Example 6:**
```
Input: arr1 = [1, 2, 3, 5, 6], arr2 = [4, 7, 8, 9, 10], k = 6
Output: 7

Visual Representation:
arr1: [1, 2, 3, 5, 6]
arr2: [4, 7, 8, 9, 10]

Merged: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Pos:     1  2  3  4  5  6  7  8  9  10
                        ↑
                  6th element = 7

Binary Search Approach:
We need 6 elements on the left side of partition.

Try: Take 3 from arr1, 3 from arr2
arr1: [1, 2, 3 | 5, 6]
arr2: [4, 7, 8 | 9, 10]

Left side: {1, 2, 3, 4, 7, 8} - Wrong! 7,8 > 5

Try: Take 4 from arr1, 2 from arr2
arr1: [1, 2, 3, 5 | 6]
arr2: [4, 7 | 8, 9, 10]

Left side: {1, 2, 3, 5, 4, 7}
Max from arr1 left = 5
Max from arr2 left = 7
Min from arr1 right = 6
Min from arr2 right = 8

Check: 5 < 8 ✓ and 7 < 6 ✗

Try: Take 5 from arr1, 1 from arr2
arr1: [1, 2, 3, 5, 6 |]
arr2: [4 | 7, 8, 9, 10]

Left side: {1, 2, 3, 5, 6, 4} (6 elements)
Max from arr1 left = 6
Max from arr2 left = 4
Min from arr2 right = 7

6th element = max(6, 4) = 6
But we need to check next element = 7

Answer: 7 (first element in right partition)
```

**Example 7:**
```
Input: arr1 = [], arr2 = [1, 2, 3], k = 2
Output: 2

Visual Representation:
arr1: []
arr2: [1, 2, 3]

When one array is empty:
Simply return arr2[k-1] = arr2[1] = 2
```

### ⚙️ Constraints
- `0 <= n, m <= 10⁶`
- `1 <= arr1[i], arr2[i] <= 10⁸`
- `1 <= k <= n + m`
- Both arrays are sorted in non-decreasing order

---

# Binary Search - 2D Matrix Problems

## Table of Contents
1. [Find Row with Maximum 1's](#1-find-row-with-maximum-1s)
2. [Search in a 2D Matrix](#2-search-in-a-2d-matrix)
3. [Search in 2D Matrix - II](#3-search-in-2d-matrix---ii)
4. [Find Peak Element - II](#4-find-peak-element---ii)
5. [Matrix Median](#5-matrix-median)

---

## 1. Find Row with Maximum 1's

### 📝 Problem Description
Given a boolean 2D array where each row is sorted. Find the row with the maximum number of 1's.

**Note:** The matrix contains only 0's and 1's. Also, if two or more rows have the same number of 1's, return the row with the smaller index.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: matrix = [[0, 1, 1, 1],
                 [0, 0, 1, 1],
                 [1, 1, 1, 1],
                 [0, 0, 0, 0]]
Output: 2

Visual Representation:
Row 0: [0, 1, 1, 1]  → 3 ones
Row 1: [0, 0, 1, 1]  → 2 ones
Row 2: [1, 1, 1, 1]  → 4 ones ✓ (Maximum)
Row 3: [0, 0, 0, 0]  → 0 ones

Count visualization:
┌─────┬───────────────┬───────┐
│ Row │    Array      │ Count │
├─────┼───────────────┼───────┤
│  0  │ [0, 1, 1, 1]  │   3   │
│  1  │ [0, 0, 1, 1]  │   2   │
│  2  │ [1, 1, 1, 1]  │   4   │ ← Max
│  3  │ [0, 0, 0, 0]  │   0   │
└─────┴───────────────┴───────┘

Row 2 has maximum 1's = 4
```

**Example 2:**
```
Input: matrix = [[0, 0],
                 [1, 1]]
Output: 1

Visual Representation:
Row 0: [0, 0]  → 0 ones
Row 1: [1, 1]  → 2 ones ✓ (Maximum)

┌─────┬────────┬───────┐
│ Row │ Array  │ Count │
├─────┼────────┼───────┤
│  0  │ [0, 0] │   0   │
│  1  │ [1, 1] │   2   │ ← Max
└─────┴────────┴───────┘
```

**Example 3:**
```
Input: matrix = [[0, 0, 0, 1],
                 [0, 1, 1, 1],
                 [0, 0, 1, 1]]
Output: 1

Visual Representation:
Row 0: [0, 0, 0, 1]  → 1 one
Row 1: [0, 1, 1, 1]  → 3 ones ✓ (Maximum)
Row 2: [0, 0, 1, 1]  → 2 ones

Binary Search Approach:
Each row is sorted, so find first occurrence of 1

Row 0: [0, 0, 0, 1]
              ↑ first 1 at index 3 → count = 4-3 = 1

Row 1: [0, 1, 1, 1]
          ↑ first 1 at index 1 → count = 4-1 = 3

Row 2: [0, 0, 1, 1]
             ↑ first 1 at index 2 → count = 4-2 = 2
```

**Example 4:**
```
Input: matrix = [[1, 1, 1],
                 [1, 1, 1],
                 [1, 1, 1]]
Output: 0

Visual Representation:
Row 0: [1, 1, 1]  → 3 ones ✓ (First row with max)
Row 1: [1, 1, 1]  → 3 ones
Row 2: [1, 1, 1]  → 3 ones

All rows have same number of 1's
Return smallest index = 0
```

**Example 5:**
```
Input: matrix = [[0, 0, 0],
                 [0, 0, 0],
                 [0, 0, 0]]
Output: 0

Visual Representation:
All rows have 0 ones
Return first row index = 0
```

### ⚙️ Constraints
- `1 <= matrix.length, matrix[i].length <= 1000`
- `matrix[i][j]` is either `0` or `1`
- Each row is sorted in non-decreasing order

---

## 2. Search in a 2D Matrix

### 📝 Problem Description
You are given an `m x n` integer matrix `matrix` with the following two properties:
- Each row is sorted in non-decreasing order
- The first integer of each row is greater than the last integer of the previous row

Given an integer `target`, return `true` if `target` is in `matrix` or `false` otherwise.

You must write a solution in `O(log(m * n))` time complexity.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: matrix = [[1, 3, 5, 7],
                 [10, 11, 16, 20],
                 [23, 30, 34, 60]], 
       target = 3
Output: true

Visual Representation:
Matrix:
┌────┬────┬────┬────┐
│  1 │  3 │  5 │  7 │
├────┼────┼────┼────┤
│ 10 │ 11 │ 16 │ 20 │
├────┼────┼────┼────┤
│ 23 │ 30 │ 34 │ 60 │
└────┴────┴────┴────┘
        ↑
    target = 3 found at position [0, 1]

Treated as 1D array:
[1, 3, 5, 7, 10, 11, 16, 20, 23, 30, 34, 60]
    ↑
Index 1 in 1D = Row 0, Col 1 in 2D
```

**Example 2:**
```
Input: matrix = [[1, 3, 5, 7],
                 [10, 11, 16, 20],
                 [23, 30, 34, 60]], 
       target = 13
Output: false

Visual Representation:
Matrix:
┌────┬────┬────┬────┐
│  1 │  3 │  5 │  7 │
├────┼────┼────┼────┤
│ 10 │ 11 │ 16 │ 20 │
├────┼────┼────┼────┤
│ 23 │ 30 │ 34 │ 60 │
└────┴────┴────┴────┘

13 is not present in the matrix

Binary Search:
1D view: [1, 3, 5, 7, 10, 11, 16, 20, 23, 30, 34, 60]
                           ↑         ↑
                          11 < 13 < 16
13 would be between indices 5 and 6, but it doesn't exist
```

**Example 3:**
```
Input: matrix = [[1]], target = 1
Output: true

Visual Representation:
Matrix:
┌───┐
│ 1 │
└───┘
  ↑
Single element matrix, target found
```

**Example 4:**
```
Input: matrix = [[1, 3, 5, 7],
                 [10, 11, 16, 20],
                 [23, 30, 34, 60]], 
       target = 11
Output: true

Visual Representation:
Matrix:
┌────┬────┬────┬────┐
│  1 │  3 │  5 │  7 │
├────┼────┼────┼────┤
│ 10 │ 11 │ 16 │ 20 │
├────┼────┼────┼────┤
│ 23 │ 30 │ 34 │ 60 │
└────┴────┴────┴────┘
       ↑
target = 11 at position [1, 1]

Binary Search Process:
Step 1: mid = 5 (in 1D) → matrix[1][1] = 11
        11 == 11 → Found! ✓

Converting 1D index to 2D:
1D index = 5, columns = 4
Row = 5 / 4 = 1
Col = 5 % 4 = 1
Position = [1, 1]
```

**Example 5:**
```
Input: matrix = [[1, 1]], target = 2
Output: false

Visual Representation:
Matrix:
┌───┬───┐
│ 1 │ 1 │
└───┴───┘

Target 2 is greater than all elements
Binary search will not find it
```

### ⚙️ Constraints
- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 100`
- `-10⁴ <= matrix[i][j], target <= 10⁴`

---

## 3. Search in 2D Matrix - II

### 📝 Problem Description
Write an efficient algorithm that searches for a value `target` in an `m x n` integer matrix `matrix`. This matrix has the following properties:
- Integers in each row are sorted in ascending from left to right
- Integers in each column are sorted in ascending from top to bottom

### 🧪 Sample Test Cases

**Example 1:**
```
Input: matrix = [[1, 4, 7, 11, 15],
                 [2, 5, 8, 12, 19],
                 [3, 6, 9, 16, 22],
                 [10, 13, 14, 17, 24],
                 [18, 21, 23, 26, 30]], 
       target = 5
Output: true

Visual Representation:
Matrix:
┌────┬────┬────┬────┬────┐
│  1 │  4 │  7 │ 11 │ 15 │
├────┼────┼────┼────┼────┤
│  2 │  5 │  8 │ 12 │ 19 │ ← Row sorted
├────┼────┼────┼────┼────┤
│  3 │  6 │  9 │ 16 │ 22 │
├────┼────┼────┼────┼────┤
│ 10 │ 13 │ 14 │ 17 │ 24 │
├────┼────┼────┼────┼────┤
│ 18 │ 21 │ 23 │ 26 │ 30 │
└────┴────┴────┴────┴────┘
       ↑
   Column sorted

Target 5 found at position [1, 1]

Staircase Search (starting from top-right):
Start at [0, 4] = 15
15 > 5 → move left
[0, 3] = 11 > 5 → move left
[0, 2] = 7 > 5 → move left
[0, 1] = 4 < 5 → move down
[1, 1] = 5 == 5 → Found! ✓
```

**Example 2:**
```
Input: matrix = [[1, 4, 7, 11, 15],
                 [2, 5, 8, 12, 19],
                 [3, 6, 9, 16, 22],
                 [10, 13, 14, 17, 24],
                 [18, 21, 23, 26, 30]], 
       target = 20
Output: false

Visual Representation:
Staircase Search Path:
Start: [0, 4] = 15
       ↓
[0, 4] = 15 < 20 → move down
[1, 4] = 19 < 20 → move down
[2, 4] = 22 > 20 → move left
[2, 3] = 16 < 20 → move down
[3, 3] = 17 < 20 → move down
[4, 3] = 26 > 20 → move left
[4, 2] = 23 > 20 → move left
[4, 1] = 21 > 20 → move left
[4, 0] = 18 < 20 → move down (out of bounds)

Not found!
```

**Example 3:**
```
Input: matrix = [[1, 4],
                 [2, 5]], 
       target = 2
Output: true

Visual Representation:
Matrix:
┌───┬───┐
│ 1 │ 4 │
├───┼───┤
│ 2 │ 5 │
└───┴───┘
  ↑
Target 2 at position [1, 0]

Search Path from top-right:
[0, 1] = 4 > 2 → move left
[0, 0] = 1 < 2 → move down
[1, 0] = 2 == 2 → Found! ✓
```

**Example 4:**
```
Input: matrix = [[5]], target = 5
Output: true

Visual Representation:
Matrix:
┌───┐
│ 5 │
└───┘
  ↑
Single element equals target
```

**Example 5:**
```
Input: matrix = [[-5]], target = -10
Output: false

Visual Representation:
Single element -5 ≠ -10
Target not found
```

**Example 6:**
```
Input: matrix = [[1, 2, 3, 4, 5],
                 [6, 7, 8, 9, 10],
                 [11, 12, 13, 14, 15],
                 [16, 17, 18, 19, 20],
                 [21, 22, 23, 24, 25]], 
       target = 19
Output: true

Visual Representation:
Target 19 at position [3, 3]

Efficient Search Path (from top-right):
┌────┬────┬────┬────┬────┐
│  1 │  2 │  3 │  4 │  5 │ ← Start
├────┼────┼────┼────┼────┤
│  6 │  7 │  8 │  9 │ 10 │
├────┼────┼────┼────┼────┤
│ 11 │ 12 │ 13 │ 14 │ 15 │
├────┼────┼────┼────┼────┤
│ 16 │ 17 │ 18 │ 19 │ 20 │
├────┼────┼────┼────┼────┤
│ 21 │ 22 │ 23 │ 24 │ 25 │
└────┴────┴────┴────┴────┘
                    ↑
                 Found!

Path: [0,4]→[1,4]→[2,4]→[3,4]→[3,3] (5 steps)
```

### ⚙️ Constraints
- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= n, m <= 300`
- `-10⁹ <= matrix[i][j] <= 10⁹`
- All the integers in each row are sorted in ascending order
- All the integers in each column are sorted in ascending order
- `-10⁹ <= target <= 10⁹`

---

## 4. Find Peak Element - II

### 📝 Problem Description
A peak element in a 2D grid is an element that is **strictly greater** than all of its **adjacent neighbors** to the left, right, top, and bottom.

Given a **0-indexed** `m x n` matrix `mat` where **no two adjacent cells are equal**, find **any** peak element `mat[i][j]` and return the length 2 array `[i, j]`.

You may assume that the entire matrix is surrounded by an **outer perimeter** with the value `-1` in each cell.

You must write an algorithm that runs in `O(m log(n))` or `O(n log(m))` time.

### 🧪 Sample Test Cases

**Example 1:**
```
Input: mat = [[1, 4],
              [3, 2]]
Output: [0, 1]

Visual Representation:
Matrix:
┌───┬───┐
│ 1 │ 4 │ ← Peak at [0, 1]
├───┼───┤
│ 3 │ 2 │
└───┴───┘
      ↑

Element at [0, 1] = 4
- 4 > 1 (left neighbor)
- 4 > -1 (right boundary)
- 4 > -1 (top boundary)
- 4 > 2 (bottom neighbor)

4 is strictly greater than all neighbors → Peak!

Note: [1, 0] with value 3 is also a valid peak
```

**Example 2:**
```
Input: mat = [[10, 20, 15],
              [21, 30, 14],
              [7, 16, 32]]
Output: [1, 1]

Visual Representation:
Matrix:
┌────┬────┬────┐
│ 10 │ 20 │ 15 │
├────┼────┼────┤
│ 21 │ 30 │ 14 │ ← Peak at [1, 1]
├────┼────┼────┤
│  7 │ 16 │ 32 │
└────┴────┴────┘
       ↑

Element at [1, 1] = 30
- 30 > 21 (left)
- 30 > 14 (right)
- 30 > 20 (top)
- 30 > 16 (bottom)

30 is the maximum element and a peak

Note: [2, 2] with value 32 is also a valid peak
```

**Example 3:**
```
Input: mat = [[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]]
Output: [2, 2]

Visual Representation:
Matrix:
┌───┬───┬───┐
│ 1 │ 2 │ 3 │
├───┼───┼───┤
│ 4 │ 5 │ 6 │
├───┼───┼───┤
│ 7 │ 8 │ 9 │ ← Peak at [2, 2]
└───┴───┴───┘
          ↑

Element at [2, 2] = 9
- 9 > 8 (left)
- 9 > -1 (right boundary)
- 9 > 6 (top)
- 9 > -1 (bottom boundary)

Corner element 9 is the peak
```

**Example 4:**
```
Input: mat = [[10, 50, 40, 30, 20],
              [15, 45, 35, 25, 10],
              [12, 40, 22, 24, 8]]
Output: [0, 1]

Visual Representation:
Matrix:
┌────┬────┬────┬────┬────┐
│ 10 │ 50 │ 40 │ 30 │ 20 │
├────┼────┼────┼────┼────┤
│ 15 │ 45 │ 35 │ 25 │ 10 │
├────┼────┼────┼────┼────┤
│ 12 │ 40 │ 22 │ 24 │  8 │
└────┴────┴────┴────┴────┘
       ↑

Element at [0, 1] = 50
Checking neighbors:
- 50 > 10 (left)
- 50 > 40 (right)
- 50 > -1 (top boundary)
- 50 > 45 (bottom)

Binary Search Approach (O(m log n)):
1. Find middle column (col = 2)
2. Find max in that column → 40 at [0, 2]
3. Check neighbors of 40:
   - 40 < 50 (left is greater)
4. Move to left half and repeat
5. Eventually find peak at [0, 1] = 50
```

**Example 5:**
```
Input: mat = [[1, 3],
              [2, 4]]
Output: [1, 1]

Visual Representation:
Matrix:
┌───┬───┐
│ 1 │ 3 │
├───┼───┤
│ 2 │ 4 │ ← Peak
└───┴───┘
      ↑

Element at [1, 1] = 4
- 4 > 2 (left)
- 4 > -1 (right boundary)
- 4 > 3 (top)
- 4 > -1 (bottom boundary)

4 is greater than all neighbors
```

### ⚙️ Constraints
- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 500`
- `1 <= mat[i][j] <= 10⁵`
- No two adjacent cells are equal

---

## 5. Matrix Median

### 📝 Problem Description
Given a matrix of integers `A` of size `N x M` in which each row is sorted.

Find and return the overall median of matrix `A`.

**Note:** 
- No extra memory is allowed
- `N*M` is always odd

### 🧪 Sample Test Cases

**Example 1:**
```
Input: A = [[1, 3, 5],
            [2, 6, 9],
            [3, 6, 9]]
Output: 5

Visual Representation:
Matrix:
┌───┬───┬───┐
│ 1 │ 3 │ 5 │
├───┼───┼───┤
│ 2 │ 6 │ 9 │
├───┼───┼───┤
│ 3 │ 6 │ 9 │
└───┴───┴───┘

If we merge all elements:
[1, 2, 3, 3, 5, 6, 6, 9, 9]
            ↑
        Median = 5 (middle element)

Total elements = 9 (odd)
Median position = (9 + 1) / 2 = 5th element

Counting elements ≤ 5:
Row 0: [1, 3, 5] → 3 elements ≤ 5
Row 1: [2, 6, 9] → 1 element ≤ 5
Row 2: [3, 6, 9] → 1 element ≤ 5
Total: 5 elements ≤ 5

Since exactly 5 elements ≤ 5, median = 5
```

**Example 2:**
```
Input: A = [[1, 3, 4],
            [2, 5, 6],
            [7, 8, 9]]
Output: 5

Visual Representation:
Matrix:
┌───┬───┬───┐
│ 1 │ 3 │ 4 │
├───┼───┼───┤
│ 2 │ 5 │ 6 │
├───┼───┼───┤
│ 7 │ 8 │ 9 │
└───┴───┴───┘

Merged: [1, 2, 3, 4, 5, 6, 7, 8, 9]
                    ↑
                Median = 5

Total = 9, median at position 5

Binary Search Approach:
Range: min=1, max=9

Test mid = 5:
Count elements ≤ 5:
Row 0: [1, 3, 4] → 3 elements
Row 1: [2, 5, 6] → 2 elements
Row 2: [7, 8, 9] → 0 elements
Total: 5 elements ≤ 5

Required position = (9+1)/2 = 5
Count = 5 → median = 5 ✓
```

**Example 3:**
```
Input: A = [[1, 1, 3, 3, 3, 3, 5, 5]]
Output: 3

Visual Representation:
Matrix (single row):
[1, 1, 3, 3, 3, 3, 5, 5]
            ↑
        Median

Total = 8... wait, problem says N*M is always odd!

Corrected Input: A = [[1, 1, 3, 3, 3, 3, 5]]
Output: 3

Matrix:
[1, 1, 3, 3, 3, 3, 5]
            ↑
        Median = 3

Total = 7 (odd)
Median position = 4th element = 3
```

**Example 4:**
```
Input: A = [[5],
            [4],
            [3],
            [2],
            [1]]
Output: 3

Visual Representation:
Matrix:
┌───┐
│ 5 │
├───┤
│ 4 │
├───┤
│ 3 │ ← Median
├───┤
│ 2 │
├───┤
│ 1 │
└───┘

Sorted sequence: [1, 2, 3, 4, 5]
                        ↑
                    Median = 3

Total = 5, median at position 3
```

**Example 5:**
```
Input: A = [[1, 2, 3],
            [4, 5, 6],
            [7, 8, 9]]
Output: 5

Visual Representation:
Matrix:
┌───┬───┬───┐
│ 1 │ 2 │ 3 │
├───┼───┼───┤
│ 4 │ 5 │ 6 │
├───┼───┼───┤
│ 7 │ 8 │ 9 │
└───┴───┴───┘

All elements: [1, 2, 3, 4, 5, 6, 7, 8, 9]
                          ↑
                      Median = 5

Binary Search Process:
min = 1, max = 9

Test mid = 5:
Elements ≤ 5 in each row:
Row 0: [1, 2, 3] → all 3 ≤ 5
Row 1: [4, 5, 6] → 2 ≤ 5 (4, 5)
Row 2: [7, 8, 9] → 0 ≤ 5
Total: 5 elements ≤ 5

Median position = (9+1)/2 = 5
Since count = 5, median = 5 ✓
```

**Example 6:**
```
Input: A = [[1, 10, 20],
            [15, 25, 35],
            [5, 30, 40]]
Output: 20

Visual Representation:
Matrix (rows are sorted, not the whole matrix):
┌────┬────┬────┐
│  1 │ 10 │ 20 │
├────┼────┼────┤
│ 15 │ 25 │ 35 │
├────┼────┼────┤
│  5 │ 30 │ 40 │
└────┴────┴────┘

Merged and sorted: [1, 5, 10, 15, 20, 25, 30, 35, 40]
                                   ↑
                               Median = 20

Binary Search:
min = 1, max = 40

Test mid = 20:
Count elements ≤ 20:
Row 0: [1, 10, 20] → 3 elements
Row 1: [15, 25, 35] → 1 element (15)
Row 2: [5, 30, 40] → 1 element (5)
Total: 5 elements ≤ 20

Median position = 5, count = 5 → median = 20 ✓
```

### ⚙️ Constraints
- `1 <= N, M <= 400`
- `1 <= A[i][j] <= 10⁹`
- `N * M` is always odd
- Each row is sorted in non-decreasing order

---
