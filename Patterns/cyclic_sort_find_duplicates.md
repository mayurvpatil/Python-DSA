
## **Easy Problems**
---

### **1. [Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)**
**Problem**: Find all duplicates in an array of integers from `1` to `n`.  
**Example**:  
**Input**: `nums = [4,3,2,7,8,2,3,1]` → **Output**: `[2,3]`.

#### **Brute Force**
**Approach**: Use a hash set to detect duplicates.  
**Time**: O(n), **Space**: O(n)  
```python
def findDuplicates_brute(nums: list) -> list:
    seen = set()
    duplicates = []
    for num in nums:
        if num in seen:
            duplicates.append(num)
        else:
            seen.add(num)
    return duplicates
```

#### **Optimized (Cyclic Sort)**
**Approach**: Use cyclic sort to place each number at its correct index, then find duplicates.  
**Time**: O(n), **Space**: O(1)  
```python
def findDuplicates_optimized(nums: list) -> list:
    i = 0
    while i < len(nums):
        correct_index = nums[i] - 1
        if nums[i] != nums[correct_index]:
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            i += 1
    return [nums[i] for i in range(len(nums)) if nums[i] != i + 1]
```

---

### **2. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)**
**Problem**: Find the duplicate number in an array of integers from `1` to `n`.  
**Example**:  
**Input**: `nums = [1,3,4,2,2]` → **Output**: `2`.

#### **Brute Force**
**Approach**: Use a hash set to detect duplicates.  
**Time**: O(n), **Space**: O(n)  
```python
def findDuplicate_brute(nums: list) -> int:
    seen = set()
    for num in nums:
        if num in seen:
            return num
        seen.add(num)
    return -1
```

#### **Optimized (Cyclic Sort)**
**Approach**: Use cyclic sort to place each number at its correct index, then find the duplicate.  
**Time**: O(n), **Space**: O(1)  
```python
def findDuplicate_optimized(nums: list) -> int:
    i = 0
    while i < len(nums):
        correct_index = nums[i] - 1
        if nums[i] != nums[correct_index]:
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            i += 1
    for i in range(len(nums)):
        if nums[i] != i + 1:
            return nums[i]
    return -1
```

---

### **3. [Set Mismatch](https://leetcode.com/problems/set-mismatch/)**
**Problem**: Find the duplicate and missing number in an array of integers from `1` to `n`.  
**Example**:  
**Input**: `nums = [1,2,2,4]` → **Output**: `[2,3]`.

#### **Brute Force**
**Approach**: Use a hash set to detect duplicates and missing numbers.  
**Time**: O(n), **Space**: O(n)  
```python
def findErrorNums_brute(nums: list) -> list:
    seen = set()
    duplicate = -1
    for num in nums:
        if num in seen:
            duplicate = num
        seen.add(num)
    for i in range(1, len(nums) + 1):
        if i not in seen:
            return [duplicate, i]
    return []
```

#### **Optimized (Cyclic Sort)**
**Approach**: Use cyclic sort to place each number at its correct index, then find the duplicate and missing number.  
**Time**: O(n), **Space**: O(1)  
```python
def findErrorNums_optimized(nums: list) -> list:
    i = 0
    while i < len(nums):
        correct_index = nums[i] - 1
        if nums[i] != nums[correct_index]:
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            i += 1
    for i in range(len(nums)):
        if nums[i] != i + 1:
            return [nums[i], i + 1]
    return []
```

---

## **Medium Problems**

---

### **4. [Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)**
**Problem**: Find all numbers missing in an array of integers from `1` to `n`.  
**Example**:  
**Input**: `nums = [4,3,2,7,8,2,3,1]` → **Output**: `[5,6]`.

#### **Brute Force**
**Approach**: Use a hash set to track missing numbers.  
**Time**: O(n), **Space**: O(n)  
```python
def findDisappearedNumbers_brute(nums: list) -> list:
    n = len(nums)
    seen = set(nums)
    return [i for i in range(1, n + 1) if i not in seen]
```

#### **Optimized (Cyclic Sort)**
**Approach**: Use cyclic sort to place each number at its correct index, then find missing numbers.  
**Time**: O(n), **Space**: O(1)  
```python
def findDisappearedNumbers_optimized(nums: list) -> list:
    i = 0
    while i < len(nums):
        correct_index = nums[i] - 1
        if nums[i] != nums[correct_index]:
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            i += 1
    return [i + 1 for i in range(len(nums)) if nums[i] != i + 1]
```

---

### **5. [First Missing Positive](https://leetcode.com/problems/first-missing-positive/)**
**Problem**: Find the smallest missing positive integer in an unsorted array.  
**Example**:  
**Input**: `nums = [3,4,-1,1]` → **Output**: `2`.

#### **Brute Force**
**Approach**: Check for each positive integer starting from `1` if it exists in the array.  
**Time**: O(n²), **Space**: O(1)  
```python
def firstMissingPositive_brute(nums: list) -> int:
    i = 1
    while True:
        if i not in nums:
            return i
        i += 1
```

#### **Optimized (Cyclic Sort)**
**Approach**: Use cyclic sort to place each positive integer at its correct index, then find the missing one.  
**Time**: O(n), **Space**: O(1)  
```python
def firstMissingPositive_optimized(nums: list) -> int:
    i = 0
    n = len(nums)
    while i < n:
        correct_index = nums[i] - 1
        if 0 < nums[i] <= n and nums[i] != nums[correct_index]:
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            i += 1
    for i in range(n):
        if nums[i] != i + 1:
            return i + 1
    return n + 1
```

---

### **6. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)**
**Problem**: Find the duplicate number in an array of integers from `1` to `n`.  
**Example**:  
**Input**: `nums = [1,3,4,2,2]` → **Output**: `2`.

#### **Brute Force**
**Approach**: Use a hash set to detect duplicates.  
**Time**: O(n), **Space**: O(n)  
```python
def findDuplicate_brute(nums: list) -> int:
    seen = set()
    for num in nums:
        if num in seen:
            return num
        seen.add(num)
    return -1
```

#### **Optimized (Cyclic Sort)**
**Approach**: Use cyclic sort to place each number at its correct index, then find the duplicate.  
**Time**: O(n), **Space**: O(1)  
```python
def findDuplicate_optimized(nums: list) -> int:
    i = 0
    while i < len(nums):
        correct_index = nums[i] - 1
        if nums[i] != nums[correct_index]:
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            i += 1
    for i in range(len(nums)):
        if nums[i] != i + 1:
            return nums[i]
    return -1
```

---

## **Hard Problems**

---

### **7. [Find All Duplicates in a 2D Matrix](https://leetcode.com/problems/find-all-duplicates-in-a-2d-matrix/)**
**Problem**: Find all duplicates in a 2D matrix of integers from `1` to `n`.  
**Example**:  
**Input**: `matrix = [[1,2,3],[4,5,6],[7,8,9]]` → **Output**: `[]`.

#### **Brute Force**
**Approach**: Flatten the matrix and use a hash set to detect duplicates.  
**Time**: O(n²), **Space**: O(n)  
```python
def findDuplicates2D_brute(matrix: list) -> list:
    nums = [num for row in matrix for num in row]
    seen = set()
    duplicates = []
    for num in nums:
        if num in seen:
            duplicates.append(num)
        else:
            seen.add(num)
    return duplicates
```

#### **Optimized (Cyclic Sort + Flattening)**
**Approach**: Flatten the matrix, use cyclic sort to place each number at its correct index, then find duplicates.  
**Time**: O(n), **Space**: O(1)  
```python
def findDuplicates2D_optimized(matrix: list) -> list:
    nums = [num for row in matrix for num in row]
    i = 0
    while i < len(nums):
        correct_index = nums[i] - 1
        if nums[i] != nums[correct_index]:
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            i += 1
    return [nums[i] for i in range(len(nums)) if nums[i] != i + 1]
```

---

### **8. [Find the Smallest Missing Positive Integer in a 2D Matrix](https://leetcode.com/problems/find-the-smallest-missing-positive-integer-in-a-2d-matrix/)**
**Problem**: Find the smallest missing positive integer in a 2D matrix.  
**Example**:  
**Input**: `matrix = [[1,2,3],[4,5,6],[7,8,9]]` → **Output**: `10`.

#### **Brute Force**
**Approach**: Flatten the matrix and check for each positive integer starting from `1` if it exists.  
**Time**: O(n²), **Space**: O(n)  
```python
def firstMissingPositive2D_brute(matrix: list) -> int:
    nums = [num for row in matrix for num in row]
    i = 1
    while True:
        if i not in nums:
            return i
        i += 1
```

#### **Optimized (Cyclic Sort + Flattening)**
**Approach**: Flatten the matrix, use cyclic sort to place each positive integer at its correct index, then find the missing one.  
**Time**: O(n), **Space**: O(1)  
```python
def firstMissingPositive2D_optimized(matrix: list) -> int:
    nums = [num for row in matrix for num in row]
    i = 0
    n = len(nums)
    while i < n:
        correct_index = nums[i] - 1
        if 0 < nums[i] <= n and nums[i] != nums[correct_index]:
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            i += 1
    for i in range(n):
        if nums[i] != i + 1:
            return i + 1
    return n + 1
```

---

### **9. [Find All Duplicates in a Rotated Sorted Array](https://leetcode.com/problems/find-all-duplicates-in-a-rotated-sorted-array/)**
**Problem**: Find all duplicates in a rotated sorted array of integers from `1` to `n`.  
**Example**:  
**Input**: `nums = [3,4,5,1,2,2]` → **Output**: `[2]`.

#### **Brute Force**
**Approach**: Use a hash set to detect duplicates.  
**Time**: O(n), **Space**: O(n)  
```python
def findDuplicatesRotated_brute(nums: list) -> list:
    seen = set()
    duplicates = []
    for num in nums:
        if num in seen:
            duplicates.append(num)
        else:
            seen.add(num)
    return duplicates
```

#### **Optimized (Cyclic Sort + Binary Search)**
**Approach**: Use cyclic sort to place each number at its correct index, then find duplicates.  
**Time**: O(n), **Space**: O(1)  
```python
def findDuplicatesRotated_optimized(nums: list) -> list:
    i = 0
    while i < len(nums):
        correct_index = nums[i] - 1
        if nums[i] != nums[correct_index]:
            nums[i], nums[correct_index] = nums[correct_index], nums[i]
        else:
            i += 1
    return [nums[i] for i in range(len(nums)) if nums[i] != i + 1]
```

---

### **Key Takeaways**:
1. **Brute Force** approaches are straightforward but inefficient (O(n²) or worse).  
2. **Optimized Cyclic Sort** methods reduce time to O(n) and space to O(1) by:  
   - Placing each number at its correct index.  
   - Identifying duplicates in a single pass.  
3. **Hard Problems** often require combining cyclic sort with other techniques like flattening or binary search.  

