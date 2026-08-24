# Pattern 2: Two Pointers
**Focus:** Eliminating nested loops by using two indices (pointers) to traverse a data structure simultaneously, usually bringing **O(n²)** time down to **O(n)**. 

## 📚 Trigger Dictionary

### Group 1: Converging Pointers (The Classics)
* **Problem:** [Valid Palindrome](125-Valid-Palindrome.md)
  * **Trigger Identification:** "Is it a palindrome" or "Reads the same forward and backward." A palindrome is perfectly symmetrical. This immediately triggers **Opposite-End Pointers**. You place one pointer at index `0` and one at `array.length - 1`, moving them inward and comparing characters until they meet in the middle.

### Group 2: Target Searching in Sorted Arrays
* **Problem:** [Two Sum II - Input Array Is Sorted](167-Two-Sum-II.md)
  * **Trigger Identification:** "Find two numbers that add up to target" AND "The array is already sorted" AND "Constant extra space." Because it is sorted, you do not need a Hash Map (which takes **O(n)** space). This triggers the **Directional Sum Pointers**. Start at opposite ends: if the sum is too big, move the right pointer left (to shrink the sum). If the sum is too small, move the left pointer right (to grow the sum).

* **Problem:** [3Sum](015-3Sum.md)
  * **Trigger Identification:** "Find three elements that sum to 0" AND "No duplicate triplets allowed." Three elements usually mean **O(n³)**. The trigger here is to **Sort the Array First**. Once sorted, you iterate through the array with a fixed pointer `i`, and the problem magically transforms into **Two Sum II** for the remaining elements, dropping the time to **O(n²)**. The sorted nature also makes skipping duplicates trivial.

### Group 3: Optimization & Greedy Boundaries (The Water Problems)
* **Problem:** [Container With Most Water](011-Container-With-Most-Water.md)
  * **Trigger Identification:** "Maximize the area" formed by vertical lines. Area is constrained by the *shorter* line and the *width* between them. This triggers the **Greedy Boundary Pointers**. You start at the absolute maximum width (pointers at `0` and `length - 1`). To find a larger area, your only hope is to increase the height, so you aggressively discard the shorter line and move its pointer inward.

* **Problem:** [Trapping Rain Water](042-Trapping-Rain-Water.md)
  * **Trigger Identification:** "Compute how much water it can trap." The trapped water at any specific spot depends strictly on the highest wall to its left and the highest wall to its right. While you *could* pre-calculate these in two **O(n)** space arrays, the optimal trigger is **Dynamic Two Pointers**. By maintaining a `left_max` and `right_max` integer and moving whichever pointer has the smaller max, you calculate the trapped water in a single pass with **O(1)** space.
