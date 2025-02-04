
---

## **Easy Problems**

---

### **1. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)**
**Problem**: Merge overlapping intervals.  
**Example**:  
**Input**: `intervals = [[1,3],[2,6],[8,10],[15,18]]` → **Output**: `[[1,6],[8,10],[15,18]]`.

#### **Brute Force**
**Approach**: Check all pairs of intervals and merge overlapping ones.  
**Time**: O(n²), **Space**: O(n)  
```python
def merge_brute(intervals: list) -> list:
    intervals.sort()
    merged = []
    for interval in intervals:
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            merged[-1][1] = max(merged[-1][1], interval[1])
    return merged
```

#### **Optimized (Sorting + Linear Scan)**
**Approach**: Sort intervals and merge overlapping ones in a single pass.  
**Time**: O(n log n), **Space**: O(n)  
```python
def merge_optimized(intervals: list) -> list:
    intervals.sort()
    merged = []
    for interval in intervals:
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            merged[-1][1] = max(merged[-1][1], interval[1])
    return merged
```

---

### **2. [Insert Interval](https://leetcode.com/problems/insert-interval/)**
**Problem**: Insert a new interval into a list of non-overlapping intervals.  
**Example**:  
**Input**: `intervals = [[1,3],[6,9]], newInterval = [2,5]` → **Output**: `[[1,5],[6,9]]`.

#### **Brute Force**
**Approach**: Insert the new interval, sort the list, and merge overlapping intervals.  
**Time**: O(n log n), **Space**: O(n)  
```python
def insert_brute(intervals: list, newInterval: list) -> list:
    intervals.append(newInterval)
    intervals.sort()
    merged = []
    for interval in intervals:
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            merged[-1][1] = max(merged[-1][1], interval[1])
    return merged
```

#### **Optimized (Linear Scan)**
**Approach**: Traverse the list and merge the new interval with overlapping intervals.  
**Time**: O(n), **Space**: O(n)  
```python
def insert_optimized(intervals: list, newInterval: list) -> list:
    merged = []
    i = 0
    # Add non-overlapping intervals before newInterval
    while i < len(intervals) and intervals[i][1] < newInterval[0]:
        merged.append(intervals[i])
        i += 1
    # Merge overlapping intervals
    while i < len(intervals) and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i += 1
    merged.append(newInterval)
    # Add remaining intervals
    while i < len(intervals):
        merged.append(intervals[i])
        i += 1
    return merged
```

---

### **3. [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)**
**Problem**: Find the minimum number of intervals to remove to make the rest non-overlapping.  
**Example**:  
**Input**: `intervals = [[1,2],[2,3],[3,4],[1,3]]` → **Output**: `1` (Remove `[1,3]`).

#### **Brute Force**
**Approach**: Check all possible combinations of intervals and count overlaps.  
**Time**: O(2^n), **Space**: O(n)  
```python
def eraseOverlapIntervals_brute(intervals: list) -> int:
    intervals.sort()
    def dfs(index, last_end):
        if index == len(intervals):
            return 0
        count = 0
        if intervals[index][0] < last_end:
            count = 1 + dfs(index + 1, last_end)
        else:
            count = dfs(index + 1, intervals[index][1])
        return min(count, 1 + dfs(index + 1, last_end))
    return dfs(0, float('-inf'))
```

#### **Optimized (Greedy)**
**Approach**: Sort intervals by end time and greedily select non-overlapping intervals.  
**Time**: O(n log n), **Space**: O(1)  
```python
def eraseOverlapIntervals_optimized(intervals: list) -> int:
    if not intervals:
        return 0
    intervals.sort(key=lambda x: x[1])
    last_end = intervals[0][1]
    count = 0
    for i in range(1, len(intervals)):
        if intervals[i][0] < last_end:
            count += 1
        else:
            last_end = intervals[i][1]
    return count
```

---

## **Medium Problems**

---

### **4. [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)**
**Problem**: Find the minimum number of conference rooms required.  
**Example**:  
**Input**: `intervals = [[0,30],[5,10],[15,20]]` → **Output**: `2`.

#### **Brute Force**
**Approach**: Check all overlapping intervals and count the maximum overlaps.  
**Time**: O(n²), **Space**: O(n)  
```python
def minMeetingRooms_brute(intervals: list) -> int:
    if not intervals:
        return 0
    intervals.sort()
    max_rooms = 0
    for i in range(len(intervals)):
        rooms = 1
        for j in range(i):
            if intervals[j][1] > intervals[i][0]:
                rooms += 1
        max_rooms = max(max_rooms, rooms)
    return max_rooms
```

#### **Optimized (Sorting + Min-Heap)**
**Approach**: Use a min-heap to track the end times of meetings.  
**Time**: O(n log n), **Space**: O(n)  
```python
import heapq
def minMeetingRooms_optimized(intervals: list) -> int:
    if not intervals:
        return 0
    intervals.sort()
    heap = []
    heapq.heappush(heap, intervals[0][1])
    for i in range(1, len(intervals)):
        if intervals[i][0] >= heap[0]:
            heapq.heappop(heap)
        heapq.heappush(heap, intervals[i][1])
    return len(heap)
```

---

### **5. [Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)**
**Problem**: Find the intersection of two lists of intervals.  
**Example**:  
**Input**: `A = [[0,2],[5,10],[13,23],[24,25]], B = [[1,5],[8,12],[15,24],[25,26]]` → **Output**: `[[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]`.

#### **Brute Force**
**Approach**: Check all pairs of intervals for intersections.  
**Time**: O(m*n), **Space**: O(m+n)  
```python
def intervalIntersection_brute(A: list, B: list) -> list:
    result = []
    for a in A:
        for b in B:
            start = max(a[0], b[0])
            end = min(a[1], b[1])
            if start <= end:
                result.append([start, end])
    return result
```

#### **Optimized (Two Pointers)**
**Approach**: Use two pointers to traverse both lists and find intersections.  
**Time**: O(m+n), **Space**: O(m+n)  
```python
def intervalIntersection_optimized(A: list, B: list) -> list:
    result = []
    i = j = 0
    while i < len(A) and j < len(B):
        start = max(A[i][0], B[j][0])
        end = min(A[i][1], B[j][1])
        if start <= end:
            result.append([start, end])
        if A[i][1] < B[j][1]:
            i += 1
        else:
            j += 1
    return result
```

---

### **6. [Remove Covered Intervals](https://leetcode.com/problems/remove-covered-intervals/)**
**Problem**: Remove intervals that are covered by another interval.  
**Example**:  
**Input**: `intervals = [[1,4],[3,6],[2,8]]` → **Output**: `2` (Remove `[3,6]`).

#### **Brute Force**
**Approach**: Check all pairs of intervals and remove covered ones.  
**Time**: O(n²), **Space**: O(1)  
```python
def removeCoveredIntervals_brute(intervals: list) -> int:
    intervals.sort()
    count = 0
    for i in range(len(intervals)):
        covered = False
        for j in range(len(intervals)):
            if i != j and intervals[j][0] <= intervals[i][0] and intervals[j][1] >= intervals[i][1]:
                covered = True
                break
        if not covered:
            count += 1
    return count
```

#### **Optimized (Sorting + Linear Scan)**
**Approach**: Sort intervals and track the maximum end time.  
**Time**: O(n log n), **Space**: O(1)  
```python
def removeCoveredIntervals_optimized(intervals: list) -> int:
    intervals.sort(key=lambda x: (x[0], -x[1]))
    count = 0
    max_end = 0
    for interval in intervals:
        if interval[1] > max_end:
            count += 1
            max_end = interval[1]
    return count
```

---

## **Hard Problems**

---

### **7. [Employee Free Time](https://leetcode.com/problems/employee-free-time/)**
**Problem**: Find the free time intervals for all employees.  
**Example**:  
**Input**: `schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]` → **Output**: `[[3,4]]`.

#### **Brute Force**
**Approach**: Merge all intervals and find gaps between them.  
**Time**: O(n log n), **Space**: O(n)  
```python
def employeeFreeTime_brute(schedule: list) -> list:
    intervals = []
    for emp in schedule:
        intervals.extend(emp)
    intervals.sort()
    merged = []
    for interval in intervals:
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            merged[-1][1] = max(merged[-1][1], interval[1])
    free = []
    for i in range(1, len(merged)):
        if merged[i][0] > merged[i-1][1]:
            free.append([merged[i-1][1], merged[i][0]])
    return free
```

#### **Optimized (Sorting + Linear Scan)**
**Approach**: Use a min-heap to merge intervals and find gaps.  
**Time**: O(n log n), **Space**: O(n)  
```python
import heapq
def employeeFreeTime_optimized(schedule: list) -> list:
    intervals = []
    for emp in schedule:
        intervals.extend(emp)
    intervals.sort()
    merged = []
    for interval in intervals:
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            merged[-1][1] = max(merged[-1][1], interval[1])
    free = []
    for i in range(1, len(merged)):
        if merged[i][0] > merged[i-1][1]:
            free.append([merged[i-1][1], merged[i][0]])
    return free
```

---

### **8. [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)**
**Problem**: Find the minimum number of arrows to burst all balloons.  
**Example**:  
**Input**: `points = [[10,16],[2,8],[1,6],[7,12]]` → **Output**: `2`.

#### **Brute Force**
**Approach**: Check all overlapping intervals and count the minimum arrows.  
**Time**: O(n²), **Space**: O(n)  
```python
def findMinArrowShots_brute(points: list) -> int:
    if not points:
        return 0
    points.sort()
    arrows = 0
    while points:
        arrow = points[0][1]
        arrows += 1
        points = [p for p in points if p[0] > arrow]
    return arrows
```

#### **Optimized (Greedy)**
**Approach**: Sort intervals by end time and greedily shoot arrows.  
**Time**: O(n log n), **Space**: O(1)  
```python
def findMinArrowShots_optimized(points: list) -> int:
    if not points:
        return 0
    points.sort(key=lambda x: x[1])
    arrows = 1
    end = points[0][1]
    for i in range(1, len(points)):
        if points[i][0] > end:
            arrows += 1
            end = points[i][1]
    return arrows
```

---

### **9. [My Calendar I](https://leetcode.com/problems/my-calendar-i/)**
**Problem**: Implement a calendar to book events without overlapping.  
**Example**:  
**Input**: `book(10, 20)` → **Output**: `True` (Event booked).  
**Input**: `book(15, 25)` → **Output**: `False` (Event not booked).

#### **Brute Force**
**Approach**: Check all booked intervals for overlaps.  
**Time**: O(n²), **Space**: O(n)  
```python
class MyCalendar_brute:
    def __init__(self):
        self.events = []

    def book(self, start: int, end: int) -> bool:
        for event in self.events:
            if event[0] < end and event[1] > start:
                return False
        self.events.append([start, end])
        return True
```

#### **Optimized (Binary Search Tree)**
**Approach**: Use a balanced BST to efficiently check for overlaps.  
**Time**: O(n log n), **Space**: O(n)  
```python
from bisect import bisect_left, insort
class MyCalendar_optimized:
    def __init__(self):
        self.events = []

    def book(self, start: int, end: int) -> bool:
        idx = bisect_left(self.events, (start, end))
        if (idx > 0 and self.events[idx-1][1] > start) or (idx < len(self.events) and self.events[idx][0] < end):
            return False
        insort(self.events, (start, end))
        return True
```

---

### **Key Takeaways**:
1. **Brute Force** approaches are straightforward but inefficient (O(n²) or worse).  
2. **Optimized Overlap Handling** methods reduce time to O(n log n) or O(n) by:  
   - Sorting intervals and merging them in a single pass.  
   - Using data structures like heaps or BSTs for efficient overlap checks.  
3. **Hard Problems** often require combining sorting with greedy algorithms or advanced data structures.  

