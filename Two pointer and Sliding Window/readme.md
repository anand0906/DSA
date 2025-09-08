# Two Pointer & Sliding Window

## 📋 Table of Contents
1. [Two Pointer Technique](#two-pointer-technique)
2. [Sliding Window Patterns](#sliding-window-patterns)

---

## Two Pointer Technique

### 🎯 What is Two Pointer?
The two-pointer technique is a fundamental algorithmic approach that plays a pivotal role in optimizing solutions to specific types of problems in computer science.

This strategy involves to use two/more pointers which points to indices of a data structure, that pointers traverse in data structure in different ways to achieve specific goal to solve problems efficiently.

By manipulating the positions of these two pointers, the technique aims to efficiently address a wide range of problems, making it a valuable tool in a programmer's toolkit.

### 📝 When To Use?

#### 1. **Searching for Pairs or Subarrays**
- When you need to find pairs of elements with certain properties, such as pairs that sum to a target value or pairs with a specific difference, the two-pointer technique can be very effective.
- **Example**: Finding a pair of numbers in a sorted array that sums to a target value.

#### 2. **Checking for Palindrome**
- When you need to check if a sequence is a palindrome (reads the same forwards and backwards), you can use two pointers starting from both ends of the sequence and move them toward each other while comparing elements.
- **Example**: Checking if a string is a palindrome.

#### 3. **Removing Duplicates in Sorted Arrays**
- When you have a sorted array and want to remove duplicate elements in-place, you can use two pointers to track the unique elements.
- **Example**: Removing duplicates from a sorted array.

#### 4. **Finding Triplets or Quadruplets**
- When you need to find triplets or quadruplets of elements with specific properties, you can use multiple pointers and combinations to explore different possibilities efficiently.
- **Example**: Finding triplets in an array that sum to a target value.

#### 5. **Merging or Intersecting Arrays**
- When you have two sorted arrays and want to merge them or find their intersection, the two-pointer technique can be employed to compare and merge elements in a single pass.
- **Example**: Merging two sorted arrays or finding the intersection of two sorted arrays.

#### 6. **Searching in a Sorted Matrix**
- When you have a 2D matrix with sorted rows and columns, you can use two pointers to navigate through the matrix efficiently while searching for a specific element.
- **Example**: Searching for a target element in a sorted matrix.

#### 7. **Sliding Window Problems**
- When solving problems that involve finding a subarray of fixed or variable length with specific properties (e.g., maximum sum, minimum length, etc.), two pointers can help maintain the subarray efficiently as you slide the window.
- **Example**: Finding the maximum sum of a subarray of a fixed size.

#### 8. **Partitioning and Sorting**
- When partitioning an array (e.g., QuickSort's partition step) or sorting elements with specific conditions, the two-pointer technique can be used to rearrange elements efficiently.
- **Example**: Implementing the partition step in QuickSort.

#### 9. **Linked List Problems**
- It can be used in problems involving linked lists, two pointers can be used to find cycles, determine intersections or merge sorted linked lists.
- **Example**: Check cycle present in linked list.

#### 10. **String Manipulation**
- Many string manipulation problems, such as checking for palindromes, finding sub-strings, or performing in-place replacements, can benefit from Two Pointers.
- **Example**: Check given string is palindrome or not.

### 🔧 How to Use?

1. Determine if the problem involves any above pattern. Check if any two positions are need to move simultaneously to solve problem in a linear data structure. Then we can use the approach
2. Determine what pointers should represent in given problem, indices or values..etc
3. Initialize pointers, depending up on the problem need, assign starting position for each pointer
4. Find out the condition for moving pointers
5. Determine, how pointers should move/change. Adjust pointers based on the current condition or requirement of the problem
6. Determine when to stop the moving of pointers
7. Consider edge cases such as empty arrays, single-element arrays, or scenarios where no valid pairs or subarrays exist

### 📊 Types of Two Pointers
*Based on direction and movement of pointers*

#### 1. **Collision**
- Two pointers start from opposite ends and move towards each other.
- **Example**: Finding two numbers in a sorted array that sum up to a target.

#### 2. **Forward/Backward**
- Two pointers move either forward or backward in the same direction through a single array.

#### 3. **Parallel**
- Two pointers simultaneously traverse two separate arrays or lists.
- **Example**: Finding intersections between two lists of intervals.

#### 4. **Sliding Window**
- Two pointers move in the same direction with a fixed difference between them.
- **Example**: Finding the maximum sum of a subarray of size k in an array.

#### 5. **Fast/Slow**
- Two pointers where one moves faster than the other.
- **Example**: Removing duplicates from a sorted array or detecting cycles in a linked list.

---

## Sliding Window Patterns

**Here's a detailed explanation of each sliding window pattern with real-world examples:**

### 1. **Fixed Sliding Window Pattern**

The window size is fixed, and you slide it over the array or string to perform calculations. This is useful when the window size is constant, and you need to process a sequence of consecutive elements.

#### Example 1: Maximum Sum Subarray of Size K
You are given an array, and you need to find the maximum sum of any contiguous subarray of size K.

- **Input**: [2, 1, 5, 1, 3, 2], K = 3
- **Output**: 9
- **Explanation**: The subarrays of size 3 are [2, 1, 5], [1, 5, 1], [5, 1, 3], and [1, 3, 2]. The maximum sum is 9 from the subarray [5, 1, 3].

#### Example 2: First Negative Integer in Every Window of Size K
In this problem, you are asked to find the first negative integer for every window of size K.

- **Input**: [12, -1, -7, 8, 15, 30, 16, 28], K = 3
- **Output**: [-1, -1, -7, -7, 0, 0]
- **Explanation**: For each window, you slide and check the first negative integer. If none exists, return 0.

### 2. **Dynamic Sliding Window Pattern**

In dynamic sliding window problems, the window size varies based on conditions, and you dynamically expand or contract the window until the condition is satisfied.

#### Example 1: Smallest Subarray with a Given Sum
You are given an array and a target sum. Find the smallest contiguous subarray whose sum is greater than or equal to the target.

- **Input**: [2, 1, 5, 2, 3, 2], Target = 7
- **Output**: 2
- **Explanation**: The smallest subarray with a sum of at least 7 is [5, 2] with a length of 2.

#### Example 2: Longest Substring with K Distinct Characters
Given a string, find the length of the longest substring that contains exactly K distinct characters.

- **Input**: "araaci", K = 2
- **Output**: 4
- **Explanation**: The longest substring with exactly 2 distinct characters is "araa" with a length of 4.

### 3. **Caterpillar Method (Two-Pointer Sliding Window)**

This method involves using two pointers (start and end) to expand and contract a window over the array. It's often used when you need to count subarrays that meet a certain condition.

#### Example 1: Number of Subarrays with Sum Equals K
Find the number of contiguous subarrays that sum up to a given value K.

- **Input**: [1, 1, 1], K = 2
- **Output**: 2
- **Explanation**: There are two subarrays [1, 1] that sum to 2.

#### Example 2: Subarray Product Less Than K
Given an array of integers, count the number of contiguous subarrays where the product of the elements is less than K.

- **Input**: [10, 5, 2, 6], K = 100
- **Output**: 8
- **Explanation**: The valid subarrays are [10], [5], [2], [6], [5, 2], [2, 6], [5, 2, 6], and [10, 5].

### 4. **Expanding/Shrinking Sliding Window Pattern**

This pattern is common for string problems where you need to adjust the window size dynamically by expanding and shrinking it to meet certain constraints (e.g., unique characters, distinct elements).

#### Example 1: Longest Substring Without Repeating Characters
Find the length of the longest substring without any repeating characters.

- **Input**: "abcabcbb"
- **Output**: 3
- **Explanation**: The longest substring without repeating characters is "abc", which has a length of 3.

#### Example 2: Longest Substring with At Most Two Distinct Characters
Given a string, find the length of the longest substring that contains at most 2 distinct characters.

- **Input**: "eceba"
- **Output**: 3
- **Explanation**: The longest substring with at most two distinct characters is "ece" with a length of 3.

### 5. **Exact Subarray Count Pattern**

In this pattern, the goal is to count the number of subarrays with exactly K elements or features. It involves finding subarrays with at most K elements and subtracting subarrays with at most K-1 elements.

#### Example 1: Subarrays with Exactly K Distinct Integers
Find the number of subarrays with exactly K distinct integers.

- **Input**: [1, 2, 1, 2, 3], K = 2
- **Output**: 7
- **Explanation**: The subarrays with exactly 2 distinct integers are [1, 2], [2, 1], [1, 2], [2, 3], [1, 2, 1], [2, 1, 2], and [1, 2, 3].

#### Example 2: Subarrays with Exactly K Odd Numbers
Find the number of subarrays with exactly K odd numbers.

- **Input**: [1, 2, 3, 4, 5], K = 2
- **Output**: 4
- **Explanation**: The subarrays with exactly two odd numbers are [1, 2, 3], [1, 2, 3, 4], [3, 4, 5], and [2, 3, 4, 5].

### 6. **Important LeetCode Problems**

Here are some well-known problems from LeetCode based on these patterns:

#### **Fixed Sliding Window:**
- Maximum Sum of K Consecutive Elements
- First Negative Integer in Every Window of Size K
- Average of Subarrays of Size K

#### **Dynamic Sliding Window:**
- Smallest Subarray with a Given Sum
- Longest Substring with K Distinct Characters
- Fruits into Baskets (Longest Subarray with At Most Two Distinct Characters)

#### **Caterpillar Method:**
- Number of Subarrays with Sum Equals K
- Subarray Product Less than K

#### **Expanding/Shrinking Sliding Window:**
- Longest Substring Without Repeating Characters
- Minimum Window Substring
