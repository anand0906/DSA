## Table of Contents
14. [Reverse LL in Groups of Size K](#14-reverse-ll-in-groups-of-size-k)
15. [Rotate a Linked List](#15-rotate-a-linked-list)
16. [Merge K Sorted Lists](#16-merge-k-sorted-lists)
17. [Flattening of LL](#17-flattening-of-ll)
18. [Sort Linked List](#18-sort-linked-list)
19. [Clone LL with Random and Next Pointer](#19-clone-ll-with-random-and-next-pointer)

---

## 14. Reverse LL in Groups of Size K

### Problem Description
Given a linked list, reverse the nodes in groups of k. If the number of nodes is not a multiple of k, the remaining nodes should stay as is (not reversed). Only change the links, not the node values.

### Examples

#### Example 1
**Input:** `head -> 1 -> 2 -> 3 -> 4 -> 5`, k = 2  
**Output:** `head -> 2 -> 1 -> 4 -> 3 -> 5`  
**Explanation:** Groups (1→2) and (3→4) are reversed

#### Example 2
**Input:** `head -> 1 -> 2 -> 3 -> 4 -> 5`, k = 3  
**Output:** `head -> 3 -> 2 -> 1 -> 4 -> 5`  
**Explanation:** Group (1→2→3) is reversed, (4→5) stays same

### Solution

#### Intuition
We need to reverse k nodes at a time. For each group:
1. Find the kth node from current position
2. If we have k nodes, reverse them
3. Connect the reversed group to previous and next groups
4. If less than k nodes remain, leave them unchanged

Think of it like flipping pages in a book - flip k pages at a time, but if you have fewer than k pages left, leave them as is!

#### Approach Steps
1. **Helper function `findKthNode(node, k)`:** Find the kth node from given node
2. **Helper function `reverse(head)`:** Reverse a linked list
3. **Main algorithm:**
   - Initialize `temp`, `prevNode`, `nextNode`
   - While `temp` exists:
     - Find kth node from temp
     - If kth node doesn't exist (less than k nodes left):
       - Connect prevNode to remaining nodes and break
     - Store the next node after kth node
     - Break the connection (set kth.next = None)
     - Reverse the k-node group
     - Update head if this is the first group
     - Connect previous group to this reversed group
     - Update prevNode to current group's last node (original temp)
     - Move temp to nextNode
4. Return head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def findKthNode(node, k):
    """Find the kth node from the given node"""
    cnt = 1
    temp = node
    while temp:
        if cnt == k:
            return temp
        temp = temp.next
        cnt += 1
    return None

def reverse(head):
    """Reverse a linked list and return new head"""
    temp = head
    prev = None
    while temp:
        front = temp.next
        temp.next = prev
        prev = temp
        temp = front
    return prev

class Solution:
    def reverseKGroup(self, head, k):
        temp = head
        prevNode = None
        nextNode = None
        
        while temp:
            # Find the kth node from current position
            kthNode = findKthNode(temp, k)
            
            # If less than k nodes remain, don't reverse
            if kthNode is None:
                if prevNode:
                    prevNode.next = temp
                break
            
            # Store next node after kth node
            nextNode = kthNode.next
            # Break the connection
            kthNode.next = None
            
            # Reverse the k-node group
            reverse(temp)
            
            # Update head if this is first group
            if temp == head:
                head = kthNode
            else:
                # Connect previous group to current reversed group
                prevNode.next = kthNode
            
            # Update prevNode (temp is now last node of reversed group)
            prevNode = temp
            # Move to next group
            temp = nextNode
        
        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Visit each node twice (once to find kth, once to reverse)
- **Space Complexity:** O(1) - Only using pointers, in-place reversal

---

## 15. Rotate a Linked List

### Problem Description
Rotate the linked list to the right by k places. This means moving the last k nodes to the front.

### Examples

#### Example 1
**Input:** `head -> 1 -> 2 -> 3 -> 4 -> 5`, k = 2  
**Output:** `head -> 4 -> 5 -> 1 -> 2 -> 3`  
**Explanation:** Last 2 nodes move to front

#### Example 2
**Input:** `head -> 1 -> 2 -> 3 -> 4 -> 5`, k = 7  
**Output:** `head -> 4 -> 5 -> 1 -> 2 -> 3`  
**Explanation:** k=7 is same as k=2 (7 % 5 = 2)

### Solution

### Approach 1: Rotate k Times (Brute Force)

#### Intuition
Perform k single rotations. Each rotation moves the last node to the front.

#### Approach Steps
1. For each rotation (k times):
   - Find second-last node
   - Get last node
   - Break link between second-last and last
   - Make last node new head
2. Return head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def rotateRight(self, head, k):
        if not head or not head.next:
            return head

        # Rotate k times
        for _ in range(k):
            temp = head
            # Find second last node
            while temp.next.next:
                temp = temp.next
            
            # Get last node
            end = temp.next
            # Break link
            temp.next = None
            # Make last node new head
            end.next = head
            head = end

        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(n × k) - For each of k rotations, traverse n nodes
- **Space Complexity:** O(1) - Only pointers

---

### Approach 2: Optimized (Single Pass)

#### Intuition
Instead of rotating k times, find the new head directly:
1. Calculate length
2. Reduce k using modulo (k = k % length)
3. Find the break point at position (length - k)
4. Make the list circular, then break at correct position

#### Approach Steps
1. Handle edge cases: empty, single node, or k=0
2. **Calculate length and find last node**
3. **Optimize k:** `k = k % length` (if k=0 after this, return head)
4. **Find break point:** Node at position `(length - k)`
5. **Rearrange:**
   - New head = breakpoint.next
   - Break link: breakpoint.next = None
   - Connect last node to old head
6. Return new head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def rotateRight(self, head, k):
        # Edge cases
        if head is None or head.next is None or k == 0:
            return head

        # Step 1: Calculate length and find last node
        length = 0
        current = head
        last_node = head

        while current:
            last_node = current
            current = current.next
            length += 1

        # Step 2: Optimize k
        k = k % length
        if k == 0:
            return head

        # Step 3: Find break point at position (length - k)
        current = head
        position = 1
        break_point_node = None

        while current:
            if position == (length - k):
                break_point_node = current
                break
            current = current.next
            position += 1

        # Step 4: Rearrange links
        new_head = break_point_node.next
        break_point_node.next = None
        last_node.next = head

        return new_head
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single pass to find length, another to find break point
- **Space Complexity:** O(1) - Only using pointers

---

## 16. Merge K Sorted Lists

### Problem Description
Given k sorted linked lists, merge them into one sorted linked list.

### Examples

#### Example 1
**Input:**  
```
List 1: 1 -> 4 -> 5
List 2: 1 -> 3 -> 4
List 3: 2 -> 6
```
**Output:** `1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6`

#### Example 2
**Input:**  
```
List 1: 2 -> 4
List 2: 1 -> 3
```
**Output:** `1 -> 2 -> 3 -> 4`

### Solution

### Approach 1: Brute Force (Collect and Sort)

#### Intuition
Collect all values from all lists into an array, sort it, then create a new linked list from the sorted array.

#### Approach Steps
1. Traverse all k lists and collect all values in an array
2. Sort the array
3. Create a new linked list from sorted array
4. Return head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists):
        # Collect all values
        arr = []
        for head in lists:
            temp = head
            while temp:
                arr.append(temp.val)
                temp = temp.next
        
        # Sort array
        arr.sort()
        
        # Create new list
        dummy = ListNode(-1)
        temp = dummy
        for val in arr:
            temp.next = ListNode(val)
            temp = temp.next
        
        return dummy.next
```

#### Time and Space Complexity
- **Time Complexity:** O(N log N) where N = total nodes across all lists
- **Space Complexity:** O(N) - Array stores all values

---

### Approach 2: Merge Lists Pairwise (Optimal)

#### Intuition
Merge lists two at a time, similar to merge sort. Use the standard two-list merge algorithm repeatedly.

#### Approach Steps
1. **Helper function `mergeTwoLists`:** Merge two sorted lists
2. **Main algorithm:**
   - If no lists or empty, return None
   - While more than one list exists:
     - Merge lists pairwise and store results
     - Update lists array with merged results
   - Return the final merged list

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeTwoLists(self, l1, l2):
        """Merge two sorted linked lists"""
        dummy = ListNode(-1)
        temp = dummy
        
        while l1 and l2:
            if l1.val < l2.val:
                temp.next = l1
                l1 = l1.next
            else:
                temp.next = l2
                l2 = l2.next
            temp = temp.next
        
        # Attach remaining nodes
        if l1:
            temp.next = l1
        if l2:
            temp.next = l2
        
        return dummy.next
    
    def mergeKLists(self, lists):
        if not lists or len(lists) == 0:
            return None
        
        # Merge lists pairwise until one list remains
        while len(lists) > 1:
            merged_lists = []
            
            # Merge pairs of lists
            for i in range(0, len(lists), 2):
                l1 = lists[i]
                l2 = lists[i + 1] if i + 1 < len(lists) else None
                merged_lists.append(self.mergeTwoLists(l1, l2))
            
            lists = merged_lists
        
        return lists[0]
```

#### Time and Space Complexity
- **Time Complexity:** O(N log k) where N = total nodes, k = number of lists
  - log k levels of merging, each level processes N nodes
- **Space Complexity:** O(1) - Only pointers (excluding recursion stack if using recursive merge)

---

## 17. Flattening of LL

### Problem Description
Given a special linked list where each node has:
- `next`: Points to next node in main list
- `child`: Points to a sorted vertical list

Flatten the list so all nodes appear in a single sorted layer connected by `child` pointers.

### Examples

#### Example 1
**Input:**  
```
[[3], [2, 10], [1, 7, 11, 12], [4, 9], [5, 6]]
```
**Output:** `1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 9 -> 10 -> 11 -> 12`

#### Example 2
**Input:**  
```
[[4, 10], [2, 5, 20], [12, 13, 16, 17]]
```
**Output:** `2 -> 4 -> 5 -> 10 -> 12 -> 13 -> 16 -> 17 -> 20`

### Solution

### Approach 1: Brute Force (Collect and Sort)

#### Intuition
Collect all values from all vertical lists into an array, sort it, then create a new flattened list.

#### Approach Steps
1. Traverse the main list (using `next`)
2. For each node, traverse its vertical list (using `child`)
3. Collect all values in an array
4. Sort the array
5. Create new linked list from sorted array using `child` pointers
6. Return head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None, child=None):
        self.val = val
        self.next = next
        self.child = child

class Solution:
    def convertArrToLinkedList(self, arr):
        """Convert array to linked list using child pointers"""
        dummyNode = ListNode(-1)
        temp = dummyNode

        for val in arr:
            temp.child = ListNode(val)
            temp = temp.child
        
        return dummyNode.child

    def flattenLinkedList(self, head):
        arr = []

        # Traverse main list
        while head is not None:
            # Traverse vertical list
            t2 = head
            while t2 is not None:
                arr.append(t2.val)
                t2 = t2.child
            
            head = head.next

        # Sort and convert back
        arr.sort()
        return self.convertArrToLinkedList(arr)
```

#### Time and Space Complexity
- **Time Complexity:** O(N log N) where N = total nodes
- **Space Complexity:** O(N) - Array stores all values

---

### Approach 2: Merge Lists Recursively (Optimal)

#### Intuition
Since each vertical list is already sorted, we can merge them like merging sorted lists:
1. Recursively flatten the next part
2. Merge current vertical list with the flattened next part
3. This is similar to merge sort!

#### Approach Steps
1. **Helper function `merge(list1, list2)`:**
   - Merge two sorted lists using `child` pointers
   - Set `next` to None to break horizontal connections
2. **Main recursive function:**
   - Base case: if head is None or head.next is None, return head
   - Recursively flatten head.next
   - Merge current vertical list with flattened rest
   - Return merged head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None, child=None):
        self.val = val
        self.next = next
        self.child = child

class Solution:
    def merge(self, list1, list2):
        """Merge two sorted lists using child pointers"""
        dummyNode = ListNode(-1)
        res = dummyNode

        # Merge based on values
        while list1 is not None and list2 is not None:
            if list1.val < list2.val:
                res.child = list1
                res = list1
                list1 = list1.child
            else:
                res.child = list2
                res = list2
                list2 = list2.child
            res.next = None  # Break horizontal links

        # Attach remaining nodes
        if list1:
            res.child = list1
        else:
            res.child = list2

        # Prevent cycles
        if dummyNode.child:
            dummyNode.child.next = None

        return dummyNode.child

    def flattenLinkedList(self, head):
        # Base case
        if head is None or head.next is None:
            return head

        # Recursively flatten the rest
        mergedHead = self.flattenLinkedList(head.next)

        # Merge current with flattened rest
        head = self.merge(head, mergedHead)
        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(N × M) where N = nodes in main list, M = average vertical list length
- **Space Complexity:** O(N) - Recursion stack depth equals number of main list nodes

---

## 18. Sort Linked List

### Problem Description
Sort a linked list in non-decreasing order.

### Examples

#### Example 1
**Input:** `head -> 5 -> 6 -> 1 -> 2 -> 1`  
**Output:** `head -> 1 -> 1 -> 2 -> 5 -> 6`

#### Example 2
**Input:** `head -> 6 -> 5 -> -1 -> -2 -> -3`  
**Output:** `head -> -3 -> -2 -> -1 -> 5 -> 6`

### Solution

### Approach 1: Collect, Sort, Reassign

#### Intuition
Simple approach: collect all values in an array, sort the array, then reassign sorted values back to nodes.

#### Approach Steps
1. Traverse list and collect all values in array
2. Sort the array
3. Traverse list again and assign sorted values
4. Return head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def sortList(self, head):
        # Collect values
        arr = []
        temp = head
        
        while temp:
            arr.append(temp.val)
            temp = temp.next

        # Sort array
        arr.sort()

        # Reassign sorted values
        temp = head
        for val in arr:
            temp.val = val
            temp = temp.next

        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(n log n) - Sorting dominates
- **Space Complexity:** O(n) - Array stores all values

---

### Approach 2: Merge Sort (Optimal)

#### Intuition
Use merge sort algorithm on linked list:
1. Find middle and split list into two halves
2. Recursively sort both halves
3. Merge the two sorted halves

This is optimal because it's in-place (only modifies links) and has O(n log n) complexity!

#### Approach Steps
1. **Helper function `findMiddle(head)`:**
   - Use slow-fast pointers to find middle
   - Important: fast starts at head.next (to get first middle for even length)
2. **Helper function `merge(list1, list2)`:**
   - Merge two sorted lists
3. **Main recursive function:**
   - Base case: if head is None or single node, return head
   - Find middle
   - Split into left and right (middle.next = None)
   - Recursively sort both halves
   - Merge sorted halves
   - Return merged result

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def merge(list1, list2):
    """Merge two sorted linked lists"""
    dummy = ListNode(-1)
    temp = dummy
    temp1 = list1
    temp2 = list2
    
    while temp1 and temp2:
        if temp1.val < temp2.val:
            temp.next = temp1
            temp1 = temp1.next
        else:
            temp.next = temp2
            temp2 = temp2.next
        temp = temp.next
    
    if temp1:
        temp.next = temp1
    if temp2:
        temp.next = temp2
    
    return dummy.next

def findMiddle(head):
    """Find middle using slow-fast pointers"""
    fast = head.next  # Start fast at head.next
    slow = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow

class Solution:
    def sortList(self, head):
        # Base case
        if head is None or head.next is None:
            return head
        
        # Find middle and split
        middle = findMiddle(head)
        left = head
        right = middle.next
        middle.next = None  # Split the list
        
        # Recursively sort both halves
        left = self.sortList(left)
        right = self.sortList(right)
        
        # Merge sorted halves
        return merge(left, right)
```

#### Time and Space Complexity
- **Time Complexity:** O(n log n) - Standard merge sort complexity
- **Space Complexity:** O(log n) - Recursion stack depth

---

## 19. Clone LL with Random and Next Pointer

### Problem Description
Create a deep copy of a linked list where each node has:
- `next`: Points to next node
- `random`: Points to any random node (or None)

### Examples

#### Example 1
**Input:**  
```
1 (random -> 3) -> 2 (random -> 1) -> 3 (random -> None)
```
**Output:** Complete deep copy with same structure

### Solution

### Approach 1: Using HashMap

#### Intuition
Use a hashmap to map original nodes to their copies:
1. First pass: Create copy of each node and store mapping
2. Second pass: Set random pointers using the mapping

#### Approach Steps
1. Create dummy node
2. **First pass:**
   - For each node, create a copy
   - Store mapping: `map[originalNode] = copyNode`
   - Build next connections
3. **Second pass:**
   - For each original node, set copy's random using map
   - `copy.random = map[original.random]`
4. Return dummy.next

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None, random=None):
        self.val = val
        self.next = next
        self.random = random

class Solution:
    def copyRandomList(self, head):
        dummy = ListNode(-1)
        temp = head
        temp2 = dummy
        map_dict = {}
        
        # First pass: Create copies and build map
        while temp:
            copy = ListNode(temp.val)
            temp2.next = copy
            map_dict[temp] = copy
            temp = temp.next
            temp2 = temp2.next
        
        # Second pass: Set random pointers
        temp = head
        while temp:
            copy = map_dict[temp]
            if temp.random:
                copy.random = map_dict[temp.random]
            temp = temp.next
        
        return dummy.next
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Two passes
- **Space Complexity:** O(n) - HashMap stores all nodes

---

### Approach 2: Interweaving Nodes (Optimal)

#### Intuition
Clever trick to avoid extra space:
1. Insert copy of each node right after the original
2. Set random pointers (copy.random = original.random.next)
3. Separate the two lists

Pattern: `1 -> 1' -> 2 -> 2' -> 3 -> 3'`

#### Approach Steps
1. **Insert copies in between:**
   - For each node, create copy and insert after it
   - Original: 1 -> 2 -> 3
   - After: 1 -> 1' -> 2 -> 2' -> 3 -> 3'
2. **Connect random pointers:**
   - For each original node's copy:
   - `copy.random = original.random.next`
3. **Separate lists:**
   - Extract copied nodes
   - Restore original list
4. Return copy head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None, random=None):
        self.val = val
        self.next = next
        self.random = random

class Solution:
    def insertCopyInBetween(self, head):
        """Insert copy of each node right after it"""
        temp = head
        while temp:
            nextElement = temp.next
            copy = ListNode(temp.val)
            copy.next = nextElement
            temp.next = copy
            temp = nextElement

    def connectRandomPointers(self, head):
        """Set random pointers for copied nodes"""
        temp = head
        while temp:
            copyNode = temp.next
            
            if temp.random:
                copyNode.random = temp.random.next
            else:
                copyNode.random = None
            
            temp = temp.next.next

    def getDeepCopyList(self, head):
        """Extract copied list and restore original"""
        temp = head
        dummyNode = ListNode(-1)
        res = dummyNode

        while temp:
            # Point to copied node
            res.next = temp.next
            res = res.next

            # Restore original list
            temp.next = temp.next.next
            temp = temp.next
        
        return dummyNode.next

    def copyRandomList(self, head):
        if not head:
            return None

        # Step 1: Insert copies
        self.insertCopyInBetween(head)
        # Step 2: Connect random pointers
        self.connectRandomPointers(head)
        # Step 3: Separate and return copy
        return self.getDeepCopyList(head)
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Three passes through the list
- **Space Complexity:** O(1) - No extra space (only the new list being created)

---

## 20. Delete All Occurrences in DLL

### Problem Description
Given a doubly linked list and a target value, delete all nodes with that value.

### Examples

#### Example 1
**Input:** `head -> 1 <-> 2 <-> 3 <-> 1 <-> 4`, target = 1  
**Output:** `head -> 2 <-> 3 <-> 4`

#### Example 2
**Input:** `head -> 2 <-> 3 <-> -1 <-> 4 <-> 2`, target = 2  
**Output:** `head -> 3 <-> -1 <-> 4`

### Solution

#### Intuition
Traverse the list. When we find a node with target value:
1. If it's head, update head
2. Update prev and next pointers to skip this node
3. Handle edge cases for first/last nodes

#### Approach Steps
1. Traverse list with temp pointer
2. For each node:
   - If value == target:
     - If it's head, update head to next node
     - Get nextNode and prevNode
     - Update connections: skip current node
     - Move temp to nextNode
   - Else: move temp forward
3. Return head

#### Code
```python
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
                # Update head if needed
                if temp == head:
                    head = temp.next
                
                nextNode = temp.next
                prevNode = temp.prev
                
                # Update connections
                if nextNode:
                    nextNode.prev = prevNode
                if prevNode:
                    prevNode.next = nextNode
                
                temp = nextNode
            else:
                temp = temp.next
        
        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single traversal
- **Space Complexity:** O(1) - Only pointers

---


#### Example 1
**Input:** `head -> 1 <-> 1 <-> 3 <-> 3 <-> 4 <-> 5`  
**Output:** `head -> 1 <-> 3 <-> 4 <-> 5`
