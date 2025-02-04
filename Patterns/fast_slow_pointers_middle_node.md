
---

## **Easy Problems**

---

### **1. [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)**
**Problem**: Find the middle node of a linked list.  
**Example**:  
**Input**: `head = [1,2,3,4,5]` → **Output**: `[3,4,5]`.

#### **Brute Force**
**Approach**: Traverse the list to find its length, then traverse again to the middle.  
**Time**: O(n), **Space**: O(1)  
```python
def middleNode_brute(head):
    length = 0
    current = head
    while current:
        length += 1
        current = current.next
    middle = length // 2
    current = head
    for _ in range(middle):
        current = current.next
    return current
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Use two pointers, one moving twice as fast as the other.  
**Time**: O(n), **Space**: O(1)  
```python
def middleNode_optimized(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

---

### **2. [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)**
**Problem**: Check if a linked list is a palindrome.  
**Example**:  
**Input**: `head = [1,2,2,1]` → **Output**: `True`.

#### **Brute Force**
**Approach**: Copy the linked list to an array and check if it's a palindrome.  
**Time**: O(n), **Space**: O(n)  
```python
def isPalindrome_brute(head):
    values = []
    while head:
        values.append(head.val)
        head = head.next
    return values == values[::-1]
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Use fast and slow pointers to find the middle and reverse the second half.  
**Time**: O(n), **Space**: O(1)  
```python
def isPalindrome_optimized(head):
    # Find the middle
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    # Reverse the second half
    prev = None
    while slow:
        next_node = slow.next
        slow.next = prev
        prev = slow
        slow = next_node
    # Compare the two halves
    left, right = head, prev
    while right:
        if left.val != right.val:
            return False
        left = left.next
        right = right.next
    return True
```

---

### **3. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)**
**Problem**: Remove the nth node from the end of a linked list.  
**Example**:  
**Input**: `head = [1,2,3,4,5], n = 2` → **Output**: `[1,2,3,5]`.

#### **Brute Force**
**Approach**: Traverse the list to find its length, then remove the nth node from the end.  
**Time**: O(n), **Space**: O(1)  
```python
def removeNthFromEnd_brute(head, n: int):
    length = 0
    current = head
    while current:
        length += 1
        current = current.next
    if length == n:
        return head.next
    current = head
    for _ in range(length - n - 1):
        current = current.next
    current.next = current.next.next
    return head
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Use two pointers to maintain a gap of `n` nodes.  
**Time**: O(n), **Space**: O(1)  
```python
def removeNthFromEnd_optimized(head, n: int):
    dummy = ListNode(0)
    dummy.next = head
    slow = fast = dummy
    for _ in range(n + 1):
        fast = fast.next
    while fast:
        slow = slow.next
        fast = fast.next
    slow.next = slow.next.next
    return dummy.next
```

---

## **Medium Problems**

---

### **4. [Reorder List](https://leetcode.com/problems/reorder-list/)**
**Problem**: Reorder a linked list in-place (e.g., `1->2->3->4` becomes `1->4->2->3`).  
**Example**:  
**Input**: `head = [1,2,3,4]` → **Output**: `[1,4,2,3]`.

#### **Brute Force**
**Approach**: Copy the linked list to an array and reorder it.  
**Time**: O(n), **Space**: O(n)  
```python
def reorderList_brute(head):
    nodes = []
    while head:
        nodes.append(head)
        head = head.next
    left, right = 0, len(nodes) - 1
    while left < right:
        nodes[left].next = nodes[right]
        left += 1
        if left == right:
            break
        nodes[right].next = nodes[left]
        right -= 1
    nodes[left].next = None
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Use fast and slow pointers to find the middle, reverse the second half, and merge.  
**Time**: O(n), **Space**: O(1)  
```python
def reorderList_optimized(head):
    # Find the middle
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    # Reverse the second half
    prev = None
    while slow:
        next_node = slow.next
        slow.next = prev
        prev = slow
        slow = next_node
    # Merge the two halves
    first, second = head, prev
    while second.next:
        first.next, first = second, first.next
        second.next, second = first, second.next
```

---

### **5. [Sort List](https://leetcode.com/problems/sort-list/)**
**Problem**: Sort a linked list in ascending order.  
**Example**:  
**Input**: `head = [4,2,1,3]` → **Output**: `[1,2,3,4]`.

#### **Brute Force**
**Approach**: Copy the linked list to an array, sort it, and rebuild the list.  
**Time**: O(n log n), **Space**: O(n)  
```python
def sortList_brute(head):
    values = []
    while head:
        values.append(head.val)
        head = head.next
    values.sort()
    dummy = ListNode(0)
    current = dummy
    for val in values:
        current.next = ListNode(val)
        current = current.next
    return dummy.next
```

#### **Optimized (Fast & Slow Pointers + Merge Sort)**
**Approach**: Use fast and slow pointers to split the list into halves and merge sort.  
**Time**: O(n log n), **Space**: O(log n) (recursion stack)  
```python
def sortList_optimized(head):
    if not head or not head.next:
        return head
    # Find the middle
    slow = fast = head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next
    # Split the list
    mid = slow.next
    slow.next = None
    # Recursively sort both halves
    left = sortList_optimized(head)
    right = sortList_optimized(mid)
    # Merge the two halves
    dummy = ListNode(0)
    current = dummy
    while left and right:
        if left.val < right.val:
            current.next = left
            left = left.next
        else:
            current.next = right
            right = right.next
        current = current.next
    current.next = left or right
    return dummy.next
```

---

### **6. [Partition List](https://leetcode.com/problems/partition-list/)**
**Problem**: Partition a linked list around a value `x`.  
**Example**:  
**Input**: `head = [1,4,3,2,5,2], x = 3` → **Output**: `[1,2,2,4,3,5]`.

#### **Brute Force**
**Approach**: Create two lists for nodes less than `x` and nodes greater than or equal to `x`, then merge them.  
**Time**: O(n), **Space**: O(n)  
```python
def partition_brute(head, x: int):
    less = []
    greater = []
    current = head
    while current:
        if current.val < x:
            less.append(current.val)
        else:
            greater.append(current.val)
        current = current.next
    dummy = ListNode(0)
    current = dummy
    for val in less + greater:
        current.next = ListNode(val)
        current = current.next
    return dummy.next
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Use two pointers to partition the list in-place.  
**Time**: O(n), **Space**: O(1)  
```python
def partition_optimized(head, x: int):
    less = less_head = ListNode(0)
    greater = greater_head = ListNode(0)
    while head:
        if head.val < x:
            less.next = head
            less = less.next
        else:
            greater.next = head
            greater = greater.next
        head = head.next
    greater.next = None
    less.next = greater_head.next
    return less_head.next
```

---

## **Hard Problems**

---

### **7. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)**
**Problem**: Merge `k` sorted linked lists into one sorted list.  
**Example**:  
**Input**: `lists = [[1,4,5],[1,3,4],[2,6]]` → **Output**: `[1,1,2,3,4,4,5,6]`.

#### **Brute Force**
**Approach**: Collect all nodes, sort them, and rebuild the list.  
**Time**: O(n log n), **Space**: O(n)  
```python
def mergeKLists_brute(lists):
    nodes = []
    for l in lists:
        while l:
            nodes.append(l.val)
            l = l.next
    nodes.sort()
    dummy = ListNode(0)
    current = dummy
    for val in nodes:
        current.next = ListNode(val)
        current = current.next
    return dummy.next
```

#### **Optimized (Divide and Conquer + Fast & Slow Pointers)**
**Approach**: Use a divide-and-conquer approach to merge pairs of lists.  
**Time**: O(n log k), **Space**: O(1)  
```python
def mergeKLists_optimized(lists):
    def mergeTwoLists(l1, l2):
        dummy = ListNode(0)
        current = dummy
        while l1 and l2:
            if l1.val < l2.val:
                current.next = l1
                l1 = l1.next
            else:
                current.next = l2
                l2 = l2.next
            current = current.next
        current.next = l1 or l2
        return dummy.next

    if not lists:
        return None
    interval = 1
    while interval < len(lists):
        for i in range(0, len(lists) - interval, interval * 2):
            lists[i] = mergeTwoLists(lists[i], lists[i + interval])
        interval *= 2
    return lists[0]
```

---

### **8. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)**
**Problem**: Reverse nodes in groups of `k` in a linked list.  
**Example**:  
**Input**: `head = [1,2,3,4,5], k = 2` → **Output**: `[2,1,4,3,5]`.

#### **Brute Force**
**Approach**: Traverse the list and reverse each group of `k` nodes.  
**Time**: O(n), **Space**: O(1)  
```python
def reverseKGroup_brute(head, k: int):
    def reverse(head, k):
        prev = None
        current = head
        for _ in range(k):
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
        return prev, current

    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    while True:
        group_start = prev_group_end.next
        count = 0
        current = group_start
        while current and count < k:
            current = current.next
            count += 1
        if count < k:
            break
        reversed_head, next_group_start = reverse(group_start, k)
        prev_group_end.next = reversed_head
        group_start.next = next_group_start
        prev_group_end = group_start
    return dummy.next
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Use two pointers to reverse each group of `k` nodes.  
**Time**: O(n), **Space**: O(1)  
```python
def reverseKGroup_optimized(head, k: int):
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    while True:
        group_start = prev_group_end.next
        count = 0
        current = group_start
        while current and count < k:
            current = current.next
            count += 1
        if count < k:
            break
        prev, current = None, group_start
        for _ in range(k):
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
        prev_group_end.next = prev
        group_start.next = current
        prev_group_end = group_start
    return dummy.next
```

---

### **9. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)**
**Problem**: Find the node where the cycle begins in a linked list.  
**Example**:  
**Input**: `head = [3,2,0,-4], pos = 1` → **Output**: `1` (Node with value 2).

#### **Brute Force**
**Approach**: Use a hash set to track visited nodes.  
**Time**: O(n), **Space**: O(n)  
```python
def detectCycle_brute(head):
    visited = set()
    while head:
        if head in visited:
            return head
        visited.add(head)
        head = head.next
    return None
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Use two pointers to detect the cycle and find the starting node.  
**Time**: O(n), **Space**: O(1)  
```python
def detectCycle_optimized(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    else:
        return None  # No cycle
    # Find the start of the cycle
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    return slow
```

---

### **Key Takeaways**:
1. **Brute Force** approaches are straightforward but inefficient (O(n) or worse).  
2. **Optimized Fast & Slow Pointers** methods reduce time to O(n) and space to O(1) by:  
   - Finding the middle of a linked list.  
   - Reversing or partitioning linked lists.  
3. **Hard Problems** often require combining fast and slow pointers with other techniques like merge sort or cycle detection.  

