
---

## **Easy Problems**

---

### **1. [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)**
**Problem**: Find the maximum average of any contiguous subarray of length `k`.  
**Example**:  
**Input**: `nums = [1,12,-5,-6,50,3], k = 4` → **Output**: `12.75` (Subarray `[12,-5,-6,50]`).

#### **Brute Force**
**Approach**: Check every subarray of length `k` and compute the average.  
**Time**: O(nk), **Space**: O(1)  
```python
def findMaxAverage_brute(nums: list, k: int) -> float:
    max_avg = float('-inf')
    for i in range(len(nums) - k + 1):
        current_sum = sum(nums[i:i+k])
        max_avg = max(max_avg, current_sum / k)
    return max_avg
```

#### **Optimized (Sliding Window)**
**Approach**: Compute the sum once, then slide and update.  
**Time**: O(n), **Space**: O(1)  
```python
def findMaxAverage_optimized(nums: list, k: int) -> float:
    current_sum = sum(nums[:k])
    max_sum = current_sum
    for i in range(k, len(nums)):
        current_sum += nums[i] - nums[i - k]
        max_sum = max(max_sum, current_sum)
    return max_sum / k
```

---

### **2. [Maximum Number of Vowels in a Substring](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)**
**Problem**: Find the maximum number of vowels in any substring of length `k`.  
**Example**:  
**Input**: `s = "abciiidef", k = 3` → **Output**: `3` ("iii").

#### **Brute Force**
**Approach**: Check vowels in every window of size `k`.  
**Time**: O(nk), **Space**: O(1)  
```python
def maxVowels_brute(s: str, k: int) -> int:
    vowels = {'a', 'e', 'i', 'o', 'u'}
    max_count = 0
    for i in range(len(s) - k + 1):
        current = sum(1 for c in s[i:i+k] if c in vowels)
        max_count = max(max_count, current)
    return max_count
```

#### **Optimized (Fixed Window)**
**Approach**: Track vowels as the window slides.  
**Time**: O(n), **Space**: O(1)  
```python
def maxVowels_optimized(s: str, k: int) -> int:
    vowels = {'a', 'e', 'i', 'o', 'u'}
    max_count = current = sum(1 for c in s[:k] if c in vowels)
    for i in range(k, len(s)):
        current += (s[i] in vowels) - (s[i - k] in vowels)
        max_count = max(max_count, current)
    return max_count
```

---

### **3. [Find All K-Distant Indices](https://leetcode.com/problems/find-all-k-distant-indices-in-an-array/)**
**Problem**: Find indices `i` where there exists `j` with `|i - j| ≤ k` and `nums[j] == key`.  
**Example**:  
**Input**: `nums = [3,4,9,1,3,9,5], key = 9, k = 1` → **Output**: `[1,2,3,4,5,6]`.

#### **Brute Force**
**Approach**: Check all indices for each occurrence of `key`.  
**Time**: O(n²), **Space**: O(n)  
```python
def findKDistantIndices_brute(nums: list, key: int, k: int) -> list:
    key_indices = [i for i, num in enumerate(nums) if num == key]
    result = set()
    for j in key_indices:
        for i in range(max(0, j - k), min(len(nums), j + k + 1)):
            result.add(i)
    return sorted(result)
```

#### **Optimized (Merge Intervals)**
**Approach**: Track ranges and merge overlapping intervals.  
**Time**: O(n), **Space**: O(n)  
```python
def findKDistantIndices_optimized(nums: list, key: int, k: int) -> list:
    intervals = []
    for i, num in enumerate(nums):
        if num == key:
            intervals.append([max(0, i - k), min(len(nums) - 1, i + k)])
    # Merge intervals
    merged = []
    for start, end in sorted(intervals):
        if merged and merged[-1][1] >= start - 1:
            merged[-1][1] = max(merged[-1][1], end)
        else:
            merged.append([start, end])
    # Generate indices
    result = []
    for start, end in merged:
        result.extend(range(start, end + 1))
    return result
```

---

## **Medium Problems**

---

### **4. [Permutation in String](https://leetcode.com/problems/permutation-in-string/)**
**Problem**: Check if `s2` contains a permutation of `s1`.  
**Example**:  
**Input**: `s1 = "ab", s2 = "eidbaooo"` → **Output**: `True` ("ba" is a permutation).

#### **Brute Force**
**Approach**: Check all substrings of `s2` of length `len(s1)`.  
**Time**: O(n²), **Space**: O(1)  
```python
def checkInclusion_brute(s1: str, s2: str) -> bool:
    from collections import Counter
    target = Counter(s1)
    k = len(s1)
    for i in range(len(s2) - k + 1):
        if Counter(s2[i:i+k]) == target:
            return True
    return False
```

#### **Optimized (Frequency Sliding Window)**
**Approach**: Track character frequencies in a fixed window.  
**Time**: O(n), **Space**: O(1)  
```python
def checkInclusion_optimized(s1: str, s2: str) -> bool:
    from collections import defaultdict
    target = defaultdict(int)
    window = defaultdict(int)
    for c in s1:
        target[c] += 1
    k = len(s1)
    # Initialize first window
    for c in s2[:k]:
        window[c] += 1
    if window == target:
        return True
    # Slide window
    for i in range(k, len(s2)):
        left_char = s2[i - k]
        right_char = s2[i]
        window[left_char] -= 1
        if window[left_char] == 0:
            del window[left_char]
        window[right_char] += 1
        if window == target:
            return True
    return False
```

---

### **5. [Diet Plan Performance](https://leetcode.com/problems/diet-plan-performance/)**
**Problem**: Calculate total points for a diet plan where each window of `k` days scores `+1` if sum > `upper`, `-1` if sum < `lower`.  
**Example**:  
**Input**: `calories = [6,5,0,0], k = 2, lower = 1, upper = 5` → **Output**: `0`.

#### **Brute Force**
**Approach**: Compute sum for every window.  
**Time**: O(nk), **Space**: O(1)  
```python
def dietPlanPerformance_brute(calories: list, k: int, lower: int, upper: int) -> int:
    total = 0
    for i in range(len(calories) - k + 1):
        s = sum(calories[i:i+k])
        if s > upper:
            total += 1
        elif s < lower:
            total -= 1
    return total
```

#### **Optimized (Fixed Window Sum)**
**Approach**: Use sliding window to compute sums.  
**Time**: O(n), **Space**: O(1)  
```python
def dietPlanPerformance_optimized(calories: list, k: int, lower: int, upper: int) -> int:
    total = current_sum = 0
    # Initialize first window
    for i in range(k):
        current_sum += calories[i]
    # Check first window
    if current_sum > upper:
        total += 1
    elif current_sum < lower:
        total -= 1
    # Slide window
    for i in range(k, len(calories)):
        current_sum += calories[i] - calories[i - k]
        if current_sum > upper:
            total += 1
        elif current_sum < lower:
            total -= 1
    return total
```

---

### **6. [Maximum Sum of Two Non-Overlapping Subarrays](https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays/)**
**Problem**: Find the maximum sum of two non-overlapping subarrays of lengths `L` and `M`.  
**Example**:  
**Input**: `nums = [0,6,5,2,2,5,1,9,4], L = 1, M = 2` → **Output**: `20`.

#### **Brute Force**
**Approach**: Check all possible pairs of subarrays.  
**Time**: O(n²), **Space**: O(1)  
```python
def maxSumTwoNoOverlap_brute(nums: list, L: int, M: int) -> int:
    max_sum = 0
    n = len(nums)
    for i in range(n - L + 1):
        sum_L = sum(nums[i:i+L])
        for j in range(n - M + 1):
            if j + M <= i or j >= i + L:
                sum_M = sum(nums[j:j+M])
                max_sum = max(max_sum, sum_L + sum_M)
    return max_sum
```

#### **Optimized (Prefix Sum + Sliding Window)**
**Approach**: Precompute prefix sums and track max L/M windows.  
**Time**: O(n), **Space**: O(n)  
```python
def maxSumTwoNoOverlap_optimized(nums: list, L: int, M: int) -> int:
    prefix = [0] * (len(nums) + 1)
    for i in range(len(nums)):
        prefix[i+1] = prefix[i] + nums[i]
    max_L = max_M = res = 0
    # Case 1: L before M
    for i in range(L + M, len(prefix)):
        sum_L = prefix[i - M] - prefix[i - M - L]
        sum_M = prefix[i] - prefix[i - M]
        max_L = max(max_L, sum_L)
        res = max(res, max_L + sum_M)
    # Case 2: M before L
    for i in range(L + M, len(prefix)):
        sum_M = prefix[i - L] - prefix[i - L - M]
        sum_L = prefix[i] - prefix[i - L]
        max_M = max(max_M, sum_M)
        res = max(res, max_M + sum_L)
    return res
```

---

## **Hard Problems**

---

### **7. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)**
**Problem**: Return the max element in every sliding window of size `k`.  
**Example**:  
**Input**: `nums = [1,3,-1,-3,5,3,6,7], k = 3` → **Output**: `[3,3,5,5,6,7]`.

#### **Brute Force**
**Approach**: For each window, compute the maximum.  
**Time**: O(nk), **Space**: O(1)  
```python
def maxSlidingWindow_brute(nums: list, k: int) -> list:
    return [max(nums[i:i+k]) for i in range(len(nums) - k + 1)]
```

#### **Optimized (Deque)**
**Approach**: Use a deque to track potential maxima.  
**Time**: O(n), **Space**: O(k)  
```python
def maxSlidingWindow_optimized(nums: list, k: int) -> list:
    from collections import deque
    q = deque()  # Stores indices of elements in decreasing order
    result = []
    for i in range(len(nums)):
        # Remove elements outside the window
        while q and q[0] <= i - k:
            q.popleft()
        # Remove smaller elements from the end
        while q and nums[i] >= nums[q[-1]]:
            q.pop()
        q.append(i)
        # Add max to result once window is formed
        if i >= k - 1:
            result.append(nums[q[0]])
    return result
```

---

### **8. [Minimum Number of K Consecutive Bit Flips](https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips/)**
**Problem**: Find the minimum number of bit flips in windows of size `k` to make all bits `1`.  
**Example**:  
**Input**: `nums = [0,0,0,1,0,1,1,0], k = 3` → **Output**: `3`.

#### **Brute Force**
**Approach**: Flip bits greedily and track changes.  
**Time**: O(nk), **Space**: O(n)  
```python
def minKBitFlips_brute(nums: list, k: int) -> int:
    n = len(nums)
    flipped = [0] * n
    res = 0
    for i in range(n - k + 1):
        if (nums[i] + flipped[i]) % 2 == 0:
            res += 1
            for j in range(i, i + k):
                flipped[j] += 1
    # Check if all bits are 1 after flips
    for i in range(n):
        if (nums[i] + flipped[i]) % 2 == 0:
            return -1
    return res
```

#### **Optimized (Greedy + Sliding Window)**
**Approach**: Track flip effects using a difference array.  
**Time**: O(n), **Space**: O(n)  
```python
def minKBitFlips_optimized(nums: list, k: int) -> int:
    n = len(nums)
    flip_diff = [0] * (n + 1)  # Tracks net flips at each position
    res = current_flips = 0
    for i in range(n):
        current_flips += flip_diff[i]
        if (nums[i] + current_flips) % 2 == 0:
            if i + k > n:
                return -1
            res += 1
            current_flips += 1
            flip_diff[i + k] -= 1
    return res
```

---

### **9. [Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers/)**
**Problem**: Count subarrays with **exactly** `k` distinct integers.  
**Example**:  
**Input**: `nums = [1,2,1,2,3], k = 2` → **Output**: `7`.

#### **Brute Force**
**Approach**: Check all subarrays for distinct counts.  
**Time**: O(n²), **Space**: O(n)  
```python
def subarraysWithKDistinct_brute(nums: list, k: int) -> int:
    res = 0
    for i in range(len(nums)):
        seen = set()
        for j in range(i, len(nums)):
            seen.add(nums[j])
            if len(seen) == k:
                res += 1
            elif len(seen) > k:
                break
    return res
```

#### **Optimized (Sliding Window + AtMostK Trick)**
**Approach**: Use the "at most K" minus "at most K-1" trick.  
**Time**: O(n), **Space**: O(n)  
```python
def subarraysWithKDistinct_optimized(nums: list, k: int) -> int:
    def atMostK(K):
        from collections import defaultdict
        count = defaultdict(int)
        left = res = 0
        for right in range(len(nums)):
            if count[nums[right]] == 0:
                K -= 1
            count[nums[right]] += 1
            while K < 0:
                count[nums[left]] -= 1
                if count[nums[left]] == 0:
                    K += 1
                left += 1
            res += right - left + 1
        return res
    return atMostK(k) - atMostK(k - 1)
```

---

### **Key Takeaways**:
1. **Brute Force** approaches are straightforward but inefficient (O(nk) or O(n²)).  
2. **Optimized Sliding Window** methods reduce time to O(n) using:  
   - Frequency maps for substring checks.  
   - Deques for tracking max/min.  
   - Prefix sums for range queries.  
3. **Hard Problems** often combine sliding windows with greedy algorithms or advanced data structures.  

