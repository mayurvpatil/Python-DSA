
---

### **Unbounded Knapsack Problems**

#### **Easy**
1. **[Coin Change](https://leetcode.com/problems/coin-change/)**
   - **Problem**: Given coins of different denominations and a total amount, find the minimum number of coins needed to make up that amount.
   - **Approach**:
     - Brute Force: Recursively try all combinations of coins.
     - Optimized: Use dynamic programming (DP) to find the minimum number of coins.
   - **Time Complexity**:
     - Brute Force: \(O(\text{amount}^n)\), where \(n\) is the number of coins.
     - Optimized: \(O(n \cdot \text{amount})\)
   - **Space Complexity**:
     - Brute Force: \(O(\text{amount})\) (recursion stack)
     - Optimized: \(O(\text{amount})\)

   ```python
   def coinChange(coins, amount):
       dp = [float('inf')] * (amount + 1)
       dp[0] = 0
       for coin in coins:
           for i in range(coin, amount + 1):
               dp[i] = min(dp[i], dp[i - coin] + 1)
       return dp[amount] if dp[amount] != float('inf') else -1
   ```

2. **[Minimum Cost For Tickets](https://leetcode.com/problems/minimum-cost-for-tickets/)**
   - **Problem**: Given days of travel and costs for 1-day, 7-day, and 30-day passes, find the minimum cost to travel all days.
   - **Approach**:
     - Brute Force: Recursively try all combinations of passes.
     - Optimized: Use DP to calculate the minimum cost for each day.
   - **Time Complexity**:
     - Brute Force: \(O(3^n)\)
     - Optimized: \(O(n)\), where \(n\) is the number of days.
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n)\)

   ```python
   def mincostTickets(days, costs):
       dp = [0] * (days[-1] + 1)
       for i in range(1, len(dp)):
           if i not in days:
               dp[i] = dp[i - 1]
           else:
               dp[i] = min(
                   dp[i - 1] + costs[0],
                   dp[max(0, i - 7)] + costs[1],
                   dp[max(0, i - 30)] + costs[2]
               )
       return dp[-1]
   ```

3. **[Perfect Squares](https://leetcode.com/problems/perfect-squares/)**
   - **Problem**: Given a positive integer `n`, find the minimum number of perfect square numbers that sum to `n`.
   - **Approach**:
     - Brute Force: Recursively try all combinations of perfect squares.
     - Optimized: Use DP to find the minimum number of perfect squares.
   - **Time Complexity**:
     - Brute Force: \(O(n^n)\)
     - Optimized: \(O(n \cdot \sqrt{n})\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n)\)

   ```python
   def numSquares(n):
       dp = [float('inf')] * (n + 1)
       dp[0] = 0
       for i in range(1, int(n**0.5) + 1):
           square = i * i
           for j in range(square, n + 1):
               dp[j] = min(dp[j], dp[j - square] + 1)
       return dp[n]
   ```

---

#### **Medium**
4. **[Coin Change II](https://leetcode.com/problems/coin-change-ii/)**
   - **Problem**: Given coins of different denominations and a total amount, find the number of combinations to make up that amount.
   - **Approach**:
     - Brute Force: Recursively try all combinations of coins.
     - Optimized: Use DP to count the number of ways to make up the amount.
   - **Time Complexity**:
     - Brute Force: \(O(\text{amount}^n)\)
     - Optimized: \(O(n \cdot \text{amount})\)
   - **Space Complexity**:
     - Brute Force: \(O(\text{amount})\) (recursion stack)
     - Optimized: \(O(\text{amount})\)

   ```python
   def change(amount, coins):
       dp = [0] * (amount + 1)
       dp[0] = 1
       for coin in coins:
           for i in range(coin, amount + 1):
               dp[i] += dp[i - coin]
       return dp[amount]
   ```

5. **[Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/)**
   - **Problem**: Given an array of distinct integers and a target, find the number of possible combinations that add up to the target.
   - **Approach**:
     - Brute Force: Recursively try all combinations.
     - Optimized: Use DP to count the number of ways to reach the target.
   - **Time Complexity**:
     - Brute Force: \(O(\text{target}^n)\)
     - Optimized: \(O(n \cdot \text{target})\)
   - **Space Complexity**:
     - Brute Force: \(O(\text{target})\) (recursion stack)
     - Optimized: \(O(\text{target})\)

   ```python
   def combinationSum4(nums, target):
       dp = [0] * (target + 1)
       dp[0] = 1
       for i in range(1, target + 1):
           for num in nums:
               if i >= num:
                   dp[i] += dp[i - num]
       return dp[target]
   ```

6. **[Minimum Falling Path Sum II](https://leetcode.com/problems/minimum-falling-path-sum-ii/)**
   - **Problem**: Given a grid, find the minimum sum of a falling path where you can move to any column in the next row.
   - **Approach**:
     - Brute Force: Recursively try all paths.
     - Optimized: Use DP to track the minimum sum for each cell.
   - **Time Complexity**:
     - Brute Force: \(O(n \cdot m^n)\)
     - Optimized: \(O(n \cdot m^2)\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n \cdot m)\)

   ```python
   def minFallingPathSum(grid):
       n = len(grid)
       for i in range(1, n):
           for j in range(n):
               grid[i][j] += min(grid[i - 1][k] for k in range(n) if k != j)
       return min(grid[-1])
   ```

---

#### **Hard**
7. **[Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)**
   - **Problem**: Given jobs with start time, end time, and profit, find the maximum profit without overlapping jobs.
   - **Approach**:
     - Brute Force: Recursively try all combinations of jobs.
     - Optimized: Use DP with binary search to find the maximum profit.
   - **Time Complexity**:
     - Brute Force: \(O(2^n)\)
     - Optimized: \(O(n \log n)\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n)\)

   ```python
   def jobScheduling(startTime, endTime, profit):
       jobs = sorted(zip(startTime, endTime, profit), key=lambda x: x[1])
       dp = [[0, 0]]
       for s, e, p in jobs:
           i = bisect.bisect_right(dp, [s + 1]) - 1
           if dp[i][1] + p > dp[-1][1]:
               dp.append([e, dp[i][1] + p])
       return dp[-1][1]
   ```

8. **[Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/)**
   - **Problem**: Given an array of binary strings and limits `m` (0s) and `n` (1s), find the maximum number of strings you can form.
   - **Approach**:
     - Brute Force: Recursively try all combinations of strings.
     - Optimized: Use DP to track the maximum number of strings for given `m` and `n`.
   - **Time Complexity**:
     - Brute Force: \(O(2^n)\)
     - Optimized: \(O(l \cdot m \cdot n)\), where \(l\) is the number of strings.
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(m \cdot n)\)

   ```python
   def findMaxForm(strs, m, n):
       dp = [[0] * (n + 1) for _ in range(m + 1)]
       for s in strs:
           zeros = s.count('0')
           ones = len(s) - zeros
           for i in range(m, zeros - 1, -1):
               for j in range(n, ones - 1, -1):
                   dp[i][j] = max(dp[i][j], dp[i - zeros][j - ones] + 1)
       return dp[m][n]
   ```

9. **[Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)**
   - **Problem**: Given a target distance, starting fuel, and gas stations, find the minimum number of refuels needed to reach the target.
   - **Approach**:
     - Brute Force: Recursively try all combinations of refuels.
     - Optimized: Use a max-heap to greedily refuel at the most optimal stations.
   - **Time Complexity**:
     - Brute Force: \(O(2^n)\)
     - Optimized: \(O(n \log n)\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n)\)

   ```python
   import heapq
   def minRefuelStops(target, startFuel, stations):
       heap = []
       stops = 0
       prev = 0
       fuel = startFuel
       for location, capacity in stations + [(target, 0)]:
           fuel -= (location - prev)
           while heap and fuel < 0:
               fuel += -heapq.heappop(heap)
               stops += 1
           if fuel < 0:
               return -1
           heapq.heappush(heap, -capacity)
           prev = location
       return stops
   ```

---

### **Summary of Approaches**
1. **Brute Force**:
   - Recursively explore all possible combinations.
   - Time: \(O(\text{amount}^n)\), Space: \(O(\text{amount})\) (recursion stack).

2. **Optimized DP**:
   - Use a DP table to store intermediate results.
   - Time: \(O(n \cdot \text{amount})\), Space: \(O(\text{amount})\).

3. **Greedy + Heap**:
   - Use a max-heap to make optimal choices at each step.
   - Time: \(O(n \log n)\), Space: \(O(n)\).

---

