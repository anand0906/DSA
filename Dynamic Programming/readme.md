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
