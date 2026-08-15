# 242 - Valid Anagram
**Date:** August 15, 2026
**Topic:** Arrays & Hashing
**Pattern:** Frequency Counter (Fixed-Size Array vs. Hash Map)

## 📝 Problem Statement
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.
* An Anagram is a word or phrase formed by rearranging the letters of a different word, typically using all the original letters exactly once.
* Constraints: Strings consist of lowercase English letters.

---

## 🧠 Architectural Decision Flow: Array vs. Hash Map
When approaching frequency-counting problems, how do you choose the right data structure?

1. **Check the Universe (Is it dense and fixed?):**
   * If the input is restricted to a known, continuous range (e.g., lowercase English letters `a-z`, digits `0-9`, or standard ASCII), **always use a Fixed-Size Array (`int[26]`)**.
   * *Why?* Hash Maps require object allocation (boxing), hashing overhead, and bucket management. A primitive array is a contiguous block of memory that executes in raw CPU instructions with zero allocation overhead.
2. **Check for Unbounded/Sparse Inputs:**
   * If the input contains mixed Unicode symbols, arbitrary integers, or non-contiguous ranges, a fixed array will waste massive memory or break because of non-sequential ASCII values. **Fallback to `HashMap<Character, Integer>`** so you only allocate memory for the elements that actually appear.

---

## 💡 The "Aha!" Moments & Debugging Log
* **The ASCII Offset Trick:** Characters in Java have underlying decimal ASCII values (`'a'` = 97, `'z'` = 122). To map any lowercase letter to an index between `0` and `25`, use the formula: `c - 'a'`. 
* **The Single-Pass Frequency Engine:** Instead of building two separate frequency maps and comparing them, increment counts for string `s` (`+1`), then decrement counts for string `t` (`-1`) in a single loop. If all slots end at `0`, they are anagrams.
* **The Ultimate Shortcut:** If `s.length() != t.length()`, they can never be anagrams. Always check this first in $\mathcal{O}(1)$ time before doing any heavy work.

---

## 🧠 The Optimal Mental Model (3-Step Engine)
1. **Length Guard:** If `s.length() != t.length()`, return `false`.
2. **Frequency Tally (Array):** 
   * Convert strings to character arrays.
   * Traverse both in a single loop: increment index for `s`, decrement index for `t`.
3. **Validation Scan:** Walk through the `int[26]` array. If any index contains a non-zero value, return `false`. Otherwise, return `true`.

---

## 💻 Final Java Solution

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        // Guard clause: If lengths differ, they cannot be anagrams
        if (s.length() != t.length()) {
            return false;
        }

        int length = s.length();

        char charS[] = s.toCharArray();
        char charT[] = t.toCharArray();

        // Fixed-size frequency array for lowercase English letters (a-z)
        int[] frequency = new int[26];

        // Tally frequencies from string s and subtract from string t in one pass
        for (int i = 0; i < length; i++) {
            int sChar = charS[i] - 'a';
            int tChar = charT[i] - 'a';

            frequency[sChar]++;
            frequency[tChar]--;
        }

        // Validate that all frequencies balanced out to zero
        for (int i = 0; i < frequency.length; i++) {
            if (frequency[i] != 0) {
                return false;
            }
        }

        return true;
    }
}
