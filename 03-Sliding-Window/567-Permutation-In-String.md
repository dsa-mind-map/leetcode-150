# Permutation in String (567)

**Platform:** [LeetCode](https://leetcode.com/problems/permutation-in-string/)  
**Difficulty:** Medium  
**Pattern:** Sliding Window (Fixed Step) + Frequency State Tracking

---

## 📝 Problem Description

Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise. In other words, return `true` if one of `s1`'s permutations is a contiguous substring of `s2`.

---

## 🧠 Pattern Recognition & Approach

* **The Trigger:** Finding a contiguous substring of a fixed length (`s1.length()`) that matches a target character composition maps directly to **Pattern 1: Fixed Sliding Window**.
* **The Strategy:** We use our universal Fixed Window template where the Head (`end`) expands the frame and the Tail (`start`) follows. We compare character frequency states using arrays to validate permutations in linear time.

---

## 💡 Key Learnings & Pitfalls

* **The Ambiguity of Zero in State Arrays:** A frequency count of `0` in a dynamic array is inherently ambiguous. It can mean two entirely different things:
  1. The character was part of `s1` (starting at $>0$) and was perfectly balanced down to `0` by the window.
  2. The character was *never* part of `s1` to begin with, meaning its frequency has sat at `0` since initialization.
* **Why the Second Array Solves It:** To eliminate this ambiguity and prevent accidental state corruption during Head decrements and Tail increments, we use a static reference array (`existsInS1`). It serves as an unchangeable "permission slip" that permanently tracks what `s1` originally contained, ensuring we only modify frequencies for characters that actually matter.
* **The Rule of Symmetrical State Tracking:** Whatever filtering or conditional guard you apply to the Head when it enters the window, you must apply the exact same guard to the Tail when it leaves to maintain perfect state synchronization.
* **The Golden Loop Rule:** The Head (`end`) pointer must always take its step forward at the **very bottom** of the `while` loop to guarantee correct window sizing.

---

## 💻 Java Code: Solution 1 (Two-Array Guarded Approach)

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {

        int n1 = s1.length();
        int n2 = s2.length();
        if (n1 > n2) return false;

        int[] freq = new int[26];
        int[] existsInS1 = new int[26];

        // Populate initial frequencies and static reference permission slips
        for(int i = 0; i < n1; i++){
            int index = s1.charAt(i) - 'a';
            freq[index]++;
            existsInS1[index]++;
        }

        int start = 0;
        int end = 0;

        while(end < n2){
            
            // Head checks permission slip before decrementing
            int indexEnd = s2.charAt(end) - 'a';
            if(existsInS1[indexEnd] > 0){
                freq[indexEnd]--;
            }

            // Window size validation
            if(end - start + 1 == n1){
                boolean permutation = true;

                for(int i = 0; i < 26; i++){
                    if(freq[i] != 0){
                        permutation = false;
                        break;
                    }
                }
                
                if(permutation) return true;

                // Tail checks permission slip before incrementing
                int indexStart = s2.charAt(start) - 'a';
                if(existsInS1[indexStart] > 0){
                    freq[indexStart]++;
                }

                start++;
            }

            // Head always steps forward at the END of the loop
            end++;
        }

        return false;
    }
}
