# Dynamic Sliding Window Pattern: Master Reference Guide

All problems below use the exact same dynamic sliding window template `while (end < array.length)` with `start` (Tail) and `end` (Head) pointers.

## 1. Maximum Average Subarray I (Fixed-Size as Dynamic)
You are given an integer array nums consisting of n elements, and an integer k.

Find a contiguous subarray whose length is equal to k that has the maximum average value and return this value. Any answer with a calculation error less than 10-5 will be accepted.

 

Example 1:

Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
Example 2:

Input: nums = [5], k = 1
Output: 5.00000
 

Constraints:

n == nums.length
1 <= k <= n <= 105
-104 <= nums[i] <= 104

```java
public double findMaxAverage(int[] nums, int k) {
    int start = 0, end = 0;
    double maxAvg = -Double.MAX_VALUE;
    double currentSum = 0;
    
    while (end < nums.length) {
        // 1. Head expands
        currentSum += nums[end];
        
        // 2. Tail squeezes (Condition: window size exceeds k)
        while (end - start + 1 > k) {
            currentSum -= nums[start];
            start++;
        }
        
        // 3. Update result (Only when window is exactly size k)
        if (end - start + 1 == k) {
            maxAvg = Math.max(maxAvg, currentSum / k);
        }
        
        end++;
    }
    return maxAvg;
}
```

## 2. Longest Substring Without Repeating Characters
```java
public int lengthOfLongestSubstring(String s) {
    int start = 0, end = 0, maxLength = 0;
    int[] count = new int[128]; 
    
    while (end < s.length()) {
        // 1. Head expands
        char endChar = s.charAt(end);
        count[endChar]++;
        
        // 2. Tail squeezes (Condition: a character appears more than once)
        while (count[endChar] > 1) {
            char startChar = s.charAt(start);
            count[startChar]--;
            start++;
        }
        
        // 3. Update result
        maxLength = Math.max(maxLength, end - start + 1);
        
        end++;
    }
    return maxLength;
}
```

## 3. Max Consecutive Ones III
```java
public int longestOnes(int[] nums, int k) {
    int start = 0, end = 0, maxLength = 0;
    int zeroCount = 0;
    
    while (end < nums.length) {
        // 1. Head expands
        if (nums[end] == 0) zeroCount++;
        
        // 2. Tail squeezes (Condition: more than k zeros)
        while (zeroCount > k) {
            if (nums[start] == 0) zeroCount--;
            start++;
        }
        
        // 3. Update result
        maxLength = Math.max(maxLength, end - start + 1);
        
        end++;
    }
    return maxLength;
}
```

## 4. Fruit Into Baskets
```java
public int totalFruit(int[] fruits) {
    int start = 0, end = 0, maxFruits = 0;
    Map<Integer, Integer> fruitCount = new HashMap<>();
    
    while (end < fruits.length) {
        // 1. Head expands
        fruitCount.put(fruits[end], fruitCount.getOrDefault(fruits[end], 0) + 1);
        
        // 2. Tail squeezes (Condition: more than 2 distinct fruit types)
        while (fruitCount.size() > 2) {
            int startFruit = fruits[start];
            fruitCount.put(startFruit, fruitCount.get(startFruit) - 1);
            if (fruitCount.get(startFruit) == 0) {
                fruitCount.remove(startFruit);
            }
            start++;
        }
        
        // 3. Update result
        maxFruits = Math.max(maxFruits, end - start + 1);
        
        end++;
    }
    return maxFruits;
}
```

## 5. Longest Repeating Character Replacement
```java
public int characterReplacement(String s, int k) {
    int start = 0, end = 0, maxLength = 0;
    int maxFreq = 0; 
    int[] count = new int[128]; 
    
    while (end < s.length()) {
        // 1. Head expands
        char endChar = s.charAt(end);
        count[endChar]++;
        
        maxFreq = Math.max(maxFreq, count[endChar]);
        
        // 2. Tail squeezes (Condition: chars to replace > k)
        while ((end - start + 1) - maxFreq > k) {
            char startChar = s.charAt(start);
            count[startChar]--;
            start++;
        }
        
        // 3. Update result
        maxLength = Math.max(maxLength, end - start + 1);
        
        end++;
    }
    return maxLength;
}
```

## 6. Minimum Window Substring
```java
public String minWindow(String s, String t) {
    if (s.length() < t.length()) return "";
    
    int[] required = new int[128];
    for (char c : t.toCharArray()) required[c]++;
    
    int start = 0, end = 0, minLength = Integer.MAX_VALUE, minStart = 0;
    int charsNeeded = t.length(); 
    
    while (end < s.length()) {
        // 1. Head expands
        char endChar = s.charAt(end);
        if (required[endChar] > 0) charsNeeded--;
        required[endChar]--;
        
        // 2. Tail squeezes (Condition: window is valid / all characters found)
        while (charsNeeded == 0) {
            // 3. Update result INSIDE the loop for Minimum problems
            if (end - start + 1 < minLength) {
                minLength = end - start + 1;
                minStart = start;
            }
            
            char startChar = s.charAt(start);
            required[startChar]++;
            if (required[startChar] > 0) charsNeeded++;
            start++;
        }
        
        end++;
    }
    
    return minLength == Integer.MAX_VALUE ? "" : s.substring(minStart, minStart + minLength);
}
```

---

## Comparing the "Tail Squeeze" (Shrink Phase)

The power of this pattern is that the `while` loop condition directly maps to the problem's primary constraint.

| Problem | Head Expansion | Tail Squeeze Condition | What the Tail Does |
| :--- | :--- | :--- | :--- |
| **Max Average** | Adds to sum. | `(end - start + 1) > k` | Pulls the tail forward to strictly maintain a window size of exactly `k`. |
| **No Repeating Chars** | Adds to char count. | `count[endChar] > 1` | Hunts down the specific duplicate character and drops it from the window. |
| **Max Ones III** | Increments zero count. | `zeroCount > k` | Drops elements until enough zeros are removed to get back under the flip limit `k`. |
| **Fruit Baskets** | Adds to HashMap count. | `map.size() > 2` | Shrinks the window until an entire category of fruit is completely removed from the map. |
| **Char Replacement** | Adds to char count, updates `maxFreq`. | `(size - maxFreq) > k` | Shrinks the window until the number of non-matching characters fits back within the replacement budget `k`. |
| **Min Window**| Marks off required characters. | `charsNeeded == 0` | Harvests the answer, then aggressively chops off characters to see if a smaller valid window still exists. |
