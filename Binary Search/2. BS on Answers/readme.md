# Find Square Root of a Number

## Problem Description

Given a positive integer `n`, find and return its square root. If `n` is not a perfect square, then return the floor value of `sqrt(n)`.

**Notes on constraints:**
- For large values of n, a linear search approach will be too slow. Consider using binary search for O(log n) time complexity.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: n = 36
Output: 6
```
**Explanation:** 6 is the square root of 36 because 6 × 6 = 36.

**Test Case 2:**
```
Input: n = 28
Output: 5
```
**Explanation:** The square root of 28 is approximately 5.292. Since 28 is not a perfect square, we return the floor value, which is 5 (because 5 × 5 = 25 ≤ 28, but 6 × 6 = 36 > 28).

**Test Case 3:**
```
Input: n = 16
Output: 4
```
**Explanation:** 4 is the square root of 16 because 4 × 4 = 16.

## Approaches

### Approach 1: Linear Search

**Summary:** Check every number from 0 to n to find the largest number whose square is less than or equal to n.

**Intuition**

The simplest approach is to iterate through all numbers starting from 0 and check if their square is less than or equal to n. We keep updating our answer as long as the condition holds. Once we find a number whose square exceeds n, we return the previous valid answer.

**Approach Steps**

1. Initialize `ans = 0` to store the result
2. Iterate through all numbers from 0 to n
3. For each number, check if `num * num <= n`
4. If true, update `ans = num` (this could be our answer)
5. If false, return the current `ans` (we've gone too far)
6. Return `ans` after the loop completes

**Example**

Let's find the square root of n = 28:
- num = 0: 0 × 0 = 0 ≤ 28, so ans = 0
- num = 1: 1 × 1 = 1 ≤ 28, so ans = 1
- num = 2: 2 × 2 = 4 ≤ 28, so ans = 2
- num = 3: 3 × 3 = 9 ≤ 28, so ans = 3
- num = 4: 4 × 4 = 16 ≤ 28, so ans = 4
- num = 5: 5 × 5 = 25 ≤ 28, so ans = 5
- num = 6: 6 × 6 = 36 > 28, return ans = 5

**Code**

```python
def solve(n):
    # Initialize answer variable to store the floor of square root
    ans=0
    # Iterate through all numbers from 0 to n
    for num in range(n+1):
        # Check if the square of current number is less than or equal to n
        if((num*num)<=n):
            # Update answer as this could be the floor of square root
            ans=num
        else:
            # If square exceeds n, return the previous valid answer
            return ans
    # Return the answer if loop completes
    return ans
```

**Time Complexity**

O(n) — We iterate through all numbers from 0 to n in the worst case.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search on the range [0, n] to efficiently find the floor value of the square root.

**Intuition**

Since we're searching for a number in a sorted range where squares are monotonically increasing, binary search is ideal. We search for the largest number whose square is less than or equal to n. This reduces our search space by half in each iteration.

**Approach Steps**

1. Initialize `low = 0`, `high = n + 1`, and `ans = 0`
2. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - If `mid * mid <= n`, this could be our answer:
     - Update `ans = mid`
     - Search for a potentially larger answer: `low = mid + 1`
   - Otherwise, `mid` is too large:
     - Search in the left half: `high = mid - 1`
3. Return `ans`

**Example**

Let's find the square root of n = 28:
- Initial: low = 0, high = 29, ans = 0
- Iteration 1: mid = 14, 14 × 14 = 196 > 28, so high = 13
- Iteration 2: mid = 6, 6 × 6 = 36 > 28, so high = 5
- Iteration 3: mid = 2, 2 × 2 = 4 ≤ 28, so ans = 2, low = 3
- Iteration 4: mid = 4, 4 × 4 = 16 ≤ 28, so ans = 4, low = 5
- Iteration 5: mid = 5, 5 × 5 = 25 ≤ 28, so ans = 5, low = 6
- low > high, return ans = 5

**Code**

```python
def optimized(n):
    # Store the target value
    sqrt=n
    # Initialize binary search bounds: low=0, high=n+1
    low,high=0,sqrt+1
    # Variable to store the answer (floor of square root)
    ans=0
    # Binary search loop
    while low<=high:
        # Calculate middle point
        mid=(low+high)//2
        # Check if mid's square is less than or equal to target
        if(mid*mid<=sqrt):
            # Update answer as mid could be the floor of square root
            ans=mid
            # Search in right half for potentially larger answer
            low=mid+1
        else:
            # mid's square is too large, search in left half
            high=mid-1
    # Return the floor of square root
    return ans
```

**Time Complexity**

O(log n) — Binary search divides the search space in half with each iteration.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

---

# Find Nth Root of a Number

## Problem Description

Given two numbers N and M, find the Nth root of M. The Nth root of a number M is defined as a number X such that when X is raised to the power of N, it equals M. If the Nth root is not an integer, return -1.

**Notes on constraints:**
- For large values of M, a linear search will be inefficient. Binary search provides O(log M) time complexity.
- The Nth root must be an exact integer; otherwise, return -1.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: N = 3, M = 27
Output: 3
```
**Explanation:** The cube root of 27 is 3 because 3³ = 27.

**Test Case 2:**
```
Input: N = 4, M = 69
Output: -1
```
**Explanation:** The 4th root of 69 does not exist as an integer. The actual value is approximately 2.88, so we return -1.

**Test Case 3:**
```
Input: N = 2, M = 16
Output: 4
```
**Explanation:** The square root of 16 is 4 because 4² = 16.

## Approaches

### Approach 1: Linear Search

**Summary:** Check every number from 0 to M to find if any number raised to power N equals M.

**Intuition**

We iterate through all possible candidates from 0 to M and check if raising each number to the Nth power equals M. If we find such a number, it's our answer. If we exceed M without finding an exact match, the Nth root doesn't exist as an integer, so we return -1.

**Approach Steps**

1. Iterate through all numbers from 0 to M
2. For each number, calculate `temp = num^N`
3. If `temp == M`, return `num` (exact Nth root found)
4. If `temp > M`, return -1 (we've exceeded M without finding a match)
5. If loop completes without finding a match, return -1

**Example**

Let's find the 3rd root (cube root) of M = 27:
- num = 0: 0³ = 0 < 27, continue
- num = 1: 1³ = 1 < 27, continue
- num = 2: 2³ = 8 < 27, continue
- num = 3: 3³ = 27 = 27, return 3 ✓

Now let's find the 4th root of M = 69:
- num = 0: 0⁴ = 0 < 69, continue
- num = 1: 1⁴ = 1 < 69, continue
- num = 2: 2⁴ = 16 < 69, continue
- num = 3: 3⁴ = 81 > 69, return -1 (no exact match)

**Code**

```python
def solve(n,m):
    # Iterate through all possible candidates from 0 to m
    for num in range(m+1):
        # Calculate num raised to the power n
        temp=(num**n)
        # If temp equals m, we found the exact Nth root
        if(temp==m):
            return num
        # If temp exceeds m, Nth root doesn't exist as an integer
        elif(temp>m):
            return -1
    # Return -1 if no exact Nth root found
    return -1
```

**Time Complexity**

O(M) — In the worst case, we iterate through all numbers from 0 to M.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search on the range [0, M] to efficiently find the Nth root.

**Intuition**

Since the function f(x) = x^N is monotonically increasing for positive x, we can use binary search. We search for a number whose Nth power exactly equals M. If we find it, that's our answer; otherwise, return -1.

**Approach Steps**

1. Initialize `low = 0`, `high = M + 1`
2. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - Calculate `temp = mid^N`
   - If `temp == M`, return `mid` (exact match found)
   - If `temp < M`, search in the right half: `low = mid + 1`
   - If `temp > M`, search in the left half: `high = mid - 1`
3. If no exact match is found, return -1

**Example**

Let's find the 3rd root of M = 27:
- Initial: low = 0, high = 28
- Iteration 1: mid = 14, 14³ = 2744 > 27, so high = 13
- Iteration 2: mid = 6, 6³ = 216 > 27, so high = 5
- Iteration 3: mid = 2, 2³ = 8 < 27, so low = 3
- Iteration 4: mid = 4, 4³ = 64 > 27, so high = 3
- Iteration 5: mid = 3, 3³ = 27 = 27, return 3 ✓

**Code**

```python
def optimized(n,m):
    # Store the target value
    sqrt=m
    # Initialize binary search bounds: low=0, high=m+1
    low,high=0,sqrt+1
    # Binary search loop
    while low<=high:
        # Calculate middle point
        mid=(low+high)//2
        # Calculate mid raised to the power n
        temp=mid**n
        # If temp equals target, we found the exact Nth root
        if(temp==sqrt):
            return mid
        # If temp is less than target, search in right half
        elif(temp<sqrt):
            low=mid+1
        # If temp is greater than target, search in left half
        else:
            high=mid-1
    # Return -1 if no exact Nth root exists
    return -1
```

**Time Complexity**

O(log M × log N) — Binary search takes O(log M) iterations, and computing mid^N takes O(log N) time for exponentiation.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

---

# Find the Smallest Divisor

## Problem Description

Given an array of integers `nums` and an integer `limit` as the threshold value, find the smallest positive integer divisor such that upon dividing all the elements of the array by this divisor, the sum of the division results is less than or equal to the threshold value.

After dividing each element by the chosen divisor, take the ceiling of the result (i.e., round up to the next whole number).

**Notes on constraints:**
- For large arrays and large maximum values, a linear search will be inefficient. Binary search provides O(n log max(nums)) time complexity.
- The divisor must result in a sum ≤ limit.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: nums = [1, 2, 3, 4, 5], limit = 8
Output: 3
```
**Explanation:** With divisor 1, sum = 15 (too high). With divisor 2, sum = ⌈1/2⌉ + ⌈2/2⌉ + ⌈3/2⌉ + ⌈4/2⌉ + ⌈5/2⌉ = 1 + 1 + 2 + 2 + 3 = 9 (still too high). With divisor 3, sum = ⌈1/3⌉ + ⌈2/3⌉ + ⌈3/3⌉ + ⌈4/3⌉ + ⌈5/3⌉ = 1 + 1 + 1 + 2 + 2 = 7 ≤ 8, so 3 is the minimum divisor.

**Test Case 2:**
```
Input: nums = [8, 4, 2, 3], limit = 10
Output: 2
```
**Explanation:** With divisor 1, sum = 17 (too high). With divisor 2, sum = ⌈8/2⌉ + ⌈4/2⌉ + ⌈2/2⌉ + ⌈3/2⌉ = 4 + 2 + 1 + 2 = 9 ≤ 10, so 2 is the minimum divisor.

## Approaches

### Approach 1: Linear Search

**Summary:** Try every divisor from 1 to max(nums) and return the first one that satisfies the condition.

**Intuition**

The simplest approach is to iterate through all possible divisors starting from 1 up to the maximum element in the array. For each divisor, we calculate the sum of ceiling divisions and check if it's within the limit. The first divisor that satisfies the condition is our answer.

**Approach Steps**

1. Find the maximum element in the array (this is the upper bound for divisors)
2. Iterate through divisors from 1 to max(nums)
3. For each divisor, use a helper function to check if the sum of ceiling divisions ≤ limit
4. Return the first divisor that satisfies the condition
5. Return -1 if no valid divisor is found

**Example**

Let's find the smallest divisor for nums = [8, 4, 2, 3], limit = 10:
- Divisor 1: ⌈8/1⌉ + ⌈4/1⌉ + ⌈2/1⌉ + ⌈3/1⌉ = 8 + 4 + 2 + 3 = 17 > 10 ✗
- Divisor 2: ⌈8/2⌉ + ⌈4/2⌉ + ⌈2/2⌉ + ⌈3/2⌉ = 4 + 2 + 1 + 2 = 9 ≤ 10 ✓
- Return 2

**Code**

```python
import math


def check(n,arr,divisor,limit):
    # Initialize total sum to 0
    total=0
    # Iterate through all elements in the array
    for i in range(n):
        # Add the ceiling of arr[i]/divisor to the total
        total+=math.ceil(arr[i]/divisor)
    # Return True if total is within the limit, False otherwise
    return total<=limit


def solve(n,arr,limit):
    # Find the maximum element in the array (upper bound for divisor)
    maxi=max(arr)
    # Try each divisor from 1 to maximum element
    for num in range(1,maxi+1):
        # Check if current divisor satisfies the condition
        temp=check(n,arr,num,limit)
        if(temp):
            # Return the first divisor that works
            return num
    # Return -1 if no valid divisor found
    return -1
```

**Time Complexity**

O(n × max(nums)) — We iterate through max(nums) divisors, and for each divisor, we compute the sum in O(n) time.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search on the range [1, max(nums)] to efficiently find the smallest valid divisor.

**Intuition**

As the divisor increases, the sum of ceiling divisions decreases. This monotonic property allows us to use binary search. We search for the smallest divisor that satisfies the condition by narrowing down the search space efficiently.

**Approach Steps**

1. Find the maximum element in the array (upper bound for divisor)
2. Initialize `low = 1`, `high = max(nums) + 1`, and `ans = -1`
3. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - Check if mid as a divisor satisfies the condition
   - If yes, update `ans = mid` and search for a smaller divisor: `high = mid - 1`
   - If no, search for a larger divisor: `low = mid + 1`
4. Return `ans`

**Example**

Let's find the smallest divisor for nums = [8, 4, 2, 3], limit = 10:
- Initial: low = 1, high = 9, ans = -1
- Iteration 1: mid = 5, sum = ⌈8/5⌉ + ⌈4/5⌉ + ⌈2/5⌉ + ⌈3/5⌉ = 2 + 1 + 1 + 1 = 5 ≤ 10 ✓, ans = 5, high = 4
- Iteration 2: mid = 2, sum = ⌈8/2⌉ + ⌈4/2⌉ + ⌈2/2⌉ + ⌈3/2⌉ = 4 + 2 + 1 + 2 = 9 ≤ 10 ✓, ans = 2, high = 1
- Iteration 3: mid = 1, sum = 8 + 4 + 2 + 3 = 17 > 10 ✗, low = 2
- low > high, return ans = 2

**Code**

```python
import math


def check(n,arr,divisor,limit):
    # Initialize total sum to 0
    total=0
    # Iterate through all elements in the array
    for i in range(n):
        # Add the ceiling of arr[i]/divisor to the total
        total+=math.ceil(arr[i]/divisor)
    # Return True if total is within the limit, False otherwise
    return total<=limit


def optimized(n,arr,limit):
    # Find the maximum element in the array (upper bound for divisor)
    maxi=max(arr)
    # Initialize binary search bounds
    low,high=1,maxi+1
    # Variable to store the answer
    ans=-1
    # Binary search loop
    while low<=high:
        # Calculate middle point
        mid=(low+high)//2
        # Check if mid as a divisor satisfies the condition
        temp=check(n,arr,mid,limit)
        if(temp):
            # Update answer and search for smaller divisor
            ans=mid
            high=mid-1
        else:
            # Current divisor is too small, search for larger divisor
            low=mid+1
    # Return the smallest valid divisor
    return ans
```

**Time Complexity**

O(n × log(max(nums))) — Binary search takes O(log(max(nums))) iterations, and for each iteration, we compute the sum in O(n) time.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

---

# Koko Eating Bananas

## Problem Description

A monkey is given n piles of bananas, where the ith pile has `nums[i]` bananas. An integer `h` represents the total time in hours to eat all the bananas.

Each hour, the monkey chooses a non-empty pile of bananas and eats `k` bananas. If the pile contains fewer than `k` bananas, the monkey eats all the bananas in that pile and does not consume any more bananas in that hour.

Determine the minimum number of bananas the monkey must eat per hour to finish all the bananas within `h` hours.

**Notes on constraints:**
- For large arrays and large pile sizes, linear search is inefficient. Binary search provides O(n log max(nums)) time complexity.
- The eating speed k must allow Koko to finish all bananas within h hours.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: n = 4, nums = [7, 15, 6, 3], h = 8
Output: 5
```
**Explanation:** If Koko eats 5 bananas/hr, he will take ⌈7/5⌉ = 2 hours, ⌈15/5⌉ = 3 hours, ⌈6/5⌉ = 2 hours, and ⌈3/5⌉ = 1 hour to eat the piles. Total = 2 + 3 + 2 + 1 = 8 hours, which is within the limit.

**Test Case 2:**
```
Input: n = 5, nums = [25, 12, 8, 14, 19], h = 5
Output: 25
```
**Explanation:** If Koko eats 25 bananas/hr, he will take ⌈25/25⌉ = 1, ⌈12/25⌉ = 1, ⌈8/25⌉ = 1, ⌈14/25⌉ = 1, and ⌈19/25⌉ = 1 hour to eat each pile. Total = 5 hours, exactly the limit.

## Approaches

### Approach 1: Linear Search

**Summary:** Try every possible eating speed from 1 to max(nums) and return the first one that allows finishing within h hours.

**Intuition**

The minimum eating speed is 1 banana per hour, and the maximum needed is the largest pile size (eating the entire largest pile in one hour). We try each speed sequentially and check if Koko can finish all bananas within h hours at that speed.

**Approach Steps**

1. Find the maximum pile size (this is the upper bound for eating speed)
2. Iterate through speeds from 1 to max(nums)
3. For each speed, calculate total hours needed using ceiling division for each pile
4. Return the first speed that allows finishing within h hours
5. Return -1 if no valid speed is found (though mathematically, max(nums) will always work)

**Example**

Let's find the minimum speed for nums = [7, 15, 6, 3], h = 8:
- Speed 1: ⌈7/1⌉ + ⌈15/1⌉ + ⌈6/1⌉ + ⌈3/1⌉ = 7 + 15 + 6 + 3 = 31 hours > 8 ✗
- Speed 2: ⌈7/2⌉ + ⌈15/2⌉ + ⌈6/2⌉ + ⌈3/2⌉ = 4 + 8 + 3 + 2 = 17 hours > 8 ✗
- Speed 3: ⌈7/3⌉ + ⌈15/3⌉ + ⌈6/3⌉ + ⌈3/3⌉ = 3 + 5 + 2 + 1 = 11 hours > 8 ✗
- Speed 4: ⌈7/4⌉ + ⌈15/4⌉ + ⌈6/4⌉ + ⌈3/4⌉ = 2 + 4 + 2 + 1 = 9 hours > 8 ✗
- Speed 5: ⌈7/5⌉ + ⌈15/5⌉ + ⌈6/5⌉ + ⌈3/5⌉ = 2 + 3 + 2 + 1 = 8 hours ≤ 8 ✓
- Return 5

**Code**

```python
import math


def check(n,arr,count,hours):
    # Initialize total hours needed to 0
    total=0
    # Iterate through all piles
    for i in range(n):
        # Add the ceiling of arr[i]/count (hours needed for this pile)
        total+=math.ceil(arr[i]/count)
    # Return True if total hours is within the limit, False otherwise
    return total<=hours


def solve(n,arr,hours):
    # Find the maximum pile size (upper bound for eating speed)
    maxi=max(arr)
    # Try each eating speed from 1 to maximum pile size
    for num in range(1,maxi+1):
        # Check if current speed allows finishing within hours
        temp=check(n,arr,num,hours)
        if(temp):
            # Return the first speed that works
            return num
    # Return -1 if no valid speed found
    return -1
```

**Time Complexity**

O(n × max(nums)) — We iterate through max(nums) speeds, and for each speed, we compute the total hours in O(n) time.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search on the range [1, max(nums)] to efficiently find the minimum valid eating speed.

**Intuition**

As the eating speed increases, the total hours needed decreases. This monotonic property allows us to use binary search. We search for the minimum speed that satisfies the condition by efficiently narrowing down the search space.

**Approach Steps**

1. Find the maximum pile size (upper bound for eating speed)
2. Initialize `low = 1`, `high = max(nums) + 1`, and `ans = -1`
3. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - Check if mid as eating speed allows finishing within h hours
   - If yes, update `ans = mid` and search for a smaller speed: `high = mid - 1`
   - If no, search for a larger speed: `low = mid + 1`
4. Return `ans`

**Example**

Let's find the minimum speed for nums = [7, 15, 6, 3], h = 8:
- Initial: low = 1, high = 16, ans = -1
- Iteration 1: mid = 8, hours = ⌈7/8⌉ + ⌈15/8⌉ + ⌈6/8⌉ + ⌈3/8⌉ = 1 + 2 + 1 + 1 = 5 ≤ 8 ✓, ans = 8, high = 7
- Iteration 2: mid = 4, hours = ⌈7/4⌉ + ⌈15/4⌉ + ⌈6/4⌉ + ⌈3/4⌉ = 2 + 4 + 2 + 1 = 9 > 8 ✗, low = 5
- Iteration 3: mid = 6, hours = ⌈7/6⌉ + ⌈15/6⌉ + ⌈6/6⌉ + ⌈3/6⌉ = 2 + 3 + 1 + 1 = 7 ≤ 8 ✓, ans = 6, high = 5
- Iteration 4: mid = 5, hours = ⌈7/5⌉ + ⌈15/5⌉ + ⌈6/5⌉ + ⌈3/5⌉ = 2 + 3 + 2 + 1 = 8 ≤ 8 ✓, ans = 5, high = 4
- low > high, return ans = 5

**Code**

```python
import math


def check(n,arr,count,hours):
    # Initialize total hours needed to 0
    total=0
    # Iterate through all piles
    for i in range(n):
        # Add the ceiling of arr[i]/count (hours needed for this pile)
        total+=math.ceil(arr[i]/count)
    # Return True if total hours is within the limit, False otherwise
    return total<=hours


def optimized(n,arr,hours):
    # Find the maximum pile size (upper bound for eating speed)
    maxi=max(arr)
    # Initialize binary search bounds
    low,high=1,maxi+1
    # Variable to store the answer
    ans=-1
    # Binary search loop
    while low<=high:
        # Calculate middle point
        mid=(low+high)//2
        # Check if mid as eating speed allows finishing within hours
        temp=check(n,arr,mid,hours)
        if(temp):
            # Update answer and search for smaller speed
            ans=mid
            high=mid-1
        else:
            # Current speed is too slow, search for larger speed
            low=mid+1
    # Return the minimum valid eating speed
    return ans
```

**Time Complexity**

O(n × log(max(nums))) — Binary search takes O(log(max(nums))) iterations, and for each iteration, we compute the total hours in O(n) time.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

---

# Minimum Days to Make M Bouquets

## Problem Description

Given n roses and an array `nums` where `nums[i]` denotes that the ith rose will bloom on the `nums[i]`th day, only adjacent bloomed roses can be picked to make a bouquet. Exactly k adjacent bloomed roses are required to make a single bouquet. Find the minimum number of days required to make at least m bouquets, each containing k roses. Return -1 if it is not possible.

**Notes on constraints:**
- If m × k > n, it's impossible to make m bouquets, so return -1.
- For large arrays and large bloom days, binary search provides O(n log max(nums)) time complexity.

## Sample Test Cases With Explanation

**Test Case 1:**
```
Input: n = 8, nums = [7, 7, 7, 7, 13, 11, 12, 7], m = 2, k = 3
Output: 12
```
**Explanation:** On day 12, the first 4 roses (indices 0-3) and the last 3 roses (indices 5-7) have bloomed. We can make 2 bouquets: one with roses at indices 0-2 and another with roses at indices 5-7.

**Test Case 2:**
```
Input: n = 5, nums = [1, 10, 3, 10, 2], m = 3, k = 2
Output: -1
```
**Explanation:** To make 3 bouquets with 2 roses each, we need at least 6 roses. Since we only have 5 roses, it's impossible to make 3 bouquets.

## Approaches

### Approach 1: Linear Search

**Summary:** Try every day from 1 to max(nums) and return the first day when we can make at least m bouquets.

**Intuition**

The earliest possible day is day 1, and the latest needed is the maximum bloom day. For each day, we check how many bouquets can be made by counting adjacent bloomed roses. The first day that allows making at least m bouquets is our answer.

**Approach Steps**

1. Find the maximum bloom day (upper bound for search)
2. Iterate through days from 1 to max(nums)
3. For each day, count how many bouquets can be made:
   - Count consecutive roses that have bloomed by that day
   - When we find k consecutive bloomed roses, increment bouquet count
   - Reset count when we encounter an unbloomed rose
4. Return the first day that allows making at least m bouquets
5. Return -1 if no valid day is found

**Example**

Let's find the minimum days for nums = [7, 7, 7, 7, 13, 11, 12, 7], m = 2, k = 3:
- Day 7: Bloomed roses at indices [0,1,2,3,7]. Consecutive groups: [0-3] (4 roses = 1 bouquet), [7] (1 rose). Total = 1 bouquet < 2 ✗
- Day 11: Bloomed roses at indices [0,1,2,3,5,7]. Consecutive groups: [0-3] (4 roses = 1 bouquet), [5] (1 rose), [7] (1 rose). Total = 1 bouquet < 2 ✗
- Day 12: Bloomed roses at indices [0,1,2,3,5,6,7]. Consecutive groups: [0-3] (4 roses = 1 bouquet), [5-7] (3 roses = 1 bouquet). Total = 2 bouquets ≥ 2 ✓
- Return 12

**Code**

```python
def check(n,arr,days,m,k):
    # Total number of bouquets made
    total=0
    # Count of consecutive bloomed roses
    cnt=0
    # Iterate through all roses
    for i in range(n):
        # Check if current rose has bloomed by 'days'
        if(arr[i]<=days):
            # Increment consecutive bloom count
            cnt+=1
        else:
            # Rose hasn't bloomed, calculate bouquets from previous consecutive roses
            total+=(cnt//k)
            # Reset consecutive count
            cnt=0
    else:
        # After loop, add remaining bouquets from last consecutive group
        total+=(cnt//k)
    # Return True if we can make at least m bouquets
    return total>=m


def solve(n,arr,m,k):
    # Find the maximum bloom day (upper bound for search)
    maxi=max(arr)
    # Try each day from 1 to maximum bloom day
    for num in range(1,maxi+1):
        # Check if we can make m bouquets by day 'num'
        temp=check(n,arr,num,m,k)
        if(temp):
            # Return the first day that works
            return num
    # Return -1 if no valid day found
    return -1
```

**Time Complexity**

O(n × max(nums)) — We iterate through max(nums) days, and for each day, we traverse the array in O(n) time.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search on the range [1, max(nums)] to efficiently find the minimum day when we can make m bouquets.

**Intuition**

As the number of days increases, more roses bloom, so the number of bouquets we can make increases monotonically. This property allows us to use binary search to efficiently find the minimum day that satisfies our requirement.

**Approach Steps**

1. Find the maximum bloom day (upper bound for search)
2. Initialize `low = 1`, `high = max(nums) + 1`, and `ans = -1`
3. While `low <= high`:
   - Calculate `mid = (low + high) // 2`
   - Check if we can make at least m bouquets by day mid
   - If yes, update `ans = mid` and search for an earlier day: `high = mid - 1`
   - If no, search for a later day: `low = mid + 1`
4. Return `ans`

**Example**

Let's find the minimum days for nums = [7, 7, 7, 7, 13, 11, 12, 7], m = 2, k = 3:
- Initial: low = 1, high = 14, ans = -1
- Iteration 1: mid = 7, bouquets = 1 < 2 ✗, low = 8
- Iteration 2: mid = 11, bouquets = 1 < 2 ✗, low = 12
- Iteration 3: mid = 13, bouquets = 2 ≥ 2 ✓, ans = 13, high = 12
- Iteration 4: mid = 12, bouquets = 2 ≥ 2 ✓, ans = 12, high = 11
- low > high, return ans = 12

**Code**

```python
def check(n,arr,days,m,k):
    # Total number of bouquets made
    total=0
    # Count of consecutive bloomed roses
    cnt=0
    # Iterate through all roses
    for i in range(n):
        # Check if current rose has bloomed by 'days'
        if(arr[i]<=days):
            # Increment consecutive bloom count
            cnt+=1
        else:
            # Rose hasn't bloomed, calculate bouquets from previous consecutive roses
            total+=(cnt//k)
            # Reset consecutive count
            cnt=0
    else:
        # After loop, add remaining bouquets from last consecutive group
        total+=(cnt//k)
    # Return True if we can make at least m bouquets
    return total>=m


def optimized(n,arr,m,k):
    # Find the maximum bloom day (upper bound for search)
    maxi=max(arr)
    # Initialize binary search bounds
    low,high=1,maxi+1
    # Variable to store the answer
    ans=-1
    # Binary search loop
    while low<=high:
        # Calculate middle point
        mid=(low+high)//2
        # Check if we can make m bouquets by day mid
        temp=check(n,arr,mid,m,k)
        if(temp):
            # Update answer and search for earlier day
            ans=mid
            high=mid-1
        else:
            # Current day is too early, search for later day
            low=mid+1
    # Return the minimum day to make m bouquets
    return ans
```

**Time Complexity**

O(n × log(max(nums))) — Binary search takes O(log(max(nums))) iterations, and for each iteration, we traverse the array in O(n) time.

**Space Complexity**

O(1) — We only use a constant amount of extra space for variables.

---

# Kth Missing Positive Number

## Problem Description

You are given a strictly increasing array `vec` and a positive integer `k`. Your task is to find the `kth` positive integer missing from `vec`.

**Notes on constraints:**

* Since the array is strictly increasing and can be large, a brute-force approach may be too slow for large `n`.
* An optimized solution should aim for **O(log n)** or **O(n)** time complexity.

---

## Sample Test Cases With Explanation

### Test Case 1

**Input:**

```python
vec = [4, 7, 9, 10]
k = 1
```

**Output:**

```python
1
```

**Explanation:**
The missing numbers are `[1, 2, 3, 5, 6, 8, 11, 12, ...]`.
The 1st missing number is `1`.

---

### Test Case 2

**Input:**

```python
vec = [4, 7, 9, 10]
k = 4
```

**Output:**

```python
5
```

**Explanation:**
The missing numbers are `[1, 2, 3, 5, 6, 8, 11, 12, ...]`.
The 4th missing number is `5`.

---

## Approaches

### Approach 1: Linear Scan (Brute Force)

**Summary:** Check for missing numbers sequentially until the `kth` missing one is found.

---

**Intuition**
We start with the assumption that the first missing number is `k`.
Each time we encounter a number in the array that’s less than or equal to our current guess (`missing`), it means one fewer missing number before that element — so we increase our missing counter.

---

**Approach Steps**

1. Initialize `missing` with `k`.
2. Traverse each element in the array:

   * If the element `arr[i]` is less than or equal to the current `missing`, increment `missing` by 1.
3. After the loop ends, `missing` will represent the `kth` missing number.
4. Return `missing`.

---

**Example**
Let `arr = [4, 7, 9, 10]`, `k = 4`.

* Initially, `missing = 4`.
* `arr[0] = 4 <= 4`, so increment `missing = 5`.
* `arr[1] = 7 > 5`, no change.
* End of loop → result = `5`.

Hence, the 4th missing number is `5`.

---

**Code**

```python
def solve(n, arr, k):
    missing = k  # initial assumption for kth missing number
    for i in range(n):
        # if array element is less than or equal to the current missing number
        if arr[i] <= missing:
            missing += 1  # increment missing count
    return missing
```

---

**Time Complexity**
`O(n)` — We traverse the array once.

**Space Complexity**
`O(1)` — Only a few variables are used.

---

### Approach 2: Binary Search (Optimized)

**Summary:** Use binary search to find the point where the number of missing integers becomes greater than or equal to `k`.

---

**Intuition**
For each element `arr[mid]`, we can calculate how many numbers are missing up to that point using:

```
missing_count = arr[mid] - (mid + 1)
```

* If `missing_count` is less than or equal to `k`, move right.
* Otherwise, move left.
  At the end, compute the result based on the last valid position.

---

**Approach Steps**

1. Initialize `low = 0`, `high = n - 1`, and `ans = -1`.
2. While `low <= high`:

   * Compute `mid = (low + high) // 2`.
   * Calculate `missingCnt = arr[mid] - (mid + 1)`.
   * If `missingCnt <= k`, the `kth` missing number is beyond `mid`. Update `ans` and move right.
   * Otherwise, move left.
3. Return `ans` as the computed `kth` missing number.

---

**Example**
Let `arr = [4, 7, 9, 10]`, `k = 4`.

1. **mid = 1** → `arr[mid] = 7`, `missingCnt = 7 - (1+1) = 5`.
   Since `5 > 4`, move left (`high = mid - 1`).
2. **mid = 0** → `arr[mid] = 4`, `missingCnt = 4 - (0+1) = 3`.
   Since `3 <= 4`, move right (`low = mid + 1`, `ans = 4 + (4 - 3) = 5`).
3. Exit loop → return `ans = 5`.

Result = `5`.

---

**Code**

```python
def optimized(n, arr, k):
    low, high = 0, n - 1
    ans = -1
    while low <= high:
        mid = (low + high) // 2
        # calculate number of missing elements up to index mid
        missingCnt = arr[mid] - (mid + 1)
        
        # if missing count so far is less than or equal to k,
        # kth missing number lies after this index
        if missingCnt <= k:
            ans = arr[mid] + (k - missingCnt)
            low = mid + 1
        else:
            high = mid - 1  # move search space to left
    return ans
```

---

**Time Complexity**
`O(log n)` — Binary search over the array.

**Space Complexity**
`O(1)` — Constant auxiliary space.

---

**Example Run**

```python
arr = [1, 2, 3, 4]
n = len(arr)
k = 2
print(optimized(n, arr, k))  # Output: 6
```

**Explanation:**
No numbers are missing up to 4. The next missing numbers are `[5, 6, 7, ...]`, so the 2nd missing is `6`.

---
