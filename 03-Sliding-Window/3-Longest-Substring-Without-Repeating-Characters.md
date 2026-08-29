# 3. Longest Substring Without Repeating Characters

## Problem Statement
Given a string `s`, find the length of the longest substring without duplicate characters.

**Examples:**
* Input: `s = "abcabcbb"` -> Output: `3` (The answer is "abc")
* Input: `s = "bbbbb"` -> Output: `1` (The answer is "b")

---

## 🧠 Key Learnings: The Variable Sliding Window Pattern

### 1. "Expand First, Fix Later"
In a sliding window, the state (like a frequency array) **must always represent exactly what is physically inside the window right now**, even if the window is currently invalid (contains duplicates). 

You always **add without checking**. Moving the `end` pointer means that character has physically entered your window; you cannot pretend it isn’t there.

**Why checking first fails:**
If you find a duplicate and refuse to add it to the array, you break the connection between your window boundaries (`start` to `end`) and your frequency map. Your pointers might contain two 'a's, but your array says there is only one. Because your array is lying about reality, you are forced to write complex, nested loops to manually scan the window.

### 2. The Shrinking Logic (Why we use a `while` loop)
When a duplicate is added, the alarm rings (`freq > 1`). We use a `while` loop to iterate because **we do not know where that duplicate character is located** between `start` and `end-1`. 

As long as the newly added character's frequency is not reduced back to `1`, the window is invalid. So, we keep moving `start` forward—removing characters from our frequency map along the way—until we finally kick out the old duplicate.

---

## The Solution (Java)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // Use an array of 128 to cover all standard ASCII characters
        int[] freq = new int[128]; 
        
        int start = 0;
        int end = 0;
        int maxSubstring = 0;

        while (end < s.length()) {
            // 1. EXPAND: Add the current character to our window
            char rightChar = s.charAt(end);
            freq[rightChar]++;

            // 2. CHECK & SHRINK: Did we just add a duplicate?
            // Keep shrinking from the left until the duplicate is removed
            while (freq[rightChar] > 1) {
                char leftChar = s.charAt(start);
                freq[leftChar]--; // Remove from window state
                start++;          // Move tail forward
            }

            // 3. UPDATE: The window is now valid (no duplicates)
            int currentWindowSize = end - start + 1;
            if (currentWindowSize > maxSubstring) {
                maxSubstring = currentWindowSize;
            }

            // 4. MOVE FORWARD
            end++;
        }
        
        return maxSubstring;
    }
}
```
---
Complexity Analysis
Time Complexity: O(n) - Each character is visited at most twice (once by end, once by start).

Space Complexity: O(1) - The frequency array size is fixed at 128 regardless of the input string length.
