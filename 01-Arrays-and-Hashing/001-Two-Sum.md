# 001 - Two Sum
**Date:** August 15, 2026
**Topic:** Arrays & Hashing
**Pattern:** Hash Map (On-the-Go / One-Pass)

## 📝 Problem Statement
Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that they add up to `target`.
* You may assume that each input would have exactly one solution.
* You may not use the same element twice.
* Return the answer with the smaller index first.

## 💡 The "Aha!" Moments & Corrections
* **Sorting Destroys Indices:** Sorting + Two Pointers takes $O(n \log n)$ time, but it destroys the original indices. If a problem asks to return indices, sorting is instantly disqualified.
* **Map Contents:** The Hash Map stores the *actual array element* as the Key, and its *original index* as the Value. You calculate the remaining value on the fly.
* **The "Self-Match" Trap:** Pre-populating the map can cause you to accidentally match an element with itself (e.g., target 6, array `[3, 2, 4]` returning `[0, 0]`).
* **The Golden Inventory Rule:**
  * *On-the-Go (One-Pass):* Use for finding Pairs or Complements (looking backward into history). Guarantees no self-matching.
  * *Pre-Populated (Two-Pass):* Use ONLY when a global inventory/frequency count is required before solving (e.g., Valid Anagram).

## 🧠 The Optimal Mental Model (4-Step Engine)
1. **Calculate:** `complement = target - current_element`
2. **Check:** Does `complement` exist as a key in my map?
3. **If Found:** Return `[map.get(complement), current_index]`. (This ensures the smaller index is returned first).
4. **If Not Found:** Store the `current_element` and `current_index` into the map, and move forward.

## 💻 Final Java Solution

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // Base case / Guard clause
        if (nums == null || nums.length < 2) {
            return new int[]{};
        }

        // Map to store: <Actual Element, Index Original>
        Map<Integer, Integer> visited = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];

            // If we found the matching partner in our history
            if (visited.containsKey(complement)) {
                return new int[]{visited.get(complement), i};
            }

            // Otherwise, store current element and index into history
            visited.put(nums[i], i);
        }

        // Fallback if no solution exists (though constraints guarantee one)
        return new int[]{-1, -1};
    }
}
