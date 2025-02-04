

---

### **1. Sliding Window**
- **Subtypes**: Fixed Size, Dynamic Size  
- **Key Logic**: Maintain a window and adjust based on constraints.
- **When to Consider**:
     - The problem involves arrays, strings, or sequences.
     - You need to find a subarray or substring that meets a specific condition (e.g., maximum sum, longest substring with unique characters).
     - The problem has a fixed window size or a condition that can be satisfied by adjusting the window size.

#### Fixed Size Window (Max Subarray Sum)
```python
def max_subarray_sum(nums, k):
    """
    Input: nums = [1, 3, -1, -3, 5, 3], k = 3
    Output: 8 (Subarray [5, 3] with sum 8)
    Time: O(n), Space: O(1)
    """
    max_sum = current_sum = sum(nums[:k])
    for i in range(k, len(nums)):
        current_sum += nums[i] - nums[i - k]
        max_sum = max(max_sum, current_sum)
    return max_sum
```
[More Questions](Patterns/sliding_window_fixed_size.md)

---

#### Dynamic Size Window (Longest Substring with K Distinct)  
```python
def longest_substring(s, k):
    """
    Input: s = "araaci", k = 2
    Output: 4 ("araa" or "raac")
    Time: O(n), Space: O(k)
    """
    char_count = {}
    left = max_len = 0
    for right, char in enumerate(s):
        char_count[char] = char_count.get(char, 0) + 1
        while len(char_count) > k:
            left_char = s[left]
            char_count[left_char] -= 1
            if char_count[left_char] == 0:
                del char_count[left_char]
            left += 1
        max_len = max(max_len, right - left + 1)
    return max_len
```
[More Questions](Patterns/sliding_window_dynamic_size.md)

---

### **2. Two Pointers**
- **Subtypes**: Converging, Parallel  
- **Key Logic**: Move pointers inward or through separate arrays.
- **When to Consider**:
     - The problem involves a sorted array or linked list.
     - You need to find pairs, triplets, or manipulate elements in place.
     - The problem requires comparing or processing elements from both ends or at different speeds.

#### Converging (Two Sum in Sorted Array)
```python
def two_sum_sorted(nums, target):
    """
    Input: nums = [2, 7, 11, 15], target = 9
    Output: [0, 1] (Indices of 2 and 7)
    Time: O(n), Space: O(1)
    """
    left, right = 0, len(nums) - 1
    while left < right:
        current = nums[left] + nums[right]
        if current == target:
            return [left, right]
        elif current < target:
            left += 1
        else:
            right -= 1
    return []
```

[More Questions](Patterns/two_pointer_converging.md)

---

#### Parallel (Merge Sorted Arrays) 
```python
def merge_sorted(arr1, arr2):
    """
    Input: arr1 = [1, 3, 5], arr2 = [2, 4, 6]
    Output: [1, 2, 3, 4, 5, 6]
    Time: O(n + m), Space: O(n + m)
    """
    ptr1 = ptr2 = 0
    merged = []
    while ptr1 < len(arr1) and ptr2 < len(arr2):
        if arr1[ptr1] <= arr2[ptr2]:
            merged.append(arr1[ptr1])
            ptr1 += 1
        else:
            merged.append(arr2[ptr2])
            ptr2 += 1
    merged.extend(arr1[ptr1:] or arr2[ptr2:])
    return merged
```
[More Questions](Patterns/two_pointer_parallel.md)

---

### **3. Fast & Slow Pointers**
- **Subtypes**: Cycle Detection, Middle Node  
- **Key Logic**: `fast` moves twice as fast as `slow`.
- **When to Consider**:
     - The problem involves linked lists or arrays with cycles.
     - You need to detect cycles, find the middle of a list, or solve problems involving circular structures.

#### Cycle Detection in Linked List 
```python
class ListNode:
    def __init__(self, val=0):
        self.val = val
        self.next = None

def has_cycle(head):
    """
    Input: 1 -> 2 -> 3 -> 4 -> 2 (cycle)
    Output: True
    Time: O(n), Space: O(1)
    """
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

[More Questions](Patterns/fast_slow_pointers_cycle_detection.md)

---

#### Find Middle Node 
```python
def find_middle(head):
    """
    Input: 1 -> 2 -> 3 -> 4 -> 5
    Output: 3 (Middle node)
    Time: O(n), Space: O(1)
    """
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow.val
```
[More Questions](Patterns/fast_slow_pointers_middle_node.md)

---

### **4. Merge Intervals**
- **Subtypes**: Overlap Handling, Interval Insertion  
- **Key Logic**: Sort intervals first.
- **When to Consider**:
     - The problem involves intervals or ranges.
     - You need to merge overlapping intervals, find conflicting intervals, or insert new intervals.

#### Merge Overlapping Intervals 
```python
def merge_intervals(intervals):
    """
    Input: [[1,3], [2,6], [8,10]]
    Output: [[1,6], [8,10]]
    Time: O(n log n), Space: O(n)
    """
    intervals.sort(key=lambda x: x[0])
    merged = []
    for interval in intervals:
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            merged[-1][1] = max(merged[-1][1], interval[1])
    return merged
```

[More Questions](Patterns/merge_overlapping_intervals.md)

---

#### Insert Interval
```python
def insert_interval(intervals, new_interval):
    """
    Input: intervals = [[1,2], [3,5], [6,7]], new_interval = [4,8]
    Output: [[1,2], [3,8]]
    Time: O(n), Space: O(n)
    """
    merged = []
    for interval in intervals:
        if interval[1] < new_interval[0]:
            merged.append(interval)
        elif interval[0] > new_interval[1]:
            merged.append(new_interval)
            new_interval = interval
        else:
            new_interval = [min(interval[0], new_interval[0]),
                            max(interval[1], new_interval[1])]
    merged.append(new_interval)
    return merged
```
[More Questions](Patterns/merge_insert_interval.md)

---

### **5. Cyclic Sort**
- **Subtypes**: Missing Number, Duplicate Detection  
- **Key Logic**: Place elements at their correct index.
- **When to Consider**:
     - The problem involves an array containing numbers in a given range (e.g., 1 to `n`).
     - You need to sort the array in place or find missing/duplicate numbers.

#### Find Missing Number (1 to n) 
```python
def find_missing(nums):
    """
    Input: [3, 0, 1]
    Output: 2 (Missing number)
    Time: O(n), Space: O(1)
    """
    i = 0
    while i < len(nums):
        j = nums[i]
        if j < len(nums) and nums[i] != nums[j]:
            nums[i], nums[j] = nums[j], nums[i]
        else:
            i += 1
    for i in range(len(nums)):
        if nums[i] != i:
            return i
    return len(nums)
```
[More Questions](Patterns/cyclic_sort_find_missing_number.md)

---

#### Find All Duplicates 
```python
def find_duplicates(nums):
    """
    Input: [4,3,2,7,8,2,3,1]
    Output: [2,3] (Duplicates)
    Time: O(n), Space: O(1)
    """
    duplicates = []
    i = 0
    while i < len(nums):
        j = nums[i] - 1
        if nums[i] != nums[j]:
            nums[i], nums[j] = nums[j], nums[i]
        else:
            i += 1
    for i in range(len(nums)):
        if nums[i] != i + 1:
            duplicates.append(nums[i])
    return duplicates
```

[More Questions](Patterns/cyclic_sort_find_duplicates.md)

---

### **6. In-place Linked List Reversal**  
- **Subtypes**: Full Reversal, Sublist Reversal  
- **Key Logic**: Use pointers to reverse links incrementally.
- **When to Consider**:
     - The problem involves reversing a linked list or part of it.
     - You need to manipulate the linked list in place without using extra space.

#### Full Reversal 
```python
class ListNode:
    def __init__(self, val=0):
        self.val = val
        self.next = None

def reverse_list(head):
    """
    Input: 1 -> 2 -> 3 -> 4
    Output: 4 -> 3 -> 2 -> 1
    Time: O(n), Space: O(1)
    """
    prev = None
    current = head
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    return prev
```
[More Questions](Patterns/linked_list_full_reversal.md)

---

#### Sublist Reversal (Between Positions) 
```python
def reverse_between(head, left, right):
    """
    Input: head = 1 -> 2 -> 3 -> 4 -> 5, left=2, right=4
    Output: 1 -> 4 -> 3 -> 2 -> 5
    Time: O(n), Space: O(1)
    """
    dummy = ListNode(0)
    dummy.next = head
    prev = dummy
    for _ in range(left - 1):
        prev = prev.next
    current = prev.next
    for _ in range(right - left):
        next_node = current.next
        current.next = next_node.next
        next_node.next = prev.next
        prev.next = next_node
    return dummy.next
```
[More Questions](Patterns/linked_list_sublist_reversal.md)

---

### **7. BFS (Breadth-First Search)**  
- **Subtypes**: Level Order Traversal, Shortest Path  
- **Key Logic**: Use a queue to track nodes level by level.
- **When to Consider**:
     - The problem involves traversing trees or graphs level by level.
     - You need to find the shortest path in an unweighted graph or explore all nodes at a given depth.

#### Level Order Traversal (Binary Tree) 
```python
from collections import deque
class TreeNode:
    def __init__(self, val=0):
        self.val = val
        self.left = None
        self.right = None

def level_order(root):
    """
    Input:
        1
       / \
      2   3
    Output: [[1], [2, 3]]
    Time: O(n), Space: O(n)
    """
    if not root:
        return []
    queue = deque([root])
    result = []
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

[MOre Questions](Patterns/bfs_level_order_traversal.md)

---

#### Shortest Path in Unweighted Grid 
```python
def shortest_path(grid):
    """
    Input: grid = [
        [0, 0, 0],
        [1, 1, 0],
        [0, 0, 0]
    ]
    Output: 4 (Path: (0,0) → (0,1) → (0,2) → (1,2) → (2,2))
    Time: O(mn), Space: O(mn)
    """
    rows, cols = len(grid), len(grid[0])
    if grid[0][0] == 1:
        return -1
    directions = [(-1,0), (1,0), (0,-1), (0,1)]
    queue = deque([(0, 0)])
    grid[0][0] = 1  # Mark distance
    while queue:
        x, y = queue.popleft()
        if x == rows-1 and y == cols-1:
            return grid[x][y]
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < rows and 0 <= ny < cols and grid[nx][ny] == 0:
                grid[nx][ny] = grid[x][y] + 1
                queue.append((nx, ny))
    return -1
```
[More Questions](Patterns/bfs_shortest_path.md)

---

### **8. DFS (Depth-First Search)**  
- **Subtypes**: Pathfinding, Permutations  
- **Key Logic**: Recursive traversal with backtracking.
- **When to Consider**:
     - The problem involves traversing trees or graphs, exploring as far as possible before backtracking.
     - You need to find connected components, paths, or solve problems involving recursion.

#### Path Sum in Binary Tree 
```python
def has_path_sum(root, target):
    """
    Input: root = [5,4,8,11,None,13,4,7,2], target=22
    Output: True (Path: 5 → 4 → 11 → 2)
    Time: O(n), Space: O(h) (height of tree)
    """
    if not root:
        return False
    if not root.left and not root.right:
        return target == root.val
    return (has_path_sum(root.left, target - root.val) or 
            has_path_sum(root.right, target - root.val))
```
[More Questions](Patterns/dfs_path_finding.md)

----
#### Number of Islands (DFS Approach) 
```python
def num_islands(grid):
    """
    Input: grid = [
        ["1","1","0","0","0"],
        ["1","1","0","0","0"],
        ["0","0","1","0","0"],
        ["0","0","0","1","1"]
    ]
    Output: 3 (Number of islands)
    Time: O(mn), Space: O(mn) (call stack)
    """
    def dfs(x, y):
        if 0 <= x < len(grid) and 0 <= y < len(grid[0]) and grid[x][y] == '1':
            grid[x][y] = '0'
            dfs(x+1, y)
            dfs(x-1, y)
            dfs(x, y+1)
            dfs(x, y-1)
    count = 0
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j] == '1':
                dfs(i, j)
                count += 1
    return count
```
[More Questions](Patterns/dfs_permutations.md)

---

### **9. Heaps (Priority Queue)**  
- **Subtypes**: Top K Elements, Kth Smallest  
- **Key Logic**: Use `heapq` for efficient min/max operations.
- **When to Consider**:
     - The problem involves finding the smallest/largest elements or maintaining a dynamic ordering.
     - You need to efficiently manage a collection of elements with priority.

#### Top K Frequent Elements
```python
import heapq
def top_k_frequent(nums, k):
    """
    Input: nums = [1,1,1,2,2,3], k = 2
    Output: [1, 2]
    Time: O(n log k), Space: O(n)
    """
    freq = {}
    for num in nums:
        freq[num] = freq.get(num, 0) + 1
    return heapq.nlargest(k, freq.keys(), key=lambda x: freq[x])
```

#### Kth Smallest Element in Sorted Matrix
```python
def kth_smallest(matrix, k):
    """
    Input: matrix = [
        [1, 5, 9],
        [10, 11, 13],
        [12, 13, 15]
    ], k = 8
    Output: 13
    Time: O(k log n), Space: O(n)
    """
    min_heap = []
    for row in matrix:
        heapq.heappush(min_heap, (row[0], 0, row))
    count = 0
    while min_heap:
        num, idx, row = heapq.heappop(min_heap)
        count += 1
        if count == k:
            return num
        if idx + 1 < len(row):
            heapq.heappush(min_heap, (row[idx+1], idx+1, row))
    return -1
```

[More Questions](Patterns/heap.md)

---

### **10. Subsets & Combinations**  
- **Subtypes**: Subsets, Combination Sum  
- **Key Logic**: Backtracking with start index.
- **When to Consider**:
     - The problem involves generating all possible subsets, combinations, or permutations.
     - You need to explore all possible solutions, often using recursion.

#### Generate All Subsets 
```python
def subsets(nums):
    """
    Input: nums = [1,2,3]
    Output: [[], [1], [2], [3], [1,2], [1,3], [2,3], [1,2,3]]
    Time: O(n * 2^n), Space: O(n)
    """
    def backtrack(start, path):
        result.append(path.copy())
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()
    result = []
    backtrack(0, [])
    return result
```

[More Questions](Patterns/subsets.md)

---

#### Combination Sum (Unlimited Reuse) 
```python
def combination_sum(candidates, target):
    """
    Input: candidates = [2,3,5], target = 8
    Output: [[2,2,2,2], [2,3,3], [3,5]]
    Time: O(n^(t/m)), Space: O(t/m) (t=target, m=min candidate)
    """
    def backtrack(start, path, remaining):
        if remaining == 0:
            result.append(path.copy())
            return
        for i in range(start, len(candidates)):
            if candidates[i] > remaining:
                continue
            path.append(candidates[i])
            backtrack(i, path, remaining - candidates[i])
            path.pop()
    result = []
    backtrack(0, [], target)
    return result
```
[More Questions](Patterns/combination_sum.md)

---

### **11. Modified Binary Search**  
- **Subtypes**: Rotated Arrays, Infinite Arrays  
- **Key Logic**: Adjust search range based on sorted halves.
- **When to Consider**:
     - The problem involves searching in a sorted array or finding the boundary of a condition.
     - The problem requires efficient searching with a time complexity of O(log n).

#### Search in Rotated Sorted Array 
```python
def search_rotated(nums, target):
    """
    Input: nums = [4,5,6,7,0,1,2], target = 0
    Output: 4 (Index of 0)
    Time: O(log n), Space: O(1)
    """
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        # Check if left half is sorted
        if nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    return -1
```
[More Questions](Patterns/binary_search_rotated_arrays.md)

---

#### Find Minimum in Rotated Sorted Array
```python
def find_min_rotated(nums):
    """
    Input: nums = [3,4,5,1,2]
    Output: 1 (Minimum element)
    Time: O(log n), Space: O(1)
    """
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > nums[right]:
            left = mid + 1
        else:
            right = mid
    return nums[left]
```
[More Questins on Infinite](Patterns/binary_search_rotated_arrays.md)

---

### **12. Topological Sort**  
- **Subtypes**: Kahn’s Algorithm, DFS-Based  
- **Key Logic**: Track in-degrees or detect cycles.
- **When to Consider**:
     - The problem involves directed acyclic graphs (DAGs) with dependencies.
     - You need to find a linear ordering of vertices where each vertex comes before its dependencies.

#### Kahn’s Algorithm (Course Schedule) 
```python
from collections import deque
def can_finish(num_courses, prerequisites):
    """
    Input: num_courses = 2, prerequisites = [[1,0]]
    Output: True (Can finish course 0 then 1)
    Time: O(V + E), Space: O(V + E)
    """
    in_degree = [0] * num_courses
    graph = [[] for _ in range(num_courses)]
    for course, prereq in prerequisites:
        graph[prereq].append(course)
        in_degree[course] += 1
    queue = deque([i for i in range(num_courses) if in_degree[i] == 0])
    count = 0
    while queue:
        node = queue.popleft()
        count += 1
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    return count == num_courses
```

[More Questions](Patterns/topological_sort_kahn_algo.md)

---

#### DFS-Based Cycle Detection 
```python
def can_finish_dfs(num_courses, prerequisites):
    """
    Input: num_courses = 2, prerequisites = [[1,0], [0,1]]
    Output: False (Cycle detected)
    Time: O(V + E), Space: O(V)
    """
    graph = [[] for _ in range(num_courses)]
    visited = [0] * num_courses  # 0=unvisited, 1=visiting, 2=visited
    for course, prereq in prerequisites:
        graph[course].append(prereq)
    def has_cycle(course):
        if visited[course] == 1:
            return True
        if visited[course] == 2:
            return False
        visited[course] = 1
        for prereq in graph[course]:
            if has_cycle(prereq):
                return True
        visited[course] = 2
        return False
    for course in range(num_courses):
        if has_cycle(course):
            return False
    return True
```

[More Questions](Patterns/topological_sort_dfs.md)


---

### **13. Knapsack (Dynamic Programming)**  
- **Subtypes**: 0/1 Knapsack, Unbounded Knapsack  
- **Key Logic**: Build DP table for item inclusion/exclusion.
- **When to Consider**:
     - The problem involves optimization with constraints (e.g., selecting items with maximum value without exceeding a limit).
     - The problem can be broken into overlapping subproblems.

#### 0/1 Knapsack Problem 
```python
def knapsack(weights, values, capacity):
    """
    Input: weights = [1,2,3], values = [6,10,12], capacity=5
    Output: 22 (Items 2 and 3)
    Time: O(n * capacity), Space: O(n * capacity)
    """
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                dp[i][w] = max(dp[i-1][w], values[i-1] + dp[i-1][w - weights[i-1]])
            else:
                dp[i][w] = dp[i-1][w]
    return dp[n][capacity]
```

[More Questions](Patterns/knapsack_zero_one.md)

---

#### Unbounded Knapsack (Coin Change) 
```python
def coin_change(coins, amount):
    """
    Input: coins = [1,2,5], amount = 11
    Output: 3 (5 + 5 + 1)
    Time: O(n * amount), Space: O(amount)
    """
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1
```
[More Questions](Patterns/knapsack_unbounded.md)

---

### **14. Backtracking**  
- **Subtypes**: Permutations, N-Queens  
- **Key Logic**: Generate solutions via trial and error.
- **When to Consider**:
     - The problem involves exploring all possible solutions, often with recursion.
     - You need to generate permutations, combinations, or solve constraint satisfaction problems.

#### Generate All Permutations 
```python
def permute(nums):
    """
    Input: nums = [1,2,3]
    Output: [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]
    Time: O(n!), Space: O(n)
    """
    def backtrack(start):
        if start == len(nums):
            result.append(nums.copy())
            return
        for i in range(start, len(nums)):
            nums[start], nums[i] = nums[i], nums[start]
            backtrack(start + 1)
            nums[start], nums[i] = nums[i], nums[start]
    result = []
    backtrack(0)
    return result
```

[More Questions](Patterns/backtracking_permutations.md)

---

#### N-Queens Problem 
```python
def solve_n_queens(n):
    """
    Input: n = 4
    Output: [
        [".Q..","...Q","Q...","..Q."],
        ["..Q.","Q...","...Q",".Q.."]
    ]
    Time: O(n!), Space: O(n^2)
    """
    def backtrack(row, cols, diag1, diag2):
        if row == n:
            result.append(["".join(row) for row in board])
            return
        for col in range(n):
            d1, d2 = row - col, row + col
            if col in cols or d1 in diag1 or d2 in diag2:
                continue
            cols.add(col)
            diag1.add(d1)
            diag2.add(d2)
            board[row][col] = 'Q'
            backtrack(row + 1, cols, diag1, diag2)
            board[row][col] = '.'
            cols.remove(col)
            diag1.remove(d1)
            diag2.remove(d2)
    result = []
    board = [['.'] * n for _ in range(n)]
    backtrack(0, set(), set(), set())
    return result
```

[More Questions](Patterns/backtracking_n_queen.md)

---

### **15. Bit Manipulation**  
- **Subtypes**: Single Number, Bit Counting  
- **Key Logic**: Use XOR or bitwise operations for optimization.
- **When to Consider**:
     - The problem involves binary operations or requires working with bits.
     - You need to count set bits, find unique numbers, or perform efficient bitwise operations.

#### Single Number (Non-Duplicate) 
```python
def single_number(nums):
    """
    Input: nums = [4,1,2,1,2]
    Output: 4
    Time: O(n), Space: O(1)
    """
    result = 0
    for num in nums:
        result ^= num
    return result
```

[More Questions](Patterns/bit_manipulation_single_number.md)

---

#### Count Set Bits (Hamming Weight) 
```python
def count_set_bits(n):
    """
    Input: n = 11 (Binary: 1011)
    Output: 3
    Time: O(1), Space: O(1)
    """
    count = 0
    while n:
        n &= n - 1  # Clear the least significant set bit
        count += 1
    return count
```
[More Questions](Patterns/bit_manipulation_bit_counting.md)

---

### **16. Union-Find (Disjoint Set)**  
- **Subtypes**: Basic Operations, Cycle Detection  
- **Key Logic**: Path compression and union by rank.
- **When to Consider**:
     - The problem involves grouping elements into disjoint sets.
     - You need to efficiently perform union and find operations.

#### Union-Find Class 
```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))
        self.rank = [0] * size
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # Path compression
        return self.parent[x]
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x != root_y:
            if self.rank[root_x] > self.rank[root_y]:  # Union by rank
                self.parent[root_y] = root_x
            else:
                self.parent[root_x] = root_y
                if self.rank[root_x] == self.rank[root_y]:
                    self.rank[root_y] += 1
```

[More Questions](Patterns/union_find_disjoint_set.md)

---

#### Detect Cycle in Undirected Graph 
```python
def has_cycle(edges, n):
    """
    Input: edges = [[0,1], [1,2], [2,0]], n = 3
    Output: True (Cycle detected)
    Time: O(α(n)), Space: O(n)
    """
    uf = UnionFind(n)
    for u, v in edges:
        root_u = uf.find(u)
        root_v = uf.find(v)
        if root_u == root_v:
            return True
        uf.union(u, v)
    return False
```
[More Questions](Patterns/union_find_cycle_detect.md)

---

### **17. Dynamic Programming**  
- **Subtypes**: Memoization (Top-Down), Tabulation (Bottom-Up)  
- **Key Logic**: Cache subproblem results to avoid recomputation.
- The problem involves overlapping subproblems and optimal substructure.
     - You need to break the problem into smaller subproblems and store their results to avoid redundant calculations.

#### Fibonacci with Memoization
```python
from functools import lru_cache
def fib(n):
    """
    Input: n = 5
    Output: 5 (Sequence: 0,1,1,2,3,5)
    Time: O(n), Space: O(n)
    """
    @lru_cache(maxsize=None)
    def dfs(n):
        if n <= 1:
            return n
        return dfs(n-1) + dfs(n-2)
    return dfs(n)
```

#### Fibonacci with Tabulation
```python
def fib_tabulation(n):
    """
    Input: n = 5
    Output: 5
    Time: O(n), Space: O(n)
    """
    if n <= 1:
        return n
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

---

### Summery


| **Pattern**                     | **When to Consider**                                                                 | **High-Level Solving Approach**                                                                                   |
|----------------------------------|-------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Sliding Window**               | Subarrays/substrings with fixed or dynamic window size.                             | Use two pointers to represent the window. Adjust the window based on the problem condition (e.g., expand/shrink). |
| **Two Pointers**                 | Sorted arrays/linked lists, finding pairs or manipulating elements.                 | Use two pointers (start/end or slow/fast) to traverse the array/list and compare/process elements.                |
| **Fast & Slow Pointers**         | Linked lists/arrays with cycles, finding middle or detecting cycles.                | Use two pointers moving at different speeds (e.g., one moves 2x faster) to detect cycles or find the middle.      |
| **Merge Intervals**              | Overlapping intervals, merging or inserting intervals.                              | Sort intervals by start time, then iterate and merge overlapping intervals.                                       |
| **Cyclic Sort**                  | Arrays with numbers in a range, finding missing/duplicate numbers.                  | Iterate through the array and place each number in its correct index.                                             |
| **In-place Linked List Reversal**| Reversing linked lists or parts of them.                                            | Use three pointers (prev, curr, next) to reverse the list in place.                                               |
| **BFS**                          | Level-by-level traversal, shortest path in unweighted graphs.                       | Use a queue to process nodes level by level. Track visited nodes to avoid cycles.                                 |
| **DFS**                          | Exploring all paths, connected components, or recursive traversal.                  | Use recursion or a stack to explore as far as possible before backtracking.                                       |
| **Heaps**                        | Finding smallest/largest elements, maintaining dynamic ordering.                    | Use a min-heap or max-heap to efficiently retrieve the smallest/largest elements.                                 |
| **Subsets & Combinations**       | Generating all possible subsets, combinations, or permutations.                     | Use backtracking to explore all possible combinations, pruning invalid paths.                                     |
| **Modified Binary Search**       | Searching in sorted arrays, finding boundaries or specific elements.                | Adjust the search space based on the mid-value comparison. Handle edge cases for boundaries.                      |
| **Topological Sort**             | Directed acyclic graphs (DAGs) with dependencies.                                   | Use BFS (Kahn's algorithm) or DFS to process nodes in linear order based on dependencies.                         |
| **Knapsack**                     | Optimization with constraints, selecting items with maximum value.                  | Use dynamic programming to build a table of subproblem solutions.                                                 |
| **Backtracking**                 | Exploring all possible solutions, often with recursion.                             | Use recursion to explore choices, backtrack when constraints are violated.                                        |
| **Bit Manipulation**             | Binary operations, counting set bits, or finding unique numbers.                    | Use bitwise operations (AND, OR, XOR, shifts) to manipulate and extract information from binary representations.   |
| **Union-Find**                   | Grouping elements into disjoint sets, detecting cycles.                             | Use path compression and union by rank to efficiently manage and merge disjoint sets.                             |
| **Dynamic Programming**          | Overlapping subproblems, breaking problems into smaller subproblems.                | Use memoization (top-down) or tabulation (bottom-up) to store and reuse subproblem results.                       |

---

### Key Notes:
1. **Sliding Window**: Maintain a window of elements and slide it based on the problem condition.
2. **Two Pointers**: Move pointers towards each other or in the same direction based on comparisons.
3. **Fast & Slow Pointers**: Use two pointers moving at different speeds to detect cycles or find the middle.
4. **Merge Intervals**: Sort intervals and merge overlapping ones by comparing start and end times.
5. **Cyclic Sort**: Place each number in its correct index by swapping.
6. **In-place Linked List Reversal**: Reverse pointers iteratively or recursively.
7. **BFS**: Use a queue to process nodes level by level.
8. **DFS**: Use recursion or a stack to explore paths deeply.
9. **Heaps**: Use a priority queue to efficiently retrieve min/max elements.
10. **Subsets & Combinations**: Use backtracking to generate all possible combinations.
11. **Modified Binary Search**: Adjust the search space based on mid-value comparisons.
12. **Topological Sort**: Use BFS or DFS to process nodes in dependency order.
13. **Knapsack**: Build a DP table to store solutions for subproblems.
14. **Backtracking**: Explore choices recursively and backtrack when constraints are violated.
15. **Bit Manipulation**: Use bitwise operations to solve problems efficiently.
16. **Union-Find**: Use path compression and union by rank to manage disjoint sets.
17. **Dynamic Programming**: Store and reuse subproblem results to avoid redundant calculations.

---
