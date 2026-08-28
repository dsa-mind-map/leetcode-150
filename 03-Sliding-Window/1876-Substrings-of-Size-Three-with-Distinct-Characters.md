# Substrings of Size Three with Distinct Characters (1876)

**Platform:** [LeetCode](https://leetcode.com/problems/substrings-of-size-three-with-distinct-characters/)  
**Difficulty:** Easy  
**Pattern:** Sliding Window (Fixed Step / Optimized)

---

## 📝 Problem Description

A string is **good** if there are no repeated characters.
Given a string `s`, return the number of **good substrings** of length 3 in `s`.

---

## 🧠 Pattern Recognition & Approach

* **The Trigger:** The problem asks for substrings of exactly length 3. This is **Pattern 1: Fixed Sliding Window**.
* **The Optimization:** Because the window size $k$ is so small and permanently locked to exactly 3, we don't need a complex state tracker (like a `HashSet` or `HashMap`) or even a traditional `end` pointer. 
* We can simply slide a single `start` pointer across the array (stopping at `n - 2` to avoid out-of-bounds errors) and manually compare the three characters at `start`, `start+1`, and `start+2`.

---

## 💻 Java Code

```java
class Solution {
    public int countGoodSubstrings(String s, int k) { // k is now a parameter, e.g., 10
        
        int n = s.length();
        int start = 0;
        int end = 0;
        int goodSubStrings = 0;
        
        // State Trackers
        int[] freq = new int[26]; // Tracks counts of 'a' through 'z'
        int duplicateCount = 0;   // How many characters are violating the uniqueness rule?

        while (end < n) {
            
            // 1. STATE UPDATE: Head eats a new character
            char endChar = s.charAt(end);
            freq[endChar - 'a']++; 
            if (freq[endChar - 'a'] == 2) {
                // We just found a duplicate!
                duplicateCount++;
            }

            // 2. CHECK WINDOW SIZE
            if (end - start + 1 == k) {
                
                // 3. EVALUATE: Are there zero duplicates in our size 10 window?
                if (duplicateCount == 0) {
                    goodSubStrings++;
                }

                // 4. PREPARE TO SLIDE: Tail drops the left element
                char startChar = s.charAt(start);
                freq[startChar - 'a']--;
                if (freq[startChar - 'a'] == 1) {
                    // We just removed a duplicate, so our window is healing!
                    duplicateCount--;
                }
                
                start++;
            }

            // 5. Head always takes its step forward at the END
            end++;
        }

        return goodSubStrings;
    }
}
