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
    public int countGoodSubstrings(String s) {

        int n = s.length();
        int start = 0;
        int goodSubStrings = 0;

        // Stop at n - 2 to ensure start+1 and start+2 are always in bounds
        while (start < n - 2) {

            // Grab the three characters in our fixed window
            char first = s.charAt(start);
            char second = s.charAt(start + 1);
            char third  = s.charAt(start + 2);

            // Check if they are completely unique
            if (first != second && second != third && third != first) {
                goodSubStrings++;
            }

            // Slide the window forward by 1
            start++;
        }

        return goodSubStrings;
    }
}
