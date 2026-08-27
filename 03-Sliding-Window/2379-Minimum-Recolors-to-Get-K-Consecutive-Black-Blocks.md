# Minimum Recolors to Get K Consecutive Black Blocks (2379)

**Platform:** [LeetCode](https://leetcode.com/problems/minimum-recolors-to-get-k-consecutive-black-blocks/)  
**Difficulty:** Easy  
**Pattern:** Sliding Window (Fixed Step)

---

## 📝 Problem Description

You are given a 0-indexed string `blocks` of length `n`, where `blocks[i]` is either `'W'` (white) or `'B'` (black), and an integer `k`.

In one operation, you can recolor a white block to a black block. Return the minimum number of operations needed to create a block of exactly `k` consecutive black blocks.

---

## 🧠 Pattern Recognition & Hints

* **The Trigger:** The problem asks for exactly `k` consecutive blocks, which instantly tells us this is **Pattern 1: Fixed Sliding Window**.
* **The State:** Instead of a numerical sum, our window tracks a "state"—specifically, the count of `'W'` characters inside the current window.
* **The Proactive Slide:** Once the window reaches size `k`, we record the minimum operations needed. Then, *before* moving to the next iteration, we proactively drop the Tail element. If the Tail element was a `'W'`, we subtract it from our operation count so the next window has accurate data.

---

## 💻 Java Code

```java
class Solution {
    public int minimumRecolors(String blocks, int k) {
        
        int n = blocks.length();
        int start = 0;
        int end = 0;
        
        // Track the minimum operations seen so far
        int minOccurence = Integer.MAX_VALUE;
        // Track 'W' count in the current window
        int numOperations = 0; 

        // blocks = "WBBWWBBWBW", k = 7
        while(end < n) {

            // 1. Head eats the new element (Use 'end', not 'start'!)
            if(blocks.charAt(end) == 'W') {
                numOperations++;
            }

            // 2. Is our window exactly size k?
            if(end - start + 1 == k) { 
                
                // Update the minimum operations needed
                if(numOperations < minOccurence) {
                    minOccurence = numOperations;
                }

                // 3. Prepare to slide: Tail drops the left element
                if(blocks.charAt(start) == 'W') {
                    numOperations--; // reduce only if it was a white block
                }
                
                start++; // move tail
            }

            // 4. Head always takes a step forward
            end++; 
        }
        
        return minOccurence;
    }
}
```

---

⏱️ Complexity AnalysisTime Complexity: $O(n)$The while loop iterates through the string exactly once.
The charAt checks and variable updates all happen in constant time.
Space Complexity: $O(1)$We are only using a few integer variables to track the state of the window, requiring zero extra data structures.
