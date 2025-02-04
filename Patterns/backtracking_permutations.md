

### **Permutations Problems**

#### **Easy**
1. **[Permutations](https://leetcode.com/problems/permutations/)**
   - **Problem**: Given an array of distinct integers, return all possible permutations.
   - **Approach**:
     - Brute Force: Recursively generate all permutations by swapping elements.
     - Optimized: Use backtracking to generate permutations efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(n! \cdot n)\)
     - Optimized: \(O(n!)\)
   - **Space Complexity**:
     - Brute Force: \(O(n!)\) (output space)
     - Optimized: \(O(n!)\) (output space)

   ```python
   def permute(nums):
       def backtrack(start):
           if start == len(nums):
               result.append(nums[:])
               return
           for i in range(start, len(nums)):
               nums[start], nums[i] = nums[i], nums[start]
               backtrack(start + 1)
               nums[start], nums[i] = nums[i], nums[start]
       result = []
       backtrack(0)
       return result
   ```

2. **[Permutations II](https://leetcode.com/problems/permutations-ii/)**
   - **Problem**: Given an array of integers that may contain duplicates, return all unique permutations.
   - **Approach**:
     - Brute Force: Recursively generate all permutations and filter duplicates.
     - Optimized: Use backtracking with pruning to avoid duplicate permutations.
   - **Time Complexity**:
     - Brute Force: \(O(n! \cdot n)\)
     - Optimized: \(O(n!)\)
   - **Space Complexity**:
     - Brute Force: \(O(n!)\) (output space)
     - Optimized: \(O(n!)\) (output space)

   ```python
   def permuteUnique(nums):
       def backtrack(start):
           if start == len(nums):
               result.append(nums[:])
               return
           used = set()
           for i in range(start, len(nums)):
               if nums[i] in used:
                   continue
               used.add(nums[i])
               nums[start], nums[i] = nums[i], nums[start]
               backtrack(start + 1)
               nums[start], nums[i] = nums[i], nums[start]
       result = []
       backtrack(0)
       return result
   ```

3. **[Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)**
   - **Problem**: Given a string, transform every letter individually to be lowercase or uppercase to create another string.
   - **Approach**:
     - Brute Force: Recursively generate all combinations of lowercase and uppercase letters.
     - Optimized: Use backtracking to generate combinations efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(2^n \cdot n)\)
     - Optimized: \(O(2^n)\)
   - **Space Complexity**:
     - Brute Force: \(O(2^n)\) (output space)
     - Optimized: \(O(2^n)\) (output space)

   ```python
   def letterCasePermutation(s):
       def backtrack(index, path):
           if index == len(s):
               result.append(path)
               return
           if s[index].isalpha():
               backtrack(index + 1, path + s[index].upper())
               backtrack(index + 1, path + s[index].lower())
           else:
               backtrack(index + 1, path + s[index])
       result = []
       backtrack(0, "")
       return result
   ```

---

#### **Medium**
4. **[Next Permutation](https://leetcode.com/problems/next-permutation/)**
   - **Problem**: Given an array of integers, rearrange it into the lexicographically next greater permutation.
   - **Approach**:
     - Brute Force: Generate all permutations and find the next one.
     - Optimized: Use a single pass to find the next permutation.
   - **Time Complexity**:
     - Brute Force: \(O(n!)\)
     - Optimized: \(O(n)\)
   - **Space Complexity**:
     - Brute Force: \(O(n!)\) (output space)
     - Optimized: \(O(1)\)

   ```python
   def nextPermutation(nums):
       i = len(nums) - 2
       while i >= 0 and nums[i] >= nums[i + 1]:
           i -= 1
       if i >= 0:
           j = len(nums) - 1
           while j >= 0 and nums[j] <= nums[i]:
               j -= 1
           nums[i], nums[j] = nums[j], nums[i]
       left, right = i + 1, len(nums) - 1
       while left < right:
           nums[left], nums[right] = nums[right], nums[left]
           left += 1
           right -= 1
   ```

5. **[Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)**
   - **Problem**: Given `n` and `k`, return the `k`-th permutation sequence of `n` numbers.
   - **Approach**:
     - Brute Force: Generate all permutations and return the `k`-th one.
     - Optimized: Use factorial decomposition to construct the `k`-th permutation directly.
   - **Time Complexity**:
     - Brute Force: \(O(n!)\)
     - Optimized: \(O(n^2)\)
   - **Space Complexity**:
     - Brute Force: \(O(n!)\) (output space)
     - Optimized: \(O(n)\)

   ```python
   def getPermutation(n, k):
       factorials = [1]
       for i in range(1, n):
           factorials.append(factorials[-1] * i)
       numbers = list(range(1, n + 1))
       result = []
       k -= 1
       for i in range(n - 1, -1, -1):
           index = k // factorials[i]
           result.append(str(numbers.pop(index)))
           k %= factorials[i]
       return "".join(result)
   ```

6. **[Palindrome Permutation II](https://leetcode.com/problems/palindrome-permutation-ii/)**
   - **Problem**: Given a string, return all possible palindromic permutations of it.
   - **Approach**:
     - Brute Force: Generate all permutations and filter palindromic ones.
     - Optimized: Use backtracking to generate only valid palindromic permutations.
   - **Time Complexity**:
     - Brute Force: \(O(n!)\)
     - Optimized: \(O((n/2)!)\)
   - **Space Complexity**:
     - Brute Force: \(O(n!)\) (output space)
     - Optimized: \(O((n/2)!)\) (output space)

   ```python
   def generatePalindromes(s):
       from collections import Counter
       count = Counter(s)
       odd = [char for char, freq in count.items() if freq % 2 != 0]
       if len(odd) > 1:
           return []
       mid = odd[0] if odd else ""
       half = "".join([char * (freq // 2) for char, freq in count.items()])
       result = []
       def backtrack(path, used):
           if len(path) == len(half):
               result.append(path + mid + path[::-1])
               return
           for i in range(len(half)):
               if used[i] or (i > 0 and half[i] == half[i - 1] and not used[i - 1]):
                   continue
               used[i] = True
               backtrack(path + half[i], used)
               used[i] = False
       backtrack("", [False] * len(half))
       return result
   ```

---

#### **Hard**
7. **[Number of Squareful Arrays](https://leetcode.com/problems/number-of-squareful-arrays/)**
   - **Problem**: Given an array of integers, return the number of permutations that are squareful.
   - **Approach**:
     - Brute Force: Generate all permutations and check if they are squareful.
     - Optimized: Use backtracking with pruning to count valid permutations.
   - **Time Complexity**:
     - Brute Force: \(O(n!)\)
     - Optimized: \(O(n!)\)
   - **Space Complexity**:
     - Brute Force: \(O(n!)\) (output space)
     - Optimized: \(O(n!)\) (output space)

   ```python
   def numSquarefulPerms(nums):
       def is_square(x):
           return int(x**0.5)**2 == x
       def backtrack(path, used):
           if len(path) == len(nums):
               result.append(path)
               return
           for i in range(len(nums)):
               if used[i] or (i > 0 and nums[i] == nums[i - 1] and not used[i - 1]):
                   continue
               if path and not is_square(path[-1] + nums[i]):
                   continue
               used[i] = True
               backtrack(path + [nums[i]], used)
               used[i] = False
       nums.sort()
       result = []
       backtrack([], [False] * len(nums))
       return len(result)
   ```

8. **[Permutations with Duplicates](https://leetcode.com/problems/permutations-ii/)**
   - **Problem**: Given an array of integers that may contain duplicates, return all unique permutations.
   - **Approach**:
     - Brute Force: Generate all permutations and filter duplicates.
     - Optimized: Use backtracking with pruning to avoid duplicate permutations.
   - **Time Complexity**:
     - Brute Force: \(O(n!)\)
     - Optimized: \(O(n!)\)
   - **Space Complexity**:
     - Brute Force: \(O(n!)\) (output space)
     - Optimized: \(O(n!)\) (output space)

   ```python
   def permuteUnique(nums):
       def backtrack(start):
           if start == len(nums):
               result.append(nums[:])
               return
           used = set()
           for i in range(start, len(nums)):
               if nums[i] in used:
                   continue
               used.add(nums[i])
               nums[start], nums[i] = nums[i], nums[start]
               backtrack(start + 1)
               nums[start], nums[i] = nums[i], nums[start]
       result = []
       backtrack(0)
       return result
   ```

9. **[Beautiful Arrangement](https://leetcode.com/problems/beautiful-arrangement/)**
   - **Problem**: Given an integer `n`, count the number of valid permutations where each element satisfies a specific condition.
   - **Approach**:
     - Brute Force: Generate all permutations and count valid ones.
     - Optimized: Use backtracking with pruning to count valid permutations.
   - **Time Complexity**:
     - Brute Force: \(O(n!)\)
     - Optimized: \(O(k)\), where \(k\) is the number of valid permutations.
   - **Space Complexity**:
     - Brute Force: \(O(n!)\) (output space)
     - Optimized: \(O(n)\) (recursion stack)

   ```python
   def countArrangement(n):
       def backtrack(index, used):
           if index > n:
               nonlocal count
               count += 1
               return
           for i in range(1, n + 1):
               if not used[i] and (i % index == 0 or index % i == 0):
                   used[i] = True
                   backtrack(index + 1, used)
                   used[i] = False
       count = 0
       backtrack(1, [False] * (n + 1))
       return count
   ```

---

### **Summary of Approaches**
1. **Brute Force**:
   - Recursively generate all permutations.
   - Time: \(O(n!)\), Space: \(O(n!)\) (output space).

2. **Optimized Backtracking**:
   - Use backtracking with pruning to avoid invalid or duplicate permutations.
   - Time: \(O(n!)\), Space: \(O(n!)\) (output space).

3. **Factorial Decomposition**:
   - Construct the `k`-th permutation directly using factorial decomposition.
   - Time: \(O(n^2)\), Space: \(O(n)\).

---
