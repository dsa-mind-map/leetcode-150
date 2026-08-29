📝 Problem Description

A string is **good** if there are no repeated characters.
Given a string `s` and an integer `k`, return the number of **good substrings** of length `k` in `s`. 

*(Note: The original LeetCode problem hardcodes the size to 3, but this implementation scales to handle any window size `k` efficiently).*

---

## 🚀 Solution 1: Optimized Sliding Window (Frequency Array + Global Alarm)

### Approach & Pattern Recognition
* **The Trigger:** The requirement to evaluate substrings of a specific, exact length (`k`) strictly maps to **Pattern 1: Fixed Sliding Window**.
* **The Strategy:** We use the universal Fixed Window template where the Head (`end`) expands and the Tail (`start`) follows. To manage the state of unique characters efficiently across a large window, we use a **Frequency Array** of size 26 combined with a global `duplicateCount` "alarm" variable.

### Key Learnings & Pitfalls
* **The Global Alarm Shortcut:** Instead of looping through a 26-slot frequency array on every iteration to check for duplicates, we use a single `duplicateCount` variable. If this alarm is `0`, the window is valid in O(1) time.
* **The "3 or More" Pitfall:** When removing a character at the Tail, it is tempting to write `if (freq > 1) { duplicateCount--; } freq--;`. This is a silent bug! If a window has three `'a'`s (`freq = 3`), removing one still leaves two `'a'`s in the window. The duplicate count shouldn't drop yet.
* **The Rule of Perfect Symmetry:** The Tail's logic must be the exact mirror of the Head's logic. 
  * Head turns the alarm ON only when crossing the threshold: `freq++; if (freq == 2) { duplicateCount++; }`
  * Tail turns the alarm OFF only when crossing back: `freq--; if (freq == 1) { duplicateCount--; }`
* **The Golden Loop Rule:** The Head (`end`) must always take its step forward at the **very bottom** of the `while` loop. Operate on the current valid window first, *then* increment into the future.

### Java Code
```java
class Solution {
    public int countGoodSubstrings(String s, int k) { 
        
        // 1. Initialize pointers and state
        int n = s.length();
        int start = 0;
        int end = 0;
        int goodSubStrings = 0;
        
        int[] freq = new int[26]; // Tracks counts of 'a' through 'z'
        int duplicateCount = 0;   // How many characters are violating uniqueness?

        // 2. The while loop (Head traverses)
        while (end < n) {
            
            // 3. Update state based on Head
            char endChar = s.charAt(end);
            freq[endChar - 'a']++; 
            
            // Only increase alarm when moving from 1 to 2
            if (freq[endChar - 'a'] == 2) {
                duplicateCount++;
            }

            // 4. Validate Window (if/while for Tail movement)
            if (end - start + 1 == k) {
                
                // Evaluate current valid window
                if (duplicateCount == 0) {
                    goodSubStrings++;
                }

                // Prepare to slide: Tail drops the left element
                char startChar = s.charAt(start);
                freq[startChar - 'a']--;
                
                // Only decrease alarm when moving from 2 to 1 (fully healed)
                if (freq[startChar - 'a'] == 1) {
                    duplicateCount--;
                }
                
                start++; // Slide the Tail
            }

            // 5. Head takes its step forward at the END of the loop
            end++;
        }

        return goodSubStrings;
    }
}
```

### Complexity Analysis (Solution 1)
* **Time Complexity:** **O(N)**, where $N$ is the length of the string `s`. Each character is processed at most twice (once when entered by the Head, once when removed by the Tail).
* **Space Complexity:** **O(1)**. The frequency array has a fixed size of 26 regardless of the input string length.

---

## 🔍 Solution 2: Brute Force Approach (Substring Extraction)

### Approach
If we want a simpler, more intuitive approach without managing sliding window pointers and frequency alarms, we can iterate through all possible starting positions of length `k` and explicitly check each substring for uniqueness using a HashSet or frequency count.

### Java Code
```java
class Solution {
    public int countGoodSubstrings(String s, int k) {
        int n = s.length();
        int goodSubStrings = 0;

        // Check every possible starting index for a substring of length k
        for (int i = 0; i <= n - k; i++) {
            if (hasAllUnique(s, i, i + k - 1)) {
                goodSubStrings++;
            }
        }

        return goodSubStrings;
    }

    private boolean hasAllUnique(String s, int start, int end) {
        boolean[] seen = new boolean[26];
        for (int i = start; i <= end; i++) {
            char c = s.charAt(i);
            if (seen[c - 'a']) {
                return false; // Duplicate found
            }
            seen[c - 'a'] = true;
        }
        return true;
    }
}
```

### Complexity Analysis (Solution 2)
* **Time Complexity:** **O(N * K)**, where $N$ is the length of the string and $K$ is the window size. For each of the $N - K + 1$ starting positions, we iterate $K$ times to check for uniqueness.
* **Space Complexity:** **O(1)** auxiliary space (excluding the call stack / minor variables), as the boolean tracking array is fixed at size 26.
