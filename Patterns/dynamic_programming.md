
---

### **Easy Problems**

---

#### 1. **Climbing Stairs**  
**Link:** [LeetCode #70](https://leetcode.com/problems/climbing-stairs/)  
**Problem:** You are climbing a staircase. It takes `n` steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

---

**Brute Force (Recursive):**  
```python
def climbStairs(n):
    if n == 0 or n == 1:
        return 1
    return climbStairs(n - 1) + climbStairs(n - 2)
```
- **Time Complexity:** O(2^n) (Exponential due to repeated calculations).  
- **Space Complexity:** O(n) (Recursion stack).

---

**Memoization (Top-Down DP):**  
```python
def climbStairs(n, memo={}):
    if n in memo:
        return memo[n]
    if n == 0 or n == 1:
        return 1
    memo[n] = climbStairs(n - 1, memo) + climbStairs(n - 2, memo)
    return memo[n]
```
- **Time Complexity:** O(n) (Each subproblem is solved once).  
- **Space Complexity:** O(n) (Memoization storage + recursion stack).

---

**Tabulation (Bottom-Up DP):**  
```python
def climbStairs(n):
    if n == 1:
        return 1
    dp = [0] * (n + 1)
    dp[0], dp[1] = 1, 1
    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    return dp[n]
```
- **Time Complexity:** O(n).  
- **Space Complexity:** O(n) (DP array).

---

**Optimized Space (Fibonacci Sequence):**  
```python
def climbStairs(n):
    if n == 1:
        return 1
    prev1, prev2 = 1, 1
    for _ in range(2, n + 1):
        curr = prev1 + prev2
        prev2 = prev1
        prev1 = curr
    return prev1
```
- **Time Complexity:** O(n).  
- **Space Complexity:** O(1) (Constant space).

---

#### 2. **Maximum Subarray**  
**Link:** [LeetCode #53](https://leetcode.com/problems/maximum-subarray/)  
**Problem:** Find the contiguous subarray with the largest sum.

---

**Brute Force:**  
```python
def maxSubArray(nums):
    max_sum = float('-inf')
    for i in range(len(nums)):
        current_sum = 0
        for j in range(i, len(nums)):
            current_sum += nums[j]
            max_sum = max(max_sum, current_sum)
    return max_sum
```
- **Time Complexity:** O(n^2).  
- **Space Complexity:** O(1).

---

**Kadane's Algorithm (Optimized DP):**  
```python
def maxSubArray(nums):
    max_sum = current_sum = nums[0]
    for num in nums[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)
    return max_sum
```
- **Time Complexity:** O(n).  
- **Space Complexity:** O(1).

---

#### 3. **House Robber**  
**Link:** [LeetCode #198](https://leetcode.com/problems/house-robber/)  
**Problem:** Rob houses to maximize profit without alerting police (can't rob adjacent houses).

---

**Brute Force (Recursive):**  
```python
def rob(nums):
    def helper(i):
        if i >= len(nums):
            return 0
        return max(nums[i] + helper(i + 2), helper(i + 1))
    return helper(0)
```
- **Time Complexity:** O(2^n).  
- **Space Complexity:** O(n).

---

**Memoization (Top-Down DP):**  
```python
def rob(nums):
    memo = {}
    def helper(i):
        if i in memo:
            return memo[i]
        if i >= len(nums):
            return 0
        memo[i] = max(nums[i] + helper(i + 2), helper(i + 1))
        return memo[i]
    return helper(0)
```
- **Time Complexity:** O(n).  
- **Space Complexity:** O(n).

---

**Tabulation (Bottom-Up DP):**  
```python
def rob(nums):
    if not nums:
        return 0
    dp = [0] * (len(nums) + 1)
    dp[1] = nums[0]
    for i in range(2, len(nums) + 1):
        dp[i] = max(nums[i - 1] + dp[i - 2], dp[i - 1])
    return dp[-1]
```
- **Time Complexity:** O(n).  
- **Space Complexity:** O(n).

---

**Optimized Space:**  
```python
def rob(nums):
    prev1 = prev2 = 0
    for num in nums:
        curr = max(prev1, prev2 + num)
        prev2 = prev1
        prev1 = curr
    return prev1
```
- **Time Complexity:** O(n).  
- **Space Complexity:** O(1).

---

### **Medium Problems**

---

#### 1. **Coin Change**  
**Link:** [LeetCode #322](https://leetcode.com/problems/coin-change/)  
**Problem:** Find the minimum number of coins to make a given amount.

---

**Brute Force (Recursive):**  
```python
def coinChange(coins, amount):
    def helper(remaining):
        if remaining == 0:
            return 0
        if remaining < 0:
            return float('inf')
        min_coins = float('inf')
        for coin in coins:
            res = helper(remaining - coin)
            if res != float('inf'):
                min_coins = min(min_coins, res + 1)
        return min_coins
    res = helper(amount)
    return res if res != float('inf') else -1
```
- **Time Complexity:** O(amount^n).  
- **Space Complexity:** O(amount).

---

**Memoization (Top-Down DP):**  
```python
def coinChange(coins, amount):
    memo = {}
    def helper(remaining):
        if remaining in memo:
            return memo[remaining]
        if remaining == 0:
            return 0
        if remaining < 0:
            return float('inf')
        min_coins = float('inf')
        for coin in coins:
            res = helper(remaining - coin)
            if res != float('inf'):
                min_coins = min(min_coins, res + 1)
        memo[remaining] = min_coins
        return memo[remaining]
    res = helper(amount)
    return res if res != float('inf') else -1
```
- **Time Complexity:** O(amount * n).  
- **Space Complexity:** O(amount).

---

**Tabulation (Bottom-Up DP):**  
```python
def coinChange(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1
```
- **Time Complexity:** O(amount * n).  
- **Space Complexity:** O(amount).

---

#### 2. **Longest Increasing Subsequence**  
**Link:** [LeetCode #300](https://leetcode.com/problems/longest-increasing-subsequence/)  
**Problem:** Find the length of the longest strictly increasing subsequence.

---

**Brute Force (Recursive):**  
```python
def lengthOfLIS(nums):
    def helper(i, prev):
        if i == len(nums):
            return 0
        taken = 0
        if nums[i] > prev:
            taken = 1 + helper(i + 1, nums[i])
        not_taken = helper(i + 1, prev)
        return max(taken, not_taken)
    return helper(0, float('-inf'))
```
- **Time Complexity:** O(2^n).  
- **Space Complexity:** O(n).

---

**Memoization (Top-Down DP):**  
```python
def lengthOfLIS(nums):
    memo = {}
    def helper(i, prev):
        if (i, prev) in memo:
            return memo[(i, prev)]
        if i == len(nums):
            return 0
        taken = 0
        if nums[i] > prev:
            taken = 1 + helper(i + 1, nums[i])
        not_taken = helper(i + 1, prev)
        memo[(i, prev)] = max(taken, not_taken)
        return memo[(i, prev)]
    return helper(0, float('-inf'))
```
- **Time Complexity:** O(n^2).  
- **Space Complexity:** O(n^2).

---

**Tabulation (Bottom-Up DP):**  
```python
def lengthOfLIS(nums):
    dp = [1] * len(nums)
    for i in range(1, len(nums)):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp)
```
- **Time Complexity:** O(n^2).  
- **Space Complexity:** O(n).

---

#### 3. **Unique Paths**  
**Link:** [LeetCode #62](https://leetcode.com/problems/unique-paths/)  
**Problem:** Find the number of unique paths from the top-left to the bottom-right of a grid.

---

**Brute Force (Recursive):**  
```python
def uniquePaths(m, n):
    def helper(i, j):
        if i == m - 1 and j == n - 1:
            return 1
        if i >= m or j >= n:
            return 0
        return helper(i + 1, j) + helper(i, j + 1)
    return helper(0, 0)
```
- **Time Complexity:** O(2^(m+n)).  
- **Space Complexity:** O(m + n).

---

**Memoization (Top-Down DP):**  
```python
def uniquePaths(m, n):
    memo = {}
    def helper(i, j):
        if (i, j) in memo:
            return memo[(i, j)]
        if i == m - 1 and j == n - 1:
            return 1
        if i >= m or j >= n:
            return 0
        memo[(i, j)] = helper(i + 1, j) + helper(i, j + 1)
        return memo[(i, j)]
    return helper(0, 0)
```
- **Time Complexity:** O(m * n).  
- **Space Complexity:** O(m * n).

---

**Tabulation (Bottom-Up DP):**  
```python
def uniquePaths(m, n):
    dp = [[1] * n for _ in range(m)]
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
    return dp[-1][-1]
```
- **Time Complexity:** O(m * n).  
- **Space Complexity:** O(m * n).

---

### **Hard Problems**

---

#### 1. **Edit Distance**  
**Link:** [LeetCode #72](https://leetcode.com/problems/edit-distance/)  
**Problem:** Find the minimum number of operations (insert, delete, replace) to convert one string to another.

---

**Brute Force (Recursive):**  
```python
def minDistance(word1, word2):
    def helper(i, j):
        if i == len(word1):
            return len(word2) - j
        if j == len(word2):
            return len(word1) - i
        if word1[i] == word2[j]:
            return helper(i + 1, j + 1)
        return 1 + min(helper(i + 1, j), helper(i, j + 1), helper(i + 1, j + 1))
    return helper(0, 0)
```
- **Time Complexity:** O(3^(m+n)).  
- **Space Complexity:** O(m + n).

---

**Memoization (Top-Down DP):**  
```python
def minDistance(word1, word2):
    memo = {}
    def helper(i, j):
        if (i, j) in memo:
            return memo[(i, j)]
        if i == len(word1):
            return len(word2) - j
        if j == len(word2):
            return len(word1) - i
        if word1[i] == word2[j]:
            memo[(i, j)] = helper(i + 1, j + 1)
        else:
            memo[(i, j)] = 1 + min(helper(i + 1, j), helper(i, j + 1), helper(i + 1, j + 1))
        return memo[(i, j)]
    return helper(0, 0)
```
- **Time Complexity:** O(m * n).  
- **Space Complexity:** O(m * n).

---

**Tabulation (Bottom-Up DP):**  
```python
def minDistance(word1, word2):
    m, n = len(word1), len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        for j in range(n + 1):
            if i == 0:
                dp[i][j] = j
            elif j == 0:
                dp[i][j] = i
            elif word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
    return dp[m][n]
```
- **Time Complexity:** O(m * n).  
- **Space Complexity:** O(m * n).

---

#### 2. **Best Time to Buy and Sell Stock with Cooldown**  
**Link:** [LeetCode #309](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)  
**Problem:** Maximize profit with a cooldown period after selling.

---

**Brute Force (Recursive):**  
```python
def maxProfit(prices):
    def helper(i, can_buy):
        if i >= len(prices):
            return 0
        if can_buy:
            return max(-prices[i] + helper(i + 1, False), helper(i + 1, True))
        else:
            return max(prices[i] + helper(i + 2, True), helper(i + 1, False))
    return helper(0, True)
```
- **Time Complexity:** O(2^n).  
- **Space Complexity:** O(n).

---

**Memoization (Top-Down DP):**  
```python
def maxProfit(prices):
    memo = {}
    def helper(i, can_buy):
        if (i, can_buy) in memo:
            return memo[(i, can_buy)]
        if i >= len(prices):
            return 0
        if can_buy:
            memo[(i, can_buy)] = max(-prices[i] + helper(i + 1, False), helper(i + 1, True))
        else:
            memo[(i, can_buy)] = max(prices[i] + helper(i + 2, True), helper(i + 1, False))
        return memo[(i, can_buy)]
    return helper(0, True)
```
- **Time Complexity:** O(n).  
- **Space Complexity:** O(n).

---

**Tabulation (Bottom-Up DP):**  
```python
def maxProfit(prices):
    n = len(prices)
    dp = [[0] * 2 for _ in range(n + 2)]
    for i in range(n - 1, -1, -1):
        dp[i][1] = max(-prices[i] + dp[i + 1][0], dp[i + 1][1])
        dp[i][0] = max(prices[i] + dp[i + 2][1], dp[i + 1][0])
    return dp[0][1]
```
- **Time Complexity:** O(n).  
- **Space Complexity:** O(n).

---

#### 3. **Regular Expression Matching**  
**Link:** [LeetCode #10](https://leetcode.com/problems/regular-expression-matching/)  
**Problem:** Implement regular expression matching with support for `.` and `*`.

---

**Brute Force (Recursive):**  
```python
def isMatch(s, p):
    if not p:
        return not s
    first_match = bool(s) and p[0] in {s[0], '.'}
    if len(p) >= 2 and p[1] == '*':
        return isMatch(s, p[2:]) or (first_match and isMatch(s[1:], p))
    else:
        return first_match and isMatch(s[1:], p[1:])
```
- **Time Complexity:** O(2^(m+n)).  
- **Space Complexity:** O(m + n).

---

**Memoization (Top-Down DP):**  
```python
def isMatch(s, p):
    memo = {}
    def helper(i, j):
        if (i, j) in memo:
            return memo[(i, j)]
        if j == len(p):
            return i == len(s)
        first_match = i < len(s) and p[j] in {s[i], '.'}
        if j + 1 < len(p) and p[j + 1] == '*':
            memo[(i, j)] = helper(i, j + 2) or (first_match and helper(i + 1, j))
        else:
            memo[(i, j)] = first_match and helper(i + 1, j + 1)
        return memo[(i, j)]