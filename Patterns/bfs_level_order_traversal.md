
---

### **Easy Problems**

#### 1. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
**Problem**: Perform level order traversal of a binary tree.

**Approach**:
1. Use a queue to process nodes level by level.
2. For each level, store the nodes in a list.

**Brute Force (Iterative)**:
```python
from collections import deque

def levelOrder(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        current_level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            current_level.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(current_level)
    
    return result
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(n), as the queue can store up to `n` nodes.

---

#### 2. [Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)
**Problem**: Find the average value of nodes at each level of a binary tree.

**Approach**:
1. Perform level order traversal.
2. Calculate the average of nodes at each level.

**Brute Force (Iterative)**:
```python
from collections import deque

def averageOfLevels(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        level_sum = 0
        
        for _ in range(level_size):
            node = queue.popleft()
            level_sum += node.val
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level_sum / level_size)
    
    return result
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(n), as the queue can store up to `n` nodes.

---

#### 3. [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
**Problem**: Find the minimum depth of a binary tree.

**Approach**:
1. Perform level order traversal.
2. Return the depth when the first leaf node is encountered.

**Brute Force (Iterative)**:
```python
from collections import deque

def minDepth(root):
    if not root:
        return 0
    
    queue = deque([(root, 1)])  # (node, depth)
    
    while queue:
        node, depth = queue.popleft()
        
        # Check if it's a leaf node
        if not node.left and not node.right:
            return depth
        
        if node.left:
            queue.append((node.left, depth + 1))
        if node.right:
            queue.append((node.right, depth + 1))
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(n), as the queue can store up to `n` nodes.

---

### **Medium Problems**

#### 4. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
**Problem**: Perform zigzag level order traversal of a binary tree.

**Approach**:
1. Use a queue to process nodes level by level.
2. Use a flag to alternate the direction of traversal.

**Brute Force (Iterative)**:
```python
from collections import deque

def zigzagLevelOrder(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    left_to_right = True
    
    while queue:
        level_size = len(queue)
        current_level = deque()
        
        for _ in range(level_size):
            node = queue.popleft()
            
            if left_to_right:
                current_level.append(node.val)
            else:
                current_level.appendleft(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(list(current_level))
        left_to_right = not left_to_right
    
    return result
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(n), as the queue can store up to `n` nodes.

---

#### 5. [Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)
**Problem**: Populate each next pointer to point to its next right node.

**Approach**:
1. Perform level order traversal.
2. Connect nodes at the same level using the `next` pointer.

**Brute Force (Iterative)**:
```python
from collections import deque

def connect(root):
    if not root:
        return root
    
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        prev = None
        
        for _ in range(level_size):
            node = queue.popleft()
            
            if prev:
                prev.next = node
            prev = node
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return root
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(n), as the queue can store up to `n` nodes.

---

#### 6. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
**Problem**: Return the rightmost node at each level of a binary tree.

**Approach**:
1. Perform level order traversal.
2. Store the last node of each level.

**Brute Force (Iterative)**:
```python
from collections import deque

def rightSideView(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        
        for i in range(level_size):
            node = queue.popleft()
            
            if i == level_size - 1:
                result.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return result
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(n), as the queue can store up to `n` nodes.

---

### **Hard Problems**

#### 7. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)
**Problem**: Serialize and deserialize a binary tree using level order traversal.

**Approach**:
1. Serialize the tree using BFS.
2. Deserialize the tree using BFS.

**Brute Force (Iterative)**:
```python
from collections import deque

class Codec:
    def serialize(self, root):
        if not root:
            return ""
        
        queue = deque([root])
        result = []
        
        while queue:
            node = queue.popleft()
            
            if node:
                result.append(str(node.val))
                queue.append(node.left)
                queue.append(node.right)
            else:
                result.append("null")
        
        return ",".join(result)
    
    def deserialize(self, data):
        if not data:
            return None
        
        nodes = data.split(",")
        root = TreeNode(int(nodes[0]))
        queue = deque([root])
        index = 1
        
        while queue:
            node = queue.popleft()
            
            if index < len(nodes) and nodes[index] != "null":
                node.left = TreeNode(int(nodes[index]))
                queue.append(node.left)
            index += 1
            
            if index < len(nodes) and nodes[index] != "null":
                node.right = TreeNode(int(nodes[index]))
                queue.append(node.right)
            index += 1
        
        return root
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(n), as the queue can store up to `n` nodes.

---

#### 8. [Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)
**Problem**: Traverse a binary tree vertically.

**Approach**:
1. Use BFS to assign column indices to nodes.
2. Group nodes by their column indices.

**Brute Force (Iterative)**:
```python
from collections import deque, defaultdict

def verticalTraversal(root):
    if not root:
        return []
    
    column_table = defaultdict(list)
    queue = deque([(root, 0, 0)])  # (node, row, col)
    
    while queue:
        node, row, col = queue.popleft()
        column_table[col].append((row, node.val))
        
        if node.left:
            queue.append((node.left, row + 1, col - 1))
        if node.right:
            queue.append((node.right, row + 1, col + 1))
    
    result = []
    for col in sorted(column_table.keys()):
        result.append([val for row, val in sorted(column_table[col])])
    
    return result
```
- **Time Complexity**: O(n log n), due to sorting.
- **Space Complexity**: O(n), as the queue and dictionary can store up to `n` nodes.

---

#### 9. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
**Problem**: Find the maximum path sum in a binary tree.

**Approach**:
1. Use BFS to traverse the tree.
2. Track the maximum path sum at each node.

**Brute Force (Iterative)**:
```python
from collections import deque

def maxPathSum(root):
    if not root:
        return 0
    
    max_sum = float('-inf')
    queue = deque([root])
    
    while queue:
        node = queue.popleft()
        
        # Calculate the maximum path sum for the current node
        left = max(0, dfs(node.left))
        right = max(0, dfs(node.right))
        max_sum = max(max_sum, left + right + node.val)
        
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    
    return max_sum

def dfs(node):
    if not node:
        return 0
    left = max(0, dfs(node.left))
    right = max(0, dfs(node.right))
    return max(left, right) + node.val
```
- **Time Complexity**: O(n), where `n` is the number of nodes.
- **Space Complexity**: O(n), as the queue can store up to `n` nodes.

---

### **Summary of Python Implementations**
1. **Iterative BFS**: Uses a queue to process nodes level by level. Preferred for its simplicity and O(n) time complexity.
2. **Recursive BFS**: Not commonly used for level order traversal due to its complexity.
3. **Optimized BFS**: Uses additional data structures (e.g., dictionaries) to solve more complex problems like vertical traversal.
