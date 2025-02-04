
---

## **Easy Problems**

---

### **1. [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)**
**Problem**: Merge two sorted arrays into one sorted array.  
**Example**:  
**Input**: `nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3` → **Output**: `[1,2,2,3,5,6]`.

#### **Brute Force**
**Approach**: Merge the arrays and sort the result.  
**Time**: O((m+n) log(m+n)), **Space**: O(1)  
```python
def merge_brute(nums1: list, m: int, nums2: list, n: int) -> None:
    nums1[m:] = nums2
    nums1.sort()
```

#### **Optimized (Parallel Two Pointers)**
**Approach**: Use two pointers to merge the arrays in place.  
**Time**: O(m+n), **Space**: O(1)  
```python
def merge_optimized(nums1: list, m: int, nums2: list, n: int) -> None:
    p1, p2, p = m - 1, n - 1, m + n - 1
    while p1 >= 0 and p2 >= 0:
        if nums1[p1] > nums2[p2]:
            nums1[p] = nums1[p1]
            p1 -= 1
        else:
            nums1[p] = nums2[p2]
            p2 -= 1
        p -= 1
    nums1[:p2 + 1] = nums2[:p2 + 1]  # Copy remaining elements from nums2
```

---

### **2. [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)**
**Problem**: Find the intersection of two arrays.  
**Example**:  
**Input**: `nums1 = [1,2,2,1], nums2 = [2,2]` → **Output**: `[2]`.

#### **Brute Force**
**Approach**: Check all elements of one array in the other.  
**Time**: O(m*n), **Space**: O(1)  
```python
def intersection_brute(nums1: list, nums2: list) -> list:
    result = []
    for num in nums1:
        if num in nums2 and num not in result:
            result.append(num)
    return result
```

#### **Optimized (Parallel Two Pointers)**
**Approach**: Sort both arrays and use two pointers to find common elements.  
**Time**: O(m log m + n log n), **Space**: O(1)  
```python
def intersection_optimized(nums1: list, nums2: list) -> list:
    nums1.sort()
    nums2.sort()
    p1, p2 = 0, 0
    result = []
    while p1 < len(nums1) and p2 < len(nums2):
        if nums1[p1] < nums2[p2]:
            p1 += 1
        elif nums1[p1] > nums2[p2]:
            p2 += 1
        else:
            if not result or nums1[p1] != result[-1]:
                result.append(nums1[p1])
            p1 += 1
            p2 += 1
    return result
```

---

### **3. [Move Zeroes](https://leetcode.com/problems/move-zeroes/)**
**Problem**: Move all zeros to the end while maintaining the order of non-zero elements.  
**Example**:  
**Input**: `nums = [0,1,0,3,12]` → **Output**: `[1,3,12,0,0]`.

#### **Brute Force**
**Approach**: Create a new array and copy non-zero elements first, then zeros.  
**Time**: O(n), **Space**: O(n)  
```python
def moveZeroes_brute(nums: list) -> None:
    non_zero = [num for num in nums if num != 0]
    zeros = [0] * (len(nums) - len(non_zero))
    nums[:] = non_zero + zeros
```

#### **Optimized (Parallel Two Pointers)**
**Approach**: Use two pointers to swap non-zero elements to the front.  
**Time**: O(n), **Space**: O(1)  
```python
def moveZeroes_optimized(nums: list) -> None:
    left = 0
    for right in range(len(nums)):
        if nums[right] != 0:
            nums[left], nums[right] = nums[right], nums[left]
            left += 1
```

---

## **Medium Problems**

---

### **4. [Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)**
**Problem**: Remove duplicates such that each element appears at most twice.  
**Example**:  
**Input**: `nums = [1,1,1,2,2,3]` → **Output**: `5` (Array becomes `[1,1,2,2,3]`).

#### **Brute Force**
**Approach**: Use a hash map to count occurrences and overwrite the array.  
**Time**: O(n), **Space**: O(n)  
```python
def removeDuplicates_brute(nums: list) -> int:
    from collections import defaultdict
    count = defaultdict(int)
    index = 0
    for num in nums:
        count[num] += 1
        if count[num] <= 2:
            nums[index] = num
            index += 1
    return index
```

#### **Optimized (Parallel Two Pointers)**
**Approach**: Use two pointers to track the current position and overwrite duplicates.  
**Time**: O(n), **Space**: O(1)  
```python
def removeDuplicates_optimized(nums: list) -> int:
    if len(nums) <= 2:
        return len(nums)
    slow = 2
    for fast in range(2, len(nums)):
        if nums[fast] != nums[slow - 2]:
            nums[slow] = nums[fast]
            slow += 1
    return slow
```

---

### **5. [Sort Colors](https://leetcode.com/problems/sort-colors/)**
**Problem**: Sort an array of 0s, 1s, and 2s in place.  
**Example**:  
**Input**: `nums = [2,0,2,1,1,0]` → **Output**: `[0,0,1,1,2,2]`.

#### **Brute Force**
**Approach**: Count the occurrences of each color and overwrite the array.  
**Time**: O(n), **Space**: O(1)  
```python
def sortColors_brute(nums: list) -> None:
    count = [0] * 3
    for num in nums:
        count[num] += 1
    index = 0
    for i in range(3):
        for _ in range(count[i]):
            nums[index] = i
            index += 1
```

#### **Optimized (Parallel Two Pointers)**
**Approach**: Use two pointers to partition the array into three sections.  
**Time**: O(n), **Space**: O(1)  
```python
def sortColors_optimized(nums: list) -> None:
    left, right = 0, len(nums) - 1
    i = 0
    while i <= right:
        if nums[i] == 0:
            nums[left], nums[i] = nums[i], nums[left]
            left += 1
            i += 1
        elif nums[i] == 2:
            nums[right], nums[i] = nums[i], nums[right]
            right -= 1
        else:
            i += 1
```

---

### **6. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)**
**Problem**: Merge overlapping intervals.  
**Example**:  
**Input**: `intervals = [[1,3],[2,6],[8,10],[15,18]]` → **Output**: `[[1,6],[8,10],[15,18]]`.

#### **Brute Force**
**Approach**: Check all pairs of intervals and merge them.  
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

#### **Optimized (Parallel Two Pointers)**
**Approach**: Sort the intervals and merge overlapping ones in a single pass.  
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

## **Hard Problems**

---

### **7. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)**
**Problem**: Compute how much water can be trapped between bars.  
**Example**:  
**Input**: `height = [0,1,0,2,1,0,1,3,2,1,2,1]` → **Output**: `6`.

#### **Brute Force**
**Approach**: For each bar, find the highest bars on its left and right.  
**Time**: O(n²), **Space**: O(1)  
```python
def trap_brute(height: list) -> int:
    total = 0
    for i in range(1, len(height) - 1):
        left_max = max(height[:i])
        right_max = max(height[i+1:])
        water = min(left_max, right_max) - height[i]
        if water > 0:
            total += water
    return total
```

#### **Optimized (Parallel Two Pointers)**
**Approach**: Use two pointers to track the highest bars on both sides.  
**Time**: O(n), **Space**: O(1)  
```python
def trap_optimized(height: list) -> int:
    left, right = 0, len(height) - 1
    left_max = right_max = total = 0
    while left < right:
        if height[left] < height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                total += left_max - height[left]
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                total += right_max - height[right]
            right -= 1
    return total
```

---

### **8. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)**
**Problem**: Find the smallest substring of `s` containing all characters of `t`.  
**Example**:  
**Input**: `s = "ADOBECODEBANC", t = "ABC"` → **Output**: `"BANC"`.

#### **Brute Force**
**Approach**: Check all substrings for containing all characters of `t`.  
**Time**: O(n²), **Space**: O(n)  
```python
def minWindow_brute(s: str, t: str) -> str:
    from collections import Counter
    target = Counter(t)
    min_len = float('inf')
    result = ""
    for i in range(len(s)):
        count = Counter()
        for j in range(i, len(s)):
            count[s[j]] += 1
            if all(count[c] >= target[c] for c in target):
                if j - i + 1 < min_len:
                    min_len = j - i + 1
                    result = s[i:j+1]
                break
    return result
```

#### **Optimized (Parallel Two Pointers)**
**Approach**: Use two pointers to track the window and shrink it when all characters are found.  
**Time**: O(n), **Space**: O(1)  
```python
def minWindow_optimized(s: str, t: str) -> str:
    from collections import Counter
    target = Counter(t)
    required = len(target)
    left = formed = 0
    min_len = float('inf')
    result = ""
    count = {}
    for right in range(len(s)):
        char = s[right]
        count[char] = count.get(char, 0) + 1
        if count[char] == target[char]:
            formed += 1
        while formed == required:  # Shrink window
            if right - left + 1 < min_len:
                min_len = right - left + 1
                result = s[left:right+1]
            left_char = s[left]
            count[left_char] -= 1
            if count[left_char] < target[left_char]:
                formed -= 1
            left += 1
    return result
```

---

### **9. [Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)**
**Problem**: Find the length of the longest substring with at most 2 distinct characters.  
**Example**:  
**Input**: `s = "eceba"` → **Output**: `3` (substring `"ece"`).

#### **Brute Force**
**Approach**: Check all substrings for at most 2 distinct characters.  
**Time**: O(n²), **Space**: O(1)  
```python
def lengthOfLongestSubstringTwoDistinct_brute(s: str) -> int:
    max_len = 0
    for i in range(len(s)):
        distinct = set()
        for j in range(i, len(s)):
            distinct.add(s[j])
            if len(distinct) > 2:
                break
            max_len = max(max_len, j - i + 1)
    return max_len
```

#### **Optimized (Parallel Two Pointers)**
**Approach**: Use two pointers to track the window and shrink it when more than 2 distinct characters are found.  
**Time**: O(n), **Space**: O(1)  
```python
def lengthOfLongestSubstringTwoDistinct_optimized(s: str) -> int:
    from collections import defaultdict
    count = defaultdict(int)
    left = max_len = 0
    for right in range(len(s)):
        count[s[right]] += 1
        while len(count) > 2:  # Shrink window
            count[s[left]] -= 1
            if count[s[left]] == 0:
                del count[s[left]]
            left += 1
        max_len = max(max_len, right - left + 1)
    return max_len
```

---

### **Key Takeaways**:
1. **Brute Force** approaches are straightforward but inefficient (O(n²) or worse).  
2. **Optimized Parallel Two Pointers** methods reduce time to O(n) or O(n log n) by:  
   - Sorting the array (if needed).  
   - Using two pointers to efficiently track or merge elements.  
3. **Hard Problems** often require combining sorting with two pointers and handling edge cases like duplicates.  

