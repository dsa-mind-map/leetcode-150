# 167. Two Sum II - Input Array Is Sorted (LeetCode)

**Difficulty:** Medium
**Topic:** Two Pointers
**Constraint:** Must use only $\mathcal{O}(1)$ extra space (Hash Map is banned).

---

## 📝 Problem Description

Given an array of integers `numbers` that is sorted in non-decreasing order.

Return the indices (1-indexed) of two numbers, `[index1, index2]`, such that they add up to a given target number `target` and `index1 < index2`. Note that `index1` and `index2` cannot be equal, therefore you may not use the same element twice.

There will always be exactly one valid solution.

### Examples

**Example 1:**
* **Input:** `numbers = [1,2,3,4]`, `target = 3`
* **Output:** `[1,2]`
* **Explanation:** The sum of 1 and 2 is 3. Since we are assuming a 1-indexed array, `index1 = 1`, `index2 = 2`. We return `[1, 2]`.

### Constraints
* `2 <= numbers.length <= 30000`
* `-1000 <= numbers[i] <= 1000`
* `numbers` is sorted in **non-decreasing order**.
* `-1000 <= target <= 1000`
* There is exactly one valid solution.

---

## 🧠 The Mental Model: Directional Sum Pointers
In the original *Two Sum* problem, the array was unsorted, forcing us to use a Hash Map which consumed $\mathcal{O}(n)$ extra space. 

Here, the interviewer bans the Hash Map by demanding **$\mathcal{O}(1)$ space**. 
The cheat code is the word **"Sorted"**. Because the array is sorted, the smallest elements live on the far left, and the largest elements live on the far right. 

We place our pointers at opposite ends (`start = 0`, `end = numbers.length - 1`) and evaluate their sum:
* If `sum == target`: We found our pair! We return `[start + 1, end + 1]` to satisfy the 1-indexed requirement.
* If `sum < target`: The sum is too small. We need a larger number, so we shift the `start` pointer right (`start++`).
* If `sum > target`: The sum is too big. We need a smaller number, so we shift the `end` pointer left (`end--`).

---

## 💻 Optimal Java Solution ($\mathcal{O}(1)$ Space)

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int n = numbers.length;

        int start = 0;
        int end = n - 1;

        while (start < end) {
            int sum = numbers[start] + numbers[end];

            if (sum < target) {
                // Sum is too small, increase the lower boundary
                start++;
            } else if (sum > target) {
                // Sum is too large, decrease the upper boundary
                end--;
            } else {
                // Target found, return 1-indexed positions
                return new int[] { start + 1, end + 1 };
            }
        }

        return new int[] {}; // Guaranteed to find a solution per constraints
    }
}
