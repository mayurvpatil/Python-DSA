
---

### **Easy Problems**

#### 1. [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
**Problem**: Search for a target value in a rotated sorted array.

**Approach**:
1. Use binary search to find the pivot (the smallest element).
2. Perform binary search on the appropriate half of the array.

**Brute Force (Linear Search)**:
```python
def search(nums, target):
    for i in range(len(nums)):
        if nums[i] == target:
            return i
    return -1
```
- **Time Complexity**: O(n), as we perform a linear search.
- **Space Complexity**: O(1), as we use constant extra space.

**Optimized (Binary Search)**:
```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        # Check if the left half is sorted
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
- **Time Complexity**: O(log n), as we perform binary search.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 2. [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
**Problem**: Find the minimum element in a rotated sorted array.

**Approach**:
1. Use binary search to find the pivot (the smallest element).

**Brute Force (Linear Search)**:
```python
def findMin(nums):
    return min(nums)
```
- **Time Complexity**: O(n), as we perform a linear search.
- **Space Complexity**: O(1), as we use constant extra space.

**Optimized (Binary Search)**:
```python
def findMin(nums):
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > nums[right]:
            left = mid + 1
        else:
            right = mid
    return nums[left]
```
- **Time Complexity**: O(log n), as we perform binary search.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 3. [Check if Array Is Sorted and Rotated](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/)
**Problem**: Check if an array is sorted and rotated.

**Approach**:
1. Count the number of places where the array is not sorted.
2. If the count is 0 or 1, the array is sorted and rotated.

**Brute Force (Linear Check)**:
```python
def check(nums):
    count = 0
    for i in range(len(nums)):
        if nums[i] > nums[(i + 1) % len(nums)]:
            count += 1
    return count <= 1
```
- **Time Complexity**: O(n), as we perform a linear check.
- **Space Complexity**: O(1), as we use constant extra space.

---

### **Medium Problems**

#### 4. [Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)
**Problem**: Search for a target value in a rotated sorted array that may contain duplicates.

**Approach**:
1. Use binary search to find the pivot (the smallest element).
2. Handle duplicates by skipping them.

**Brute Force (Linear Search)**:
```python
def search(nums, target):
    for i in range(len(nums)):
        if nums[i] == target:
            return True
    return False
```
- **Time Complexity**: O(n), as we perform a linear search.
- **Space Complexity**: O(1), as we use constant extra space.

**Optimized (Binary Search)**:
```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return True
        # Handle duplicates
        if nums[left] == nums[mid] == nums[right]:
            left += 1
            right -= 1
        elif nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    return False
```
- **Time Complexity**: O(log n) on average, but O(n) in the worst case due to duplicates.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 5. [Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)
**Problem**: Find the minimum element in a rotated sorted array that may contain duplicates.

**Approach**:
1. Use binary search to find the pivot (the smallest element).
2. Handle duplicates by skipping them.

**Brute Force (Linear Search)**:
```python
def findMin(nums):
    return min(nums)
```
- **Time Complexity**: O(n), as we perform a linear search.
- **Space Complexity**: O(1), as we use constant extra space.

**Optimized (Binary Search)**:
```python
def findMin(nums):
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > nums[right]:
            left = mid + 1
        elif nums[mid] < nums[right]:
            right = mid
        else:
            right -= 1  # Handle duplicates
    return nums[left]
```
- **Time Complexity**: O(log n) on average, but O(n) in the worst case due to duplicates.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 6. [Find Peak Element](https://leetcode.com/problems/find-peak-element/)
**Problem**: Find a peak element in an array (an element greater than its neighbors).

**Approach**:
1. Use binary search to find a peak element.
2. Compare the middle element with its neighbors.

**Brute Force (Linear Search)**:
```python
def findPeakElement(nums):
    for i in range(len(nums)):
        if (i == 0 or nums[i] > nums[i - 1]) and (i == len(nums) - 1 or nums[i] > nums[i + 1]):
            return i
    return -1
```
- **Time Complexity**: O(n), as we perform a linear search.
- **Space Complexity**: O(1), as we use constant extra space.

**Optimized (Binary Search)**:
```python
def findPeakElement(nums):
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] < nums[mid + 1]:
            left = mid + 1
        else:
            right = mid
    return left
```
- **Time Complexity**: O(log n), as we perform binary search.
- **Space Complexity**: O(1), as we use constant extra space.

---

### **Hard Problems**

#### 7. [Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/)
**Problem**: Find the index of a target value in a mountain array.

**Approach**:
1. Use binary search to find the peak of the mountain.
2. Perform binary search on the ascending and descending sides.

**Brute Force (Linear Search)**:
```python
def findInMountainArray(target, mountain_arr):
    for i in range(len(mountain_arr)):
        if mountain_arr[i] == target:
            return i
    return -1
```
- **Time Complexity**: O(n), as we perform a linear search.
- **Space Complexity**: O(1), as we use constant extra space.

**Optimized (Binary Search)**:
```python
def findInMountainArray(target, mountain_arr):
    # Find the peak
    left, right = 0, len(mountain_arr) - 1
    while left < right:
        mid = (left + right) // 2
        if mountain_arr[mid] < mountain_arr[mid + 1]:
            left = mid + 1
        else:
            right = mid
    peak = left
    
    # Search in the ascending part
    left, right = 0, peak
    while left <= right:
        mid = (left + right) // 2
        if mountain_arr[mid] == target:
            return mid
        elif mountain_arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    # Search in the descending part
    left, right = peak, len(mountain_arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if mountain_arr[mid] == target:
            return mid
        elif mountain_arr[mid] < target:
            right = mid - 1
        else:
            left = mid + 1
    
    return -1
```
- **Time Complexity**: O(log n), as we perform binary search.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 8. [Search in Rotated Sorted Array with Duplicates](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)
**Problem**: Similar to Search in Rotated Sorted Array II, but with additional constraints.

**Approach**:
1. Use binary search to find the pivot (the smallest element).
2. Handle duplicates by skipping them.

**Optimized (Binary Search)**:
```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return True
        # Handle duplicates
        if nums[left] == nums[mid] == nums[right]:
            left += 1
            right -= 1
        elif nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    return False
```
- **Time Complexity**: O(log n) on average, but O(n) in the worst case due to duplicates.
- **Space Complexity**: O(1), as we use constant extra space.

---

#### 9. [Find Minimum in Rotated Sorted Array with Duplicates](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)
**Problem**: Similar to Find Minimum in Rotated Sorted Array II, but with additional constraints.

**Approach**:
1. Use binary search to find the pivot (the smallest element).
2. Handle duplicates by skipping them.

**Optimized (Binary Search)**:
```python
def findMin(nums):
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > nums[right]:
            left = mid + 1
        elif nums[mid] < nums[right]:
            right = mid
        else:
            right -= 1  # Handle duplicates
    return nums[left]
```
- **Time Complexity**: O(log n) on average, but O(n) in the worst case due to duplicates.
- **Space Complexity**: O(1), as we use constant extra space.

---

### **Summary of Python Implementations**
1. **Binary Search**: The standard approach for rotated array problems.
2. **Handling Duplicates**: Skip duplicates to optimize the search.
3. **Mountain Array**: Use binary search to find the peak and then search on both sides.

