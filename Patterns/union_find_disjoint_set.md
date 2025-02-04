
---

### **Easy Problems**

#### 1. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
**Problem**: Find the number of connected components in a graph represented by an adjacency matrix.

**Approach**:
1. Use Union-Find to group connected cities.
2. Count the number of unique roots.

**Brute Force (DFS)**:
```python
def findCircleNum(isConnected):
    n = len(isConnected)
    visited = [False] * n
    count = 0

    def dfs(city):
        for neighbor in range(n):
            if isConnected[city][neighbor] == 1 and not visited[neighbor]:
                visited[neighbor] = True
                dfs(neighbor)

    for i in range(n):
        if not visited[i]:
            dfs(i)
            count += 1
    return count
```
- **Time Complexity**: O(n²), as we traverse the adjacency matrix.
- **Space Complexity**: O(n), as we store the visited array.

**Optimized (Union-Find)**:
```python
def findCircleNum(isConnected):
    n = len(isConnected)
    parent = [i for i in range(n)]

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            parent[rootX] = rootY

    for i in range(n):
        for j in range(n):
            if isConnected[i][j] == 1:
                union(i, j)
    
    return len(set(find(i) for i in range(n)))
```
- **Time Complexity**: O(n² * α(n)), where `α(n)` is the inverse Ackermann function (nearly constant).
- **Space Complexity**: O(n), as we store the parent array.

---

#### 2. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)
**Problem**: Find the edge that forms a cycle in a graph.

**Approach**:
1. Use Union-Find to detect the first edge that connects two already connected nodes.

**Brute Force (DFS)**:
```python
def findRedundantConnection(edges):
    from collections import defaultdict

    graph = defaultdict(set)
    for u, v in edges:
        visited = set()
        if dfs(u, v, visited, graph):
            return [u, v]
        graph[u].add(v)
        graph[v].add(u)
    return []

def dfs(u, v, visited, graph):
    if u == v:
        return True
    visited.add(u)
    for neighbor in graph[u]:
        if neighbor not in visited:
            if dfs(neighbor, v, visited, graph):
                return True
    return False
```
- **Time Complexity**: O(n²), as we perform DFS for each edge.
- **Space Complexity**: O(n), as we store the graph and visited set.

**Optimized (Union-Find)**:
```python
def findRedundantConnection(edges):
    parent = [i for i in range(len(edges) + 1)]

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX == rootY:
            return False
        parent[rootX] = rootY
        return True

    for u, v in edges:
        if not union(u, v):
            return [u, v]
    return []
```
- **Time Complexity**: O(n * α(n)), where `α(n)` is the inverse Ackermann function.
- **Space Complexity**: O(n), as we store the parent array.

---

#### 3. [Friend Circles](https://leetcode.com/problems/friend-circles/)
**Problem**: Similar to Number of Provinces, but with a different name.

**Approach**:
1. Use Union-Find to group connected friends.
2. Count the number of unique roots.

**Optimized (Union-Find)**:
```python
def findCircleNum(isConnected):
    n = len(isConnected)
    parent = [i for i in range(n)]

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            parent[rootX] = rootY

    for i in range(n):
        for j in range(n):
            if isConnected[i][j] == 1:
                union(i, j)
    
    return len(set(find(i) for i in range(n)))
```
- **Time Complexity**: O(n² * α(n)), where `α(n)` is the inverse Ackermann function.
- **Space Complexity**: O(n), as we store the parent array.

---

### **Medium Problems**

#### 4. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)
**Problem**: Merge accounts with common emails.

**Approach**:
1. Use Union-Find to group accounts with common emails.
2. Merge emails for each group.

**Optimized (Union-Find)**:
```python
def accountsMerge(accounts):
    from collections import defaultdict

    parent = {}
    email_to_name = {}

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            parent[rootX] = rootY

    for account in accounts:
        name = account[0]
        for email in account[1:]:
            if email not in parent:
                parent[email] = email
            email_to_name[email] = name
            union(account[1], email)
    
    groups = defaultdict(list)
    for email in parent:
        groups[find(email)].append(email)
    
    result = []
    for root in groups:
        result.append([email_to_name[root]] + sorted(groups[root]))
    return result
```
- **Time Complexity**: O(n * α(n)), where `n` is the number of emails.
- **Space Complexity**: O(n), as we store the parent and email-to-name mappings.

---

#### 5. [Number of Islands II](https://leetcode.com/problems/number-of-islands-ii/)
**Problem**: Given a grid of water and land, dynamically count the number of islands after each operation.

**Approach**:
1. Use Union-Find to dynamically connect islands.
2. Track the number of islands after each operation.

**Optimized (Union-Find)**:
```python
def numIslands2(m, n, positions):
    parent = {}
    count = 0
    result = []

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        nonlocal count
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            parent[rootX] = rootY
            count -= 1

    for x, y in positions:
        if (x, y) not in parent:
            parent[(x, y)] = (x, y)
            count += 1
            for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                nx, ny = x + dx, y + dy
                if (nx, ny) in parent:
                    union((x, y), (nx, ny))
        result.append(count)
    return result
```
- **Time Complexity**: O(k * α(k)), where `k` is the number of operations.
- **Space Complexity**: O(k), as we store the parent array.

---

#### 6. [Most Stones Removed with Same Row or Column](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/)
**Problem**: Remove stones such that no two stones share the same row or column.

**Approach**:
1. Use Union-Find to group stones in the same row or column.
2. Calculate the maximum number of stones that can be removed.

**Optimized (Union-Find)**:
```python
def removeStones(stones):
    parent = {}

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            parent[rootX] = rootY

    for x, y in stones:
        row = f"row-{x}"
        col = f"col-{y}"
        if row not in parent:
            parent[row] = row
        if col not in parent:
            parent[col] = col
        union(row, col)
    
    return len(stones) - len(set(find(x) for x in parent))
```
- **Time Complexity**: O(n * α(n)), where `n` is the number of stones.
- **Space Complexity**: O(n), as we store the parent array.

---

### **Hard Problems**

#### 7. [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
**Problem**: Find the length of the longest consecutive sequence in an unsorted array.

**Approach**:
1. Use Union-Find to group consecutive numbers.
2. Track the size of each group.

**Optimized (Union-Find)**:
```python
def longestConsecutive(nums):
    if not nums:
        return 0
    parent = {num: num for num in nums}
    size = {num: 1 for num in nums}

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            if size[rootX] < size[rootY]:
                parent[rootX] = rootY
                size[rootY] += size[rootX]
            else:
                parent[rootY] = rootX
                size[rootX] += size[rootY]

    for num in nums:
        if num - 1 in parent:
            union(num, num - 1)
        if num + 1 in parent:
            union(num, num + 1)
    
    return max(size.values())
```
- **Time Complexity**: O(n * α(n)), where `n` is the number of elements.
- **Space Complexity**: O(n), as we store the parent and size arrays.

---

#### 8. [Regions Cut By Slashes](https://leetcode.com/problems/regions-cut-by-slashes/)
**Problem**: Given a grid of slashes, backslashes, and spaces, count the number of regions.

**Approach**:
1. Use Union-Find to group connected regions.
2. Handle slashes and backslashes by splitting each cell into 4 sub-cells.

**Optimized (Union-Find)**:
```python
def regionsBySlashes(grid):
    n = len(grid)
    parent = [i for i in range(4 * n * n)]

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            parent[rootX] = rootY

    for i in range(n):
        for j in range(n):
            cell = grid[i][j]
            base = 4 * (i * n + j)
            # Union within the cell
            if cell == '/':
                union(base, base + 3)
                union(base + 1, base + 2)
            elif cell == '\\':
                union(base, base + 1)
                union(base + 2, base + 3)
            else:
                union(base, base + 1)
                union(base + 1, base + 2)
                union(base + 2, base + 3)
            # Union with neighboring cells
            if i > 0:
                union(base, base - 4 * n + 2)
            if j > 0:
                union(base + 3, base - 4 + 1)
    
    return len(set(find(i) for i in range(4 * n * n)))
```
- **Time Complexity**: O(n² * α(n)), where `n` is the size of the grid.
- **Space Complexity**: O(n²), as we store the parent array.

---

#### 9. [Bricks Falling When Hit](https://leetcode.com/problems/bricks-falling-when-hit/)
**Problem**: Simulate the effect of hitting bricks and count the number of bricks that fall.

**Approach**:
1. Use Union-Find to simulate the grid and track connected components.
2. Reverse the operations to efficiently calculate the number of falling bricks.

**Optimized (Union-Find)**:
```python
def hitBricks(grid, hits):
    m, n = len(grid), len(grid[0])
    parent = [i for i in range(m * n + 1)]
    size = [1] * (m * n + 1)

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            if size[rootX] < size[rootY]:
                parent[rootX] = rootY
                size[rootY] += size[rootX]
            else:
                parent[rootY] = rootX
                size[rootX] += size[rootY]

    # Mark hits
    for x, y in hits:
        if grid[x][y] == 1:
            grid[x][y] = 2
    
    # Initialize Union-Find
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 1:
                if i == 0:
                    union(i * n + j, m * n)
                if i > 0 and grid[i - 1][j] == 1:
                    union(i * n + j, (i - 1) * n + j)
                if j > 0 and grid[i][j - 1] == 1:
                    union(i * n + j, i * n + j - 1)
    
    # Reverse the hits
    result = []
    for x, y in reversed(hits):
        if grid[x][y] == 2:
            grid[x][y] = 1
            if x == 0:
                union(x * n + y, m * n)
            if x > 0 and grid[x - 1][y] == 1:
                union(x * n + y, (x - 1) * n + y)
            if x < m - 1 and grid[x + 1][y] == 1:
                union(x * n + y, (x + 1) * n + y)
            if y > 0 and grid[x][y - 1] == 1:
                union(x * n + y, x * n + y - 1)
            if y < n - 1 and grid[x][y + 1] == 1:
                union(x * n + y, x * n + y + 1)
            result.append(max(0, size[find(m * n)] - size[find(x * n + y)] - 1))
        else:
            result.append(0)
    return result[::-1]
```
- **Time Complexity**: O(k * α(n)), where `k` is the number of hits and `n` is the grid size.
- **Space Complexity**: O(n²), as we store the parent and size arrays.

---

### **Summary of Python Implementations**
1. **Union-Find**: The standard approach for grouping and finding connected components.
2. **Path Compression**: Optimizes the `find` operation to make it nearly constant time.
3. **Union by Size/Rank**: Balances the tree to optimize the `union` operation.

