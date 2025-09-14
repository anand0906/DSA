# Array Problems Collection 📚

A comprehensive collection of common array manipulation problems with multiple solution approaches, detailed explanations, and complexity analysis.

## Table of Contents
1. [Move All Zeros to End](#1-move-all-zeros-to-end)
2. [Remove Duplicates from Sorted Array](#2-remove-duplicates-from-sorted-array)
3. [Sort Array of 0s, 1s, and 2s](#3-sort-array-of-0s-1s-and-2s)
4. [Two Sum Problem](#4-two-sum-problem)
5. [Three Sum Problem](#5-three-sum-problem)
6. [Four Sum Problem](#6-four-sum-problem)
7. [Container With Most Water](#7-container-with-most-water)
8. [Trapping Rain Water](#8-trapping-rain-water)

---

## 1. Move All Zeros to End

**Problem Statement:**
Given an array of non-negative integers, move all zeros to the end while maintaining the relative order of non-zero elements.

**Example:**
```
Input:  [1, 2, 0, 0, 2, 3]
Output: [1, 2, 2, 3, 0, 0]
```

### Solutions:

#### Brute Force Approach
**Approach:** Create a new array with non-zero elements, then fill with zeros  
**Time Complexity:** O(n)  
**Space Complexity:** O(n) - for the nonZeros array

```python
def bruteforce(n, arr):
    nonZeros = []
    
    # Collect all non-zero elements
    for i in arr:
        if i != 0:
            nonZeros.append(i)
    
    # Place non-zero elements at the beginning
    for i in range(len(nonZeros)):
        arr[i] = nonZeros[i]
    
    # Fill remaining positions with zeros
    for i in range(len(nonZeros), n):
        arr[i] = 0
    
    return arr
```

#### Optimized Two-Pointer Approach
**Approach:** Use two pointers to swap non-zero elements with zeros  
**Time Complexity:** O(n)  
**Space Complexity:** O(1) - constant extra space

```python
def optimized(n, arr):
    left = -1
    
    # Find the first zero element
    for i in range(n):
        if arr[i] == 0:
            left = i
            break
    
    # If no zeros found, array is already optimal
    if left == -1:
        return arr
    
    # Move non-zero elements to the left
    for right in range(left + 1, n):
        if arr[right] != 0:
            arr[right], arr[left] = arr[left], arr[right]
            left += 1
    
    return arr
```

---

## 2. Remove Duplicates from Sorted Array

**Problem Statement:**
Remove duplicates from a sorted array in-place, keeping only unique elements in their original order.

**Example:**
```
Input:  [1, 1, 2, 2, 2, 3, 3]
Output: [1, 2, 3, _, _, _, _] (returns count = 3)
```

### Solutions:

#### Brute Force Approach
**Approach:** Use additional array to store unique elements  
**Time Complexity:** O(n)  
**Space Complexity:** O(n) - for the unique array

```python
def bruteforce(n, arr):
    unique = []
    count = 0
    
    # Collect unique elements
    for i in arr:
        if i not in unique:
            unique.append(i)
            count += 1
    
    # Place unique elements back in original array
    for i in range(len(unique)):
        arr[i] = unique[i]
    
    return count
```

#### Optimized Two-Pointer Approach
**Approach:** Use two pointers to maintain unique elements  
**Time Complexity:** O(n)  
**Space Complexity:** O(1) - constant extra space

```python
def optimized(n, arr):
    left = 0
    count = 1
    
    # Compare adjacent elements and move unique ones
    for right in range(1, n):
        if arr[left] != arr[right]:
            left += 1
            arr[left], arr[right] = arr[right], arr[left]
            count += 1
    
    return count
```

---

## 3. Sort Array of 0s, 1s, and 2s

**Problem Statement:**
Sort an array containing only 0s, 1s, and 2s in a single pass with constant space (Dutch National Flag Algorithm).

**Example:**
```
Input:  [2, 0, 2, 1, 1, 0]
Output: [0, 0, 1, 1, 2, 2]
```

### Solutions:

#### Brute Force Approach
**Approach:** Use built-in sort function  
**Time Complexity:** O(n log n)  
**Space Complexity:** O(1) - if sort is in-place

```python
def bruteforce(n, arr):
    arr.sort()
    return arr
```

#### Better Counting Approach
**Approach:** Count occurrences and reconstruct array  
**Time Complexity:** O(2n) = O(n) - two passes  
**Space Complexity:** O(1) - constant extra space

```python
def better(n, arr):
    zeros, ones, twos = 0, 0, 0
    
    # Count occurrences of each element
    for i in arr:
        if i == 0:
            zeros += 1
        elif i == 1:
            ones += 1
        else:
            twos += 1
    
    # Reconstruct the array
    for i in range(zeros):
        arr[i] = 0
    for i in range(zeros, zeros + ones):
        arr[i] = 1
    for i in range(zeros + ones, n):
        arr[i] = 2
    
    return arr
```

#### Optimized Dutch National Flag Algorithm
**Approach:** Three-pointer technique (Dutch National Flag)  
**Time Complexity:** O(n) - single pass  
**Space Complexity:** O(1) - constant extra space

```python
def optimized(n, arr):
    left, mid, right = 0, 0, n - 1
    
    while mid <= right:
        if arr[mid] == 0:
            # Swap with left and move both pointers
            arr[left], arr[mid] = arr[mid], arr[left]
            left += 1
            mid += 1
        elif arr[mid] == 1:
            # Element is in correct position
            mid += 1
        else:  # arr[mid] == 2
            # Swap with right and move right pointer
            arr[right], arr[mid] = arr[mid], arr[right]
            right -= 1
            # Don't increment mid as we need to check swapped element
    
    return arr
```

---

## 4. Two Sum Problem

**Problem Statement:**
Find two numbers in an array that add up to a given target sum. Return their indices.

**Example:**
```
Input:  arr = [2, 7, 11, 15], target = 9
Output: [0, 1] (arr[0] + arr[1] = 2 + 7 = 9)
```

### Solutions:

#### Brute Force Approach
**Approach:** Check all possible pairs  
**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

```python
def bruteforce(n, arr, target):
    for i in range(n):
        for j in range(i + 1, n):
            if arr[i] + arr[j] == target:
                return [i, j]
    return [-1, -1]
```

#### Better Hash Map Approach
**Approach:** Use hash map to store complements  
**Time Complexity:** O(n)  
**Space Complexity:** O(n) - for the hash map

```python
def better(n, arr, target):
    map = {}
    
    for i in range(n):
        rem = target - arr[i]
        if rem in map:
            return [i, map[rem]]
        map[arr[i]] = i
    
    return [-1, -1]
```

#### Optimized Two-Pointer Approach
**Approach:** Sort array and use two pointers  
**Time Complexity:** O(n log n) - due to sorting  
**Space Complexity:** O(1)  
**Note:** This modifies original array and loses original indices

```python
def optimized(n, arr, target):
    arr.sort()
    left, right = 0, n - 1
    
    while left < right:
        sum = arr[left] + arr[right]
        if sum == target:
            return [left, right]
        elif sum < target:
            left += 1
        else:
            right -= 1
    
    return [-1, -1]
```

---

## 5. Three Sum Problem

**Problem Statement:**
Find all unique triplets in the array that sum to zero.

**Example:**
```
Input:  [-1, 0, 1, 2, -1, -4]
Output: [[-1, -1, 2], [-1, 0, 1]]
```

### Solutions:

#### Brute Force Approach
**Approach:** Check all possible triplets  
**Time Complexity:** O(n³)  
**Space Complexity:** O(k) - where k is number of unique triplets

```python
def bruteforce(n, arr, target):
    ans = set()
    
    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                if arr[i] + arr[j] + arr[k] == target:
                    temp = sorted([arr[i], arr[j], arr[k]])
                    ans.add(tuple(temp))
    
    return list(ans)
```

#### Better Hash Map Approach
**Approach:** Fix one element, use hash map for remaining two  
**Time Complexity:** O(n²)  
**Space Complexity:** O(n) + O(k) - for hash map and result

```python
def better(n, arr, target):
    ans = set()
    
    for i in range(n):
        map = {}
        for j in range(i + 1, n):
            third = target - (arr[i] + arr[j])
            if third in map:
                temp = sorted([arr[i], arr[j], arr[map[third]]])
                ans.add(tuple(temp))
            map[arr[j]] = j
    
    return list(ans)
```

#### Optimized Two-Pointer Approach
**Approach:** Sort array, fix one element, use two pointers for rest  
**Time Complexity:** O(n log n) + O(n²) = O(n²)  
**Space Complexity:** O(k) - for storing result

```python
def optimized(n, arr, target):
    ans = set()
    arr.sort()
    
    for i in range(n):
        # Skip duplicates for first element
        if i != 0 and arr[i] == arr[i - 1]:
            continue
        
        left, right = i + 1, n - 1
        
        while left < right:
            sum = arr[i] + arr[left] + arr[right]
            
            if sum == target:
                temp = sorted([arr[left], arr[right], arr[i]])
                ans.add(tuple(temp))
                left += 1
                right -= 1
                
                # Skip duplicates
                while left < right and arr[left] == arr[left - 1]:
                    left += 1
                while right > left and arr[right] == arr[right + 1]:
                    right -= 1
            elif sum < target:
                left += 1
            else:
                right -= 1
    
    return list(ans)
```

---

## 6. Four Sum Problem

**Problem Statement:**
Find all unique quadruplets in the array that sum to the target value.

**Example:**
```
Input:  [1, 0, -1, 0, -2, 2], target = 0
Output: [[-2, -1, 1, 2], [-2, 0, 0, 2], [-1, 0, 0, 1]]
```

### Solutions:

#### Brute Force Approach
**Approach:** Check all possible quadruplets  
**Time Complexity:** O(n⁴)  
**Space Complexity:** O(k) - where k is number of unique quadruplets

```python
def bruteforce(n, arr, target):
    ans = set()
    
    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                for m in range(k + 1, n):
                    if arr[i] + arr[j] + arr[k] + arr[m] == target:
                        temp = sorted([arr[i], arr[j], arr[k], arr[m]])
                        ans.add(tuple(temp))
    
    return list(ans)
```

#### Better Hash Map Approach
**Approach:** Fix two elements, use hash map for remaining two  
**Time Complexity:** O(n³)  
**Space Complexity:** O(n) + O(k) - for hash map and result

```python
def better(n, arr, target):
    ans = set()
    
    for i in range(n):
        for j in range(i + 1, n):
            map = {}
            for k in range(j + 1, n):
                third = target - (arr[i] + arr[j] + arr[k])
                if third in map:
                    temp = sorted([arr[i], arr[j], arr[k], arr[map[third]]])
                    ans.add(tuple(temp))
                map[arr[k]] = k
    
    return list(ans)
```

#### Optimized Two-Pointer Approach
**Approach:** Sort array, fix two elements, use two pointers for rest  
**Time Complexity:** O(n log n) + O(n³) = O(n³)  
**Space Complexity:** O(k) - for storing result

```python
def optimized(n, arr, target):
    ans = set()
    arr.sort()
    
    for i in range(n):
        # Skip duplicates for first element
        if i != 0 and arr[i] == arr[i - 1]:
            continue
            
        for j in range(i + 1, n):
            # Skip duplicates for second element
            if j != i + 1 and arr[j] == arr[j - 1]:
                continue
                
            left, right = j + 1, n - 1
            
            while left < right:
                total = arr[i] + arr[j] + arr[left] + arr[right]
                
                if total == target:
                    temp = sorted([arr[i], arr[j], arr[left], arr[right]])
                    ans.add(tuple(temp))
                    left += 1
                    right -= 1
                    
                    # Skip duplicates
                    while left < right and arr[left] == arr[left - 1]:
                        left += 1
                    while right > left and arr[right] == arr[right + 1]:
                        right -= 1
                elif total < target:
                    left += 1
                else:
                    right -= 1
    
    return list(ans)
```

---

## 7. Container With Most Water

**Problem Statement:**
Given heights of vertical lines, find two lines that form a container holding the most water.

**Example:**
```
Input:  [1, 8, 6, 2, 5, 4, 8, 3, 7]
Output: 49 (lines at positions 1 and 8: width=7, height=min(8,7)=7)
```

### Solutions:

#### Brute Force Approach
**Approach:** Check all possible pairs of lines  
**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

```python
def bruteforce(n, arr):
    largest = 0
    
    for i in range(n):
        for j in range(i + 1, n):
            width = j - i
            height = min(arr[i], arr[j])
            area = width * height
            largest = max(area, largest)
    
    return largest
```

#### Optimized Two-Pointer Approach
**Approach:** Use two pointers from both ends  
**Time Complexity:** O(n)  
**Space Complexity:** O(1)

```python
def optimized(n, arr):
    left, right = 0, n - 1
    largest = 0
    
    while left < right:
        width = right - left
        height = min(arr[left], arr[right])
        area = width * height
        largest = max(largest, area)
        
        # Move the pointer with smaller height
        if arr[left] < arr[right]:
            left += 1
        else:
            right -= 1
    
    return largest
```

---

## 8. Trapping Rain Water

**Problem Statement:**
Calculate how much rainwater can be trapped given an elevation map.

**Example:**
```
Input:  [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
Output: 6 units of trapped water
```

### Solutions:

#### Brute Force Approach
**Approach:** For each position, find left and right maximum heights  
**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

```python
def bruteforce(n, arr):
    total = 0
    
    for i in range(n):
        # Find maximum height to the left
        leftMax = 0
        for j in range(i):
            leftMax = max(leftMax, arr[j])
        
        # Find maximum height to the right
        rightMax = 0
        for j in range(i + 1, n):
            rightMax = max(rightMax, arr[j])
        
        # Calculate trapped water at position i
        water = min(leftMax, rightMax) - arr[i]
        if water > 0:
            total += water
    
    return total
```

#### Better Preprocessing Approach
**Approach:** Precompute right maximums, compute left maximums on the fly  
**Time Complexity:** O(n)  
**Space Complexity:** O(n) - for rightMax array

```python
def better(n, arr):
    total = 0
    rightMax = [0] * n
    
    # Precompute right maximum for each position
    maxi = 0
    for i in range(n - 1, -1, -1):
        rightMax[i] = maxi
        maxi = max(maxi, arr[i])
    
    # Calculate trapped water using precomputed values
    leftMax = 0
    for i in range(n):
        water = min(leftMax, rightMax[i]) - arr[i]
        if water > 0:
            total += water
        leftMax = max(leftMax, arr[i])
    
    return total
```

#### Optimized Two-Pointer Approach
**Approach:** Use two pointers with running maximums  
**Time Complexity:** O(n)  
**Space Complexity:** O(1)

```python
def optimized(n, arr):
    total = 0
    left, right = 0, n - 1
    leftMax, rightMax = 0, 0
    
    while left <= right:
        if arr[left] < arr[right]:
            if arr[left] > leftMax:
                leftMax = arr[left]
            else:
                water = leftMax - arr[left]
                if water > 0:
                    total += water
            left += 1
        else:
            if arr[right] > rightMax:
                rightMax = arr[right]
            else:
                water = rightMax - arr[right]
                if water > 0:
                    total += water
            right -= 1
    
    return total
```
