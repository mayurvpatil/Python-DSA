
---

### **Easy Problems**

#### 1. [Course Schedule](https://leetcode.com/problems/course-schedule/)
**Problem**: Determine if it is possible to finish all courses given prerequisites.

**Approach**:
1. Use Kahn’s Algorithm to perform topological sorting.
2. Detect if there is a cycle in the graph.

**Brute Force (DFS with Cycle Detection)**:
```python
def canFinish(numCourses, prerequisites):
    from collections import defaultdict

    graph = defaultdict(list)
    for u, v in prerequisites:
        graph[u].append(v)
    
    visited = [0] * numCourses  # 0: unvisited, 1: visiting, 2: visited

    def hasCycle(node):
        if visited[node] == 1:
            return True
        if visited[node] == 2:
            return False
        visited[node] = 1
        for neighbor in graph[node]:
            if hasCycle(neighbor):
                return True
        visited[node] = 2
        return False

    for i in range(numCourses):
        if hasCycle(i):
            return False
    return True
```
- **Time Complexity**: O(V + E), where `V` is the number of courses and `E` is the number of prerequisites.
- **Space Complexity**: O(V + E), as we store the graph and recursion stack.

**Optimized (Kahn’s Algorithm)**:
```python
def canFinish(numCourses, prerequisites):
    from collections import defaultdict, deque

    graph = defaultdict(list)
    in_degree = [0] * numCourses
    for u, v in prerequisites:
        graph[v].append(u)
        in_degree[u] += 1
    
    queue = deque([i for i in range(numCourses) if in_degree[i] == 0])
    count = 0

    while queue:
        node = queue.popleft()
        count += 1
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    return count == numCourses
```
- **Time Complexity**: O(V + E), where `V` is the number of courses and `E` is the number of prerequisites.
- **Space Complexity**: O(V + E), as we store the graph and queue.

---

#### 2. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
**Problem**: Find the order of courses to finish all courses given prerequisites.

**Approach**:
1. Use Kahn’s Algorithm to perform topological sorting.
2. Return the topological order if it exists.

**Brute Force (DFS with Cycle Detection)**:
```python
def findOrder(numCourses, prerequisites):
    from collections import defaultdict

    graph = defaultdict(list)
    for u, v in prerequisites:
        graph[u].append(v)
    
    visited = [0] * numCourses  # 0: unvisited, 1: visiting, 2: visited
    order = []

    def hasCycle(node):
        if visited[node] == 1:
            return True
        if visited[node] == 2:
            return False
        visited[node] = 1
        for neighbor in graph[node]:
            if hasCycle(neighbor):
                return True
        visited[node] = 2
        order.append(node)
        return False

    for i in range(numCourses):
        if hasCycle(i):
            return []
    return order[::-1]
```
- **Time Complexity**: O(V + E), where `V` is the number of courses and `E` is the number of prerequisites.
- **Space Complexity**: O(V + E), as we store the graph and recursion stack.

**Optimized (Kahn’s Algorithm)**:
```python
def findOrder(numCourses, prerequisites):
    from collections import defaultdict, deque

    graph = defaultdict(list)
    in_degree = [0] * numCourses
    for u, v in prerequisites:
        graph[v].append(u)
        in_degree[u] += 1
    
    queue = deque([i for i in range(numCourses) if in_degree[i] == 0])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    return order if len(order) == numCourses else []
```
- **Time Complexity**: O(V + E), where `V` is the number of courses and `E` is the number of prerequisites.
- **Space Complexity**: O(V + E), as we store the graph and queue.

---

#### 3. [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)
**Problem**: Given a sorted dictionary of an alien language, find the order of characters.

**Approach**:
1. Build a graph from the given words.
2. Use Kahn’s Algorithm to perform topological sorting.

**Brute Force (DFS with Cycle Detection)**:
```python
def alienOrder(words):
    from collections import defaultdict

    graph = defaultdict(set)
    in_degree = {char: 0 for word in words for char in word}

    for i in range(len(words) - 1):
        word1, word2 = words[i], words[i + 1]
        for j in range(min(len(word1), len(word2))):
            if word1[j] != word2[j]:
                if word2[j] not in graph[word1[j]]:
                    graph[word1[j]].add(word2[j])
                    in_degree[word2[j]] += 1
                break
        else:
            if len(word1) > len(word2):
                return ""
    
    order = []
    stack = [char for char in in_degree if in_degree[char] == 0]

    while stack:
        char = stack.pop()
        order.append(char)
        for neighbor in graph[char]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                stack.append(neighbor)
    
    return "".join(order) if len(order) == len(in_degree) else ""
```
- **Time Complexity**: O(C), where `C` is the total number of characters in all words.
- **Space Complexity**: O(1), as the number of unique characters is limited.

---

### **Medium Problems**

#### 4. [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)
**Problem**: Find the root of the minimum height trees in a graph.

**Approach**:
1. Use Kahn’s Algorithm to perform topological sorting.
2. Remove leaves layer by layer until one or two nodes remain.

**Optimized (Kahn’s Algorithm)**:
```python
def findMinHeightTrees(n, edges):
    from collections import defaultdict, deque

    if n == 1:
        return [0]
    
    graph = defaultdict(set)
    in_degree = [0] * n
    for u, v in edges:
        graph[u].add(v)
        graph[v].add(u)
        in_degree[u] += 1
        in_degree[v] += 1
    
    queue = deque([i for i in range(n) if in_degree[i] == 1])
    remaining_nodes = n

    while remaining_nodes > 2:
        size = len(queue)
        remaining_nodes -= size
        for _ in range(size):
            node = queue.popleft()
            for neighbor in graph[node]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 1:
                    queue.append(neighbor)
    
    return list(queue)
```
- **Time Complexity**: O(V + E), where `V` is the number of nodes and `E` is the number of edges.
- **Space Complexity**: O(V + E), as we store the graph and queue.

---

#### 5. [Sequence Reconstruction](https://leetcode.com/problems/sequence-reconstruction/)
**Problem**: Check if a sequence can be uniquely reconstructed from a list of subsequences.

**Approach**:
1. Use Kahn’s Algorithm to perform topological sorting.
2. Ensure the topological order matches the given sequence.

**Optimized (Kahn’s Algorithm)**:
```python
def sequenceReconstruction(org, seqs):
    from collections import defaultdict, deque

    graph = defaultdict(set)
    in_degree = {num: 0 for num in org}
    for seq in seqs:
        for i in range(len(seq) - 1):
            if seq[i] not in in_degree or seq[i + 1] not in in_degree:
                return False
            if seq[i + 1] not in graph[seq[i]]:
                graph[seq[i]].add(seq[i + 1])
                in_degree[seq[i + 1]] += 1
    
    queue = deque([num for num in in_degree if in_degree[num] == 0])
    order = []

    while queue:
        if len(queue) > 1:
            return False
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    return order == org
```
- **Time Complexity**: O(V + E), where `V` is the number of unique numbers and `E` is the number of edges.
- **Space Complexity**: O(V + E), as we store the graph and queue.

---

#### 6. [Parallel Courses](https://leetcode.com/problems/parallel-courses/)
**Problem**: Find the minimum number of semesters to finish all courses given prerequisites.

**Approach**:
1. Use Kahn’s Algorithm to perform topological sorting.
2. Track the number of semesters required.

**Optimized (Kahn’s Algorithm)**:
```python
def minimumSemesters(n, relations):
    from collections import defaultdict, deque

    graph = defaultdict(list)
    in_degree = [0] * (n + 1)
    for u, v in relations:
        graph[u].append(v)
        in_degree[v] += 1
    
    queue = deque([i for i in range(1, n + 1) if in_degree[i] == 0])
    semesters = 0
    count = 0

    while queue:
        size = len(queue)
        semesters += 1
        for _ in range(size):
            node = queue.popleft()
            count += 1
            for neighbor in graph[node]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 0:
                    queue.append(neighbor)
    
    return semesters if count == n else -1
```
- **Time Complexity**: O(V + E), where `V` is the number of courses and `E` is the number of prerequisites.
- **Space Complexity**: O(V + E), as we store the graph and queue.

---

### **Hard Problems**

#### 7. [Course Schedule III](https://leetcode.com/problems/course-schedule-iii/)
**Problem**: Find the maximum number of courses that can be taken given their duration and deadlines.

**Approach**:
1. Use a greedy approach with a max-heap to prioritize courses with the earliest deadlines.
2. Adjust the schedule if a course cannot be completed on time.

**Optimized (Greedy + Max-Heap)**:
```python
import heapq

def scheduleCourse(courses):
    courses.sort(key=lambda x: x[1])  # Sort by deadline
    heap = []
    time = 0
    for duration, end in courses:
        if time + duration <= end:
            heapq.heappush(heap, -duration)
            time += duration
        elif heap and -heap[0] > duration:
            time += duration + heapq.heappop(heap)
            heapq.heappush(heap, -duration)
    return len(heap)
```
- **Time Complexity**: O(n log n), where `n` is the number of courses.
- **Space Complexity**: O(n), as we store the heap.

---

#### 8. [Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)
**Problem**: Reconstruct the itinerary given a list of tickets.

**Approach**:
1. Use Kahn’s Algorithm to perform topological sorting.
2. Ensure the itinerary is lexicographically smallest.

**Optimized (Kahn’s Algorithm)**:
```python
def findItinerary(tickets):
    from collections import defaultdict, deque

    graph = defaultdict(list)
    for u, v in sorted(tickets)[::-1]:
        graph[u].append(v)
    
    stack = ["JFK"]
    itinerary = []

    while stack:
        while graph[stack[-1]]:
            stack.append(graph[stack[-1]].pop())
        itinerary.append(stack.pop())
    
    return itinerary[::-1]
```
- **Time Complexity**: O(E log E), where `E` is the number of tickets.
- **Space Complexity**: O(E), as we store the graph and stack.

---

#### 9. [Parallel Courses II](https://leetcode.com/problems/parallel-courses-ii/)
**Problem**: Find the minimum number of semesters to finish all courses given prerequisites and a limit on the number of courses per semester.

**Approach**:
1. Use Kahn’s Algorithm to perform topological sorting.
2. Track the number of semesters required while respecting the limit.

**Optimized (Kahn’s Algorithm)**:
```python
def minimumSemesters(n, relations, k):
    from collections import defaultdict, deque

    graph = defaultdict(list)
    in_degree = [0] * (n + 1)
    for u, v in relations:
        graph[u].append(v)
        in_degree[v] += 1
    
    queue = deque([i for i in range(1, n + 1) if in_degree[i] == 0])
    semesters = 0
    count = 0

    while queue:
        size = min(len(queue), k)
        semesters += 1
        for _ in range(size):
            node = queue.popleft()
            count += 1
            for neighbor in graph[node]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 0:
                    queue.append(neighbor)
    
    return semesters if count == n else -1
```
- **Time Complexity**: O(V + E), where `V` is the number of courses and `E` is the number of prerequisites.
- **Space Complexity**: O(V + E), as we store the graph and queue.

---

### **Summary of Python Implementations**
1. **Kahn’s Algorithm**: The standard approach for topological sorting.
2. **DFS with Cycle Detection**: Used for cycle detection in graphs.
3. **Greedy + Max-Heap**: Used for scheduling problems with constraints.

