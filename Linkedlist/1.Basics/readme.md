# Comprehensive Linked List Guide

## Table of Contents
1. [Introduction to Linked Lists](#1-introduction-to-linked-lists)
2. [Singly Linked List](#2-singly-linked-list)
   - 2.1 [Node Structure](#21-node-structure)
   - 2.2 [Basic Operations](#22-basic-operations)
   - 2.3 [Insertion Operations](#23-insertion-operations)
   - 2.4 [Deletion Operations](#24-deletion-operations)
3. [Doubly Linked List](#3-doubly-linked-list)
   - 3.1 [Node Structure](#31-node-structure)
   - 3.2 [Basic Operations](#32-basic-operations)
   - 3.3 [Insertion Operations](#33-insertion-operations)
   - 3.4 [Deletion Operations](#34-deletion-operations)
4. [Operations Summary](#4-operations-summary)

---

## 1. Introduction to Linked Lists

### What is a Linked List?
A **linked list** is a linear data structure that stores a sequence of elements called **nodes**. Each node contains:
- **Data field**: Stores the actual value
- **Link/Address field**: Stores the address of the next node

### Key Characteristics
- Elements are **not stored in contiguous memory locations**
- It's an **ordered set** of data items
- **Dynamic size** (unlike arrays with fixed size)
- Efficient **insertion and deletion** operations

### Advantages Over Arrays
- No fixed size limitation
- Efficient insertion/deletion (no shifting required)
- Dynamic memory allocation

### Real-World Applications
- **Dynamic Memory Allocation**: Implementing stacks, queues, hash tables
- **Memory Management**: Memory allocation and garbage collection algorithms
- **Text Editors**: Buffer implementation where each node represents a line
- **Navigation Systems**: Storing sequences of directions/waypoints
- **Web Browsers**: Page traversal history

### Types of Linked Lists
1. **Singly Linked List**: Each node points to the next node only
2. **Doubly Linked List**: Each node points to both next and previous nodes
3. **Circular Linked List**: Last node points back to the first node

---

## 2. Singly Linked List

### Overview
A singly linked list is a linear data structure where each node points only to its **successor**. The last node points to `None`, indicating the end of the list.

### 2.1 Node Structure

```python
class Node:
    def __init__(self, data, next=None):
        self.data = data  # Stores the value
        self.next = next  # Reference to the next node
```

**Components:**
- `data`: The value stored in the node
- `next`: Pointer to the next node in sequence
- **Head Pointer**: Reference to the first node (entry point)

**Advantages:**
- Efficient insertion/deletion at beginning or middle
- Simple and memory-efficient structure

**Disadvantages:**
- No random access (must traverse to reach elements)
- Extra memory for storing next pointers

---

### 2.2 Basic Operations

#### 2.2.1 Array to Linked List Conversion

**Problem**: Convert an array into a singly linked list.

**Example:**
```
Input: [4, 2, 5, 1]
Output: 4 -> 2 -> 5 -> 1
```

**Algorithm:**
1. Create head node from first array element
2. Store head in current variable for tracking
3. Loop through remaining elements:
   - Create node for each element
   - Link current node to new node
   - Update current to new node
4. Return head

```python
class Node:
    def __init__(self, data, next=None):
        self.data = data
        self.next = next

def convert(arr):
    n = len(arr)
    head = Node(arr[0])  # Create head from first element
    current = head       # Track current position
    
    # Process remaining elements
    for i in range(1, n):
        temp = Node(arr[i])     # Create new node
        current.next = temp      # Link to current node
        current = temp           # Move to new node
    
    return head

def printLinkedList(head):
    while head:
        print(head.data, end="->")
        head = head.next

# Example usage
arr = list(map(int, input().split()))
head = convert(arr)
printLinkedList(head)
```

**Complexity:**
- Time: O(N)
- Space: O(N)

---

#### 2.2.2 Print Linked List

**Problem**: Print all elements of a linked list.

**Algorithm:**
1. Store head in temp variable
2. Loop until temp is None:
   - Print current node data
   - Move to next node

```python
def printLinkedList(head):
    temp = head
    while temp:
        print(temp.data, end="->")
        temp = temp.next  # Move to next node

# Example usage
head = Node(10)
head.next = Node(20)
head.next.next = Node(30)
printLinkedList(head)  # Output: 10->20->30->
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 2.2.3 Length of Linked List

**Problem**: Find the total number of nodes in a linked list.

**Algorithm:**
1. Initialize count to 0
2. Traverse the list and increment count for each node
3. Return count

```python
def length(head):
    count = 0
    temp = head
    
    while temp:
        count += 1          # Increment counter
        temp = temp.next    # Move to next node
    
    return count

# Example usage
head = Node(10)
head.next = Node(20)
head.next.next = Node(30)
print(length(head))  # Output: 3
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 2.2.4 Search Element in Linked List

**Problem**: Check if a given element exists in the linked list.

**Algorithm:**
1. Traverse the list
2. Compare each node's data with search value
3. Return True if found, False otherwise

```python
def search(head, searchData):
    temp = head
    
    while temp:
        if temp.data == searchData:  # Element found
            return True
        temp = temp.next             # Move to next node
    
    return False  # Element not found

# Example usage
head = Node(10)
head.next = Node(20)
head.next.next = Node(30)

print(search(head, 10))   # Output: True
print(search(head, 100))  # Output: False
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

### 2.3 Insertion Operations

#### 2.3.1 Insert at Head

**Problem**: Insert a new node at the beginning of the list.

**Algorithm:**
1. Create new node with given data
2. Point new node's next to current head
3. Return new node as head

```python
def insertAtHead(head, data):
    temp = Node(data)  # Create new node
    temp.next = head   # Link to existing list
    return temp        # New node becomes head
```

**Complexity:**
- Time: O(1)
- Space: O(1)

---

#### 2.3.2 Insert at Tail

**Problem**: Insert a new node at the end of the list.

**Algorithm:**
1. Handle empty list case
2. Traverse to the last node
3. Link last node to new node

```python
def insertAtTail(head, data):
    # Handle empty list
    if head == None:
        return Node(data)
    
    temp = head
    # Traverse to last node
    while temp.next:
        temp = temp.next
    
    temp.next = Node(data)  # Link to new node
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 2.3.3 Insert at Position

**Problem**: Insert a new node at a specific position (1-indexed).

**Algorithm:**
1. Handle position 1 (insert at head)
2. Use counter to find target position
3. Insert node at position - 1, linking properly

```python
def insertAtPosition(head, data, pos):
    # Handle empty list
    if head == None:
        if pos == 1:
            return Node(data)
        return head
    
    # Insert at head if position is 1
    if pos == 1:
        temp = Node(data)
        temp.next = head
        return temp
    
    count = 0
    temp = head
    
    while temp:
        count += 1
        # Insert at position - 1
        if count == pos - 1:
            new = Node(data, temp.next)  # Create node with next link
            temp.next = new               # Link previous node
            break
        temp = temp.next
    
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 2.3.4 Insert Before Value

**Problem**: Insert a new node before the first occurrence of a specific value.

**Algorithm:**
1. Check if value is at head
2. Traverse until next node's data matches value
3. Insert new node before that position

```python
def insertAtValue(head, data, value):
    if head == None:
        return None
    
    # Check if value is at head
    if head.data == value:
        temp = Node(data)
        temp.next = head
        return temp
    
    temp = head
    while temp:
        # Check if next node contains the value
        if temp.next != None and temp.next.data == value:
            new = Node(data, temp.next)  # Create and link new node
            temp.next = new
            break
        temp = temp.next
    
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

### 2.4 Deletion Operations

#### 2.4.1 Delete Head

**Problem**: Remove the first node of the linked list.

**Algorithm:**
1. Handle empty list
2. Move head to next node
3. Return new head

```python
def deleteHead(head):
    if head == None:
        return None
    
    temp = head
    head = head.next  # Move head to next node
    return head
```

**Complexity:**
- Time: O(1)
- Space: O(1)

---

#### 2.4.2 Delete Tail

**Problem**: Remove the last node of the linked list.

**Algorithm:**
1. Handle empty or single-node list
2. Traverse to second-last node
3. Set its next to None

```python
def deleteTail(head):
    # Handle empty or single node
    if head == None or head.next == None:
        return None
    
    temp = head
    # Traverse to second-last node
    while temp.next.next:
        temp = temp.next
    
    temp.next = None  # Remove last node
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 2.4.3 Delete by Position

**Problem**: Delete node at a specific position (1-indexed).

**Algorithm:**
1. Handle position 1 (delete head)
2. Use counter to find target position
3. Link previous node to next node, skipping target

```python
def deleteByPosition(head, pos):
    if head == None:
        return None
    
    # Delete head if position is 1
    if pos == 1:
        temp = head
        head = head.next
        return head
    
    previous = None
    temp = head
    count = 0
    
    while temp:
        count += 1
        if count == pos:
            # Skip the current node
            previous.next = temp.next
            return head
        previous = temp
        temp = temp.next
    
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 2.4.4 Delete by Value

**Problem**: Delete the first node containing a specific value.

**Algorithm:**
1. Check if value is at head
2. Traverse to find matching node
3. Link previous node to next node

```python
def deleteByValue(head, value):
    if head == None:
        return None
    
    # Check if head contains the value
    if head.data == value:
        temp = head
        head = head.next
        return head
    
    previous = None
    temp = head
    
    while temp:
        if temp.data == value:
            # Skip the current node
            previous.next = temp.next
            return head
        previous = temp
        temp = temp.next
    
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

## 3. Doubly Linked List

### Overview
A doubly linked list allows **bidirectional traversal**. Each node contains references to both the **next** and **previous** nodes.

### Advantages
- **Bidirectional Traversal**: Move forward and backward through the list
- **Efficient Insertion/Deletion**: Direct access to previous node
- **Previous Node Access**: Enables operations requiring both neighbors
- **Undo Functionality**: Easy backward traversal for undo operations
- **Data Structure Implementation**: Essential for queues and deques

### 3.1 Node Structure

```python
class Node:
    def __init__(self, data, prev=None, next=None):
        self.data = data  # Stores the value
        self.prev = prev  # Reference to previous node
        self.next = next  # Reference to next node
```

**Components:**
- `data`: The value stored in the node
- `prev`: Pointer to the previous node
- `next`: Pointer to the next node

---

### 3.2 Basic Operations

#### 3.2.1 Array to Doubly Linked List Conversion

**Problem**: Convert an array into a doubly linked list.

**Example:**
```
Input: [4, 2, 5, 1]
Output: 4 <-> 2 <-> 5 <-> 1
```

**Algorithm:**
1. Create head node from first element
2. Store head in previous variable
3. Loop through remaining elements:
   - Create new node
   - Link new node's prev to previous
   - Link previous node's next to new node
   - Update previous to new node
4. Return head

```python
class Node:
    def __init__(self, data, prev=None, next=None):
        self.data = data
        self.prev = prev
        self.next = next

def convert(array):
    n = len(array)
    head = Node(array[0])  # Create head from first element
    previous = head         # Track previous node
    
    # Process remaining elements
    for i in range(1, n):
        temp = Node(array[i])   # Create new node
        temp.prev = previous     # Link to previous node
        previous.next = temp     # Link previous to new node
        previous = temp          # Update previous
    
    return head

def printLinkedList(head):
    temp = head
    while temp:
        print(temp.data, end="<->")
        temp = temp.next

# Example usage
array = list(map(int, input().split()))
head = convert(array)
printLinkedList(head)
```

**Complexity:**
- Time: O(N)
- Space: O(N)

---

### 3.3 Insertion Operations

#### 3.3.1 Insert Before Head

**Problem**: Insert a new node at the beginning.

**Algorithm:**
1. Handle empty list
2. Create new node
3. Link new node to current head
4. Update head's prev to new node
5. Return new node as head

```python
def insertBeforeHead(head, data):
    if head == None:
        return Node(data)
    
    new = Node(data)    # Create new node
    new.next = head     # Link to current head
    head.prev = new     # Update head's prev
    return new          # New node becomes head
```

**Complexity:**
- Time: O(1)
- Space: O(1)

---

#### 3.3.2 Insert Before Tail

**Problem**: Insert a new node before the last node.

**Algorithm:**
1. Handle empty list
2. Handle single-node list (insert before head)
3. Traverse to tail
4. Insert between previous and tail

```python
def insertBeforeTail(head, data):
    if head == None:
        return Node(data)
    
    # Single node case
    if head.next == None:
        head = insertBeforeHead(head, data)
        return head
    
    # Traverse to tail
    temp = head
    while temp.next:
        temp = temp.next
    
    tail = temp
    previous = tail.prev
    new = Node(data)
    
    # Link new node
    new.next = tail
    new.prev = previous
    tail.prev = new
    previous.next = new
    
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 3.3.3 Insert Before Kth Node

**Problem**: Insert a new node before the Kth position (1-indexed).

**Algorithm:**
1. Handle empty list
2. Use counter to find Kth node
3. Insert between previous and Kth node

```python
def insertBeforeKthElement(head, data, k):
    if head == None:
        return None
    
    cnt = 0
    temp = head
    
    # Find Kth node
    while temp:
        cnt += 1
        if cnt == k:
            break
        temp = temp.next
    else:
        return None  # Kth position doesn't exist
    
    previous = temp.prev
    new = Node(data)
    
    # Link new node
    new.next = temp
    new.prev = previous
    previous.next = new
    temp.prev = new
    
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

### 3.4 Deletion Operations

#### 3.4.1 Delete Head

**Problem**: Remove the first node from the doubly linked list.

**Example:**
```
Input: 5 <-> 8 <-> 3 <-> 7 <-> 9
Output: 8 <-> 3 <-> 7 <-> 9
```

**Algorithm:**
1. Handle empty or single-node list
2. Move head to next node
3. Clear links from old head
4. Set new head's prev to None

```python
def deleteHead(head):
    # Handle empty or single node
    if head == None or head.next == None:
        return None
    
    temp = head
    head = head.next     # Move head to next node
    head.prev = None     # Clear new head's prev
    temp.next = None     # Clear old head's next
    
    return head
```

**Complexity:**
- Time: O(1)
- Space: O(1)

---

#### 3.4.2 Delete Tail

**Problem**: Remove the last node from the doubly linked list.

**Example:**
```
Input: 5 <-> 8 <-> 3 <-> 7 <-> 9
Output: 5 <-> 8 <-> 3 <-> 7
```

**Algorithm:**
1. Handle empty or single-node list
2. Traverse to tail
3. Get previous node
4. Clear links

```python
def deleteTail(head):
    # Handle empty or single node
    if head == None or head.next == None:
        return None
    
    tail = head
    # Traverse to tail
    while tail.next != None:
        tail = tail.next
    
    previous = tail.prev
    previous.next = None  # Clear previous node's next
    tail.prev = None      # Clear tail's prev
    
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 3.4.3 Delete by Position

**Problem**: Delete the node at the Kth position.

**Example:**
```
Input: k = 3, list = [1, 2, 5, 4]
Output: [1, 2, 4]
```

**Algorithm:**
1. Find node at Kth position using counter
2. Check if node is head, tail, or middle
3. Handle each case appropriately
4. Relink previous and next nodes

```python
def deleteByPosition(head, k):
    if head == None:
        return head
    
    cnt = 0
    temp = head
    
    # Find Kth node
    while temp:
        cnt += 1
        if cnt == k:
            break
        temp = temp.next
    
    prevNode = temp.prev
    nextNode = temp.next
    
    # Case 1: Only one node
    if prevNode == None and nextNode == None:
        return None
    
    # Case 2: Head node
    if prevNode == None:
        head = deleteHead(head)
        return head
    
    # Case 3: Tail node
    if nextNode == None:
        head = deleteTail(head)
        return head
    
    # Case 4: Middle node
    prevNode.next = nextNode
    nextNode.prev = prevNode
    temp.next = None
    temp.prev = None
    
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 3.4.4 Delete by Value

**Problem**: Delete the first node containing a specific value.

**Algorithm:**
1. Find node with matching value
2. Check if node is head, tail, or middle
3. Handle each case appropriately
4. Relink surrounding nodes

```python
def deleteByValue(head, data):
    if head == None:
        return head
    
    temp = head
    # Find node with matching value
    while temp:
        if temp.data == data:
            break
        temp = temp.next
    
    prevNode = temp.prev
    nextNode = temp.next
    
    # Case 1: Only one node
    if prevNode == None and nextNode == None:
        return None
    
    # Case 2: Head node
    if prevNode == None:
        head = deleteHead(head)
        return head
    
    # Case 3: Tail node
    if nextNode == None:
        head = deleteTail(head)
        return head
    
    # Case 4: Middle node
    prevNode.next = nextNode
    nextNode.prev = prevNode
    temp.next = None
    temp.prev = None
    
    return head
```

**Complexity:**
- Time: O(N)
- Space: O(1)

---

#### 3.4.5 Delete Given Node

**Problem**: Delete a specific node when the node reference is provided (guaranteed not to be head).

**Example:**
```
List: 1 <-> 2 <-> 3 <-> 4
Node to delete: 3
Output: 1 <-> 2 <-> 4
```

**Algorithm:**
1. Get previous and next nodes
2. Check if node is head, tail, or middle
3. Relink surrounding nodes

```python
def deleteNode(head, node):
    if head == None:
        return head
    
    prevNode = node.prev
    nextNode = node.next
    
    # Case 1: Only one node
    if prevNode == None and nextNode == None:
        return None
    
    # Case 2: Head node
    if prevNode == None:
        head = deleteHead(head)
        return head
    
    # Case 3: Tail node
    if nextNode == None:
        head = deleteTail(head)
        return head
    
    # Case 4: Middle node
    prevNode.next = nextNode
    nextNode.prev = prevNode
    node.next = None
    node.prev = None
    
    return head
```

**Complexity:**
- Time: O(1) when node reference is given
- Space: O(1)

---

## 4. Operations Summary

### Singly Linked List Operations

| Operation | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Insert at Head | O(1) | O(1) |
| Insert at Tail | O(N) | O(1) |
| Insert at Position | O(N) | O(1) |
| Insert Before Value | O(N) | O(1) |
| Delete Head | O(1) | O(1) |
| Delete Tail | O(N) | O(1) |
| Delete by Position | O(N) | O(1) |
| Delete by Value | O(N) | O(1) |
| Search Element | O(N) | O(1) |
| Print List | O(N) | O(1) |
| Find Length | O(N) | O(1) |

### Doubly Linked List Operations

| Operation | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Insert Before Head | O(1) | O(1) |
| Insert Before Tail | O(N) | O(1) |
| Insert Before Kth Node | O(N) | O(1) |
| Delete Head | O(1) | O(1) |
| Delete Tail | O(N) | O(1) |
| Delete by Position | O(N) | O(1) |
| Delete by Value | O(N) | O(1) |
| Delete Given Node | O(1) | O(1) |

### Key Takeaways

**Singly Linked List:**
- Simple structure with forward-only traversal
- Efficient head operations (O(1))
- Tail operations require full traversal (O(N))
- Less memory overhead (one pointer per node)

**Doubly Linked List:**
- Bidirectional traversal capability
- More efficient deletion when node reference is known
- Requires more memory (two pointers per node)
- Better for operations requiring backward movement

---

**Note:** All space complexities refer to auxiliary space used by the algorithm, not including the space for the linked list itself.
