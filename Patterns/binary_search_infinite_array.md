
---

### **Easy Problems**

#### 1. [Search in a Sorted Infinite Array](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/)
**Problem**: Search for a target value in a sorted infinite array.

**Approach**:
1. Use exponential search to find the bounds of the array.
2. Perform binary search within the bounds.

**Brute Force (Linear Search)**:
```python
def search(reader, target):
    index = 0
    while True:
        val = reader.get(index)
        if val == target:
            return index
        if val > target:
            return -1
        index += 1
```
- **Time Complexity**: O(n), where `n` is the position of the target.
- **Space Complexity**: O(1), as we use constant extra space.

**Optimized (Exponential Search + Binary Search)**:
```python
def search(reader, target):
    # Find the bounds
    left, right = 0, 1
    while reader.get(right) < target:
        left = right
        right *= 2
    # Binary search within the bounds
    while left <= right:
        mid = (left + right) // 2
        val = reader.get(mid)
        if val == target:
            return mid
        elif val < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```
- **Time Complexity**: O(log n), where `n` is the position of the target.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 2. [Find Position of an Element in a Sorted Infinite Array](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/)
**Problem**: Similar to the previous problem, but with additional constraints.

**Approach**:
1. Use exponential search to find the bounds of the array.
2. Perform binary search within the bounds.

**Optimized (Exponential Search + Binary Search)**:
```python
def search(reader, target):
    # Find the bounds
    left, right = 0, 1
    while reader.get(right) < target:
        left = right
        right *= 2
    # Binary search within the bounds
    while left <= right:
        mid = (left + right) // 2
        val = reader.get(mid)
        if val == target:
            return mid
        elif val < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```
- **Time Complexity**: O(log n), where `n` is the position of the target.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 3. [Check if an Element Exists in a Sorted Infinite Array](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/)
**Problem**: Check if a target value exists in a sorted infinite array.

**Approach**:
1. Use exponential search to find the bounds of the array.
2. Perform binary search within the bounds.

**Optimized (Exponential Search + Binary Search)**:
```python
def search(reader, target):
    # Find the bounds
    left, right = 0, 1
    while reader.get(right) < target:
        left = right
        right *= 2
    # Binary search within the bounds
    while left <= right:
        mid = (left + right) // 2
        val = reader.get(mid)
        if val == target:
            return True
        elif val < target:
            left = mid + 1
        else:
            right = mid - 1
    return False
```
- **Time Complexity**: O(log n), where `n` is the position of the target.
- **Space Complexity**: O(1), as we use constant extra space.

---

### **Medium Problems**

#### 4. [Find the First Occurrence of a Target in a Sorted Infinite Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
**Problem**: Find the first occurrence of a target value in a sorted infinite array.

**Approach**:
1. Use exponential search to find the bounds of the array.
2. Perform binary search within the bounds to find the first occurrence.

**Optimized (Exponential Search + Binary Search)**:
```python
def search(reader, target):
    # Find the bounds
    left, right = 0, 1
    while reader.get(right) < target:
        left = right
        right *= 2
    # Binary search within the bounds
    first = -1
    while left <= right:
        mid = (left + right) // 2
        val = reader.get(mid)
        if val == target:
            first = mid
            right = mid - 1
        elif val < target:
            left = mid + 1
        else:
            right = mid - 1
    return first
```
- **Time Complexity**: O(log n), where `n` is the position of the target.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 5. [Find the Last Occurrence of a Target in a Sorted Infinite Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
**Problem**: Find the last occurrence of a target value in a sorted infinite array.

**Approach**:
1. Use exponential search to find the bounds of the array.
2. Perform binary search within the bounds to find the last occurrence.

**Optimized (Exponential Search + Binary Search)**:
```python
def search(reader, target):
    # Find the bounds
    left, right = 0, 1
    while reader.get(right) < target:
        left = right
        right *= 2
    # Binary search within the bounds
    last = -1
    while left <= right:
        mid = (left + right) // 2
        val = reader.get(mid)
        if val == target:
            last = mid
            left = mid + 1
        elif val < target:
            left = mid + 1
        else:
            right = mid - 1
    return last
```
- **Time Complexity**: O(log n), where `n` is the position of the target.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 6. [Find the Range of a Target in a Sorted Infinite Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
**Problem**: Find the range (first and last occurrence) of a target value in a sorted infinite array.

**Approach**:
1. Use exponential search to find the bounds of the array.
2. Perform binary search within the bounds to find the first and last occurrence.

**Optimized (Exponential Search + Binary Search)**:
```python
def searchRange(reader, target):
    # Find the bounds
    left, right = 0, 1
    while reader.get(right) < target:
        left = right
        right *= 2
    # Binary search within the bounds
    first, last = -1, -1
    # Find first occurrence
    while left <= right:
        mid = (left + right) // 2
        val = reader.get(mid)
        if val == target:
            first = mid
            right = mid - 1
        elif val < target:
            left = mid + 1
        else:
            right = mid - 1
    # Find last occurrence
    left, right = 0, 1
    while reader.get(right) < target:
        left = right
        right *= 2
    while left <= right:
        mid = (left + right) // 2
        val = reader.get(mid)
        if val == target:
            last = mid
            left = mid + 1
        elif val < target:
            left = mid + 1
        else:
            right = mid - 1
    return [first, last]
```
- **Time Complexity**: O(log n), where `n` is the position of the target.
- **Space Complexity**: O(1), as we use constant extra space.

---

### **Hard Problems**

#### 7. [Find the Smallest Element in a Sorted Infinite Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
**Problem**: Find the smallest element in a sorted infinite array that may be rotated.

**Approach**:
1. Use exponential search to find the bounds of the array.
2. Perform binary search within the bounds to find the smallest element.

**Optimized (Exponential Search + Binary Search)**:
```python
def findMin(reader):
    # Find the bounds
    left, right = 0, 1
    while reader.get(right) > reader.get(left):
        left = right
        right *= 2
    # Binary search within the bounds
    while left < right:
        mid = (left + right) // 2
        if reader.get(mid) > reader.get(right):
            left = mid + 1
        else:
            right = mid
    return reader.get(left)
```
- **Time Complexity**: O(log n), where `n` is the position of the smallest element.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 8. [Find the Kth Smallest Element in a Sorted Infinite Array](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
**Problem**: Find the k-th smallest element in a sorted infinite array.

**Approach**:
1. Use exponential search to find the bounds of the array.
2. Perform binary search within the bounds to find the k-th smallest element.

**Optimized (Exponential Search + Binary Search)**:
```python
def findKthSmallest(reader, k):
    # Find the bounds
    left, right = 0, 1
    while reader.get(right) < k:
        left = right
        right *= 2
    # Binary search within the bounds
    while left <= right:
        mid = (left + right) // 2
        val = reader.get(mid)
        if val == k:
            return mid
        elif val < k:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```
- **Time Complexity**: O(log n), where `n` is the position of the k-th smallest element.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 9. [Find the Kth Largest Element in a Sorted Infinite Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
**Problem**: Find the k-th largest element in a sorted infinite array.

**Approach**:
1. Use exponential search to find the bounds of the array.
2. Perform binary search within the bounds to find the k-th largest element.

**Optimized (Exponential Search + Binary Search)**:
```python
def findKthLargest(reader, k):
    # Find the bounds
    left, right = 0, 1
    while reader.get(right) < k:
        left = right
        right *= 2
    # Binary search within the bounds
    while left <= right:
        mid = (left + right) // 2
        val = reader.get(mid)
        if val == k:
            return mid
        elif val < k:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```
- **Time Complexity**: O(log n), where `n` is the position of the k-th largest element.
- **Space Complexity**: O(1), as we use constant extra space.

---

### **Summary of Python Implementations**
1. **Exponential Search**: Used to find the bounds of the infinite array.
2. **Binary Search**: Used to search within the bounds for the target or k-th element.
3. **Handling Rotated Arrays**: Use binary search to find the smallest element in a rotated array.

