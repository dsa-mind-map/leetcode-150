# Sliding Window Maximum (239)

**Platform:** [LeetCode](https://leetcode.com/problems/sliding-window-maximum/)  
**Difficulty:** Hard  
**Pattern:** Sliding Window (Fixed Step) + Frequency TreeMap

---

## 📝 Problem Description

You are given an array of integers `nums`, and there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position, return the max sliding window.

---

## 🧠 Pattern Recognition & Approach

* **The Trigger:** The requirement to evaluate sliding windows of a specific, exact length (`k`) and dynamically track a running maximum maps directly to **Pattern 1: Fixed Sliding Window**.
* **The Strategy:** We use the universal Fixed Window template where the Head (`end`) expands and the Tail (`start`) follows. To manage elements dynamically while keeping them sorted for instant maximum retrieval without an $\mathcal{O}(k)$ scan, we use a **`TreeMap`** configured as a frequency map (`value -> frequency`).

---

## 💡 Key Learnings & Pitfalls

* **Why HashSet Fails (The Duplicate Removal Trap):** A `HashSet` can only keep unique elements. If the window contains duplicate maximum elements (e.g., `[3, 1, 3]`), removing the `start` element from a `HashSet` would completely delete the value `3` from the collection. Even though that exact same `3` is still validly sitting inside the remaining window (`start + 1` to `end`), the set loses it entirely. This breaks future max calculations because the true maximum is prematurely deleted.
* **The TreeMap Frequency Solution:** To solve this, we must map `value -> frequency` using a `TreeMap`. When the Tail slides, we decrement the frequency of `startElement`. We only invoke `treemap.remove()` when the frequency hits `0`, guaranteeing that duplicate elements sharing the maximum value remain alive as long as at least one instance is still inside the active window.
* **Handy TreeMap API Utilities:** 
  * `treemap.lastKey()` instantly returns the highest key, giving us our current window maximum in $\mathcal{O}(\log k)$ time.
  * `treemap.getOrDefault(key, 0)` safely retrieves or initializes counts during insertion.
  * `treemap.get(key)` checks current frequencies before deletion.
* **The Golden Loop Rule:** The Head (`end`) must always take its step forward at the **very bottom** of the `while` loop. Operate on the current valid window first, *then* increment into the future.

---

## 💻 Java Code

```java
// Strictly adhering to the agreed-upon pattern template
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        
        // 1. Initialize pointers and state
        int n = nums.length;
        int start = 0;
        int end = 0;

        TreeMap<Integer, Integer> treemap = new TreeMap<>();
        List<Integer> list = new ArrayList<>();

        // 2. The while loop (Head traverses)
        while (end < n) {
            
            // 3. Update state based on Head
            int endElement = nums[end];
            treemap.put(endElement, treemap.getOrDefault(endElement, 0) + 1);

            // 4. Validate Window (Fixed size check for Tail movement)
            if (end - start + 1 == k) {
                
                // Evaluate current valid window (Find max and add to list)
                list.add(treemap.lastKey());

                // Prepare to slide: Tail drops the left element
                int startElement = nums[start];
                treemap.put(startElement, treemap.get(startElement) - 1);

                // Remove the key entirely only if its frequency drops to 0
                if (treemap.get(startElement) == 0) {
                    treemap.remove(startElement);
                }

                start++; // Slide the Tail
            }

            // 5. Head takes its step forward at the END of the loop
            end++;
        }

        return list.stream().mapToInt(i -> i).toArray();
    }
}
```

---

## ⏱️ Complexity Analysis

* **Time Complexity:** $\mathcal{O}(n \log k)$
  * Inserting, deleting, and retrieving the maximum (`lastKey()`) in a `TreeMap` takes logarithmic time relative to the number of unique elements in the window (`k`). Since we do this for all `n` elements, the total time is $\mathcal{O}(n \log k)$.
* **Space Complexity:** $\mathcal{O}(k)$
  * The `TreeMap` stores at most `k` unique elements at any given time, maintaining linear-bounded extra space.
