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
        int n = nums.length;
        int longestConSeq = 0;

        Set<Integer> set = new HashSet<>(); // space o(n) 

        for(int i=0; i< n; i++){
            set.add(nums[i]);
        }
       

        for(int i=0; i<n; i++){ //time o(n)

            int num = nums[i];
            if (!set.contains(num - 1)) {
                int count = 0;
                while(set.contains(num)){
                    count++;
                    num++;
                }

                if(longestConSeq < count)
                    longestConSeq = count;
            }
        }

        return longestConSeq;
    }
}

