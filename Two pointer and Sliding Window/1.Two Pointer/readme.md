# Two Pointer & Sliding Window

## 📋 Table of Contents
1. [Container With Most Water](#container-with-most-water)
2. [Trapping Rain Water](#trapping-rain-water)
3. [Longest Substring Without Repeating Characters](#longest-substring-without-repeating-characters)
4. [Maximum Consecutive 1's with At Most K Flips](#maximum-consecutive-1s-with-at-most-k-flips)
5. [Fruits Into Basket](#fruits-into-basket-longest-subarray-with-at-most-2-different-elements)
6. [Longest Repeating Character Replacement](#longest-repeating-character-replacement-with-at-most-k-operations)
7. [Longest Substring With At Most K Different Characters](#longest-substring-with-at-most-k-different-characters)
8. [Minimum Window Substring](#minimum-window-substring)
9. [Count Of Subarrays With Sum Equals Target](#count-of-subarrays-whose-sum-equals-to-given-value-binary-array)
10. [Count Number of Nice Subarrays](#count-number-of-nice-subarrays)
11. [Number of Substrings Containing All Three Characters](#number-of-substrings-containing-all-three-characters)
12. [Subarrays with K Different Integers](#subarrays-with-k-different-integers)
13. [Maximum Points from Cards](#maximum-points-you-can-obtain-from-cards)

---

## Container With Most Water

### 📝 Problem Statement
Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of the line i are at (i, ai) and (i, 0). Find two lines, which together with the x-axis form a container, such that the container contains the most water.

<img src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg" />

### 🧪 Examples
**Input**: [1, 8, 6, 2, 5, 4, 8, 3, 7]  
**Output**: 49  
**Explanation**: The vertical lines at positions 1 and 8 form the container with the maximum area (7 units wide and height of min(8, 7) = 7 units).

**Input**: [6, 5, 4, 3, 2, 1]  
**Output**: 6  
**Explanation**: The first and last lines form the container with the maximum area.

**Input**: [10000, 1, 1, 1, 10000]  
**Output**: 40000  
**Explanation**: The first and last lines form the container with the maximum area.

**Input**: [3, 3, 3, 3, 3]  
**Output**: 12  
**Explanation**: The first and last lines form the container with the maximum area.

### 💡 Solutions

#### Brute Force Approach
In this approach, we will find out all possible unique pairs in given array. Then we will calculate the area for each combination. Out of all we will get the maximum array as our answer.

**Steps To Follow**:
- largest: This variable keeps track of the largest area found so far. It is initialized to 0.
- The outer loop runs from 0 to n-1, where n is the number of elements in the array arr
  - The inner loop runs from i+1 to n, ensuring that we only consider pairs (i, j) where i < j.
    - For each combination of i and j, we will find width and height
    - width: The distance between the two lines at positions i and j, which is j - i.
    - height: The height of the container, determined by the shorter of the two lines, which is min(arr[i], arr[j]).
    - calculate area, The area of the container formed by the lines at i and j, calculated as width * height.
    - largest: Update this variable if the current area is greater than the largest area found so far.
- After all pairs have been checked, the function returns the largest area found.

```python
def bruteForce(n, arr):
    largest = 0
    for i in range(n):
        for j in range(i + 1, n):
            width = j - i
            height = min(arr[i], arr[j])
            area = width * height
            largest = max(largest, area)
    return largest

arr=list(map(int,input().split()))
n=len(arr)
print(bruteForce(n,arr))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(1)

#### Optimized Approach
Here we can observe that the area is determined by the shorter of two lines and the width between them. By considering width at most from beginning, we can find out max area for each line by using two pointer approach.

- Initialize two pointers, one at the beginning (left) and one at the end (right) of the list.
- Calculate the area formed by the lines at these two pointers.
- Move the pointer pointing to the shorter line inward, as this might lead to a larger area by potentially increasing the height. Since we have found max area formed for shorter line as width is maximum.

**Steps To Follow**:
1. **Initialization:**
   - left: Pointer starting at the beginning of the list.
   - right: Pointer starting at the end of the list.
   - largest: Variable to keep track of the largest area found, initialized to 0.

2. **While Loop**
   - The loop runs as long as left is less than right
     - **Calculate Area Between Left and right**
       - width: The distance between the two pointers, calculated as right - left.
       - height: The height of the container, determined by the shorter of the two lines, which is min(arr[left], arr[right]).
       - area: The area of the container formed by the lines at left and right, calculated as width * height
       - largest: Update this variable if the current area is greater than the largest area found so far.
     - **Move Pointers**
       - If the height at left is less than the height at right, increment the left pointer to potentially find a taller line. That means, we have found max area possible with left pointer
       - Otherwise, decrement the right pointer.

3. After the loop terminates, the function returns the largest area found.

```python
def optimized(n, arr):
    left, right = 0, n - 1
    largest = 0
    
    while left < right:
        width = right - left
        height = min(arr[left], arr[right])
        area = width * height
        largest = max(largest, area)
        
        # Move the pointer pointing to the shorter line inward
        if arr[left] < arr[right]:
            left += 1
        else:
            right -= 1
    
    return largest

arr=list(map(int,input().split()))
n=len(arr)
print(optimized(n,arr))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

---

## Trapping Rain Water

### 📝 Problem Statement
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

![Rain Water Trap](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

### 🧪 Examples
**Input**: [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]  
**Output**: 6  
**Explanation**: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

**Input**: [4, 2, 0, 3, 2, 5]  
**Output**: 9  
**Explanation**: The elevation map can trap 9 units of water.

**Input**: [1, 0, 2]  
**Output**: 1  
**Explanation**: The elevation map can trap 1 unit of water.

**Input**: [3, 0, 0, 2, 0, 4]  
**Output**: 10  
**Explanation**: The elevation map can trap 10 units of water.

### 💡 Solutions

#### Brute Force Approach
In this approach, we will try to find maximum water can be trapped at each step and we will sum all those to get our final answer. The amount of water that can be trapped at a step will be minimum of (maximum height of step which is left to it and maximum height of step which is right to it) - current step size.

![Trapping Rain Water Problem](https://media.geeksforgeeks.org/wp-content/uploads/20240613172631/Trapping-Rain-Water-Problem.png)

By observing above example, we can say water at step-3, can be calculated as:
- maximum step height to its left is 3, i.e is step-1
- maximum step height to its right is 4, i.e is step-5
- So, maximum step-3, we can have water up to 3 units
- since we have a step of size-1 already, then water at step-3. will be 3-1=2

**Steps To Follow**:
1. Loop through given array
   - For each element, find the left maximum height and right maximum height
   - Find water content at current element, currentWater=min(leftHeight,rightHeight)-current element height
   - **Edge Case**: If there is negative value for current water, that means, current element can't trap any water, so continue with next element
   - add current water to total, to calculate total water
2. return total as final answer

```python
def bruteForce(n,arr):
    total=0
    for i in range(n):
        leftMax,rightMax=0,0
        for j in range(0,i):
            leftMax=max(leftMax,arr[j])
        for j in range(i+1,n):
            rightMax=max(rightMax,arr[j])
        currentWater=min(leftMax,rightMax)-arr[i]
        if(currentWater<0):
            continue
        total+=currentWater
    return total
arr=list(map(int,input().split()))
n=len(arr)
print(bruteForce(n,arr))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(1)

#### Better Approach
In previous approach, we try to loop for every element to find its maximum left height and minimum left height. We can loop from last element to first element once and store maximum right height at each element and store it in an array. Then, we will loop from first to last element, we will track current maximum height that can used as left height at each element. Now, at each element, we will find out water content using current maximum height and maximum right height stored in a array previously. This resolves inner looping for each element.

```python
def better(n,arr):
    total=0
    rightMax=[0]*n
    maxi=0
    for i in range(n-1,-1,-1):
        rightMax[i]=maxi
        maxi=max(maxi,arr[i])
    leftMax=0
    for i in range(n):
        currentWater=min(leftMax,rightMax[i])-arr[i]
        leftMax=max(leftMax,arr[i])
        if(currentWater<0):
            continue
        total+=currentWater
    return total
arr=list(map(int,input().split()))
n=len(arr)
print(better(n,arr))
```

**Time Complexity**: O(2n)  
**Space Complexity**: O(n)

#### Optimized Approach
In this approach, we will use two pointer algorithm to solve the problem. We will place a pointer on first element called left pointer and a pointer on last element called right pointer. We will loop through array element until left < right.

- If left value is less than right pointer value then we know 100% current building (left) will trap the water = currentHeight - leftMax. We can update the total water trapped and increase the left pointer.
- Similarly if the right value is less than left then we can calculate the water that will be trapped on the current building (right). We can update the total water trapped and reduce the right pointer.

To calculate water content, we need left maximum and right maximum, we will track it along with pointer movements.

**Steps to solve**:
1. Initialize left,right,leftMax,rightMax
2. Loop until left < right
   - check if left height is greater than right height
     - if true, that means, left bar can trap elements
     - check if left element greater than left maximum height
       - if true, that means it can't hold water, since left maximum is small, water may spill out, so update left max with current left element
       - if false, that means it can hold water, calculate water=leftMaximum-leftelement and update total
     - increase left pointer
   - if false, that means, right bar can trap elements
   - check if right element greater than right maximum height
     - if true, that means it can't hold water, since right maximum is small, water may spill out, so update right max with current right element
     - if false, that means it can hold water, calculate water=rightmaximum-rightelement and update total
   - reduce right pointer

```python
def optimized(n,arr):
    left,right=0,n-1
    leftMax,rightMax=0,0
    total=0
    while left<right:
        if(arr[left]<=arr[right]):
            if(arr[left] > leftMax):
                leftMax=arr[left]
            else:
                currentWater=(leftMax-arr[left])
                total+=currentWater
            left+=1
        else:
            if(arr[right]>rightMax):
                rightMax=arr[right]
            else:
                currentWater=(rightMax-arr[right])
                total+=currentWater
            right-=1
    return total

arr=list(map(int,input().split()))
n=len(arr)
print(optimized(n,arr))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

---

## Longest Substring Without Repeating Characters

### 📝 Problem Statement
Given a string s, find the length of the longest substring without repeating characters.

### 🧪 Examples
**Input**: s = "abcabcbb"  
**Output**: 3  
**Explanation**: The answer is "abc", with the length of 3.

**Input**: s = "bbbbb"  
**Output**: 1  
**Explanation**: The answer is "b", with the length of 1.

**Input**: s = "pwwkew"  
**Output**: 3  
**Explanation**: The answer is "wke", with the length of 3.  
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Input**: s = ""  
**Output**: 0  
**Explanation**: There is no substring in an empty string.

**Input**: s = "abcdef"  
**Output**: 6  
**Explanation**: All characters are unique, so the entire string is the longest substring without repeating characters.

### 💡 Solutions

#### Brute Force Approach
In this approach, we will find out all possible substrings for given string, for each substring we will check if has any duplicate characters, if not we will track its length. Then, we will return longest length.

```python
def bruteForce(n,s):
    maxi=0
    for i in range(n):
        for j in range(i+1,n+1):
            substr=s[i:j]
            count={}
            for k in substr:
                if k in count:
                    count[k]+=1
                else:
                    count[k]=1
                if(count[k]>1):
                    break
            else:
                length=j-i
                maxi=max(maxi,length)
    return maxi
s=input()
n=len(s)
print(bruteForce(n,s))
```

**Time Complexity**: O(n³)  
**Space Complexity**: O(n)

#### Better Approach
In previous approach, we are using three loops, instead of that we can reduce to two loops. Outer loop is used to determine starting point for each substring. Inner loop is used to determine ending point for each substring. For current starting point, the substrings will be generated by adding character by character to next to it. For each character adding new substring will be created, we can track duplicates and track its length. The final answer will be longest length.

```python
def better(n,s):
    maxi=0
    for i in range(n):
        check={}
        substr=""
        for j in range(i,n):
            if s[j] in check:
                break
            substr+=s[j]
            check[s[j]]=1
            length=j-i+1
            maxi=max(maxi,length)
    return maxi
s=input()
n=len(s)
print(better(n,s))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(n)

We can also use array of size 256 instead of dictionary, that can store any character count, this will take constant amount of space:

```python
def better2(n,s):
    maxi=0
    for i in range(n):
        check=[0]*256
        for j in range(i,n):
            if check[ord(s[j])]:
                break
            check[ord(s[j])]=1
            length=j-i+1
            maxi=max(maxi,length)
    return maxi
s=input()
n=len(s)
print(better2(n,s))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(256)

#### Optimized Approach
In this approach, we can use sliding window approach. We will create two pointers left and right which points 0th position of string. We will move right pointer towards nth position. While moving, we will check if current window, i.e substring from left to right contains unique characters or not. If it has unique characters we will track its length. Otherwise we will shrink the window, by moving left pointer towards a position that will result the unique substring window.

**Steps To Follow**:
1. **Initialization:**
   - maxi: Keeps track of the maximum length of substrings found.
   - check: A dictionary to store the index of each character.
   - left: Pointer to mark the start of the current window.
   - right: Pointer to iterate through the string

2. **While Loop:**
   - The loop runs as long as right is less than n (length of the string).

3. **Handling Duplicates:**
   - If the character at right is already in 'check', update left to be the maximum of check[s[right]] + 1 (next position after the last occurrence of the current character) and left (to ensure it never moves backwards).
   - Update 'check' with the current index of s[right].

4. **Calculating Length and Updating Maximum:**
   - Calculate the length of the current window as right - left + 1.
   - Update maxi with the maximum of the current length and maxi.

5. **Increment Right Pointer:**
   - Move the right pointer to the next character.

6. **Return Result:**
   - After the loop completes, maxi will contain the length of the longest substring without repeating characters.

```python
def optimized(n,s):
    maxi=0
    check={}
    left,right=0,0
    while right<n:
        if s[right] in check:
            left=max(check[s[right]]+1,left)
        check[s[right]]=right
        length=right-left+1
        maxi=max(maxi,length)
        right+=1
    return maxi
s=input()
n=len(s)
print(optimized(n,s))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(n)

We can also use array of size 256 to manage duplicate characters instead of dictionary:

```python
def optimized2(n,s):
    maxi=0
    check=[-1]*256
    left,right=0,0
    while right<n:
        if check[ord(s[right])]!=-1:
            left=max(check[ord(s[right])]+1,left)
        check[ord(s[right])]=right
        length=right-left+1
        maxi=max(maxi,length)
        right+=1
    return maxi
s=input()
n=len(s)
print(optimized2(n,s))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(256)

---

## Maximum Consecutive 1's with At Most K Flips

### 📝 Problem Statement
Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

### 🧪 Examples
**Input**: nums = [1,1,0,0,1,1,1,0,1,1], k = 2  
**Output**: 8  
**Explanation**: By flipping the two 0's at indices 2 and 3, we get the array [1,1,1,1,1,1,1,0,1,1], which has 8 consecutive 1's.

**Input**: nums = [1,0,1,1,0,1,0,1], k = 1  
**Output**: 4  
**Explanation**: By flipping the 0 at index 1, we get [1,1,1,1,0,1,0,1], which has 4 consecutive 1's.

**Input**: nums = [1,1,1,1], k = 0  
**Output**: 4  
**Explanation**: The array already has 4 consecutive 1's without any flips.

**Input**: nums = [0,0,0,0], k = 2  
**Output**: 2  
**Explanation**: We can flip any two 0's to get at most 2 consecutive 1's.

### 💡 Solutions

In this problem, simply we have to find longest subarray contains less than or equal to k zeros.

#### Brute Force Approach
In this approach, we will find all possible subarrays, and for each subarray we will count number of zeros in it, if count less than or equal to k, then we will find length of that subarray. Out of all subarrays, we will return longest length.

```python
def bruteforce(n,arr,k):
    maxi=0
    for i in range(n):
        for j in range(i,n):
            zeros=0
            for t in range(i,j+1):
                if(arr[t]==0):
                    zeros+=1
            if(zeros<=k):
                length=j-i+1
                maxi=max(maxi,length)
    return maxi
arr=list(map(int,input().split()))
n=len(arr)
k=int(input())
print(bruteforce(n,arr,k))
```

**Time Complexity**: O(n³)  
**Space Complexity**: O(1)

#### Better Approach
In this approach, we will modify previous approach to eliminate third inner loop. We don't need to loop for every combination of i and j. Current subarray will come from just adding current element to previous subarray.

```python
def better(n,arr,k):
    maxi=0
    for i in range(n):
        zeros=0
        for j in range(i,n):
            if(arr[j]==0):
                zeros+=1
            if(zeros<=k):
                length=j-i+1
                maxi=max(maxi,length)
    return maxi
arr=list(map(int,input().split()))
n=len(arr)
k=int(input())
print(better(n,arr,k))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(1)

#### Optimized Approach
In this approach, we will use two pointer sliding window approach. Here, we will use two pointer left and right, that points to 0th position initially. We will move right pointer towards nth position. We will check current window zeros count for each move, if it has less than or equal to k, then we will track its length. Otherwise, we will shrink window, by moving left pointer towards to right, so that window should contain at most k zeros. Finally, we will return maximum window length as answer.

**Steps**:
1. Initialize two pointers, `left` and `right`, both at the beginning of the array.
2. Use a variable `zeros` to keep track of the number of zeros in the current window.
3. Iterate with the `right` pointer through the array:
   - If the element at `right` is 0, increment `zeros`.
   - If `zeros` exceeds `k`, increment the `left` pointer until `zeros` is less than or equal to `k`.
   - Update the maximum length of the window as `right - left + 1`.
4. Return the maximum length found.

```python
def optimized(n,arr,k):
    maxi=0
    left,right=0,0
    zeros=0
    while right<n:
        if(arr[right]==0):
            zeros+=1
        if(zeros>k):
            while (zeros>k):
                if(arr[left]==0):
                    zeros-=1
                left+=1
        length=right-left+1
        maxi=max(maxi,length)
        right+=1
    return maxi
arr=list(map(int,input().split()))
n=len(arr)
k=int(input())
print(optimized(n,arr,k))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

---

## Fruits Into Basket: Longest Subarray with At Most 2 Different Elements

### 📝 Problem Statement
You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array fruits of size N, where fruits[i] is the type of fruit the ith tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:
- You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
- Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of the baskets.
- Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array fruits, return the maximum number of fruits you can pick.

### 🧪 Examples
**Input**: fruits [] = { 2, 1, 2 }  
**Output**: 3  
**Explanation**: We can pick from all three trees.

**Input**: fruits [] = { 0, 1, 2, 2, 2, 2 }  
**Output**: 5  
**Explanation**: It's optimal to pick from trees(0-indexed) [1,2,3,4,5].

### 💡 Solutions

In this problem, length of maximum subarray which contains at most 2 different integers.

#### Brute Force Approach
In this approach, we will find all possible subarrays, for each subarray we will check if it has 2 different integers or not. If it has, we will track its length. Out of all possibilities, longest length will be our answer.

```python
def bruteForce(n,arr):
    maxi=0
    for i in range(n):
        for j in range(i,n):
            s=set()
            for k in range(i,j+1):
                s.add(arr[k])
                if(len(s)>2):
                    break
            else:
                length=j-i+1
                maxi=max(maxi,length)
    return maxi
arr=list(map(int,input().split()))
n=len(arr)
print(bruteForce(n,arr))
```

**Time Complexity**: O(n³)  
**Space Complexity**: O(n)

#### Better Approach
In this approach, we will just modify previous approach, to eliminate third inner loop.

```python
def better(n,arr):
    maxi=0
    for i in range(n):
        s=set()
        for j in range(i,n):
            s.add(arr[j])
            if(len(s)>2):
                break
            length=j-i+1
            maxi=max(maxi,length)
    return maxi
arr=list(map(int,input().split()))
n=len(arr)
print(better(n,arr))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(n)

#### Optimized Approach
In this approach, we will use two pointer sliding window approach. We will create two pointers left and right which points 0th position of string. We will move right pointer towards nth position. While moving, we will check if current window, i.e subarray from left to right contains 2 unique integers or not. If it has 2 unique integers we will track its length. Otherwise we will shrink the window, by moving left pointer towards a position that will result the 2 unique integer subarray window.

**Steps to solve**:
1. Initialize two pointers, `left` and `right`, both at the beginning of the array.
2. Use a dictionary `count` to keep track of the frequency of characters in the current window.
3. Iterate with the `right` pointer through the array:
   - If the character at `right` is in `count`, increment its count.
   - If the character is not in `count`, add it with a count of 1.
   - If the number of distinct characters exceeds 2, increment the `left` pointer until the number of distinct characters is less than or equal to 2.
   - Update the maximum length of the window as `right - left + 1`.
4. Return the maximum length found.

```python
def optimized(n,arr):
    maxi=0
    left,right=0,0
    count={}
    while right<n:
        if arr[right] in count:
            count[arr[right]]+=1
        else:
            count[arr[right]]=1
        if(len(count)>2):
            while (len(count)>2):
                count[arr[left]]-=1
                if(count[arr[left]]==0):
                    del count[arr[left]]
                left+=1
        length=right-left+1
        maxi=max(maxi,length)
        right+=1
    return maxi
arr=list(map(int,input().split()))
n=len(arr)
print(optimized(n,arr))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

---

## Longest Repeating Character Replacement With At Most K Operations

### 📝 Problem Statement
You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

### 🧪 Examples
**Input**: s = "ABAB", k = 2  
**Output**: 4  
**Explanation**: Replace the two 'A's with two 'B's or vice versa.

**Input**: s = "AABABBA", k = 1  
**Output**: 4  
**Explanation**: Replace the one 'A' in the middle with 'B' and form "AABBBBA". The substring "BBBB" has the longest repeating letters, which is 4. There may exist other ways to achieve this answer too.

### 💡 Solutions

In this problem, length of maximum substring which contains all elements same, we can change up to k character.

#### Brute Force Approach
In this approach, we will find all possible subarrays, for each substring, we will minimum operations to convert the substring contains all characters same.

minimum operations = length of substring - maxFrequency

maxFrequency = count of element occurs most number of times in substring

if minimum operations less than or equal to k, we will track its length. Out of all possibilities, longest length will be our answer.

```python
def bruteforce(n,s,k):
    maxi=0
    for i in range(n):
        for j in range(i,n):
            maxFreq=0
            count={}
            for t in range(i,j+1):
                if s[t] in count:
                    count[s[t]]+=1
                else:
                    count[s[t]]=1
                maxFreq=max(maxFreq,count[s[t]])
            length=j-i+1
            if((length-maxFreq)<=k):
                maxi=max(maxi,length)
    return maxi
s=input()
n=len(s)
k=int(input())
print(bruteforce(n,s,k))
```

**Time Complexity**: O(n³)  
**Space Complexity**: O(n)

#### Better Approach
In this approach, we will just modify previous approach, to eliminate third inner loop.

```python
def better(n,s,k):
    maxi=0
    for i in range(n):
        maxFreq=0
        count={}
        for j in range(i,n):
            if s[j] in count:
                count[s[j]]+=1
            else:
                count[s[j]]=1
            maxFreq=max(maxFreq,count[s[j]])
            length=j-i+1
            if((length-maxFreq)<=k):
                maxi=max(maxi,length)
    return maxi
s=input()
n=len(s)
k=int(input())
print(better(n,s,k))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(n)

#### Optimized Approach
In this approach, we will use two pointer sliding window approach. We will create two pointers left and right which points 0th position of string. We will move right pointer towards nth position. While moving, we will check if current window, i.e substring from left to right can be converted to have all same characters with at most k operations. Otherwise we will shrink the window, by moving left pointer towards a position that will result required substring.

**Steps to solve**:
1. Initialize two pointers, `left` and `right`, both at the beginning of the string.
2. Use a dictionary `count` to keep track of the frequency of characters in the current window.
3. Use a variable `maxFreq` to keep track of the highest frequency of any character in the current window.
4. Iterate with the `right` pointer through the string:
   - Increment the frequency of the character at `right` in the `count` dictionary.
   - Update `maxFreq` to be the maximum frequency in the current window.
   - If the number of characters to replace (i.e., `length - maxFreq`) exceeds `k`, increment the `left` pointer to reduce the window size until the number of replacements is less than or equal to `k`.
   - Update the maximum length of the window as `right - left + 1`.
5. Return the maximum length found.

```python
def optimized(n,s,k):
    left,right=0,0
    maxi=0
    maxFreq=0
    count={}
    while(right<n):
        if s[right] in count:
            count[s[right]]+=1
        else:
            count[s[right]]=1
        maxFreq=max(maxFreq,count[s[right]])
        length=right-left+1
        while((left<right) and (length-maxFreq)>k):
            count[s[left]]-=1
            maxFreq=max(maxFreq,count[s[left]])
            left+=1
            length=right-left+1
        maxi=max(maxi,length)
        right+=1
    return maxi
s=input()
n=len(s)
k=int(input())
print(optimized(n,s,k))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

---

## Longest Substring With At Most K Different Characters

### 📝 Problem Statement
Given a string `s` and an integer `k`, return the length of the longest substring that contains at most `k` different characters.

### 🧪 Examples
- **Basic Case**  
  **Input**: `s = "eceba"`, `k = 2`  
  **Output**: `3`  
  **Explanation**: The substring `"ece"` contains at most two different characters.

- **Single Character**  
  **Input**: `s = "aaaa"`, `k = 1`  
  **Output**: `4`  
  **Explanation**: The entire string contains only one distinct character.

- **Two Characters**  
  **Input**: `s = "ab"`, `k = 2`  
  **Output**: `2`  
  **Explanation**: The entire string contains exactly two distinct characters.

- **Multiple Characters**  
  **Input**: `s = "abcabcabc"`, `k = 2`  
  **Output**: `2`  
  **Explanation**: The longest substring with at most two distinct characters is `"ab"`.

### 💡 Solutions

In this problem, we have to find longest substring contains at most k distinct characters.

#### Brute Force Approach
In this approach, we will find all possible subarrays, for each substring, we will find number of unique characters present in it. If count less than or equal to k, we will track its length. Out of all possibilities, longest length will be our answer.

```python
def bruteForce(n,s,k):
    maxi=0
    for i in range(n):
        for j in range(i,n):
            temp=set()
            for t in range(i,j+1):
                temp.add(s[t])
                if(len(temp)>k):
                    break
            else:
                length=j-i+1
                maxi=max(maxi,length)
    return maxi

s=input()
n=len(s)
k=int(input())
print(bruteForce(n,s,k))
```

**Time Complexity**: O(n³)  
**Space Complexity**: O(1)

#### Better Approach
In this approach, we will just modify previous approach, to eliminate third inner loop.

```python
def better(n,s,k):
    maxi=0
    for i in range(n):
        temp=set()
        for j in range(i,n):
            temp.add(s[j])
            if(len(temp)>k):
                break
            length=j-i+1
            maxi=max(maxi,length)
    return maxi
s=input()
n=len(s)
k=int(input())
print(better(n,s,k))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(1)

#### Optimized Approach

**Steps**:
1. Initialize two pointers, `left` and `right`, both at the beginning of the string.
2. Use a dictionary `count` to keep track of the frequency of characters in the current window.
3. Iterate with the `right` pointer through the string:
   - If the character at `right` is in `count`, increment its count.
   - If the character is not in `count`, add it with a count of 1.
   - If the number of distinct characters exceeds `k`, increment the `left` pointer to reduce the window size until the number of distinct characters is less than or equal to `k`.
   - Update the maximum length of the window as `right - left + 1`.
4. Return the maximum length found.

```python
def optimized(n,s,k):
    maxi=0
    left,right=0,0
    count={}
    while right<n:
        if s[right] in count:
            count[s[right]]+=1
        else:
            count[s[right]]=1
        if(len(count)>k):
            while (len(count)>k):
                count[s[left]]-=1
                if(count[s[left]]==0):
                    del count[s[left]]
                left+=1
        length=right-left+1
        maxi=max(maxi,length)
        right+=1
    return maxi

s=input()
n=len(s)
k=int(input())
print(optimized(n,s,k))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

---

## Minimum Window Substring

### 📝 Problem Statement
Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

### 🧪 Examples
**Input**: s = "ADOBECODEBANC", t = "ABC"  
**Output**: "BANC"  
**Explanation**: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

**Input**: s = "a", t = "a"  
**Output**: "a"  
**Explanation**: The entire string s is the minimum window.

**Input**: s = "a", t = "aa"  
**Output**: ""  
**Explanation**: Both 'a's from t must be included in the window. Since the largest window of s only has one 'a', return empty string.

### 💡 Solutions

In this problem, we have to find minimum length of substring which contains all characters of another string.

#### Brute Force Approach
In this approach, we will find all possible substrings for given string, for each substring we will check if it contains all characters of another string or not. If it has, we have to check its length, out of all possible substrings, we have to find minimum length substring.

**How to Check a string contains all characters of another string**:
- create a visited dictionary
- loop over the another string and store frequency of each character
- Then, loop through the given string, if any character is already there in dictionary then reduce the frequency, that means that character is already there in another string and we have visited that in current string, we will reduce the frequency by one
- When, the frequency of one character becomes zero or positive, that means one character is visited, then we can track the count of visited characters
- when, visited count equal to length of another string, then substring contains all characters of another string

```python
def bruteForce(n,s,t):
    minLen=float('inf')
    minSubStr=""
    cnt=len(t)
    for i in range(n):
        for j in range(i,n):
            visited={}
            for temp in t:
                if temp in visited:
                    visited[temp]+=1
                else:
                    visited[temp]=1
            currentCnt=0
            for k in range(i,j+1):
                if s[k] in visited:
                    visited[s[k]]-=1
                else:
                    visited[s[k]]=-1
                if(visited[s[k]]>=0):
                    currentCnt+=1
            if(currentCnt>=cnt and (j-i+1)<minLen):
                minLen=j-i+1
                minSubStr=s[i:j+1]
    return minSubStr
s=input()
t=input()
n=len(s)
print(bruteForce(n,s,t))
```

**Time Complexity**: O(n³)  
**Space Complexity**: O(1)

#### Better Approach
We can modify the previous approach from 3 inner loops to 2 inner loops, as previous problems.

```python
def better(n,s,t):
    minLen=float('inf')
    minSubStr=""
    cnt=len(t)
    for i in range(n):
        visited={}
        for temp in t:
            if temp in visited:
                visited[temp]+=1
            else:
                visited[temp]=1
        currentCnt=0
        for j in range(i,n):
            if s[j] in visited:
                visited[s[j]]-=1
            else:
                visited[s[j]]=-1
            if(visited[s[j]]>=0):
                currentCnt+=1
            if(currentCnt>=cnt and (j-i+1)<minLen):
                minLen=j-i+1
                minSubStr=s[i:j+1]
    return minSubStr
s=input()
t=input()
n=len(s)
print(better(n,s,t))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(1)

#### Optimized Approach
In this approach, we will use two pointer sliding window technique to solve the problem.

**Steps**:
1. Create a dictionary `visited` to count the characters in `t`.
2. Initialize `left` and `right` pointers to the start of the string `s`.
3. Initialize `currentCnt` to track how many characters in `t` have been matched in the current window.
4. Iterate with the `right` pointer through the string `s`:
   - If the character at `right` is in `visited`, decrement its count in the dictionary.
   - If the count of the character at `right` is greater than or equal to zero, increment `currentCnt`.
   - While `currentCnt` is equal to the length of `t`, indicating all characters of `t` are in the current window:
     - Update the minimum length and substring if the current window is smaller.
     - Move the `left` pointer to the right, adjusting counts and `currentCnt` accordingly.
5. Return the minimum substring found, or `""` if no such substring exists.

```python
def optimized(n,s,t):
    left,right=0,0
    minLength=float('inf')
    minSubStr=""
    visited={}
    for temp in t:
        if temp in visited:
            visited[temp]+=1
        else:
            visited[temp]=1
    cnt=len(t)
    currentCnt=0
    while(right<n):
        if(s[right] in visited):
            visited[s[right]]-=1
        else:
            visited[s[right]]=-1
        if(visited[s[right]]>=0):
            currentCnt+=1
        while(currentCnt==cnt):
            length=right-left+1
            if(length<minLength):
                minLength=length
                minSubStr=s[left:right+1]
            visited[s[left]]+=1
            if(visited[s[left]]>0):
                currentCnt-=1
            left+=1
        right+=1
    return minSubStr
s=input()
t=input()
n=len(s)
print(optimized(n,s,t))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

---

## Count Of Subarrays Whose Sum Equals To Given Value: Binary Array

### 📝 Problem Statement
Given a binary array nums and an integer goal, return the number of non-empty subarrays with a sum goal.

A subarray is a contiguous part of the array.

### 🧪 Examples
**Input**: nums = [1,0,1,0,1], goal = 2  
**Output**: 4  
**Explanation**: The 4 subarrays are bolded and underlined below.

**Input**: nums = [0,0,0,0,0], goal = 0  
**Output**: 15  
**Explanation**: Each subarray has sum 0.

### 💡 Solutions

In this problem, we have given a binary array which contains only 0 and 1, we have to find count of subarrays whose sum equal to given value.

#### Brute Force Approach
In this approach, we can find all possible subarrays, and for each subarray we will find the possible sum, if sum equals to given target then we will count that subarray. Finally, we can return the count of all possible subarrays.

```python
def bruteForce(n,arr,target):
    cnt=0
    for i in range(n):
        for j in range(i,n):
            sum=0
            for k in range(i,j+1):
                sum+=arr[k]
            if(sum==target):
                cnt+=1
    return cnt
arr=list(map(int,input().split()))
n=len(arr)
target=int(input())
print(bruteForce(n,arr,target))
```

**Time Complexity**: O(n³)  
**Space Complexity**: O(1)

#### Better Approach
In this approach, we will just modify the previous approach to eliminate 3rd inner loop as previous problems.

```python
def better(n,arr,target):
    cnt=0
    for i in range(n):
        sum=0
        for j in range(i,n):
            sum+=arr[j]
            if(sum==target):
                cnt+=1
            if(sum>target):
                break
    return cnt
arr=list(map(int,input().split()))
n=len(arr)
target=int(input())
print(better(n,arr,target))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(1)

#### Optimized Approach
In this approach, we use two pointer approach. Instead of directly counting subarrays with a sum equal to the target, we can make use of two simpler problems:
- Count the number of subarrays with a sum less than or equal to the target.
- Count the number of subarrays with a sum less than or equal to the target minus one.

The difference between these two counts will give us the number of subarrays with a sum exactly equal to the target.

**Explanation**:
1. **Subarrays with Sum ≤ Target:**
   - We use a sliding window (two pointers, left and right) to find the number of subarrays whose sum is less than or equal to the target.
   - Start with both pointers at the beginning of the array.
   - Move the right pointer to expand the window and add the value at arr[right] to currentSum.
   - If currentSum exceeds the target, move the left pointer to reduce the window size and subtract arr[left] from currentSum until currentSum is less than or equal to the target again.
   - For each position of the right pointer, the number of valid subarrays ending at right is (right - left + 1) because any subarray starting from left to right is valid.
   - Sum up these counts to get the total number of subarrays with a sum ≤ target.

2. **Difference of Counts**:
   - To find the number of subarrays with a sum exactly equal to the target, calculate:
     - Number of subarrays with sum ≤ target.
     - Number of subarrays with sum ≤ target - 1.
   - The difference between these two counts gives the number of subarrays with sum exactly equal to the target. This works because the first count includes all subarrays with sum ≤ target, while the second count excludes subarrays with sum exactly equal to target.

```python
def optimizedAtmostTarget(n,arr,target):
    if(target<0):
        return 0
    cnt=0
    left,right=0,0
    currentSum=0
    while (right<n):
        currentSum+=arr[right]
        while currentSum>target and left<=right:
            currentSum-=arr[left]
            left+=1
        cnt+=(right-left+1)
        right+=1
    return cnt
        
        
def optimized(n,arr,target):
    return optimizedAtmostTarget(n,arr,target)-optimizedAtmostTarget(n,arr,target-1)

arr=list(map(int,input().split()))
n=len(arr)
target=int(input())
print(optimized(n,arr,target))
```

**Time Complexity**: O(2n)  
**Space Complexity**: O(1)

---

## Count Number of Nice Subarrays

### 📝 Problem Statement
Given an array of integers nums and an integer k. A continuous subarray is called nice if there are k odd numbers on it.

Return the number of nice sub-arrays.

### 🧪 Examples
**Input**: nums = [1,1,2,1,1], k = 3  
**Output**: 2  
**Explanation**: The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].

**Input**: nums = [2,4,6], k = 1  
**Output**: 0  
**Explanation**: There are no odd numbers in the array.

**Input**: nums = [2,2,2,1,2,2,1,2,2,2], k = 2  
**Output**: 16

### 💡 Solutions

This problem is similar to previous problem. Here, we have to convert the integer array into binary subarray. We can have same approaches as previously. After converting [2,3,4,5] to binary, it will be like [0,1,0,1], we are just modulo division each number with 2. Now we have to find subarray with sum equal to k, that means, 1 always represents odd number. Number of odd numbers equal to sum.

```python
def bruteForce(n,arr,target):
    cnt=0
    for i in range(n):
        for j in range(i,n):
            sum=0
            for k in range(i,j+1):
                sum+=arr[k]
            if(sum==target):
                cnt+=1
    return cnt

def better(n,arr,target):
    cnt=0
    for i in range(n):
        sum=0
        for j in range(i,n):
            sum+=arr[j]
            if(sum==target):
                cnt+=1
            if(sum>target):
                break
    return cnt

def optimizedAtMostTarget(n,arr,target):
    if(target<0):
        return 0
    cnt=0
    left,right=0,0
    currentSum=0
    while (right<n):
        currentSum+=arr[right]
        while currentSum>target and left<=right:
            currentSum-=arr[left]
            left+=1
        cnt+=(right-left+1)
        right+=1
    return cnt
        
        
def optimized(n,arr,target):
    return optimizedAtMostTarget(n,arr,target)-optimizedAtMostTarget(n,arr,target-1)

arr=list(map(int,input().split()))
arr=[i&1 for i in arr]
n=len(arr)
target=int(input())
print(bruteForce(n,arr,target))
print(better(n,arr,target))
print(optimized(n,arr,target))
```

---

## Number of Substrings Containing All Three Characters

### 📝 Problem Statement
Given a string s consisting only of characters a, b and c. Return the number of substrings containing at least one occurrence of all these characters a, b and c.

### 🧪 Examples
**Input**: s = "abcabc"  
**Output**: 10  
**Explanation**: The substrings containing at least one occurrence of the characters a, b and c are "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc" and "abc" (again).

**Input**: s = "aaacb"  
**Output**: 3  
**Explanation**: The substrings containing at least one occurrence of the characters a, b and c are "aaacb", "aacb" and "acb".

**Input**: s = "abc"  
**Output**: 1

### 💡 Solutions

In this problem, we have to find count of all possible substrings which contains a, b and c characters.

#### Brute Force Approach
In this approach, we will find all possible substrings and check whether it contains a, b, c. If any substring contains, then we will increase the count by one. Finally, we will return count as our answer.

```python
def bruteforce(n,s):
    cnt=0
    for i in range(n):
        for j in range(i,n):
            temp=[0,0,0]
            for k in range(i,j+1):
                temp[ord(s[k])-ord('a')]=1
                if(temp[0] and temp[1] and temp[2]):
                    cnt+=1
                    break
    return cnt

s=input()
n=len(s)
print(bruteforce(n,s))
```

**Time Complexity**: O(n³)  
**Space Complexity**: O(1)

#### Better Approach
We can modify previous approach to reduce 3 loops to 2 loops as discussed previously.

```python
def better(n,s):
    cnt=0
    for i in range(n):
        temp=[0,0,0]
        for j in range(i,n):
            temp[ord(s[j])-ord('a')]=1
            if(temp[0] and temp[1] and temp[2]):
                cnt+=1
    return cnt

s=input()
n=len(s)
print(better(n,s))
```

**Time Complexity**: O(n²)  
**Space Complexity**: O(1)

#### Optimized Approach
In this approach, we can use two pointer approach.

**Steps**:
- **Initialize Variables:**
  - **left** and **right** are pointers starting at the beginning of the string.
  - **cnt** is a counter to keep track of the number of valid substrings.
  - **temp** is an array of size 3 to count occurrences of 'a', 'b', and 'c'.
- **Right Pointer Movement:** The right pointer moves through the string to expand the window. For each character at the right pointer:
  - Update the **temp** array by incrementing the count of the character.
- **Check Valid Substring:** After updating the **temp** array:
  - If the window (substring) contains at least one 'a', one 'b', and one 'c', we have a valid substring.
- **Count Valid Substrings:**
  - For every valid substring, the number of substrings ending at **right** is **n-right** (since any substring starting from the current left to any position after the right is valid).
  - Add **n-right** to **cnt**.
- **Left Pointer Movement:** Move the left pointer to the right to reduce the window size:
  - Decrement the count of the character at the left pointer in the **temp** array.
  - Continue moving the left pointer until the window is no longer valid (i.e., it no longer contains at least one 'a', one 'b', and one 'c').
- **Return Result:** Finally, return the value of **cnt**, which represents the total number of valid substrings.

```python
def optimized(n,s):
    left,right=0,0
    cnt=0
    temp=[0,0,0]
    while right<n:
        temp[ord(s[right])-ord('a')]+=1
        while(temp[0] and temp[1] and temp[2]):
            cnt+=(n-right)
            temp[ord(s[left])-ord('a')]-=1
            left+=1
        right+=1
    return cnt

s=input()
n=len(s)
print(optimized(n,s))
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

---

## Subarrays with K Different Integers

### Problem Statement
Given an integer array `nums` and an integer `k`, return the number of good subarrays of `nums`.

A good array is an array where the number of different integers in that array is exactly `k`.

For example, `[1,2,3,1,2]` has 3 different integers: 1, 2, and 3.

A subarray is a contiguous part of an array.

### Examples

**Example 1:**
- **Input:** `nums = [1,2,1,2,3]`, `k = 2`
- **Output:** `7`
- **Explanation:** Subarrays formed with exactly 2 different integers: `[1,2]`, `[2,1]`, `[1,2]`, `[2,3]`, `[1,2,1]`, `[2,1,2]`, `[1,2,1,2]`

**Example 2:**
- **Input:** `nums = [1,2,1,3,4]`, `k = 3`
- **Output:** `3`
- **Explanation:** Subarrays formed with exactly 3 different integers: `[1,2,1,3]`, `[2,1,3]`, `[1,3,4]`

### Solutions

#### Approach 1: Brute Force

**Algorithm:**
In this approach, we can find all possible subarrays, and for each subarray we will check if it has k different integers or not. If it has, we will increment the count by one. Finally, we can return the count of all possible subarrays.

```python
def bruteForce(n,arr,target):
    cnt=0
    for i in range(n):
        for j in range(i,n):
            visited={}
            for k in range(i,j+1):
                if(arr[k] in visited):
                    visited[arr[k]]+=1
                else:
                    visited[arr[k]]=1
                if(visited[arr[k]]==0):
                    del visited[arr[k]]
            if(len(visited)==target):
                cnt+=1
    return cnt

arr=list(map(int,input().split()))
n=len(arr)
target=int(input())
print(bruteForce(n,arr,target))
```

**Complexity Analysis:**
- **Time Complexity:** O(n³)
- **Space Complexity:** O(1) - We can store at most 256 characters in visited

#### Approach 2: Better Approach

**Algorithm:**
In this approach, we will just modify the previous approach to eliminate the 3rd inner loop as in previous problems.

```python
def better(n,arr,target):
    cnt=0
    for i in range(n):
        visited={}
        for j in range(i,n):
            if(arr[j] in visited):
                visited[arr[j]]+=1
            else:
                visited[arr[j]]=1
            if(visited[arr[j]]==0):
                del visited[arr[j]]
            if(len(visited)==target):
                cnt+=1
    return cnt

arr=list(map(int,input().split()))
n=len(arr)
target=int(input())
print(better(n,arr,target))
```

**Complexity Analysis:**
- **Time Complexity:** O(n²)
- **Space Complexity:** O(1) - We can store at most 256 characters in visited

#### Approach 3: Optimized Approach (Two Pointer)

**Algorithm:**
In this approach, we can use two pointer approach. We want to count the number of subarrays with exactly **target** distinct elements. Instead of directly counting subarrays with exactly **target** distinct elements, we can find the difference between:

- The number of subarrays with at most **target** distinct elements
- The number of subarrays with at most **target - 1** distinct elements

The difference between these two counts gives the number of subarrays with exactly **target** distinct elements.

**Function Definitions:**

1. **optimizedAtMostTarget Function:**
   - Initialize **cnt** (counter), **left**, **right** pointers, and **visited** dictionary
   - Traverse the array with the **right** pointer
   - Add elements to the **visited** dictionary and count their occurrences
   - If the number of distinct elements in **visited** exceeds **target**, increment the **left** pointer to shrink the window until the condition is met
   - Count all valid subarrays ending at **right** by adding **(right - left + 1)** to **cnt**

2. **optimized Function:**
   - Calculate the number of subarrays with at most **target** distinct elements
   - Calculate the number of subarrays with at most **target - 1** distinct elements
   - The difference between these two counts gives the number of subarrays with exactly **target** distinct elements

```python
def optimizedAtMostTarget(n,arr,target):
    cnt=0
    left,right=0,0
    visited={}
    while (right<n):
        if arr[right] in visited:
            visited[arr[right]]+=1
        else:
            visited[arr[right]]=1
        while len(visited)>target and left<=right:
            visited[arr[left]]-=1
            if(visited[arr[left]]==0):
                del visited[arr[left]]
            left+=1
        cnt+=(right-left+1)
        right+=1
    return cnt

def optimized(n,arr,target):
    tgtCnt=optimizedAtMostTarget(n,arr,target)
    tgtCnt2=optimizedAtMostTarget(n,arr,target-1)
    return tgtCnt-tgtCnt2

arr=list(map(int,input().split()))
n=len(arr)
target=int(input())
print(optimized(n,arr,target))
```

**Complexity Analysis:**
- **Time Complexity:** O(2n)
- **Space Complexity:** O(1) - We can store at most 256 characters in visited

---

## Maximum Points You Can Obtain from Cards

### Problem Statement
There are several cards arranged in a row, and each card has an associated number of points. The points are given in the integer array `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly `k` cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array `cardPoints` and the integer `k`, return the maximum score you can obtain.

### Examples

**Example 1:**
- **Input:** `cardPoints = [1,2,3,4,5,6,1]`, `k = 3`
- **Output:** `12`
- **Explanation:** After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.

**Example 2:**
- **Input:** `cardPoints = [2,2,2]`, `k = 2`
- **Output:** `4`
- **Explanation:** Regardless of which two cards you take, your score will always be 4.

**Example 3:**
- **Input:** `cardPoints = [9,7,7,9,7,7,9]`, `k = 7`
- **Output:** `55`
- **Explanation:** You have to take all the cards. Your score is the sum of points of all cards.

### Solutions

#### Approach 1: Brute Force

**Algorithm:**
We want to find the maximum sum we can get by picking a total of **k** elements from either the beginning or the end of the array.

**Approach:**
- We can choose some elements from the start of the array and the rest from the end
- We need to try all possible ways of distributing the **k** picks between the start and end of the array to find the best (maximum) sum

**Steps:**
1. Loop through all possible ways of splitting the **k** picks between the start and end
2. For each way, calculate the sum of the elements picked from the start and the sum of the elements picked from the end
3. Keep track of the maximum sum we can get from all these possible ways

**Example:**
- Suppose we have an array **[1, 2, 3, 4, 5]** and **k = 3**
- We can take:
  - 0 elements from the start and 3 from the end: **[3, 4, 5]**
  - 1 element from the start and 2 from the end: **[1] + [4, 5]**
  - 2 elements from the start and 1 from the end: **[1, 2] + [5]**
  - 3 elements from the start and 0 from the end: **[1, 2, 3]**
- We calculate the sum for each of these combinations and find the maximum

**Why It Works:**
- By considering all possible splits of the **k** picks between the start and end, we ensure that we don't miss any potential combination that might give us the maximum sum

```python
def bruteForce(n,arr,k):
    n=len(arr)
    maxi=0
    for i in range(k+1):
        leftSum=0
        for j in range(k-i):
            leftSum+=arr[j]
        rightSum=0
        for j in range(n-i,n):
            rightSum+=arr[j]
        total=leftSum+rightSum
        maxi=max(maxi,total)
    return maxi

arr=list(map(int,input().split()))
n=len(arr)
k=int(input())
print(bruteForce(n,arr,k))
```

**Complexity Analysis:**
- **Time Complexity:** O(n²)
- **Space Complexity:** O(1)

#### Approach 2: Optimized Approach

**Algorithm:**

**Initial Sum:**
- First, calculate the sum of the first **k** elements from the start of the array. This is our starting point.

**Shift Elements:**
1. Gradually shift one element from the start to the end of the array:
   - Subtract the element we move away from the start from the **leftSum**
   - Add the new element we take from the end to the **rightSum**
2. Each time you shift, calculate the new total sum (**leftSum** + **rightSum**)
3. Update the maximum sum (**maxi**) if the new total is greater

**Why It Works:**
- By shifting elements from the start to the end one by one, we consider all possible ways to split the **k** picks between the start and end
- This way, we find the maximum possible sum efficiently without recalculating sums from scratch

In essence, you start by taking all **k** elements from the start, then move one element from the start to the end one by one while keeping track of the maximum sum encountered.

```python
def optimized(n,arr,k):
    leftSum,rightSum=0,0
    left,right=-1,n-1
    maxi=0
    total=0
    for i in range(k):
        left+=1
        leftSum+=arr[left]
    maxi=max(maxi,leftSum)
    for i in range(k):
        leftSum-=arr[left]
        rightSum+=arr[right]
        right-=1
        left-=1
        total=leftSum+rightSum
        maxi=max(maxi,total)
    return maxi

arr=list(map(int,input().split()))
n=len(arr)
k=int(input())
print(optimized(n,arr,k))
```

**Complexity Analysis:**
- **Time Complexity:** O(n)
- **Space Complexity:** O(1)
