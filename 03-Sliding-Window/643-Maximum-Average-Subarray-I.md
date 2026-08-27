# 643. Maximum Average Subarray I

**Difficulty:** Easy  
**Topic:** Arrays / Fixed Sliding Window

---

## 📝 Problem Description

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.
Find aHere is a clean, structured Markdown file for your solution. 

> **A Quick Note:** I have slightly adjusted your initial `avgMax` declaration in the code below. Initializing `avgMax = 0` will cause your solution to fail on LeetCode if the array contains all negative numbers (e.g., `[-1, -5, -3]`). Changing it to `-Double.MAX_VALUE` ensures it works for negative constraints as well.

***

```markdown
# Maximum Average Subarray I (643)

**Platform:** [LeetCode](https://leetcode.com/problems/maximum-average-subarray-i/)  
**Difficulty:** Easy  
**Pattern:** Sliding Window

## Approach
This solution leverages the **Sliding Window** technique to find the maximum average of any contiguous subarray of size `k`. 

Instead of recalculating the sum of `k` elements from scratch every time (which would take $O(n \times k)$ time), a window of size `k` is maintained. As the window slides to the right, the new element is added to the `windowSum`, and the element left behind is subtracted. 

## Java Code

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        
        int n = nums.length;
        int start = 0;
        int end = 0;
        int windowSum = 0;
        int count = 0;
        
        // Initialize to the smallest possible double to handle negative arrays
        double avgMax = -Double.MAX_VALUE; 

        // nums = [1,12,-5,-6,50,3], k = 4
        //         0  1  2  3  4  5
        // start   end            windowsum        avgMax       count
        // 1>2      1->2->3>4>5>6    -6>50>3      0           1->2->3>4>3>4>3

        while(end < n) {

            windowSum = windowSum + nums[end];
            end++; // next element index
            count++; // number of elements in the window

            if(count == k) {
                double avg = (double) windowSum / k;
                
                if(avg > avgMax) {
                    avgMax = avg;
                }

                // remove tail element to slide the window
                windowSum = windowSum - nums[start];
                start++;
                count--;
            }
        }
        
        return avgMax;
    }
}
```
---

## ⏱️ Complexity Analysis

* **Time Complexity:** $O(n)$
  * The `while` loop iterates through the `nums` array exactly once. Both `start` and `end` pointers only move forward, performing constant-time operations at each step.
* **Space Complexity:** $O(1)$
  * The algorithm only uses a few integer and double variables (`start`, `end`, `windowSum`, `count`, `avgMax`) for tracking, requiring no extra auxiliary data structures.
