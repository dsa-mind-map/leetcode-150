# Sliding Window Maximum (239)

**Platform:** [LeetCode](https://leetcode.com/problems/sliding-window-maximum/)  
**Difficulty:** Hard  
**Pattern:** Sliding Window (Fixed Step) + Frequency TreeMap

---

## 📝 Problem Description

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

**Example 1:**
* **Input:** `nums = [1,3,-1,-3,5,3,6,7], k = 3`
* **Output:** `[3,3,5,5,6,7]`
* **Explanation:**

* Window position                Max

[1  3  -1] -3  5  3  6  7       3
1 [3  -1  -3] 5  3  6  7       3
1  3 [-1  -3  5] 3  6  7       5
1  3  -1 [-3  5  3] 6  7       5
1  3  -1  -3 [5  3  6] 7       6
1  3  -1  -3  5 [3  6  7]      7

---

## 🚀 Solution 1: Frequency TreeMap Approach

### Approach & Pattern Recognition
* **The Trigger:** The requirement to evaluate sliding windows of a specific, exact length (`k`) and dynamically track a running maximum maps directly to **Pattern 1: Fixed Sliding Window**.
* **The Strategy:** We use the universal Fixed Window template where the Head (`end`) expands and the Tail (`start`) follows. To manage elements dynamically while keeping them sorted for instant maximum retrieval without an $\mathcal{O}(k)$ scan, we use a **`TreeMap`** configured as a frequency map (`value -> frequency`).

### Key Learnings & Pitfalls
* **Why HashSet Fails (The Duplicate Removal Trap):** A `HashSet` can only keep unique elements. If the window contains duplicate maximum elements (e.g., `[3, 1, 3]`), removing the `start` element from a `HashSet` would completely delete the value `3` from the collection. Even though that exact same `3` is still validly sitting inside the remaining window (`start + 1` to `end`), the set loses it entirely. This breaks future max calculations because the true maximum is prematurely deleted.
* **The TreeMap Frequency Solution:** To solve this, we must map `value -> frequency` using a `TreeMap`. When the Tail slides, we decrement the frequency of `startElement`. We only invoke `treemap.remove()` when the frequency hits `0`, guaranteeing that duplicate elements sharing the maximum value remain alive as long as at least one instance is still inside the active window.
* **Handy TreeMap API Utilities:** 
  * `treemap.lastKey()` instantly returns the highest key, giving us our current window maximum in $\mathcal{O}(\log k)$ time.
  * `treemap.getOrDefault(key, 0)` safely retrieves or initializes counts during insertion.
  * `treemap.get(key)` checks current frequencies before deletion.
* **The Golden Loop Rule:** The Head (`end`) must always take its step forward at the **very bottom** of the `while` loop. Operate on the current valid window first, *then* increment into the future.

### Java Code
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {

        int n = nums.length;

        int start = 0;
        int end = 0;

        TreeMap<Integer, Integer> treemap = new TreeMap<>();

        List<Integer> list = new ArrayList<>();

        while( end < n){

            int endElement = nums[end];
            treemap.put(endElement, treemap.getOrDefault(endElement,0)+1);

            if( end - start + 1 == k){

                // find max & add to list
                list.add(treemap.lastKey());


                // remove start
                int startElement = nums[start];
                treemap.put(startElement, treemap.get(startElement)-1);

                if(treemap.get(startElement) == 0){
                    treemap.remove(startElement);
                }

                start++;

            }

            end++;

        }

        return list.stream().mapToInt(i->i).toArray();
        
    }
}
```

---

### Complexity Analysis (Solution 1)

* **Time Complexity:** $\mathcal{O}(n \log k)$, where $N$ is the length of the array and $K$ is the window size. Inserting, deleting, and retrieving the maximum (`lastKey()`) in a `TreeMap` takes logarithmic time relative to the number of unique elements in the window ($k$). Since we do this for all $n$ elements, the total time is $\mathcal{O}(n \log k)$.
* **Space Complexity:** $\mathcal{O}(k)$. The `TreeMap` stores at most $k$ unique elements at any given time, maintaining linear-bounded extra space.
