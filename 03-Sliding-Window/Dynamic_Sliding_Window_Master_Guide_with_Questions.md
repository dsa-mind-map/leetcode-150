# Dynamic Sliding Window Pattern: Master Reference Guide

All problems below use the exact same dynamic sliding window template `while (end < array.length)` with `start` (Tail) and `end` (Head) pointers.

## 1. Maximum Average Subarray I (LeetCode 643)
> **Question:** You are given an integer array `nums` consisting of `n` elements, and an integer `k`. Find a contiguous subarray whose length is equal to `k` that has the maximum average value and return this value. Any answer with a calculation error less than $10^{-5}$ will be accepted.

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

## 2. Longest Substring Without Repeating Characters (LeetCode 3)
> **Question:** Given a string `s`, find the length of the longest substring without repeating characters.

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

## 3. Max Consecutive Ones III (LeetCode 1004)
> **Question:** Given a binary array `nums` and an integer `k`, return the maximum number of consecutive `1`s in the array if you can flip at most `k` `0`s.

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

## 4. Fruit Into Baskets (LeetCode 904)
> **Question:** You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the type of fruit the `i`th tree produces. You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:
> * You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
> * Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit into one of your baskets.
> * Once you reach a tree with fruit that cannot fit in your baskets, you must stop.
> 
> Given the integer array `fruits`, return the maximum number of fruits you can pick.

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

## 5. Longest Repeating Character Replacement (LeetCode 424)
> **Question:** You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times. Return the length of the longest substring containing the same letter you can get after performing the above operations.

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

## 6. Minimum Window Substring (LeetCode 76)
> **Question:** Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.

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
