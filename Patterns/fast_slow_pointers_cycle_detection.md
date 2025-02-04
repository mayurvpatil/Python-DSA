
---

## **Easy Problems**

---

### **1. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)**
**Problem**: Detect if a linked list has a cycle.  
**Example**:  
**Input**: `head = [3,2,0,-4], pos = 1` → **Output**: `True`.

#### **Brute Force**
**Approach**: Use a hash set to track visited nodes.  
**Time**: O(n), **Space**: O(n)  
```python
def hasCycle_brute(head):
    visited = set()
    while head:
        if head in visited:
            return True
        visited.add(head)
        head = head.next
    return False
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Use two pointers, one moving twice as fast as the other.  
**Time**: O(n), **Space**: O(1)  
```python
def hasCycle_optimized(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

---

### **2. [Happy Number](https://leetcode.com/problems/happy-number/)**
**Problem**: Determine if a number is happy (reaches 1 by summing squares of digits).  
**Example**:  
**Input**: `19` → **Output**: `True` (1² + 9² = 82 → 8² + 2² = 68 → 6² + 8² = 100 → 1² + 0² + 0² = 1).

#### **Brute Force**
**Approach**: Use a hash set to detect cycles.  
**Time**: O(n), **Space**: O(n)  
```python
def isHappy_brute(n: int) -> bool:
    def get_next(num):
        return sum(int(digit) ** 2 for digit in str(num))
    visited = set()
    while n != 1 and n not in visited:
        visited.add(n)
        n = get_next(n)
    return n == 1
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Use two pointers to detect cycles.  
**Time**: O(n), **Space**: O(1)  
```python
def isHappy_optimized(n: int) -> bool:
    def get_next(num):
        return sum(int(digit) ** 2 for digit in str(num))
    slow = fast = n
    while fast != 1:
        slow = get_next(slow)
        fast = get_next(get_next(fast))
        if slow == fast and fast != 1:
            return False
    return True
```

---

### **3. [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)**
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

## **Medium Problems**

---

### **4. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)**
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

### **5. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)**
**Problem**: Find the duplicate number in an array of integers.  
**Example**:  
**Input**: `nums = [1,3,4,2,2]` → **Output**: `2`.

#### **Brute Force**
**Approach**: Use a hash set to detect duplicates.  
**Time**: O(n), **Space**: O(n)  
```python
def findDuplicate_brute(nums: list) -> int:
    seen = set()
    for num in nums:
        if num in seen:
            return num
        seen.add(num)
    return -1
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Treat the array as a linked list and detect the cycle.  
**Time**: O(n), **Space**: O(1)  
```python
def findDuplicate_optimized(nums: list) -> int:
    slow = fast = nums[0]
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    return slow
```

---

### **6. [Reorder List](https://leetcode.com/problems/reorder-list/)**
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

## **Hard Problems**

---

### **7. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)**
**Problem**: Find the duplicate number in an array of integers (revisited).  
**Example**:  
**Input**: `nums = [1,3,4,2,2]` → **Output**: `2`.

#### **Brute Force**
**Approach**: Use a hash set to detect duplicates.  
**Time**: O(n), **Space**: O(n)  
```python
def findDuplicate_brute(nums: list) -> int:
    seen = set()
    for num in nums:
        if num in seen:
            return num
        seen.add(num)
    return -1
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Treat the array as a linked list and detect the cycle.  
**Time**: O(n), **Space**: O(1)  
```python
def findDuplicate_optimized(nums: list) -> int:
    slow = fast = nums[0]
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    return slow
```

---

### **8. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)**
**Problem**: Find the node where the cycle begins in a linked list (revisited).  
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

### **9. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)**
**Problem**: Find the duplicate number in an array of integers (revisited).  
**Example**:  
**Input**: `nums = [1,3,4,2,2]` → **Output**: `2`.

#### **Brute Force**
**Approach**: Use a hash set to detect duplicates.  
**Time**: O(n), **Space**: O(n)  
```python
def findDuplicate_brute(nums: list) -> int:
    seen = set()
    for num in nums:
        if num in seen:
            return num
        seen.add(num)
    return -1
```

#### **Optimized (Fast & Slow Pointers)**
**Approach**: Treat the array as a linked list and detect the cycle.  
**Time**: O(n), **Space**: O(1)  
```python
def findDuplicate_optimized(nums: list) -> int:
    slow = fast = nums[0]
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    return slow
```

---

### **Key Takeaways**:
1. **Brute Force** approaches are straightforward but inefficient (O(n) or worse).  
2. **Optimized Fast & Slow Pointers** methods reduce time to O(n) and space to O(1) by:  
   - Detecting cycles in linked lists or arrays.  
   - Finding the starting point of cycles.  
3. **Hard Problems** often require combining cycle detection with other techniques like reversing linked lists or merging arrays.  

