
---

### **Bit Counting Problems**

#### **Easy**
1. **[Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)**
   - **Problem**: Write a function that takes an unsigned integer and returns the number of `1` bits it has (also known as the Hamming weight).
   - **Approach**:
     - Brute Force: Iterate through all bits and count `1`s.
     - Optimized: Use bitwise operations to count `1`s efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(\log n)\)
     - Optimized: \(O(k)\), where \(k\) is the number of `1` bits.
   - **Space Complexity**:
     - Brute Force: \(O(1)\)
     - Optimized: \(O(1)\)

   ```python
   def hammingWeight(n):
       count = 0
       while n:
           count += n & 1
           n >>= 1
       return count
   ```

2. **[Counting Bits](https://leetcode.com/problems/counting-bits/)**
   - **Problem**: Given an integer `n`, return an array of length `n + 1` where each element is the number of `1` bits in its binary representation.
   - **Approach**:
     - Brute Force: For each number, count `1` bits using a loop.
     - Optimized: Use dynamic programming to compute the number of `1` bits.
   - **Time Complexity**:
     - Brute Force: \(O(n \log n)\)
     - Optimized: \(O(n)\)
   - **Space Complexity**:
     - Brute Force: \(O(1)\)
     - Optimized: \(O(n)\)

   ```python
   def countBits(n):
       result = [0] * (n + 1)
       for i in range(1, n + 1):
           result[i] = result[i & (i - 1)] + 1
       return result
   ```

3. **[Binary Number with Alternating Bits](https://leetcode.com/problems/binary-number-with-alternating-bits/)**
   - **Problem**: Given a positive integer, check if it has alternating bits.
   - **Approach**:
     - Brute Force: Convert the number to binary and check for alternating bits.
     - Optimized: Use bitwise operations to check for alternating bits.
   - **Time Complexity**:
     - Brute Force: \(O(\log n)\)
     - Optimized: \(O(1)\)
   - **Space Complexity**:
     - Brute Force: \(O(\log n)\)
     - Optimized: \(O(1)\)

   ```python
   def hasAlternatingBits(n):
       prev = n & 1
       n >>= 1
       while n:
           curr = n & 1
           if curr == prev:
               return False
           prev = curr
           n >>= 1
       return True
   ```

---

#### **Medium**
4. **[Total Hamming Distance](https://leetcode.com/problems/total-hamming-distance/)**
   - **Problem**: Given an array of integers, return the sum of Hamming distances between all pairs of numbers.
   - **Approach**:
     - Brute Force: Compute the Hamming distance for all pairs.
     - Optimized: Count the number of `1`s and `0`s at each bit position and compute the total Hamming distance.
   - **Time Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n \log m)\), where \(m\) is the maximum number.
   - **Space Complexity**:
     - Brute Force: \(O(1)\)
     - Optimized: \(O(1)\)

   ```python
   def totalHammingDistance(nums):
       total = 0
       for i in range(32):
           count = 0
           for num in nums:
               count += (num >> i) & 1
           total += count * (len(nums) - count)
       return total
   ```

5. **[Hamming Distance](https://leetcode.com/problems/hamming-distance/)**
   - **Problem**: Given two integers, return the Hamming distance between them.
   - **Approach**:
     - Brute Force: Convert both numbers to binary and count differing bits.
     - Optimized: Use XOR to find differing bits and count them.
   - **Time Complexity**:
     - Brute Force: \(O(\log n)\)
     - Optimized: \(O(1)\)
   - **Space Complexity**:
     - Brute Force: \(O(\log n)\)
     - Optimized: \(O(1)\)

   ```python
   def hammingDistance(x, y):
       xor = x ^ y
       count = 0
       while xor:
           count += xor & 1
           xor >>= 1
       return count
   ```

6. **[Binary Watch](https://leetcode.com/problems/binary-watch/)**
   - **Problem**: Given a non-negative integer representing the number of LEDs that are currently on, return all possible times the watch could represent.
   - **Approach**:
     - Brute Force: Generate all possible times and count the number of `1` bits.
     - Optimized: Use bitwise operations to generate valid times.
   - **Time Complexity**:
     - Brute Force: \(O(12 \cdot 60)\)
     - Optimized: \(O(12 \cdot 60)\)
   - **Space Complexity**:
     - Brute Force: \(O(1)\)
     - Optimized: \(O(1)\)

   ```python
   def readBinaryWatch(turnedOn):
       result = []
       for h in range(12):
           for m in range(60):
               if bin(h).count('1') + bin(m).count('1') == turnedOn:
                   result.append(f"{h}:{m:02d}")
       return result
   ```

---

#### **Hard**
7. **[Maximum Product of Word Lengths](https://leetcode.com/problems/maximum-product-of-word-lengths/)**
   - **Problem**: Given a string array, return the maximum value of `length(word[i]) * length(word[j])` where the two words do not share common letters.
   - **Approach**:
     - Brute Force: Check all pairs of words for common letters.
     - Optimized: Use bitmasks to represent words and check for common letters efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(n^2 \cdot m)\), where \(m\) is the average word length.
     - Optimized: \(O(n^2)\)
   - **Space Complexity**:
     - Brute Force: \(O(1)\)
     - Optimized: \(O(n)\)

   ```python
   def maxProduct(words):
       masks = [0] * len(words)
       for i, word in enumerate(words):
           for char in word:
               masks[i] |= 1 << (ord(char) - ord('a'))
       max_prod = 0
       for i in range(len(words)):
           for j in range(i + 1, len(words)):
               if masks[i] & masks[j] == 0:
                   max_prod = max(max_prod, len(words[i]) * len(words[j]))
       return max_prod
   ```

8. **[Number of Valid Words for Each Puzzle](https://leetcode.com/problems/number-of-valid-words-for-each-puzzle/)**
   - **Problem**: Given a list of words and a list of puzzles, return the number of valid words for each puzzle.
   - **Approach**:
     - Brute Force: Check each word against each puzzle.
     - Optimized: Use bitmasks and a frequency map to count valid words efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(n \cdot m)\), where \(n\) is the number of words and \(m\) is the number of puzzles.
     - Optimized: \(O(n + m)\)
   - **Space Complexity**:
     - Brute Force: \(O(1)\)
     - Optimized: \(O(n)\)

   ```python
   def findNumOfValidWords(words, puzzles):
       from collections import defaultdict
       freq = defaultdict(int)
       for word in words:
           mask = 0
           for char in word:
               mask |= 1 << (ord(char) - ord('a'))
           freq[mask] += 1
       result = []
       for puzzle in puzzles:
           mask = 0
           for char in puzzle:
               mask |= 1 << (ord(char) - ord('a'))
           first_char = 1 << (ord(puzzle[0]) - ord('a'))
           count = 0
           submask = mask
           while submask:
               if submask & first_char:
                   count += freq.get(submask, 0)
               submask = (submask - 1) & mask
           result.append(count)
       return result
   ```

9. **[Minimum Number of Flips to Convert Binary Matrix to Zero Matrix](https://leetcode.com/problems/minimum-number-of-flips-to-convert-binary-matrix-to-zero-matrix/)**
   - **Problem**: Given a binary matrix, find the minimum number of flips required to convert it to a zero matrix.
   - **Approach**:
     - Brute Force: Try all possible flip combinations.
     - Optimized: Use BFS with bitmasking to find the minimum number of flips.
   - **Time Complexity**:
     - Brute Force: \(O(2^{m \cdot n})\)
     - Optimized: \(O(m \cdot n \cdot 2^{m \cdot n})\)
   - **Space Complexity**:
     - Brute Force: \(O(2^{m \cdot n})\)
     - Optimized: \(O(2^{m \cdot n})\)

   ```python
   def minFlips(mat):
       from collections import deque
       m, n = len(mat), len(mat[0])
       start = 0
       for i in range(m):
           for j in range(n):
               start |= mat[i][j] << (i * n + j)
       queue = deque([(start, 0)])
       visited = set([start])
       while queue:
           state, steps = queue.popleft()
           if state == 0:
               return steps
           for i in range(m):
               for j in range(n):
                   new_state = state
                   for dx, dy in [(0, 0), (0, 1), (0, -1), (1, 0), (-1, 0)]:
                       x, y = i + dx, j + dy
                       if 0 <= x < m and 0 <= y < n:
                           new_state ^= 1 << (x * n + y)
                   if new_state not in visited:
                       visited.add(new_state)
                       queue.append((new_state, steps + 1))
       return -1
   ```

---

### **Summary of Approaches**
1. **Brute Force**:
   - Iterate through all bits or pairs and count `1`s or check conditions.
   - Time: \(O(n)\) to \(O(n^2)\), Space: \(O(1)\).

2. **Optimized Bit Manipulation**:
   - Use bitwise operations to count `1`s or check conditions efficiently.
   - Time: \(O(n)\), Space: \(O(1)\).

3. **Dynamic Programming**:
   - Use DP to compute the number of `1` bits or solve complex problems.
   - Time: \(O(n)\), Space: \(O(n)\).

4. **BFS with Bitmasking**:
   - Use BFS to explore states and find the minimum number of flips.
   - Time: \(O(m \cdot n \cdot 2^{m \cdot n})\), Space: \(O(2^{m \cdot n})\).

---

