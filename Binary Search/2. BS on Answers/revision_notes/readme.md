# Binary Search Problems - Revision Notes

## Index

1. [Find Square Root of a Number](#1-find-square-root-of-a-number)
2. [Find Nth Root of a Number](#2-find-nth-root-of-a-number)
3. [Find the Smallest Divisor](#3-find-the-smallest-divisor)
4. [Koko Eating Bananas](#4-koko-eating-bananas)
5. [Minimum Days to Make M Bouquets](#5-minimum-days-to-make-m-bouquets)
6. [Kth Missing Positive Number](#6-kth-missing-positive-number)
7. [Comparison Table](#comparison-table)

---

## 1. Find Square Root of a Number

### Problem Description
Given a positive integer n, find and return its square root. If n is not a perfect square, return the floor value of sqrt(n).

### Sample Test Cases
```
Input: n = 36
Output: 6
Explanation: 6 is the square root of 36.

Input: n = 28
Output: 5
Explanation: The square root of 28 is approximately 5.292. Floor value is 5.
```

### Approach 1: Linear Search

**Intuition:** Check every number from 0 to n and find the largest number whose square is less than or equal to n.

**Code:**
```python
def solve(n):
    ans = 0
    # Check every number from 0 to n
    for num in range(n+1):
        # If square of num is <= n, update answer
        if (num * num) <= n:
            ans = num
        else:
            # Once square exceeds n, return previous answer
            return ans
    return ans
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

### Approach 2: Binary Search (Optimized)

**Intuition:** Since numbers are sorted, use binary search on the range [0, n]. If mid*mid ≤ n, the answer could be mid or something larger, so search right. Otherwise, search left.

**Code:**
```python
def optimized(n):
    sqrt = n
    low, high = 0, sqrt + 1
    ans = 0
    # Binary search in range [0, n]
    while low <= high:
        mid = (low + high) // 2
        # If mid*mid <= n, mid could be answer, search right for larger values
        if (mid * mid) <= sqrt:
            ans = mid
            low = mid + 1
        else:
            # If mid*mid > n, search left for smaller values
            high = mid - 1
    return ans
```

**Time Complexity:** O(log n)  
**Space Complexity:** O(1)

---

## 2. Find Nth Root of a Number

### Problem Description
Given two numbers N and M, find the Nth root of M. Return the number X such that X^N = M. If the Nth root is not an integer, return -1.

### Sample Test Cases
```
Input: N = 3, M = 27
Output: 3
Explanation: The cube root of 27 is equal to 3.

Input: N = 4, M = 69
Output: -1
Explanation: The 4th root of 69 does not exist as an integer.
```

### Approach 1: Linear Search

**Intuition:** Check every number from 0 to M and find if any number raised to power N equals M.

**Code:**
```python
def solve(n, m):
    # Try every number from 0 to m
    for num in range(m+1):
        temp = (num ** n)
        # If num^n equals m, found the answer
        if (temp == m):
            return num
        # If num^n > m, no integer root exists
        elif (temp > m):
            return -1
    return -1
```

**Time Complexity:** O(m)  
**Space Complexity:** O(1)

### Approach 2: Binary Search (Optimized)

**Intuition:** Use binary search on range [0, m]. If mid^n equals m, return mid. If mid^n < m, search right. Otherwise, search left.

**Code:**
```python
def optimized(n, m):
    sqrt = m
    low, high = 0, sqrt + 1
    # Binary search in range [0, m]
    while low <= high:
        mid = (low + high) // 2
        temp = mid ** n
        # If mid^n equals m, found exact answer
        if (temp == sqrt):
            return mid
        # If mid^n < m, search right for larger values
        elif (temp < sqrt):
            low = mid + 1
        # If mid^n > m, search left for smaller values
        else:
            high = mid - 1
    return -1
```

**Time Complexity:** O(log m)  
**Space Complexity:** O(1)

---

## 3. Find the Smallest Divisor

### Problem Description
Given an array of integers nums and an integer limit, find the smallest positive integer divisor such that when dividing all elements by this divisor (taking ceiling), the sum is less than or equal to the limit.

### Sample Test Cases
```
Input: nums = [1, 2, 3, 4, 5], limit = 8
Output: 3
Explanation: Dividing by 3 gives [1,1,1,2,2], sum = 7 ≤ 8.

Input: nums = [8,4,2,3], limit = 10
Output: 2
Explanation: Dividing by 2 gives [4,2,1,2], sum = 9 ≤ 10.
```

### Approach 1: Linear Search

**Intuition:** Try every divisor from 1 to max(nums) and check if the ceiling sum satisfies the limit condition.

**Code:**
```python
import math

def check(n, arr, divisor, limit):
    total = 0
    # Calculate sum of ceiling divisions
    for i in range(n):
        total += math.ceil(arr[i] / divisor)
    # Return True if sum is within limit
    return total <= limit

def solve(n, arr, limit):
    maxi = max(arr)
    # Try every divisor from 1 to max element
    for num in range(1, maxi+1):
        temp = check(n, arr, num, limit)
        # Return first divisor that satisfies condition
        if (temp):
            return num
    return -1
```

**Time Complexity:** O(max(nums) * n)  
**Space Complexity:** O(1)

### Approach 2: Binary Search (Optimized)

**Intuition:** Use binary search on divisor range [1, max(nums)]. If current divisor satisfies the condition, try smaller divisors (search left). Otherwise, search right.

**Code:**
```python
def optimized(n, arr, limit):
    maxi = max(arr)
    low, high = 1, maxi + 1
    ans = -1
    # Binary search on divisor range [1, max]
    while low <= high:
        mid = (low + high) // 2
        temp = check(n, arr, mid, limit)
        # If condition satisfied, try smaller divisor
        if (temp):
            ans = mid
            high = mid - 1
        # Otherwise, try larger divisor
        else:
            low = mid + 1
    return ans
```

**Time Complexity:** O(n * log(max(nums)))  
**Space Complexity:** O(1)

---

## 4. Koko Eating Bananas

### Problem Description
A monkey has n piles of bananas. Each hour, it eats k bananas from one pile. Find the minimum k such that all bananas can be eaten within h hours.

### Sample Test Cases
```
Input: n = 4, nums = [7, 15, 6, 3], h = 8
Output: 5
Explanation: Eating 5 bananas/hr takes 2+3+2+1 = 8 hours.

Input: n = 5, nums = [25, 12, 8, 14, 19], h = 5
Output: 25
Explanation: Eating 25 bananas/hr takes 1+1+1+1+1 = 5 hours.
```

### Approach 1: Linear Search

**Intuition:** Try every eating speed from 1 to max(nums) and check if all bananas can be consumed within h hours.

**Code:**
```python
import math

def check(n, arr, count, hours):
    total = 0
    # Calculate total hours needed with eating speed 'count'
    for i in range(n):
        total += math.ceil(arr[i] / count)
    # Return True if can finish within given hours
    return total <= hours

def solve(n, arr, hours):
    maxi = max(arr)
    # Try every eating speed from 1 to max pile size
    for num in range(1, maxi+1):
        temp = check(n, arr, num, hours)
        # Return first speed that works
        if (temp):
            return num
    return -1
```

**Time Complexity:** O(max(nums) * n)  
**Space Complexity:** O(1)

### Approach 2: Binary Search (Optimized)

**Intuition:** Use binary search on eating speed range [1, max(nums)]. If current speed works, try slower speeds (search left). Otherwise, search right.

**Code:**
```python
def optimized(n, arr, hours):
    maxi = max(arr)
    low, high = 1, maxi + 1
    ans = -1
    # Binary search on eating speed range [1, max]
    while low <= high:
        mid = (low + high) // 2
        temp = check(n, arr, mid, hours)
        # If current speed works, try slower speed
        if (temp):
            ans = mid
            high = mid - 1
        # Otherwise, need faster speed
        else:
            low = mid + 1
    return ans
```

**Time Complexity:** O(n * log(max(nums)))  
**Space Complexity:** O(1)

---

## 5. Minimum Days to Make M Bouquets

### Problem Description
Given n roses with bloom days in nums array, find the minimum days needed to make m bouquets, each requiring k adjacent bloomed roses. Return -1 if impossible.

### Sample Test Cases
```
Input: n = 8, nums = [7, 7, 7, 7, 13, 11, 12, 7], m = 2, k = 3
Output: 12
Explanation: By day 12, first 4 and last 3 roses bloom, making 2 bouquets possible.

Input: n = 5, nums = [1, 10, 3, 10, 2], m = 3, k = 2
Output: -1
Explanation: Need 6 roses for 3 bouquets, but only 5 available.
```

### Approach 1: Linear Search

**Intuition:** Try every day from 1 to max(nums) and count how many bouquets can be made with roses bloomed by that day.

**Code:**
```python
def check(n, arr, days, m, k):
    total = 0  # Total bouquets made
    cnt = 0    # Count of adjacent bloomed roses
    for i in range(n):
        # If rose has bloomed by 'days'
        if (arr[i] <= days):
            cnt += 1
        else:
            # Rose not bloomed, calculate bouquets from current streak
            total += (cnt // k)
            cnt = 0  # Reset streak
    else:
        # Add remaining bouquets at the end
        total += (cnt // k)
    # Check if we can make at least m bouquets
    return total >= m

def solve(n, arr, m, k):
    maxi = max(arr)
    # Try every day from 1 to max bloom day
    for num in range(1, maxi+1):
        temp = check(n, arr, num, m, k)
        # Return first day when m bouquets can be made
        if (temp):
            return num
    return -1
```

**Time Complexity:** O(max(nums) * n)  
**Space Complexity:** O(1)

### Approach 2: Binary Search (Optimized)

**Intuition:** Use binary search on days range [1, max(nums)]. If m bouquets can be made by mid day, try earlier days (search left). Otherwise, search right.

**Code:**
```python
def optimized(n, arr, m, k):
    maxi = max(arr)
    low, high = 1, maxi + 1
    ans = -1
    # Binary search on days range [1, max]
    while low <= high:
        mid = (low + high) // 2
        temp = check(n, arr, mid, m, k)
        # If m bouquets can be made by day mid, try earlier days
        if (temp):
            ans = mid
            high = mid - 1
        # Otherwise, need more days
        else:
            low = mid + 1
    return ans
```

**Time Complexity:** O(n * log(max(nums)))  
**Space Complexity:** O(1)

---

## 6. Kth Missing Positive Number

### Problem Description
Given a **strictly increasing array** and a positive integer **k**, find the **kth positive integer missing** from the array.

---

### Sample Test Cases

#### Example 1:
```
Input: vec = [4, 7, 9, 10], k = 1
Output: 1
Explanation: Missing numbers are [1, 2, 3, 5, 6, 8, 11, 12...]
             The 1st missing number is 1.
```

#### Example 2:
```
Input: vec = [4, 7, 9, 10], k = 4
Output: 5
Explanation: Missing numbers are [1, 2, 3, 5, 6, 8, 11, 12...]
             The 4th missing number is 5.
```

---

### Key Insight

**Formula:** At any index `i`, the count of missing positive integers before `arr[i]` is:
```
missingCnt = arr[i] - (i + 1)
```

**Why?** 
- Ideally, at index `i`, we should have `(i + 1)` numbers: [1, 2, 3, ..., i+1]
- If `arr[i]` is present, then `arr[i] - (i + 1)` numbers are missing before it

**Finding the kth missing number:**
- When `missingCnt < k`, the kth missing number is: `arr[i] + (k - missingCnt)`

---

## Approach 1: Linear Search

### Intuition
Iterate through the array and calculate the missing count at each position. Keep updating the answer whenever we find a position where `missingCnt < k`.

### Algorithm
1. Initialize `ans = k`
2. For each index `i` from 0 to n-1:
   - Calculate `missingCnt = arr[i] - (i + 1)`
   - If `missingCnt < k`, update `ans = arr[i] + (k - missingCnt)`
3. Return `ans`

### Code
```python
def findKthPositive(arr, k):
    n = len(arr)
    ans = k  # Initialize answer as k
    
    # Iterate through each element
    for i in range(n):
        # Count missing numbers before arr[i]
        missingCnt = arr[i] - (i + 1)
        
        # If missing count < k, update answer
        if missingCnt < k:
            ans = arr[i] + (k - missingCnt)
    
    return ans
```

### Complexity Analysis
- **Time Complexity:** O(n) - Single pass through the array
- **Space Complexity:** O(1) - Only using constant extra space

---

## Approach 2: Binary Search (Optimized)

### Intuition
Since the array is sorted, we can use binary search to efficiently find the position where the missing count transitions around k. This reduces the time complexity from O(n) to O(log n).

### Algorithm
1. Initialize `low = 0`, `high = n - 1`, `ans = k`
2. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - Calculate `missingCnt = arr[mid] - (mid + 1)`
   - If `missingCnt < k`:
     - Update `ans = arr[mid] + (k - missingCnt)`
     - Search right half: `low = mid + 1`
   - Else:
     - Search left half: `high = mid - 1`
3. Return `ans`

### Code
```python
def findKthPositive(arr, k):
    n = len(arr)
    low, high = 0, n - 1
    ans = k  # Initialize answer as k
    
    # Binary search on array indices
    while low <= high:
        mid = (low + high) // 2
        
        # Count missing numbers before arr[mid]
        missingCnt = arr[mid] - (mid + 1)
        
        # If missing count < k, search right half
        if missingCnt < k:
            ans = arr[mid] + (k - missingCnt)
            low = mid + 1
        # Otherwise, search left half
        else:
            high = mid - 1
    
    return ans
```

### Complexity Analysis
- **Time Complexity:** O(log n) - Binary search reduces search space by half each iteration
- **Space Complexity:** O(1) - Only using constant extra space

---

## Comparison

| Approach | Time Complexity | Space Complexity | Best For |
|----------|----------------|------------------|----------|
| Linear Search | O(n) | O(1) | Small arrays or when simplicity is preferred |
| Binary Search | O(log n) | O(1) | Large arrays where performance matters |

---

## Dry Run Example

**Input:** arr = [4, 7, 9, 10], k = 4

### Linear Search:
```
i=0: arr[0]=4, missingCnt=4-1=3 < 4 → ans=4+(4-3)=5
i=1: arr[1]=7, missingCnt=7-2=5 ≥ 4 → no update
i=2: arr[2]=9, missingCnt=9-3=6 ≥ 4 → no update
i=3: arr[3]=10, missingCnt=10-4=6 ≥ 4 → no update
Output: 5
```

### Binary Search:
```
Iteration 1: low=0, high=3, mid=1
  missingCnt=7-2=5 ≥ 4 → high=0

Iteration 2: low=0, high=0, mid=0
  missingCnt=4-1=3 < 4 → ans=4+(4-3)=5, low=1

low > high → Exit
Output: 5
```

---

## Comparison Table

| Problem | Brute Force | Optimized | Search Space | Key Concept |
|---------|-------------|-----------|--------------|-------------|
| Square Root | O(n) | O(log n) | [0, n] | Find largest num where num² ≤ n |
| Nth Root | O(m) | O(log m) | [0, m] | Find num where num^n = m |
| Smallest Divisor | O(max * n) | O(n * log max) | [1, max(nums)] | Minimize divisor for ceiling sum ≤ limit |
| Koko Bananas | O(max * n) | O(n * log max) | [1, max(nums)] | Minimize eating speed to finish in h hours |
| M Bouquets | O(max * n) | O(n * log max) | [1, max(nums)] | Minimize days to make m bouquets |
| Kth Missing | O(n) | O(log n) | [0, n-1] | Find kth missing using missing count formula |

**Pattern:** All problems use binary search on answer space to minimize/find a value that satisfies a monotonic condition.
