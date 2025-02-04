
---

### **Easy Problems**

#### 1. [Permutations](https://leetcode.com/problems/permutations/)
**Problem**: Generate all permutations of a list of unique integers.

**Approach**:
1. Use DFS to explore all possible permutations.
2. Swap elements to generate permutations in-place.

**Brute Force (DFS with Backtracking)**:
```python
def permute(nums):
    def dfs(start):
        if start == len(nums):
            result.append(nums[:])
            return
        for i in range(start, len(nums)):
            nums[start], nums[i] = nums[i], nums[start]  # Swap
            dfs(start + 1)
            nums[start], nums[i] = nums[i], nums[start]  # Backtrack
    
    result = []
    dfs(0)
    return result
```
- **Time Complexity**: O(n * n!), where `n` is the number of elements. There are `n!` permutations, and each permutation takes O(n) time to generate.
- **Space Complexity**: O(n!), as we store all permutations.

---

#### 2. [Permutations II](https://leetcode.com/problems/permutations-ii/)
**Problem**: Generate all unique permutations of a list of integers that may contain duplicates.

**Approach**:
1. Use DFS with backtracking.
2. Skip duplicates to avoid generating duplicate permutations.

**Optimized (DFS with Backtracking and Pruning)**:
```python
def permuteUnique(nums):
    def dfs(start):
        if start == len(nums):
            result.append(nums[:])
            return
        used = set()
        for i in range(start, len(nums)):
            if nums[i] in used:
                continue  # Skip duplicates
            used.add(nums[i])
            nums[start], nums[i] = nums[i], nums[start]  # Swap
            dfs(start + 1)
            nums[start], nums[i] = nums[i], nums[start]  # Backtrack
    
    result = []
    dfs(0)
    return result
```
- **Time Complexity**: O(n * n!), where `n` is the number of elements. There are `n!` permutations, and each permutation takes O(n) time to generate.
- **Space Complexity**: O(n!), as we store all unique permutations.

---

#### 3. [Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)
**Problem**: Generate all possible strings by transforming each letter in a string to lowercase or uppercase.

**Approach**:
1. Use DFS to explore all possible combinations of lowercase and uppercase letters.

**Brute Force (DFS)**:
```python
def letterCasePermutation(s):
    def dfs(index, path):
        if index == len(s):
            result.append(path)
            return
        if s[index].isalpha():
            dfs(index + 1, path + s[index].lower())
            dfs(index + 1, path + s[index].upper())
        else:
            dfs(index + 1, path + s[index])
    
    result = []
    dfs(0, "")
    return result
```
- **Time Complexity**: O(2^k * n), where `k` is the number of letters and `n` is the length of the string.
- **Space Complexity**: O(2^k * n), as we store all possible combinations.

---

### **Medium Problems**

#### 4. [Subsets](https://leetcode.com/problems/subsets/)
**Problem**: Generate all possible subsets of a list of unique integers.

**Approach**:
1. Use DFS to explore all possible subsets.
2. Include or exclude each element in the current subset.

**Brute Force (DFS)**:
```python
def subsets(nums):
    def dfs(index, path):
        result.append(path)
        for i in range(index, len(nums)):
            dfs(i + 1, path + [nums[i]])
    
    result = []
    dfs(0, [])
    return result
```
- **Time Complexity**: O(2^n), where `n` is the number of elements. There are `2^n` subsets.
- **Space Complexity**: O(2^n), as we store all subsets.

---

#### 5. [Subsets II](https://leetcode.com/problems/subsets-ii/)
**Problem**: Generate all unique subsets of a list of integers that may contain duplicates.

**Approach**:
1. Use DFS with backtracking.
2. Sort the list and skip duplicates to avoid generating duplicate subsets.

**Optimized (DFS with Backtracking and Pruning)**:
```python
def subsetsWithDup(nums):
    def dfs(index, path):
        result.append(path)
        for i in range(index, len(nums)):
            if i > index and nums[i] == nums[i - 1]:
                continue  # Skip duplicates
            dfs(i + 1, path + [nums[i]])
    
    nums.sort()
    result = []
    dfs(0, [])
    return result
```
- **Time Complexity**: O(2^n), where `n` is the number of elements. There are `2^n` subsets.
- **Space Complexity**: O(2^n), as we store all unique subsets.

---

#### 6. [Combination Sum](https://leetcode.com/problems/combination-sum/)
**Problem**: Find all unique combinations of numbers that sum to a target.

**Approach**:
1. Use DFS with backtracking.
2. Allow reusing the same number multiple times.

**Brute Force (DFS with Backtracking)**:
```python
def combinationSum(candidates, target):
    def dfs(index, path, remaining):
        if remaining == 0:
            result.append(path)
            return
        for i in range(index, len(candidates)):
            if candidates[i] > remaining:
                continue
            dfs(i, path + [candidates[i]], remaining - candidates[i])
    
    result = []
    dfs(0, [], target)
    return result
```
- **Time Complexity**: O(2^t), where `t` is the target value.
- **Space Complexity**: O(2^t), as we store all valid combinations.

---

### **Hard Problems**

#### 7. [Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)
**Problem**: Find the k-th permutation of a sequence of numbers.

**Approach**:
1. Use factorial-based counting to determine the k-th permutation.
2. Build the permutation step by step.

**Optimized (Factorial Counting)**:
```python
def getPermutation(n, k):
    from math import factorial
    numbers = list(range(1, n + 1))
    result = []
    k -= 1  # Convert to 0-based index
    
    while n > 0:
        n -= 1
        fact = factorial(n)
        index = k // fact
        result.append(str(numbers.pop(index)))
        k %= fact
    
    return "".join(result)
```
- **Time Complexity**: O(n^2), where `n` is the number of elements.
- **Space Complexity**: O(n), as we store the result.

---

#### 8. [Palindrome Permutation II](https://leetcode.com/problems/palindrome-permutation-ii/)
**Problem**: Generate all unique palindromic permutations of a string.

**Approach**:
1. Check if the string can form a palindrome.
2. Use DFS to generate permutations of half the string and mirror them.

**Optimized (DFS with Backtracking)**:
```python
def generatePalindromes(s):
    from collections import Counter
    count = Counter(s)
    odd = [char for char, cnt in count.items() if cnt % 2 != 0]
    if len(odd) > 1:
        return []
    
    mid = odd[0] if odd else ""
    half = []
    for char, cnt in count.items():
        half.extend([char] * (cnt // 2))
    
    result = []
    def dfs(index, path):
        if index == len(half):
            result.append("".join(path) + mid + "".join(reversed(path)))
            return
        used = set()
        for i in range(index, len(half)):
            if half[i] in used:
                continue
            used.add(half[i])
            half[index], half[i] = half[i], half[index]
            dfs(index + 1, path + [half[index]])
            half[index], half[i] = half[i], half[index]
    
    dfs(0, [])
    return result
```
- **Time Complexity**: O((n/2)!), where `n` is the length of the string.
- **Space Complexity**: O((n/2)!), as we store all unique palindromic permutations.

---

#### 9. [Word Squares](https://leetcode.com/problems/word-squares/)
**Problem**: Find all word squares that can be formed from a list of words.

**Approach**:
1. Use DFS to build word squares row by row.
2. Use a prefix hash map to optimize word lookup.

**Optimized (DFS with Prefix Hash Map)**:
```python
def wordSquares(words):
    from collections import defaultdict
    prefix_map = defaultdict(list)
    for word in words:
        for i in range(len(word)):
            prefix_map[word[:i + 1]].append(word)
    
    result = []
    def dfs(square):
        if len(square) == len(square[0]):
            result.append(square)
            return
        prefix = "".join([word[len(square)] for word in square])
        for word in prefix_map[prefix]:
            dfs(square + [word])
    
    for word in words:
        dfs([word])
    return result
```
- **Time Complexity**: O(n * k), where `n` is the number of words and `k` is the length of each word.
- **Space Complexity**: O(n * k), as we store the prefix map and result.

---

### **Summary of Python Implementations**
1. **DFS with Backtracking**: The standard approach for generating permutations and combinations.
2. **DFS with Pruning**: Optimizes by skipping duplicates or invalid paths.
3. **Factorial Counting**: Used for problems like finding the k-th permutation.
