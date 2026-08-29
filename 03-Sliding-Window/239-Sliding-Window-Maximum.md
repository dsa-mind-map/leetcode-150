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


# Sliding Window Maximum (239)

**Platform:** [LeetCode](https://leetcode.com/problems/sliding-window-maximum/)  
**Difficulty:** Hard  
**Pattern:** Sliding Window (Fixed Step) + Monotonic Deque

---

## 🚀 Solution: Monotonic Deque Approach

## DEQUE
As long as you are adding elements at one end and removing them from the opposite end, it is always a FIFO (First-In, First-Out) queue.

Add at Tail + Remove at Head: FIFO (Standard flow)

Add at Head + Remove at Tail: FIFO (Reverse flow—the first item you added gets pushed all the way to the tail over time, so it still gets removed first!)

It only turns into a LIFO (Stack) if you add and remove from the exact same end (e.g., add at head and remove at head).

### Approach & Pattern Recognition
* **The Trigger:** Finding the maximum value across a moving window of fixed size `k` maps directly to **Pattern 1: Fixed Sliding Window**.
* **The Strategy:** To avoid the $\mathcal{O}(n \times k)$ penalty of scanning every window, or the $\mathcal{O}(n \log k)$ overhead of a Balanced BST (`TreeMap`/`TreeSet`), we use a **Monotonic Deque** (`ArrayDeque` storing **indices**). We maintain a strict descending order inside the deque so that the maximum element is always instantly accessible at the front.

### Key Learnings & Pitfalls
* **Store Indices, Not Values:** Storing indices instead of raw values allows us to mathematically check whether an element has fallen out of the left window boundary (`i - k + 1`), which is impossible if we only store raw values.
* **The Monotonic Decreasing Rule:** Before adding a new element, we purge smaller elements from the **back** of the deque (`pollLast()`). These smaller elements are useless because they are smaller *and* older—they will never become the maximum.
* **Pruning Out-of-Bounds from the Front:** If the index at the front of the deque is older than our current window bounds (`i - k + 1`), we pop it from the **front** (`pollFirst()`).
* **Amortized $\mathcal{O}(1)$ Efficiency:** Even though there is a `while` loop inside the traversal loop to clear out smaller elements, each index is pushed into and popped from the deque at most once. This guarantees a true linear runtime.

### Java Code
```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || k <= 0) return new int[0];
        
        int n = nums.length;
        int[] result = new int[n - k + 1];
        int resIdx = 0;
        
        // Deque stores indices of the array elements
        Deque<Integer> deque = new ArrayDeque<>();
        
        for (int i = 0; i < n; i++) {
            
            // 1. Remove indices that are out of the current window range [i - k + 1, i]
            if (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
                deque.pollFirst();
            }
            
            // 2. Remove elements from the back that are smaller than the current element
            // (They are useless because current element is larger and more recent)
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }
            
            // 3. Push current element index to the back
            deque.offerLast(i);
            
            // 4. Record the maximum for the window once we reach size k
            if (i >= k - 1) {
                result[resIdx++] = nums[deque.peekFirst()];
            }
        }
        
        return result;
    }
}
```

---

## ⏱️ Complexity Analysis

* **Time Complexity:** $\mathcal{O}(n)$, where $N$ is the length of the array. Every index is pushed into the deque once and popped from the deque at most once. This guarantees linear runtime regardless of how large window $k$ grows.
* **Space Complexity:** $\mathcal{O}(k)$. In the worst-case scenario (a strictly increasing array), the deque will store up to $k$ indices at any given moment.
