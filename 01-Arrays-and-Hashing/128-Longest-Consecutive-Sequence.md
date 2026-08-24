# 9. Longest Consecutive Sequence (LeetCode 128)

**Difficulty:** Medium
**Topic:** Arrays & Hashing
**Constraint:** Must run in strictly $\mathcal{O}(n)$ time.

---

## 🧠 The Mental Model: Finding the Starting Line
If we can't sort the array, we must use a `HashSet` to achieve $\mathcal{O}(1)$ lookups. 

To avoid doing $\mathcal{O}(n^2)$ repeated work, we only build a sequence if we are standing at the **start** of a sequence. A number is the start of a sequence *only* if `(number - 1)` is **not** in the `HashSet`.

For example, in the array `[100, 4, 200, 1, 3, 2]`:
*   Is 3 a start? No, because 2 is in the set. (Skip)
*   Is 2 a start? No, because 1 is in the set. (Skip)
*   Is 1 a start? Yes! 0 is not in the set. (Start counting: 1, 2, 3, 4).

---

## 💻 Optimal Java Solution ($\mathcal{O}(n)$ Time)

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums == null || nums.length == 0) return 0;

        // 1. Throw everything into a HashSet for O(1) lookups
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }

        int longestStreak = 0;

        // 2. Iterate through the set
        for (int num : set) {
            
            // 3. ONLY start counting if this is the start of a sequence!
            if (!set.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                // 4. Count upwards as far as we can go
                while (set.contains(currentNum + 1)) {
                    currentNum++;
                    currentStreak++;
                }

                // 5. Update our max streak
                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
}
