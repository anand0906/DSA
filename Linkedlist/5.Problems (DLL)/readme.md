## Table of Contents
1. [Delete All Occurrences of a Key in DLL](#1-delete-all-occurrences-of-a-key-in-dll)
2. [Remove Duplicates from Sorted DLL](#2-remove-duplicates-from-sorted-dll)
3. [Design Browser History](#3-design-browser-history)

---

## 1. Delete All Occurrences of a Key in DLL

### Problem Description
Given the head of a doubly linked list and an integer target, delete all nodes with the value equal to target and return the head of the modified list.

### Examples

#### Example 1
**Input:** `head -> 1 <-> 2 <-> 3 <-> 1 <-> 4`, target = 1  
**Output:** `head -> 2 <-> 3 <-> 4`  
**Explanation:** All nodes with value 1 are removed

#### Example 2
**Input:** `head -> 2 <-> 3 <-> -1 <-> 4 <-> 2`, target = 2  
**Output:** `head -> 3 <-> -1 <-> 4`  
**Explanation:** All nodes with value 2 are removed (head changes)

### Solution

#### Intuition
Traverse the list and whenever we find a node with the target value, we need to:
- Update the `next` pointer of the previous node to skip current node
- Update the `prev` pointer of the next node to point to previous node
- Handle the special case when we're deleting the head node

Since it's a doubly linked list, we have both `prev` and `next` pointers, making deletion easier than in a singly linked list!

#### Approach Steps
1. Initialize `temp` pointer at head
2. **While temp exists:**
   - If `temp.val == target`:
     - If temp is head, update head to `temp.next`
     - Store `nextNode` and `prevNode`
     - If nextNode exists: `nextNode.prev = prevNode`
     - If prevNode exists: `prevNode.next = nextNode`
     - Move to nextNode
   - Else:
     - Move to next node normally
3. Return head

#### Code
```python
# Definition of doubly linked list:
class ListNode:
    def __init__(self, val=0, next=None, prev=None):
        self.val = val
        self.next = next
        self.prev = prev

class Solution:
    def deleteAllOccurrences(self, head, target):
        temp = head
        
        while temp:
            if temp.val == target:
                # Check if deleting head
                if temp == head:
                    head = temp.next
                
                nextNode = temp.next
                prevNode = temp.prev
                
                # Update next node's prev pointer
                if nextNode:
                    nextNode.prev = prevNode
                
                # Update previous node's next pointer
                if prevNode:
                    prevNode.next = nextNode
                
                # Move to next node
                temp = nextNode
            else:
                # Move to next normally
                temp = temp.next
        
        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single traversal through the list
- **Space Complexity:** O(1) - Only using pointers, no extra space

---

## 2. Remove Duplicates from Sorted DLL

### Problem Description
Given a sorted doubly linked list (non-decreasing order), remove all duplicate occurrences so that only distinct values remain. Return the head of the modified list.

### Examples

#### Example 1
**Input:** `head -> 1 <-> 1 <-> 3 <-> 3 <-> 4 <-> 5`  
**Output:** `head -> 1 <-> 3 <-> 4 <-> 5`  
**Explanation:** Keep only first occurrence of each value

#### Example 2
**Input:** `head -> 1 <-> 1 <-> 1 <-> 1 <-> 1 <-> 2`  
**Output:** `head -> 1 <-> 2`  
**Explanation:** Multiple duplicates of 1 removed

### Solution

#### Intuition
Since the list is sorted, all duplicates are adjacent to each other. We can:
- Keep track of the previous value seen
- If current value equals previous value → it's a duplicate, delete it
- Otherwise, update previous value and move forward

This is simpler than unsorted lists where we'd need hashing!

#### Approach Steps
1. Initialize `temp` at head
2. Initialize `prevVal = None` to track previous value
3. **While temp exists:**
   - If `temp.val == prevVal` (duplicate found):
     - Store `nextNode` and `prevNode`
     - Update `nextNode.prev = prevNode`
     - Update `prevNode.next = nextNode`
     - Move to nextNode
   - Else (new value):
     - Update `prevVal = temp.val`
     - Move to next node
4. Return head

#### Code
```python
# Definition of doubly linked list:
class ListNode:
    def __init__(self, val=0, next=None, prev=None):
        self.val = val
        self.next = next
        self.prev = prev

class Solution:
    def removeDuplicates(self, head):
        temp = head
        prevVal = None
        
        while temp:
            if temp.val == prevVal:
                # Duplicate found, remove it
                nextNode = temp.next
                prevNode = temp.prev
                
                if nextNode:
                    nextNode.prev = prevNode
                if prevNode:
                    prevNode.next = nextNode
                
                temp = nextNode
            else:
                # New value, update prevVal
                prevVal = temp.val
                temp = temp.next
        
        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single traversal through the list
- **Space Complexity:** O(1) - Only using pointers

---

## 3. Design Browser History

### Problem Description
Design a browser history system that supports visiting URLs, moving back in history, and moving forward in history. Implement the `BrowserHistory` class with these operations:

- `BrowserHistory(homepage)` - Initialize with homepage
- `visit(url)` - Visit a new URL from current page (clears forward history)
- `back(steps)` - Move back in history by steps (return current URL)
- `forward(steps)` - Move forward in history by steps (return current URL)

### Examples

#### Example 1
**Operations:**
```
BrowserHistory("leetcode.com")
visit("google.com")      # leetcode.com -> google.com
visit("facebook.com")    # google.com -> facebook.com
visit("youtube.com")     # facebook.com -> youtube.com
back(1)                  # return "facebook.com"
back(1)                  # return "google.com"
forward(1)               # return "facebook.com"
visit("linkedin.com")    # facebook.com -> linkedin.com (forward history cleared)
forward(2)               # return "linkedin.com" (can't go forward)
back(2)                  # return "google.com"
back(7)                  # return "leetcode.com" (can only go back 1 step)
```

**Output:**
```
[null, null, null, null, "facebook.com", "google.com", "facebook.com", 
 null, "linkedin.com", "google.com", "leetcode.com"]
```

### Solution

#### Intuition
A browser history works like a doubly linked list where:
- `current` pointer tracks the current page
- `prev` pointers allow going back
- `next` pointers allow going forward
- When visiting a new page, all forward history is cleared

Think of it like an undo-redo system where visiting a new page is like making a new edit (clears all redo history)!

#### Approach Steps

**Constructor:**
1. Create a Node class with `url`, `prev`, and `next`
2. Initialize with homepage as head
3. Set `current` to head

**visit(url):**
1. Create new node with url
2. Clear forward history: set `current.next = None` if exists
3. Link: `current.next = newNode`, `newNode.prev = current`
4. Update `current` to newNode

**back(steps):**
1. Move `current` backward steps times
2. Stop if `current.prev` is None
3. Return `current.url`

**forward(steps):**
1. Move `current` forward steps times
2. Stop if `current.next` is None
3. Return `current.url`

#### Code
```python
class Node:
    def __init__(self, url, prev=None, next=None):
        self.url = url
        self.prev = prev
        self.next = next

class BrowserHistory:
    def __init__(self, homepage: str):
        # Initialize with homepage
        self.current = Node(homepage)
    
    def visit(self, url: str) -> None:
        # Create new node
        new_node = Node(url)
        
        # Clear forward history
        if self.current.next:
            self.current.next = None
        
        # Link nodes
        new_node.prev = self.current
        self.current.next = new_node
        
        # Update current to new page
        self.current = new_node
    
    def back(self, steps: int) -> str:
        # Move back steps times or until can't go back
        while steps > 0 and self.current.prev:
            self.current = self.current.prev
            steps -= 1
        
        return self.current.url
    
    def forward(self, steps: int) -> str:
        # Move forward steps times or until can't go forward
        while steps > 0 and self.current.next:
            self.current = self.current.next
            steps -= 1
        
        return self.current.url


# Usage Example:
# browserHistory = BrowserHistory("leetcode.com")
# browserHistory.visit("google.com")
# browserHistory.visit("facebook.com")
# browserHistory.visit("youtube.com")
# print(browserHistory.back(1))      # "facebook.com"
# print(browserHistory.back(1))      # "google.com"
# print(browserHistory.forward(1))   # "facebook.com"
# browserHistory.visit("linkedin.com")
# print(browserHistory.forward(2))   # "linkedin.com"
# print(browserHistory.back(2))      # "google.com"
# print(browserHistory.back(7))      # "leetcode.com"
```

#### Time and Space Complexity

**Constructor:**
- **Time Complexity:** O(1) - Just creating one node
- **Space Complexity:** O(1) - Single node

**visit(url):**
- **Time Complexity:** O(1) - Create and link new node
- **Space Complexity:** O(1) - One new node per call

**back(steps):**
- **Time Complexity:** O(min(steps, n)) - Where n is history length
- **Space Complexity:** O(1) - Only moving pointer

**forward(steps):**
- **Time Complexity:** O(min(steps, m)) - Where m is forward history length
- **Space Complexity:** O(1) - Only moving pointer

---

## Summary Table

| Problem | Approach | Time | Space |
|---------|----------|------|-------|
| Delete All Occurrences | Traverse & Delete | O(n) | O(1) |
| Remove Duplicates | Track Previous Value | O(n) | O(1) |
| Design Browser History | Doubly Linked List | O(1) visit, O(k) back/forward | O(n) |

---

**Key Concepts for DLL:**
- **Two-way traversal** - Can move both forward and backward
- **Easier deletion** - No need to track previous node separately
- **Update both pointers** - Always update both `prev` and `next` when modifying
- **Edge cases** - Handle head/tail deletion carefully

**Happy Learning! 🚀**
