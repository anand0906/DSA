# Linked List Problems - Revision Notes

## Table of Contents
1. [Add Two Numbers in Linked List](#1-add-two-numbers-in-linked-list)
2. [Segregate Odd and Even Nodes](#2-segregate-odd-and-even-nodes)
3. [Sort a Linked List of 0's, 1's and 2's](#3-sort-a-linked-list-of-0s-1s-and-2s)
4. [Remove Nth Node from the Back](#4-remove-nth-node-from-the-back)
5. [Reverse a Linked List](#5-reverse-a-linked-list)

---

## 1. Add Two Numbers in Linked List

### Problem Description
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in **reverse order**, meaning the first node contains the ones place, the second node contains the tens place, and so on. Your task is to add these two numbers and return the sum as a linked list, also in reverse order.

Think of it like adding numbers digit by digit from right to left, just like we do on paper, but here the numbers are already reversed for us!

### Examples

#### Input
```
l1 = head -> 5 -> 4
l2 = head -> 4
```

#### Output
```
head -> 9 -> 4
```

#### Explanation
- l1 represents 45 (reversed: 5 is ones place, 4 is tens place)
- l2 represents 4 (reversed: 4 is ones place)
- 45 + 4 = 49
- Result in reversed form: 9 -> 4

---

#### Input
```
l1 = head -> 4 -> 5 -> 6
l2 = head -> 1 -> 2 -> 3
```

#### Output
```
head -> 5 -> 7 -> 9
```

#### Explanation
- l1 represents 654 (reversed: 4, 5, 6)
- l2 represents 321 (reversed: 1, 2, 3)
- 654 + 321 = 975
- Result in reversed form: 5 -> 7 -> 9

### Solution
Traverse both linked lists simultaneously, adding corresponding digits along with any carry from the previous addition. Create a new node for each digit of the result. Handle remaining nodes if one list is longer, and don't forget to add a final node if there's a leftover carry.

### Intuition
Since the numbers are already in reverse order, we can add them digit by digit from the beginning of both lists, just like adding numbers on paper from right to left. We maintain a carry variable to handle cases where the sum of two digits exceeds 9. This is similar to how you'd add 47 + 38 on paper: 7+8=15 (write 5, carry 1), then 4+3+1=8.

### Approach Steps

1. **Create a dummy head node** to simplify the result list construction
2. **Initialize pointers**: `current` for building result, `node1` and `node2` for traversing input lists, and `rem` (remainder/carry) = 0
3. **While both lists have nodes**:
   - Add values from both nodes + carry
   - Calculate new carry: `rem = sum // 10`
   - Create new node with digit: `sum % 10`
   - Move all pointers forward
4. **Process remaining nodes in l1** (if l1 is longer):
   - Add node value + carry
   - Update carry and create new nodes
5. **Process remaining nodes in l2** (if l2 is longer):
   - Add node value + carry
   - Update carry and create new nodes
6. **If carry remains after all nodes**, create one final node with the carry value
7. **Return `head.next`** (skipping the dummy node)

### Code

**Approach 1: Processing Lists Separately**
```python
# Definition of singly Linked List
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def addTwoNumbers(self, l1, l2):
        # Dummy head to simplify list construction
        head = ListNode(-1)
        current = head
        node1 = l1
        node2 = l2
        rem = 0  # Carry/remainder
        
        # Process both lists while both have nodes
        while node1 and node2:
            s = node1.val + node2.val + rem
            rem = s // 10
            node = ListNode(s % 10)
            current.next = node
            current = node
            node1 = node1.next
            node2 = node2.next
        
        # Process remaining nodes in l1
        while node1:
            s = node1.val + rem
            rem = s // 10
            node = ListNode(s % 10)
            current.next = node
            current = node
            node1 = node1.next
        
        # Process remaining nodes in l2
        while node2:
            s = node2.val + rem
            rem = s // 10
            node = ListNode(s % 10)
            current.next = node
            current = node
            node2 = node2.next
        
        # Handle final carry
        if rem != 0:
            node = ListNode(rem)
            current.next = node
            current = node
        
        return head.next
```

**Approach 2: Single Loop (Cleaner & Optimal)**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        # Dummy node to act as the starting point of the result list
        dummy = ListNode(0)
        # Temp pointer to build the result list
        temp = dummy
        # Initialize carry
        carry = 0

        # Iterate while there are nodes in l1 or l2, or there's a carry to process
        while l1 or l2 or carry:
            sum = 0

            # Add the value from l1 if available
            if l1:
                sum += l1.val
                l1 = l1.next

            # Add the value from l2 if available
            if l2:
                sum += l2.val
                l2 = l2.next

            # Add the carry
            sum += carry
            # Update the carry
            carry = sum // 10

            # Create a new node with the digit value and attach it to the result list
            node = ListNode(sum % 10)
            temp.next = node
            # Move to the next position in the result list
            temp = temp.next

        # Return the result list skipping the dummy node
        return dummy.next
```

### Time and Space Complexity

**Approach 1**:
- **Time Complexity**: O(max(m, n)) - Three separate loops, but linear overall
- **Space Complexity**: O(max(m, n)) - Result list size

**Approach 2 (Optimal)**:
- **Time Complexity**: O(max(m, n)) - Single loop processes all nodes
- **Space Complexity**: O(max(m, n)) - Result list size
- Cleaner code with single loop condition

---

## 2. Segregate Odd and Even Nodes

### Problem Description
Given a linked list, rearrange it so that all nodes at odd positions (1st, 3rd, 5th, etc.) come first, followed by all nodes at even positions (2nd, 4th, 6th, etc.). The relative order within odd and even groups must remain the same as the original list.

**Note**: We're talking about the **position** of nodes (their index), not their values!

### Examples

#### Input
```
head -> 1 -> 2 -> 3 -> 4 -> 5
```

#### Output
```
head -> 1 -> 3 -> 5 -> 2 -> 4
```

#### Explanation
- Odd positioned nodes (index 1, 3, 5): 1, 3, 5
- Even positioned nodes (index 2, 4): 2, 4
- Combine them: odd group first, then even group

---

#### Input
```
head -> 4 -> 3 -> 2 -> 1
```

#### Output
```
head -> 4 -> 2 -> 3 -> 1
```

#### Explanation
- Odd positioned nodes (index 1, 3): 4, 2
- Even positioned nodes (index 2, 4): 3, 1
- Result: 4 -> 2 -> 3 -> 1

### Solution
Traverse the linked list once and separate nodes into two groups (odd-positioned and even-positioned). 

**Approach 1**: Use arrays to store values, then refill the list.

**Approach 2 (Optimal)**: Rearrange node pointers directly without extra space by maintaining two separate chains (odd and even), then connecting them.

### Intuition

**Approach 1**: Think of this like organizing a queue where people at odd positions (1st, 3rd, 5th) go to one counter and people at even positions (2nd, 4th) go to another counter. After separating them, the odd counter people are served first, then the even counter people. We use a counter to track positions and arrays to store the values temporarily.

**Approach 2**: Instead of storing values in arrays, imagine you have two separate chains - one for odd-positioned nodes and one for even-positioned nodes. As you walk through the original list, you unhook each node and attach it to the appropriate chain. Finally, you connect the odd chain to the even chain. This is more efficient because you're rearranging actual links, not copying values.

### Approach Steps

**Approach 1 (Using Arrays)**:
1. **Initialize two empty arrays** `even` and `odd` to store values
2. **Initialize counter** `cnt = 0` to track position (0-indexed)
3. **Traverse the linked list**:
   - If position is even (cnt % 2 == 0), add value to `even` array
   - If position is odd, add value to `odd` array
   - Increment counter
4. **Reset counter** and pointer to head
5. **Traverse the list again**:
   - First fill with values from `even` array
   - Then fill with values from `odd` array
6. **Return the head** of modified list

**Approach 2 (Pointer Rearrangement - Optimal)**:
1. **Handle edge cases**: If list is empty or has only one node, return head
2. **Initialize pointers**:
   - `odd` = head (points to first odd-positioned node)
   - `even` = head.next (points to first even-positioned node)
   - `firstEven` = head.next (remember where even chain starts)
3. **Rearrange nodes** while even and even.next exist:
   - Connect odd node to next odd node: `odd.next = odd.next.next`
   - Connect even node to next even node: `even.next = even.next.next`
   - Move both pointers forward
4. **Connect chains**: `odd.next = firstEven`
5. **Return head**

### Code

**Approach 1: Using Arrays**
```python
# Definition of singly linked list
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def oddEvenList(self, head):
        odd, even = [], []
        cnt = 0
        current = head
        
        # Separate values into odd and even positioned arrays
        while current:
            if cnt % 2 == 0:
                even.append(current.val)
            else:
                odd.append(current.val)
            current = current.next
            cnt += 1
        
        cnt = 0
        n1, n2 = len(even), len(odd)
        current = head
        
        # Refill the linked list: even positioned first, then odd
        while current:
            if cnt < n1:
                current.val = even[cnt]
            else:
                current.val = odd[cnt - n1]
            current = current.next
            cnt += 1
        
        return head
```

**Approach 2: Pointer Rearrangement (Optimal)**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def oddEvenList(self, head):
        if not head or not head.next:
            return head

        # Initialize pointers for odd and even nodes
        # Keep track of the first even node
        odd = head
        even = head.next
        firstEven = head.next

        # Rearranging nodes by adjusting pointers
        while even and even.next:
            # Connect current odd to next odd
            odd.next = odd.next.next
            # Connect current even to next even
            even.next = even.next.next
            # Move pointers forward
            odd = odd.next
            even = even.next

        # Connect the last odd node to the first even node
        odd.next = firstEven

        return head
```

### Time and Space Complexity

**Approach 1 (Arrays)**:
- **Time Complexity**: O(n)
  - First traversal: O(n) to separate values
  - Second traversal: O(n) to refill values
  - Total: O(n) where n is the number of nodes
- **Space Complexity**: O(n)
  - We use two arrays that together store all n values from the linked list

**Approach 2 (Pointer Rearrangement - Optimal)**:
- **Time Complexity**: O(n)
  - Single traversal through the list
- **Space Complexity**: O(1)
  - Only using a few pointers, no extra space for data
  - True in-place rearrangement

---

## 3. Sort a Linked List of 0's, 1's and 2's

### Problem Description
Given a linked list containing only 0s, 1s, and 2s, sort the list in ascending order. You must do this **in-place** by changing the links between nodes, not by creating new nodes.

Think of it like sorting colored balls (0=red, 1=white, 2=blue) where you want all reds first, then whites, then blues.

### Examples

#### Input
```
head -> 1 -> 0 -> 2 -> 0 -> 1
```

#### Output
```
head -> 0 -> 0 -> 1 -> 1 -> 2
```

#### Explanation
After sorting: [0, 0, 1, 1, 2]

---

#### Input
```
head -> 1 -> 1 -> 1 -> 0
```

#### Output
```
head -> 0 -> 1 -> 1 -> 1
```

#### Explanation
After sorting: [0, 1, 1, 1]

### Solution

**Approach 1 (Counting)**: Count the number of 0s, 1s, and 2s. Then traverse the list again and fill it with the appropriate counts of each value.

**Approach 2 (Three Pointers - Optimal)**: Create three separate dummy lists for 0s, 1s, and 2s. Traverse the original list and attach each node to the appropriate list. Finally, connect the three lists together.

### Intuition
This is similar to the Dutch National Flag problem. Since we only have three distinct values, we can:
- **Method 1**: Count how many of each value we have, then reconstruct the list
- **Method 2**: Separate nodes into three buckets based on their value, then join the buckets

Method 2 is better because it actually rearranges links (true in-place) rather than just changing values.

### Approach Steps

**Approach 1 (Counting)**:
1. Count occurrences of 0s, 1s, and 2s in the list
2. Traverse the list again
3. Fill first `cnt0` nodes with 0, next `cnt1` nodes with 1, remaining nodes with 2
4. Return head

**Approach 2 (Three Pointers - Optimal)**:
1. Create three dummy nodes: `zero`, `one`, `two`
2. Keep pointers to the start of each list: `first0`, `first1`, `first2`
3. Traverse the original list:
   - Attach nodes with value 0 to the `zero` list
   - Attach nodes with value 1 to the `one` list
   - Attach nodes with value 2 to the `two` list
4. Connect the three lists: `zero -> one -> two`
5. Handle edge case: if no 1s exist, connect `zero` directly to `two`
6. Set `two.next = None` to terminate the list
7. Return `first0.next`

### Code

**Approach 1: Counting Method**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def sortList(self, head):
        temp = head
        cnt0, cnt1, cnt2 = 0, 0, 0
        
        # Count occurrences
        while temp:
            if temp.val == 0:
                cnt0 += 1
            if temp.val == 1:
                cnt1 += 1
            if temp.val == 2:
                cnt2 += 1
            temp = temp.next
        
        temp = head
        cnt = 1
        
        # Fill the list with sorted values
        while temp:
            if cnt <= cnt0:
                temp.val = 0
            elif cnt > cnt0 and cnt <= (cnt0 + cnt1):
                temp.val = 1
            else:
                temp.val = 2
            temp = temp.next
            cnt += 1
        
        return head
```

**Approach 2: Three Pointers (Optimal)**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def sortList(self, head):
        # Create three dummy lists
        zero = ListNode(-1)
        one = ListNode(-1)
        two = ListNode(-1)
        
        temp = head
        first0 = zero
        first1 = one
        first2 = two
        
        # Distribute nodes into three lists
        while temp:
            if temp.val == 0:
                zero.next = temp
                zero = zero.next
            if temp.val == 1:
                one.next = temp
                one = one.next
            if temp.val == 2:
                two.next = temp
                two = two.next
            temp = temp.next
        
        # Connect the three lists
        if first1.next:
            zero.next = first1.next
            one.next = first2.next
        else:
            zero.next = first2.next
        
        two.next = None  # Terminate the list
        
        return first0.next
```

### Time and Space Complexity

**Approach 1 (Counting)**:
- **Time Complexity**: O(n) - Two passes through the list
- **Space Complexity**: O(1) - Only using counters

**Approach 2 (Three Pointers)**:
- **Time Complexity**: O(n) - Single pass through the list
- **Space Complexity**: O(1) - Only using pointers, no extra space for data

---

## 4. Remove Nth Node from the Back

### Problem Description
Given a linked list and an integer n, remove the nth node from the end of the list and return the head of the modified list. The value of n will always be valid (less than or equal to the total number of nodes).

For example, if you have 5 nodes and n=2, you need to remove the 2nd node from the end (which is the 4th node from the beginning).

### Examples

#### Input
```
head -> 1 -> 2 -> 3 -> 4 -> 5, n = 2
```

#### Output
```
head -> 1 -> 2 -> 3 -> 5
```

#### Explanation
The 2nd node from the back is 4, so we remove it.

---

#### Input
```
head -> 5 -> 4 -> 3 -> 2 -> 1, n = 5
```

#### Output
```
head -> 4 -> 3 -> 2 -> 1
```

#### Explanation
The 5th node from the back is the first node (5), so we remove it.

### Solution

**Approach 1**: Calculate the length of the list, then find which position from the start needs to be removed, and remove it.

**Approach 2 (Two Pointer/Fast-Slow)**: Use two pointers with a gap of n nodes between them. When the fast pointer reaches the end, the slow pointer will be just before the node to be removed.

### Intuition

**Approach 1**: If a list has 5 nodes and we want to remove the 2nd from the end, that's the 4th from the start (5-2+1=4). So we can convert the problem from "nth from end" to "kth from start".

**Approach 2**: Imagine two people walking in a line with n people between them. When the front person reaches the end, the back person is exactly n positions from the end! This is the clever two-pointer technique.

### Approach Steps

**Approach 1 (Length Calculation)**:
1. Calculate the length of the linked list
2. Convert nth from end to kth from start: `k = length - n + 1`
3. Handle edge case: if k=1 (removing head), return `head.next`
4. Traverse to the (k-1)th node
5. Remove the kth node by adjusting pointers: `node.next = node.next.next`
6. Return head

**Approach 2 (Two Pointer - Optimal)**:
1. Initialize two pointers: `fast` and `slow`, both at head
2. Move `fast` pointer n steps ahead
3. Edge case: if `fast` is None, we're removing the head, return `head.next`
4. Move both pointers together until `fast.next` is None
5. Now `slow` is just before the node to remove
6. Remove the node: `slow.next = slow.next.next`
7. Return head

### Visual Example: Two Pointer Approach

Let's remove the 2nd node from end in list: `1 -> 2 -> 3 -> 4 -> 5`

**Step 1: Move fast pointer n=2 steps ahead**
```
     slow
       ↓
   1 → 2 → 3 → 4 → 5
           ↑
          fast
```

**Step 2: Move both pointers until fast.next is None**
```
             slow
               ↓
   1 → 2 → 3 → 4 → 5
                   ↑
                  fast
```

**Step 3: Remove node (slow.next = slow.next.next)**
```
             slow
               ↓
   1 → 2 → 3 → 4   5
               ↓___↗
Result: 1 → 2 → 3 → 5
```

The gap between slow and fast is always n nodes, so when fast reaches the end, slow is right before the node to delete!

### Code

**Approach 1: Length Calculation**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def removeNthFromEnd(self, head, n):
        temp = head
        length = 0
        
        # Calculate length
        while temp:
            length += 1
            temp = temp.next
        
        # Find position from start
        k = length - n + 1
        
        # Edge case: removing head
        if k == 1:
            return head.next
        
        cnt = 0
        temp = head
        
        # Traverse to (k-1)th node and remove kth node
        while temp:
            cnt += 1
            if cnt == k - 1:
                temp.next = temp.next.next
                break
            temp = temp.next
        
        return head
```

**Approach 2: Two Pointer (Optimal)**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def removeNthFromEnd(self, head, n):
        fast = head
        slow = head
        
        # Move fast pointer n steps ahead
        for _ in range(n):
            fast = fast.next
        
        # Edge case: removing head
        if fast is None:
            return head.next
        
        # Move both pointers until fast reaches the end
        while fast.next:
            fast = fast.next
            slow = slow.next
        
        # Remove the node
        slow.next = slow.next.next
        
        return head
```

### Time and Space Complexity

**Approach 1**:
- **Time Complexity**: O(n) - Two passes: one for length, one for removal
- **Space Complexity**: O(1) - Only using pointers

**Approach 2 (Optimal)**:
- **Time Complexity**: O(n) - Single pass through the list
- **Space Complexity**: O(1) - Only using two pointers

---

## 5. Reverse a Linked List

### Problem Description
Given the head of a singly linked list, reverse the entire list and return the new head. After reversal, the last node becomes the first node, the second-last becomes the second, and so on.

Think of it like reversing a train: the last car becomes the engine, and all connections between cars are reversed.

### Examples

#### Input
```
head -> 1 -> 2 -> 3 -> 4 -> 5
```

#### Output
```
head -> 5 -> 4 -> 3 -> 2 -> 1
```

#### Explanation
All links are reversed: 1 <- 2 <- 3 <- 4 <- 5 <- head

---

#### Input
```
head -> 6 -> 8
```

#### Output
```
head -> 8 -> 6
```

#### Explanation
Simple reversal: 6 <- 8 <- head

### Solution

**Approach 1 (Stack)**: Use a stack to store all node values, then pop them back to reverse the order and update the list.

**Approach 2 (Iterative - Three Pointers)**: Use three pointers to reverse links iteratively as we traverse.

**Approach 3 (Recursive)**: Use recursion to reverse links while unwinding the call stack.

### Intuition

**Stack Approach**: A stack follows LIFO (Last In First Out). If we push all nodes onto a stack, they come out in reverse order. We can then use this reversed order to update node values or rebuild the list.

**Iterative Approach**: Imagine flipping arrows one by one. We need to remember three things at each step:
- Current node we're working on
- Previous node (to point current node back to it)
- Next node (so we don't lose the rest of the list)

**Recursive Approach**: The recursion goes to the end of the list first, then while coming back, it reverses each link. It's like going to the end of a tunnel, then turning everyone around on the way back.

### Approach Steps

**Stack Approach**:
1. Create an empty stack
2. Traverse the list and push all nodes onto the stack
3. Reset pointer to head
4. Pop nodes from stack and update current node's value
5. Move to next node
6. Return head

**Iterative (Three Pointer)**:
1. Initialize `temp` at head (current node)
2. Initialize `prev` as None (previous node)
3. While `temp` is not None:
   - Store `temp.next` in `front` (to preserve the link)
   - Reverse the link: `temp.next = prev`
   - Move `prev` to `temp`
   - Move `temp` to `front`
4. Return `prev` (new head)

**Recursive**:
1. Base case: if node is None, return `prev`
2. Recursively call function with `node.next` and current `node`
3. On returning, reverse the link: `node.next = prev`
4. Return the new head from base case

### Code

**Approach 1: Using Stack**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head):
        # Handle empty list
        if not head:
            return None
        
        # Push all nodes onto stack
        stack = []
        temp = head
        while temp:
            stack.append(temp)
            temp = temp.next
        
        # Pop nodes and rebuild connections
        temp = head
        while stack:
            node = stack.pop()
            temp.val = node.val
            temp = temp.next
        
        return head
```

**Approach 2: Iterative (Three Pointers) - Optimal**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head):
        # Initialize temp at head
        temp = head
        
        # Initialize prev as None
        prev = None
        
        # Traverse the list
        while temp:
            # Store next node
            front = temp.next
            
            # Reverse the link
            temp.next = prev
            
            # Move prev to current node
            prev = temp
            
            # Move to next node
            temp = front
        
        # Return new head
        return prev
```

**Approach 3: Recursive**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head):
        # Store the new head in a list to maintain reference
        ans = [-1]
        
        def recurse(node, prev):
            # Base case: reached the end
            if not node:
                ans[0] = prev
                return
            
            # Recurse to the end
            recurse(node.next, node)
            
            # Reverse the link while unwinding
            node.next = prev
        
        recurse(head, None)
        return ans[0]
```

### Time and Space Complexity

**Stack Approach**:
- **Time Complexity**: O(n) - Two passes: one to push, one to pop
- **Space Complexity**: O(n) - Stack stores all n nodes

**Iterative Approach (Optimal for Space)**:
- **Time Complexity**: O(n) - Single pass through all n nodes
- **Space Complexity**: O(1) - Only using three pointers

**Recursive Approach**:
- **Time Complexity**: O(n) - Visit each node once
- **Space Complexity**: O(n) - Recursion call stack uses n space

---

## Summary Table

| Problem | Best Approach | Time | Space |
|---------|--------------|------|-------|
| Add Two Numbers | Single Loop | O(max(m,n)) | O(max(m,n)) |
| Segregate Odd/Even | Pointer Rearrangement | O(n) | O(1) |
| Sort 0s 1s 2s | Three Pointers | O(n) | O(1) |
| Remove Nth from End | Two Pointer (Fast-Slow) | O(n) | O(1) |
| Reverse List | Iterative (3 pointers) | O(n) | O(1) |

---

**Happy Learning! 🚀**
