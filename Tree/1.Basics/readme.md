# 🌳 Tree Data Structure

## 📖 Definition

The **Tree** is a non-linear data structure that stores and organizes data in a **HIERARCHICAL** way. It can store a collection of data items called **nodes**, which are connected with each other by using **edges** to form a hierarchy.

---

## 🌍 Real Life Examples

### 1. **Family Tree**
A family tree with many generations such as grandparents, parents, children, etc. Family trees are organized hierarchically.

![Family Tree](https://miro.medium.com/max/1400/1*9xW9uddLqt7gujPYBpRp8g.png)

### 2. **File System**
The File System in a computer is arranged in the form of hierarchy.

![File System](https://andysbrainbook.readthedocs.io/en/stable/_images/UnixTree.png)

### 3. **HTML DOM (Document Object Model)**
In HTML, the document object model is represented as a tree. The HTML tag contains other tags like head and body, which contain specific elements.

![HTML DOM](https://miro.medium.com/max/1118/1*P7lguZAgq5blMOqJyPHopQ.png)

### 4. **Organization Structure**
An organization's structure is another example of hierarchy where employees are arranged based on their roles.

![Organization Structure](https://cdn-media-1.freecodecamp.org/images/1*GsBCmW5E1GuJ3MpH3Zz0Ew.jpeg)

---

## 💡 Applications

1. **File System** - Folders and subfolders organization
2. **E-Commerce Websites** - Category → SubCategory → Products
3. **Databases** - B and B+ trees used for indexing and fast searching
4. **Auto-Suggestions** - Trie (special kind of tree) used for dynamic spell checking and word suggestions
5. **Heap Data Structure** - Used in priority queues
6. **Syntax Analysis** - Used in compilers
7. **Routing Algorithms** - Network routing and pathfinding

---

## 📚 Tree Terminology

| Term | Description |
|------|-------------|
| **Node** | A data item in the tree containing a value or data |
| **Root Node** | The topmost node with no incoming edges; only one root per tree |
| **Edge** | Connection between two nodes |
| **Parent Node** | A node connected to other nodes below it |
| **Child Node** | A node that is an immediate successor of another node |
| **Ancestor** | Any predecessor node from root to a given node |
| **Descendant** | Any successor node from a given node to leaf nodes |
| **Subtree** | A node and its corresponding descendants |
| **Degree** | Total count of child nodes attached to a node |
| **Leaf Node / External Node** | A node with no children |
| **Siblings** | Nodes having the same parent |
| **Internal Node** | A node with at least one child |
| **Path** | An ordered list of nodes connected by edges |
| **Depth** | Length of path (number of edges) from root to a node; root depth = 0 |
| **Height** | Length of longest path from a node to a leaf; leaf height = 0 |
| **Level** | The depth of a node in the tree |

---

## 🎯 Types of Tree Data Structures

1. General Tree
2. Binary Tree
3. Binary Search Tree
4. AVL Tree
5. Red-Black Tree

---

## 🔷 General Tree

A tree where a node can have **0 to n** number of children. There is no restriction on the degree of nodes.

![General Tree](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTsmGJw2gxctsfkR5Djfa3iwwaFu0aQ-u9UVA&s)

**Key Features:**
- No restriction on number of children
- Root node at the top
- Children form subtrees

---

## 🔸 Binary Tree

A tree in which **each parent contains at most 2 children** (left and right child).

![Binary Tree](https://cdn-media-1.freecodecamp.org/images/1*ofbwuz4inpf2OlB-l9gtHw.jpeg)

### ⚡ Properties of Binary Tree

- Each node contains: data, left child pointer, right child pointer
- **Maximum nodes at level x**: `2^x`
  - Level 0 (root): 2^0 = 1 node
  - Level 1: 2^1 = 2 nodes
- **Maximum nodes in tree of height h**: `2^(h+1) - 1`
  - Height 0: 2^1 - 1 = 1 node
  - Height 1: 2^2 - 1 = 3 nodes
- **Height of tree with n nodes**: `⌊log₂(n+1)⌋ - 1`
- **Minimum levels for L leaves**: `⌊log₂L⌋ + 1`
- In a binary tree where every node has 0 or 2 children, **leaf nodes = internal nodes + 1**

---

## 🎨 Types of Binary Trees

![Types of Binary Trees](https://www.theknowledgeacademy.com/_files/images/Types_of_Binary_Trees.png)

### 1️⃣ **Full Binary Tree** (Strict/Proper Binary Tree)
A tree where all nodes have either **0 or 2 children** (no node has exactly 1 child).

### 2️⃣ **Complete Binary Tree**
All levels are completely filled except possibly the last level, which is filled from **left to right**.

### 3️⃣ **Perfect Binary Tree**
All internal nodes have **two children** and all leaf nodes are at the **same level**.

### 4️⃣ **Degenerate Binary Tree** (Skewed Tree)
All internal nodes have **only one child**.
- **Right-skewed**: All nodes have only right children
- **Left-skewed**: All nodes have only left children

### 5️⃣ **Balanced Binary Tree**
A tree where the height is **O(log n)** where n is the number of nodes. The height difference between left and right subtrees is minimal.

---

## 🚀 Quick Reference

```
Tree Structure:
       Root
      /    \
   Child  Child
   /  \
Leaf  Leaf

Time Complexities (Average):
- Search: O(log n)
- Insert: O(log n)
- Delete: O(log n)
```

---

## 📌 Summary

Trees are fundamental hierarchical data structures used extensively in computer science. They provide efficient organization and retrieval of data, making them essential for file systems, databases, compilers, and many other applications.

**Key Takeaway**: Understanding trees is crucial for building efficient algorithms and systems that require hierarchical data organization.

---

*Happy Learning! 🌟*
