# Aggressive Cows

## Problem Description

Given an array `nums` of size `n`, which denotes the positions of stalls, and an integer `k`, which denotes the number of aggressive cows, assign stalls to `k` cows such that the minimum distance between any two cows is the maximum possible. Find the maximum possible minimum distance.

**Notes on constraints:**
- The positions are not necessarily sorted initially
- We need to place exactly `k` cows in `n` stalls
- Goal: Maximize the minimum distance between any two cows

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: n = 6, k = 4, nums = [0, 3, 4, 7, 10, 9]
Output: 3
```
**Explanation:** The maximum possible minimum distance between any two cows will be 3 when 4 cows are placed at positions [0, 3, 7, 10]. Here the distances between cows are 3, 4, and 3 respectively. We cannot make the minimum distance greater than 3 in any way.

**Test Case 2:**
```
Input: n = 5, k = 2, nums = [4, 2, 1, 3, 6]
Output: 5
```
**Explanation:** The maximum possible minimum distance between any two cows will be 5 when 2 cows are placed at positions [1, 6]. This gives us the maximum separation possible between the two cows.

## Approaches

### Approach 1: Linear Search (Brute Force)

**Summary:** Try every possible minimum distance from 0 to maximum range and find the largest distance where we can place all k cows.

**Intuition**

The key insight is that if we can place `k` cows with a minimum distance of `d`, then we can also place them with any smaller distance. This means we need to find the largest distance `d` such that placing `k` cows is still possible. We sort the stalls first, then try each possible distance starting from 0 and check if we can place all cows with that minimum distance.

**Approach Steps**

1. Sort the array of stall positions
2. Calculate the maximum possible distance (last position - first position)
3. For each distance from 0 to maximum distance:
   - Check if we can place `k` cows with at least this distance between them
   - Use a greedy approach: place the first cow at the first stall, then place each subsequent cow at the first available stall that is at least `dist` away from the previous cow
   - If we can place all `k` cows, update the answer
   - If we cannot place `k` cows, break (no larger distance will work)
4. Return the maximum distance found

**Example**

Consider `nums = [0, 3, 4, 7, 10, 9]` and `k = 4`:

1. After sorting: `[0, 3, 4, 7, 9, 10]`
2. Maximum range = 10 - 0 = 10
3. Try distance = 0: Place at [0, 3, 4, 7] → Can place 4 cows ✓
4. Try distance = 1: Place at [0, 3, 4, 7] → Can place 4 cows ✓
5. Try distance = 2: Place at [0, 3, 7, 9] → Can place 4 cows ✓
6. Try distance = 3: Place at [0, 3, 7, 10] → Can place 4 cows ✓
7. Try distance = 4: Place at [0, 4, 9] → Can only place 3 cows ✗
8. Answer = 3

**Code**

```python
def isPossible(n, arr, k, dist):
    # Place first cow at first stall
    prev = arr[0]
    cnt = 1
    
    # Try to place remaining cows
    for i in range(1, n):
        # If current stall is at least 'dist' away from previous cow
        if(arr[i] - prev >= dist):
            cnt += 1
            prev = arr[i]
        # If we've placed all k cows successfully
        if(cnt >= k):
            return True
    return False

def solve(n, arr, k):
    # Sort the stall positions
    arr.sort()
    
    # Calculate maximum possible distance
    limit = arr[-1] - arr[0]
    ans = limit
    
    # Try each possible distance from 0 to limit
    for dist in range(limit + 1):
        if(isPossible(n, arr, k, dist)):
            # Update answer if this distance is possible
            ans = dist
        else:
            # If this distance doesn't work, larger distances won't work either
            break
    return ans
           
    
arr = [0, 3, 4, 7, 10, 9]
n = len(arr)
k = 4
print(solve(n, arr, k))
```

**Time Complexity**

O(n log n + n × max_distance) — Sorting takes O(n log n), and we iterate through all possible distances (up to max_distance), checking each one in O(n) time. In the worst case, max_distance can be very large.

**Space Complexity**

O(1) — Only using a constant amount of extra space (excluding the input array).

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search on the answer space (possible minimum distances) to efficiently find the maximum distance.

**Intuition**

Instead of checking every possible distance linearly, we can use binary search on the answer. The key observation is that if we can place `k` cows with minimum distance `d`, then we can also place them with any smaller distance. This creates a search space where all distances ≤ answer are valid, and all distances > answer are invalid. We binary search on this space to find the maximum valid distance.

**Approach Steps**

1. Sort the array of stall positions
2. Set the search range: `low = 0`, `high = (last position - first position)`
3. While `low <= high`:
   - Calculate `mid = (low + high) / 2`
   - Check if we can place `k` cows with minimum distance `mid`
   - If yes: update answer to `mid` and search for a larger distance (set `low = mid + 1`)
   - If no: search for a smaller distance (set `high = mid - 1`)
4. Return the maximum distance found

**Example**

Consider `nums = [0, 3, 4, 7, 10, 9]` and `k = 4`:

1. After sorting: `[0, 3, 4, 7, 9, 10]`
2. Initial: `low = 0`, `high = 10`
3. Iteration 1: `mid = 5` → Can't place 4 cows with distance 5 → `high = 4`
4. Iteration 2: `mid = 2` → Can place 4 cows → `ans = 2`, `low = 3`
5. Iteration 3: `mid = 3` → Can place 4 cows → `ans = 3`, `low = 4`
6. Iteration 4: `mid = 4` → Can't place 4 cows → `high = 3`
7. Loop ends, answer = 3

**Code**

```python
def isPossible(n, arr, k, dist):
    # Place first cow at first stall
    prev = arr[0]
    cnt = 1
    
    # Try to place remaining cows
    for i in range(1, n):
        # If current stall is at least 'dist' away from previous cow
        if(arr[i] - prev >= dist):
            cnt += 1
            prev = arr[i]
        # If we've placed all k cows successfully
        if(cnt >= k):
            return True
    return False

def optimized(n, arr, k):
    # Sort the stall positions
    arr.sort()
    
    # Calculate maximum possible distance
    limit = arr[-1] - arr[0]
    ans = limit
    
    # Binary search on the answer
    low, high = 0, limit
    while low <= high:
        mid = (low + high) // 2
        
        # If we can place k cows with distance mid
        if(isPossible(n, arr, k, mid)):
            ans = mid
            # Try for a larger distance
            low = mid + 1
        else:
            # Try for a smaller distance
            high = mid - 1
    return ans
           
    
arr = [0, 3, 4, 7, 10, 9]
n = len(arr)
k = 4
print(optimized(n, arr, k))
```

**Time Complexity**

O(n log n + n × log(max_distance)) — Sorting takes O(n log n), and binary search takes O(log(max_distance)) iterations, each requiring O(n) time to check if placement is possible.

**Space Complexity**

O(1) — Only using a constant amount of extra space (excluding the input array).

---

# Split Array - Largest Sum

## Problem Description

Given an integer array `a` of size `n` and an integer `k`, split the array `a` into `k` non-empty subarrays such that the largest sum of any subarray is minimized. Return the minimized largest sum of the split.

**Notes on constraints:**
- The array must be split into exactly `k` consecutive (contiguous) subarrays
- Each subarray must be non-empty
- The goal is to minimize the maximum sum among all subarrays
- We need to balance the subarray sums to achieve the minimum possible maximum

---

## Sample Test Cases With Explanation

### Test Case 1
```
Input: a = [1, 2, 3, 4, 5], k = 3
Output: 6
```
**Explanation:** There are many ways to split the array into 3 consecutive subarrays. The best way is to split into [1, 2, 3], [4], and [5], where the largest sum among the three subarrays is only 6. This is optimal because it balances the sums across subarrays.

### Test Case 2
```
Input: a = [3, 5, 1], k = 3
Output: 5
```
**Explanation:** There is only one way to split the array into 3 subarrays: [3], [5], and [1]. The largest sum among these subarrays is 5, which comes from the second subarray.

---

## Approaches

### Approach 1: Linear Search

**Summary:** Try every possible maximum sum value from the largest element to the total sum, and find the smallest value that allows splitting into k or fewer subarrays.

---

**Intuition**

The key insight is that the answer lies between `max(array)` (when each element is in its own subarray) and `sum(array)` (when all elements are in one subarray). 

For any given maximum sum limit, we can greedily determine how many subarrays we need by adding elements until the sum exceeds the limit. 

We use `cnt <= k` instead of `cnt == k` because if we can split into fewer subarrays, we can always add more splits to reach exactly `k` subarrays without increasing the maximum sum. 

Since we want to minimize the maximum sum, we need to balance the subarray sums by checking every possibility from smallest to largest until we find the first valid answer.

---

**Approach Steps**

1. Set the search range:
   - `low = max(array)` (minimum possible max sum - each element in its own subarray)
   - `high = sum(array)` (maximum possible max sum - all elements in one subarray)

2. For each possible maximum sum from `low` to `high`:
   - Count how many subarrays are needed if no subarray sum exceeds this maximum
   - Use a greedy approach: keep adding elements to the current subarray until adding the next element would exceed the maximum sum
   - If the count of subarrays is ≤ k, this is a valid answer (we can always split further if needed)
   - Return the first valid maximum sum found (this will be the minimum)

3. Return the answer

---

**Example**

Consider `a = [1, 2, 3, 4, 5]` and `k = 3`:

**Step 1:** Initialize search range
- `low = 5` (max element)
- `high = 15` (sum of all elements)

**Step 2:** Try maxSum = 5
- Start with subarray [1, 2]
- Cannot add 3 (would make sum = 6 > 5), so start new subarray [3]
- Cannot add 4 (would make sum = 7 > 5), so start new subarray [4]
- Cannot add 5 (would make sum = 9 > 5), so start new subarray [5]
- Result: [1, 2], [3], [4], [5] → 4 subarrays
- 4 > 3, so this doesn't work ✗

**Step 3:** Try maxSum = 6
- Start with subarray [1, 2, 3] (sum = 6)
- Cannot add 4 (would make sum = 10 > 6), so start new subarray [4]
- Cannot add 5 (would make sum = 9 > 6), so start new subarray [5]
- Result: [1, 2, 3], [4], [5] → 3 subarrays
- 3 ≤ 3, so this works ✓

**Answer = 6**

---

**Code**

```python
def cntPartions(n, arr, maxSum):
    # Track current subarray sum
    currentSum = 0
    # Count of subarrays
    cnt = 0
    
    for i in range(n):
        # If we can add current element to current subarray
        if(currentSum + arr[i] <= maxSum):
            currentSum = currentSum + arr[i]
        else:
            # Start a new subarray
            cnt += 1
            currentSum = arr[i]
    
    # Count the last subarray if it has elements
    if(currentSum <= maxSum):
        cnt += 1
    return cnt
    
def solve(n, arr, k):
    # Minimum possible max sum is the largest element
    # Maximum possible max sum is the sum of all elements
    low, high = max(arr), sum(arr)
    ans = high
    
    # Try every possible maximum sum
    for maxSum in range(low, high + 1):
        # Count partitions needed with this max sum
        cnt = cntPartions(n, arr, maxSum)
        
        # We use cnt <= k instead of cnt == k because:
        # If we can split into fewer subarrays, we can always
        # add more splits to reach exactly k without increasing max sum
        if(cnt <= k):
            ans = maxSum
            break
    return ans
           
    
arr = [0, 3, 4, 7, 10, 9]
n = len(arr)
k = 4
print(solve(n, arr, k))
```

---

**Time Complexity**

O(n × (sum - max)) — For each possible maximum sum value (which can range from max element to total sum), we count partitions in O(n) time. In the worst case, this range can be large.

---

**Space Complexity**

O(1) — Only using a constant amount of extra space (excluding the input array).

---

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search on the answer space (possible maximum sums) to efficiently find the minimum maximum sum.

---

**Intuition**

Instead of checking every possible maximum sum linearly, we can use binary search on the answer space.

The key observation is that if we can split the array into `k` or fewer subarrays with maximum sum `m`, then we can also do it with any larger maximum sum. This creates a monotonic search space:
- Smaller values might not work (too many subarrays needed)
- Once we find a working value, all larger values also work

We use `cnt <= k` instead of `cnt == k` because:
- If we can split into fewer subarrays than `k`, we have "room" to add more splits
- Adding more splits doesn't increase the maximum sum
- So it's still a valid solution

Since we want to minimize the maximum sum, we need to balance the subarray sums. Binary search helps us systematically find the smallest valid maximum sum.

---

**Approach Steps**

1. Set the search range:
   - `low = max(array)` (minimum possible max sum)
   - `high = sum(array)` (maximum possible max sum)

2. While `low <= high`:
   - Calculate `mid = (low + high) / 2`
   - Count how many subarrays are needed with maximum sum `mid` using greedy partitioning
   - If `count <= k`: 
     - This is a valid solution
     - Update answer to `mid`
     - Search for smaller values: set `high = mid - 1`
   - If `count > k`: 
     - We need a larger maximum sum
     - Set `low = mid + 1`

3. Return the minimum maximum sum found

---

**Example**

Consider `a = [1, 2, 3, 4, 5]` and `k = 3`:

**Initial state:**
- `low = 5`, `high = 15`

---

**Iteration 1:**
- `mid = (5 + 15) / 2 = 10`
- Count partitions with maxSum = 10:
  - [1, 2, 3, 4] (sum = 10), [5] → 2 subarrays
- 2 ≤ 3 ✓ → Valid solution
- Update: `ans = 10`, `high = 9`

---

**Iteration 2:**
- `mid = (5 + 9) / 2 = 7`
- Count partitions with maxSum = 7:
  - [1, 2, 3] (sum = 6), [4], [5] → 3 subarrays
- 3 ≤ 3 ✓ → Valid solution
- Update: `ans = 7`, `high = 6`

---

**Iteration 3:**
- `mid = (5 + 6) / 2 = 5`
- Count partitions with maxSum = 5:
  - [1, 2], [3], [4], [5] → 4 subarrays
- 4 > 3 ✗ → Not valid
- Update: `low = 6`

---

**Iteration 4:**
- `mid = (6 + 6) / 2 = 6`
- Count partitions with maxSum = 6:
  - [1, 2, 3] (sum = 6), [4], [5] → 3 subarrays
- 3 ≤ 3 ✓ → Valid solution
- Update: `ans = 6`, `high = 5`

---

**Loop terminates** (low > high)

**Answer = 6**

---

**Code**

```python
def cntPartions(n, arr, maxSum):
    # Track current subarray sum
    currentSum = 0
    # Count of subarrays
    cnt = 0
    
    for i in range(n):
        # If we can add current element to current subarray
        if(currentSum + arr[i] <= maxSum):
            currentSum = currentSum + arr[i]
        else:
            # Start a new subarray
            cnt += 1
            currentSum = arr[i]
    
    # Count the last subarray if it has elements
    if(currentSum <= maxSum):
        cnt += 1
    return cnt

def optimized(n, arr, k):
    # Minimum possible max sum is the largest element
    # Maximum possible max sum is the sum of all elements
    low, high = max(arr), sum(arr)
    ans = high
    
    # Binary search on the answer
    while low <= high:
        mid = (low + high) // 2
        
        # Count partitions needed with this max sum
        cnt = cntPartions(n, arr, mid)
        
        # We use cnt <= k instead of cnt == k because:
        # If we can split into fewer subarrays, we can always
        # add more splits to reach exactly k without increasing max sum
        # Since we want minimum max sum, we balance subarray sums
        if(cnt <= k):
            ans = mid
            # Try for a smaller maximum sum
            high = mid - 1
        else:
            # Need a larger maximum sum
            low = mid + 1
    return ans
           
    
arr = [0, 3, 4, 7, 10, 9]
n = len(arr)
k = 4
print(optimized(n, arr, k))
```

---

**Time Complexity**

O(n × log(sum - max)) — Binary search takes O(log(sum - max)) iterations, and each iteration requires O(n) time to count partitions.

---

**Space Complexity**

O(1) — Only using a constant amount of extra space (excluding the input array).

---
