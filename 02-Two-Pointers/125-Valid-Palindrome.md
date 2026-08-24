# 125. Valid Palindrome (LeetCode)

**Difficulty:** Easy
**Topic:** Two Pointers
**Constraint:** Must run in $\mathcal{O}(n)$ time and strictly $\mathcal{O}(1)$ extra space.

---

## 📝 Problem Description

Given a string `s`, return `true` if it is a palindrome, otherwise return `false`.

A palindrome is a string that reads the same forward and backward. It is also case-insensitive and ignores all non-alphanumeric characters.

**Note:** Alphanumeric characters consist of letters (`A-Z`, `a-z`) and numbers (`0-9`).

### Examples

**Example 1:**
* **Input:** `s = "Was it a car or a cat I saw?"`
* **Output:** `true`
* **Explanation:** After considering only alphanumerical characters we have `"wasitacaroracatisaw"`, which is a palindrome.

**Example 2:**
* **Input:** `s = "tab a cat"`
* **Output:** `false`
* **Explanation:** `"tabacat"` is not a palindrome.

### Constraints
* `1 <= s.length <= 1000`
* `s` is made up of only printable ASCII characters.

---

## 🧠 The Mental Model: On-the-Fly Skipping
The brute-force way is to create a new string with all punctuation removed, then check if it's a palindrome. But creating a new string takes $\mathcal{O}(n)$ memory.

To achieve $\mathcal{O}(1)$ space, we use **Opposite-End Pointers** on the *original* string. 
* We place a `start` pointer at index `0` and an `end` pointer at index `n - 1`.
* If a pointer lands on a non-alphanumeric character (like a space or punctuation), we use a guarded inner `while` loop like a broom to skip over it on the fly.
* Once both pointers are standing on valid alphanumeric characters, we compare them (after converting to lowercase).
* **Crucial Safety Rule:** Because inner loops increment/decrement pointers independently, we must include `start < end` inside the inner loops. Without it, continuous spaces or garbage characters could cause the pointers to cross and throw an out-of-bounds error.

---

## 💻 Optimal Java Solution ($\mathcal{O}(1)$ Space)

```java
class Solution {
    public boolean isPalindrome(String s) {
        // 1. Convert string to lowercase for case-insensitive comparison
        s = s.toLowerCase();
        int n = s.length();

        int start = 0;
        int end = n - 1;

        while (start < end) {
            
            // 2. Skip non-alphanumeric characters from the left (with boundary guard)
            while (start < end && !Character.isLetterOrDigit(s.charAt(start))) {
                start++;
            }
            
            // 3. Skip non-alphanumeric characters from the right (with boundary guard)
            while (start < end && !Character.isLetterOrDigit(s.charAt(end))) {
                end--;
            }

            // 4. Compare valid characters
            if (s.charAt(start) != s.charAt(end)) {
                return false; // Mismatch found
            }
            
            // 5. Move both pointers inward
            start++;
            end--;
        }

        return true; // Successfully matched all characters
    }
}
