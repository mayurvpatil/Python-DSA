
---

### **Detect Cycle Problems**

#### **Easy**
1. **[Redundant Connection](https://leetcode.com/problems/redundant-connection/)**
   - **Problem**: Given a graph with `n` nodes labeled from `1` to `n`, and a list of edges, return an edge that can be removed to make the graph a tree.
   - **Approach**:
     - Brute Force: Use DFS to detect cycles.
     - Optimized: Use Union-Find (Disjoint Set) to detect cycles efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n \cdot \alpha(n))\), where \(\alpha(n)\) is the inverse Ackermann function.
   - **Space Complexity**:
     - Brute Force: \(O(n)\)
     - Optimized: \(O(n)\)

   ```python
   def findRedundantConnection(edges):
       parent = [i for i in range(len(edges) + 1)]
       def find(x):
           if parent[x] != x:
               parent[x] = find(parent[x])
           return parent[x]
       def union(x, y):
           parent[find(x)] = find(y)
       for u, v in edges:
           if find(u) == find(v):
               return [u, v]
           union(u, v)
       return []
   ```

2. **[Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)**
   - **Problem**: Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges, check if these edges form a valid tree.
   - **Approach**:
     - Brute Force: Use DFS to detect cycles and check connectivity.
     - Optimized: Use Union-Find (Disjoint Set) to detect cycles and check connectivity.
   - **Time Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n \cdot \alpha(n))\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\)
     - Optimized: \(O(n)\)

   ```python
   def validTree(n, edges):
       if len(edges) != n - 1:
           return False
       parent = [i for i in range(n)]
       def find(x):
           if parent[x] != x:
               parent[x] = find(parent[x])
           return parent[x]
       def union(x, y):
           parent[find(x)] = find(y)
       for u, v in edges:
           if find(u) == find(v):
               return False
           union(u, v)
       return True
   ```

3. **[Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)**
   - **Problem**: Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges, return the number of connected components.
   - **Approach**:
     - Brute Force: Use DFS to count connected components.
     - Optimized: Use Union-Find (Disjoint Set) to count connected components.
   - **Time Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n \cdot \alpha(n))\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\)
     - Optimized: \(O(n)\)

   ```python
   def countComponents(n, edges):
       parent = [i for i in range(n)]
       def find(x):
           if parent[x] != x:
               parent[x] = find(parent[x])
           return parent[x]
       def union(x, y):
           parent[find(x)] = find(y)
       for u, v in edges:
           if find(u) != find(v):
               union(u, v)
               n -= 1
       return n
   ```

---

#### **Medium**
4. **[Redundant Connection II](https://leetcode.com/problems/redundant-connection-ii/)**
   - **Problem**: Given a directed graph with `n` nodes labeled from `1` to `n`, and a list of directed edges, return an edge that can be removed to make the graph a tree.
   - **Approach**:
     - Brute Force: Use DFS to detect cycles.
     - Optimized: Use Union-Find (Disjoint Set) to detect cycles efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n \cdot \alpha(n))\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\)
     - Optimized: \(O(n)\)

   ```python
   def findRedundantDirectedConnection(edges):
       parent = [i for i in range(len(edges) + 1)]
       def find(x):
           if parent[x] != x:
               parent[x] = find(parent[x])
           return parent[x]
       def union(x, y):
           parent[find(x)] = find(y)
       candidate1, candidate2 = None, None
       for u, v in edges:
           if find(v) != v:
               candidate1, candidate2 = [u, v], [parent[v], v]
               break
           union(u, v)
       parent = [i for i in range(len(edges) + 1)]
       for u, v in edges:
           if [u, v] == candidate2:
               continue
           if find(u) == find(v):
               return candidate1 if candidate1 else [u, v]
           union(u, v)
       return candidate2
   ```

5. **[Accounts Merge](https://leetcode.com/problems/accounts-merge/)**
   - **Problem**: Given a list of accounts where each account contains a name and a list of emails, merge accounts that share common emails.
   - **Approach**:
     - Brute Force: Use DFS to merge accounts.
     - Optimized: Use Union-Find (Disjoint Set) to merge accounts efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n \cdot \alpha(n))\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\)
     - Optimized: \(O(n)\)

   ```python
   def accountsMerge(accounts):
       parent = {}
       def find(x):
           if parent[x] != x:
               parent[x] = find(parent[x])
           return parent[x]
       def union(x, y):
           parent[find(x)] = find(y)
       email_to_name = {}
       for account in accounts:
           name = account[0]
           for email in account[1:]:
               if email not in parent:
                   parent[email] = email
               email_to_name[email] = name
               union(account[1], email)
       merged = {}
       for email in parent:
           root = find(email)
           if root not in merged:
               merged[root] = []
           merged[root].append(email)
       return [[email_to_name[root]] + sorted(emails) for root, emails in merged.items()]
   ```

6. **[Regions Cut By Slashes](https://leetcode.com/problems/regions-cut-by-slashes/)**
   - **Problem**: Given a grid of slashes (`/`, `\`), and spaces, determine the number of regions formed.
   - **Approach**:
     - Brute Force: Use DFS to count regions.
     - Optimized: Use Union-Find (Disjoint Set) to count regions efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n^2 \cdot \alpha(n))\)
   - **Space Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n^2)\)

   ```python
   def regionsBySlashes(grid):
       n = len(grid)
       parent = [i for i in range(4 * n * n)]
       def find(x):
           if parent[x] != x:
               parent[x] = find(parent[x])
           return parent[x]
       def union(x, y):
           parent[find(x)] = find(y)
       for i in range(n):
           for j in range(n):
               index = 4 * (i * n + j)
               if grid[i][j] == '/':
                   union(index, index + 3)
                   union(index + 1, index + 2)
               elif grid[i][j] == '\\':
                   union(index, index + 1)
                   union(index + 2, index + 3)
               else:
                   union(index, index + 1)
                   union(index + 1, index + 2)
                   union(index + 2, index + 3)
               if i > 0:
                   union(index, index - 4 * n + 2)
               if j > 0:
                   union(index + 3, index - 4 + 1)
       return len(set(find(x) for x in range(4 * n * n)))
   ```

---

#### **Hard**
7. **[Bricks Falling When Hit](https://leetcode.com/problems/bricks-falling-when-hit/)**
   - **Problem**: Given a grid representing bricks, and a list of hits, determine how many bricks fall after each hit.
   - **Approach**:
     - Brute Force: Simulate each hit and count falling bricks.
     - Optimized: Use Union-Find (Disjoint Set) to track connected components.
   - **Time Complexity**:
     - Brute Force: \(O(n^2 \cdot h)\), where \(h\) is the number of hits.
     - Optimized: \(O(n^2 \cdot \alpha(n))\)
   - **Space Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n^2)\)

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
           xroot, yroot = find(x), find(y)
           if xroot == yroot:
               return
           if size[xroot] < size[yroot]:
               xroot, yroot = yroot, xroot
           parent[yroot] = xroot
           size[xroot] += size[yroot]
       for i, j in hits:
           grid[i][j] -= 1
       for i in range(m):
           for j in range(n):
               if grid[i][j] == 1:
                   if i == 0:
                       union(j, m * n)
                   if i > 0 and grid[i - 1][j] == 1:
                       union(i * n + j, (i - 1) * n + j)
                   if j > 0 and grid[i][j - 1] == 1:
                       union(i * n + j, i * n + j - 1)
       result = []
       for i, j in reversed(hits):
           grid[i][j] += 1
           if grid[i][j] == 1:
               prev_size = size[find(m * n)]
               if i == 0:
                   union(j, m * n)
               if i > 0 and grid[i - 1][j] == 1:
                   union(i * n + j, (i - 1) * n + j)
               if j > 0 and grid[i][j - 1] == 1:
                   union(i * n + j, i * n + j - 1)
               if j < n - 1 and grid[i][j + 1] == 1:
                   union(i * n + j, i * n + j + 1)
               if i < m - 1 and grid[i + 1][j] == 1:
                   union(i * n + j, (i + 1) * n + j)
               new_size = size[find(m * n)]
               result.append(max(0, new_size - prev_size - 1))
           else:
               result.append(0)
       return result[::-1]
   ```

8. **[Remove Max Number of Edges to Keep Graph Fully Traversable](https://leetcode.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable/)**
   - **Problem**: Given a graph with `n` nodes and three types of edges, remove the maximum number of edges while keeping the graph fully traversable.
   - **Approach**:
     - Brute Force: Try all combinations of edges.
     - Optimized: Use Union-Find (Disjoint Set) to prioritize edges.
   - **Time Complexity**:
     - Brute Force: \(O(2^m)\)
     - Optimized: \(O(m \cdot \alpha(n))\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\)
     - Optimized: \(O(n)\)

   ```python
   def maxNumEdgesToRemove(n, edges):
       parent = [i for i in range(n + 1)]
       def find(x):
           if parent[x] != x:
               parent[x] = find(parent[x])
           return parent[x]
       def union(x, y):
           parent[find(x)] = find(y)
       edges.sort(reverse=True)
       count = 0
       for type, u, v in edges:
           if type == 3:
               if find(u) != find(v):
                   union(u, v)
               else:
                   count += 1
       parent_copy = parent[:]
       for type, u, v in edges:
           if type == 1:
               if find(u) != find(v):
                   union(u, v)
               else:
                   count += 1
       parent = parent_copy
       for type, u, v in edges:
           if type == 2:
               if find(u) != find(v):
                   union(u, v)
               else:
                   count += 1
       root = find(1)
       for i in range(2, n + 1):
           if find(i) != root:
               return -1
       return count
   ```

9. **[Smallest String With Swaps](https://leetcode.com/problems/smallest-string-with-swaps/)**
   - **Problem**: Given a string and a list of pairs of indices, return the lexicographically smallest string after any number of swaps.
   - **Approach**:
     - Brute Force: Try all possible swaps.
     - Optimized: Use Union-Find (Disjoint Set) to group indices and sort characters within each group.
   - **Time Complexity**:
     - Brute Force: \(O(n!)\)
     - Optimized: \(O(n \log n)\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\)
     - Optimized: \(O(n)\)

   ```python
   def smallestStringWithSwaps(s, pairs):
       parent = [i for i in range(len(s))]
       def find(x):
           if parent[x] != x:
               parent[x] = find(parent[x])
           return parent[x]
       def union(x, y):
           parent[find(x)] = find(y)
       for u, v in pairs:
           union(u, v)
       groups = {}
       for i in range(len(s)):
           root = find(i)
           if root not in groups:
               groups[root] = []
           groups[root].append(i)
       result = list(s)
       for group in groups.values():
           chars = sorted([s[i] for i in group])
           for i, char in zip(sorted(group), chars):
               result[i] = char
       return "".join(result)
   ```

---

### **Summary of Approaches**
1. **Brute Force**:
   - Use DFS or try all combinations to detect cycles or solve the problem.
   - Time: \(O(n^2)\) to \(O(n!)\), Space: \(O(n)\).

2. **Optimized Union-Find**:
   - Use Union-Find (Disjoint Set) to detect cycles or solve the problem efficiently.
   - Time: \(O(n \cdot \alpha(n))\), Space: \(O(n)\).

3. **Path Compression and Union by Rank**:
   - Optimize Union-Find with path compression and union by rank.
   - Time: \(O(n \cdot \alpha(n))\), Space: \(O(n)\).

---

