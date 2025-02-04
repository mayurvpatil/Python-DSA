
---

### **Easy Problems**

#### 1. [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
**Problem**: Reverse a portion of the linked list from position `left` to `right`.

**Approach**:
1. Traverse to the `left-1` node.
2. Reverse the sublist from `left` to `right`.
3. Reconnect the reversed sublist back to the original list.

**Brute Force (Iterative)**:
```python
def reverseBetween(head, left, right):
    if not head or left == right:
        return head
    
    dummy = ListNode(0)
    dummy.next = head
    prev = dummy
    
    # Move to the node before the left position
    for _ in range(left - 1):
        prev = prev.next
    
    # Reverse the sublist
    curr = prev.next
    for _ in range(right - left):
        next_node = curr.next
        curr.next = next_node.next
        next_node.next = prev.next
        prev.next = next_node
    
    return dummy.next
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(1), as we are using constant extra space.

---

#### 2. [Reverse Sublist of Linked List](https://leetcode.com/problems/reverse-linked-list-ii/) (Same as above)
**Problem**: Reverse a sublist of a linked list.

**Approach**:
1. Traverse to the start of the sublist.
2. Reverse the sublist.
3. Reconnect the reversed sublist to the original list.

**Optimized (Iterative)**:
```python
def reverseBetween(head, left, right):
    if not head or left == right:
        return head
    
    dummy = ListNode(0)
    dummy.next = head
    prev = dummy
    
    # Move to the node before the left position
    for _ in range(left - 1):
        prev = prev.next
    
    # Reverse the sublist
    curr = prev.next
    for _ in range(right - left):
        next_node = curr.next
        curr.next = next_node.next
        next_node.next = prev.next
        prev.next = next_node
    
    return dummy.next
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(1), as we are using constant extra space.

---

#### 3. [Reverse Nodes in Even Length Groups](https://leetcode.com/problems/reverse-nodes-in-even-length-groups/)
**Problem**: Reverse nodes in even-length groups of a linked list.

**Approach**:
1. Traverse the list in groups of increasing size.
2. Reverse groups with even length.

**Brute Force (Iterative)**:
```python
def reverseEvenLengthGroups(head):
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    group_size = 1
    
    while prev_group_end.next:
        group_start = prev_group_end.next
        curr = group_start
        count = 0
        
        # Traverse the group
        while curr and count < group_size:
            curr = curr.next
            count += 1
        
        # Reverse if the group has even length
        if count % 2 == 0:
            prev = None
            curr = group_start
            for _ in range(count):
                next_node = curr.next
                curr.next = prev
                prev = curr
                curr = next_node
            prev_group_end.next = prev
            group_start.next = curr
        else:
            prev_group_end = curr if curr else group_start
        
        group_size += 1
    
    return dummy.next
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(1), as we are using constant extra space.

---

### **Medium Problems**

#### 4. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)
**Problem**: Reverse nodes of a linked list `k` at a time.

**Approach**:
1. Traverse the list in groups of `k`.
2. Reverse each group and reconnect it to the main list.

**Brute Force (Iterative)**:
```python
def reverseKGroup(head, k):
    def reverse(head, k):
        prev = None
        curr = head
        for _ in range(k):
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
        return prev, head
    
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    
    while True:
        # Check if there are k nodes left
        kth_node = prev_group_end
        for _ in range(k):
            kth_node = kth_node.next
            if not kth_node:
                return dummy.next
        
        # Reverse the group
        group_start = prev_group_end.next
        group_end = kth_node
        next_group_start = group_end.next
        group_end.next = None
        reversed_head, reversed_tail = reverse(group_start, k)
        
        # Reconnect the reversed group
        prev_group_end.next = reversed_head
        reversed_tail.next = next_group_start
        prev_group_end = reversed_tail
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(1), as we are using constant extra space.

---

#### 5. [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)
**Problem**: Swap every two adjacent nodes in a linked list.

**Approach**:
1. Use a dummy node to handle edge cases.
2. Swap pairs iteratively.

**Brute Force (Iterative)**:
```python
def swapPairs(head):
    dummy = ListNode(0)
    dummy.next = head
    prev = dummy
    
    while prev.next and prev.next.next:
        first = prev.next
        second = first.next
        
        # Swap nodes
        first.next = second.next
        second.next = first
        prev.next = second
        
        # Move to the next pair
        prev = first
    
    return dummy.next
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(1), as we are using constant extra space.

---

#### 6. [Reorder List](https://leetcode.com/problems/reorder-list/)
**Problem**: Reorder a linked list in the form `L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...`.

**Approach**:
1. Find the middle of the list.
2. Reverse the second half.
3. Merge the two halves alternately.

**Brute Force (Using Reversal)**:
```python
def reorderList(head):
    if not head or not head.next:
        return
    
    # Find the middle of the list
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    # Reverse the second half
    prev = None
    curr = slow
    while curr:
        next_node = curr.next
        curr.next = prev
        prev = curr
        curr = next_node
    
    # Merge the two halves
    first, second = head, prev
    while second.next:
        first.next, first = second, first.next
        second.next, second = first, second.next
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(1), as we are using constant extra space.

---

### **Hard Problems**

#### 7. [Reverse Nodes in Even Length Groups](https://leetcode.com/problems/reverse-nodes-in-even-length-groups/)
**Problem**: Reverse nodes in even-length groups of a linked list.

**Approach**:
1. Traverse the list in groups of increasing size.
2. Reverse groups with even length.

**Brute Force (Iterative)**:
```python
def reverseEvenLengthGroups(head):
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    group_size = 1
    
    while prev_group_end.next:
        group_start = prev_group_end.next
        curr = group_start
        count = 0
        
        # Traverse the group
        while curr and count < group_size:
            curr = curr.next
            count += 1
        
        # Reverse if the group has even length
        if count % 2 == 0:
            prev = None
            curr = group_start
            for _ in range(count):
                next_node = curr.next
                curr.next = prev
                prev = curr
                curr = next_node
            prev_group_end.next = prev
            group_start.next = curr
        else:
            prev_group_end = curr if curr else group_start
        
        group_size += 1
    
    return dummy.next
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(1), as we are using constant extra space.

---

#### 8. [Reverse Nodes in Alternating Groups](https://leetcode.com/problems/reverse-nodes-in-alternating-groups/)
**Problem**: Reverse nodes in alternating groups of a linked list.

**Approach**:
1. Traverse the list in groups of increasing size.
2. Reverse alternate groups.

**Brute Force (Iterative)**:
```python
def reverseAlternateGroups(head):
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    group_size = 1
    reverse = True
    
    while prev_group_end.next:
        group_start = prev_group_end.next
        curr = group_start
        count = 0
        
        # Traverse the group
        while curr and count < group_size:
            curr = curr.next
            count += 1
        
        # Reverse alternate groups
        if reverse:
            prev = None
            curr = group_start
            for _ in range(count):
                next_node = curr.next
                curr.next = prev
                prev = curr
                curr = next_node
            prev_group_end.next = prev
            group_start.next = curr
        else:
            prev_group_end = curr if curr else group_start
        
        reverse = not reverse
        group_size += 1
    
    return dummy.next
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(1), as we are using constant extra space.

---

#### 9. [Reverse Nodes in Maximum Groups](https://leetcode.com/problems/reverse-nodes-in-maximum-groups/)
**Problem**: Reverse nodes in maximum-length groups of a linked list.

**Approach**:
1. Traverse the list in groups of maximum possible size.
2. Reverse each group.

**Brute Force (Iterative)**:
```python
def reverseMaxGroups(head):
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    
    while prev_group_end.next:
        group_start = prev_group_end.next
        curr = group_start
        count = 0
        
        # Traverse the group
        while curr:
            curr = curr.next
            count += 1
        
        # Reverse the group
        prev = None
        curr = group_start
        for _ in range(count):
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
        prev_group_end.next = prev
        group_start.next = curr
        prev_group_end = group_start
    
    return dummy.next
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(1), as we are using constant extra space.

---

### **Summary of Python Implementations**
1. **Iterative Approach**: Uses pointers to reverse the sublist in-place. Preferred for its O(1) space complexity.
2. **Group Reversal**: Extends the iterative approach to reverse groups of nodes. Useful for problems like `k-Group Reversal` or `Reorder List`.

