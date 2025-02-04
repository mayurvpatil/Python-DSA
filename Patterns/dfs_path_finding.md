
---

### **Easy Problems**

#### 1. [Number of Islands](https://leetcode.com/problems/number-of-islands/)
**Problem**: Count the number of islands in a binary grid.

**Approach**:
1. Use DFS to explore all connected `1`s (islands).
2. Mark visited cells to avoid reprocessing.

**Brute Force (DFS)**:
```python
def numIslands(grid):
    if not grid:
        return 0
    
    n, m = len(grid), len(grid[0])
    count = 0
    
    def dfs(row, col):
        if row < 0 or row >= n or col < 0 or col >= m or grid[row][col] == '0':
            return
        grid[row][col] = '0'  # Mark as visited
        dfs(row + 1, col)
        dfs(row - 1, col)
        dfs(row, col + 1)
        dfs(row, col - 1)
    
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '1':
                count += 1
                dfs(i, j)
    
    return count
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), due to the recursion stack in the worst case.

---

#### 2. [Flood Fill](https://leetcode.com/problems/flood-fill/)
**Problem**: Perform a flood fill on an image starting from a given pixel.

**Approach**:
1. Use DFS to explore all connected pixels with the same color.
2. Update the color of visited pixels.

**Brute Force (DFS)**:
```python
def floodFill(image, sr, sc, newColor):
    if not image or image[sr][sc] == newColor:
        return image
    
    n, m = len(image), len(image[0])
    oldColor = image[sr][sc]
    
    def dfs(row, col):
        if row < 0 or row >= n or col < 0 or col >= m or image[row][col] != oldColor:
            return
        image[row][col] = newColor  # Mark as visited
        dfs(row + 1, col)
        dfs(row - 1, col)
        dfs(row, col + 1)
        dfs(row, col - 1)
    
    dfs(sr, sc)
    return image
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), due to the recursion stack in the worst case.

---

#### 3. [Max Area of Island](https://leetcode.com/problems/max-area-of-island/)
**Problem**: Find the maximum area of an island in a binary grid.

**Approach**:
1. Use DFS to explore all connected `1`s (islands).
2. Track the area of each island.

**Brute Force (DFS)**:
```python
def maxAreaOfIsland(grid):
    if not grid:
        return 0
    
    n, m = len(grid), len(grid[0])
    max_area = 0
    
    def dfs(row, col):
        if row < 0 or row >= n or col < 0 or col >= m or grid[row][col] == 0:
            return 0
        grid[row][col] = 0  # Mark as visited
        return 1 + dfs(row + 1, col) + dfs(row - 1, col) + dfs(row, col + 1) + dfs(row, col - 1)
    
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1:
                max_area = max(max_area, dfs(i, j))
    
    return max_area
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), due to the recursion stack in the worst case.

---

### **Medium Problems**

#### 4. [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)
**Problem**: Capture all regions surrounded by `'X'` in a grid.

**Approach**:
1. Use DFS to mark all `'O'`s connected to the boundary.
2. Flip the remaining `'O'`s to `'X'`.

**Brute Force (DFS)**:
```python
def solve(board):
    if not board:
        return
    
    n, m = len(board), len(board[0])
    
    def dfs(row, col):
        if row < 0 or row >= n or col < 0 or col >= m or board[row][col] != 'O':
            return
        board[row][col] = 'T'  # Mark as temporary
        dfs(row + 1, col)
        dfs(row - 1, col)
        dfs(row, col + 1)
        dfs(row, col - 1)
    
    # Mark boundary-connected 'O's
    for i in range(n):
        dfs(i, 0)
        dfs(i, m - 1)
    for j in range(m):
        dfs(0, j)
        dfs(n - 1, j)
    
    # Flip remaining 'O's to 'X' and restore 'T's to 'O'
    for i in range(n):
        for j in range(m):
            if board[i][j] == 'O':
                board[i][j] = 'X'
            elif board[i][j] == 'T':
                board[i][j] = 'O'
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), due to the recursion stack in the worst case.

---

#### 5. [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)
**Problem**: Find cells in a grid where water can flow to both the Pacific and Atlantic oceans.

**Approach**:
1. Use DFS to mark cells reachable from the Pacific and Atlantic oceans.
2. Find the intersection of cells reachable from both oceans.

**Brute Force (DFS)**:
```python
def pacificAtlantic(heights):
    if not heights:
        return []
    
    n, m = len(heights), len(heights[0])
    pacific = [[False] * m for _ in range(n)]
    atlantic = [[False] * m for _ in range(n)]
    
    def dfs(row, col, visited, prev_height):
        if row < 0 or row >= n or col < 0 or col >= m or visited[row][col] or heights[row][col] < prev_height:
            return
        visited[row][col] = True
        dfs(row + 1, col, visited, heights[row][col])
        dfs(row - 1, col, visited, heights[row][col])
        dfs(row, col + 1, visited, heights[row][col])
        dfs(row, col - 1, visited, heights[row][col])
    
    # Mark cells reachable from Pacific
    for i in range(n):
        dfs(i, 0, pacific, heights[i][0])
    for j in range(m):
        dfs(0, j, pacific, heights[0][j])
    
    # Mark cells reachable from Atlantic
    for i in range(n):
        dfs(i, m - 1, atlantic, heights[i][m - 1])
    for j in range(m):
        dfs(n - 1, j, atlantic, heights[n - 1][j])
    
    # Find intersection
    result = []
    for i in range(n):
        for j in range(m):
            if pacific[i][j] and atlantic[i][j]:
                result.append([i, j])
    return result
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), due to the recursion stack and visited matrices.

---

#### 6. [Word Search](https://leetcode.com/problems/word-search/)
**Problem**: Check if a word exists in a 2D grid of characters.

**Approach**:
1. Use DFS to explore all possible paths in the grid.
2. Backtrack if the current path does not match the word.

**Brute Force (DFS with Backtracking)**:
```python
def exist(board, word):
    if not board or not word:
        return False
    
    n, m = len(board), len(board[0])
    
    def dfs(row, col, index):
        if index == len(word):
            return True
        if row < 0 or row >= n or col < 0 or col >= m or board[row][col] != word[index]:
            return False
        temp = board[row][col]
        board[row][col] = '#'  # Mark as visited
        found = (dfs(row + 1, col, index + 1) or
                 dfs(row - 1, col, index + 1) or
                 dfs(row, col + 1, index + 1) or
                 dfs(row, col - 1, index + 1))
        board[row][col] = temp  # Backtrack
        return found
    
    for i in range(n):
        for j in range(m):
            if dfs(i, j, 0):
                return True
    return False
```
- **Time Complexity**: O(n * m * 4^k), where `n` is the number of rows, `m` is the number of columns, and `k` is the length of the word.
- **Space Complexity**: O(k), due to the recursion stack.

---

### **Hard Problems**

#### 7. [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)
**Problem**: Find the length of the longest increasing path in a matrix.

**Approach**:
1. Use DFS with memoization to explore all increasing paths.
2. Track the longest path length.

**Optimized (DFS with Memoization)**:
```python
def longestIncreasingPath(matrix):
    if not matrix:
        return 0
    
    n, m = len(matrix), len(matrix[0])
    memo = [[0] * m for _ in range(n)]
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    def dfs(row, col):
        if memo[row][col]:
            return memo[row][col]
        max_path = 1
        for dr, dc in directions:
            r, c = row + dr, col + dc
            if 0 <= r < n and 0 <= c < m and matrix[r][c] > matrix[row][col]:
                max_path = max(max_path, 1 + dfs(r, c))
        memo[row][col] = max_path
        return max_path
    
    result = 0
    for i in range(n):
        for j in range(m):
            result = max(result, dfs(i, j))
    return result
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), due to the memoization matrix.

---

#### 8. [Making A Large Island](https://leetcode.com/problems/making-a-large-island/)
**Problem**: Find the size of the largest island after changing at most one `0` to `1`.

**Approach**:
1. Use DFS to assign unique IDs to each island and calculate their sizes.
2. For each `0`, check the size of the island formed by connecting adjacent islands.

**Optimized (DFS with Union-Find)**:
```python
def largestIsland(grid):
    if not grid:
        return 0
    
    n, m = len(grid), len(grid[0])
    island_id = 2
    island_sizes = {}
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    def dfs(row, col, id):
        if row < 0 or row >= n or col < 0 or col >= m or grid[row][col] != 1:
            return 0
        grid[row][col] = id
        size = 1
        for dr, dc in directions:
            size += dfs(row + dr, col + dc, id)
        return size
    
    # Assign IDs and calculate sizes
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 1:
                island_sizes[island_id] = dfs(i, j, island_id)
                island_id += 1
    
    # Find the maximum island size
    max_size = max(island_sizes.values(), default=0)
    
    # Check each 0
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 0:
                unique_ids = set()
                for dr, dc in directions:
                    r, c = i + dr, j + dc
                    if 0 <= r < n and 0 <= c < m and grid[r][c] != 0:
                        unique_ids.add(grid[r][c])
                total = 1 + sum(island_sizes[id] for id in unique_ids)
                max_size = max(max_size, total)
    
    return max_size
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), due to the DFS recursion stack and island sizes dictionary.

---

#### 9. [Robot Room Cleaner](https://leetcode.com/problems/robot-room-cleaner/)
**Problem**: Clean an entire room using a robot with limited API access.

**Approach**:
1. Use DFS to explore all reachable cells.
2. Backtrack to clean the entire room.

**Optimized (DFS with Backtracking)**:
```python
def cleanRoom(robot):
    def dfs(row, col, d):
        visited.add((row, col))
        robot.clean()
        for _ in range(4):
            new_d = (d + _) % 4
            new_row = row + directions[new_d][0]
            new_col = col + directions[new_d][1]
            if (new_row, new_col) not in visited and robot.move():
                dfs(new_row, new_col, new_d)
                robot.turnRight()
                robot.turnRight()
                robot.move()
                robot.turnRight()
                robot.turnRight()
            robot.turnRight()
    
    directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]
    visited = set()
    dfs(0, 0, 0)
```
- **Time Complexity**: O(n - m), where `n` is the number of cells and `m` is the number of obstacles.
- **Space Complexity**: O(n - m), due to the recursion stack and visited set.

---

### **Summary of Python Implementations**
1. **DFS**: The standard approach for exploring all paths in a grid.
2. **DFS with Backtracking**: Used for problems requiring exploration of all possible paths (e.g., Word Search).
3. **DFS with Memoization**: Optimizes repeated subproblems (e.g., Longest Increasing Path).

