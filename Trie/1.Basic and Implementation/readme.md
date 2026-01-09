# Complete Guide to Trie Data Structure

## Table of Contents
1. [What is a Trie?](#what-is-a-trie)
2. [Why Use a Trie?](#why-use-a-trie)
3. [Basic Structure](#basic-structure)
4. [Implementation 1: Basic Trie](#implementation-1-basic-trie)
5. [Implementation 2: Advanced Trie with Counting](#implementation-2-advanced-trie-with-counting)
6. [Visual Examples](#visual-examples)
7. [Time & Space Complexity](#time--space-complexity)
8. [Common Use Cases](#common-use-cases)
9. [Complete implementation code (basic-trie)](#complete-implementation-code---basic-trie)
10. [Complete implementation code (advanced-trie)](#complete-implementation-code---advanced-trie)

---

## What is a Trie?

A **Trie** (pronounced "try") is a tree-like data structure used to store and retrieve strings efficiently. It's also called a **prefix tree** because it's particularly good at finding all strings with a common prefix.

### Key Characteristics
- Each node represents a character
- Root node is empty
- Path from root to any node forms a string
- Children of a node share a common prefix

---

## Why Use a Trie?

### Advantages
- **Fast prefix searches**: O(m) where m is the length of the search string
- **Autocomplete**: Efficiently find all words with a given prefix
- **Spell checking**: Quick dictionary lookups
- **IP routing**: Used in routers for longest prefix matching

### Comparison with Other Data Structures

| Operation | Trie | Hash Table | Binary Search Tree |
|-----------|------|------------|-------------------|
| Insert | O(m) | O(m) | O(m log n) |
| Search | O(m) | O(m) | O(m log n) |
| Prefix Search | O(m + k) | O(n) | O(m log n + k) |

*m = length of word, n = number of words, k = number of results*

---

## Basic Structure

### Node Structure

Each node in a Trie contains:
1. **Links array**: 26 pointers for lowercase letters (a-z)
2. **Flag/Counter**: Marks if this node represents the end of a word

```
Node
├── links[26]      // Array of pointers to child nodes
├── flag          // (Basic) Boolean marking word end
└── counters      // (Advanced) Prefix and end counts
```

### Node Class Methods Explained

**Basic Trie Node Methods:**

1. **`__init__()`**: Initializes a new node with 26 empty links and flag set to False
2. **`containsKey(char)`**: Checks if a character exists as a child node (returns True/False)
3. **`getKey(char)`**: Returns the child node for a given character
4. **`put(char, node)`**: Creates a link to a child node for the given character
5. **`setEnd()`**: Marks this node as the end of a complete word
6. **`isEnd()`**: Checks if this node marks the end of a word (returns True/False)

**Advanced Trie Node Methods (Additional):**

7. **`increasePrefixCount()`**: Increments count of words passing through this node
8. **`increaseEndCount()`**: Increments count of words ending at this node
9. **`decreasePrefixCount()`**: Decrements prefix count (used in erase)
10. **`decreaseEndCount()`**: Decrements end count (used in erase)

**How Character Indexing Works:**
- Each character 'a' to 'z' maps to index 0 to 25
- Formula: `index = ord(char) - ord('a')`
- Example: 'a' → 0, 'b' → 1, 'c' → 2, ... 'z' → 25

---
---

## Implementation 1: Basic Trie

This implementation supports three core operations:

### Operations

#### 1. Insert
Adds a word to the trie by creating nodes for each character if they don't exist.

```python
def insert(self, word):
    temp = self.root
    for char in word:
        if not temp.containsKey(char):
            temp.put(char, Node())
        temp = temp.getKey(char)
    temp.setEnd()  # Mark end of word
```

**Example**: Inserting "cat"
```
Before:         After:
root            root
                 └── c
                     └── a
                         └── t [END]
```

#### 2. Search
Returns `True` if the exact word exists in the trie.

```python
def search(self, word):
    temp = self.root
    for char in word:
        if not temp.containsKey(char):
            return False
        temp = temp.getKey(char)
    return temp.isEnd()  # Must be marked as end
```

**Example**: Searching "cat"
- Traverses: root → c → a → t
- Checks if 't' node is marked as END
- Returns: `True`

#### 3. StartsWith
Returns `True` if any word in the trie starts with the given prefix.

```python
def startsWith(self, prefix):
    temp = self.root
    for char in prefix:
        if not temp.containsKey(char):
            return False
        temp = temp.getKey(char)
    return True  # Just needs to reach end of prefix
```

**Example**: Checking prefix "ca"
- Traverses: root → c → a
- Returns: `True` (because "cat" exists)

### Visual Example: Basic Trie

Let's insert: "cat", "car", "card", "dog"

```
                    root
                   /    \
                  c      d
                  |      |
                  a      o
                 / \     |
                t   r    g [END]
             [END]  |
                    d [END]
                    |
                    d [END]
```

- `search("car")` → True ✓
- `search("ca")` → False ✗ (not marked as END)
- `startsWith("ca")` → True ✓
- `startsWith("do")` → True ✓

---

## Implementation 2: Advanced Trie with Counting

This implementation adds counting capabilities to track:
- How many times each word was inserted
- How many words share a prefix

### Additional Features

#### Node Counters
```python
class Node:
    def __init__(self):
        self.links = [None] * 26
        self.prefixCount = 0  # Words passing through this node
        self.endCount = 0     # Complete words ending here
```

#### 1. Insert with Counting
```python
def insert(self, word):
    temp = self.root
    for char in word:
        if not temp.containsKey(char):
            temp.insertKey(char, Node())
        temp = temp.getKey(char)
        temp.increasePrefixCount()  # Increment at each node
    temp.increaseEndCount()  # Increment at end
```

#### 2. Count Words Equal To
Returns how many times an exact word was inserted.

```python
def countWordsEqualTo(self, word):
    temp = self.root
    for char in word:
        if not temp.containsKey(char):
            return 0
        temp = temp.getKey(char)
    return temp.endCount
```

#### 3. Count Words Starting With
Returns how many words share a prefix.

```python
def countWordsStartingWith(self, prefix):
    temp = self.root
    for char in prefix:
        if not temp.containsKey(char):
            return 0
        temp = temp.getKey(char)
    return temp.prefixCount
```

#### 4. Erase
Removes one occurrence of a word.

```python
def erase(self, word):
    temp = self.root
    for char in word:
        if not temp.containsKey(char):
            return
        temp = temp.getKey(char)
        temp.decreasePrefixCount()  # Decrement counts
    temp.decreaseEndCount()
```

### Visual Example: Advanced Trie with Counters

Insert: "app" (2x), "apple", "application"

```
                        root
                         |
                         a (prefix=4)
                         |
                         p (prefix=4)
                         |
                         p (prefix=4, end=2)  ← "app" inserted twice
                         |
                         l (prefix=2)
                         |
                         e (prefix=2, end=1)  ← "apple"
                         |
                    [continues...]
                         i (prefix=1)
                         c → a → t → i → o → n (end=1)  ← "application"
```

**Query Results**:
- `countWordsEqualTo("app")` → 2
- `countWordsEqualTo("apple")` → 1
- `countWordsStartingWith("app")` → 4
- `countWordsStartingWith("appl")` → 2

**After `erase("app")`**:
- `countWordsEqualTo("app")` → 1
- `countWordsStartingWith("app")` → 3

---

## Visual Examples

### Example 1: Building a Trie Step by Step

**Insert Sequence**: "tea", "ten", "to", "inn"

```
Step 1: Insert "tea"
    root
     |
     t
     |
     e
     |
     a [END]

Step 2: Insert "ten"
    root
     |
     t
     |
     e
    / \
   a   n
[END] [END]

Step 3: Insert "to"
    root
     |
     t
    / \
   e   o [END]
  / \
 a   n
[END] [END]

Step 4: Insert "inn"
      root
     /    \
    t      i
   / \     |
  e   o    n
 / \  [END]|
a   n      n [END]
[END][END]
```

### Example 2: Search Operations

Using the trie from Example 1:

| Operation | Path Taken | Result | Reason |
|-----------|-----------|--------|---------|
| `search("tea")` | t→e→a | True ✓ | Node 'a' has END flag |
| `search("te")` | t→e | False ✗ | Node 'e' lacks END flag |
| `startsWith("te")` | t→e | True ✓ | Path exists |
| `search("inn")` | i→n→n | True ✓ | Node 'n' has END flag |
| `search("in")` | i→n | False ✗ | First 'n' lacks END flag |

---

## Time & Space Complexity

### Time Complexity

| Operation | Complexity | Explanation |
|-----------|-----------|-------------|
| Insert | O(m) | m = length of word |
| Search | O(m) | Traverse each character |
| StartsWith | O(m) | Traverse prefix characters |
| CountWordsEqualTo | O(m) | Same as search |
| CountWordsStartingWith | O(m) | Same as startsWith |
| Erase | O(m) | Traverse and update counts |

### Space Complexity

- **Per Node**: O(26) = O(1) for lowercase English letters
- **Total Space**: O(N × M × 26) where:
  - N = number of words
  - M = average length of words
  - In practice, many nodes are shared through common prefixes

**Optimization Note**: For larger alphabets (Unicode), use a HashMap instead of a fixed-size array in each node.

---

## Common Use Cases

### 1. Autocomplete Systems
```python
# Find all words with prefix "app"
def autocomplete(trie, prefix):
    # Navigate to prefix node
    # Perform DFS to collect all words
    pass
```

### 2. Spell Checker
```python
# Check if word exists, suggest alternatives
def spell_check(trie, word):
    if trie.search(word):
        return "Correct"
    else:
        return suggest_corrections(trie, word)
```

### 3. IP Routing (Longest Prefix Match)
```python
# Find longest matching IP prefix
# Used in network routers
```

### 4. Contact/Phone Book
```python
# Search contacts by name prefix
# Efficient for "type-ahead" search
```

### 5. Word Games
```python
# Boggle, Scrabble word validation
# Check if sequence of letters forms valid words
```

---

## Key Differences Between Implementations

| Feature | Basic Trie | Advanced Trie |
|---------|-----------|---------------|
| Track word frequency | ✗ | ✓ |
| Count prefix matches | ✗ | ✓ |
| Delete operation | ✗ | ✓ |
| Memory per node | Minimal | +2 integers |
| Use case | Simple search | Frequency analysis |

---


---

## Complete Implementation Code - Basic Trie

```python
class Node:
    def __init__(self):
        self.links = [None] * 26  # Array to store 26 children (a-z)
        self.flag = False  # Marks if this node is end of a word
    
    def containsKey(self, char):
        # Check if character exists as child
        return self.links[ord(char) - ord('a')] is not None
    
    def getKey(self, char):
        # Return the child node for given character
        return self.links[ord(char) - ord('a')]
    
    def put(self, char, node):
        # Insert a child node for the character
        self.links[ord(char) - ord('a')] = node
    
    def setEnd(self):
        # Mark this node as end of word
        self.flag = True
    
    def isEnd(self):
        # Check if this node marks end of word
        return self.flag


class Trie:
    def __init__(self):
        self.root = Node()  # Initialize with empty root node

    def insert(self, word):
        # Insert a word into the trie
        temp = self.root
        for i in range(len(word)):
            if not temp.containsKey(word[i]):
                # Create new node if character doesn't exist
                new = Node()
                temp.put(word[i], new)
            # Move to child node
            temp = temp.getKey(word[i])
        # Mark last node as end of word
        temp.setEnd()

    def search(self, word):
        # Search for exact word in trie
        temp = self.root
        for i in range(len(word)):
            if not temp.containsKey(word[i]):
                return False  # Character not found
            temp = temp.getKey(word[i])
        # Check if last node is marked as end
        return temp.isEnd()

    def startsWith(self, prefix):
        # Check if any word starts with given prefix
        temp = self.root
        for i in range(len(prefix)):
            if not temp.containsKey(prefix[i]):
                return False  # Prefix path doesn't exist
            temp = temp.getKey(prefix[i])
        return True  # Successfully traversed prefix


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

---

## Complete Implementation Code - Advanced Trie

```python
class Node:
    def __init__(self):
        self.links = [None] * 26  # Array to store 26 children (a-z)
        self.prefixCount = 0  # Count of words passing through this node
        self.endCount = 0  # Count of words ending at this node
    
    def containsKey(self, char):
        # Check if character exists as child
        return self.links[ord(char) - ord('a')] is not None
    
    def getKey(self, char):
        # Return the child node for given character
        return self.links[ord(char) - ord('a')]
    
    def insertKey(self, char, node):
        # Insert a child node for the character
        self.links[ord(char) - ord('a')] = node
    
    def increasePrefixCount(self):
        # Increment prefix count
        self.prefixCount += 1
    
    def increaseEndCount(self):
        # Increment end count
        self.endCount += 1
    
    def decreasePrefixCount(self):
        # Decrement prefix count
        self.prefixCount -= 1
    
    def decreaseEndCount(self):
        # Decrement end count
        self.endCount -= 1


class Trie:
    def __init__(self):
        self.root = Node()  # Initialize with empty root node

    def insert(self, word):
        # Insert a word and update counters
        temp = self.root
        for i in range(len(word)):
            char = word[i]
            if not temp.containsKey(char):
                # Create new node if character doesn't exist
                new = Node()
                temp.insertKey(char, new)
            # Move to child node
            temp = temp.getKey(char)
            # Increment prefix count at each node
            temp.increasePrefixCount()
        # Increment end count at last node
        temp.increaseEndCount()

    def countWordsEqualTo(self, word):
        # Count occurrences of exact word
        temp = self.root
        for i in range(len(word)):
            char = word[i]
            if not temp.containsKey(char):
                return 0  # Word doesn't exist
            temp = temp.getKey(char)
        # Return end count at last node
        return temp.endCount

    def countWordsStartingWith(self, prefix):
        # Count words with given prefix
        temp = self.root
        for i in range(len(prefix)):
            char = prefix[i]
            if not temp.containsKey(char):
                return 0  # Prefix doesn't exist
            temp = temp.getKey(char)
        # Return prefix count at last node
        return temp.prefixCount

    def erase(self, word):
        # Remove one occurrence of word
        temp = self.root
        for i in range(len(word)):
            char = word[i]
            if not temp.containsKey(char):
                return  # Word doesn't exist, nothing to erase
            temp = temp.getKey(char)
            # Decrement prefix count at each node
            temp.decreasePrefixCount()
        # Decrement end count at last node
        temp.decreaseEndCount()


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.countWordsEqualTo(word)
# param_3 = obj.countWordsStartingWith(prefix)
# obj.erase(word)
```

## Practice Problems

1. Implement a function to find all words with a given prefix
2. Add support for case-insensitive search
3. Implement a function to find the longest common prefix of all words
4. Add wildcard search (e.g., "c.t" matches "cat", "cut")
5. Implement a space-optimized trie using HashMap instead of arrays

---




## Conclusion

Tries are powerful data structures for string operations, especially when dealing with:
- Prefix-based searches
- Autocomplete features
- Dictionary implementations
- Pattern matching problems

Choose the **basic implementation** for simple presence/absence checks, and the **advanced implementation** when you need to track frequencies or support deletion operations.
