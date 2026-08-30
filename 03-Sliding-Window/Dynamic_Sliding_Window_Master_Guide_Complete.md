# Dynamic Sliding Window Pattern: Master Reference Guide

All problems below use the exact same dynamic sliding window template `while (end < array.length)` with `start` (Tail) and `end` (Head) pointers.

## 1. Maximum Average Subarray I (LeetCode 643)
> **Question:** You are given an integer array `nums` consisting of `n` elements, and an integer `k`.
> Find a contiguous subarray whose length is equal to `k` that has the maximum average value and return this value. Any answer with a calculation error less than 10^-5 will be accepted.
> 
> **Example 1:**
> * **Input:** `nums = [1,12,-5,-6,50,3]`, `k = 4`
> * **Output:** `12.75000`
> * **Explanation:** Maximum average is `(12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75`
> 
> **Example 2:**
> * **Input:** `nums = [5]`, `k = 1`
> * **Output:** `5.00000`

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
> **Question:** Given a string `s`, find the length of the longest substring without duplicate characters.
> 
> **Example 1:**
> * **Input:** `s = "abcabcbb"`
> * **Output:** `3`
> * **Explanation:** The answer is "abc", with the length of 3. Note that "bca" and "cab" are also correct answers.
> 
> **Example 2:**
> * **Input:** `s = "bbbbb"`
> * **Output:** `1`
> * **Explanation:** The answer is "b", with the length of 1.
> 
> **Example 3:**
> * **Input:** `s = "pwwkew"`
> * **Output:** `3`
> * **Explanation:** The answer is "wke", with the length of 3. Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
> 
> **Constraints:**
> * `0 <= s.length <= 10^5`
> * `s` consists of English letters, digits, symbols and spaces.

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
```
class Solution {
    public int longestOnes(int[] nums, int k) {
        
        int n = nums.length;
        int start = 0, end = 0, maxConsecutiveOne = 0;

        int[] freq = new int[2];

        while( end < n){

            freq[nums[end]]++;

            // freq of end > allowed freq ( k)
            while(freq[0] > k){

                freq[nums[start]]--;
                start++;

            }

            // max of substring
            maxConsecutiveOne = Integer.max(maxConsecutiveOne, end - start + 1); 

            System.out.println(maxConsecutiveOne);

            end++;

        }

        return maxConsecutiveOne;
    }
}
```


## 3. Max Consecutive Ones III (LeetCode 1004)
> **Question:** Given a binary array `nums` and an integer `k`, return the maximum number of consecutive `1`'s in the array if you can flip at most `k` `0`'s.
> 
> **Example 1:**
> * **Input:** `nums = [1,1,1,0,0,0,1,1,1,1,0]`, `k = 2`
> * **Output:** `6`
> * **Explanation:** `[1,1,1,0,0,1,1,1,1,1,1]` - Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
> 
> **Example 2:**
> * **Input:** `nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1]`, `k = 3`
> * **Output:** `10`
> * **Explanation:** `[0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]`
> 
> **Constraints:**
> * `1 <= nums.length <= 10^5`
> * `nums[i]` is either `0` or `1`.
> * `0 <= k <= nums.length`

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
> **Question:** You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the type of fruit the `i`th tree produces.
> You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:
> * You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
> * Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
> * Once you reach a tree with fruit that cannot fit in your baskets, you must stop.
> 
> Given the integer array `fruits`, return the maximum number of fruits you can pick.
> 
> **Example 1:**
> * **Input:** `fruits = [1,2,1]`
> * **Output:** `3`
> * **Explanation:** We can pick from all 3 trees.
> 
> **Example 2:**
> * **Input:** `fruits = [0,1,2,2]`
> * **Output:** `3`
> * **Explanation:** We can pick from trees `[1,2,2]`. If we had started at the first tree, we would only pick from trees `[0,1]`.
> 
> **Example 3:**
> * **Input:** `fruits = [1,2,3,2,2]`
> * **Output:** `4`
> * **Explanation:** We can pick from trees `[2,3,2,2]`. If we had started at the first tree, we would only pick from trees `[1,2]`.
> 
> **Constraints:**
> * `1 <= fruits.length <= 10^5`
> * `0 <= fruits[i] < fruits.length`

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
> **Question:** You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.
> Return the length of the longest substring containing the same letter you can get after performing the above operations.
> 
> **Example 1:**
> * **Input:** `s = "ABAB"`, `k = 2`
> * **Output:** `4`
> * **Explanation:** Replace the two 'A's with two 'B's or vice versa.
> 
> **Example 2:**
> * **Input:** `s = "AABABBA"`, `k = 1`
> * **Output:** `4`
> * **Explanation:** Replace the one 'A' in the middle with 'B' and form "AABBBBA". The substring "BBBB" has the longest repeating letters, which is 4. There may exists other ways to achieve this answer too.
> 
> **Constraints:**
> * `1 <= s.length <= 10^5`
> * `s` consists of only uppercase English letters.
> * `0 <= k <= s.length`

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
> The testcases will be generated such that the answer is unique.
> 
> **Example 1:**
> * **Input:** `s = "ADOBECODEBANC"`, `t = "ABC"`
> * **Output:** `"BANC"`
> * **Explanation:** The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
> 
> **Example 2:**
> * **Input:** `s = "a"`, `t = "a"`
> * **Output:** `"a"`
> * **Explanation:** The entire string s is the minimum window.
> 
> **Example 3:**
> * **Input:** `s = "a"`, `t = "aa"`
> * **Output:** `""`
> * **Explanation:** Both 'a's from t must be included in the window. Since the largest window of s only has one 'a', return empty string.
> 
> **Constraints:**
> * `m == s.length`
> * `n == t.length`
> * `1 <= m, n <= 10^5`
> * `s` and `t` consist of uppercase and lowercase English letters.
> 
> **Follow up:** Could you find an algorithm that runs in `O(m + n)` time?

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
