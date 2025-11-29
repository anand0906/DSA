# Linked List Advanced Problems - Revision Notes

## Table of Contents
6. [Add One to a Number Represented by LL](#6-add-one-to-a-number-represented-by-ll)
7. [Find Middle of Linked List](#7-find-middle-of-linked-list)
8. [Delete the Middle Node in LL](#8-delete-the-middle-node-in-ll)
9. [Check if LL is Palindrome](#9-check-if-ll-is-palindrome)
10. [Find Intersection Point of Y LL](#10-find-intersection-point-of-y-ll)
11. [Detect a Loop in LL](#11-detect-a-loop-in-ll)
12. [Find the Starting Point of Loop](#12-find-the-starting-point-of-loop)
13. [Length of Loop in LL](#13-length-of-loop-in-ll)

---

## 6. Add One to a Number Represented by LL

### Problem Description
Given a linked list where each node represents a digit of a positive integer (stored in normal order, left to right), add 1 to this number and return the modified linked list. The first node contains the leftmost (most significant) digit.

### Examples

#### Example 1
**Input:** `head -> 1 -> 2 -> 3`  
**Output:** `head -> 1 -> 2 -> 4`  
**Explanation:** 123 + 1 = 124

#### Example 2
**Input:** `head -> 9 -> 9`  
**Output:** `head -> 1 -> 0 -> 0`  
**Explanation:** 99 + 1 = 100 (new node added for carry)

### Solution

#### Intuition
When adding 1 to a number, we start from the rightmost digit. If it becomes 10, we carry 1 to the left. Since our list is stored left-to-right but addition happens right-to-left, we reverse the list first to make addition easier, then reverse it back!

#### Approach Steps
1. **Reverse the linked list** to access the least significant digit first
2. **Initialize carry = 1** (we're adding 1)
3. **Traverse the reversed list:**
   - Add current node value + carry
   - Update carry: `carry = sum // 10`
   - Update node value: `node.val = sum % 10`
   - If carry becomes 0, stop early
   - If at end and carry exists, add new node
4. **Reverse the list again** to restore original order
5. **Return head**

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head):
        prev = None
        current = head
        
        while current is not None:
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
        
        return prev
    
    def addOne(self, head):
        # Reverse the list
        head = self.reverseList(head)
        
        current = head
        carry = 1  # Initialize with 1 (adding 1)
        
        while current is not None:
            sum_val = current.val + carry
            carry = sum_val // 10
            current.val = sum_val % 10
            
            # Early termination if no carry
            if carry == 0:
                break
            
            # Add new node if carry remains at end
            if current.next is None and carry != 0:
                current.next = ListNode(carry)
                break
            
            current = current.next
        
        # Reverse back to original order
        head = self.reverseList(head)
        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Three traversals (reverse, add, reverse)
- **Space Complexity:** O(1) - Only using pointers, in-place modification

---

## 7. Find Middle of Linked List

### Problem Description
Given a singly linked list, return the middle node. If the list has even number of nodes, return the **second middle node**.

### Examples

#### Example 1
**Input:** `head -> 3 -> 8 -> 7 -> 1 -> 3`  
**Output:** `7`  
**Explanation:** 5 nodes → middle is 3rd node

#### Example 2
**Input:** `head -> 2 -> 9 -> 1 -> 4 -> 0 -> 4`  
**Output:** `4`  
**Explanation:** 6 nodes → return 4th node (second middle)

### Solution

### Approach 1: Count and Traverse

#### Intuition
Simple math - count total nodes (n), calculate middle position (n//2 + 1), then traverse to that position.

#### Approach Steps
1. Count total nodes in the list
2. Calculate middle position: `mid = (count // 2) + 1`
3. Traverse to the middle position
4. Return middle node

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def middleOfLinkedList(self, head):
        temp = head
        count = 0
        
        # Count nodes
        while temp is not None:
            count += 1
            temp = temp.next
        
        # Calculate middle position
        mid_position = (count // 2) + 1
        
        # Traverse to middle
        middle_node = head
        for _ in range(1, mid_position):
            middle_node = middle_node.next
        
        return middle_node
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Two passes through the list
- **Space Complexity:** O(1) - Only using counters and pointers

---

### Approach 2: Slow-Fast Pointer (Optimal)

#### Intuition
Imagine two runners - one runs twice as fast. When the fast runner finishes, the slow runner is exactly at the middle! This is the "tortoise and hare" technique.

#### Visual Example
```
Initial: both at head
     s,f
      ↓
   3 → 8 → 7 → 1 → 3

After iteration 1:
         s
         ↓
   3 → 8 → 7 → 1 → 3
             ↑
             f

After iteration 2:
             s
             ↓
   3 → 8 → 7 → 1 → 3
                   ↑
                   f (stops)

Slow is at middle!
```

#### Approach Steps
1. Handle edge case: single node → return head
2. Initialize `slow` and `fast` at head
3. While `fast` and `fast.next` exist:
   - Move `slow` one step
   - Move `fast` two steps
4. Return `slow` (it's at middle)

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def middleOfLinkedList(self, head):
        if head.next is None:
            return head
        
        slow = head
        fast = head
        
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        
        return slow
```

#### Time and Space Complexity
- **Time Complexity:** O(n/2) = O(n) - Single pass, half the list
- **Space Complexity:** O(1) - Only two pointers

---

## 8. Delete the Middle Node in LL

### Problem Description
Given a non-empty linked list, delete the middle node. Middle is defined as ⌊n/2⌋ + 1-th node (1-based indexing).

### Examples

#### Example 1
**Input:** `head -> 1 -> 2 -> 3 -> 4 -> 5`  
**Output:** `head -> 1 -> 2 -> 4 -> 5`  
**Explanation:** n=5, ⌊5/2⌋ + 1 = 3 → delete 3rd node

#### Example 2
**Input:** `head -> 7 -> 6 -> 5 -> 4`  
**Output:** `head -> 7 -> 6 -> 4`  
**Explanation:** n=4, ⌊4/2⌋ + 1 = 3 → delete 3rd node

### Solution

### Approach 1: Count and Delete

#### Intuition
Count nodes, find middle index, traverse to the node **before** middle, then adjust its `next` pointer to skip the middle node.

#### Approach Steps
1. Handle edge case: single node → return None
2. Count total nodes (n)
3. Calculate middle index: `mid = n // 2`
4. Traverse to (mid-1)th node
5. Delete middle: `node.next = node.next.next`
6. Return head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def deleteMiddle(self, head):
        if not head or not head.next:
            return None

        temp = head
        n = 0
        
        # Count nodes
        while temp:
            n += 1
            temp = temp.next
        
        middleIndex = n // 2
        temp = head
        
        # Traverse to node before middle
        for _ in range(1, middleIndex):
            temp = temp.next
        
        # Delete middle node
        if temp.next:
            middle = temp.next
            temp.next = temp.next.next
            del middle
        
        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Two passes
- **Space Complexity:** O(1) - Only pointers

---

### Approach 2: Slow-Fast Pointer (Optimal)

#### Intuition
Use slow-fast pointers, but keep a `prev` pointer that stays one step behind `slow`. When `fast` reaches the end, `prev` is just before the middle, making deletion easy!

#### Approach Steps
1. Handle edge case: single node → return None
2. Initialize `slow`, `fast` at head, `prev = None`
3. While `fast` and `fast.next` exist:
   - Update `prev = slow`
   - Move `slow` one step
   - Move `fast` two steps
4. Delete middle: `prev.next = prev.next.next`
5. Return head

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def deleteMiddle(self, head):
        if head.next is None:
            return None
        
        slow = head
        fast = head
        prev = None
        
        while fast and fast.next:
            prev = slow
            slow = slow.next
            fast = fast.next.next
        
        # Delete middle
        prev.next = prev.next.next
        
        return head
```

#### Time and Space Complexity
- **Time Complexity:** O(n/2) = O(n) - Single pass to middle
- **Space Complexity:** O(1) - Three pointers only

---

## 9. Check if LL is Palindrome

### Problem Description
Check if the linked list values form a palindrome (reads same forwards and backwards).

### Examples

#### Example 1
**Input:** `head -> 3 -> 7 -> 5 -> 7 -> 3`  
**Output:** `true`  
**Explanation:** 37573 is a palindrome

#### Example 2
**Input:** `head -> 1 -> 1 -> 2 -> 1`  
**Output:** `false`  
**Explanation:** 1121 is not a palindrome

### Solution

### Approach 1: Stack

#### Intuition
A stack reverses order (LIFO). Push all values onto stack, then pop while traversing the list again. If all values match, it's a palindrome!

#### Approach Steps
1. Create empty stack
2. Traverse list and push all values
3. Reset pointer to head
4. Traverse again, comparing each value with popped value
5. If any mismatch → return False
6. If all match → return True

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def isPalindrome(self, head):
        stack = []
        temp = head
        
        # Push all values
        while temp is not None:
            stack.append(temp.val)
            temp = temp.next
        
        # Compare with popped values
        temp = head
        while temp is not None:
            if temp.val != stack.pop():
                return False
            temp = temp.next
        
        return True
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Two traversals
- **Space Complexity:** O(n) - Stack stores all values

---

### Approach 2: Reverse Second Half (Optimal)

#### Intuition
For a palindrome, first half should equal second half (reversed). Find middle, reverse second half, compare both halves, then restore the list.

#### Approach Steps
1. Handle edge case: empty or single node → True
2. Use slow-fast to find middle
3. Reverse second half (from slow.next)
4. Compare first half with reversed second half
5. If mismatch → restore and return False
6. Restore second half
7. Return True

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseLinkedList(self, head):
        prev = None
        curr = head
        
        while curr is not None:
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
        
        return prev

    def isPalindrome(self, head):
        if head is None or head.next is None:
            return True
        
        # Find middle
        slow = head
        fast = head
        
        while fast.next is not None and fast.next.next is not None:
            slow = slow.next
            fast = fast.next.next
        
        # Reverse second half
        newHead = self.reverseLinkedList(slow.next)
        
        # Compare halves
        first = head
        second = newHead
        
        while second is not None:
            if first.val != second.val:
                self.reverseLinkedList(newHead)
                return False
            first = first.next
            second = second.next
        
        # Restore
        self.reverseLinkedList(newHead)
        return True
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Find middle + reverse + compare
- **Space Complexity:** O(1) - Only pointers

---

## 10. Find Intersection Point of Y LL

### Problem Description
Given two linked lists that may intersect, find the intersection node. If no intersection, return null.

### Examples

#### Example 1
**Input:**  
```
listA: 1 → 2 → 3 ↘
                  4 → 5
listB: 7 → 8 -----↗
```
**Output:** `4`  
**Explanation:** Lists merge at node with value 4

#### Example 2
**Input:**  
```
listA: 1 → 2 → 3
listB: 8 → 9
```
**Output:** `null`  
**Explanation:** No intersection

### Solution

### Approach 1: Hashing

#### Intuition
Store all nodes of first list in a set. Then traverse second list - the first node found in the set is the intersection!

#### Approach Steps
1. Create empty set
2. Traverse listA, add all nodes to set
3. Traverse listB, check if each node exists in set
4. Return first node found in set (or None)

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def getIntersectionNode(self, headA, headB):
        nodes_set = set()

        # Store all nodes from listA
        while headA is not None:
            nodes_set.add(headA)
            headA = headA.next

        # Check listB for intersection
        while headB is not None:
            if headB in nodes_set:
                return headB
            headB = headB.next

        return None
```

#### Time and Space Complexity
- **Time Complexity:** O(n + m) - Traverse both lists
- **Space Complexity:** O(n) - Set stores first list

---

### Approach 2: Length Difference

#### Intuition
If two roads merge, walkers starting from different distances will meet if we give the one farther back a head start equal to the distance difference!

#### Approach Steps
1. Calculate lengths of both lists (n1, n2)
2. Find difference: `diff = |n1 - n2|`
3. Move pointer of longer list `diff` steps forward
4. Now pointers are aligned
5. Traverse together until they meet
6. Return intersection node

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def getIntersectionNode(self, headA, headB):
        temp1 = headA
        temp2 = headB
        n1, n2 = 0, 0

        # Calculate lengths
        while temp1:
            n1 += 1
            temp1 = temp1.next

        while temp2:
            n2 += 1
            temp2 = temp2.next

        # Align and find collision
        if n1 < n2:
            return self.collisionPoint(headA, headB, n2 - n1)
        return self.collisionPoint(headB, headA, n1 - n2)

    def collisionPoint(self, smallerListHead, longerListHead, lengthDiff):
        temp1, temp2 = smallerListHead, longerListHead

        # Advance longer list
        for _ in range(lengthDiff):
            temp2 = temp2.next

        # Find intersection
        while temp1 != temp2:
            temp1 = temp1.next
            temp2 = temp2.next

        return temp1
```

#### Time and Space Complexity
- **Time Complexity:** O(n + m) - Calculate lengths + traverse
- **Space Complexity:** O(1) - Only pointers

---

### Approach 3: Two Pointers (Most Elegant)

#### Intuition
Two pointers traverse both lists. When one reaches the end, it switches to the other list's head. Both travel the same total distance and meet at intersection!

#### Visual Example
```
ListA: 1 → 2 → 3 ↘
                   4 → 5 (length 5)
ListB:     7 → 8 ↗ (length 4)

d1: 1→2→3→4→5→(switch)→7→8→4 (meets here)
d2: 7→8→4→5→(switch)→1→2→3→4 (meets here)
```

#### Approach Steps
1. Initialize `d1` at headA, `d2` at headB
2. Traverse both:
   - Move both forward
   - If they meet, return node
   - If `d1` reaches end, reset to headB
   - If `d2` reaches end, reset to headA
3. Return intersection (or None)

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def getIntersectionNode(self, headA, headB):
        if headA is None or headB is None:
            return None

        d1, d2 = headA, headB

        while d1 != d2:
            d1 = d1.next
            d2 = d2.next

            if d1 == d2:
                return d1

            # Switch lists when reaching end
            if d1 is None:
                d1 = headB
            if d2 is None:
                d2 = headA

        return d1
```

#### Time and Space Complexity
- **Time Complexity:** O(n + m) - At most 2 full traversals
- **Space Complexity:** O(1) - Two pointers only

---

## 11. Detect a Loop in LL

### Problem Description
Determine if a linked list contains a cycle (loop). A loop exists if a node can be reached again by following next pointers.

### Examples

#### Example 1
**Input:** `head -> 1 -> 2 -> 3 -> 4 -> 5`, pos = 1  
(tail connects to node at index 1)  
**Output:** `true`

#### Example 2
**Input:** `head -> 1 -> 3 -> 7 -> 4`, pos = -1  
**Output:** `false`

### Solution

### Approach 1: Hashing

#### Intuition
Keep a record of visited nodes. If we encounter a node we've already seen, there's a loop!

#### Approach Steps
1. Create empty set
2. Traverse list with pointer
3. For each node:
   - If in set → return True (loop found)
   - Add to set
4. If reach None → return False

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def hasCycle(self, head):
        temp = head
        nodeSet = set()

        while temp is not None:
            if temp in nodeSet:
                return True
            nodeSet.add(temp)
            temp = temp.next

        return False
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single traversal
- **Space Complexity:** O(n) - Set stores nodes

---

### Approach 2: Floyd's Cycle Detection (Optimal)

#### Intuition
Like a circular race track - if two runners start together with one running twice as fast, the faster one will lap the slower one if there's a loop!

#### Visual Example
```
Loop: 1 → 2 → 3 → 4 → 5
          ↑___________|

Initial: slow=1, fast=1
Step 1:  slow=2, fast=3
Step 2:  slow=3, fast=5
Step 3:  slow=4, fast=4 ✓ Meet!
```

#### Approach Steps
1. Initialize `slow` and `fast` at head
2. While `fast` and `fast.next` exist:
   - Move `slow` one step
   - Move `fast` two steps
   - If `slow == fast` → return True
3. If loop ends → return False

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def hasCycle(self, head):
        slow = head
        fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                return True

        return False
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Traverse the list
- **Space Complexity:** O(1) - Two pointers only

---

## 12. Find the Starting Point of Loop

### Problem Description
If a loop exists in a linked list, find the node where the loop starts. Return null if no loop.

### Examples

#### Example 1
**Input:** `head -> 1 -> 2 -> 3 -> 4 -> 5`, pos = 1  
**Output:** `2`  
**Explanation:** Loop starts at node with value 2

#### Example 2
**Input:** `head -> 1 -> 3 -> 7 -> 4`, pos = -1  
**Output:** `null`  
**Explanation:** No loop

### Solution

### Approach 1: Hashing

#### Intuition
Store visited nodes in a dictionary. The first node we encounter again is the loop's starting point!

#### Approach Steps
1. Create empty dictionary
2. Traverse list
3. For each node:
   - If in dictionary → return node (loop start)
   - Store node in dictionary
4. If reach None → return None

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def findStartingPoint(self, head):
        temp = head
        visited = {}
        
        while temp is not None:
            if temp in visited:
                return temp
            visited[temp] = True
            temp = temp.next
        
        return None
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single traversal
- **Space Complexity:** O(n) - Dictionary stores nodes

---

### Approach 2: Floyd's Algorithm (Optimal)

#### Intuition
**Phase 1:** Detect loop using slow-fast pointers.  
**Phase 2:** Once they meet, reset slow to head. Move both one step at a time. They'll meet at the loop's starting point! (Mathematical proof based on distances)

#### Approach Steps
1. **Phase 1 - Detect Loop:**
   - Use slow-fast pointers
   - If they meet, loop exists
2. **Phase 2 - Find Start:**
   - Reset `slow` to head
   - Move both one step at a time
   - When they meet again → loop start
3. If no loop → return None

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def findStartingPoint(self, head):
        slow = head
        fast = head

        # Phase 1: Detect loop
        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                # Loop detected, find start
                slow = head

                # Phase 2: Find starting point
                while slow != fast:
                    slow = slow.next
                    fast = fast.next

                return slow
        
        # No loop
        return None
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Two phases of traversal
- **Space Complexity:** O(1) - Two pointers only

---

## 13. Length of Loop in LL

### Problem Description
If a loop exists, find its length (number of nodes in the loop). Return 0 if no loop.

### Examples

#### Example 1
**Input:** `head -> 1 -> 2 -> 3 -> 4 -> 5`, pos = 1  
**Output:** `4`  
**Explanation:** Loop: 2 → 3 → 4 → 5 → 2 (4 nodes)

#### Example 2
**Input:** `head -> 1 -> 3 -> 7 -> 4`, pos = -1  
**Output:** `0`  
**Explanation:** No loop

### Solution

### Approach 1: Hashing with Timer

#### Intuition
Store each node with a timer value. When we revisit a node, the difference between current timer and stored timer gives the loop length!

#### Approach Steps
1. Create dictionary and timer
2. Traverse list:
   - If node in dictionary → return `timer - visited[node]`
   - Store node with current timer
   - Increment timer
3. If reach None → return 0

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def findLengthOfLoop(self, head):
        visited_nodes = {}
        temp = head
        timer = 0

        while temp is not None:
            if temp in visited_nodes:
                # Calculate loop length
                loop_length = timer - visited_nodes[temp]
                return loop_length
            
            visited_nodes[temp] = timer
            temp = temp.next
            timer += 1

        return 0
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Single traversal
- **Space Complexity:** O(n) - Dictionary stores nodes

---

### Approach 2: Floyd's Algorithm (Optimal)

#### Intuition
First detect loop using slow-fast. Once they meet, keep one pointer fixed and move the other around the loop counting steps until they meet again. That count is the loop length!

#### Approach Steps
1. Use slow-fast to detect loop
2. If no loop → return 0
3. Once they meet:
   - Keep `slow` fixed
   - Move `fast` one step at a time
   - Count steps until `fast` meets `slow` again
4. Return count

#### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def findLengthOfLoop(self, head):
        slow = head
        fast = head
        
        # Detect loop
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                break
        else:
            # No loop
            return 0
        
        # Count loop length
        length = 1
        fast = fast.next
        while slow != fast:
            fast = fast.next
            length += 1
        
        return length
```

#### Time and Space Complexity
- **Time Complexity:** O(n) - Detect loop + count length
- **Space Complexity:** O(1) - Two pointers only

---

## Summary Table

| Problem | Best Approach | Time | Space |
|---------|--------------|------|-------|
| Add One | Reverse Method | O(n) | O(1) |
| Find Middle | Slow-Fast Pointer | O(n) | O(1) |
| Delete Middle | Slow-Fast with Prev | O(n) | O(1) |
| Palindrome Check | Reverse Second Half | O(n) | O(1) |
| Intersection Point | Two Pointers | O(n+m) | O(1) |
| Detect Loop | Floyd's Algorithm | O(n) | O(1) |
| Loop Starting Point | Floyd's Algorithm | O(n) | O(1) |
| Loop Length | Floyd's + Count | O(n) | O(1) |

---

**Happy Learning! 🚀**
