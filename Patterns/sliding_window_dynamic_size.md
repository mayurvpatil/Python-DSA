
---

## **Easy Problems**

---

### **1. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)**
**Problem**: Find the length of the longest substring without repeating characters.  
**Example**:  
**Input**: `"abcabcbb"` → **Output**: `3` ("abc").

#### **Brute Force**
**Approach**: Check all substrings for uniqueness.  
**Time**: O(n²), **Space**: O(n)  
```python
def lengthOfLongestSubstring_brute(s: str) -> int:
    max_len = 0
    for i in range(len(s)):
        seen = set()
        for j in range(i, len(s)):
            if s[j] in seen:
                break
            seen.add(s[j])
            max_len = max(max_len, j - i + 1)
    return max_len
```

#### **Optimized (Sliding Window + Hash Map)**
**Approach**: Use a hash map to track the last index of each character.  
**Time**: O(n), **Space**: O(1) (fixed 256 ASCII characters)  
```python
def lengthOfLongestSubstring_optimized(s: str) -> int:
    last_seen = {}  # Tracks the last index of each character
    left = max_len = 0
    for right, char in enumerate(s):
        if char in last_seen and last_seen[char] >= left:
            left = last_seen[char] + 1  # Move left past the duplicate
        last_seen[char] = right
        max_len = max(max_len, right - left + 1)
    return max_len
```

---

### **2. [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)**
**Problem**: Find the minimal length of a subarray with sum ≥ target.  
**Example**:  
**Input**: `nums = [2,3,1,2,4,3], target = 7` → **Output**: `2` (subarray `[4,3]`).

#### **Brute Force**
**Approach**: Check all subarrays for sum ≥ target.  
**Time**: O(n²), **Space**: O(1)  
```python
def minSubArrayLen_brute(target: int, nums: list) -> int:
    min_len = float('inf')
    for i in range(len(nums)):
        current_sum = 0
        for j in range(i, len(nums)):
            current_sum += nums[j]
            if current_sum >= target:
                min_len = min(min_len, j - i + 1)
                break
    return min_len if min_len != float('inf') else 0
```

#### **Optimized (Sliding Window)**
**Approach**: Expand right until sum ≥ target, then shrink left.  
**Time**: O(n), **Space**: O(1)  
```python
def minSubArrayLen_optimized(target: int, nums: list) -> int:
    left = current_sum = 0
    min_len = float('inf')
    for right in range(len(nums)):
        current_sum += nums[right]
        while current_sum >= target:
            min_len = min(min_len, right - left + 1)
            current_sum -= nums[left]
            left += 1
    return min_len if min_len != float('inf') else 0
```

---

### **3. [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)**
**Problem**: Find the maximum number of consecutive 1s after flipping at most `k` 0s.  
**Example**:  
**Input**: `nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2` → **Output**: `6`.

#### **Brute Force**
**Approach**: Check all subarrays and count allowed flips.  
**Time**: O(n²), **Space**: O(1)  
```python
def longestOnes_brute(nums: list, k: int) -> int:
    max_len = 0
    for i in range(len(nums)):
        flips = 0
        for j in range(i, len(nums)):
            if nums[j] == 0:
                flips += 1
            if flips > k:
                break
            max_len = max(max_len, j - i + 1)
    return max_len
```

#### **Optimized (Sliding Window)**
**Approach**: Track the number of 0s in the window and shrink when necessary.  
**Time**: O(n), **Space**: O(1)  
```python
def longestOnes_optimized(nums: list, k: int) -> int:
    left = max_len = 0
    for right in range(len(nums)):
        if nums[right] == 0:
            k -= 1
        while k < 0:  # Shrink window if more than k flips are needed
            if nums[left] == 0:
                k += 1
            left += 1
        max_len = max(max_len, right - left + 1)
    return max_len
```

---

## **Medium Problems**

---

### **4. [Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/)**
**Problem**: Find the maximum number of fruits you can collect with 2 baskets.  
**Example**:  
**Input**: `fruits = [1,2,3,2,2]` → **Output**: `4` (subarray `[2,3,2,2]`).

#### **Brute Force**
**Approach**: Check all subarrays with at most 2 distinct fruits.  
**Time**: O(n²), **Space**: O(1)  
```python
def totalFruit_brute(fruits: list) -> int:
    max_fruits = 0
    for i in range(len(fruits)):
        basket = set()
        for j in range(i, len(fruits)):
            basket.add(fruits[j])
            if len(basket) > 2:
                break
            max_fruits = max(max_fruits, j - i + 1)
    return max_fruits
```

#### **Optimized (Sliding Window + Hash Map)**
**Approach**: Track the frequency of fruits in the window.  
**Time**: O(n), **Space**: O(1)  
```python
def totalFruit_optimized(fruits: list) -> int:
    from collections import defaultdict
    basket = defaultdict(int)
    left = max_fruits = 0
    for right in range(len(fruits)):
        basket[fruits[right]] += 1
        while len(basket) > 2:  # Shrink window if more than 2 fruits
            basket[fruits[left]] -= 1
            if basket[fruits[left]] == 0:
                del basket[fruits[left]]
            left += 1
        max_fruits = max(max_fruits, right - left + 1)
    return max_fruits
```

---

### **5. [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)**
**Problem**: Find the length of the longest substring with at most `k` replacements.  
**Example**:  
**Input**: `s = "AABABBA", k = 1` → **Output**: `4` (subarray `"AABA"`).

#### **Brute Force**
**Approach**: Check all subarrays and count replacements needed.  
**Time**: O(n²), **Space**: O(1)  
```python
def characterReplacement_brute(s: str, k: int) -> int:
    max_len = 0
    for i in range(len(s)):
        count = {}
        for j in range(i, len(s)):
            count[s[j]] = count.get(s[j], 0) + 1
            max_freq = max(count.values())
            if (j - i + 1) - max_freq <= k:
                max_len = max(max_len, j - i + 1)
    return max_len
```

#### **Optimized (Sliding Window + Frequency Map)**
**Approach**: Track the frequency of characters and shrink the window when needed.  
**Time**: O(n), **Space**: O(1)  
```python
def characterReplacement_optimized(s: str, k: int) -> int:
    from collections import defaultdict
    count = defaultdict(int)
    left = max_freq = max_len = 0
    for right in range(len(s)):
        count[s[right]] += 1
        max_freq = max(max_freq, count[s[right]])
        if (right - left + 1) - max_freq > k:  # Shrink window
            count[s[left]] -= 1
            left += 1
        max_len = max(max_len, right - left + 1)
    return max_len
```

---

### **6. [Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)**
**Problem**: Count the number of subarrays with product < `k`.  
**Example**:  
**Input**: `nums = [10,5,2,6], k = 100` → **Output**: `8`.

#### **Brute Force**
**Approach**: Check all subarrays and compute their product.  
**Time**: O(n²), **Space**: O(1)  
```python
def numSubarrayProductLessThanK_brute(nums: list, k: int) -> int:
    count = 0
    for i in range(len(nums)):
        product = 1
        for j in range(i, len(nums)):
            product *= nums[j]
            if product < k:
                count += 1
            else:
                break
    return count
```

#### **Optimized (Sliding Window)**
**Approach**: Expand right and shrink left to maintain product < `k`.  
**Time**: O(n), **Space**: O(1)  
```python
def numSubarrayProductLessThanK_optimized(nums: list, k: int) -> int:
    if k <= 1:
        return 0
    left = count = 0
    product = 1
    for right in range(len(nums)):
        product *= nums[right]
        while product >= k:  # Shrink window
            product /= nums[left]
            left += 1
        count += right - left + 1  # Add all subarrays ending at right
    return count
```

---

## **Hard Problems**

---

### **7. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)**
**Problem**: Find the smallest substring of `s` containing all characters of `t`.  
**Example**:  
**Input**: `s = "ADOBECODEBANC", t = "ABC"` → **Output**: `"BANC"`.

#### **Brute Force**
**Approach**: Check all substrings for containing all characters of `t`.  
**Time**: O(n²), **Space**: O(n)  
```python
def minWindow_brute(s: str, t: str) -> str:
    from collections import Counter
    target = Counter(t)
    min_len = float('inf')
    result = ""
    for i in range(len(s)):
        count = Counter()
        for j in range(i, len(s)):
            count[s[j]] += 1
            if all(count[c] >= target[c] for c in target):
                if j - i + 1 < min_len:
                    min_len = j - i + 1
                    result = s[i:j+1]
                break
    return result
```

#### **Optimized (Sliding Window + Hash Map)**
**Approach**: Track character frequencies and shrink the window when all characters are found.  
**Time**: O(n), **Space**: O(1)  
```python
def minWindow_optimized(s: str, t: str) -> str:
    from collections import Counter
    target = Counter(t)
    required = len(target)
    left = formed = 0
    min_len = float('inf')
    result = ""
    count = {}
    for right in range(len(s)):
        char = s[right]
        count[char] = count.get(char, 0) + 1
        if count[char] == target[char]:
            formed += 1
        while formed == required:  # Shrink window
            if right - left + 1 < min_len:
                min_len = right - left + 1
                result = s[left:right+1]
            left_char = s[left]
            count[left_char] -= 1
            if count[left_char] < target[left_char]:
                formed -= 1
            left += 1
    return result
```

---

### **8. [Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)**
**Problem**: Find the length of the longest substring with at most 2 distinct characters.  
**Example**:  
**Input**: `s = "eceba"` → **Output**: `3` (substring `"ece"`).

#### **Brute Force**
**Approach**: Check all substrings for at most 2 distinct characters.  
**Time**: O(n²), **Space**: O(1)  
```python
def lengthOfLongestSubstringTwoDistinct_brute(s: str) -> int:
    max_len = 0
    for i in range(len(s)):
        distinct = set()
        for j in range(i, len(s)):
            distinct.add(s[j])
            if len(distinct) > 2:
                break
            max_len = max(max_len, j - i + 1)
    return max_len
```

#### **Optimized (Sliding Window + Hash Map)**
**Approach**: Track the frequency of characters and shrink the window when necessary.  
**Time**: O(n), **Space**: O(1)  
```python
def lengthOfLongestSubstringTwoDistinct_optimized(s: str) -> int:
    from collections import defaultdict
    count = defaultdict(int)
    left = max_len = 0
    for right in range(len(s)):
        count[s[right]] += 1
        while len(count) > 2:  # Shrink window
            count[s[left]] -= 1
            if count[s[left]] == 0:
                del count[s[left]]
            left += 1
        max_len = max(max_len, right - left + 1)
    return max_len
```

---

### **9. [Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers/)**
**Problem**: Count the number of subarrays with exactly `k` distinct integers.  
**Example**:  
**Input**: `nums = [1,2,1,2,3], k = 2` → **Output**: `7`.

#### **Brute Force**
**Approach**: Check all subarrays for exactly `k` distinct integers.  
**Time**: O(n²), **Space**: O(n)  
```python
def subarraysWithKDistinct_brute(nums: list, k: int) -> int:
    count = 0
    for i in range(len(nums)):
        distinct = set()
        for j in range(i, len(nums)):
            distinct.add(nums[j])
            if len(distinct) == k:
                count += 1
            elif len(distinct) > k:
                break
    return count
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
1. **Brute Force** approaches are straightforward but inefficient (O(n²)).  
2. **Optimized Sliding Window** methods reduce time to O(n) using:  
   - Frequency maps for substring checks.  
   - Deques for tracking max/min.  
   - Prefix sums for range queries.  
3. **Hard Problems** often combine sliding windows with greedy algorithms or advanced data structures.  

