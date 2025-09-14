# Sliding Window Problems Collection 🪟

A comprehensive collection of sliding window technique problems with multiple solution approaches, detailed explanations, and complexity analysis.

## Table of Contents
1. [Max Sum Subarray of Size K](#1-max-sum-subarray-of-size-k)
2. [Find All Maximum Elements of Subarrays of Size K](#2-find-all-maximum-elements-of-subarrays-of-size-k)
3. [Sum of Min and Max Elements of Subarray of Size K](#3-sum-of-min-and-max-elements-of-subarray-of-size-k)
4. [Find First Negative Number of Subarray of Size K](#4-find-first-negative-number-of-subarray-of-size-k)
5. [Count Distinct Elements in All Subarrays of Size K](#5-count-distinct-elements-in-all-subarrays-of-size-k)
6. [Maximum Points from Cards](#6-maximum-points-from-cards)

---

## 1. Max Sum Subarray of Size K

**Problem Statement:**
Given an array of integers and an integer k, find the maximum sum of any subarray of size k.

**Example:**
```
Input:  arr = [2, 1, 5, 1, 3, 2], k = 3
Output: 11
Explanation: The subarray [5, 1, 3] has the maximum sum of 11.
```

### Solutions:

#### Brute Force Approach
**Approach:** Check all possible subarrays of size k  
**Time Complexity:** O(n × k) where n is array length  
**Space Complexity:** O(1)

```python
def bruteforce(n, arr, k):
    maxSum = float('-inf')
    
    # Check each possible subarray of size k
    for i in range(n - k + 1):
        currentSum = 0
        # Calculate sum of current subarray
        for j in range(i, i + k):
            currentSum += arr[j]
        maxSum = max(maxSum, currentSum)
    
    return maxSum
```

#### Optimized Sliding Window Approach
**Approach:** Use sliding window technique with two pointers  
**Time Complexity:** O(n)  
**Space Complexity:** O(1)

```python
def optimized(n, arr, k):
    maxSum = float('-inf')
    left, right = 0, 0
    currentSum = 0
    
    # Build initial window of size k
    while right < k:
        currentSum += arr[right]
        right += 1
    maxSum = max(maxSum, currentSum)
    
    # Slide the window
    while right < n:
        # Add new element and remove leftmost element
        currentSum += arr[right]
        currentSum -= arr[left]
        left += 1
        right += 1
        maxSum = max(maxSum, currentSum)
    
    return maxSum
```

---

## 2. Find All Maximum Elements of Subarrays of Size K

**Problem Statement:**
Given an array of integers and an integer k, find the maximum element in every subarray of size k.

**Example:**
```
Input:  arr = [2, 1, 5, 1, 3, 2], k = 3
Output: [5, 5, 5, 3]
Explanation: Max of [2,1,5]=5, [1,5,1]=5, [5,1,3]=5, [1,3,2]=3
```

### Solutions:

#### Brute Force Approach
**Approach:** Check all subarrays and find maximum in each  
**Time Complexity:** O(n × k)  
**Space Complexity:** O(1) excluding output array

```python
def bruteforce(n, arr, k):
    ans = []
    
    # Check each subarray of size k
    for i in range(n - k + 1):
        maxi = float('-inf')
        # Find maximum in current subarray
        for j in range(i, i + k):
            maxi = max(maxi, arr[j])
        ans.append(maxi)
    
    return ans
```

#### Optimized Deque Approach
**Approach:** Use deque to maintain maximum elements efficiently  
**Time Complexity:** O(n)  
**Space Complexity:** O(k) for the deque

```python
from collections import deque

def optimized(n, arr, k):
    ans = []
    left, right = 0, 0
    queue = deque()  # Store indices
    
    # Build initial window
    while right < k:
        # Remove smaller elements from back
        while queue and arr[right] >= arr[queue[-1]]:
            queue.pop()
        queue.append(right)
        right += 1
    
    # First window's maximum
    ans.append(arr[queue[0]])
    
    # Slide the window
    while right < n:
        # Remove elements outside window
        while queue and queue[0] <= (right - k):
            queue.popleft()
        
        # Remove smaller elements from back
        while queue and arr[right] >= arr[queue[-1]]:
            queue.pop()
        
        queue.append(right)
        ans.append(arr[queue[0]])
        right += 1
    
    return ans
```

---

## 3. Sum of Min and Max Elements of Subarray of Size K

**Problem Statement:**
Given an array of integers and an integer k, calculate the sum of minimum and maximum values in every contiguous subarray of size k.

**Example:**
```
Input:  arr = [1, 3, 2, 5, 4], k = 3
Output: 21
Explanation: [1,3,2]: min=1, max=3, sum=4
             [3,2,5]: min=2, max=5, sum=7
             [2,5,4]: min=2, max=5, sum=7
             Total: 4+7+7=18
```

### Solutions:

#### Brute Force Approach
**Approach:** Find min and max for each subarray separately  
**Time Complexity:** O(n × k)  
**Space Complexity:** O(1)

```python
def bruteforce(n, arr, k):
    ans = 0
    
    # Check each subarray of size k
    for i in range(n - k + 1):
        maxi = float('-inf')
        mini = float('inf')
        
        # Find min and max in current subarray
        for j in range(i, i + k):
            maxi = max(maxi, arr[j])
            mini = min(mini, arr[j])
        
        ans += (maxi + mini)
    
    return ans
```

#### Optimized Dual Deque Approach
**Approach:** Use separate deques for tracking min and max  
**Time Complexity:** O(n)  
**Space Complexity:** O(k) for both deques

```python
from collections import deque

def optimized(n, arr, k):
    ans = 0
    left, right = 0, 0
    maxQueue = deque()  # For maximum elements
    minQueue = deque()  # For minimum elements
    
    # Build initial window
    while right < k:
        # Maintain decreasing order for max queue
        while maxQueue and arr[right] >= arr[maxQueue[-1]]:
            maxQueue.pop()
        
        # Maintain increasing order for min queue
        while minQueue and arr[right] <= arr[minQueue[-1]]:
            minQueue.pop()
        
        maxQueue.append(right)
        minQueue.append(right)
        right += 1
    
    # Add first window's min + max
    ans += (arr[maxQueue[0]] + arr[minQueue[0]])
    
    # Slide the window
    while right < n:
        # Remove elements outside window
        while maxQueue and maxQueue[0] <= (right - k):
            maxQueue.popleft()
        while minQueue and minQueue[0] <= (right - k):
            minQueue.popleft()
        
        # Maintain queue properties
        while maxQueue and arr[right] >= arr[maxQueue[-1]]:
            maxQueue.pop()
        while minQueue and arr[right] <= arr[minQueue[-1]]:
            minQueue.pop()
        
        minQueue.append(right)
        maxQueue.append(right)
        ans += (arr[maxQueue[0]] + arr[minQueue[0]])
        right += 1
    
    return ans
```

---

## 4. Find First Negative Number of Subarray of Size K

**Problem Statement:**
Given an array of integers and an integer k, find the first negative integer in every contiguous subarray of size k. Return 0 if no negative number exists.

**Example:**
```
Input:  arr = [1, -2, 3, -4, 5], k = 2
Output: [-2, -2, -4, -4]
Explanation: [1,-2]: first negative = -2
             [-2,3]: first negative = -2
             [3,-4]: first negative = -4
             [-4,5]: first negative = -4
```

### Solutions:

#### Brute Force Approach
**Approach:** Check each subarray and find first negative  
**Time Complexity:** O(n × k) in worst case  
**Space Complexity:** O(1) excluding output array

```python
def bruteforce(n, arr, k):
    ans = []
    
    # Check each subarray of size k
    for i in range(n - k + 1):
        # Look for first negative in current subarray
        for j in range(i, i + k):
            if arr[j] < 0:
                ans.append(arr[j])
                break
        else:
            # No negative found
            ans.append(0)
    
    return ans
```

#### Optimized Deque Approach
**Approach:** Use deque to track negative numbers in current window  
**Time Complexity:** O(n)  
**Space Complexity:** O(k) for the deque

```python
from collections import deque

def optimized(n, arr, k):
    ans = []
    right = 0
    queue = deque()  # Store indices of negative numbers
    
    # Build initial window
    while right < k:
        if arr[right] < 0:
            queue.append(right)
        right += 1
    
    # First window result
    if queue:
        ans.append(arr[queue[0]])
    else:
        ans.append(0)
    
    # Slide the window
    while right < n:
        # Remove elements outside window
        while queue and queue[0] <= right - k:
            queue.popleft()
        
        # Add new element if negative
        if arr[right] < 0:
            queue.append(right)
        
        # Get result for current window
        if queue:
            ans.append(arr[queue[0]])
        else:
            ans.append(0)
        right += 1
    
    return ans
```

---

## 5. Count Distinct Elements in All Subarrays of Size K

**Problem Statement:**
Given an array of integers and an integer k, find the count of distinct integers in every contiguous subarray of size k.

**Example:**
```
Input:  arr = [1, 2, 1, 3, 4], k = 3
Output: [2, 3, 3]
Explanation: [1,2,1]: distinct count = 2 (1,2)
             [2,1,3]: distinct count = 3 (1,2,3)
             [1,3,4]: distinct count = 3 (1,3,4)
```

### Solutions:

#### Brute Force Approach
**Approach:** Use set to count distinct elements in each subarray  
**Time Complexity:** O(n × k)  
**Space Complexity:** O(k) for the set

```python
def bruteforce(n, arr, k):
    ans = []
    
    # Check each subarray of size k
    for i in range(n - k + 1):
        unique = set()
        # Add all elements of current subarray to set
        for j in range(i, i + k):
            unique.add(arr[j])
        ans.append(len(unique))
    
    return ans
```

#### Optimized Hash Map Approach
**Approach:** Use frequency map with sliding window  
**Time Complexity:** O(n)  
**Space Complexity:** O(k) for the hash map

```python
def optimized(n, arr, k):
    ans = []
    count = {}  # Frequency map
    left, right = 0, 0
    unique = 0  # Count of distinct elements
    
    # Build initial window
    while right < k:
        if arr[right] in count:
            count[arr[right]] += 1
        else:
            unique += 1
            count[arr[right]] = 1
        right += 1
    
    ans.append(unique)
    
    # Slide the window
    while right < n:
        # Remove leftmost element
        if count[arr[left]] == 1:
            del count[arr[left]]
            unique -= 1
        else:
            count[arr[left]] -= 1
        left += 1
        
        # Add new rightmost element
        if arr[right] in count:
            count[arr[right]] += 1
        else:
            unique += 1
            count[arr[right]] = 1
        right += 1
        
        ans.append(unique)
    
    return ans
```

---

## 6. Maximum Points from Cards

**Problem Statement:**
You have cards arranged in a row, each with associated points. You can take exactly k cards, but only from the beginning or end of the row. Find the maximum score possible.

**Example:**
```
Input:  cardPoints = [1, 2, 3, 4, 5, 6, 1], k = 3
Output: 12
Explanation: Take cards from right: [6, 5, 1] = 12
```

### Solutions:

#### Brute Force Approach
**Approach:** Try all combinations of taking i cards from left and (k-i) from right  
**Time Complexity:** O(k²)  
**Space Complexity:** O(1)

```python
def bruteforce(n, arr, k):
    maxi = 0
    
    # Try taking i cards from left, (k-i) from right
    for i in range(k + 1):
        leftSum = 0
        # Sum of first (k-i) cards
        for j in range(k - i):
            leftSum += arr[j]
        
        rightSum = 0
        # Sum of last i cards
        for j in range(n - i, n):
            rightSum += arr[j]
        
        total = leftSum + rightSum
        maxi = max(maxi, total)
    
    return maxi
```

#### Optimized Sliding Window Approach
**Approach:** Start with k cards from left, then slide window by removing from left and adding from right  
**Time Complexity:** O(k)  
**Space Complexity:** O(1)

```python
def optimized(n, arr, k):
    maxPoints = 0
    
    # Start with taking all k cards from left
    points = sum(arr[:k])
    maxPoints = max(points, maxPoints)
    
    # Slide the window: remove from left, add from right
    left, right = k - 1, n - 1
    
    for i in range(k):
        # Remove leftmost card, add rightmost card
        points -= arr[left]
        points += arr[right]
        left -= 1
        right -= 1
        maxPoints = max(maxPoints, points)
    
    return maxPoints
```

---

