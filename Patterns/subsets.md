
---

### **Easy Problems**

#### 1. [Subsets](https://leetcode.com/problems/subsets/)
**Problem**: Generate all possible subsets of a list of unique integers.

**Approach**:
1. Use backtracking to explore all possible subsets.
2. Include or exclude each element in the current subset.

**Brute Force (Backtracking)**:
```python
def subsets(nums):
    def backtrack(start, path):
        result.append(path)
        for i in range(start, len(nums)):
            backtrack(i + 1, path + [nums[i]])
    
    result = []
    backtrack(0, [])
    return result
```
- **Time Complexity**: O(2^n), where `n` is the number of elements. There are `2^n` subsets.
- **Space Complexity**: O(2^n), as we store all subsets.

---

#### 2. [Subsets II](https://leetcode.com/problems/subsets-ii/)
**Problem**: Generate all unique subsets of a list of integers that may contain duplicates.

**Approach**:
1. Sort the list to handle duplicates.
2. Use backtracking with pruning to skip duplicate elements.

**Optimized (Backtracking with Pruning)**:
```python
def subsetsWithDup(nums):
    def backtrack(start, path):
        result.append(path)
        for i in range(start, len(nums)):
            if i > start and nums[i] == nums[i - 1]:
                continue  # Skip duplicates
            backtrack(i + 1, path + [nums[i]])
    
    nums.sort()
    result = []
    backtrack(0, [])
    return result
```
- **Time Complexity**: O(2^n), where `n` is the number of elements. There are `2^n` subsets.
- **Space Complexity**: O(2^n), as we store all unique subsets.

---

#### 3. [Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)
**Problem**: Generate all possible strings by transforming each letter in a string to lowercase or uppercase.

**Approach**:
1. Use backtracking to explore all possible combinations of lowercase and uppercase letters.

**Brute Force (Backtracking)**:
```python
def letterCasePermutation(s):
    def backtrack(index, path):
        if index == len(s):
            result.append(path)
            return
        if s[index].isalpha():
            backtrack(index + 1, path + s[index].lower())
            backtrack(index + 1, path + s[index].upper())
        else:
            backtrack(index + 1, path + s[index])
    
    result = []
    backtrack(0, "")
    return result
```
- **Time Complexity**: O(2^k * n), where `k` is the number of letters and `n` is the length of the string.
- **Space Complexity**: O(2^k * n), as we store all possible combinations.

---

### **Medium Problems**

#### 4. [Combination Sum](https://leetcode.com/problems/combination-sum/)
**Problem**: Find all unique combinations of numbers that sum to a target.

**Approach**:
1. Use backtracking to explore all possible combinations.
2. Allow reusing the same number multiple times.

**Brute Force (Backtracking)**:
```python
def combinationSum(candidates, target):
    def backtrack(start, path, remaining):
        if remaining == 0:
            result.append(path)
            return
        for i in range(start, len(candidates)):
            if candidates[i] > remaining:
                continue
            backtrack(i, path + [candidates[i]], remaining - candidates[i])
    
    result = []
    backtrack(0, [], target)
    return result
```
- **Time Complexity**: O(2^t), where `t` is the target value.
- **Space Complexity**: O(2^t), as we store all valid combinations.

---

#### 5. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)
**Problem**: Find all unique combinations of numbers that sum to a target, where each number can only be used once.

**Approach**:
1. Sort the list to handle duplicates.
2. Use backtracking with pruning to skip duplicate elements.

**Optimized (Backtracking with Pruning)**:
```python
def combinationSum2(candidates, target):
    def backtrack(start, path, remaining):
        if remaining == 0:
            result.append(path)
            return
        for i in range(start, len(candidates)):
            if i > start and candidates[i] == candidates[i - 1]:
                continue  # Skip duplicates
            if candidates[i] > remaining:
                break
            backtrack(i + 1, path + [candidates[i]], remaining - candidates[i])
    
    candidates.sort()
    result = []
    backtrack(0, [], target)
    return result
```
- **Time Complexity**: O(2^n), where `n` is the number of elements.
- **Space Complexity**: O(2^n), as we store all unique combinations.

---

#### 6. [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)
**Problem**: Find all unique combinations of k numbers that sum to n, using numbers from 1 to 9.

**Approach**:
1. Use backtracking to explore all possible combinations.
2. Ensure the combination size is exactly k.

**Brute Force (Backtracking)**:
```python
def combinationSum3(k, n):
    def backtrack(start, path, remaining):
        if len(path) == k and remaining == 0:
            result.append(path)
            return
        for i in range(start, 10):
            if i > remaining:
                break
            backtrack(i + 1, path + [i], remaining - i)
    
    result = []
    backtrack(1, [], n)
    return result
```
- **Time Complexity**: O(9^k), as we explore combinations of size `k`.
- **Space Complexity**: O(9^k), as we store all valid combinations.

---

### **Hard Problems**

#### 7. [Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)
**Problem**: Determine if an array can be partitioned into k subsets with equal sums.

**Approach**:
1. Use backtracking to explore all possible partitions.
2. Track the sum of each subset and ensure it matches the target.

**Brute Force (Backtracking)**:
```python
def canPartitionKSubsets(nums, k):
    total = sum(nums)
    if total % k != 0:
        return False
    target = total // k
    used = [False] * len(nums)
    
    def backtrack(start, k, current_sum):
        if k == 1:
            return True
        if current_sum == target:
            return backtrack(0, k - 1, 0)
        for i in range(start, len(nums)):
            if not used[i] and current_sum + nums[i] <= target:
                used[i] = True
                if backtrack(i + 1, k, current_sum + nums[i]):
                    return True
                used[i] = False
        return False
    
    return backtrack(0, k, 0)
```
- **Time Complexity**: O(k * 2^n), where `n` is the number of elements.
- **Space Complexity**: O(n), as we track used elements.

---

#### 8. [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)
**Problem**: Partition a string into all possible palindrome substrings.

**Approach**:
1. Use backtracking to explore all possible partitions.
2. Check if each substring is a palindrome.

**Brute Force (Backtracking)**:
```python
def partition(s):
    def is_palindrome(s):
        return s == s[::-1]
    
    def backtrack(start, path):
        if start == len(s):
            result.append(path)
            return
        for i in range(start, len(s)):
            if is_palindrome(s[start:i + 1]):
                backtrack(i + 1, path + [s[start:i + 1]])
    
    result = []
    backtrack(0, [])
    return result
```
- **Time Complexity**: O(n * 2^n), where `n` is the length of the string.
- **Space Complexity**: O(n * 2^n), as we store all valid partitions.

---

#### 9. [Word Break II](https://leetcode.com/problems/word-break-ii/)
**Problem**: Given a string and a dictionary, generate all possible sentences by breaking the string into valid words.

**Approach**:
1. Use backtracking to explore all possible word breaks.
2. Use memoization to optimize repeated subproblems.

**Optimized (Backtracking with Memoization)**:
```python
def wordBreak(s, wordDict):
    def backtrack(s, memo):
        if s in memo:
            return memo[s]
        if not s:
            return [""]
        result = []
        for word in wordDict:
            if s.startswith(word):
                for substr in backtrack(s[len(word):], memo):
                    if substr:
                        result.append(word + " " + substr)
                    else:
                        result.append(word)
        memo[s] = result
        return result
    
    return backtrack(s, {})
```
- **Time Complexity**: O(n * 2^n), where `n` is the length of the string.
- **Space Complexity**: O(n * 2^n), as we store all valid sentences.

---

### **Summary of Python Implementations**
1. **Backtracking**: The standard approach for generating subsets and combinations.
2. **Backtracking with Pruning**: Optimizes by skipping duplicates or invalid paths.
3. **Backtracking with Memoization**: Optimizes repeated subproblems (e.g., Word Break II).

