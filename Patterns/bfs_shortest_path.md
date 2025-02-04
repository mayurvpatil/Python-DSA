
---

### **Easy Problems**

#### 1. [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/)
**Problem**: Find the shortest path from the top-left to the bottom-right of a binary matrix, traversing only through `0`s.

**Approach**:
1. Use BFS to explore all possible paths.
2. Track the number of steps taken to reach each cell.

**Brute Force (BFS)**:
```python
from collections import deque

def shortestPathBinaryMatrix(grid):
    if not grid or grid[0][0] == 1 or grid[-1][-1] == 1:
        return -1
    
    n = len(grid)
    queue = deque([(0, 0, 1)])  # (row, col, steps)
    grid[0][0] = 1  # Mark as visited
    
    directions = [(-1, -1), (-1, 0), (-1, 1),
                  (0, -1),          (0, 1),
                  (1, -1),  (1, 0), (1, 1)]
    
    while queue:
        row, col, steps = queue.popleft()
        
        if row == n - 1 and col == n - 1:
            return steps
        
        for dr, dc in directions:
            r, c = row + dr, col + dc
            if 0 <= r < n and 0 <= c < n and grid[r][c] == 0:
                grid[r][c] = 1  # Mark as visited
                queue.append((r, c, steps + 1))
    
    return -1
```
- **Time Complexity**: O(n²), where `n` is the size of the grid.
- **Space Complexity**: O(n²), as the queue can store up to `n²` cells.

---

#### 2. [Number of Islands](https://leetcode.com/problems/number-of-islands/)
**Problem**: Count the number of islands in a binary grid.

**Approach**:
1. Use BFS to explore all connected `1`s (islands).
2. Mark visited cells to avoid reprocessing.

**Brute Force (BFS)**:
```python
from collections import deque

def numIslands(grid):
    if not grid:
        return 0
    
    n, m = len(grid), len(grid[0])
    count = 0
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '1':
                count += 1
                queue = deque([(i, j)])
                grid[i][j] = '0'  # Mark as visited
                
                while queue:
                    row, col = queue.popleft()
                    for dr, dc in directions:
                        r, c = row + dr, col + dc
                        if 0 <= r < n and 0 <= c < m and grid[r][c] == '1':
                            grid[r][c] = '0'  # Mark as visited
                            queue.append((r, c))
    
    return count
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), as the queue can store up to `n * m` cells.

---

#### 3. [Flood Fill](https://leetcode.com/problems/flood-fill/)
**Problem**: Perform a flood fill on an image starting from a given pixel.

**Approach**:
1. Use BFS to explore all connected pixels with the same color.
2. Update the color of visited pixels.

**Brute Force (BFS)**:
```python
from collections import deque

def floodFill(image, sr, sc, newColor):
    if not image or image[sr][sc] == newColor:
        return image
    
    n, m = len(image), len(image[0])
    oldColor = image[sr][sc]
    queue = deque([(sr, sc)])
    image[sr][sc] = newColor  # Mark as visited
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while queue:
        row, col = queue.popleft()
        for dr, dc in directions:
            r, c = row + dr, col + dc
            if 0 <= r < n and 0 <= c < m and image[r][c] == oldColor:
                image[r][c] = newColor  # Mark as visited
                queue.append((r, c))
    
    return image
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), as the queue can store up to `n * m` pixels.

---

### **Medium Problems**

#### 4. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
**Problem**: Find the minimum time required to rot all oranges in a grid.

**Approach**:
1. Use BFS to simulate the rotting process.
2. Track the time taken for each orange to rot.

**Brute Force (BFS)**:
```python
from collections import deque

def orangesRotting(grid):
    if not grid:
        return -1
    
    n, m = len(grid), len(grid[0])
    queue = deque()
    fresh = 0
    
    # Initialize queue with rotten oranges and count fresh oranges
    for i in range(n):
        for j in range(m):
            if grid[i][j] == 2:
                queue.append((i, j, 0))  # (row, col, time)
            elif grid[i][j] == 1:
                fresh += 1
    
    if fresh == 0:
        return 0
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    max_time = 0
    
    while queue:
        row, col, time = queue.popleft()
        max_time = max(max_time, time)
        
        for dr, dc in directions:
            r, c = row + dr, col + dc
            if 0 <= r < n and 0 <= c < m and grid[r][c] == 1:
                grid[r][c] = 2  # Mark as rotten
                fresh -= 1
                queue.append((r, c, time + 1))
    
    return max_time if fresh == 0 else -1
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), as the queue can store up to `n * m` cells.

---

#### 5. [01 Matrix](https://leetcode.com/problems/01-matrix/)
**Problem**: Find the distance of the nearest `0` for each cell in a binary matrix.

**Approach**:
1. Use BFS starting from all `0`s.
2. Propagate the distance to neighboring cells.

**Brute Force (BFS)**:
```python
from collections import deque

def updateMatrix(mat):
    if not mat:
        return []
    
    n, m = len(mat), len(mat[0])
    queue = deque()
    
    # Initialize queue with all 0s and mark others as unvisited
    for i in range(n):
        for j in range(m):
            if mat[i][j] == 0:
                queue.append((i, j))
            else:
                mat[i][j] = float('inf')
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while queue:
        row, col = queue.popleft()
        for dr, dc in directions:
            r, c = row + dr, col + dc
            if 0 <= r < n and 0 <= c < m and mat[r][c] > mat[row][col] + 1:
                mat[r][c] = mat[row][col] + 1
                queue.append((r, c))
    
    return mat
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), as the queue can store up to `n * m` cells.

---

#### 6. [Walls and Gates](https://leetcode.com/problems/walls-and-gates/)
**Problem**: Fill each empty room with the distance to the nearest gate.

**Approach**:
1. Use BFS starting from all gates.
2. Propagate the distance to neighboring rooms.

**Brute Force (BFS)**:
```python
from collections import deque

def wallsAndGates(rooms):
    if not rooms:
        return
    
    n, m = len(rooms), len(rooms[0])
    queue = deque()
    
    # Initialize queue with all gates
    for i in range(n):
        for j in range(m):
            if rooms[i][j] == 0:
                queue.append((i, j))
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while queue:
        row, col = queue.popleft()
        for dr, dc in directions:
            r, c = row + dr, col + dc
            if 0 <= r < n and 0 <= c < m and rooms[r][c] == 2147483647:
                rooms[r][c] = rooms[row][col] + 1
                queue.append((r, c))
```
- **Time Complexity**: O(n * m), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O(n * m), as the queue can store up to `n * m` cells.

---

### **Hard Problems**

#### 7. [Shortest Path to Get All Keys](https://leetcode.com/problems/shortest-path-to-get-all-keys/)
**Problem**: Find the shortest path to collect all keys in a grid.

**Approach**:
1. Use BFS with a state to track collected keys.
2. Avoid revisiting cells with the same state.

**Brute Force (BFS with State)**:
```python
from collections import deque

def shortestPathAllKeys(grid):
    if not grid:
        return -1
    
    n, m = len(grid), len(grid[0])
    start = None
    keys = 0
    
    # Find the starting position and total number of keys
    for i in range(n):
        for j in range(m):
            if grid[i][j] == '@':
                start = (i, j)
            elif grid[i][j] in 'abcdef':
                keys += 1
    
    if not start:
        return -1
    
    target = (1 << keys) - 1  # Bitmask for all keys collected
    queue = deque([(start[0], start[1], 0, 0)])  # (row, col, steps, state)
    visited = set()
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while queue:
        row, col, steps, state = queue.popleft()
        
        if state == target:
            return steps
        
        if (row, col, state) in visited:
            continue
        visited.add((row, col, state))
        
        for dr, dc in directions:
            r, c = row + dr, col + dc
            if 0 <= r < n and 0 <= c < m and grid[r][c] != '#':
                cell = grid[r][c]
                if cell in 'ABCDEF' and not (state & (1 << (ord(cell) - ord('A')))):
                    continue  # Locked door and no key
                new_state = state
                if cell in 'abcdef':
                    new_state |= (1 << (ord(cell) - ord('a')))  # Collect key
                queue.append((r, c, steps + 1, new_state))
    
    return -1
```
- **Time Complexity**: O(n * m * 2^k), where `n` is the number of rows, `m` is the number of columns, and `k` is the number of keys.
- **Space Complexity**: O(n * m * 2^k), as the queue and visited set can store up to `n * m * 2^k` states.

---

#### 8. [Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/)
**Problem**: Find the minimum number of moves to solve a 2x3 sliding puzzle.

**Approach**:
1. Use BFS to explore all possible states of the puzzle.
2. Track the number of moves taken to reach each state.

**Brute Force (BFS)**:
```python
from collections import deque

def slidingPuzzle(board):
    target = "123450"
    start = "".join(str(cell) for row in board for cell in row)
    
    if start == target:
        return 0
    
    queue = deque([(start, start.index('0'), 0)])  # (state, zero_index, moves)
    visited = set([start])
    
    # Possible moves for each position of the zero
    moves = {
        0: [1, 3],
        1: [0, 2, 4],
        2: [1, 5],
        3: [0, 4],
        4: [1, 3, 5],
        5: [2, 4]
    }
    
    while queue:
        state, zero, steps = queue.popleft()
        
        for move in moves[zero]:
            new_state = list(state)
            new_state[zero], new_state[move] = new_state[move], new_state[zero]
            new_state = "".join(new_state)
            
            if new_state == target:
                return steps + 1
            
            if new_state not in visited:
                visited.add(new_state)
                queue.append((new_state, move, steps + 1))
    
    return -1
```
- **Time Complexity**: O((n * m)!), where `n` is the number of rows and `m` is the number of columns.
- **Space Complexity**: O((n * m)!), as the queue and visited set can store up to `(n * m)!` states.

---

#### 9. [Bus Routes](https://leetcode.com/problems/bus-routes/)
**Problem**: Find the minimum number of buses required to travel from a source to a target stop.

**Approach**:
1. Use BFS to explore all possible bus routes.
2. Track the number of buses taken to reach each stop.

**Brute Force (BFS)**:
```python
from collections import deque, defaultdict

def numBusesToDestination(routes, source, target):
    if source == target:
        return 0
    
    stop_to_routes = defaultdict(set)
    for i, route in enumerate(routes):
        for stop in route:
            stop_to_routes[stop].add(i)
    
    queue = deque([(source, 0)])
    visited_stops = set([source])
    visited_routes = set()
    
    while queue:
        stop, buses = queue.popleft()
        
        for route_idx in stop_to_routes[stop]:
            if route_idx not in visited_routes:
                visited_routes.add(route_idx)
                for next_stop in routes[route_idx]:
                    if next_stop == target:
                        return buses + 1
                    if next_stop not in visited_stops:
                        visited_stops.add(next_stop)
                        queue.append((next_stop, buses + 1))
    
    return -1
```
- **Time Complexity**: O(n * m), where `n` is the number of routes and `m` is the maximum number of stops in a route.
- **Space Complexity**: O(n * m), as the queue and visited sets can store up to `n * m` stops and routes.

---

### **Summary of Python Implementations**
1. **BFS**: The standard approach for shortest path problems in unweighted grids.
2. **BFS with State**: Used for problems involving multiple states (e.g., keys, puzzle configurations).
3. **Optimized BFS**: Uses additional data structures (e.g., bitmasking) to handle complex constraints.
