
---

## **Easy Problems**

---

### **1. [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)**
**Problem**: Find two numbers in a sorted array that add up to a target.  
**Example**:  
**Input**: `nums = [2,7,11,15], target = 9` → **Output**: `[1,2]` (Indices of 2 and 7).

#### **Brute Force**
**Approach**: Check all pairs of numbers.  
**Time**: O(n²), **Space**: O(1)  
```python
def twoSum_brute(nums: list, target: int) -> list:
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i + 1, j + 1]
    return []
```

#### **Optimized (Converging Two Pointers)**
**Approach**: Use two pointers starting at the ends and move inward.  
**Time**: O(n), **Space**: O(1)  
```python
def twoSum_optimized(nums: list, target: int) -> list:
    left, right = 0, len(nums) - 1
    while left < right:
        current = nums[left] + nums[right]
        if current == target:
            return [left + 1, right + 1]
        elif current < target:
            left += 1
        else:
            right -= 1
    return []
```

---

### **2. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)**
**Problem**: Check if a string is a palindrome after removing non-alphanumeric characters and ignoring case.  
**Example**:  
**Input**: `s = "A man, a plan, a canal: Panama"` → **Output**: `True`.

#### **Brute Force**
**Approach**: Clean the string and compare it to its reverse.  
**Time**: O(n), **Space**: O(n)  
```python
def isPalindrome_brute(s: str) -> bool:
    cleaned = ''.join(c.lower() for c in s if c.isalnum())
    return cleaned == cleaned[::-1]
```

#### **Optimized (Converging Two Pointers)**
**Approach**: Use two pointers to compare characters while skipping non-alphanumeric characters.  
**Time**: O(n), **Space**: O(1)  
```python
def isPalindrome_optimized(s: str) -> bool:
    left, right = 0, len(s) - 1
    while left < right:
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        if s[left].lower() != s[right].lower():
            return False
        left += 1
        right -= 1
    return True
```

---

### **3. [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)**
**Problem**: Return the squares of a sorted array in sorted order.  
**Example**:  
**Input**: `nums = [-4,-1,0,3,10]` → **Output**: `[0,1,9,16,100]`.

#### **Brute Force**
**Approach**: Square all elements and sort.  
**Time**: O(n log n), **Space**: O(n)  
```python
def sortedSquares_brute(nums: list) -> list:
    return sorted(x * x for x in nums)
```

#### **Optimized (Converging Two Pointers)**
**Approach**: Use two pointers to merge squares from both ends.  
**Time**: O(n), **Space**: O(n)  
```python
def sortedSquares_optimized(nums: list) -> list:
    n = len(nums)
    result = [0] * n
    left, right = 0, n - 1
    for i in range(n - 1, -1, -1):
        if abs(nums[left]) > abs(nums[right]):
            result[i] = nums[left] * nums[left]
            left += 1
        else:
            result[i] = nums[right] * nums[right]
            right -= 1
    return result
```

---

## **Medium Problems**

---

### **4. [3Sum](https://leetcode.com/problems/3sum/)**
**Problem**: Find all unique triplets that sum to zero.  
**Example**:  
**Input**: `nums = [-1,0,1,2,-1,-4]` → **Output**: `[[-1,-1,2],[-1,0,1]]`.

#### **Brute Force**
**Approach**: Check all possible triplets.  
**Time**: O(n³), **Space**: O(1)  
```python
def threeSum_brute(nums: list) -> list:
    nums.sort()
    result = []
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            for k in range(j + 1, len(nums)):
                if nums[i] + nums[j] + nums[k] == 0:
                    triplet = [nums[i], nums[j], nums[k]]
                    if triplet not in result:
                        result.append(triplet)
    return result
```

#### **Optimized (Converging Two Pointers)**
**Approach**: Fix one number and use two pointers for the remaining two.  
**Time**: O(n²), **Space**: O(1)  
```python
def threeSum_optimized(nums: list) -> list:
    nums.sort()
    result = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:
            continue  # Skip duplicates
        left, right = i + 1, len(nums) - 1
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            if total < 0:
                left += 1
            elif total > 0:
                right -= 1
            else:
                result.append([nums[i], nums[left], nums[right]])
                while left < right and nums[left] == nums[left + 1]:
                    left += 1  # Skip duplicates
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1  # Skip duplicates
                left += 1
                right -= 1
    return result
```

---

### **5. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)**
**Problem**: Find two lines that form the largest container with the x-axis.  
**Example**:  
**Input**: `height = [1,8,6,2,5,4,8,3,7]` → **Output**: `49`.

#### **Brute Force**
**Approach**: Check all pairs of lines.  
**Time**: O(n²), **Space**: O(1)  
```python
def maxArea_brute(height: list) -> int:
    max_area = 0
    for i in range(len(height)):
        for j in range(i + 1, len(height)):
            area = min(height[i], height[j]) * (j - i)
            max_area = max(max_area, area)
    return max_area
```

#### **Optimized (Converging Two Pointers)**
**Approach**: Use two pointers to maximize the area.  
**Time**: O(n), **Space**: O(1)  
```python
def maxArea_optimized(height: list) -> int:
    left, right = 0, len(height) - 1
    max_area = 0
    while left < right:
        area = min(height[left], height[right]) * (right - left)
        max_area = max(max_area, area)
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    return max_area
```

---

### **6. [3Sum Closest](https://leetcode.com/problems/3sum-closest/)**
**Problem**: Find the triplet with the sum closest to the target.  
**Example**:  
**Input**: `nums = [-1,2,1,-4], target = 1` → **Output**: `2` (Triplet `[-1,2,1]`).

#### **Brute Force**
**Approach**: Check all possible triplets.  
**Time**: O(n³), **Space**: O(1)  
```python
def threeSumClosest_brute(nums: list, target: int) -> int:
    closest = float('inf')
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            for k in range(j + 1, len(nums)):
                total = nums[i] + nums[j] + nums[k]
                if abs(total - target) < abs(closest - target):
                    closest = total
    return closest
```

#### **Optimized (Converging Two Pointers)**
**Approach**: Fix one number and use two pointers for the remaining two.  
**Time**: O(n²), **Space**: O(1)  
```python
def threeSumClosest_optimized(nums: list, target: int) -> int:
    nums.sort()
    closest = float('inf')
    for i in range(len(nums) - 2):
        left, right = i + 1, len(nums) - 1
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            if abs(total - target) < abs(closest - target):
                closest = total
            if total < target:
                left += 1
            else:
                right -= 1
    return closest
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

#### **Optimized (Converging Two Pointers)**
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

### **8. [4Sum](https://leetcode.com/problems/4sum/)**
**Problem**: Find all unique quadruplets that sum to the target.  
**Example**:  
**Input**: `nums = [1,0,-1,0,-2,2], target = 0` → **Output**: `[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]`.

#### **Brute Force**
**Approach**: Check all possible quadruplets.  
**Time**: O(n⁴), **Space**: O(1)  
```python
def fourSum_brute(nums: list, target: int) -> list:
    nums.sort()
    result = []
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            for k in range(j + 1, len(nums)):
                for l in range(k + 1, len(nums)):
                    if nums[i] + nums[j] + nums[k] + nums[l] == target:
                        quadruplet = [nums[i], nums[j], nums[k], nums[l]]
                        if quadruplet not in result:
                            result.append(quadruplet)
    return result
```

#### **Optimized (Converging Two Pointers)**
**Approach**: Fix two numbers and use two pointers for the remaining two.  
**Time**: O(n³), **Space**: O(1)  
```python
def fourSum_optimized(nums: list, target: int) -> list:
    nums.sort()
    result = []
    for i in range(len(nums) - 3):
        if i > 0 and nums[i] == nums[i - 1]:
            continue  # Skip duplicates
        for j in range(i + 1, len(nums) - 2):
            if j > i + 1 and nums[j] == nums[j - 1]:
                continue  # Skip duplicates
            left, right = j + 1, len(nums) - 1
            while left < right:
                total = nums[i] + nums[j] + nums[left] + nums[right]
                if total < target:
                    left += 1
                elif total > target:
                    right -= 1
                else:
                    result.append([nums[i], nums[j], nums[left], nums[right]])
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1  # Skip duplicates
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1  # Skip duplicates
                    left += 1
                    right -= 1
    return result
```

---

### **9. [Valid Triangle Number](https://leetcode.com/problems/valid-triangle-number/)**
**Problem**: Count the number of valid triplets that can form a triangle.  
**Example**:  
**Input**: `nums = [2,2,3,4]` → **Output**: `3`.

#### **Brute Force**
**Approach**: Check all possible triplets.  
**Time**: O(n³), **Space**: O(1)  
```python
def triangleNumber_brute(nums: list) -> int:
    count = 0
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            for k in range(j + 1, len(nums)):
                if nums[i] + nums[j] > nums[k] and nums[i] + nums[k] > nums[j] and nums[j] + nums[k] > nums[i]:
                    count += 1
    return count
```

#### **Optimized (Converging Two Pointers)**
**Approach**: Sort the array and use two pointers to count valid triplets.  
**Time**: O(n²), **Space**: O(1)  
```python
def triangleNumber_optimized(nums: list) -> int:
    nums.sort()
    count = 0
    for i in range(len(nums) - 1, 1, -1):
        left, right = 0, i - 1
        while left < right:
            if nums[left] + nums[right] > nums[i]:
                count += right - left
                right -= 1
            else:
                left += 1
    return count
```

---

### **Key Takeaways**:
1. **Brute Force** approaches are straightforward but inefficient (O(n²) or worse).  
2. **Optimized Converging Two Pointers** methods reduce time to O(n) or O(n²) by:  
   - Sorting the array (if needed).  
   - Using two pointers to efficiently find pairs or triplets.  
3. **Hard Problems** often require combining sorting with two pointers and handling edge cases like duplicates.  

