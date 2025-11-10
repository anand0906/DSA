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

# Book Allocation Problem

## Problem Description

Given an array `nums` of `n` integers, where `nums[i]` represents the number of pages in the i-th book, and an integer `m` representing the number of students, allocate all the books to the students so that:
- Each student gets at least one book
- Each book is allocated to only one student
- The allocation is contiguous (consecutive books)

Allocate the books to `m` students in such a way that the maximum number of pages assigned to a student is minimized. If the allocation of books is not possible, return `-1`.

**Notes on constraints:**
- If `m > n` (more students than books), allocation is impossible, return `-1`
- Books must be allocated in contiguous manner (no skipping books)
- We need to balance the page distribution to minimize the maximum pages any student receives

---

## Sample Test Cases With Explanation

### Test Case 1
```
Input: nums = [12, 34, 67, 90], m = 2
Output: 113
```
**Explanation:** The allocation of books will be [12, 34, 67] | [90]. One student will get the first 3 books (total pages = 113) and the other will get the last one (total pages = 90). The maximum pages assigned to any student is 113, which is the minimum possible.

### Test Case 2
```
Input: nums = [25, 46, 28, 49, 24], m = 4
Output: 71
```
**Explanation:** The allocation of books will be [25, 46] | [28] | [49] | [24]. The maximum pages assigned to any student is 71 (first student getting books with 25 and 46 pages), which is minimized.

---

## Approaches

### Approach 1: Linear Search

**Summary:** Try every possible maximum page count from the largest book to the total pages, and find the smallest value that allows allocating books to m or fewer students.

---

**Intuition**

The answer lies between:
- `max(nums)` — The minimum possible maximum (when each student gets at most one book, with the largest book determining the minimum)
- `sum(nums)` — The maximum possible maximum (when one student gets all books)

For any given maximum page limit, we can greedily determine how many students are needed by allocating books consecutively until adding the next book would exceed the limit.

We use `cnt <= m` instead of `cnt == m` because:
- If we can allocate books to fewer students, we can always distribute them among more students without increasing the maximum
- For example, if we can allocate to 2 students and need 3, we can split one student's allocation further

Since we want to minimize the maximum pages, we need to balance the page distribution by checking every possibility from smallest to largest until we find the first valid answer.

---

**Approach Steps**

1. Check if allocation is possible:
   - If `m > n`, return `-1` (more students than books)

2. Set the search range:
   - `low = max(nums)` (minimum possible maximum pages)
   - `high = sum(nums)` (maximum possible maximum pages)

3. For each possible maximum page count from `low` to `high`:
   - Count how many students are needed if no student gets more than this maximum
   - Use a greedy approach: keep allocating books to the current student until adding the next book would exceed the maximum
   - If the count of students is ≤ m, this is a valid answer
   - Return the first valid maximum page count found (this will be the minimum)

4. Return the answer

---

**Example**

Consider `nums = [12, 34, 67, 90]` and `m = 2`:

**Step 1:** Check feasibility
- `m = 2`, `n = 4` → 2 ≤ 4, allocation is possible ✓

**Step 2:** Initialize search range
- `low = 90` (max element)
- `high = 203` (sum of all pages)

**Step 3:** Try maxPages = 90
- Student 1: [12, 34] (sum = 46), cannot add 67 (would make sum = 113 > 90)
- Student 2: [67] (sum = 67), cannot add 90 (would make sum = 157 > 90)
- Student 3: [90] (sum = 90)
- Result: 3 students needed
- 3 > 2, so this doesn't work ✗

**Step 4:** Try maxPages = 91, 92, ... (skipping for brevity)

**Step 5:** Try maxPages = 113
- Student 1: [12, 34, 67] (sum = 113), cannot add 90 (would make sum = 203 > 113)
- Student 2: [90] (sum = 90)
- Result: 2 students needed
- 2 ≤ 2, so this works ✓

**Answer = 113**

---

**Code**

```python
def cntPartions(n, arr, maxSum):
    # Track current student's page count
    currentSum = 0
    # Count of students needed
    cnt = 0
    
    for i in range(n):
        # If we can allocate current book to current student
        if(currentSum + arr[i] <= maxSum):
            currentSum = currentSum + arr[i]
        else:
            # Allocate to next student
            cnt += 1
            currentSum = arr[i]
    
    # Count the last student if they have books
    if(currentSum <= maxSum):
        cnt += 1
    return cnt
    
def solve(n, arr, k):
    # If more students than books, allocation is impossible
    if(k > n):
        return -1
    
    # Minimum possible max pages is the largest book
    # Maximum possible max pages is the sum of all pages
    low, high = max(arr), sum(arr)
    ans = -1
    
    # Try every possible maximum page count
    for maxSum in range(low, high + 1):
        # Count students needed with this max page limit
        cnt = cntPartions(n, arr, maxSum)
        
        # We use cnt <= k instead of cnt == k because:
        # If we can allocate to fewer students, we can always
        # distribute among more students without increasing the maximum
        if(cnt <= k):
            ans = maxSum
            break
    return ans
           
    
books = [12, 34, 67, 90]
n = len(books)
m = 2
arr, k = books, m
print(solve(n, arr, k))
```

---

**Time Complexity**

O(n × (sum - max)) — For each possible maximum page count (which can range from max book to total pages), we count partitions in O(n) time. In the worst case, this range can be large.

---

**Space Complexity**

O(1) — Only using a constant amount of extra space (excluding the input array).

---

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search on the answer space (possible maximum page counts) to efficiently find the minimum maximum pages.

---

**Intuition**

Instead of checking every possible maximum page count linearly, we can use binary search on the answer space.

The key observation is that if we can allocate books to `m` or fewer students with maximum pages `p`, then we can also do it with any larger maximum. This creates a monotonic search space:
- Smaller values might not work (too many students needed)
- Once we find a working value, all larger values also work

We use `cnt <= m` instead of `cnt == m` because:
- If we can allocate books to fewer students than `m`, we have "room" to distribute among more students
- Distributing among more students doesn't increase the maximum pages
- So it's still a valid solution

Since we want to minimize the maximum pages, we need to balance the page distribution. Binary search helps us systematically find the smallest valid maximum page count.

---

**Approach Steps**

1. Check if allocation is possible:
   - If `m > n`, return `-1` (more students than books)

2. Set the search range:
   - `low = max(nums)` (minimum possible maximum pages)
   - `high = sum(nums)` (maximum possible maximum pages)

3. While `low <= high`:
   - Calculate `mid = (low + high) / 2`
   - Count how many students are needed with maximum pages `mid` using greedy allocation
   - If `count <= m`:
     - This is a valid solution
     - Update answer to `mid`
     - Search for smaller values: set `high = mid - 1`
   - If `count > m`:
     - We need a larger maximum page count
     - Set `low = mid + 1`

4. Return the minimum maximum page count found

---

**Example**

Consider `nums = [12, 34, 67, 90]` and `m = 2`:

**Initial state:**
- Check: `m = 2`, `n = 4` → Allocation possible ✓
- `low = 90`, `high = 203`

---

**Iteration 1:**
- `mid = (90 + 203) / 2 = 146`
- Count students with maxPages = 146:
  - Student 1: [12, 34, 67] (sum = 113)
  - Student 2: [90] (sum = 90)
  - Total: 2 students
- 2 ≤ 2 ✓ → Valid solution
- Update: `ans = 146`, `high = 145`

---

**Iteration 2:**
- `mid = (90 + 145) / 2 = 117`
- Count students with maxPages = 117:
  - Student 1: [12, 34, 67] (sum = 113)
  - Student 2: [90] (sum = 90)
  - Total: 2 students
- 2 ≤ 2 ✓ → Valid solution
- Update: `ans = 117`, `high = 116`

---

**Iteration 3:**
- `mid = (90 + 116) / 2 = 103`
- Count students with maxPages = 103:
  - Student 1: [12, 34] (sum = 46), cannot add 67 (would exceed 103)
  - Student 2: [67] (sum = 67), cannot add 90 (would exceed 103)
  - Student 3: [90] (sum = 90)
  - Total: 3 students
- 3 > 2 ✗ → Not valid
- Update: `low = 104`

---

**Iteration 4:**
- `mid = (104 + 116) / 2 = 110`
- Count students with maxPages = 110:
  - Student 1: [12, 34] (sum = 46), cannot add 67 (would exceed 110)
  - Student 2: [67] (sum = 67), cannot add 90 (would exceed 110)
  - Student 3: [90] (sum = 90)
  - Total: 3 students
- 3 > 2 ✗ → Not valid
- Update: `low = 111`

---

**Iteration 5:**
- `mid = (111 + 116) / 2 = 113`
- Count students with maxPages = 113:
  - Student 1: [12, 34, 67] (sum = 113)
  - Student 2: [90] (sum = 90)
  - Total: 2 students
- 2 ≤ 2 ✓ → Valid solution
- Update: `ans = 113`, `high = 112`

---

**Iteration 6:**
- `mid = (111 + 112) / 2 = 111`
- Count students with maxPages = 111:
  - Student 1: [12, 34] (sum = 46), cannot add 67 (would exceed 111)
  - Student 2: [67] (sum = 67), cannot add 90 (would exceed 111)
  - Student 3: [90] (sum = 90)
  - Total: 3 students
- 3 > 2 ✗ → Not valid
- Update: `low = 112`

---

**Iteration 7:**
- `mid = (112 + 112) / 2 = 112`
- Count students with maxPages = 112:
  - Student 1: [12, 34] (sum = 46), cannot add 67 (would exceed 112)
  - Student 2: [67] (sum = 67), cannot add 90 (would exceed 112)
  - Student 3: [90] (sum = 90)
  - Total: 3 students
- 3 > 2 ✗ → Not valid
- Update: `low = 113`

---

**Loop terminates** (low > high)

**Answer = 113**

---

**Code**

```python
def cntPartions(n, arr, maxSum):
    # Track current student's page count
    currentSum = 0
    # Count of students needed
    cnt = 0
    
    for i in range(n):
        # If we can allocate current book to current student
        if(currentSum + arr[i] <= maxSum):
            currentSum = currentSum + arr[i]
        else:
            # Allocate to next student
            cnt += 1
            currentSum = arr[i]
    
    # Count the last student if they have books
    if(currentSum <= maxSum):
        cnt += 1
    return cnt

def optimized(n, arr, k):
    # If more students than books, allocation is impossible
    if(k > n):
        return -1
    
    # Minimum possible max pages is the largest book
    # Maximum possible max pages is the sum of all pages
    low, high = max(arr), sum(arr)
    ans = -1
    
    # Binary search on the answer
    while low <= high:
        mid = (low + high) // 2
        
        # Count students needed with this max page limit
        cnt = cntPartions(n, arr, mid)
        
        # We use cnt <= k instead of cnt == k because:
        # If we can allocate to fewer students, we can always
        # distribute among more students without increasing the maximum
        # Since we want minimum max pages, we balance page distribution
        if(cnt <= k):
            ans = mid
            # Try for a smaller maximum page count
            high = mid - 1
        else:
            # Need a larger maximum page count
            low = mid + 1
    return ans
           
    
books = [12, 34, 67, 90]
n = len(books)
m = 2
arr, k = books, m
print(optimized(n, arr, k))
```

---

**Time Complexity**

O(n × log(sum - max)) — Binary search takes O(log(sum - max)) iterations, and each iteration requires O(n) time to count partitions.

---

**Space Complexity**

O(1) — Only using a constant amount of extra space (excluding the input array).

---

# The Painter's Partition Problem-II

## Problem Description

Given an array `arr[]` where each element denotes the length of a board, and an integer `k` representing the number of painters available. Each painter takes 1 unit of time to paint 1 unit length of a board.

Determine the minimum amount of time required to paint all the boards, under the constraint that each painter can paint only a contiguous sequence of boards (no skipping or splitting allowed).

**Notes on constraints:**
- Each painter must paint a contiguous sequence of boards
- All painters work simultaneously (in parallel)
- The total time is determined by the painter who takes the longest time
- We need to minimize the maximum time taken by any single painter

---

## Sample Test Cases With Explanation

### Test Case 1
```
Input: arr[] = [5, 10, 30, 20, 15], k = 3
Output: 35
```
**Explanation:** The optimal allocation of boards among 3 painters is:
- Painter 1 → [5, 10] → time = 15
- Painter 2 → [30] → time = 30
- Painter 3 → [20, 15] → time = 35

Job will be done when all painters finish, i.e., at time = max(15, 30, 35) = 35.

### Test Case 2
```
Input: arr[] = [10, 20, 30, 40], k = 2
Output: 60
```
**Explanation:** A valid optimal partition is:
- Painter 1 → [10, 20, 30] → time = 60
- Painter 2 → [40] → time = 40

Job will be complete at time = max(60, 40) = 60.

### Test Case 3
```
Input: arr[] = [100, 200, 300, 400], k = 1
Output: 1000
```
**Explanation:** There is only one painter, so the painter must paint all boards sequentially. The total time taken will be the sum of all board lengths, i.e., 100 + 200 + 300 + 400 = 1000.

---

## Approaches

### Approach 1: Linear Search

**Summary:** Try every possible maximum time from the longest board to the total length, and find the smallest time that allows completing the job with k or fewer painters.

---

**Intuition**

The answer lies between:
- `max(arr)` — The minimum possible time (when we have enough painters so that the longest board determines the minimum time)
- `sum(arr)` — The maximum possible time (when one painter paints all boards)

For any given maximum time limit, we can greedily determine how many painters are needed by assigning boards consecutively until adding the next board would exceed the time limit.

We use `cnt <= k` instead of `cnt == k` because:
- If we can complete the job with fewer painters, we can always use more painters without increasing the maximum time
- Additional painters can take smaller portions, keeping the maximum time the same or lower

Since we want to minimize the maximum time, we need to balance the workload distribution by checking every possibility from smallest to largest until we find the first valid answer.

---

**Approach Steps**

1. Set the search range:
   - `low = max(arr)` (minimum possible maximum time - when each painter gets at most the longest board)
   - `high = sum(arr)` (maximum possible maximum time - when one painter paints all boards)

2. For each possible maximum time from `low` to `high`:
   - Count how many painters are needed if no painter takes more than this maximum time
   - Use a greedy approach: keep assigning boards to the current painter until adding the next board would exceed the maximum time
   - If the count of painters is ≤ k, this is a valid answer
   - Return the first valid maximum time found (this will be the minimum)

3. Return the answer

---

**Example**

Consider `arr = [5, 10, 30, 20, 15]` and `k = 3`:

**Step 1:** Initialize search range
- `low = 30` (max element - longest board)
- `high = 80` (sum of all lengths)

**Step 2:** Try maxTime = 30
- Painter 1: [5, 10] (time = 15), cannot add 30 (would make time = 45 > 30)
- Painter 2: [30] (time = 30), cannot add 20 (would make time = 50 > 30)
- Painter 3: [20] (time = 20), cannot add 15 (would make time = 35 > 30)
- Painter 4: [15] (time = 15)
- Result: 4 painters needed
- 4 > 3, so this doesn't work ✗

**Step 3:** Try maxTime = 31, 32, 33, 34 (similar failures)

**Step 4:** Try maxTime = 35
- Painter 1: [5, 10] (time = 15), cannot add 30 (would make time = 45 > 35)
- Painter 2: [30] (time = 30), cannot add 20 (would make time = 50 > 35)
- Painter 3: [20, 15] (time = 35)
- Result: 3 painters needed
- 3 ≤ 3, so this works ✓

**Answer = 35**

---

**Code**

```python
def cntPartions(n, arr, maxSum):
    # Track current painter's time
    currentSum = 0
    # Count of painters needed
    cnt = 0
    
    for i in range(n):
        # If we can assign current board to current painter
        if(currentSum + arr[i] <= maxSum):
            currentSum = currentSum + arr[i]
        else:
            # Assign to next painter
            cnt += 1
            currentSum = arr[i]
    
    # Count the last painter if they have boards
    if(currentSum <= maxSum):
        cnt += 1
    return cnt
    
def solve(n, arr, k):
    # Minimum possible max time is the longest board
    # Maximum possible max time is the sum of all boards
    low, high = max(arr), sum(arr)
    ans = low
    
    # Try every possible maximum time
    for maxSum in range(low, high + 1):
        # Count painters needed with this max time limit
        cnt = cntPartions(n, arr, maxSum)
        
        # We use cnt <= k instead of cnt == k because:
        # If we can complete with fewer painters, we can always
        # use more painters without increasing the maximum time
        if(cnt <= k):
            ans = maxSum
            break
    return ans
           
    
arr = [12, 34, 67, 90]
n = len(arr)
k = 2
print(solve(n, arr, k))
```

---

**Time Complexity**

O(n × (sum - max)) — For each possible maximum time (which can range from max board to total length), we count partitions in O(n) time. In the worst case, this range can be large.

---

**Space Complexity**

O(1) — Only using a constant amount of extra space (excluding the input array).

---

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search on the answer space (possible maximum times) to efficiently find the minimum maximum time.

---

**Intuition**

Instead of checking every possible maximum time linearly, we can use binary search on the answer space.

The key observation is that if we can complete the job with `k` or fewer painters in maximum time `t`, then we can also do it with any larger time. This creates a monotonic search space:
- Smaller times might not work (too many painters needed)
- Once we find a working time, all larger times also work

We use `cnt <= k` instead of `cnt == k` because:
- If we can complete the job with fewer painters than `k`, we have "room" to use more painters
- Using more painters doesn't increase the maximum time (they can split work further)
- So it's still a valid solution

Since we want to minimize the maximum time, we need to balance the workload distribution. Binary search helps us systematically find the smallest valid maximum time.

---

**Approach Steps**

1. Set the search range:
   - `low = max(arr)` (minimum possible maximum time)
   - `high = sum(arr)` (maximum possible maximum time)

2. While `low <= high`:
   - Calculate `mid = (low + high) / 2`
   - Count how many painters are needed with maximum time `mid` using greedy assignment
   - If `count <= k`:
     - This is a valid solution
     - Update answer to `mid`
     - Search for smaller times: set `high = mid - 1`
   - If `count > k`:
     - We need a larger maximum time
     - Set `low = mid + 1`

3. Return the minimum maximum time found

---

**Example**

Consider `arr = [5, 10, 30, 20, 15]` and `k = 3`:

**Initial state:**
- `low = 30`, `high = 80`

---

**Iteration 1:**
- `mid = (30 + 80) / 2 = 55`
- Count painters with maxTime = 55:
  - Painter 1: [5, 10, 30] (time = 45)
  - Painter 2: [20, 15] (time = 35)
  - Total: 2 painters
- 2 ≤ 3 ✓ → Valid solution
- Update: `ans = 55`, `high = 54`

---

**Iteration 2:**
- `mid = (30 + 54) / 2 = 42`
- Count painters with maxTime = 42:
  - Painter 1: [5, 10] (time = 15), cannot add 30 (would make time = 45 > 42)
  - Painter 2: [30] (time = 30), cannot add 20 (would make time = 50 > 42)
  - Painter 3: [20, 15] (time = 35)
  - Total: 3 painters
- 3 ≤ 3 ✓ → Valid solution
- Update: `ans = 42`, `high = 41`

---

**Iteration 3:**
- `mid = (30 + 41) / 2 = 35`
- Count painters with maxTime = 35:
  - Painter 1: [5, 10] (time = 15), cannot add 30 (would make time = 45 > 35)
  - Painter 2: [30] (time = 30), cannot add 20 (would make time = 50 > 35)
  - Painter 3: [20, 15] (time = 35)
  - Total: 3 painters
- 3 ≤ 3 ✓ → Valid solution
- Update: `ans = 35`, `high = 34`

---

**Iteration 4:**
- `mid = (30 + 34) / 2 = 32`
- Count painters with maxTime = 32:
  - Painter 1: [5, 10] (time = 15), cannot add 30 (would make time = 45 > 32)
  - Painter 2: [30] (time = 30), cannot add 20 (would make time = 50 > 32)
  - Painter 3: [20] (time = 20), cannot add 15 (would make time = 35 > 32)
  - Painter 4: [15] (time = 15)
  - Total: 4 painters
- 4 > 3 ✗ → Not valid
- Update: `low = 33`

---

**Iteration 5:**
- `mid = (33 + 34) / 2 = 33`
- Count painters with maxTime = 33:
  - Painter 1: [5, 10] (time = 15), cannot add 30 (would make time = 45 > 33)
  - Painter 2: [30] (time = 30), cannot add 20 (would make time = 50 > 33)
  - Painter 3: [20] (time = 20), cannot add 15 (would make time = 35 > 33)
  - Painter 4: [15] (time = 15)
  - Total: 4 painters
- 4 > 3 ✗ → Not valid
- Update: `low = 34`

---

**Iteration 6:**
- `mid = (34 + 34) / 2 = 34`
- Count painters with maxTime = 34:
  - Painter 1: [5, 10] (time = 15), cannot add 30 (would make time = 45 > 34)
  - Painter 2: [30] (time = 30), cannot add 20 (would make time = 50 > 34)
  - Painter 3: [20] (time = 20), cannot add 15 (would make time = 35 > 34)
  - Painter 4: [15] (time = 15)
  - Total: 4 painters
- 4 > 3 ✗ → Not valid
- Update: `low = 35`

---

**Loop terminates** (low > high)

**Answer = 35**

---

**Code**

```python
def cntPartions(n, arr, maxSum):
    # Track current painter's time
    currentSum = 0
    # Count of painters needed
    cnt = 0
    
    for i in range(n):
        # If we can assign current board to current painter
        if(currentSum + arr[i] <= maxSum):
            currentSum = currentSum + arr[i]
        else:
            # Assign to next painter
            cnt += 1
            currentSum = arr[i]
    
    # Count the last painter if they have boards
    if(currentSum <= maxSum):
        cnt += 1
    return cnt

def optimized(n, arr, k):
    # Minimum possible max time is the longest board
    # Maximum possible max time is the sum of all boards
    low, high = max(arr), sum(arr)
    ans = low
    
    # Binary search on the answer
    while low <= high:
        mid = (low + high) // 2
        
        # Count painters needed with this max time limit
        cnt = cntPartions(n, arr, mid)
        
        # We use cnt <= k instead of cnt == k because:
        # If we can complete with fewer painters, we can always
        # use more painters without increasing the maximum time
        # Since we want minimum max time, we balance workload distribution
        if(cnt <= k):
            ans = mid
            # Try for a smaller maximum time
            high = mid - 1
        else:
            # Need a larger maximum time
            low = mid + 1
    return ans
           
    
arr = [12, 34, 67, 90]
n = len(arr)
k = 2
print(optimized(n, arr, k))
```

---

**Time Complexity**

O(n × log(sum - max)) — Binary search takes O(log(sum - max)) iterations, and each iteration requires O(n) time to count partitions.

---

**Space Complexity**

O(1) — Only using a constant amount of extra space (excluding the input array).

---

# Minimize Max Distance to Gas Station

## Problem Description

Given a sorted array `arr` of size `n`, containing integer positions of `n` gas stations on the X-axis, and an integer `k`, place `k` new gas stations on the X-axis.

The new gas stations can be placed anywhere on the non-negative side of the X-axis, including non-integer positions.

Let `dist` be the maximum distance between adjacent gas stations after adding the `k` new gas stations. Find the minimum value of `dist`.

Your answer will be accepted if it is within `1e-6` of the true value.

**Notes on constraints:**
- New gas stations can be placed at non-integer positions (floating-point coordinates)
- The goal is to minimize the maximum gap between any two adjacent gas stations
- We need to optimally distribute `k` new stations to balance the gaps
- The answer requires precision up to `1e-6`

---

## Sample Test Cases With Explanation

### Test Case 1
```
Input: n = 10, arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 9
Output: 0.50000
```
**Explanation:** One of the possible ways to place 9 new gas stations is [1, 1.5, 2, 2.5, 3, 3.5, 4, 4.5, 5, 5.5, 6, 6.5, 7, 7.5, 8, 8.5, 9, 9.5, 10]. Thus the maximum difference between adjacent gas stations is 0.5. Hence, the value of dist is 0.5. It can be shown that there is no possible way to add 9 gas stations in such a way that the value of dist is lower than this.

### Test Case 2
```
Input: n = 10, arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 1
Output: 1.00000
```
**Explanation:** One of the possible ways to place 1 gas station is [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]. The new gas station is at position 11. Thus the maximum difference between adjacent gas stations is still 1. Hence, the value of dist is 1. It can be shown that there is no possible way to add 1 gas station in such a way that the value of dist is lower than this.

---

## Approaches

### Approach 1: Greedy - Repeatedly Split Largest Gap

**Summary:** For each of the k new stations, find the current largest gap between adjacent stations and place a new station to split it, reducing the maximum gap iteratively.

---

**Intuition**

To minimize the maximum distance, we should always target the largest current gap and split it by adding a new gas station. By repeatedly splitting the largest gap, we progressively reduce the maximum distance.

For each gap between stations at positions `a` and `b`, if we place `m` new stations in this gap, they divide it into `(m+1)` equal sections, each of length `(b-a)/(m+1)`.

We maintain a count of how many new stations have been placed in each original gap, and iteratively add stations to the gap that currently has the largest section length.

---

**Approach Steps**

1. Initialize a `splitCnt` array of size `(n-1)` to track how many new stations are placed in each gap (initially all zeros)

2. For each of the `k` new stations:
   - Iterate through all gaps to find the one with the maximum section length
   - Section length for gap `i` = `(arr[i+1] - arr[i]) / (splitCnt[i] + 1)`
   - Increment `splitCnt` for the gap with maximum section length

3. After placing all `k` stations:
   - Calculate the maximum section length across all gaps
   - This is our answer

---

**Example**

Consider `arr = [1, 2, 3, 4, 5]` and `k = 2`:

**Initial state:**
- Gaps: [1-2], [2-3], [3-4], [4-5]
- All gaps have length 1
- `splitCnt = [0, 0, 0, 0]`

**Place 1st new station:**
- All gaps have section length = 1/1 = 1.0
- Choose any gap, say gap 0 (between 1 and 2)
- `splitCnt = [1, 0, 0, 0]`
- Gap 0 now has section length = 1/2 = 0.5

**Place 2nd new station:**
- Gap 0: section length = 1/2 = 0.5
- Gap 1: section length = 1/1 = 1.0 (maximum)
- Gap 2: section length = 1/1 = 1.0 (maximum)
- Gap 3: section length = 1/1 = 1.0 (maximum)
- Choose gap 1, `splitCnt = [1, 1, 0, 0]`

**Final calculation:**
- Gap 0: 1/2 = 0.5
- Gap 1: 1/2 = 0.5
- Gap 2: 1/1 = 1.0
- Gap 3: 1/1 = 1.0
- Maximum = 1.0

**Answer = 1.0**

---

**Code**

```python
def solve(n, arr, k):
    # Track how many new stations placed in each gap
    splitCnt = [0] * (n - 1)
    
    # Place k new stations one by one
    for _ in range(k):
        maxSection, maxIndex = -1, -1
        
        # Find the gap with maximum section length
        for i in range(n - 1):
            gap = arr[i + 1] - arr[i]
            # Section length if splitCnt[i] stations already placed
            sectionLen = gap / (splitCnt[i] + 1)
            
            if(sectionLen > maxSection):
                maxSection = sectionLen
                maxIndex = i
        
        # Place new station in the gap with max section length
        splitCnt[maxIndex] += 1
    
    # Calculate final maximum section length
    maxLen = 0
    for i in range(n - 1):
        gap = arr[i + 1] - arr[i]
        sectionLen = gap / (splitCnt[i] + 1)
        maxLen = max(maxLen, sectionLen)
        
    return maxLen

arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
n = len(arr)
k = 9
print(solve(n, arr, k))
```

---

**Time Complexity**

O(k × n) — For each of the `k` stations, we iterate through all `n-1` gaps to find the maximum, taking O(n) time per station.

---

**Space Complexity**

O(n) — We use a `splitCnt` array of size `n-1` to track placements in each gap.

---

### Approach 2: Greedy with Max Heap

**Summary:** Use a max heap (priority queue) to efficiently find and update the largest gap, reducing the time to find the maximum gap from O(n) to O(log n).

---

**Intuition**

The previous approach spends O(n) time finding the maximum gap in each iteration. We can optimize this using a max heap to maintain gaps sorted by their section lengths.

Initially, push all gaps with their lengths into the heap. For each new station:
1. Extract the gap with the maximum section length from the heap
2. Place a new station in that gap
3. Recalculate the section length for that gap
4. Push the updated section length back into the heap

After placing all `k` stations, the maximum section length will be at the top of the heap.

---

**Approach Steps**

1. Initialize:
   - `splitCnt` array of size `(n-1)` to track placements
   - Max heap (priority queue) to store `(-sectionLength, index)` pairs

2. For each gap, calculate initial section length and push into heap:
   - Section length = `gap / 1` (no splits initially)
   - Use negative values for max heap behavior in Python

3. For each of the `k` new stations:
   - Pop the gap with maximum section length from heap
   - Increment `splitCnt` for that gap
   - Recalculate section length = `gap / (splitCnt[index] + 1)`
   - Push updated section length back into heap

4. After placing all stations, the top of heap contains the maximum section length (negate to get positive value)

---

**Example**

Consider `arr = [1, 3, 6]` and `k = 2`:

**Initial state:**
- Gaps: [1-3] (length 2), [3-6] (length 3)
- Heap: [(-3, 1), (-2, 0)]
- `splitCnt = [0, 0]`

**Place 1st new station:**
- Pop: (-3, 1) → Gap 1 has max section length 3.0
- `splitCnt = [0, 1]`
- Gap 1 new section length = 3/2 = 1.5
- Push: (-1.5, 1)
- Heap: [(-2, 0), (-1.5, 1)]

**Place 2nd new station:**
- Pop: (-2, 0) → Gap 0 has max section length 2.0
- `splitCnt = [1, 1]`
- Gap 0 new section length = 2/2 = 1.0
- Push: (-1.0, 0)
- Heap: [(-1.5, 1), (-1.0, 0)]

**Final result:**
- Top of heap: -1.5
- Answer = 1.5

---

**Code**

```python
import heapq

def solve2(n, arr, k):
    # Track how many new stations placed in each gap
    splitCnt = [0] * (n - 1)
    # Max heap to efficiently find largest gap
    pq = []
    
    # Initialize heap with all gaps
    for i in range(n - 1):
        gap = arr[i + 1] - arr[i]
        # Use negative for max heap (Python has min heap by default)
        heapq.heappush(pq, (-gap, i))
    
    # Place k new stations
    for _ in range(k):
        # Get gap with maximum section length
        _, index = heapq.heappop(pq)
        # Place new station in this gap
        splitCnt[index] += 1
        
        # Recalculate section length for this gap
        gap = arr[index + 1] - arr[index]
        sectionLen = gap / (splitCnt[index] + 1)
        # Push updated section length back to heap
        heapq.heappush(pq, (-sectionLen, index))
    
    # Maximum section length is at top of heap
    return -pq[0][0]

arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
n = len(arr)
k = 9
print(solve2(n, arr, k))
```

---

**Time Complexity**

O(k × log n) — For each of the `k` stations, we perform heap operations (pop and push) which take O(log n) time. Initial heap construction takes O(n log n).

---

**Space Complexity**

O(n) — We use a `splitCnt` array and a heap, both of size proportional to `n`.

---

### Approach 3: Binary Search on Answer (Optimized)

**Summary:** Use binary search on the possible values of the maximum distance to find the minimum distance that requires at most k new stations.

---

**Intuition**

Instead of directly placing stations, we can binary search on the answer (the maximum distance). For a given distance `d`, we can calculate how many new stations are needed to ensure no gap exceeds `d`.

For each gap of length `gap`, the number of new stations needed = `ceil(gap/d) - 1`:
- If `gap = 6` and `d = 2`, we need `ceil(6/2) - 1 = 3 - 1 = 2` new stations
- This divides the gap into 3 sections: [0-2], [2-4], [4-6]

The key observation is:
- If we can achieve maximum distance `d` with ≤ `k` stations, we can also achieve any larger distance with ≤ `k` stations
- This creates a monotonic search space perfect for binary search

We search for the smallest distance `d` such that the total number of required stations is ≤ `k`.

---

**Approach Steps**

1. Find the search range:
   - `low = 0` (theoretical minimum)
   - `high = max gap` (maximum gap in original array)

2. Binary search with precision `1e-6`:
   - While `(high - low) >= 1e-6`:
     - Calculate `mid = (low + high) / 2.0`
     - Count how many new stations needed for max distance `mid`
     - For each gap: `stations_needed = ceil(gap/mid) - 1`
     - If total stations ≤ k: this distance is achievable, try smaller (set `high = mid`)
     - Else: need larger distance (set `low = mid`)

3. Return the answer

---

**Example**

Consider `arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]` and `k = 9`:

**Initial state:**
- Max gap = 1 (all gaps are of length 1)
- `low = 0`, `high = 1.0`

**Iteration 1:**
- `mid = 0.5`
- Count stations needed:
  - Each gap of length 1 needs `ceil(1/0.5) - 1 = 2 - 1 = 1` station
  - Total gaps = 9, so total stations = 9
- 9 ≤ 9 ✓ → Achievable
- Update: `ans = 0.5`, `high = 0.5`

**Iteration 2:**
- `mid = 0.25`
- Count stations needed:
  - Each gap needs `ceil(1/0.25) - 1 = 4 - 1 = 3` stations
  - Total = 9 × 3 = 27 stations
- 27 > 9 ✗ → Not achievable
- Update: `low = 0.25`

**Iteration 3:**
- `mid = 0.375`
- Count stations needed:
  - Each gap needs `ceil(1/0.375) - 1 = 3 - 1 = 2` stations
  - Total = 9 × 2 = 18 stations
- 18 > 9 ✗ → Not achievable
- Update: `low = 0.375`

**Continue iterations until convergence...**

**Answer ≈ 0.5**

---

**Code**

```python
import math

def cntSections(n, arr, dist):
    # Count total new stations needed for max distance 'dist'
    cnt = 0
    for i in range(n - 1):
        gap = arr[i + 1] - arr[i]
        # Number of new stations needed in this gap
        # ceil(gap/dist) gives number of sections
        # Subtract 1 to get number of new stations
        cnt += math.ceil(gap / dist) - 1
    return cnt

def optimized(n, arr, k):
    # Find maximum gap in original array
    maxGap = 0
    for i in range(n - 1):
        maxGap = max(maxGap, (arr[i + 1] - arr[i]))
    
    # Binary search on the answer
    low, high = 0, maxGap
    ans = maxGap
    
    # Continue until precision of 1e-6 is achieved
    while (high - low) >= (1e-6):
        mid = (low + high) / 2.0
        
        # Count stations needed for max distance 'mid'
        cnt = cntSections(n, arr, mid)
        
        # If we can achieve this distance with <= k stations
        if(cnt <= k):
            ans = mid
            # Try for smaller distance
            high = mid
        else:
            # Need larger distance
            low = mid
    
    return ans

arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
n = len(arr)
k = 9
print(optimized(n, arr, k))
```

---

**Time Complexity**

O(n × log(maxGap / 1e-6)) — Binary search takes O(log(maxGap / 1e-6)) iterations (determined by precision requirement), and each iteration requires O(n) time to count stations needed across all gaps.

---

**Space Complexity**

O(1) — Only using a constant amount of extra space (excluding the input array).

---
