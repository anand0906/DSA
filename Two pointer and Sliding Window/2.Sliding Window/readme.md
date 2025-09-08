# Sliding Window Problems

## Table of Contents
1. [Max Sum Subarray Of Size K](#max-sum-subarray-of-size-k)
2. [Find All Maximum Elements Of Subarrays Of Size K](#find-all-maximum-elements-of-subarrays-of-size-k)
3. [Sum of Minimum and Maximum Elements of Subarray of Size K](#sum-of-minimum-and-maximum-elements-of-subarray-of-size-k)
4. [Count Of Distinct Elements In All Subarrays Of Size K](#count-of-distinct-elements-in-all-subarrays-of-size-k)
5. [Find First Negative Number Of Subarray Of Size K](#find-first-negative-number-of-subarray-of-size-k)

---

## Max Sum Subarray Of Size K

### Problem Statement
Given an array of integers **arr** and an integer **k**, find the maximum sum of any subarray of size **k**.

### Sample Test Cases

**Example 1:**
- **Input:** arr = [2, 1, 5, 1, 3, 2], k = 3
- **Output:** 11
- **Explanation:** The subarray [5, 1, 3] has the maximum sum of 11.

**Example 2:**
- **Input:** arr = [1, 9, -1, -2, 7, 3, -1, 2], k = 4
- **Output:** 16
- **Explanation:** The subarray [9, -1, -2, 7] has the maximum sum of 16.

### Solutions

#### Approach 1: Brute Force

**Intuition:**
In the brute force approach, we calculate the sum for every possible subarray of size **k** by using two nested loops. For each subarray, we compute the sum and track the maximum sum found.

**Steps to Solve:**
1. Use an outer loop to select the starting index of each subarray of size **k**.
2. Use an inner loop to calculate the sum of elements from the starting index to the end of the subarray.
3. Track the maximum sum as we iterate over all possible subarrays.
4. Return the maximum sum found.

```python
def bruteForce(n, arr, k):
    maxi = -1
    for i in range(n - k + 1):  # Loop to generate all subarrays of size k
        currentSum = 0
        for j in range(i, i + k):  # Calculate the sum of each subarray
            currentSum += arr[j]
        maxi = max(maxi, currentSum)  # Update the maximum sum
    return maxi
```

**Complexity Analysis:**
- **Time Complexity:** O(n * k), as we calculate the sum for each subarray.
- **Space Complexity:** O(1), no extra space is used.

#### Approach 2: Optimized Approach (Sliding Window)

**Intuition:**
We can optimize this by using the **sliding window** technique. Instead of recalculating the sum for each subarray from scratch, we can adjust the sum by subtracting the element that is sliding out of the window and adding the element that is sliding into the window.

**Steps to Solve:**
1. First, calculate the sum of the initial window (the first **k** elements).
2. Store this sum as the current maximum.
3. For each subsequent subarray, update the sum by adding the next element in the array and subtracting the element that is sliding out of the window.
4. Continue to update the maximum sum and return it after all subarrays have been processed.

```python
def optimized(n, arr, k):
    maxi = -1
    currentSum = 0
    left, right = 0, 0
    
    # Calculate the sum of the first window of size k
    while right < k:
        currentSum += arr[right]
        right += 1
    maxi = max(maxi, currentSum)
    
    # Slide the window across the array
    while right < n:
        currentSum += arr[right]  # Add the next element in the window
        currentSum -= arr[left]   # Remove the element that is sliding out of the window
        maxi = max(maxi, currentSum)  # Update the maximum sum
        right += 1
        left += 1
    
    return maxi
```

**Complexity Analysis:**
- **Time Complexity:** O(n), since we traverse the array once and update the sum in constant time.
- **Space Complexity:** O(1), as no extra space is required.

---

## Find All Maximum Elements Of Subarrays Of Size K

### Problem Statement
Given an array of integers **arr** and an integer **k**, find the maximum element in every subarray of size **k**. Return a list of these maximum values for all subarrays.

### Sample Test Cases

**Example 1:**
- **Input:** arr = [2, 1, 5, 1, 3, 2], k = 3
- **Output:** [5, 5, 5, 3]
- **Explanation:** The maximum values of the subarrays [2,1,5], [1,5,1], [5,1,3], and [1,3,2] are 5, 5, 5, and 3 respectively.

**Example 2:**
- **Input:** arr = [1, 3, -1, -3, 5, 3, 6, 7], k = 3
- **Output:** [3, 3, 5, 5, 6, 7]
- **Explanation:** The maximum values of the subarrays are [3, 3, 5, 5, 6, 7].

### Solutions

#### Approach 1: Brute Force

**Intuition:**
In the brute force approach, we calculate the maximum for every subarray of size **k** by using two nested loops. For each subarray, we find the maximum element by iterating over its elements and store the result in a list.

**Steps to Solve:**
1. Use an outer loop to iterate over all starting indices of subarrays of size **k**.
2. For each subarray, use an inner loop to find the maximum element.
3. Store the maximum value of each subarray in a list.
4. Return the list of maximum values.

```python
def bruteForce(n, arr, k):
    final = []
    for i in range(n - k + 1):  # Loop to generate all subarrays of size k
        maxi = float('-inf')  # Track the maximum value
        for j in range(i, i + k):  # Find the maximum in the current subarray
            maxi = max(maxi, arr[j])
        final.append(maxi)  # Store the maximum of the subarray
    return final
```

**Complexity Analysis:**
- **Time Complexity:** O(n * k), as we calculate the maximum for each subarray.
- **Space Complexity:** O(n), as we store the result in a list.

#### Approach 2: Optimized Approach (Sliding Window + Deque)

**Intuition:**
The optimized approach utilizes a combination of the **sliding window technique** and a **deque** (double-ended queue) to efficiently find the maximum values of all subarrays of size **k**. 

1. **Sliding Window:** This technique involves maintaining a window of size **k** that moves across the array. As the window slides from left to right, we update the maximum value by removing elements that are no longer in the window and adding new elements.
  
2. **Using Deque:** The deque is particularly useful here because it allows us to maintain a list of indices of the array elements in such a way that we can quickly access the maximum element. By maintaining indices of the elements in decreasing order, we ensure that the front of the deque always points to the maximum element in the current window. If we encounter an element larger than the elements represented by the indices in the deque, we remove those indices from the back. This guarantees that the largest element is always available at the front of the deque, facilitating constant time retrieval of the maximum for the current window.

This combined approach ensures that we can efficiently calculate the maximum for each sliding window of size **k** in linear time.

**Detailed Steps to Solve:**
1. **Initialization:**
   - Create a deque **q** to store indices of the elements in the current window.
   - Create a list **final** to store the maximums of each subarray.

2. **Processing the First Window:**
   - Iterate through the first **k** elements of the array.
   - For each element, remove elements from the back of the deque while the current element is greater than or equal to the elements represented by those indices. This ensures that the deque will always contain indices of elements in decreasing order.
   - Add the current index to the deque.

3. **Sliding the Window:**
   - For each subsequent element in the array (from index **k** to **n-1**):
     - Add the element at the index represented by the front of the deque to the **final** list. This index points to the maximum element of the previous window.
     - Remove indices from the front of the deque that are out of the bounds of the current window (i.e., if the index at the front of the deque is less than or equal to **i - k**).
     - Repeat the process of removing elements from the back of the deque that are less than or equal to the current element to maintain the decreasing order.
     - Add the current index to the deque.

4. **Final Step:**
   - Add the maximum of the last window (pointed to by the front of the deque) to the **final** list.

```python
from collections import deque
def optimized(n,arr,k):
    q=deque()
    for i in range(k):
        while q and arr[i]>=arr[q[-1]]:
            q.pop()
        q.append(i)
    final=[arr[q[0]]]
    for i in range(k,n):
        while q and q[0]<=i-k:
            q.popleft()
        while q and arr[i]>=arr[q[-1]]:
            q.pop()
        q.append(i)
        final.append(arr[q[0]])
    return final
```

**Complexity Analysis:**
- **Time Complexity:** O(n), since each element is added and removed from the deque at most once, leading to a linear pass through the array.
- **Space Complexity:** O(k), as the deque stores at most **k** indices at a time, corresponding to the current window of size **k**.

---

## Sum of Minimum and Maximum Elements of Subarray of Size K

### Problem Statement
Given an array of integers **arr** and an integer **k**, calculate the sum of the minimum and maximum values in every contiguous subarray of size **k**. Return the total sum of these values for all subarrays.

### Sample Test Cases

**Example 1:**
- **Input:** arr = [1, 3, 2, 5, 4], k = 3
- **Output:** 21
- **Explanation:** The subarrays of size 3 are [1, 3, 2], [3, 2, 5], and [2, 5, 4]. Their minimum and maximum sums are (1+3) + (2+5) + (2+5) = 4 + 8 + 6 = 18.

**Example 2:**
- **Input:** arr = [4, 2, 1, 3, 5], k = 2
- **Output:** 22
- **Explanation:** The subarrays are [4, 2], [2, 1], [1, 3], [3, 5]. Their minimum and maximum sums are (2+4) + (1+2) + (1+3) + (3+5) = 6 + 3 + 4 + 8 = 21.

### Solutions

#### Approach 1: Brute Force

**Intuition:**
In the brute force approach, we iterate through all possible contiguous subarrays of size **k**. For each subarray, we determine the minimum and maximum elements by iterating through the subarray elements. We then accumulate the sum of these min-max pairs for all subarrays.

**Steps to Solve:**
1. Initialize a variable **total** to store the cumulative sum.
2. Use an outer loop to iterate over all starting indices of subarrays of size **k**.
3. For each subarray, initialize **mini** to infinity and **maxi** to negative infinity.
4. Use an inner loop to find the minimum and maximum in the current subarray.
5. Add the sum of the minimum and maximum to **total**.
6. Return **total**.

```python
def bruteForce(n, arr, k):
    total = 0
    for i in range(n - k + 1):  # Loop to generate all subarrays of size k
        mini, maxi = float('inf'), float('-inf')  # Track min and max
        for j in range(i, i + k):  # Find min and max in the current subarray
            mini = min(mini, arr[j])
            maxi = max(maxi, arr[j])
        total += (mini + maxi)  # Update total with min + max
    return total
```

**Complexity Analysis:**
- **Time Complexity:** O(n * k), as we calculate the min and max for each subarray.
- **Space Complexity:** O(1), since no extra space is required except for variables.

#### Approach 2: Optimized Approach (Sliding Window + Deque)

**Intuition:**
The optimized approach uses a **sliding window technique** along with two deques to efficiently track the minimum and maximum values in the current window of size **k**. The deques store indices of the array elements in a way that allows quick access to the minimum and maximum values as the window slides across the array.

1. **Maintaining Deques:** We maintain two deques:
   - One for the minimum values, which stores indices of elements in increasing order (so the front always points to the smallest element).
   - Another for the maximum values, which stores indices in decreasing order (so the front always points to the largest element).

2. **Updating the Window:** As we slide the window across the array, we remove elements that are out of the current window from both deques and add the new element, ensuring the order is maintained. This allows us to get the min and max of the current window in constant time.

This method allows us to compute the sum of the minimum and maximum of all subarrays of size **k** in linear time.

**Detailed Steps to Solve:**
1. **Initialization:**
   - Create two deques, **smaller** for tracking the minimums and **greater** for tracking the maximums.
   - Initialize a variable **total** to accumulate the sums.

2. **Processing the First Window:**
   - Iterate through the first **k** elements of the array.
   - For each element, maintain the order in both deques by popping elements from the back if the current element is smaller (for min deque) or larger (for max deque).
   - Add the current index to both deques.

3. **Sliding the Window:**
   - For each subsequent element in the array (from index **k** to **n-1**):
     - Add the current window's minimum and maximum (from the front of the respective deques) to **total**.
     - Remove elements from the front of the deques that are out of the bounds of the current window.
     - Maintain the order in both deques for the new element by removing smaller (for min) or larger (for max) elements from the back.
     - Add the current index to both deques.

4. **Final Step:**
   - Add the minimum and maximum of the last window (pointed to by the front of the deques) to **total**.

```python
from collections import deque

def optimized_min_max_sum(n, arr, k):
    min_q = deque()  # For tracking the minimum elements
    max_q = deque()  # For tracking the maximum elements

    # Process the first k elements
    for i in range(k):
        while min_q and arr[i] <= arr[min_q[-1]]:
            min_q.pop()
        while max_q and arr[i] >= arr[max_q[-1]]:
            max_q.pop()
        min_q.append(i)
        max_q.append(i)
    
    # Initialize the total sum with the first window
    total = arr[min_q[0]] + arr[max_q[0]]

    # Process the rest of the array
    for i in range(k, n):
        # Remove indices outside the current window
        while min_q and min_q[0] <= i - k:
            min_q.popleft()
        while max_q and max_q[0] <= i - k:
            max_q.popleft()
        
        # Add the current element to the deques
        while min_q and arr[i] <= arr[min_q[-1]]:
            min_q.pop()
        while max_q and arr[i] >= arr[max_q[-1]]:
            max_q.pop()
        min_q.append(i)
        max_q.append(i)

        # Update the total with the minimum and maximum of the current window
        total += arr[min_q[0]] + arr[max_q[0]]
    
    return total
```

**Complexity Analysis:**
- **Time Complexity:** O(n), since each element is added and removed from the deques at most once.
- **Space Complexity:** O(k), as the deques store at most **k** indices at a time.

---

## Count Of Distinct Elements In All Subarrays Of Size K

### Problem Statement
Given an array of integers **arr** and an integer **k**, find the count of distinct integers in every contiguous subarray of size **k**.

### Sample Test Cases

**Example 1:**
- **Input:** arr = [1, 2, 1, 3, 4], k = 3
- **Output:** [2, 3, 3]
- **Explanation:** The subarrays of size 3 are: 
  - [1, 2, 1] - Distinct Count: 2
  - [2, 1, 3] - Distinct Count: 3
  - [1, 3, 4] - Distinct Count: 3

**Example 2:**
- **Input:** arr = [1, 1, 2, 2, 3], k = 2
- **Output:** [2, 2, 2, 2]
- **Explanation:** The subarrays of size 2 are: 
  - [1, 1] - Distinct Count: 1
  - [1, 2] - Distinct Count: 2
  - [2, 2] - Distinct Count: 1
  - [2, 3] - Distinct Count: 2

### Solutions

#### Approach 1: Brute Force

**Intuition:**
In the brute force approach, we iterate through all possible contiguous subarrays of size **k**. For each subarray, we use a dictionary to track the distinct elements and their counts. The size of this dictionary gives the count of distinct integers in the current subarray.

**Steps to Solve:**
1. Initialize an empty list **ans** to store the counts of distinct integers.
2. Use an outer loop to iterate over all starting indices of subarrays of size **k**.
3. For each subarray, initialize a dictionary **distinct** to track counts of elements.
4. Use an inner loop to populate the dictionary with the elements in the current subarray.
5. Add the length of the dictionary to **ans**.
6. Return **ans**.

```python
from collections import defaultdict
def bruteForce(n, arr, k):
    ans = []
    for i in range(n - k + 1):  # Loop to generate all subarrays of size k
        distinct = defaultdict(lambda: 0)  # Track distinct elements
        for j in range(i, i + k):  # Populate distinct elements in the current subarray
            distinct[arr[j]] += 1
        ans.append(len(distinct))  # Append count of distinct elements
    return ans
```

**Complexity Analysis:**
- **Time Complexity:** O(n * k), as we iterate over all subarrays and count elements.
- **Space Complexity:** O(k), for the dictionary storing distinct elements in the current subarray.

#### Approach 2: Optimized Approach (Sliding Window)

**Intuition:**
The optimized approach uses a **sliding window technique** to maintain the count of distinct integers in the current window of size **k**. This is done by adjusting the counts of elements as the window slides over the array, allowing us to efficiently compute the number of distinct integers without re-evaluating the entire subarray.

**Detailed Steps to Solve:**
1. **Initialization:**
   - Create a dictionary **count** to track counts of elements and a variable **cnt** to track the number of distinct elements.
   - Set **left** and **right** pointers to define the window, starting with **right** at the end of the first window.

2. **Processing the First Window:**
   - Iterate through the first **k** elements, updating **count** and **cnt** accordingly.

3. **Sliding the Window:**
   - For each subsequent element in the array (from index **k** to **n-1**):
     - Remove the element at **left** from **count**. If its count goes to zero, decrease **cnt**.
     - Add the element at **right** to **count**. If it's a new element, increase **cnt**.
     - Append **cnt** to **ans**.
     - Move both **left** and **right** pointers forward.

4. **Return Result:**
   - Return the list **ans** containing counts of distinct integers for all subarrays of size **k**.

```python
from collections import defaultdict
def optimized(n, arr, k):
    ans = []
    left, right = 0, 0
    count = defaultdict(lambda: 0)  # Track counts of elements
    cnt = 0  # Count of distinct elements
    
    # Process the first k elements
    while right < k:
        if arr[right] not in count:  # New distinct element
            cnt += 1
        count[arr[right]] += 1  # Increment count of the current element
        right += 1
    ans.append(cnt)  # Append count for the first window
    
    # Process the rest of the array
    while right < n:
        # Remove the leftmost element
        if count[arr[left]] == 1:  # If it was unique
            cnt -= 1
        count[arr[left]] -= 1  # Decrement count
        left += 1  # Move left pointer forward
        
        # Add the new rightmost element
        if count[arr[right]] == 0:  # New distinct element
            cnt += 1
        count[arr[right]] += 1  # Increment count
        ans.append(cnt)  # Append count for the current window
        
        right += 1  # Move right pointer forward
    return ans
```

**Complexity Analysis:**
- **Time Complexity:** O(n), since each element is processed at most twice (once added, once removed).
- **Space Complexity:** O(k), for the dictionary storing counts of elements in the current window.

---

## Find First Negative Number Of Subarray Of Size K

### Problem Statement
Given an array of integers **arr** and an integer **k**, find the first negative integer in every contiguous subarray of size **k**. If there are no negative integers in a subarray, return 0 for that subarray.

### Sample Test Cases

**Example 1:**
- **Input:** arr = [1, -2, 3, -4, 5], k = 2
- **Output:** [-2, -2, -4, -4]
- **Explanation:** 
  - Subarray [1, -2]: First negative integer is -2.
  - Subarray [-2, 3]: First negative integer is -2.
  - Subarray [3, -4]: First negative integer is -4.
  - Subarray [-4, 5]: First negative integer is -4.

**Example 2:**
- **Input:** arr = [1, 2, 3, 4, 5], k = 3
- **Output:** [0, 0, 0]
- **Explanation:** There are no negative integers in any of the subarrays of size 3.

### Solutions

#### Approach 1: Brute Force

**Intuition:**
In the brute force approach, we iterate through all possible contiguous subarrays of size **k** and check each element to find the first negative integer. If we find one, we break out of the inner loop and store that value; if there is none, we store 0.

**Steps to Solve:**
1. Initialize an empty list **ans** to store the first negative integers.
2. Use an outer loop to iterate over all starting indices of subarrays of size **k**.
3. For each subarray, use an inner loop to check for the first negative integer.
4. If a negative integer is found, append it to **ans**; otherwise, append 0.
5. Return **ans**.

```python
def bruteForce(n, arr, k):
    ans = []
    for i in range(n - k + 1):  # Loop to generate all subarrays of size k
        neg = 0
        for j in range(i, i + k):  # Check each element in the current subarray
            if arr[j] < 0:  # If negative, store it and break
                neg = arr[j]
                break
        ans.append(neg)  # Append found negative or 0
    return ans
```

**Complexity Analysis:**
- **Time Complexity:** O(n * k), as we iterate over all subarrays and check each element.
- **Space Complexity:** O(k), for the list storing the first negative integers for each subarray.

#### Approach 2: Optimized Approach (Sliding Window)

**Intuition:**
The optimized approach uses a **deque** (double-ended queue) to keep track of the indices of negative integers in the current window of size **k**. This allows us to efficiently find the first negative integer as the window slides, without needing to re-evaluate all elements in the current window.

**Detailed Steps to Solve:**
1. **Initialization:**
   - Create a deque **q** to store indices of negative integers.
   - Set **left** and **right** pointers to define the window, starting with **right** at the end of the first window.

2. **Processing the First Window:**
   - Iterate through the first **k** elements and fill the deque with indices of negative integers.

3. **Sliding the Window:**
   - For each subsequent element in the array (from index **k** to **n-1**):
     - If the deque is not empty, append the first element (the first negative integer) to **ans**.
     - Remove indices that are out of the bounds of the current window from the front of the deque.
     - If the new element is negative, add its index to the deque.

4. **Return Result:**
   - After processing all elements, append the first negative integer of the last window to **ans**.
   - Return the list **ans** containing the first negative integers for all subarrays of size **k**.

```python
from collections import deque
def optimized(n,arr,k):
    q=deque()
    for i in range(k):
        if(arr[i]<0):
            q.append(i)
    final=[]
    if(q):
        final.append(arr[q[0]])
    else:
        final.append(0)
    for i in range(k,n):
        while q and q[0]<=i-k:
            q.popleft()
        if(arr[i]<0):
            q.append(i)
        if(q):
            final.append(arr[q[0]])
        else:
            final.append(0)
    return final
```

**Complexity Analysis:**
- **Time Complexity:** O(n), as each element is processed at most twice (once added, once removed).
- **Space Complexity:** O(k), for the deque storing indices of negative elements in the current window.
