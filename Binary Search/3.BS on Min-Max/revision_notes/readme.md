# Binary Search Problems - Revision Notes (Set 2)

## Index

1. [Aggressive Cows](#1-aggressive-cows)
2. [Split Array - Largest Sum](#2-split-array---largest-sum)
3. [Book Allocation Problem](#3-book-allocation-problem)
4. [The Painter's Partition Problem](#4-the-painters-partition-problem)
5. [Minimize Max Distance to Gas Station](#5-minimize-max-distance-to-gas-station)
6. [Comparison Table](#comparison-table)

---

## 1. Aggressive Cows

### Problem Description
Given an array nums of size n denoting positions of stalls, and an integer k denoting the number of aggressive cows, assign stalls to k cows such that the minimum distance between any two cows is the maximum possible. Find the maximum possible minimum distance.

### Sample Test Cases
```
Input: n = 6, k = 4, nums = [0, 3, 4, 7, 10, 9]
Output: 3
Explanation: Place 4 cows at positions [0, 3, 7, 10]. Distances are 3, 4, 3.
Minimum distance is 3, which is the maximum possible.

Input: n = 5, k = 2, nums = [4, 2, 1, 3, 6]
Output: 5
Explanation: Place 2 cows at positions [1, 6]. Distance is 5.
```

### Approach 1: Linear Search

**Intuition:** Sort the array first. Try every possible minimum distance from 0 to (max_position - min_position) and check if we can place k cows with at least that distance between them.

**Code:**
```python
def isPossible(n, arr, k, dist):
    prev = arr[0]  # Place first cow at first stall
    cnt = 1
    # Try to place remaining cows
    for i in range(1, n):
        # If distance from previous cow is >= dist
        if (arr[i] - prev >= dist):
            cnt += 1  # Place cow here
            prev = arr[i]  # Update previous position
        if (cnt >= k):
            return True  # Successfully placed all k cows
    return False

def solve(n, arr, k):
    arr.sort()  # Sort stall positions
    limit = arr[-1] - arr[0]  # Maximum possible distance
    ans = limit
    # Try every distance from 0 to limit
    for dist in range(limit + 1):
        if (isPossible(n, arr, k, dist)):
            ans = dist  # Update answer
        else:
            break  # No point checking larger distances
    return ans
```

**Time Complexity:** O(n log n + limit * n) where limit = max - min position  
**Space Complexity:** O(1)

### Approach 2: Binary Search (Optimized)

**Intuition:** Binary search on the distance. If we can place k cows with distance mid, try for a larger distance. Otherwise, try a smaller distance.

**Code:**
```python
def optimized(n, arr, k):
    arr.sort()  # Sort stall positions
    limit = arr[-1] - arr[0]  # Maximum possible distance
    ans = limit
    low, high = 0, limit
    # Binary search on distance
    while low <= high:
        mid = (low + high) // 2
        # Check if we can place k cows with distance mid
        if (isPossible(n, arr, k, mid)):
            ans = mid  # Update answer
            low = mid + 1  # Try for larger distance
        else:
            high = mid - 1  # Try smaller distance
    return ans
```

**Time Complexity:** O(n log n + n * log(max-min))  
**Space Complexity:** O(1)

---

## 2. Split Array - Largest Sum

### Problem Description
Given an integer array a of size n and an integer k, split the array into k non-empty subarrays such that the largest sum of any subarray is minimized. Return the minimized largest sum.

### Sample Test Cases
```
Input: a = [1, 2, 3, 4, 5], k = 3
Output: 6
Explanation: Split into [1, 2, 3], [4], [5]. Largest sum is 6.

Input: a = [3, 5, 1], k = 3
Output: 5
Explanation: Split into [3], [5], [1]. Largest sum is 5.
```

### Approach 1: Linear Search

**Intuition:** The minimum possible largest sum is max(arr) (each element in its own subarray). The maximum is sum(arr) (all in one subarray). Try each value and count how many subarrays are needed.

**Code:**
```python
def cntPartions(n, arr, maxSum):
    currentSum = 0
    cnt = 0
    # Count partitions needed with maxSum limit
    for i in range(n):
        # If adding current element doesn't exceed maxSum
        if (currentSum + arr[i] <= maxSum):
            currentSum = currentSum + arr[i]
        else:
            # Start new partition
            cnt += 1
            currentSum = arr[i]
    # Count the last partition
    if (currentSum <= maxSum):
        cnt += 1
    return cnt

def solve(n, arr, k):
    low, high = max(arr), sum(arr)  # Search range
    ans = high
    # Try every possible maxSum from low to high
    for maxSum in range(low, high + 1):
        cnt = cntPartions(n, arr, maxSum)
        # If we can split into <= k partitions
        if (cnt <= k):
            ans = maxSum
            break  # Found minimum
    return ans
```

**Time Complexity:** O((sum - max) * n)  
**Space Complexity:** O(1)

### Approach 2: Binary Search (Optimized)

**Intuition:** Binary search on the maximum sum. If we can split into ≤k subarrays with maxSum = mid, try smaller maxSum. Otherwise, try larger maxSum.

**Code:**
```python
def optimized(n, arr, k):
    low, high = max(arr), sum(arr)  # Search range
    ans = high
    # Binary search on maximum sum
    while low <= high:
        mid = (low + high) // 2
        cnt = cntPartions(n, arr, mid)
        # If we can split into <= k partitions
        if (cnt <= k):
            ans = mid  # Update answer
            high = mid - 1  # Try smaller maxSum
        else:
            low = mid + 1  # Need larger maxSum
    return ans
```

**Time Complexity:** O(n * log(sum))  
**Space Complexity:** O(1)

---

## 3. Book Allocation Problem

### Problem Description
Given an array nums of n integers where nums[i] represents pages in the i-th book, and m students, allocate all books to students (each gets at least one, contiguous allocation) such that the maximum pages assigned to a student is minimized. Return -1 if allocation is not possible.

### Sample Test Cases
```
Input: nums = [12, 34, 67, 90], m = 2
Output: 113
Explanation: Allocation: [12, 34, 67] | [90]. Max pages = 113.

Input: nums = [25, 46, 28, 49, 24], m = 4
Output: 71
Explanation: Allocation: [25, 46] | [28] | [49] | [24]. Max pages = 71.
```

### Approach 1: Linear Search

**Intuition:** Similar to split array. If students > books, return -1. Search range is [max(pages), sum(pages)]. Try each value and check if allocation is possible.

**Code:**
```python
def cntPartions(n, arr, maxSum):
    currentSum = 0
    cnt = 0
    # Count students needed with maxSum pages limit
    for i in range(n):
        if (currentSum + arr[i] <= maxSum):
            currentSum = currentSum + arr[i]
        else:
            cnt += 1  # New student
            currentSum = arr[i]
    if (currentSum <= maxSum):
        cnt += 1
    return cnt

def solve(n, arr, k):
    if (k > n):
        return -1  # More students than books
    low, high = max(arr), sum(arr)
    ans = -1
    # Try every possible max pages
    for maxSum in range(low, high + 1):
        cnt = cntPartions(n, arr, maxSum)
        # If allocation possible with <= k students
        if (cnt <= k):
            ans = maxSum
            break
    return ans
```

**Time Complexity:** O((sum - max) * n)  
**Space Complexity:** O(1)

### Approach 2: Binary Search (Optimized)

**Intuition:** Binary search on maximum pages. If allocation possible with mid pages, try smaller value. Otherwise, try larger value.

**Code:**
```python
def optimized(n, arr, k):
    if (k > n):
        return -1  # More students than books
    low, high = max(arr), sum(arr)
    ans = -1
    # Binary search on maximum pages
    while low <= high:
        mid = (low + high) // 2
        cnt = cntPartions(n, arr, mid)
        # If allocation possible with <= k students
        if (cnt <= k):
            ans = mid
            high = mid - 1  # Try smaller max pages
        else:
            low = mid + 1  # Need more pages
    return ans
```

**Time Complexity:** O(n * log(sum))  
**Space Complexity:** O(1)

---

## 4. The Painter's Partition Problem

### Problem Description
Given an array arr[] where each element denotes board length, and k painters. Each painter takes 1 unit time per unit length and paints contiguous boards. Find the minimum time to paint all boards.

### Sample Test Cases
```
Input: arr = [5, 10, 30, 20, 15], k = 3
Output: 35
Explanation: [5,10] | [30] | [20,15]. Times: 15, 30, 35. Max = 35.

Input: arr = [10, 20, 30, 40], k = 2
Output: 60
Explanation: [10,20,30] | [40]. Times: 60, 40. Max = 60.

Input: arr = [100, 200, 300, 400], k = 1
Output: 1000
Explanation: One painter paints all. Total = 1000.
```

### Approach 1: Linear Search

**Intuition:** Same pattern as book allocation. Search range is [max(arr), sum(arr)]. Find minimum time such that k painters can finish all boards.

**Code:**
```python
def cntPartions(n, arr, maxSum):
    currentSum = 0
    cnt = 0
    # Count painters needed with maxSum time limit
    for i in range(n):
        if (currentSum + arr[i] <= maxSum):
            currentSum = currentSum + arr[i]
        else:
            cnt += 1  # New painter
            currentSum = arr[i]
    if (currentSum <= maxSum):
        cnt += 1
    return cnt

def solve(n, arr, k):
    low, high = max(arr), sum(arr)
    ans = low
    # Try every possible max time
    for maxSum in range(low, high + 1):
        cnt = cntPartions(n, arr, maxSum)
        # If painting possible with <= k painters
        if (cnt <= k):
            ans = maxSum
            break
    return ans
```

**Time Complexity:** O((sum - max) * n)  
**Space Complexity:** O(1)

### Approach 2: Binary Search (Optimized)

**Intuition:** Binary search on maximum time. If k painters can finish in mid time, try smaller time. Otherwise, try larger time.

**Code:**
```python
def optimized(n, arr, k):
    low, high = max(arr), sum(arr)
    ans = low
    # Binary search on maximum time
    while low <= high:
        mid = (low + high) // 2
        cnt = cntPartions(n, arr, mid)
        # If painting possible with <= k painters
        if (cnt <= k):
            ans = mid
            high = mid - 1  # Try smaller time
        else:
            low = mid + 1  # Need more time
    return ans
```

**Time Complexity:** O(n * log(sum))  
**Space Complexity:** O(1)

---

## 5. Minimize Max Distance to Gas Station

### Problem Description
Given n gas stations at sorted positions arr and k new stations to place, find the minimum possible value of the maximum distance between adjacent stations. Answer within 1e-6 accuracy.

### Sample Test Cases
```
Input: n = 10, arr = [1,2,3,4,5,6,7,8,9,10], k = 9
Output: 0.50000
Explanation: Place stations at [1,1.5,2,2.5,...]. Max distance = 0.5.

Input: n = 10, arr = [1,2,3,4,5,6,7,8,9,10], k = 1
Output: 1.00000
Explanation: Place at position 11. Max distance still 1.
```

### Approach 1: Greedy (Brute Force)

**Intuition:** Repeatedly find the section with maximum distance and place a new station there. After k placements, return the maximum section length.

**Code:**
```python
def solve(n, arr, k):
    splitCnt = [0] * (n - 1)  # Count of stations in each section
    
    # Place k new stations one by one
    for _ in range(k):
        maxSection, maxIndex = -1, -1
        # Find section with maximum length
        for i in range(n - 1):
            gap = arr[i + 1] - arr[i]
            # Calculate section length after splits
            sectionLen = gap / (splitCnt[i] + 1)
            if (sectionLen > maxSection):
                maxSection = sectionLen
                maxIndex = i
        splitCnt[maxIndex] += 1  # Place station in this section
    
    # Find maximum section length after all placements
    maxLen = 0
    for i in range(n - 1):
        gap = arr[i + 1] - arr[i]
        sectionLen = gap / (splitCnt[i] + 1)
        maxLen = max(maxLen, sectionLen)
    return maxLen
```

**Time Complexity:** O(k * n)  
**Space Complexity:** O(n)

### Approach 2: Priority Queue (Better)

**Intuition:** Use max heap to efficiently find and update the section with maximum length.

**Code:**
```python
import heapq

def solve2(n, arr, k):
    splitCnt = [0] * (n - 1)
    pq = []  # Max heap (using negative values)
    
    # Initialize heap with all sections
    for i in range(n - 1):
        gap = arr[i + 1] - arr[i]
        heapq.heappush(pq, (-gap, i))
    
    # Place k new stations
    for _ in range(k):
        _, index = heapq.heappop(pq)  # Get section with max length
        splitCnt[index] += 1
        gap = arr[index + 1] - arr[index]
        # Recalculate section length after split
        sectionLen = gap / (splitCnt[index] + 1)
        heapq.heappush(pq, (-sectionLen, index))
    
    return -pq[0][0]  # Return max section length
```

**Time Complexity:** O(n + k log n)  
**Space Complexity:** O(n)

### Approach 3: Binary Search (Optimized)

**Intuition:** Binary search on the maximum distance. For a given distance, calculate how many stations are needed to ensure no section exceeds this distance.

**Code:**
```python
import math

def cntSections(n, arr, dist):
    cnt = 0
    # Count stations needed to keep all sections <= dist
    for i in range(n - 1):
        gap = arr[i + 1] - arr[i]
        # Number of stations needed in this section
        cnt += math.ceil(gap / dist) - 1
    return cnt

def optimized(n, arr, k):
    maxGap = 0
    # Find maximum gap
    for i in range(n - 1):
        maxGap = max(maxGap, (arr[i + 1] - arr[i]))
    
    low, high = 0, maxGap
    ans = maxGap
    # Binary search on distance (with precision 1e-6)
    while (high - low) >= (1e-6):
        mid = (low + high) / 2.0
        cnt = cntSections(n, arr, mid)
        # If we can achieve distance mid with <= k stations
        if (cnt <= k):
            ans = mid
            high = mid  # Try smaller distance
        else:
            low = mid  # Need larger distance
    return ans
```

**Time Complexity:** O(n * log(maxGap / 1e-6))  
**Space Complexity:** O(1)

---

## Comparison Table

| Problem | Brute Force | Optimized | Search Space | Key Concept |
|---------|-------------|-----------|--------------|-------------|
| Aggressive Cows | O(limit * n) | O(n log n + n * log limit) | [0, max-min] | Maximize minimum distance between cows |
| Split Array | O((sum-max) * n) | O(n * log sum) | [max, sum] | Minimize maximum subarray sum |
| Book Allocation | O((sum-max) * n) | O(n * log sum) | [max, sum] | Minimize maximum pages per student |
| Painter's Partition | O((sum-max) * n) | O(n * log sum) | [max, sum] | Minimize maximum time per painter |
| Gas Station | O(k * n) / O(n + k log n) | O(n * log(maxGap/1e-6)) | [0, maxGap] | Minimize maximum distance using k stations |

**Pattern:** Problems 2-4 use the same partition counting logic. Binary search optimizes the answer space search for min-max or max-min problems.
