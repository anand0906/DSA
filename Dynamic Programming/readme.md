# Dynamic Programming

## What is Dynamic Programming?

Dynamic Programming (DP) is a technique to solve complex problems by breaking them into smaller sub-problems. The result of each sub-problem is saved so that the same sub-problem doesn't have to be computed again. These sub-problems are then optimized together to find the original solution.

## Prerequisites for DP

Dynamic Programming can only be applied if the problem has:

### 1. Overlapping Sub-problems
When a problem is divided into multiple sub-problems, if any sub-problem is repeated more than once, those are called overlapping sub-problems.

### 2. Optimal Substructure
After breaking down the problem into sub-problems, if the solution for the main problem can be constructed from the optimal solutions of its sub-problems, the problem has an optimal substructure.

---

## Two Approaches to Dynamic Programming

### Top-Down Approach
- Problem computation starts from the main problem and breaks into sub-problems
- Achieved using **recursion**
- Also known as **Memoization**

### Bottom-Up Approach
- Problem computation starts from sub-problems and leads to the main problem
- Achieved using **iteration**
- Also known as **Tabulation**

---

## Step-by-Step Problem-Solving Framework

### Step 1: Define The Problem
- Understand the problem clearly
- Observe what needs to be done in each step to find the final answer

### Step 2: Represent the Problem Programmatically
- Choose the right data structure to solve the problem
- Represent the problem in terms of indexes or state variables

### Step 3: Find Base Cases
- Identify smaller values/inputs that don't need decomposition into sub-problems
- These values act as base cases for the main problem

### Step 4: Find The Recurrence Relation
- Observe steps to solve the problem using various inputs from smaller to bigger
- Express the solution in terms of solutions to smaller sub-problems
- Derive a mathematical formula connecting the current problem with smaller sub-problems

### Step 5: Recursive Solution (Top-Down)
- Implement the recursive solution using the recurrence relation and base cases
- This provides a clear, intuitive solution but may be inefficient

### Step 6: Apply Memoization (Top-Down)
- Store solutions of the recursive approach to avoid redundant calculations
- Significantly reduces time complexity
- Uses additional space to store computed results

### Step 7: Iterative Solution (Bottom-Up)
- Implement an iterative solution that solves smaller problems first
- Build up to the solution of the original problem
- Use arrays or tables to store solutions for each sub-problem
- These stored solutions are used for subsequent cases

### Step 8: Space Optimization (Bottom-Up)
- Modify the iterative solution to store only required values
- Instead of maintaining the entire table, keep only necessary previous states
- Improves space complexity significantly

### Step 9: Analyze Complexity
- Calculate time and space complexity for each approach
- Compare different approaches and optimize according to requirements
- Choose the best approach based on constraints

---

## Complexity Progression

| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Naive Recursion | Exponential | O(recursion depth) |
| Memoization | Polynomial | O(number of states) |
| Tabulation | Polynomial | O(number of states) |
| Space Optimized | Polynomial | O(few states) |

--

# Dynamic Programming Problems 

## 📑 Complete Problem Catalog (56 Problems)

---

## 🔹 **1D DP - FOUNDATIONAL PROBLEMS**
*Basic 1D dynamic programming concepts*

### **Subtype: Basic Recursion to DP**
1. Fibonacci Number
2. Climbing Stairs
3. Climbing Stairs with K Steps

### **Subtype: Jump Problems**
4. Frog Jump
5. Frog Jump with K Distances

### **Subtype: Non-Adjacent Selection**
6. Maximum Sum of Non-Adjacent Elements
7. House Robber (Circular Street)

### **Subtype: Multiple Choices**
8. Ninja's Training

---

## 🔹 **2D DP - GRID PROBLEMS**
*Path-based problems on 2D grids*

### **Subtype: Counting Paths**
9. Grid Unique Paths
10. Unique Paths II (With Obstacles)

### **Subtype: Minimum Path Cost**
11. Minimum Path Sum
12. Minimum Falling Path Sum
13. Triangle - Minimum Path Sum

### **Subtype: Advanced Grid Problems**
14. Cherry Pickup II

---

## 🔹 **DP ON STOCKS**
*Buy and sell stock problems*

### **Subtype: Transaction Limits**
15. Best Time to Buy and Sell Stock (One Transaction)
16. Best Time to Buy and Sell Stock II (Unlimited Transactions)
17. Best Time to Buy and Sell Stock III & IV (At Most K Transactions)

### **Subtype: With Constraints**
18. Best Time to Buy and Sell Stock with Transaction Fee
19. Buy and Sell Stocks with Cooldown

---

## 🔹 **SUBSET DP / 0-1 KNAPSACK PATTERN**
*Subset selection and knapsack variants*

### **Subtype: Subset Sum Problems**
20. Subset Sum Equals to Target
21. Partition Equal Subset Sum
22. Partition with Minimum Absolute Sum Difference

### **Subtype: Counting Subsets**
23. Count Subsets with Sum K
24. Count Partitions with Given Difference / Target Sum

### **Subtype: Knapsack Variants**
25. 0/1 Knapsack
26. Unbounded Knapsack
27. Rod Cutting Problem

### **Subtype: Coin Problems**
28. Minimum Coins
29. Coin Change II

---

## 🔹 **DP ON SUBSEQUENCES - LIS PATTERN**
*Longest Increasing Subsequence variants*

30. Longest Increasing Subsequence (LIS)
31. LIS using Binary Search
32. Print Longest Increasing Subsequence
33. Largest Divisible Subset
34. Longest String Chain
35. Longest Bitonic Subsequence
36. Number of Longest Increasing Subsequences

---

## 🔹 **DP ON STRINGS - LCS PATTERN**
*Longest Common Subsequence and string matching*

### **Subtype: LCS Variants**
37. Longest Common Subsequence (LCS)
38. Print Longest Common Subsequence
39. Longest Common Substring

### **Subtype: Palindrome Problems**
40. Longest Palindromic Subsequence
41. Minimum Insertions to Make String Palindrome

### **Subtype: String Matching**
42. Shortest Common Supersequence
43. Distinct Subsequences
44. Edit Distance
45. Wildcard Matching

---

## 🔹 **DP ON PARTITION / MCM PATTERN**
*Matrix Chain Multiplication and partition problems*

46. Matrix Chain Multiplication
47. Minimum Cost to Cut a Stick
48. Burst Balloons
49. Palindrome Partitioning II
50. Partition Array for Maximum Sum

---

## 📊 **SUMMARY BY CATEGORY**

### **🟢 1D DP** (8 problems)
→ Problems: 1-8

### **🟢 2D DP - Grids** (6 problems)
→ Problems: 9-14

### **🟡 Stocks** (5 problems)
→ Problems: 15-19

### **🟡 Subset/Knapsack** (10 problems)
→ Problems: 20-29

### **🔴 LIS Pattern** (7 problems)
→ Problems: 30-36

### **🔴 Strings/LCS** (9 problems)
→ Problems: 37-45

### **🔴 Partition/MCM** (5 problems)
→ Problems: 46-50

---

## 📈 **LEARNING PATH RECOMMENDATION**

### **Phase 1: DP Fundamentals** (Start Here)
```
1 → 2 → 3 → 4 → 5 → 6 → 7 → 8
```
*Master basic recursion → memoization → tabulation*

### **Phase 2: Grid DP**
```
9 → 10 → 11 → 12 → 13 → 14
```
*Learn 2D state transitions and path problems*

### **Phase 3: Stocks**
```
15 → 16 → 19 → 18 → 17
```
*Understand state machines and constraints*

### **Phase 4: Subset & Knapsack**
```
20 → 21 → 22 → 23 → 24 → 25 → 26 → 27 → 28 → 29
```
*Master inclusion-exclusion pattern*

### **Phase 5: Subsequences**
```
30 → 31 → 32 → 37 → 38 → 39 → 33 → 34 → 35 → 36
```
*Learn LIS and LCS patterns*

### **Phase 6: Advanced Strings**
```
40 → 41 → 42 → 43 → 44 → 45
```
*String transformations and matching*

### **Phase 7: Partition Problems**
```
46 → 47 → 48 → 49 → 50
```
*Master interval/partition DP*

---

## 🎯 **CONCEPT CLUSTERING**

### **Cluster 1: Basic Recursion → DP**
*Foundation building*
- Simple Recursion: 1, 2
- With Choices: 3, 4, 5
- Optimization: 6, 7, 8

### **Cluster 2: Path Problems**
*Grid traversal and optimization*
- Counting: 9, 10
- Cost Minimization: 11, 12, 13
- Complex Paths: 14

### **Cluster 3: State Machine DP**
*Problems with states and transitions*
- Stock Trading: 15, 16, 17, 18, 19

### **Cluster 4: Subset Selection**
*Pick or not pick pattern*
- Target Sum: 20, 21, 22
- Counting: 23, 24
- Value Optimization: 25, 26, 27, 28, 29

### **Cluster 5: Increasing Patterns**
*Subsequence with ordering*
- Basic LIS: 30, 31, 32
- LIS Variants: 33, 34, 35, 36

### **Cluster 6: String Matching**
*Two-string DP*
- Common Patterns: 37, 38, 39
- Palindromes: 40, 41
- Transformations: 42, 43, 44, 45

### **Cluster 7: Interval Optimization**
*Partition and combine*
- MCM Pattern: 46, 47, 48
- Partitioning: 49, 50

---

## 💡 **PROBLEM PAIRING** *(Similar Concepts)*

**Pair 1:** Climbing Stairs (2) ↔ Climbing Stairs with K Steps (3)

**Pair 2:** Frog Jump (4) ↔ Frog Jump with K Distances (5)

**Pair 3:** Max Sum Non-Adjacent (6) ↔ House Robber Circular (7)

**Pair 4:** Unique Paths (9) ↔ Unique Paths II (10)

**Pair 5:** Subset Sum (20) ↔ Partition Equal Subset (21)

**Pair 6:** 0/1 Knapsack (25) ↔ Unbounded Knapsack (26)

**Pair 7:** Minimum Coins (28) ↔ Coin Change II (29)

**Pair 8:** LIS (30) ↔ LIS Binary Search (31)

**Pair 9:** LCS (37) ↔ Print LCS (38)

**Pair 10:** LCS (37) ↔ LCS Substring (39)

**Pair 11:** Edit Distance (44) ↔ Wildcard Matching (45)

**Pair 12:** Stock I (15) ↔ Stock II (16)

---

## 🔍 **BY DIFFICULTY LEVEL**

### **🟢 Easy** (Beginner-Friendly)
- Problems: 1, 2, 4, 6, 9, 11, 15

### **🟡 Medium** (Core Concepts)
- Problems: 3, 5, 7, 8, 10, 12, 13, 16, 18, 19, 20, 21, 23, 25, 26, 27, 28, 29, 30, 33, 37, 39, 40, 49, 50

### **🔴 Hard** (Advanced Techniques)
- Problems: 14, 17, 22, 24, 31, 32, 34, 35, 36, 38, 41, 42, 43, 44, 45, 46, 47, 48

---

## 🎓 **TECHNIQUE-WISE GROUPING**

### **Memoization (Top-Down)**
→ All problems can use memoization

### **Tabulation (Bottom-Up)**
→ Recommended for: 1-29, 37-45

### **Space Optimization**
→ Applicable to: 1-8, 15-19, 20-29, 37-39

### **Binary Search Optimization**
→ Problems: 31

### **State Machine**
→ Problems: 15-19

### **2D DP Table**
→ Problems: 9-14, 20-29, 37-45

### **3D DP Table**
→ Problems: 14, 17, 43

### **Interval DP**
→ Problems: 46-50

---

## 📚 **PATTERN RECOGNITION GUIDE**

### **When you see: "Count ways to reach..."**
→ Use: 1D DP (Problems 2, 3, 9, 10)

### **When you see: "Minimum/Maximum path sum..."**
→ Use: Grid DP (Problems 11, 12, 13)

### **When you see: "Buy and sell..."**
→ Use: State Machine DP (Problems 15-19)

### **When you see: "Subset with sum..."**
→ Use: Subset DP (Problems 20-24)

### **When you see: "0/1 Knapsack / Unbounded..."**
→ Use: Knapsack Pattern (Problems 25-29)

### **When you see: "Longest increasing/common..."**
→ Use: LIS/LCS Pattern (Problems 30-39)

### **When you see: "Edit/Transform string..."**
→ Use: String DP (Problems 44, 45)

### **When you see: "Partition/Split optimally..."**
→ Use: MCM Pattern (Problems 46-50)

---

## 🏆 **MILESTONE PROBLEMS**
*Master these to understand core patterns*

1. **Fibonacci (1)** - Basic recursion to DP
2. **Climbing Stairs (2)** - Choice-based DP
3. **Unique Paths (9)** - 2D DP introduction
4. **Stock I (15)** - State machine basics
5. **Subset Sum (20)** - Inclusion-exclusion pattern
6. **0/1 Knapsack (25)** - Classic optimization
7. **LIS (30)** - Subsequence pattern
8. **LCS (37)** - Two-string DP
9. **Edit Distance (44)** - Complex string DP
10. **MCM (46)** - Interval DP pattern

---

## 📊 **TOTAL STATISTICS**

- **Total Problems:** 50
- **1D DP:** 8 problems (16%)
- **2D DP:** 42 problems (84%)
- **Easy:** 7 problems (14%)
- **Medium:** 25 problems (50%)
- **Hard:** 18 problems (36%)

---

## 🗺️ **QUICK NAVIGATION BY NUMBER**

**1-10:** Foundational & Grid Problems  
**11-20:** Grid Paths & Stock Trading Begins  
**21-30:** Subset/Knapsack & LIS Begins  
**31-40:** LIS Variants & LCS Pattern  
**41-50:** Advanced Strings & Partition Problems

---
