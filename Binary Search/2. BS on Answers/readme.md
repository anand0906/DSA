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
