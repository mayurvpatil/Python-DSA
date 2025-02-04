
---

### **Easy Problems**

#### 1. [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
**Problem**: Design a class to find the k-th largest element in a stream of numbers.

**Approach**:
1. Use a min-heap to store the k largest elements.
2. When a new number is added, maintain the heap size by removing the smallest element if necessary.

**Brute Force (Sorting)**:
```python
class KthLargest:
    def __init__(self, k, nums):
        self.k = k
        self.nums = sorted(nums, reverse=True)[:k]

    def add(self, val):
        self.nums.append(val)
        self.nums.sort(reverse=True)
        self.nums = self.nums[:self.k]
        return self.nums[-1]
```
- **Time Complexity**: O(n log n) for initialization, O(n log n) for each `add` operation.
- **Space Complexity**: O(k), as we store the top k elements.

**Optimized (Min-Heap)**:
```python
import heapq

class KthLargest:
    def __init__(self, k, nums):
        self.k = k
        self.heap = nums
        heapq.heapify(self.heap)
        while len(self.heap) > k:
            heapq.heappop(self.heap)

    def add(self, val):
        heapq.heappush(self.heap, val)
        if len(self.heap) > self.k:
            heapq.heappop(self.heap)
        return self.heap[0]
```
- **Time Complexity**: O(n log k) for initialization, O(log k) for each `add` operation.
- **Space Complexity**: O(k), as we store the top k elements.

---

#### 2. [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)
**Problem**: Find the last remaining stone weight after repeatedly smashing the two heaviest stones.

**Approach**:
1. Use a max-heap to efficiently find and remove the two heaviest stones.
2. Simulate the smashing process until one or no stones remain.

**Brute Force (Sorting)**:
```python
def lastStoneWeight(stones):
    while len(stones) > 1:
        stones.sort()
        x, y = stones[-2], stones[-1]
        stones = stones[:-2]
        if x != y:
            stones.append(y - x)
    return stones[0] if stones else 0
```
- **Time Complexity**: O(n^2 log n), as we sort the list in each iteration.
- **Space Complexity**: O(n), as we store the stones.

**Optimized (Max-Heap)**:
```python
import heapq

def lastStoneWeight(stones):
    stones = [-x for x in stones]  # Use negative values for max-heap
    heapq.heapify(stones)
    while len(stones) > 1:
        x = heapq.heappop(stones)
        y = heapq.heappop(stones)
        if x != y:
            heapq.heappush(stones, x - y)
    return -stones[0] if stones else 0
```
- **Time Complexity**: O(n log n), as we perform heap operations.
- **Space Complexity**: O(n), as we store the stones.

---

#### 3. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)
**Problem**: Find the k closest points to the origin.

**Approach**:
1. Use a max-heap to store the k closest points.
2. Maintain the heap size by removing the farthest point if necessary.

**Brute Force (Sorting)**:
```python
def kClosest(points, k):
    points.sort(key=lambda x: x[0]**2 + x[1]**2)
    return points[:k]
```
- **Time Complexity**: O(n log n), as we sort the points.
- **Space Complexity**: O(n), as we store the sorted points.

**Optimized (Max-Heap)**:
```python
import heapq

def kClosest(points, k):
    heap = []
    for x, y in points:
        dist = -(x**2 + y**2)  # Use negative distance for max-heap
        if len(heap) < k:
            heapq.heappush(heap, (dist, x, y))
        else:
            heapq.heappushpop(heap, (dist, x, y))
    return [[x, y] for (dist, x, y) in heap]
```
- **Time Complexity**: O(n log k), as we perform heap operations.
- **Space Complexity**: O(k), as we store the k closest points.

---

### **Medium Problems**

#### 4. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
**Problem**: Find the k most frequent elements in an array.

**Approach**:
1. Use a min-heap to store the k most frequent elements.
2. Maintain the heap size by removing the least frequent element if necessary.

**Brute Force (Sorting)**:
```python
def topKFrequent(nums, k):
    from collections import Counter
    count = Counter(nums)
    return [x for x, _ in sorted(count.items(), key=lambda x: -x[1])[:k]]
```
- **Time Complexity**: O(n log n), as we sort the frequency map.
- **Space Complexity**: O(n), as we store the frequency map.

**Optimized (Min-Heap)**:
```python
import heapq
from collections import Counter

def topKFrequent(nums, k):
    count = Counter(nums)
    heap = []
    for num, freq in count.items():
        heapq.heappush(heap, (freq, num))
        if len(heap) > k:
            heapq.heappop(heap)
    return [num for (freq, num) in heap]
```
- **Time Complexity**: O(n log k), as we perform heap operations.
- **Space Complexity**: O(n), as we store the frequency map and heap.

---

#### 5. [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)
**Problem**: Find k pairs of numbers from two arrays with the smallest sums.

**Approach**:
1. Use a min-heap to store pairs with the smallest sums.
2. Populate the heap with initial pairs and iteratively add new pairs.

**Brute Force (Sorting)**:
```python
def kSmallestPairs(nums1, nums2, k):
    pairs = []
    for x in nums1:
        for y in nums2:
            pairs.append((x + y, x, y))
    pairs.sort()
    return [[x, y] for (sum_, x, y) in pairs[:k]]
```
- **Time Complexity**: O(mn log mn), where `m` and `n` are the lengths of the arrays.
- **Space Complexity**: O(mn), as we store all pairs.

**Optimized (Min-Heap)**:
```python
import heapq

def kSmallestPairs(nums1, nums2, k):
    if not nums1 or not nums2:
        return []
    heap = []
    for i in range(min(k, len(nums1))):
        heapq.heappush(heap, (nums1[i] + nums2[0], i, 0))
    result = []
    while heap and len(result) < k:
        sum_, i, j = heapq.heappop(heap)
        result.append([nums1[i], nums2[j]])
        if j + 1 < len(nums2):
            heapq.heappush(heap, (nums1[i] + nums2[j + 1], i, j + 1))
    return result
```
- **Time Complexity**: O(k log k), as we perform heap operations.
- **Space Complexity**: O(k), as we store the heap.

---

#### 6. [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
**Problem**: Find the k-th smallest element in a sorted matrix.

**Approach**:
1. Use a min-heap to store the smallest elements.
2. Populate the heap with the first row and iteratively add new elements.

**Brute Force (Flatten and Sort)**:
```python
def kthSmallest(matrix, k):
    flattened = []
    for row in matrix:
        flattened.extend(row)
    flattened.sort()
    return flattened[k - 1]
```
- **Time Complexity**: O(n^2 log n^2), as we flatten and sort the matrix.
- **Space Complexity**: O(n^2), as we store the flattened matrix.

**Optimized (Min-Heap)**:
```python
import heapq

def kthSmallest(matrix, k):
    n = len(matrix)
    heap = [(matrix[i][0], i, 0) for i in range(n)]
    heapq.heapify(heap)
    for _ in range(k):
        val, i, j = heapq.heappop(heap)
        if j + 1 < n:
            heapq.heappush(heap, (matrix[i][j + 1], i, j + 1))
    return val
```
- **Time Complexity**: O(k log n), as we perform heap operations.
- **Space Complexity**: O(n), as we store the heap.

---

### **Hard Problems**

#### 7. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
**Problem**: Merge k sorted linked lists into one sorted linked list.

**Approach**:
1. Use a min-heap to store the smallest elements from each list.
2. Iteratively pop the smallest element and add it to the result.

**Brute Force (Flatten and Sort)**:
```python
def mergeKLists(lists):
    flattened = []
    for l in lists:
        while l:
            flattened.append(l.val)
            l = l.next
    flattened.sort()
    dummy = ListNode(0)
    curr = dummy
    for val in flattened:
        curr.next = ListNode(val)
        curr = curr.next
    return dummy.next
```
- **Time Complexity**: O(n log n), where `n` is the total number of nodes.
- **Space Complexity**: O(n), as we store the flattened list.

**Optimized (Min-Heap)**:
```python
import heapq

def mergeKLists(lists):
    heap = []
    for i, l in enumerate(lists):
        if l:
            heapq.heappush(heap, (l.val, i, l))
    dummy = ListNode(0)
    curr = dummy
    while heap:
        val, i, node = heapq.heappop(heap)
        curr.next = ListNode(val)
        curr = curr.next
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))
    return dummy.next
```
- **Time Complexity**: O(n log k), where `n` is the total number of nodes and `k` is the number of lists.
- **Space Complexity**: O(k), as we store the heap.

---

#### 8. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)
**Problem**: Design a data structure to find the median of a stream of numbers.

**Approach**:
1. Use two heaps: a max-heap for the smaller half and a min-heap for the larger half.
2. Maintain the balance between the two heaps.

**Optimized (Two Heaps)**:
```python
import heapq

class MedianFinder:
    def __init__(self):
        self.max_heap = []  # Smaller half
        self.min_heap = []  # Larger half

    def addNum(self, num):
        heapq.heappush(self.max_heap, -num)
        heapq.heappush(self.min_heap, -heapq.heappop(self.max_heap))
        if len(self.min_heap) > len(self.max_heap):
            heapq.heappush(self.max_heap, -heapq.heappop(self.min_heap))

    def findMedian(self):
        if len(self.max_heap) > len(self.min_heap):
            return -self.max_heap[0]
        return (-self.max_heap[0] + self.min_heap[0]) / 2
```
- **Time Complexity**: O(log n) for `addNum`, O(1) for `findMedian`.
- **Space Complexity**: O(n), as we store the numbers in the heaps.

---

#### 9. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)
**Problem**: Find the median of each sliding window of size k in an array.

**Approach**:
1. Use two heaps: a max-heap for the smaller half and a min-heap for the larger half.
2. Maintain the balance between the two heaps as the window slides.

**Optimized (Two Heaps)**:
```python
import heapq

def medianSlidingWindow(nums, k):
    def add(num):
        heapq.heappush(max_heap, -num)
        heapq.heappush(min_heap, -heapq.heappop(max_heap))
        if len(min_heap) > len(max_heap):
            heapq.heappush(max_heap, -heapq.heappop(min_heap))

    def remove(num):
        if num <= -max_heap[0]:
            max_heap.remove(-num)
            heapq.heapify(max_heap)
        else:
            min_heap.remove(num)
            heapq.heapify(min_heap)

    max_heap = []
    min_heap = []
    result = []
    for i in range(len(nums)):
        add(nums[i])
        if i >= k:
            remove(nums[i - k])
        if i >= k - 1:
            if k % 2 == 1:
                result.append(-max_heap[0])
            else:
                result.append((-max_heap[0] + min_heap[0]) / 2)
    return result
```
- **Time Complexity**: O(n log k), as we perform heap operations for each window.
- **Space Complexity**: O(k), as we store the heaps.

---

### **Summary of Python Implementations**
1. **Heap**: The standard approach for problems involving finding the k-th largest/smallest element or merging sorted lists.
2. **Two Heaps**: Used for problems involving maintaining a balance between two halves (e.g., median finding).
3. **Heap with Pruning**: Optimizes by skipping unnecessary elements (e.g., finding k pairs with smallest sums).

