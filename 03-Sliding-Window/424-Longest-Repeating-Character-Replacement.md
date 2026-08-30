# 424. Longest Repeating Character Replacement

## Problem Statement
Given a string `s` and an integer `k`, you can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

**Example:**
* Input: `s = "BBAA"`, `k = 1` -> Output: `3` (We can change the one 'B' in "BAA" to get "AAA")

---

## 🧠 Key Learnings: The "Most Popular" Strategy

### 1. The "Same Shirt" Analogy
Imagine a room of 5 people wearing different colored shirts: **[Red, Red, Blue, Red, Green]**. 
You have a budget of `k = 1` free shirt to make everyone match. 

* **Who do you match?** You match the **most popular** color (Red), because that requires buying the fewest new shirts.
* **How many shirts do you need?** 
  `Total People (5) - People wearing Red (3) = 2 shirts needed.`
* **The Alarm:** Because 2 shirts needed is greater than your budget (`k = 1`), the alarm rings! The group is too big for your budget, so you must shrink the group.

### 2. The `start` Character Fallacy
A common mistake is trying to make every character match the character sitting at the `start` pointer. 

**Why it fails:** If your window is `[B, A, A]` and your `start` pointer is on 'B', comparing everything to 'B' means you think you need **2** replacements (to change both 'A's). But if you dynamically track the *most popular character* (which is 'A'), you realize you only need **1** replacement (to change the 'B'). The "target" character can change as the window moves!

### 3. The Magic Formula
Instead of guessing which character to match, we use math to track the "odd ones out":
`Replacements Needed = (Current Window Size) - (Count of the Most Frequent Character)`

If `Replacements Needed > k`, the window is invalid, and we must shrink.

---

## The Solution (Java)

This perfectly follows the **"Expand First, Fix Later"** template.

```java
class Solution {
    public int characterReplacement(String s, int k) {
        
        // Array to track character frequencies in our window (A-Z)
        int[] freq = new int[26]; 
        
        int start = 0;
        int end = 0;
        int maxFreq = 0; // Tracks the count of the most popular character
        int longestSubstring = 0;

        while (end < s.length()) {
            
            // 1. EXPAND: Add the new character unconditionally
            int rightCharIndex = s.charAt(end) - 'A';
            freq[rightCharIndex]++;
            
            // Update the maximum frequency of a single character in our window
            if (freq[rightCharIndex] > maxFreq) {
                maxFreq = freq[rightCharIndex];
            }

            // 2. CHECK ALARM: Are there too many "odd ones out"?
            // windowSize = end - start + 1
            while ((end - start + 1) - maxFreq > k) {
                
                // 3. SHRINK: The alarm is ringing. Remove the start character.
                int leftCharIndex = s.charAt(start) - 'A';
                freq[leftCharIndex]--;
                start++; 
            }

            // 4. UPDATE: The window is now valid
            int currentWindowSize = end - start + 1;
            if (currentWindowSize > longestSubstring) {
                longestSubstring = currentWindowSize;
            }

            // 5. MOVE HEAD
            end++;
        }

        return longestSubstring;
    }
}
```

---

Complexity Analysis
Time Complexity: O(n) - Both start and end pointers only move forward, visiting each character at most twice.

Space Complexity: O(1) - The freq array is strictly fixed at size 26 for uppercase English letters, regardless of how huge the string s is.
