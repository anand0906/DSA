# Stack & Cache Implementation - Revision Notes

## 📑 Table of Contents
1. [Stack using Arrays](#1-stack-using-arrays)
2. [Stack using Linked List](#2-stack-using-linked-list)
3. [Min Stack](#3-min-stack)
4. [LRU Cache](#4-lru-cache)
5. [LFU Cache](#5-lfu-cache)
6. [Comparison Table](#comparison-table)

---

## 1. Stack using Arrays

### Problem Description
Implement a Last-In-First-Out (LIFO) stack using an array with operations: `push()`, `pop()`, `top()`, and `isEmpty()`.

### Sample Test Cases

**Example 1:**
```
Input: ["ArrayStack", "push", "push", "top", "pop", "isEmpty"]
       [[], [5], [10], [], [], []]
Output: [null, null, null, 10, 10, false]
```

**Example 2:**
```
Input: ["ArrayStack", "isEmpty", "push", "pop", "isEmpty"]
       [[], [], [1], [], []]
Output: [null, true, null, 1, true]
```

---

### Approach 1: Dynamic Array (Python List)

#### Intuition
1. Use Python's built-in list which automatically handles resizing
2. Maintain pointer to track the top element
3. Push: Append element to the end of list
4. Pop: Remove and return the last element
5. Peek: Access the last element without removing

#### Visual Representation
```
Push(1) → Push(2) → Push(3) → Pop() → Peek()

[1]       [1,2]      [1,2,3]    [1,2]     2
 ↑         ↑           ↑          ↑
top       top         top        top
```

#### Code
```python
class Stack:
    def __init__(self):
        # Initialize empty list to store stack elements
        self.stack = []

    def is_empty(self):
        # Check if stack has no elements
        return len(self.stack) == 0

    def push(self, item):
        # Add element to the end (top) of stack
        self.stack.append(item)

    def pop(self):
        # Check for underflow condition
        if self.is_empty():
            raise IndexError("StackUnderFlow : Stack is Empty")
        # Remove and return top element
        return self.stack.pop()

    def peek(self):
        # Check for underflow condition
        if self.is_empty():
            raise IndexError("StackUnderFlow : Stack is Empty")
        # Return top element without removing
        return self.stack[-1]

    def size(self):
        # Return current number of elements
        return len(self.stack)

    def __str__(self):
        # String representation for printing
        return str(self.stack)
```

**Time Complexity:** 
- Push: O(1) - Appending to list is constant time
- Pop: O(1) - Removing last element is constant time
- Peek: O(1) - Accessing last element is constant time
- isEmpty: O(1) - Length check is constant time

**Space Complexity:** O(n) - where n is the number of elements in stack

---

### Approach 2: Fixed-Size Array

#### Intuition
1. Pre-allocate array of fixed size
2. Use `top_index` to track the position of top element (-1 for empty stack)
3. Push: Increment top_index and place element
4. Pop: Return element at top_index and decrement
5. Prevents dynamic resizing but has capacity limit

#### Visual Representation
```
Initial State:          After Push(5):         After Push(10):
[0, 0, 0, 0, 0]        [5, 0, 0, 0, 0]        [5, 10, 0, 0, 0]
 ↑                      ↑                       ↑
top_index = -1         top_index = 0           top_index = 1
(empty)                                        
```

#### Code
```python
class ArrayStack:
    def __init__(self, size=100):
        # Pre-allocate array with fixed size
        self.arr = [0] * size
        # Top index starts at -1 (empty stack indicator)
        self.top_index = -1
        self.size = size

    def push(self, x):
        # Check for overflow condition
        if self.top_index >= self.size - 1:
            return
        # Increment top_index and insert element
        self.top_index += 1
        self.arr[self.top_index] = x

    def pop(self):
        # Check for underflow condition
        if self.isEmpty():
            return
        # Get element at top
        element = self.arr[self.top_index]
        # Decrement top_index (logical removal)
        self.top_index -= 1
        return element

    def top(self):
        # Return element at top_index
        return self.arr[self.top_index]

    def isEmpty(self):
        # Stack is empty when top_index is -1
        return self.top_index == -1
```

**Time Complexity:**
- Push: O(1) - Direct array access
- Pop: O(1) - Direct array access
- Top: O(1) - Direct array access
- isEmpty: O(1) - Simple comparison

**Space Complexity:** O(n) - where n is the fixed capacity

---

## 2. Stack using Linked List

### Problem Description
Implement a LIFO stack using a singly linked list with operations: `push()`, `pop()`, `top()`, and `isEmpty()`.

### Sample Test Cases

**Example 1:**
```
Input: ["LinkedListStack", "push", "push", "pop", "top", "isEmpty"]
       [[], [3], [7], [], [], []]
Output: [null, null, null, 7, 3, false]
```

**Example 2:**
```
Input: ["LinkedListStack", "isEmpty"]
       [[]]
Output: [null, true]
```

---

### Approach: Linked List with Head Pointer

#### Intuition
1. Use singly linked list where head represents top of stack
2. Each node contains data and pointer to next node
3. Push: Create new node and make it new head
4. Pop: Remove head and return its data
5. No size limitation, dynamic memory allocation

#### Visual Representation
```
Initial: head → None

After Push(10):
head → [10|None]

After Push(20):
head → [20|→] → [10|None]

After Push(30):
head → [30|→] → [20|→] → [10|None]

After Pop():
head → [20|→] → [10|None]  (returned: 30)
```

#### Code
```python
class Node:
    def __init__(self, data):
        # Store data value
        self.data = data
        # Pointer to next node
        self.next = None

class Stack:
    def __init__(self):
        # Head points to top of stack
        self.head = None
        # Track number of elements
        self.count = 0

    def isEmpty(self):
        # Stack is empty if head is None
        return self.head == None

    def push(self, data):
        if self.head == None:
            # First element becomes head
            self.head = Node(data)
            self.count += 1
        else:
            # Create new node
            node = Node(data)
            # Point new node to current head
            node.next = self.head
            # Update head to new node
            self.head = node
            self.count += 1

    def pop(self):
        # Check for underflow
        if self.isEmpty():
            raise IndexError("StackUnderFlow : Stack is Empty")
        else:
            # Store current head
            node = self.head
            # Move head to next node
            self.head = node.next
            # Disconnect popped node
            node.next = None
            self.count -= 1
            return node.data

    def peek(self):
        # Check for underflow
        if self.isEmpty():
            raise IndexError("StackUnderFlow : Stack is Empty")
        else:
            # Return head data without removing
            return self.head.data

    def size(self):
        # Return count of elements
        return self.count

    def display(self):
        # Check if stack is empty
        if self.isEmpty():
            raise IndexError("StackUnderFlow : Stack is Empty")
        else:
            temp = self.head
            # Traverse and print all nodes
            while temp:
                print(temp.data, end="<-")
                temp = temp.next
            print()
```

**Time Complexity:**
- Push: O(1) - Insert at head
- Pop: O(1) - Remove from head
- Peek: O(1) - Access head
- isEmpty: O(1) - Check if head is None

**Space Complexity:** O(n) - n nodes for n elements

---

## 3. Min Stack

### Problem Description
Design a stack supporting push, pop, top, and retrieving the minimum element in O(1) time.

### Sample Test Cases

**Example 1:**
```
Input: ["MinStack", "push", "push", "push", "getMin", "pop", "top", "getMin"]
       [[], [-2], [0], [-3], [], [], [], []]
Output: [null, null, null, null, -3, null, 0, -2]
```

**Example 2:**
```
Input: ["MinStack", "push", "push", "getMin", "push", "pop", "getMin", "top"]
       [[], [5], [1], [], [3], [], [], []]
Output: [null, null, null, 1, null, null, 1, 1]
```

---

### Approach 1: Pair Stack (Data + Min)

#### Intuition
1. Store each element as a tuple: (value, current_minimum)
2. When pushing, calculate minimum between new value and previous minimum
3. Each element knows the minimum at that point in the stack
4. Pop simply removes the top tuple
5. GetMin returns the minimum from the top tuple

#### Visual Representation
```
Push(-2):  [(-2, -2)]
           value: -2, min: -2

Push(0):   [(-2, -2), (0, -2)]
           value: 0, min: -2

Push(-3):  [(-2, -2), (0, -2), (-3, -3)]
           value: -3, min: -3

Pop():     [(-2, -2), (0, -2)]
           Removed: -3, Current min: -2
```

#### Code
```python
class MinStack:
    def __init__(self):
        # Stack stores tuples of (value, minimum_at_this_point)
        self.stack = []

    def isEmpty(self):
        return len(self.stack) == 0

    def top(self):
        if self.isEmpty():
            raise IndexError("StackUnderFlow: Stack Is Empty")
        # Return the value (first element of tuple)
        return self.stack[-1][0]

    def getMin(self):
        if self.isEmpty():
            raise IndexError("StackUnderFlow: Stack Is Empty")
        # Return the minimum (second element of tuple)
        return self.stack[-1][1]

    def push(self, data):
        if self.isEmpty():
            # First element: minimum is itself
            mini = data
        else:
            # Calculate minimum between current data and previous minimum
            mini = min(data, self.getMin())
        # Store tuple of (value, current_minimum)
        self.stack.append((data, mini))

    def pop(self):
        if self.isEmpty():
            raise IndexError("StackUnderFlow: Stack Is Empty")
        # Remove and return the value
        return self.stack.pop()[0]

    def size(self):
        return len(self.stack)
```

**Time Complexity:**
- Push: O(1)
- Pop: O(1)
- Top: O(1)
- GetMin: O(1)

**Space Complexity:** O(2n) = O(n) - storing two values per element

---

### Approach 2: Space-Optimized with Formula

#### Intuition
1. Store only one minimum variable instead of storing min with each element
2. When pushing value less than current min:
   - Store a modified value: `2*value - min`
   - Update minimum to the new value
3. When popping modified value (value < min):
   - Actual value was the current min
   - Recover previous min using: `2*min - modified_value`
4. Uses mathematical trick to encode previous minimum

#### Visual Representation
```
Push(-2):  stack = [-2], min = -2

Push(0):   stack = [-2, 0], min = -2
           (0 >= -2, so push as is)

Push(-3):  stack = [-2, 0, -8], min = -3
           (-3 < -2, so push 2*(-3) - (-2) = -8)
           Update min = -3

Pop():     stack = [-2, 0], returned = -3
           (-8 < -3, so actual value was -3)
           Recover previous min: 2*(-3) - (-8) = 2
```

#### Code
```python
class MinStack:
    def __init__(self):
        self.stack = []
        # Single variable to track minimum
        self.min = None

    def isEmpty(self):
        return len(self.stack) == 0

    def top(self):
        if self.isEmpty():
            raise IndexError("StackUnderFlow: Stack Is Empty")
        # If top is modified value, return actual min
        if self.stack[-1] < self.min:
            return self.min
        return self.stack[-1]

    def getMin(self):
        if self.isEmpty():
            raise IndexError("StackUnderFlow: Stack Is Empty")
        return self.min

    def push(self, data):
        if self.isEmpty():
            # First element
            self.min = data
            self.stack.append(data)
        else:
            if data >= self.min:
                # Normal push
                self.stack.append(data)
            else:
                # Push modified value
                temp = 2 * data - self.min
                self.stack.append(temp)
                # Update minimum
                self.min = data

    def pop(self):
        if self.isEmpty():
            raise IndexError("StackUnderFlow: Stack Is Empty")
        y = self.stack.pop()
        if y > self.min:
            # Normal value, return as is
            return y
        else:
            # Modified value, recover previous min
            temp = 2 * self.min - y
            actual_value = self.min
            self.min = temp
            return actual_value

    def size(self):
        return len(self.stack)
```

**Time Complexity:**
- Push: O(1)
- Pop: O(1)
- Top: O(1)
- GetMin: O(1)

**Space Complexity:** O(n) - only storing one value per element

---

## 4. LRU Cache

### Problem Description
Design a Least Recently Used (LRU) cache with O(1) get and put operations. When capacity is reached, evict the least recently used item.

### Sample Test Cases

**Example 1:**
```
Input: ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
       [[2], [1,1], [2,2], [1], [3,3], [2], [4,4], [1], [3], [4]]
Output: [null, null, null, 1, null, -1, null, -1, 3, 4]
```

**Example 2:**
```
Input: ["LRUCache", "put", "put", "get", "put", "get", "put", "get"]
       [[1], [1,1], [2,2], [1], [3,3], [2], [4,4], [3]]
Output: [null, null, null, -1, null, -1, null, -1]
```

---

### Approach: Doubly Linked List + HashMap

#### Intuition
1. **HashMap**: For O(1) key lookup (key → node mapping)
2. **Doubly Linked List**: For O(1) deletion and insertion
   - Most recently used → near head
   - Least recently used → near tail
3. **Get operation**: Move accessed node to head (mark as recently used)
4. **Put operation**:
   - If key exists: update value and move to head
   - If new key and at capacity: delete tail node (LRU), insert at head
   - If new key and under capacity: insert at head

#### Visual Representation
```
Initial: capacity = 2
head ↔ tail

Put(1,1):
head ↔ [1:1] ↔ tail

Put(2,2):
head ↔ [2:2] ↔ [1:1] ↔ tail

Get(1): (move 1 to front)
head ↔ [1:1] ↔ [2:2] ↔ tail

Put(3,3): (capacity full, remove 2)
head ↔ [3:3] ↔ [1:1] ↔ tail
```

#### Code
```python
class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.next = None
        self.prev = None

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        # Dummy head and tail for easier operations
        self.head = Node(0, 0)
        self.tail = Node(0, 0)
        # HashMap to store key -> node mapping
        self.map = {}
        # Connect head and tail
        self.head.next = self.tail
        self.tail.prev = self.head

    def put(self, key, val):
        if key in self.map:
            # Key exists, delete old node
            self.delete(self.map[key])
        if len(self.map) == self.capacity:
            # At capacity, delete LRU (node before tail)
            self.delete(self.tail.prev)
        # Insert new node at head (most recently used)
        self.insertBegin(Node(key, val))

    def get(self, key):
        if key not in self.map:
            return -1
        # Get value and mark as recently used
        val = self.map[key].val
        # Delete from current position
        self.delete(self.map[key])
        # Insert at head (most recently used)
        self.insertBegin(Node(key, val))
        return self.map[key].val

    def insertBegin(self, node):
        # Add to hashmap
        self.map[node.key] = node
        # Insert after head
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node

    def delete(self, node):
        # Remove from hashmap
        del self.map[node.key]
        # Remove from linked list
        node.prev.next = node.next
        node.next.prev = node.prev
        node.next = None
        node.prev = None
```

**Time Complexity:**
- Get: O(1) - HashMap lookup + list operations
- Put: O(1) - HashMap operations + list operations

**Space Complexity:** O(capacity) - storing capacity number of nodes

---

## 5. LFU Cache

### Problem Description
Design a Least Frequently Used (LFU) cache with O(1) get and put operations. Evict the least frequently used item; in case of tie, evict the least recently used.

### Sample Test Cases

**Example 1:**
```
Input: ["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
       [[2], [1,1], [2,2], [1], [3,3], [2], [3], [4,4], [1], [3], [4]]
Output: [null, null, null, 1, null, -1, 3, null, -1, 3, 4]
```

---

### Approach: Multiple LinkedLists + Two HashMaps

#### Intuition
1. **keyMap**: Maps key → node (for O(1) access)
2. **freqMap**: Maps frequency → LinkedList (for O(1) frequency list access)
3. **minFreq**: Tracks minimum frequency for eviction
4. Each frequency has its own doubly linked list (LRU order)
5. **Get/Put**: Increment frequency, move node to next frequency list
6. **Eviction**: Remove tail of minimum frequency list

#### Visual Representation
```
Initial: capacity = 2

Put(1,1):
freq=1: [1:1]
minFreq = 1

Put(2,2):
freq=1: [2:2] ↔ [1:1]
minFreq = 1

Get(1): (increment freq of 1)
freq=1: [2:2]
freq=2: [1:1]
minFreq = 1

Put(3,3): (evict 2, minFreq=1)
freq=1: [3:3]
freq=2: [1:1]
minFreq = 1
```

#### Code
```python
class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        # Frequency count starts at 1
        self.count = 1
        self.next = None
        self.prev = None

class LinkedList:
    def __init__(self):
        # Dummy head and tail
        self.head = Node(0, 0)
        self.tail = Node(0, 0)
        self.head.next = self.tail
        self.tail.prev = self.head
        # Track size of list
        self.size = 0

    def InsertBegin(self, node):
        # Insert after head (most recently used)
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node
        self.size += 1

    def deleteAtEnd(self, node):
        # Remove node from list
        node.prev.next = node.next
        node.next.prev = node.prev
        self.size -= 1

class LFUCache:
    def __init__(self, capacity):
        self.size = capacity
        # Map: key -> node
        self.keyMap = {}
        # Map: frequency -> LinkedList
        self.freqMap = {}
        self.currentSize = 0
        # Track minimum frequency
        self.minFreq = 0

    def updateFreqencyList(self, node):
        # Remove from keyMap
        self.keyMap.pop(node.key)
        # Remove from current frequency list
        self.freqMap[node.count].deleteAtEnd(node)
        
        # If removed from minFreq list and it's empty, increment minFreq
        if node.count == self.minFreq and self.freqMap[node.count].size == 0:
            self.minFreq += 1
        
        # Increment frequency
        node.count += 1
        
        # Get or create next frequency list
        if node.count in self.freqMap:
            nextHigherList = self.freqMap[node.count]
        else:
            nextHigherList = LinkedList()
        
        # Add to new frequency list
        nextHigherList.InsertBegin(node)
        self.freqMap[node.count] = nextHigherList
        self.keyMap[node.key] = node

    def get(self, key):
        if key not in self.keyMap:
            return -1
        # Get node and update frequency
        node = self.keyMap[key]
        self.updateFreqencyList(node)
        return node.val

    def put(self, key, val):
        if self.size == 0:
            return -1

        if key in self.keyMap:
            # Key exists, update value and frequency
            node = self.keyMap[key]
            node.val = val
            self.updateFreqencyList(node)
        else:
            # New key
            if self.currentSize == self.size:
                # At capacity, evict LFU (tail of minFreq list)
                minList = self.freqMap[self.minFreq]
                self.keyMap.pop(minList.tail.prev.key)
                minList.deleteAtEnd(minList.tail.prev)
                self.currentSize -= 1
            
            # Add new node
            self.currentSize += 1
            self.minFreq = 1
            
            # Get or create frequency 1 list
            if self.minFreq in self.freqMap:
                currentList = self.freqMap[self.minFreq]
            else:
                currentList = LinkedList()
            
            node = Node(key, val)
            currentList.InsertBegin(node)
            self.keyMap[key] = node
            self.freqMap[self.minFreq] = currentList
```

**Time Complexity:**
- Get: O(1) - HashMap lookups + list operations
- Put: O(1) - HashMap operations + list operations

**Space Complexity:** O(capacity) - nodes + frequency lists

---

## Comparison Table

| Problem | Data Structure | Key Operations | Time Complexity | Space Complexity | Key Feature |
|---------|---------------|----------------|-----------------|------------------|-------------|
| **Stack (Array)** | Fixed/Dynamic Array | push, pop, peek | O(1) all ops | O(n) | Simple LIFO, fixed/dynamic size |
| **Stack (LinkedList)** | Singly Linked List | push, pop, peek | O(1) all ops | O(n) | Dynamic size, no capacity limit |
| **Min Stack (Pair)** | Array of Tuples | push, pop, getMin | O(1) all ops | O(2n) | Store min with each element |
| **Min Stack (Formula)** | Array + Min Variable | push, pop, getMin | O(1) all ops | O(n) | Space-optimized using math |
| **LRU Cache** | HashMap + DLL | get, put | O(1) both | O(capacity) | Evict least recently used |
| **LFU Cache** | 2 HashMaps + Multiple DLLs | get, put | O(1) both | O(capacity) | Evict least frequently used |

### Quick Decision Guide

**When to use:**
- **Array Stack**: Fixed capacity, simple operations
- **LinkedList Stack**: Dynamic size needed, memory flexible
- **Min Stack (Pair)**: Simple to implement, extra space acceptable
- **Min Stack (Formula)**: Space-critical applications
- **LRU Cache**: Recent access matters (browser history, page cache)
- **LFU Cache**: Frequency matters (content popularity, CDN cache)

### Memory vs Complexity Trade-offs

| Implementation | Memory Overhead | Implementation Complexity | Use Case |
|---------------|-----------------|--------------------------|----------|
| Array Stack | Low (contiguous) | Very Low | General purpose |
| LinkedList Stack | Medium (pointers) | Low | Unknown size |
| Min Stack (Pair) | 2x elements | Very Low | Simple min tracking |
| Min Stack (Formula) | 1x elements | Medium | Memory constrained |
| LRU Cache | Medium (1 DLL + HashMap) | Medium | Time-based eviction |
| LFU Cache | High (Multiple DLLs + 2 HashMaps) | High | Frequency-based eviction |

---

**📝 Revision Tips:**
1. Draw diagrams for linked list operations
2. Practice edge cases (empty, single element, capacity)
3. Remember: DLL for O(1) deletion, HashMap for O(1) lookup
4. LRU = time-based, LFU = frequency-based eviction
5. Min Stack formula: `modified_value = 2*new_value - old_min`
