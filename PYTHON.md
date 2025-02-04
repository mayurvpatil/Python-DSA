

### 1. **List (Dynamic Array)**
```python
arr = [1, 2, 3]
arr.append(4)       # TC: O(1)* (amortized)
arr.pop()           # TC: O(1)
arr.insert(1, 10)   # TC: O(n)
arr[0]              # TC: O(1)
```
- **Slicing**: `arr[start:end:step]` → TC/SC: O(k) (k = sliced elements)
  ```python
  arr = [1, 2, 3, 4, 5]
  print(arr[1:4])       # [2, 3, 4]
  print(arr[::-1])      # Reverse: [5, 4, 3, 2, 1]
  ```

---

### 2. **Heap (Priority Queue)**
- **Min-Heap** (default):
  ```python
  import heapq
  heap = []
  heapq.heappush(heap, 3)    # TC: O(log n)
  heapq.heappop(heap)        # TC: O(log n)
  heapq.heapify(arr)         # TC: O(n)
  ```
- **Max-Heap**: Invert values using `-1` (store `-x`, retrieve `-x`).
  ```python
  heapq.heappush(heap, -x)   # Push as -x for max-heap effect
  max_val = -heapq.heappop(heap)
  ```

---

### 3. **Binary Search**
```python
import bisect
arr = [1, 3, 5, 7]
# Find insertion point for target (leftmost)
index = bisect.bisect_left(arr, 6)   # TC: O(log n), returns 3
```
- **Custom Binary Search**:
  ```python
  low, high = 0, len(arr)-1
  while low <= high:
      mid = (low + high) // 2
      if arr[mid] == target: ...
  ```

---

### 4. **Dictionary (Hash Map)**
```python
d = {}
d = {'a': 1, 'b': 2}
d['c'] = 3              # TC: O(1) average
if 'a' in d: ...        # TC: O(1)
for key, val in d.items(): ...
```
- **Default Dict** (auto-initialize keys):
  ```python
  from collections import defaultdict
  d = defaultdict(int)  # Default to 0 for missing keys
  ```

---

### 5. **Tree BFS (Level Order)**
```python
from collections import deque
def bfs(root):
    queue = deque([root])
    while queue:
        node = queue.popleft()   # TC: O(1)
        if node.left: queue.append(node.left)
        if node.right: queue.append(node.right)
```
- **TC**: O(n), **SC**: O(n) (worst case for a full level)

---

### 6. **Tree DFS (Pre/In/Post Order)**
- **Iterative Pre-Order** (using a stack):
  ```python
  stack = [root]
  while stack:
      node = stack.pop()         # TC: O(1)
      if node.right: stack.append(node.right)
      if node.left: stack.append(node.left)
  ```
- **Recursive In-Order**:
  ```python
  def dfs(node):
      if not node: return
      dfs(node.left)
      print(node.val)
      dfs(node.right)
  ```
  - **TC**: O(n), **SC**: O(h) (height of tree; worst case O(n))

---

### 7. **Stack & Queue**
- **Stack** (LIFO):
  ```python
  stack = []
  stack.append(2)    # Push, TC: O(1)
  stack.pop()        # Pop, TC: O(1)
  ```
- **Queue** (FIFO):
  ```python
  from collections import deque
  q = deque()
  q.append(2)        # Enqueue, TC: O(1)
  q.popleft()        # Dequeue, TC: O(1)
  ```

---

### 8. **Deque (Double-Ended Queue)**
```python
from collections import deque
dq = deque([1, 2, 3])
dq.appendleft(0)    # TC: O(1)
dq.pop()            # TC: O(1)
dq.popleft()        # TC: O(1)
```

---

### **The `bisect` Module**
Python’s `bisect` module simplifies binary search. Here’s how it works:

#### 1. **`bisect_left(arr, target)`**
   - Returns the **leftmost insertion index** for `target` in `arr` (sorted).
   - If `target` exists in `arr`, it returns the index of the **first occurrence**.
   - If `target` doesn’t exist, it returns the index where `target` should be inserted.

   **Example:**
   ```python
   import bisect
   arr = [1, 3, 3, 5, 8]
   # Target exists (first occurrence)
   print(bisect.bisect_left(arr, 3))   # Output: 1
   # Target doesn't exist
   print(bisect.bisect_left(arr, 6))   # Output: 4 (insert between 5 and 8)
   ```

#### 2. **`bisect_right(arr, target)`**
   - Returns the **rightmost insertion index** for `target` in `arr`.
   - If `target` exists, it returns the index **after the last occurrence**.
   - If `target` doesn’t exist, behaves like `bisect_left`.

   **Example:**
   ```python
   arr = [1, 3, 3, 5, 8]
   # Target exists (after last occurrence)
   print(bisect.bisect_right(arr, 3))  # Output: 3
   # Target doesn't exist
   print(bisect.bisect_right(arr, 6))  # Output: 4 (same as bisect_left)
   ```

---

### **Key Differences**
| Case                | `bisect_left`                    | `bisect_right`                   |
|---------------------|----------------------------------|----------------------------------|
| **Target exists**   | First occurrence index           | Index after last occurrence      |
| **Target missing**  | Insertion point to the left      | Same as `bisect_left`            |

- **Time Complexity**: O(log n) for both functions.
- **Space Complexity**: O(1) (no extra space needed).

---

### **When to Use Which?**
- Use `bisect_left` for:
  - Finding the first occurrence of `target`.
  - Inserting `target` at the leftmost valid position.
- Use `bisect_right` for:
  - Finding the count of elements ≤ `target`.
  - Inserting `target` at the rightmost valid position.

---

### **Manual Binary Search Implementation**
To understand how `bisect_left` works under the hood:
```python
def binary_search_left(arr, target):
    low, high = 0, len(arr) - 1
    while low <= high:
        mid = (low + high) // 2
        if arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return low  # Insertion point (or first occurrence)
```

**Example Walkthrough**:
```python
arr = [1, 3, 3, 5, 8]
target = 3
# Step-by-step:
# Initial: low=0, high=4 → mid=2 → arr[2]=3 → move high to 1
# Next: low=0, high=1 → mid=0 → arr[0]=1 < 3 → move low to 1
# Next: low=1, high=1 → mid=1 → arr[1]=3 → move high to 0
# Loop ends → return low=1 (first occurrence of 3).
```

---

### **Edge Cases**
1. **Empty Array**:
   - Both functions return `0` (insert at the beginning).
2. **All Elements < Target**:
   - Returns `len(arr)` (insert at the end).
3. **All Elements > Target**:
   - Returns `0` (insert at the beginning).

---

### **Why Use `bisect` Over Manual Code?**
- **Conciseness**: Avoid reimplementing binary search.
- **Optimized**: Built-in module is efficient and handles edge cases.
- **Readability**: Clearly expresses intent in interviews.

---

### **Practical Example: Finding a Range**
To find the start and end indices of a target in a sorted array with duplicates:
```python
def search_range(arr, target):
    left = bisect.bisect_left(arr, target)
    right = bisect.bisect_right(arr, target) - 1
    if left <= right:
        return [left, right]
    return [-1, -1]
```

---

### **1. `collections` Module**
Useful for optimized data structures beyond basic lists/dicts.

#### **a. `deque` (Double-Ended Queue)**
- **Use Case**: Efficient FIFO queues (BFS) or LIFO stacks.
- **Java Analogy**: `LinkedList` for queue operations.
- **Example**:
  ```python
  from collections import deque
  q = deque()          # Initialize
  q.append(2)          # Enqueue → TC: O(1)
  q.popleft()          # Dequeue → TC: O(1)
  q.appendleft(1)      # Push to front → TC: O(1)
  ```
  - **TC/SC**: All operations O(1).

#### **b. `Counter`**
- **Use Case**: Frequency counting (anagrams, histogram).
- **Java Analogy**: `HashMap<Character, Integer>`.
- **Example**:
  ```python
  from collections import Counter
  s = "apple"
  count = Counter(s)       # TC: O(n), SC: O(k) (k = unique elements)
  print(count['p'])        # → 2
  print(count.most_common(1))  # → [('p', 2)]
  ```

#### **c. `defaultdict`**
- **Use Case**: Avoid `KeyError` for missing keys (graphs, adjacency lists).
- **Java Analogy**: `Map<K, V>` with default value factories.
- **Example**:
  ```python
  from collections import defaultdict
  graph = defaultdict(list)     # Default: empty list
  graph['A'].append('B')        # No KeyError
  ```

---

### **2. `itertools` Module**
For efficient iteration and combinatorial logic.

#### **a. `permutations`**
- **Use Case**: Generate all possible orderings (backtracking).
- **Example**:
  ```python
  import itertools
  nums = [1, 2, 3]
  perms = itertools.permutations(nums, 2)  # TC: O(nPr), SC: O(r)
  # Output: (1,2), (1,3), (2,1), (2,3), (3,1), (3,2)
  ```

#### **b. `combinations`**
- **Use Case**: Generate subsets (combinations without order).
- **Example**:
  ```python
  combs = itertools.combinations(nums, 2)  # TC: O(nCr), SC: O(r)
  # Output: (1,2), (1,3), (2,3)
  ```

#### **c. `product`**
- **Use Case**: Cartesian product (nested loops).
- **Example**:
  ```python
  axes = itertools.product([0, 1], repeat=2)  # 2D grid
  # Output: (0,0), (0,1), (1,0), (1,1)
  ```

#### **d. `chain`**
- **Use Case**: Merge multiple iterables (flattening).
- **Example**:
  ```python
  merged = itertools.chain([1, 2], [3, 4])  # TC: O(1) initialization
  list(merged)  # → [1, 2, 3, 4]
  ```

---

### **3. `heapq` Module**
For priority queue/heap operations.

#### **a. `nlargest`/`nsmallest`**
- **Use Case**: Find top K elements without full sorting.
- **Example**:
  ```python
  import heapq
  nums = [5, 1, 8, 3]
  top2 = heapq.nlargest(2, nums)  # → [8, 5] → TC: O(n log k)
  ```

#### **b. `merge`**
- **Use Case**: Merge sorted iterables (merge K sorted lists).
- **Example**:
  ```python
  list1 = [1, 3, 5]
  list2 = [2, 4, 6]
  merged = heapq.merge(list1, list2)  # TC: O(n) for n total elements
  ```

---

### **4. `math` Module**
For numerical operations.

#### **a. `gcd`/`lcm` (Python 3.9+)**
- **Use Case**: Number theory problems.
- **Example**:
  ```python
  import math
  print(math.gcd(12, 18))   # → 6
  print(math.lcm(4, 6))     # → 12
  ```

#### **b. `isqrt` (Python 3.8+)**
- **Use Case**: Check perfect squares efficiently.
- **Example**:
  ```python
  math.isqrt(25)   # → 5 (exact integer square root)
  ```

---

### **5. `functools` Module**
For functional programming utilities.

#### **a. `lru_cache`**
- **Use Case**: Memoization for recursive DP problems.
- **Example**:
  ```python
  from functools import lru_cache
  @lru_cache(maxsize=None)
  def fib(n):
      if n <= 1: return n
      return fib(n-1) + fib(n-2)
  ```
  - **TC**: Reduces from O(2^n) to O(n).

#### **b. `cmp_to_key`**
- **Use Case**: Custom sorting logic (rare but handy).
- **Example**:
  ```python
  from functools import cmp_to_key
  def compare(a, b):
      return a - b  # Sorts in ascending order
  sorted_nums = sorted([3, 1, 2], key=cmp_to_key(compare))
  ```

---

### **6. `array` Module**
For space-efficient arrays (rarely used in interviews but good to know).

#### **Example**:
```python
import array
arr = array.array('i', [1, 2, 3])  # 'i' = signed integer → SC: O(n)
```

---

### **Solution Code**
```python
# Example lists
list1 = ['a', 'b', 'c', 'd']
list2 = [3, 2, 4, 1]

# Pair elements, sort by list2's values, then unzip
sorted_pairs = sorted(zip(list2, list1), key=lambda x: x[0])
list1_sorted = [x[1] for x in sorted_pairs]
list2_sorted = [x[0] for x in sorted_pairs]

print("Sorted List1:", list1_sorted)  # Output: ['d', 'b', 'a', 'c']
print("Sorted List2:", list2_sorted)  # Output: [1, 2, 3, 4]
```

---

### **Step-by-Step Explanation**
1. **Pair Elements**:
   - Use `zip(list2, list1)` to create tuples of `(value_from_list2, value_from_list1)`.
   - Example: `[(3, 'a'), (2, 'b'), (4, 'c'), (1, 'd')]`.

2. **Sort Pairs**:
   - Sort the paired tuples based on the first element (values from `list2`).
   - After sorting: `[(1, 'd'), (2, 'b'), (3, 'a'), (4, 'c')]`.

3. **Unzip Sorted Pairs**:
   - Extract the sorted `list1` elements and the sorted `list2` values.

---


### **Common Use Cases**
- Sorting names (`list1`) based on their corresponding scores (`list2`).
- Reordering data points (`list1`) according to timestamps (`list2`).

This approach is efficient and leverages Python’s concise syntax for sorting paired data.


The **`zip()`** function in Python is a built-in utility that **combines elements from multiple iterables** (like lists, tuples, strings, etc.) into tuples, pairing elements at the same index. It’s analogous to a zipper merging two sides. Let’s break it down with examples and key behaviors.

---

### **Examples**

#### **1. Basic Zipping (Two Lists)**
Pair elements from two lists:
```python
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]

zipped = zip(names, ages)
print(list(zipped))  # Convert to list to see output
# Output: [('Alice', 25), ('Bob', 30), ('Charlie', 35)]
```

#### **2. Zipping Three Lists**
```python
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
scores = [85, 90, 95]

zipped = zip(names, ages, scores)
print(list(zipped))
# Output: [('Alice', 25, 85), ('Bob', 30, 90), ('Charlie', 35, 95)]
```

#### **3. Different-Length Iterables**
```python
list1 = [1, 2, 3]
list2 = ["a", "b"]

zipped = zip(list1, list2)
print(list(zipped))  # Stops at the shortest iterable
# Output: [(1, 'a'), (2, 'b')]
```

#### **4. Zipping Strings (Character Pairs)**
```python
str1 = "hello"
str2 = "12345"  # Same length as "hello"

zipped = zip(str1, str2)
print(list(zipped))
# Output: [('h', '1'), ('e', '2'), ('l', '3'), ('l', '4'), ('o', '5')]
```

#### **5. Unzipping (Reverse Zipping)**
Use `zip(*zipped)` to separate paired elements:
```python
pairs = [('Alice', 25), ('Bob', 30), ('Charlie', 35)]
names, ages = zip(*pairs)  # Unzip
print(names)  # ('Alice', 'Bob', 'Charlie')
print(ages)   # (25, 30, 35)
```

---

### **Practical Use Cases**

#### **1. Iterating Over Multiple Lists Simultaneously**
```python
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]

for name, age in zip(names, ages):
    print(f"{name} is {age} years old")
# Output:
# Alice is 25 years old
# Bob is 30 years old
# Charlie is 35 years old
```

#### **2. Creating a Dictionary from Two Lists**
```python
keys = ["a", "b", "c"]
values = [1, 2, 3]

my_dict = dict(zip(keys, values))
print(my_dict)  # {'a': 1, 'b': 2, 'c': 3}
```

#### **3. Transposing a Matrix (Rows ↔ Columns)**
```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

transposed = list(zip(*matrix))
print(transposed)
# Output: [(1, 4, 7), (2, 5, 8), (3, 6, 9)]
```

#### **4. Element-wise Operations**
Calculate sums of corresponding elements:
```python
list1 = [1, 2, 3]
list2 = [4, 5, 6]

sums = [x + y for x, y in zip(list1, list2)]
print(sums)  # [5, 7, 9]
```

---

### **Handling Unequal-Length Iterables**
To process all elements (not truncate), use `itertools.zip_longest`:
```python
from itertools import zip_longest

list1 = [1, 2, 3]
list2 = ["a", "b"]

zipped = zip_longest(list1, list2, fillvalue="N/A")
print(list(zipped))  # [(1, 'a'), (2, 'b'), (3, 'N/A')]
```

---

### **Common Pitfalls**
1. **Consuming the Iterator Twice**:
   ```python
   zipped = zip([1, 2], ["a", "b"])
   print(list(zipped))  # [(1, 'a'), (2, 'b')]
   print(list(zipped))  # [] (iterator is exhausted)
   ```
   - **Fix**: Convert to a list first (e.g., `zipped = list(zip(...))`).

2. **Assuming Equal-Length Iterables**:
   - Always check lengths if needed.

---

### **Comparison to Java**
- **Java**: No direct equivalent, but similar to `IntStream.range().mapToObj()` or custom loops.
- **Python**: Cleaner syntax with `zip()` and iterator-based efficiency.

---

### **Key Takeaways**
- Use `zip()` to pair elements from multiple iterables.
- Works seamlessly in loops, dictionaries, and data transformations.
- Use `itertools.zip_longest` for unequal-length iterables.
- Always be mindful of the iterator being consumed once.

This function is indispensable for tasks like data alignment, matrix operations, and paired iterations in Python!\


If you're looking for **similar functions or patterns to `zip()`** in Python, here are some powerful alternatives and related concepts for handling iterables:

---

### **1. `enumerate()` → Pair Elements with Indices**
Adds a counter to an iterable (like a loop index in Java).

```python
names = ["Alice", "Bob", "Charlie"]
for index, name in enumerate(names):
    print(f"Index {index}: {name}")

# Output:
# Index 0: Alice
# Index 1: Bob
# Index 2: Charlie
```

---

### **2. `itertools.product()` → Cartesian Product (Nested Loops)**
Generate all combinations of elements from multiple iterables.

```python
from itertools import product
list1 = [1, 2]
list2 = ['a', 'b']

for num, char in product(list1, list2):
    print(num, char)

# Output:
# 1 a
# 1 b
# 2 a
# 2 b
```

---

### **3. `itertools.chain()` → Merge Iterables**
Concatenate multiple iterables into a single sequence.

```python
from itertools import chain
list1 = [1, 2]
list2 = ['a', 'b']

merged = list(chain(list1, list2))  # [1, 2, 'a', 'b']
```

---

### **4. `map()` → Apply a Function to Elements**
Process elements from one or more iterables using a function.

```python
numbers = [1, 2, 3]
squared = list(map(lambda x: x**2, numbers))  # [1, 4, 9]

# With multiple iterables (like zip):
list1 = [1, 2, 3]
list2 = [4, 5, 6]
sums = list(map(lambda x, y: x + y, list1, list2))  # [5, 7, 9]
```

---

### **5. `itertools.zip_longest()` → Zip Uneven Iterables**
Fill missing values with a placeholder (default: `None`).

```python
from itertools import zip_longest
list1 = [1, 2, 3]
list2 = ['a', 'b']

for pair in zip_longest(list1, list2, fillvalue="N/A"):
    print(pair)

# Output:
# (1, 'a')
# (2, 'b')
# (3, 'N/A')
```

---

### **6. List Comprehensions → Custom Pairing**
Manually pair elements conditionally.

```python
list1 = [1, 2, 3]
list2 = ['a', 'b', 'c']

pairs = [(x, y) for x in list1 for y in list2 if x == list1.index(y)+1]
print(pairs)  # [(1, 'a'), (2, 'b'), (3, 'c')]
```

---

### **7. `dict()` → Create Dictionaries from Paired Iterables**
Convert `zip`ped key-value pairs into a dictionary.

```python
keys = ['a', 'b', 'c']
values = [1, 2, 3]

my_dict = dict(zip(keys, values))  # {'a': 1, 'b': 2, 'c': 3}
```

---

### **8. `numpy` → Vectorized Operations**
For numerical data, use NumPy to handle arrays efficiently.

```python
import numpy as np
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])

sums = arr1 + arr2  # [5, 7, 9]
```

---

### **9. `pandas` → Align Data with Indexes**
Align data using DataFrame indices (like SQL joins).

```python
import pandas as pd
df1 = pd.DataFrame({'A': [1, 2]}, index=['x', 'y'])
df2 = pd.DataFrame({'B': [3, 4]}, index=['x', 'z'])

# Align on index (like a "zip" for DataFrames):
merged = df1.join(df2, how='outer')  # NaN for missing values
```

---

### **When to Use Which?**
| Use Case                          | Tool                     |
|-----------------------------------|--------------------------|
| Iterate with indices              | `enumerate`              |
| Combine lists of unequal lengths  | `itertools.zip_longest`  |
| Cartesian product                 | `itertools.product`      |
| Apply a function to elements      | `map`                    |
| Merge iterables                   | `itertools.chain`        |
| Vectorized math                   | NumPy                    |

---


### **1. Lambda Functions (`lambda`)**
A **lambda** is an anonymous (unnamed) function defined with the keyword `lambda`. It’s ideal for short, single-expression operations.

#### **Syntax**
```python
lambda arguments: expression
```

#### **Examples**
- **Add Two Numbers**:
  ```python
  add = lambda a, b: a + b
  print(add(3, 5))  # Output: 8
  ```
  Equivalent Java code:
  ```java
  // Java (using a functional interface)
  BinaryOperator<Integer> add = (a, b) -> a + b;
  System.out.println(add.apply(3, 5));  // Output: 8
  ```

- **Sort a List of Tuples** (by the second element):
  ```python
  points = [(1, 4), (3, 1), (2, 5)]
  points.sort(key=lambda x: x[1])  # Sort by the second element
  print(points)  # Output: [(3, 1), (1, 4), (2, 5)]
  ```

- **Filter Even Numbers**:
  ```python
  nums = [1, 2, 3, 4, 5]
  evens = list(filter(lambda x: x % 2 == 0, nums))
  print(evens)  # Output: [2, 4]
  ```

---

### **2. List Comprehensions**
A concise way to create lists by applying an expression to each item in an iterable.

#### **Syntax**
```python
[expression for item in iterable if condition]
```

#### **Examples**
- **Square Numbers**:
  ```python
  squares = [x**2 for x in range(5)]
  print(squares)  # Output: [0, 1, 4, 9, 16]
  ```
  Equivalent Java code:
  ```java
  // Java (with loops)
  List<Integer> squares = new ArrayList<>();
  for (int x = 0; x < 5; x++) {
      squares.add(x * x);
  }
  ```

- **Filter Odd Numbers**:
  ```python
  odds = [x for x in range(10) if x % 2 != 0]
  print(odds)  # Output: [1, 3, 5, 7, 9]
  ```

- **Nested Loops** (Cartesian Product):
  ```python
  pairs = [(x, y) for x in [1, 2] for y in ['a', 'b']]
  print(pairs)  # Output: [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]
  ```

---

### **3. Dictionary Comprehensions**
Build dictionaries using a similar syntax.

#### **Syntax**
```python
{key_expr: value_expr for item in iterable if condition}
```

#### **Examples**
- **Map Numbers to Squares**:
  ```python
  square_dict = {x: x**2 for x in range(5)}
  print(square_dict)  # Output: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
  ```

- **Invert a Dictionary** (swap keys and values):
  ```python
  original = {'a': 1, 'b': 2}
  inverted = {v: k for k, v in original.items()}
  print(inverted)  # Output: {1: 'a', 2: 'b'}
  ```

---

### **4. Set Comprehensions**
Create sets (unique elements only).

#### **Syntax**
```python
{expression for item in iterable if condition}
```

#### **Example**
```python
unique_squares = {x**2 for x in [-2, -1, 0, 1, 2]}
print(unique_squares)  # Output: {0, 1, 4}
```

---

### **5. Generator Expressions**
Lazy-evaluated iterables (use `()` instead of `[]` or `{}`).

#### **Syntax**
```python
(expression for item in iterable if condition)
```

#### **Example**
```python
gen = (x**2 for x in range(5))
print(next(gen))  # Output: 0
print(next(gen))  # Output: 1
```

---

### **Key Differences: Lambda vs. Comprehensions**
| Feature               | Lambda Functions                         | Comprehensions                          |
|-----------------------|------------------------------------------|-----------------------------------------|
| **Purpose**           | Define small, anonymous functions        | Create lists/dicts/sets/generators      |
| **Return Type**       | A function object                        | A new iterable (list, dict, etc.)       |
| **Use Case**          | Short logic (e.g., in `map()`/`filter()`) | Transforming/filtering data structures  |
| **Readability**       | Can be cryptic for complex logic         | More readable for data transformations  |

---

### **When to Use Which?**
- **Lambda**:
  - When you need a quick function for `sorted()`, `map()`, or `filter()`.
  - Avoid for multi-step logic (use `def` instead).
- **Comprehensions**:
  - To replace loops for creating lists/dicts/sets.
  - Avoid nested comprehensions for complex logic (use loops).

---

### **Common Pitfalls**
1. **Lambda in Loops**:
   ```python
   # Wrong: All lambdas reference the last value of `i`
   funcs = [lambda: i for i in range(3)]
   print([f() for f in funcs])  # Output: [2, 2, 2]
   
   # Fix: Capture `i` as a default argument
   funcs = [lambda i=i: i for i in range(3)]
   print([f() for f in funcs])  # Output: [0, 1, 2]
   ```

2. **Overusing Nested Comprehensions**:
   ```python
   # Hard to read:
   matrix = [[1, 2], [3, 4]]
   flattened = [num for row in matrix for num in row]
   ```

---

### **Java to Python Translation**
- **Lambda** ↔ Java’s lambda expressions (e.g., `(a, b) -> a + b`).
- **List Comprehension** ↔ Java’s `Stream.map()`/`filter()` + `collect()`.

---

### **Summary**
- **Lambda**: For short, throwaway functions.
- **Comprehensions**: For clean, readable data transformations.
- Both are tools for writing concise code, but prioritize **readability** over brevity.
