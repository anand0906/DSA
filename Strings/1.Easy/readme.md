## 📑 Table of Contents

1. [Reverse a String II](#1-reverse-a-string-ii)
2. [Palindrome Check](#2-palindrome-check)
3. [Largest Odd Number in a String](#3-largest-odd-number-in-a-string)
4. [Longest Common Prefix](#4-longest-common-prefix)
5. [Isomorphic String](#5-isomorphic-string)
6. [Rotate String](#6-rotate-string)
7. [Valid Anagram](#7-valid-anagram)
8. [Sort Characters by Frequency](#8-sort-characters-by-frequency)

---

## 1. Reverse a String II

### Problem Statement
Given a string represented by an array of characters `s`, reverse it in-place with O(1) extra memory.

### Examples

**Example 1:**
```
Input:  s = ["h", "e", "l", "l", "o"]
Output: ["o", "l", "l", "e", "h"]
Explanation: "hello" becomes "olleh"
```

**Example 2:**
```
Input:  s = ["b", "y", "e"]
Output: ["e", "y", "b"]
Explanation: "bye" becomes "eyb"
```

### 💡 Intuition
- Use the **two-pointer technique** to swap characters from both ends
- Move pointers toward the center until they meet
- This achieves O(1) space complexity (in-place reversal)

### 🎯 Approach

**Two Pointer Method:**
1. Initialize `left` pointer at start (index 0)
2. Initialize `right` pointer at end (index n-1)
3. While `left < right`:
   - Swap `arr[left]` and `arr[right]`
   - Increment `left`, decrement `right`
4. Array is reversed in-place

### 📝 Visual Representation

```
Initial:  [h, e, l, l, o]
           ↑           ↑
         left        right

Step 1:   [o, e, l, l, h]  (swap h and o)
              ↑     ↑
            left  right

Step 2:   [o, l, l, e, h]  (swap e and l)
                 ↑
              left=right (stop)

Final:    [o, l, l, e, h]
```

### 💻 Code Implementation

```python
def solve(n, arr):
    # Initialize two pointers
    left, right = 0, n - 1
    
    # Continue until pointers meet
    while left < right:
        # Swap characters at left and right positions
        temp = arr[left]
        arr[left] = arr[right]
        arr[right] = temp
        
        # Move pointers toward center
        left += 1
        right -= 1
    
    return arr  # Return modified array
```

### ⏱️ Complexity Analysis

- **Time Complexity:** O(n)
  - We iterate through half the array (n/2 iterations)
  - Each swap is O(1)
  
- **Space Complexity:** O(1)
  - Only using two pointers and one temp variable
  - In-place modification

---

## 2. Palindrome Check

### Problem Statement
Given a string `s`, return `true` if the string is a palindrome, otherwise `false`. A palindrome reads the same forward and backward.

### Examples

**Example 1:**
```
Input:  s = "hannah"
Output: true
Explanation: "hannah" reversed is "hannah" (same)
```

**Example 2:**
```
Input:  s = "aabbaaa"
Output: false
Explanation: "aabbaaa" reversed is "aaabbaa" (different)
```

### 💡 Intuition
- A palindrome has matching characters from both ends moving inward
- Use two pointers to compare characters from start and end
- If any mismatch occurs, it's not a palindrome

### 🎯 Approach

**Two Pointer Comparison:**
1. Place `left` at start, `right` at end
2. While `left < right`:
   - If characters don't match, return `false`
   - Move both pointers inward
3. If loop completes, return `true`

### 📝 Visual Representation

```
Example: "hannah"
          ↓     ↓
         [h a n n a h]
          ↑           ↑
        match? YES

          [h a n n a h]
            ↑       ↑
          match? YES

          [h a n n a h]
              ↑   ↑
            match? YES

Result: true (all matched)
```

### 💻 Code Implementation

```python
def solve(n, s):
    # Initialize pointers at both ends
    left, right = 0, n - 1
    
    # Compare characters from both ends
    while left < right:
        # If characters don't match, not a palindrome
        if s[left] != s[right]:
            return False
        
        # Move pointers inward
        left += 1
        right -= 1
    
    # All characters matched
    return True
```

### ⏱️ Complexity Analysis

- **Time Complexity:** O(n)
  - Compare n/2 character pairs in worst case
  
- **Space Complexity:** O(1)
  - Only using two pointer variables

---

## 3. Largest Odd Number in a String

### Problem Statement
Given a string `s` representing a large integer, return the largest-valued odd integer (as a string) that is a substring. The result should not have leading zeros.

### Examples

**Example 1:**
```
Input:  s = "5347"
Output: "5347"
Explanation: Odd numbers: 5, 3, 53, 347, 5347
             Largest: 5347
```

**Example 2:**
```
Input:  s = "0214638"
Output: "21463"
Explanation: Odd numbers: 1, 3, 21, 63, 463, 1463, 21463
             Cannot include 021463 (leading zero)
             Largest: 21463
```

### 💡 Intuition
- For a number to be odd, its **last digit must be odd**
- To maximize value: start from the **first non-zero digit**, end at the **last odd digit**
- Key insight: We don't need to check all substrings

### 🎯 Approach

**Greedy Two-Pass:**
1. Find `startIndex`: first non-zero character (skip leading zeros)
2. Find `endIndex`: last odd digit from the end
3. If `startIndex > endIndex`, no valid odd number exists
4. Return substring from `startIndex` to `endIndex`

### 📝 Visual Representation

```
Example: "0214638"
          
Step 1: Skip leading zeros
        0 2 1 4 6 3 8
        ↓ ↑
      skip start

Step 2: Find last odd digit from end
        0 2 1 4 6 3 8
                  ↑ ↓
               last odd (3)

Result: s[1:6] = "21463"
```

### 💻 Code Implementation

```python
def solve(n, s):
    # Find the first non-zero character (skip leading zeros)
    startIndex = 0
    while startIndex < n:
        if s[startIndex] != '0':
            break
        startIndex += 1
    
    # Find the last odd digit from the end
    endIndex = n - 1
    while endIndex >= 0:
        if int(s[endIndex]) % 2 == 1:  # Check if digit is odd
            break
        endIndex -= 1
    
    # If no valid range exists, return empty string
    if startIndex > endIndex:
        return ''
    
    # Return the substring from start to end (inclusive)
    return s[startIndex:endIndex + 1]
```

### ⏱️ Complexity Analysis

- **Time Complexity:** O(n)
  - Two passes through the string (one forward, one backward)
  
- **Space Complexity:** O(1)
  - Only using index variables (output string doesn't count)

---

## 4. Longest Common Prefix

### Problem Statement
Find the longest common prefix string amongst an array of strings. If no common prefix exists, return an empty string.

### Examples

**Example 1:**
```
Input:  ["flowers", "flow", "fly", "flight"]
Output: "fl"
Explanation: All strings share "fl" prefix
```

**Example 2:**
```
Input:  ["dog", "cat", "animal", "monkey"]
Output: ""
Explanation: No common prefix
```

### 💡 Intuition
- Common prefix must be present in ALL strings
- Two approaches:
  1. **Vertical scanning**: Compare character-by-character across all strings
  2. **Sorting trick**: After sorting, only compare first and last strings

### 🎯 Approach 1: Vertical Scanning

1. Compare characters at same index across all strings
2. If all match, add to result and move to next index
3. Stop when mismatch found or any string ends

### 📝 Visual Representation (Approach 1)

```
Index:     0   1   2   3   4   5   6
           ↓   ↓
flowers:  [f] [l] [o] [w] [e] [r] [s]
flow:     [f] [l] [o] [w]
fly:      [f] [l] [y]
flight:   [f] [l] [i] [g] [h] [t]
           ✓   ✓   ✗

Result: "fl"
```

### 💻 Code Implementation (Approach 1)

```python
def solve(n, arr):
    # Get lengths of all strings
    lengths = [len(arr[i]) for i in range(n)]
    
    index = 0
    ans = ""
    
    while True:
        char = None
        flag = False
        
        # Check character at current index in all strings
        for i in range(n):
            # Check if index is valid and character matches
            if index < lengths[i] and (char is None or char == arr[i][index]):
                char = arr[i][index]
            else:
                # Mismatch or end of string reached
                flag = True
                break
        else:
            # All strings have matching character at this index
            ans += char
            index += 1
        
        # Stop if mismatch found
        if flag:
            break
    
    return ans
```

### 🎯 Approach 2: Sorting Optimization

1. Sort the array lexicographically
2. Compare only first and last strings
3. Common prefix of these two is the answer

**Why this works:** After sorting, the first and last strings are most different lexicographically. If they share a prefix, all middle strings must share it too.

### 📝 Visual Representation (Approach 2)

```
Before sort: ["flowers", "flow", "fly", "flight"]
After sort:  ["flight", "flow", "flowers", "fly"]
              ↑                              ↑
            first                          last

Compare first and last:
flight: f l i g h t
fly:    f l y
        ✓ ✓ ✗

Result: "fl"
```

### 💻 Code Implementation (Approach 2)

```python
def solve(n, arr):
    # Sort the array lexicographically
    arr.sort()
    
    # Get first and last strings after sorting
    first = arr[0]
    last = arr[n - 1]
    
    ans = ""
    
    # Compare characters of first and last strings
    for i in range(min(len(first), len(last))):
        if first[i] == last[i]:
            ans += first[i]
        else:
            break  # Stop at first mismatch
    
    return ans
```

### ⏱️ Complexity Analysis

**Approach 1 (Vertical Scanning):**
- **Time Complexity:** O(n × m)
  - n = number of strings, m = length of shortest string
  
- **Space Complexity:** O(m)
  - For storing the result and lengths array

**Approach 2 (Sorting):**
- **Time Complexity:** O(n × m × log n)
  - Sorting takes O(n log n), each comparison is O(m)
  
- **Space Complexity:** O(m)
  - For storing the result

**Note:** Approach 1 is better when strings are already somewhat similar. Approach 2 is elegant and easier to understand.

---

## 5. Isomorphic String

### Problem Statement
Determine if two strings `s` and `t` are isomorphic. Two strings are isomorphic if characters in `s` can be replaced to get `t`, with each character mapping to exactly one character (bijection).

### Examples

**Example 1:**
```
Input:  s = "egg", t = "add"
Output: true
Explanation: 
  e → a
  g → d
  Mapping is consistent
```

**Example 2:**
```
Input:  s = "apple", t = "bbnbm"
Output: false
Explanation:
  p must map to both b and n (inconsistent)
```

### 💡 Intuition
- Need **one-to-one mapping** between characters
- Track last seen index of each character in both strings
- If mapping patterns differ, strings aren't isomorphic

### 🎯 Approach

**Index Mapping Technique:**
1. Use two hash maps: `map1` for `s`, `map2` for `t`
2. For each index `i`:
   - Check if last seen positions match: `map1[s[i]] == map2[t[i]]`
   - Update both maps with current index
3. If positions ever mismatch, return `false`

### 📝 Visual Representation

```
Example 1: s = "egg", t = "add"

Index:  0   1   2
s:     [e] [g] [g]
t:     [a] [d] [d]

i=0: map1[e]=0, map2[a]=0 ✓ (both None initially)
i=1: map1[g]=1, map2[d]=1 ✓ (both None initially)
i=2: map1[g]=1, map2[d]=1 ✓ (both have same last index)

Result: true

Example 2: s = "apple", t = "bbnbm"

Index:  0   1   2   3   4
s:     [a] [p] [p] [l] [e]
t:     [b] [b] [n] [b] [m]

i=2: map1[p]=1, map2[n]=None ✗ (mismatch!)

Result: false
```

### 💻 Code Implementation

```python
def solve(s, t):
    n = len(s)
    map1, map2 = {}, {}
    
    # Iterate through both strings simultaneously
    for i in range(n):
        # Check if last occurrence indices match
        # get() returns None if key doesn't exist
        if map1.get(s[i]) != map2.get(t[i]):
            return False
        
        # Update last seen index for both characters
        map1[s[i]] = i
        map2[t[i]] = i
    
    return True
```

### ⏱️ Complexity Analysis

- **Time Complexity:** O(n)
  - Single pass through both strings
  - Hash map operations are O(1)
  
- **Space Complexity:** O(k)
  - k = number of unique characters (max 26 for lowercase, 52 for both cases)
  - Two hash maps storing character mappings

---

## 6. Rotate String

### Problem Statement
Return `true` if string `s` can become `goal` after some number of left rotations. A rotation moves the leftmost character to the rightmost position.

### Examples

**Example 1:**
```
Input:  s = "abcde", goal = "cdeab"
Output: true
Explanation:
  Rotation 1: "bcdea"
  Rotation 2: "cdeab" ✓
```

**Example 2:**
```
Input:  s = "abcde", goal = "adeac"
Output: false
Explanation: No amount of rotations work
```

### 💡 Intuition
- **Brute Force:** Simulate all n rotations
- **Optimal Trick:** If `goal` is a rotation of `s`, then `goal` must be a substring of `s+s`

**Why s+s works:**
```
s = "abcde"
s+s = "abcdeabcde"

All possible rotations:
"abcde" ← in s+s at position 0
"bcdea" ← in s+s at position 1
"cdeab" ← in s+s at position 2
"deabc" ← in s+s at position 3
"eabcd" ← in s+s at position 4
```

### 🎯 Approach 1: Brute Force Simulation

1. Perform rotation n times
2. After each rotation, check if matches goal
3. Return true if match found

### 💻 Code Implementation (Approach 1)

```python
def solve(s1, s2):
    n1, n2 = len(s1), len(s2)
    
    # Different lengths can't be rotations
    if n1 != n2:
        return False
    
    arr = list(s1)
    target = list(s2)
    
    # Try all possible rotations
    for _ in range(n1):
        # Perform left rotation: move first element to end
        temp = arr[0]
        for i in range(1, n1):
            arr[i - 1] = arr[i]
        arr[n1 - 1] = temp
        
        # Check if current rotation matches target
        if arr == target:
            return True
    
    return False
```

### 🎯 Approach 2: Concatenation Trick (Optimal)

1. Check if lengths are equal
2. Concatenate `s` with itself: `s + s`
3. Check if `goal` is substring of concatenated string

### 📝 Visual Representation (Approach 2)

```
s = "abcde"
goal = "cdeab"

s + s = "abcde" + "abcde" = "abcdeabcde"
                              ^^^^^ 
                            "cdeab" found!

Result: true
```

### 💻 Code Implementation (Approach 2)

```python
def solve(s1, s2):
    n1, n2 = len(s1), len(s2)
    
    # Different lengths can't be rotations
    if n1 != n2:
        return False
    
    # Concatenate s1 with itself
    s = s1 + s1
    
    # Check if s2 is substring of concatenated string
    return s2 in s
```

### ⏱️ Complexity Analysis

**Approach 1 (Brute Force):**
- **Time Complexity:** O(n²)
  - n rotations, each rotation takes O(n)
  
- **Space Complexity:** O(n)
  - Array copy of the string

**Approach 2 (Optimal):**
- **Time Complexity:** O(n)
  - String concatenation: O(n)
  - Substring search: O(n) using efficient algorithms
  
- **Space Complexity:** O(n)
  - Concatenated string of length 2n

---

## 7. Valid Anagram

### Problem Statement
Return `true` if string `t` is an anagram of string `s`. An anagram uses all original letters exactly once, rearranged.

### Examples

**Example 1:**
```
Input:  s = "anagram", t = "nagaram"
Output: true
Explanation: Same characters, different order
```

**Example 2:**
```
Input:  s = "dog", t = "cat"
Output: false
Explanation: Different characters
```

### 💡 Intuition
- Anagrams have **same character frequencies**
- Two approaches:
  1. **Sort and compare** - simple and clean
  2. **Frequency counting** - more efficient

### 🎯 Approach 1: Sorting

1. Check if lengths are equal
2. Sort both strings
3. Compare sorted strings

### 📝 Visual Representation (Approach 1)

```
s = "anagram"
t = "nagaram"

After sorting:
s_sorted = "aaagmnr"
t_sorted = "aaagmnr"

s_sorted == t_sorted → true
```

### 💻 Code Implementation (Approach 1)

```python
def solve(s, t):
    # Different lengths can't be anagrams
    if len(s) != len(t):
        return False
    
    # Sort both strings and compare
    return sorted(s) == sorted(t)
```

### 🎯 Approach 2: Frequency Array

1. Check if lengths are equal
2. Create frequency array (26 for lowercase letters)
3. Increment for `s`, decrement for `t`
4. Check if all frequencies are zero

### 📝 Visual Representation (Approach 2)

```
s = "anagram"
t = "nagaram"

Frequency tracking:
      a  b  c  d  e  f  g  h  i  j  k  l  m  n  ...
s:   +3              +1                 +1 +1
t:   -3              -1                 -1 -1

Final: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...]

All zeros → true
```

### 💻 Code Implementation (Approach 2)

```python
def solve(s, t):
    # Different lengths can't be anagrams
    if len(s) != len(t):
        return False
    
    n = len(s)
    freq = [0] * 256  # ASCII characters
    
    # Increment for s, decrement for t
    for i in range(n):
        freq[ord(s[i]) - ord('a')] += 1
        freq[ord(t[i]) - ord('a')] -= 1
    
    # Check if all frequencies are zero
    for i in range(256):
        if freq[i] != 0:
            return False
    
    return True
```

### ⏱️ Complexity Analysis

**Approach 1 (Sorting):**
- **Time Complexity:** O(n log n)
  - Sorting dominates
  
- **Space Complexity:** O(1) or O(n)
  - Depends on sorting algorithm (Python's Timsort uses O(n))

**Approach 2 (Frequency Array):**
- **Time Complexity:** O(n)
  - Single pass through both strings
  
- **Space Complexity:** O(1)
  - Fixed size array (256 or 26)

---

## 8. Sort Characters by Frequency

### Problem Statement
Return an array of unique characters from string `s`, sorted by frequency (highest to lowest). If frequencies are equal, sort alphabetically.

### Examples

**Example 1:**
```
Input:  s = "tree"
Output: ['e', 'r', 't']
Explanation:
  e → 2 occurrences
  r → 1 occurrence
  t → 1 occurrence
  (r and t have same frequency, so alphabetical order)
```

**Example 2:**
```
Input:  s = "raaaajj"
Output: ['a', 'j', 'r']
Explanation:
  a → 4 occurrences
  j → 2 occurrences
  r → 1 occurrence
```

### 💡 Intuition
- Count frequency of each character
- Sort by frequency (descending), then alphabetically (ascending)
- Two approaches with different sorting strategies

### 🎯 Approach 1: Hash Map + Custom Sort

1. Build frequency map using dictionary
2. Sort by custom key: (-frequency, character)
3. Extract characters from sorted list

### 📝 Visual Representation (Approach 1)

```
s = "tree"

Step 1: Build frequency map
{
  't': 1,
  'r': 1,
  'e': 2
}

Step 2: Sort by (-frequency, character)
[('e', 2), ('r', 1), ('t', 1)]
   ↓         ↓         ↓
  freq=2   freq=1    freq=1
          (alphabetically: r < t)

Step 3: Extract characters
['e', 'r', 't']
```

### 💻 Code Implementation (Approach 1)

```python
def solve(s):
    n = len(s)
    freq = {}
    
    # Build frequency map
    for char in s:
        if char in freq:
            freq[char] += 1
        else:
            freq[char] = 1
    
    # Sort by frequency (descending) then alphabetically (ascending)
    # -x[1] for descending frequency, x[0] for ascending alphabetical
    temp = sorted(freq.items(), key=lambda x: (-x[1], x[0]))
    
    # Extract just the characters
    return [char for char, cnt in temp]
```

### 🎯 Approach 2: Sort String + Remove Duplicates

1. Sort string in reverse order
2. Iterate and keep only first occurrence of each character

**Note:** This approach works but is less intuitive and doesn't perfectly match the problem requirements when frequencies are equal.

### 💻 Code Implementation (Approach 2)

```python
def solve(s):
    # Convert to list and sort in reverse
    s = list(s)
    s.sort(reverse=True)
    
    # Start with first character
    ans = s[0]
    n = len(s)
    
    # Add unique characters
    for i in range(1, n):
        if s[i] != s[i - 1]:
            ans += s[i]
    
    return ans
```

### ⏱️ Complexity Analysis

**Approach 1 (Hash Map + Custom Sort):**
- **Time Complexity:** O(n + k log k)
  - n for building frequency map
  - k log k for sorting unique characters (k ≤ n)
  
- **Space Complexity:** O(k)
  - Hash map stores k unique characters

**Approach 2 (Sort + Dedup):**
- **Time Complexity:** O(n log n)
  - Sorting the entire string
  
- **Space Complexity:** O(n)
  - Sorted list copy

**Recommendation:** Use Approach 1 for correctness and efficiency.

---

## 📊 Summary Table

| Problem | Best Time | Best Space | Key Technique |
|---------|-----------|------------|---------------|
| Reverse String | O(n) | O(1) | Two Pointers |
| Palindrome Check | O(n) | O(1) | Two Pointers |
| Largest Odd Number | O(n) | O(1) | Greedy |
| Longest Common Prefix | O(n×m) | O(m) | Vertical Scan / Sorting |
| Isomorphic String | O(n) | O(k) | Hash Maps |
| Rotate String | O(n) | O(n) | String Concatenation |
| Valid Anagram | O(n) | O(1) | Frequency Array |
| Sort by Frequency | O(n+k log k) | O(k) | Hash Map + Sort |

---

## 🎓 Key Takeaways

### Common Patterns

1. **Two Pointers**: Efficient for in-place operations and comparisons
2. **Hash Maps**: Track character frequencies and mappings
3. **Greedy Approach**: Make locally optimal choices
4. **String Tricks**: Concatenation (s+s) for rotation problems
5. **Sorting**: Simplifies comparison and ordering problems

### Optimization Tips

- Use two pointers for O(1) space when possible
- Hash maps provide O(1) lookup but use O(k) space
- Consider sorting when it simplifies the logic
- String concatenation can reveal hidden patterns
- Frequency arrays are faster than hash maps for fixed character sets

### Interview Strategy

1. **Clarify constraints**: String length? Character set? Case sensitivity?
2. **Start simple**: Brute force first, then optimize
3. **Think out loud**: Explain your reasoning
4. **Test edge cases**: Empty strings, single characters, all same characters
5. **Optimize gradually**: From O(n²) → O(n log n) → O(n)

---
