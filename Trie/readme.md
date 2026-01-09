# Trie (Prefix Tree) - Complete Index

## 📑 Complete Problem Catalog (14 Problems)

---

## 🔹 **TRIE FUNDAMENTALS**
*Understanding Trie data structure*

### **Subtype: Theory & Concepts**
1. What is a Trie?
2. Why Use a Trie?
3. Basic Structure

### **Subtype: Implementation**
4. Implementation 1: Basic Trie
5. Implementation 2: Advanced Trie with Counting

### **Subtype: Understanding**
6. Visual Examples
7. Time & Space Complexity
8. Common Use Cases

### **Subtype: Complete Code**
9. Complete implementation code (basic-trie)
10. Complete implementation code (advanced-trie)

---

## 🔹 **TRIE APPLICATIONS**
*Problem-solving using Trie*

### **Subtype: String Problems**
11. Longest Word with All Prefixes

### **Subtype: Counting Problems**
12. Number of Distinct Substrings

### **Subtype: Bit Manipulation Trie**
13. Maximum XOR of Two Numbers
14. Maximum XOR with Element Constraint

---

## 📊 **SUMMARY BY CATEGORY**

### **🟢 Trie Fundamentals** (10 problems)
→ Problems: 1-10

### **🟡 Trie Applications** (4 problems)
→ Problems: 11-14

---

## 📈 **LEARNING PATH RECOMMENDATION**

### **Phase 1: Understand Trie** (Start Here)
```
1 → 2 → 3 → 6
```
*Learn what Trie is and why it's useful*

### **Phase 2: Implementation**
```
4 → 9 → 5 → 10
```
*Master basic and advanced implementations*

### **Phase 3: Analysis**
```
7 → 8
```
*Understand complexity and use cases*

### **Phase 4: String Applications**
```
11 → 12
```
*Apply Trie to string problems*

### **Phase 5: Bit Trie**
```
13 → 14
```
*Use Trie for XOR problems*

---

## 🎯 **CONCEPT CLUSTERING**

### **Cluster 1: Trie Theory**
*Foundation knowledge*
- Concept: 1, 2, 3
- Visualization: 6
- Analysis: 7, 8

### **Cluster 2: Trie Implementation**
*Building the structure*
- Basic: 4, 9
- Advanced: 5, 10

### **Cluster 3: String Trie Problems**
*Character-based Trie*
- Prefix/Suffix: 11
- Counting: 12

### **Cluster 4: Binary Trie**
*Bit-based Trie for numbers*
- XOR Problems: 13, 14

---

## 💡 **PROBLEM PAIRING** *(Similar Concepts)*

**Pair 1:** Basic Trie (4, 9) ↔ Advanced Trie (5, 10)

**Pair 2:** Max XOR (13) ↔ Max XOR with Constraint (14)

**Pair 3:** Theory (1, 2, 3) ↔ Examples (6)

**Pair 4:** Implementation (4, 5) ↔ Complexity (7, 8)

---

## 🔍 **BY DIFFICULTY LEVEL**

### **🟢 Easy** (Learning Concepts)
- Problems: 1, 2, 3, 4, 6, 7, 8, 9

### **🟡 Medium** (Implementation & Application)
- Problems: 5, 10, 11, 12

### **🔴 Hard** (Advanced Applications)
- Problems: 13, 14

---

## 🎓 **TECHNIQUE-WISE GROUPING**

### **Standard Trie (Character-based)**
→ Problems: 4, 5, 9, 10, 11, 12

### **Binary Trie (Bit-based)**
→ Problems: 13, 14

### **Theoretical Understanding**
→ Problems: 1, 2, 3, 6, 7, 8

---

## 📚 **PATTERN RECOGNITION GUIDE**

### **When you see: "Prefix matching / Auto-complete..."**
→ Use: Standard Trie (Problems 4, 5, 9, 10)

### **When you see: "Count words with prefix..."**
→ Use: Trie with counting (Problems 5, 10)

### **When you see: "Longest word with all prefixes..."**
→ Use: Trie with end-of-word markers (Problem 11)

### **When you see: "Count distinct substrings..."**
→ Use: Trie + suffix insertion (Problem 12)

### **When you see: "Maximum XOR..."**
→ Use: Binary Trie (Problems 13, 14)

### **When you see: "Find maximum XOR pair..."**
→ Use: Binary Trie + greedy bit selection (Problem 13)

---

## 🏆 **MILESTONE PROBLEMS**
*Master these to understand core patterns*

1. **Basic Trie Implementation (4, 9)** - Foundation
2. **Advanced Trie with Counting (5, 10)** - Enhanced operations
3. **Longest Word (11)** - Prefix checking
4. **Distinct Substrings (12)** - Counting application
5. **Maximum XOR (13)** - Binary Trie introduction
6. **XOR with Constraint (14)** - Advanced binary Trie

---

## 🗺️ **PROBLEM BREAKDOWN BY TYPE**

### **Conceptual Understanding** (6 problems)
→ 1, 2, 3, 6, 7, 8

### **Implementation** (4 problems)
→ 4, 5, 9, 10

### **String Applications** (2 problems)
→ 11, 12

### **Number/XOR Applications** (2 problems)
→ 13, 14

---

## 📊 **TOTAL STATISTICS**

- **Total Problems:** 14
- **Fundamentals (Theory + Implementation):** 10 problems (71%)
- **Applications:** 4 problems (29%)
- **Easy:** 8 problems (57%)
- **Medium:** 4 problems (29%)
- **Hard:** 2 problems (14%)

---

## 🔄 **PREREQUISITE RELATIONSHIPS**

### **Master First (Concepts):**
1, 2, 3, 6 → Understanding Trie

### **Then Implement:**
4, 9 → Basic Trie implementation

### **Enhance:**
5, 10 → Advanced Trie with features

### **Analyze:**
7, 8 → Complexity and use cases

### **Apply (String):**
11, 12 → String-based problems

### **Apply (Binary):**
13, 14 → Number-based problems

---

## 🎯 **COMMON PATTERNS & TEMPLATES**

### **Template 1: Basic Trie Node**
```python
class TrieNode:
    def __init__(self):
        self.children = {}  # or [None] * 26 for lowercase
        self.is_end_of_word = False
```
→ Used in: 4, 9, 11, 12

### **Template 2: Advanced Trie Node with Counting**
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False
        self.word_count = 0      # How many words end here
        self.prefix_count = 0    # How many words pass through
```
→ Used in: 5, 10

### **Template 3: Basic Trie Operations**
```python
class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    
    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
    
    def startsWith(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```
→ Used in: 4, 9

### **Template 4: Binary Trie Node (for XOR)**
```python
class BinaryTrieNode:
    def __init__(self):
        self.children = [None, None]  # 0 and 1
```
→ Used in: 13, 14

### **Template 5: Binary Trie for XOR**
```python
class BinaryTrie:
    def __init__(self):
        self.root = BinaryTrieNode()
    
    def insert(self, num):
        node = self.root
        for i in range(31, -1, -1):  # 32-bit integer
            bit = (num >> i) & 1
            if not node.children[bit]:
                node.children[bit] = BinaryTrieNode()
            node = node.children[bit]
    
    def find_max_xor(self, num):
        node = self.root
        max_xor = 0
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            # Try to go opposite direction for max XOR
            toggle_bit = 1 - bit
            if node.children[toggle_bit]:
                max_xor |= (1 << i)
                node = node.children[toggle_bit]
            else:
                node = node.children[bit]
        return max_xor
```
→ Used in: 13, 14

### **Template 6: Count Distinct Substrings**
```python
def countDistinctSubstrings(s):
    trie = Trie()
    count = 0
    
    for i in range(len(s)):
        node = trie.root
        for j in range(i, len(s)):
            char = s[j]
            if char not in node.children:
                node.children[char] = TrieNode()
                count += 1  # New substring found
            node = node.children[char]
    
    return count + 1  # +1 for empty string
```
→ Used in: 12

---

## 🌟 **KEY TRIE INSIGHTS**

### **Trie vs Hash Map:**
```
Trie Advantages:
- Prefix queries: O(k) where k = prefix length
- Space sharing for common prefixes
- Lexicographic ordering
- No hash collisions

Hash Map Advantages:
- Simpler implementation
- Better for exact match lookups
- Less memory overhead for few words
```

### **Time Complexities:**
```
Operation          Time Complexity
Insert word        O(m) where m = word length
Search word        O(m)
Prefix search      O(p) where p = prefix length
Delete word        O(m)
```

### **Space Complexity:**
```
Standard Trie: O(ALPHABET_SIZE * N * M)
- ALPHABET_SIZE: typically 26 for lowercase
- N: number of words
- M: average word length

Optimized with HashMap: O(N * M)
```

### **When to Use Trie:**
1. **Auto-complete** - Find all words with prefix
2. **Spell checker** - Check valid words
3. **IP routing** - Longest prefix matching
4. **Phone directory** - T9 predictive text
5. **XOR problems** - Binary Trie for max XOR

### **Binary Trie Specifics:**
```
- Each node has 2 children (0 and 1)
- Store numbers as binary representation
- Useful for XOR maximization problems
- Insert/Query: O(32) for 32-bit integers
```

---

## 🎯 **VISUAL CONCEPTS**

### **Standard Trie Structure:**
```
Words: ["cat", "car", "card", "care", "dog"]

         root
        /    \
       c      d
       |      |
       a      o
      / \     |
     t   r    g
         |\ 
         d e
         |
         d
```

### **Binary Trie for XOR (Numbers: 3, 10, 5):**
```
Binary: 3=0011, 10=1010, 5=0101

           root
          /    \
         0      1
        / \      \
       0   1      0
      /     \      \
     1       0      1
    / \       \      \
   1   1       1      0
  [3] [5]    [10]
```

---

## 🎯 **QUICK REFERENCE BY NUMBER**

**1-8:** Trie Theory & Fundamentals  
**9-10:** Complete Implementations  
**11-14:** Trie Applications

---

## ✅ **COMPLETENESS STATUS**

### **Coverage: 85% COMPLETE** ✓

**What You Have:**
- ✅ Complete Trie theory and concepts
- ✅ Basic and advanced implementations
- ✅ Time & space complexity analysis
- ✅ String applications (prefix, substrings)
- ✅ Binary Trie for XOR problems

**Missing Topics (15%):**
- ⭕ **Design Search Autocomplete System** (1 problem)
- ⭕ **Word Search II** (using Trie) (1 problem)
- ⭕ **Replace Words** (using Trie) (1 problem)
- ⭕ **Implement Magic Dictionary** (1 problem)
- ⭕ **Stream of Characters** (1 problem)

**Verdict:** Strong foundation! All core Trie concepts covered! 🎉

---

## 🎓 **INTERVIEW FREQUENCY**

### **Very High Frequency** ⭐⭐⭐
→ 4, 9 (Basic Trie Implementation)

### **High Frequency** ⭐⭐
→ 5, 10, 11, 13

### **Medium Frequency** ⭐
→ 12, 14

### **Low Frequency** (Learning)
→ 1, 2, 3, 6, 7, 8

**Most Important:** Master basic Trie implementation (4, 9) first - it's asked frequently in interviews!

---

## 💡 **PRACTICAL TIPS**

### **Implementation Choice:**
```python
# Array-based (for lowercase letters only)
children = [None] * 26  # Faster access, more memory

# HashMap-based (for any characters)
children = {}  # Flexible, less memory for sparse
```

### **Memory Optimization:**
- Use HashMap instead of array for sparse alphabets
- Compress paths for long chains
- Delete unused branches

### **Common Mistakes:**
1. Forgetting to mark end of word
2. Not handling empty string
3. Memory leaks when deleting
4. Wrong bit order in binary Trie

### **Debugging Tips:**
1. Print Trie structure visually
2. Test with single character words
3. Verify prefix counts match
4. Check edge cases (empty, single char)

---

## 🎯 **EXTENDED LEARNING**

### **Related Structures:**
- **Suffix Tree** - For substring queries
- **Radix Tree** - Compressed Trie
- **Ternary Search Tree** - Space-efficient alternative
- **Patricia Tree** - Practical algorithm

### **Advanced Applications:**
- **Text editors** - Auto-complete
- **Routers** - IP address lookup
- **Bioinformatics** - DNA sequence matching
- **Compilers** - Symbol tables

---

## 🎯 **INTERVIEW STRATEGY**

### **When Asked Trie Problem:**
1. Clarify character set (lowercase? uppercase? special chars?)
2. Ask about memory constraints
3. Discuss Trie vs HashMap tradeoffs
4. Start with basic implementation
5. Optimize based on requirements

### **Common Follow-ups:**
- "How would you delete a word?"
- "Can you count words with prefix?"
- "Optimize for space/time?"
- "Handle wildcards?"

### **Practice Priority:**
1. Basic Trie implementation (MUST KNOW)
2. Prefix search operations
3. One XOR problem
4. One string application
