# 11. Container With Most Water (LeetCode)

**Difficulty:** Medium
**Topic:** Two Pointers
**Constraint:** Must solve in $\mathcal{O}(n)$ time and $\mathcal{O}(1)$ space.

---

## 📝 Problem Description

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return *the maximum amount of water a container can store*.

* **Notice:** You may not slant the container.

### Examples

**Example 1:**
* **Input:** `height = [1,8,6,2,5,4,8,3,7]`
* **Output:** `49`
* **Explanation:** The above vertical lines are represented by array `[1,8,6,2,5,4,8,3,7]`. In this case, the max area of water the container can store is `49` (formed by index 1 and index 8: height `8` and `7`, width `8 - 1 = 7`, area = `7 * 7 = 49`).

**Example 2:**
* **Input:** `height = [1,1]`
* **Output:** `1`

### Constraints
* `n == height.length`
* `2 <= n <= 10^5`
* `0 <= height[i] <= 10^4`

---

## 🧠 The Mental Model: Opposite-End Pointers & The Greediness Rule
A brute-force check of every possible pair takes $\mathcal{O}(n^2)$ time, which times out. 

To achieve $\mathcal{O}(n)$ time, we use **Opposite-End Pointers**:
1. Place `start = 0` at the beginning and `end = n - 1` at the end.
2. The area is calculated as: $\text{Area} = (\text{end} - \text{start}) \times \min(\text{height}[\text{start}], \text{height}[\text{end}])$.
3. **Understanding Width (Index Distance vs. Position):**
   When calculating width, subtracting indices gives the exact same distance as subtracting 1-based physical positions (sizes), because the relative offset cancels out:
   * If something is at **Index 1**, it is physically the **2nd element** (position/size 2).
   * If something is at **Index 7**, it is physically the **8th element** (position/size 8).
   * Calculating the gap using positions: $8 - 2 = 6$.
   * Calculating the gap using indices: $7 - 1 = 6$.
4. **The Greedy Rule:** Moving any pointer inward guarantees a shrinking width. Therefore, our only hope for a larger area is finding a taller line. Since water is bottlenecked by the shorter wall, **we always move the pointer pointing to the shorter height inward**, preserving the taller wall in hopes of finding an even taller or wider combination.

---

## 💻 Optimal Java Solution ($\mathcal{O}(n)$ Time)

```java
class Solution {
    public int maxArea(int[] heights) {
        int n = heights.length;

        int start = 0;
        int end = n - 1;
        int maxArea = 0;

        while (start < end) {
            // 1. Calculate area (Width * min Height)
            int area = (end - start) * Math.min(heights[start], heights[end]);
            
            // 2. Update maxArea if current is larger
            if (area > maxArea) {
                maxArea = area;
            }
            
            // 3. Greedy pointer movement: Always abandon the shorter wall
            // to avoid losing the chance of finding a taller boundary.
            if (heights[start] <= heights[end]) {
                start++;
            } else {
                end--;
            }
        }

        return maxArea;
    }
}
