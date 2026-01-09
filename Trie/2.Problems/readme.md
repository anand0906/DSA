# Trie Data Structure Problems

---

## Table of Contents
1. [Longest Word with All Prefixes](#1-longest-word-with-all-prefixes)
2. [Number of Distinct Substrings](#2-number-of-distinct-substrings)
3. [Maximum XOR of Two Numbers](#3-maximum-xor-of-two-numbers)
4. [Maximum XOR with Element Constraint](#4-maximum-xor-with-element-constraint)

---

## 1. Longest Word with All Prefixes

### Problem Description
Given a string array `nums` of length `n`, find the longest **complete string** where every prefix of this string is also present in the array. If there are multiple strings with the same length, return the lexicographically smallest one. If no string exists, return "None".

### Sample Test Cases

**Example 1:**
```
Input: nums = ["n", "ni", "nin", "ninj", "ninja", "nil"]
Output: "ninja"
```

**Example 2:**
```
Input: nums = ["ninja", "night", "nil"]
Output: "None"
```

### Visual Example

```
Array: ["n", "ni", "nin", "ninj", "ninja", "nil"]

Checking "ninja":
├─ "n" ✓ (exists in array)
├─ "ni" ✓ (exists in array)
├─ "nin" ✓ (exists in array)
├─ "ninj" ✓ (exists in array)
└─ "ninja" ✓ (exists in array)

All prefixes exist! ✓

Checking "nil":
├─ "n" ✓ (exists in array)
├─ "ni" ✓ (exists in array)
└─ "nil" ✓ (exists in array)

All prefixes exist! ✓

Result: "ninja" (longer than "nil")
```

---

### Approach 1: Brute Force

#### Intuition
For each word, check if all its prefixes exist in the array by searching through the entire array for each prefix.

#### Approach
1. For each word in the array
2. Generate all prefixes of that word
3. Check if each prefix exists in the array
4. Track the longest word with all prefixes present
5. Handle lexicographic ordering for ties

#### Code with Comments
```python
def solve_brute(n, arr):
    """
    Brute force approach to find longest complete string
    """
    max_len = 0
    ans = 'None'
    
    # Check each word in array
    for word in arr:
        all_prefixes_exist = True
        
        # Check all prefixes of current word
        for i in range(1, len(word) + 1):
            prefix = word[:i]
            if prefix not in arr:
                all_prefixes_exist = False
                break
        
        # Update answer if current word is valid
        if all_prefixes_exist:
            if len(word) > max_len:
                max_len = len(word)
                ans = word
            elif len(word) == max_len and word < ans:
                ans = word
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** O(n² × m²) where n is the number of words and m is average word length
  - For each word: O(n)
  - Generate prefixes: O(m)
  - Search in array: O(n × m)
- **Space Complexity:** O(1) - no extra space used

---

### Approach 2: Trie (Optimal)

#### Intuition
Use a Trie to efficiently check if all prefixes exist. In a Trie, we can mark each node as "end of word". When checking if all prefixes exist, we simply traverse the Trie and ensure each node we visit is marked as end of word.

#### Visual Representation of Trie

```
Insert: ["n", "ni", "nin", "ninj", "ninja", "nil"]

Trie Structure:
         root
          |
          n (end) ←─── "n"
          |
          i (end) ←─── "ni"
         / \
        n   l (end) ←─── "nil"
        |
        j (end) ←─── "ninj"
        |
        a (end) ←─── "ninja"

When checking "ninja":
- Start at root
- Go to 'n' → is it end? YES ✓
- Go to 'i' → is it end? YES ✓
- Go to 'n' → is it end? YES ✓
- Go to 'j' → is it end? YES ✓
- Go to 'a' → is it end? YES ✓
All prefixes exist!
```

#### Approach
1. Build a Trie and insert all words
2. Mark the end of each word with a flag
3. For each word, traverse the Trie checking if every node is marked as end
4. Track the longest valid word (lexicographically smallest for ties)

#### Code with Comments
```python
class Node:
    """Trie Node representing a single character"""
    
    def __init__(self):
        # Array to store 26 possible children (a-z)
        self.links = [None] * 26
        # Flag to mark end of a word
        self.flag = False
    
    def containsKey(self, char):
        """Check if character exists as child"""
        return self.links[ord(char) - ord('a')] is not None
    
    def getKey(self, char):
        """Get child node for character"""
        return self.links[ord(char) - ord('a')]
    
    def insertKey(self, char, node):
        """Insert child node for character"""
        self.links[ord(char) - ord('a')] = node
    
    def setEnd(self):
        """Mark this node as end of word"""
        self.flag = True
    
    def isEnd(self):
        """Check if this node marks end of word"""
        return self.flag


class Trie:
    """Trie data structure for efficient prefix operations"""
    
    def __init__(self):
        self.root = Node()
    
    def insert(self, word):
        """Insert a word into the Trie"""
        temp = self.root
        
        # Traverse/create path for each character
        for char in word:
            if not temp.containsKey(char):
                new = Node()
                temp.insertKey(char, new)
            temp = temp.getKey(char)
        
        # Mark end of word
        temp.setEnd()
    
    def checkIfAllPrefixExists(self, word):
        """Check if all prefixes of word exist in Trie"""
        temp = self.root
        
        for char in word:
            if temp.containsKey(char):
                temp = temp.getKey(char)
                # Each character must be an end of some word
                if not temp.isEnd():
                    return False
            else:
                return False
        
        return True


def solve(n, arr):
    """
    Find longest complete string using Trie
    """
    # Build Trie with all words
    trie = Trie()
    for word in arr:
        trie.insert(word)
    
    max_len = 0
    ans = 'None'
    
    # Check each word for complete prefixes
    for word in arr:
        if trie.checkIfAllPrefixExists(word):
            word_len = len(word)
            
            # Update if longer or lexicographically smaller
            if word_len > max_len:
                max_len = word_len
                ans = word
            elif word_len == max_len and word < ans:
                ans = word
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** O(n × m)
  - Building Trie: O(n × m) where n is number of words, m is average length
  - Checking each word: O(n × m)
  - Total: O(n × m)
- **Space Complexity:** O(n × m) for storing Trie structure

---

## 2. Number of Distinct Substrings

### Problem Description
Given a string `s`, determine the number of **distinct substrings** (including the empty substring).

### Sample Test Cases

**Example 1:**
```
Input: s = "aba"
Output: 6
Explanation: Distinct substrings are "", "a", "ab", "aba", "b", "ba"
```

**Example 2:**
```
Input: s = "abc"
Output: 7
Explanation: Distinct substrings are "", "a", "ab", "abc", "b", "bc", "c"
```

### Visual Example

```
String: "aba"

All substrings:
Starting at index 0: "a", "ab", "aba"
Starting at index 1: "b", "ba"
Starting at index 2: "a"

Distinct substrings (using set):
{"", "a", "ab", "aba", "b", "ba"}

Count = 6
```

---

### Approach 1: Brute Force with Set

#### Intuition
Generate all possible substrings and use a set to automatically handle duplicates.

#### Approach
1. Use two nested loops to generate all substrings
2. Add each substring to a set (automatically handles duplicates)
3. Add empty string separately
4. Return size of set

#### Code with Comments
```python
def solve_brute(n, s):
    """
    Generate all substrings and use set for uniqueness
    """
    unique = set()
    
    # Generate all substrings
    for i in range(n):
        for j in range(i, n):
            # Substring from index i to j (inclusive)
            unique.add(s[i:j+1])
    
    # Don't forget empty string
    unique.add("")
    
    return len(unique)
```

#### Complexity Analysis
- **Time Complexity:** O(n³)
  - Two nested loops: O(n²)
  - String slicing for each pair: O(n)
  - Total: O(n³)
- **Space Complexity:** O(n³) for storing all substrings in set

---

### Approach 2: Trie (Optimal)

#### Intuition
Instead of generating all substrings and storing them, we can use a Trie to count distinct substrings efficiently. Each time we insert a new character into the Trie during traversal, it represents a new distinct substring.

#### Visual Representation

```
String: "aba"

Insert all substrings starting at each index:

Starting at index 0:
  a → ab → aba

Starting at index 1:
  b → ba

Starting at index 2:
  a (already exists!)

Trie Structure:
       root
       /  \
      a    b
      |    |
      b    a
      |
      a

New nodes created: 5
+ 1 for empty string
Total distinct substrings: 6
```

#### Detailed Process

```
Processing "aba":

i=0 (substring starting at 0):
  j=0: 'a' → new node created ✓ (count=1)
  j=1: 'b' → new node created ✓ (count=2)
  j=2: 'a' → new node created ✓ (count=3)

i=1 (substring starting at 1):
  j=1: 'b' → already exists
  j=2: 'a' → new node created ✓ (count=4)

i=2 (substring starting at 2):
  j=2: 'a' → already exists

Total: 4 + 1 (empty) = 5... Wait!

Actually:
i=0: creates "a", "ab", "aba" (3 new)
i=1: "b" is new, "ba" is new (2 new)
i=2: "a" already exists (0 new)

Total: 3 + 2 + 0 + 1 = 6 ✓
```

#### Approach
1. For each starting position in the string
2. Insert characters one by one into Trie
3. Count each time a new node is created (new substring)
4. Add 1 for empty string
5. Return count

#### Code with Comments
```python
class Node:
    """Trie Node for substring counting"""
    
    def __init__(self):
        self.links = [None] * 26
        self.flag = False
    
    def containsKey(self, char):
        return self.links[ord(char) - ord('a')] is not None
    
    def getKey(self, char):
        return self.links[ord(char) - ord('a')]
    
    def insertKey(self, char, node):
        self.links[ord(char) - ord('a')] = node
    
    def setEnd(self):
        self.flag = True
    
    def isEnd(self):
        return self.flag


class Trie:
    """Trie for counting distinct substrings"""
    
    def __init__(self):
        self.root = Node()
    
    def cntDistinctSubStrings(self, n, word):
        """
        Count distinct substrings by tracking new nodes
        """
        cnt = 0
        
        # Start substring from each index
        for i in range(n):
            temp = self.root
            
            # Extend substring character by character
            for j in range(i, n):
                char = word[j]
                
                # If character doesn't exist, it's a new substring
                if not temp.containsKey(char):
                    new = Node()
                    temp.insertKey(char, new)
                    cnt += 1  # New distinct substring found
                
                # Move to next character
                temp = temp.getKey(char)
            
            temp.setEnd()
        
        # Add 1 for empty string
        cnt += 1
        return cnt


def solve(n, word):
    """
    Main function to count distinct substrings
    """
    trie = Trie()
    return trie.cntDistinctSubStrings(n, word)
```

#### Complexity Analysis
- **Time Complexity:** O(n²)
  - Outer loop: O(n) starting positions
  - Inner loop: O(n) characters per position
  - Total: O(n²)
- **Space Complexity:** O(n²) for Trie structure (worst case all substrings are unique)

---

## 3. Maximum XOR of Two Numbers

### Problem Description
Given an integer array `nums`, return the maximum result of `nums[i] XOR nums[j]`, where 0 ≤ i ≤ j < n.

### Sample Test Cases

**Example 1:**
```
Input: nums = [3, 9, 10, 5, 1]
Output: 15
Explanation: 10 XOR 5 = 15
```

**Example 2:**
```
Input: nums = [26, 49, 30, 15, 69]
Output: 116
Explanation: 69 XOR 49 = 116
```

### XOR Properties

```
XOR Truth Table:
0 XOR 0 = 0
0 XOR 1 = 1
1 XOR 0 = 1
1 XOR 1 = 0

Key Property: To maximize XOR, we want opposite bits!

Example: 10 XOR 5
  10 = 1010 (binary)
   5 = 0101 (binary)
  ──────────────────
 XOR = 1111 = 15

Maximum XOR occurs when bits are maximally different!
```

---

### Approach 1: Brute Force

#### Intuition
Try all possible pairs and compute XOR for each, tracking the maximum.

#### Approach
1. Use two nested loops for all pairs
2. Compute XOR for each pair
3. Track maximum XOR value

#### Code with Comments
```python
def solve_brute(n, arr):
    """
    Brute force: try all pairs
    """
    maxi = 0
    
    # Try all pairs
    for i in range(n):
        for j in range(i + 1, n):
            # Compute XOR and update maximum
            maxi = max(maxi, arr[i] ^ arr[j])
    
    return maxi
```

#### Complexity Analysis
- **Time Complexity:** O(n²) - all pairs
- **Space Complexity:** O(1)

---

### Approach 2: Binary Trie (Optimal)

#### Intuition
Store numbers in a binary Trie (each node has 2 children: 0 and 1). For each number, traverse the Trie trying to take the opposite bit at each level to maximize XOR.

#### Visual Representation

```
Numbers: [3, 9, 10]

Binary representations (32-bit, showing last 4 bits):
3  = 0011
9  = 1001
10 = 1010

Binary Trie:
                    root
                   /    \
                  0      1
                 /        \
               0           0
              /             \
            1                0
           /                  \
          1 (3)                1
                              /  \
                             0    1
                            /      \
                         (10)      (9)

Finding max XOR for 10 (1010):
- Bit 3 (1): Want 0? YES! Go to 0 branch
- Bit 2 (0): Want 1? NO, take 0 branch  
- Bit 1 (1): Want 0? YES! Go to 0 branch
- Bit 0 (0): Want 1? YES! Go to 1 branch

Path taken: 0-0-0-1 represents number 1 (0001)
Wait, that's wrong. Let me recalculate...

Actually for 10 = 1010:
- Bit 3 (1): Want 0? NO (no such path), take 1
- Bit 2 (0): Want 1? NO, take 0
- Bit 1 (1): Want 0? YES, take 0
- Bit 0 (0): Want 1? YES, take 1

Result: 1001 = 9
10 XOR 9 = 0011 = 3... Hmm, let me reconsider.

Better example: 10 XOR 5 = 15
10 = 1010
5  = 0101
────────────
XOR = 1111 = 15 (maximum possible with these bits)
```

#### Detailed Algorithm

```
For number 10 (1010):
1. Start from MSB (most significant bit)
2. Current bit = 1
   - Want opposite bit (0)? 
   - If exists: take it (XOR will be 1)
   - Else: take same bit (XOR will be 0)
3. Repeat for all bits
4. Build result by setting bits where we took opposite
```

#### Approach
1. Build a binary Trie with all numbers (32 bits each)
2. For each number, traverse Trie trying to take opposite bit
3. Calculate XOR value from path taken
4. Return maximum XOR

#### Code with Comments
```python
class Node:
    """Binary Trie Node (0 or 1)"""
    
    def __init__(self):
        # Only 2 children for binary representation
        self.links = [None] * 2
        self.flag = False
    
    def containsKey(self, bit):
        """Check if bit (0 or 1) exists"""
        return self.links[bit] is not None
    
    def getKey(self, bit):
        """Get child node for bit"""
        return self.links[bit]
    
    def putKey(self, bit, node):
        """Insert child node for bit"""
        self.links[bit] = node
    
    def setEnd(self):
        self.flag = True


class Trie:
    """Binary Trie for XOR operations"""
    
    def __init__(self):
        self.root = Node()
    
    def insert(self, num):
        """Insert number into binary Trie (32 bits)"""
        temp = self.root
        
        # Process from MSB to LSB (31 to 0)
        for i in range(31, -1, -1):
            # Extract i-th bit
            bit = (num >> i) & 1
            
            # Create node if doesn't exist
            if not temp.containsKey(bit):
                new = Node()
                temp.putKey(bit, new)
            
            temp = temp.getKey(bit)
        
        temp.setEnd()
    
    def getMax(self, x):
        """Find maximum XOR of x with any number in Trie"""
        temp = self.root
        max_num = 0
        
        # Traverse from MSB to LSB
        for i in range(31, -1, -1):
            # Get i-th bit of x
            bit = (x >> i) & 1
            
            # Try to take opposite bit for maximum XOR
            # In Python, ~bit doesn't work as expected for single bit
            # We need opposite bit: if bit=0, want 1; if bit=1, want 0
            opposite_bit = 1 - bit
            
            if temp.containsKey(opposite_bit):
                # Take opposite bit (XOR will give 1 at this position)
                max_num = max_num | (1 << i)
                temp = temp.getKey(opposite_bit)
            else:
                # Take same bit (XOR will give 0 at this position)
                temp = temp.getKey(bit)
        
        return max_num


def solve(n, arr):
    """
    Find maximum XOR using Binary Trie
    """
    # Build Trie with all numbers
    trie = Trie()
    for num in arr:
        trie.insert(num)
    
    max_xor = 0
    
    # For each number, find max XOR with Trie
    for num in arr:
        maxi = trie.getMax(num)
        max_xor = max(max_xor, maxi)
    
    return max_xor
```

#### Note on Implementation
The original code had a bug with `~bit`. In Python, `~` is bitwise NOT which gives `~0 = -1` and `~1 = -2`, not the expected 1 and 0. The corrected version uses `1 - bit` to get the opposite bit.

#### Complexity Analysis
- **Time Complexity:** O(n × 32) = O(n)
  - Insert n numbers: O(n × 32)
  - Query n numbers: O(n × 32)
  - Total: O(n) since 32 is constant
- **Space Complexity:** O(n × 32) = O(n) for Trie structure

---

## 4. Maximum XOR with Element Constraint

### Problem Description
Given an array `nums` and a `queries` array where `queries[i] = [xi, mi]`, find the answer to the i-th query: the maximum bitwise XOR value of `xi` and any element of `nums` that does not exceed `mi`. Return -1 if all elements are larger than `mi`.

### Sample Test Cases

**Example 1:**
```
Input: nums = [4, 9, 2, 5, 0, 1], queries = [[3, 0], [3, 10], [7, 5], [7, 9]]
Output: [3, 10, 7, 14]

Explanation:
Query 1: x=3, m=0 → Only 0 ≤ 0, so 0 XOR 3 = 3
Query 2: x=3, m=10 → Max is 3 XOR 9 = 10
Query 3: x=7, m=5 → Max is 7 XOR 0 = 7
Query 4: x=7, m=9 → Max is 7 XOR 9 = 14
```

**Example 2:**
```
Input: nums = [0, 1, 2, 3, 4], queries = [[3, 1], [1, 3], [5, 6]]
Output: [3, 3, 7]
```

### Visual Example

```
nums = [4, 9, 2, 5, 0, 1]
Query: x=3, m=5

Step 1: Filter elements ≤ 5
Valid elements: [4, 2, 5, 0, 1]

Step 2: Find max XOR with 3
3 XOR 4 = 7
3 XOR 2 = 1
3 XOR 5 = 6
3 XOR 0 = 3
3 XOR 1 = 2

Maximum: 7
```

---

### Approach 1: Brute Force

#### Intuition
For each query, iterate through array and find maximum XOR among valid elements (≤ m).

#### Approach
1. For each query (x, m)
2. Iterate through all numbers
3. If number ≤ m, compute XOR with x
4. Track maximum XOR
5. Return -1 if no valid numbers

#### Code with Comments
```python
def solve_brute(n, arr, queries):
    """
    Brute force: check all elements for each query
    """
    ans = []
    
    for x, m in queries:
        max_xor = -1
        
        # Check each element
        for i in range(n):
            if arr[i] <= m:
                # Update maximum XOR
                max_xor = max(max_xor, arr[i] ^ x)
        
        ans.append(max_xor)
    
    return ans
```

#### Complexity Analysis
- **Time Complexity:** O(q × n) where q is number of queries
- **Space Complexity:** O(q) for result array

---

### Approach 2: Offline Queries + Binary Trie (Optimal)

#### Intuition
Key insight: If we sort both array and queries by the constraint value `m`, we can progressively build our Trie! Process queries in increasing order of `m`, and insert elements into Trie as they become valid.

#### Visual Representation

```
nums = [4, 9, 2, 5, 0, 1]
Sorted nums = [0, 1, 2, 4, 5, 9]

queries = [[3,0], [3,10], [7,5], [7,9]]
Sorted by m = [[3,0], [7,5], [7,9], [3,10]]

Process:
┌─────────────────────────────────────────┐
│ Query 1: x=3, m=0                       │
│ Insert elements ≤ 0: [0]                │
│ Trie: {0}                               │
│ Max XOR: 0 XOR 3 = 3                    │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│ Query 2: x=7, m=5                       │
│ Insert elements ≤ 5: [1, 2, 4, 5]       │
│ Trie: {0, 1, 2, 4, 5}                   │
│ Max XOR: 7 XOR 0 = 7                    │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│ Query 3: x=7, m=9                       │
│ Insert elements ≤ 9: [9]                │
│ Trie: {0, 1, 2, 4, 5, 9}                │
│ Max XOR: 7 XOR 9 = 14                   │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│ Query 4: x=3, m=10                      │
│ No new elements (all ≤ 10 already in)   │
│ Trie: {0, 1, 2, 4, 5, 9}                │
│ Max XOR: 3 XOR 9 = 10                   │
└─────────────────────────────────────────┘
```

#### Algorithm Steps

```
1. Sort array: O(n log n)
2. Sort queries by m (keep track of original indices): O(q log q)
3. Process queries in sorted order:
   - Insert all elements ≤ current m into Trie
   - Find max XOR for current x
   - Store result at original query index
4. Return results in original order
```

#### Approach
1. Sort array in ascending order
2. Sort queries by constraint value `m`, keeping original indices
3. Maintain pointer to track which elements are inserted
4. Process queries in sorted order:
   - Insert elements into Trie up to current `m`
   - Query Trie for maximum XOR
   - Store result at original index
5. Return results

#### Code with Comments
```python
class Node:
    """Binary Trie Node"""
    
    def __init__(self):
        self.links = [None] * 2
        self.flag = False
    
    def containsKey(self, bit):
        return self.links[bit] is not None
    
    def getKey(self, bit):
        return self.links[bit]
    
    def putKey(self, bit, node):
        self.links[bit] = node
    
    def setEnd(self):
        self.flag = True


class Trie:
    """Binary Trie for XOR operations"""
    
    def __init__(self):
        self.root = Node()
    
    def insert(self, num):
        """Insert number into Trie"""
        temp = self.root
        
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            if not temp.containsKey(bit):
                new = Node()
                temp.putKey(bit, new)
            temp = temp.getKey(bit)
        
        temp.setEnd()
    
    def getMax(self, x):
        """Find maximum XOR with x"""
        temp = self.root
        max_num = 0
        
        for i in range(31, -1, -1):
            bit = (x >> i) & 1
            opposite_bit = 1 - bit
            
            if temp.containsKey(opposite_bit):
                max_num = max_num | (1 << i)
                temp = temp.getKey(opposite_bit)
            else:
                temp = temp.getKey(bit)
        
        return max_num


def solve(n, arr, queries):
    """
    Offline query processing with Binary Trie
    """
    # Sort array in ascending order
    arr.sort()
    
    # Initialize result array with -1
    ans = [-1] * len(queries)
    
    # Create list of (x, m, original_index) and sort by m
    updated_queries = []
    for index, (x, m) in enumerate(queries):
        updated_queries.append((x, m, index))
    
    # Sort queries by constraint value m
    updated_queries.sort(key=lambda x: x[1])
    
    # Pointer for array elements
    current = 0
    trie = Trie()
    
    # Process queries in sorted order
    for x, m, index in updated_queries:
        # Insert all elements ≤ m into Trie
        while current < n and arr[current] <= m:
            trie.insert(arr[current])
            current += 1
        
        # If at least one element inserted, find max XOR
        if current > 0:
            max_xor = trie.getMax(x)
            ans[index] = max_xor
        # else: ans[index] remains -1
    
    return ans


class Solution:
    def maximizeXor(self, nums, queries):
        """Wrapper function for solve"""
        n = len(nums)
        return solve(n, nums, queries)
```

#### Why This Works

```
Key Insight: Progressive Building

Instead of:
- Query 1: Build Trie with valid elements, query, destroy
- Query 2: Build Trie with valid elements, query, destroy
- ...

We do:
- Sort queries by m
- Build Trie incrementally
- Each element is inserted exactly once
- Trie only grows, never shrinks

This works because:
- If element is valid for m₁, it's valid for all m₂ > m₁
- By processing queries in order, we never need to remove elements
```

#### Complexity Analysis
- **Time Complexity:** O((n + q) × 32 + n log n + q log q)
  - Sort array: O(n log n)
  - Sort queries: O(q log q)
  - Insert elements: O(n × 32)
  - Process queries: O(q × 32)
  - Total: O(n log n + q log q + n + q) = O((n + q) log(n + q))
- **Space Complexity:** O(n × 32) = O(n) for Trie structure

---

## Summary Table

| Problem | Brute Force | Optimal | Data Structure |
|---------|-------------|---------|----------------|
| Longest Complete String | O(n²m²) | O(nm) | Trie |
| Distinct Substrings | O(n³) | O(n²) | Trie |
| Max XOR | O(n²) | O(n) | Binary Trie |
| Max XOR with Constraint | O(qn) | O((n+q)log(n+q)) | Binary Trie + Sorting |

---

## Key Takeaways

1. **Trie for Prefix Operations**: Tries excel at problems involving prefixes, strings, and pattern matching

2. **Binary Trie for XOR**: Use binary representation in Trie for XOR optimization problems

3. **Space-Time Tradeoff**: Tries use extra space but provide significant time improvements

4. **Offline Query Processing**: Sorting queries can enable incremental solutions that reuse computation

5. **Greedy Bit Selection**: For XOR maximization, greedily choose opposite bits from MSB to LSB

---

## Practice Tips

- Master basic Trie operations (insert, search, prefix check)
- Understand binary representation and bit manipulation
- Practice sorting and offline query techniques
- Visualize Trie structure for better understanding
- Consider space complexity carefully for large inputs
