# Minimum Difference Between Highest and Lowest of K Scores (1984)

**Platform:** [LeetCode](https://leetcode.com/problems/minimum-difference-between-highest-and-lowest-of-k-scores/)  
**Difficulty:** Easy  
**Pattern:** Sorting + Sliding Window (Fixed Step Template)

---

## 📝 Problem Description

You are given a **0-indexed** integer array `nums`, where `nums[i]` represents the score of the $i^{th}$ student. You are also given an integer `k`.

Pick the scores of any `k` students from the array so that the **difference** between the **highest** and the **lowest** of the `k` scores is minimized. Return the minimum possible difference.

---

## 🧠 Pattern Recognition & Approach

* **The Transformation:** The problem asks us to pick "any" `k` scores. To minimize the difference, we need numbers that are as close in value as possible. By **sorting** the array, numbers with the closest values are forced to be physically adjacent.
* **The Sliding Window:** Once sorted, picking `k` scores with the smallest difference is identical to finding a contiguous subarray (window) of exactly size `k`. 
* **The Universal Template:** We apply the standard Fixed Sliding Window template. The Head (`end`) expands the window. When the window size reaches exactly `k`, we calculate the difference between `nums[end]` and `nums[start]`, record the minimum, and move the Tail (`start`) forward to maintain the fixed size.

---

## 💻 Java Code

```java
class Solution {
    public int minimumDifference(int[] nums, int k) {
        
        Arrays.sort(nums);

        int n = nums.length;
        int start = 0;
        int end = 0;
        int minDiff = Integer.MAX_VALUE;

        // nums = [1,4,7,9], k = 2
        while(end < n) {

            // 1. Check if window size is exactly k
            if(end - start + 1 == k) {
            
                int firstScore = nums[start];
                int secondScore = nums[end];

                int diff = secondScore - firstScore;

                if(diff < minDiff) {
                    minDiff = diff;
                }

                // 2. Prepare to slide: Tail moves forward
                start++;
            }

            // 3. Head always takes its step forward at the END of the loop
            end++; 
        }

        if(minDiff == Integer.MAX_VALUE) {
            return 0;
        } else {
            return minDiff;
        }
    }
}
