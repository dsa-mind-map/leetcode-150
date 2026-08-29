

## 🧠 Pattern Recognition & Approach

* **The Trigger:** The requirement to evaluate substrings of a specific, exact length (`k`) strictly maps to **Pattern 1: Fixed Sliding Window**.
* **The Strategy:** We use the universal Fixed Window template where the Head (`end`) expands and the Tail (`start`) follows. To manage the state of unique characters efficiently across a large window, we use a **Frequency Array** of size 26 combined with a global `duplicateCount` "alarm" variable.

---

## 💡 Key Learnings & Pitfalls

* **The Global Alarm Shortcut:** Instead of looping through a 26-slot frequency array on every iteration to check for duplicates, we use a single `duplicateCount` variable. If this alarm is `0`, the window is valid in $\mathcal{O}(1)$ time.
* **The "3 or More" Pitfall:** When removing a character at the Tail, it is tempting to write `if (freq > 1) { duplicateCount--; } freq--;`. This is a silent bug! If a window has three `'a'`s (`freq = 3`), removing one still leaves two `'a'`s in the window. The duplicate count shouldn't drop yet.
* **The Rule of Perfect Symmetry:** The Tail's logic must be the exact mirror of the Head's logic. 
  * Head turns the alarm ON only when crossing the threshold: `freq++; if (freq == 2) { duplicateCount++; }`
  * Tail turns the alarm OFF only when crossing back: `freq--; if (freq == 1) { duplicateCount--; }`
* **The Golden Loop Rule:** The Head (`end`) must always take its step forward at the **very bottom** of the `while` loop. Operate on the current valid window first, *then* increment into the future.

---

## 💻 Java Code

```java
// Strictly adhering to the agreed-upon pattern template
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
