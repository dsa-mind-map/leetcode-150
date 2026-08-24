# 217 - Contains Duplicate
**Date:** August 15, 2026  
**Topic:** Arrays & Hashing  
**Pattern:** Hash Set (On-the-Go / One-Pass)

---

## 📝 Problem Statement
Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

---

## 💡 The "Aha!" Moments & Corrections
* **Set vs. Map:** Since the problem only asks for *existence* (true/false) and does not require us to return indices, a `HashSet` is strictly better than a `HashMap`. It avoids storing unnecessary dummy values, saving overhead.
* **The Space Optimization Fallback:** If the interviewer asks to solve this with $\mathcal{O}(1)$ space, we can sort the array first (which takes $\mathcal{O}(n \log n)$ time) and then check adjacent elements (`nums[i] == nums[i+1]`). But given space is allowed, the Hash Set is the optimal $\mathcal{O}(n)$ time solution.
* **Loop Boundary Precision:** Always ensure loop boundaries check `nums.length`, not `nums[i]`, to prevent erratic early termination or out-of-bounds exceptions.

---

## 🧠 The Optimal Mental Model (3-Step Engine)
1. **Walk:** Traverse the array from left to right.
2. **Check:** Does `nums[i]` already exist in my Hash Set (my history)? If yes, return `true`.
3. **Store:** If no, add `nums[i]` to the Hash Set and continue.

---

## 💻 Final Java Solutions

### Approach 1: Hash Set (Optimal Time: $\mathcal{O}(n)$, Space: $\mathcal{O}(n)$)
```java
class Solution {
    public boolean hasDuplicate(int[] nums) {
        // Base case: An array with 0 or 1 element cannot have duplicates
        if (nums == null || nums.length < 2) {
            return false;
        }

        // Hash Set to keep track of elements we've already visited
        Set<Integer> visited = new HashSet<>();

        for (int i = 0; i < nums.length; i++) {
            // If the element is already in our history, we found a duplicate
            if (visited.contains(nums[i])) {
                return true;
            }
            // Otherwise, commit it to history
            visited.add(nums[i]);
        }

        // If we finish the loop, all elements are unique
        return false;
    }
}
```

### Approach 2: Sorting (Optimal Space: $\mathcal{O}(1)$, Time: $\mathcal{O}(n \log n)$)
```java
class Solution {
    public boolean hasDuplicate(int[] nums) {
        // Sorting time: O(n log n)
        // Traversing array time: O(n)
        // Total time: O(n log n)
        // Space: O(1)

        Arrays.sort(nums); // O(n log n)

        int n = nums.length;

        for (int i = 0; i < n - 1; i++) { // O(n)
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }

        return false;
    }
}
```
