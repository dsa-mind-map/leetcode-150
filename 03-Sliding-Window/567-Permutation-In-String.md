# Permutation in String (567)

**Platform:** [LeetCode](https://leetcode.com/problems/permutation-in-string/)  
**Difficulty:** Medium  
**Pattern:** Sliding Window (Fixed Step) + Single-Array Netting

---

## 📝 Problem Description

Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise. In other words, return `true` if one of `s1`'s permutations is a contiguous substring of `s2`.

---

## 🧠 Pattern Recognition & Approach

* **The Trigger:** The requirement to find a contiguous substring of `s2` that matches the exact length and character composition of `s1` is the textbook trigger for **Pattern 1: Fixed Sliding Window**.
* **The Strategy:** We use a single frequency array initialized with `s1`'s counts. As the Head (`end`) expands the window, we subtract character counts. As the Tail (`start`) slides, we restore them. When the window size equals `s1.length()`, we check if all 26 slots in our array are balanced at `0`.

---

## 💡 Key Learnings & Pitfalls

* **The Single-Array "Net Debt" Trick:** Instead of maintaining two separate frequency arrays, we can initialize one array with `s1` and treat incoming characters from `s2` as subtractions. This naturally creates a net balance system.
* **The Universal Template Alignment:** Keeping `end++` strictly at the very bottom of the loop ensures that every window of size `n1` is evaluated accurately before the Tail shifts.
* **Efficiency:** This approach avoids extra memory overhead, maintaining strict $\mathcal{O}(1)$ space complexity while evaluating permutations in linear time.

---

## 💻 Java Code

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        
        int n1 = s1.length();
        int n2 = s2.length();
        if (n1 > n2) return false;

        int[] freqCount = new int[26];

        // 1. Initialize frequency array with s1 characters
        for(int i = 0; i < n1; i++){
            freqCount[s1.charAt(i) - 'a']++;
        }

        int start = 0;
        int end = 0;

        // 2. The while loop (Head traverses)
        while(end < n2){

            // 3. Update state based on Head (subtract incoming char)
            freqCount[s2.charAt(end) - 'a']--;

            // 4. Validate Window when size matches n1
            if(end - start + 1 == n1){
                
                boolean permutation = true;
                for(int i = 0; i < 26; i++){
                    if(freqCount[i] != 0){
                        permutation = false;
                        break;
                    }
                }  

                if(permutation){
                    return true;
                } 

                // Prepare to slide: restore the outgoing character
                freqCount[s2.charAt(start) - 'a']++;
                start++;
            }

            // 5. Head always takes its step forward at the END of the loop
            end++;
        }

        return false;
    }
}
