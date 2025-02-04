
---

### **0/1 Knapsack Problems**

#### **Easy**
1. **[Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)**
   - **Problem**: Given a non-empty array `nums`, determine if it can be partitioned into two subsets with equal sums.
   - **Approach**:
     - Brute Force: Recursively check all subsets.
     - Optimized: Use dynamic programming (DP) to check if a subset sums to `total_sum / 2`.
   - **Time Complexity**:
     - Brute Force: \(O(2^n)\)
     - Optimized: \(O(n \cdot \text{sum})\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n \cdot \text{sum})\)

   ```python
   def canPartition(nums):
       total = sum(nums)
       if total % 2 != 0:
           return False
       target = total // 2
       dp = [False] * (target + 1)
       dp[0] = True
       for num in nums:
           for i in range(target, num - 1, -1):
               dp[i] = dp[i] or dp[i - num]
       return dp[target]
   ```

2. **[Target Sum](https://leetcode.com/problems/target-sum/)**
   - **Problem**: Given an array `nums` and a target `S`, find the number of ways to assign `+` or `-` to each element to sum to `S`.
   - **Approach**:
     - Brute Force: Recursively try all combinations of `+` and `-`.
     - Optimized: Use DP to count the number of subsets with a given sum.
   - **Time Complexity**:
     - Brute Force: \(O(2^n)\)
     - Optimized: \(O(n \cdot \text{sum})\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n \cdot \text{sum})\)

   ```python
   def findTargetSumWays(nums, S):
       total = sum(nums)
       if (total + S) % 2 != 0 or total < abs(S):
           return 0
       target = (total + S) // 2
       dp = [0] * (target + 1)
       dp[0] = 1
       for num in nums:
           for i in range(target, num - 1, -1):
               dp[i] += dp[i - num]
       return dp[target]
   ```

3. **[Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)**
   - **Problem**: Given an array of stone weights, minimize the weight of the last stone by smashing stones.
   - **Approach**:
     - Brute Force: Recursively try all combinations of smashing stones.
     - Optimized: Use DP to partition the stones into two subsets with minimum difference.
   - **Time Complexity**:
     - Brute Force: \(O(2^n)\)
     - Optimized: \(O(n \cdot \text{sum})\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n \cdot \text{sum})\)

   ```python
   def lastStoneWeightII(stones):
       total = sum(stones)
       target = total // 2
       dp = [False] * (target + 1)
       dp[0] = True
       for stone in stones:
           for i in range(target, stone - 1, -1):
               dp[i] = dp[i] or dp[i - stone]
       for i in range(target, -1, -1):
           if dp[i]:
               return total - 2 * i
       return 0
   ```

---

#### **Medium**
4. **[Coin Change II](https://leetcode.com/problems/coin-change-ii/)**
   - **Problem**: Given coins of different denominations and a total amount, find the number of combinations to make up that amount.
   - **Approach**:
     - Brute Force: Recursively try all combinations of coins.
     - Optimized: Use DP to count the number of ways to make up the amount.
   - **Time Complexity**:
     - Brute Force: \(O(2^n)\)
     - Optimized: \(O(n \cdot \text{amount})\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n \cdot \text{amount})\)

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
     - Brute Force: \(O(2^n)\)
     - Optimized: \(O(n \cdot \text{target})\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n \cdot \text{target})\)

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

6. **[Minimum Cost For Tickets](https://leetcode.com/problems/minimum-cost-for-tickets/)**
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
   - Time: \(O(2^n)\), Space: \(O(n)\) (recursion stack).

2. **Optimized DP**:
   - Use a DP table to store intermediate results.
   - Time: \(O(n \cdot \text{sum})\), Space: \(O(n \cdot \text{sum})\).

3. **Greedy + Heap**:
   - Use a max-heap to make optimal choices at each step.
   - Time: \(O(n \log n)\), Space: \(O(n)\).

---

