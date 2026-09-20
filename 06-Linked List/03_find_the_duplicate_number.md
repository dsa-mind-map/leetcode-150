# 3. Find the Duplicate Number (LeetCode 287)

**Alignment:** Pillar 4 (Fast & Slow Pointers), Block 4 (Cycle Detection), Workflow 3 (Array as Cycle)  
**Additional Learning:** Treating array values restricted within the range $[1, N]$ as implicit pointer references (`nums[i] -> nums[nums[i]]`), mapping an array problem directly into a Linked List cycle detection problem.

---

## 🏛️ Core Architectural Concept (The Pigeonhole Principle & Cycle Mapping)
* **The Mathematical Guarantee:** Because an array of size $n + 1$ contains values exclusively in the range $[1, n]$, the **Pigeonhole Principle** guarantees that at least one duplicate value must exist. 
* **Array as a Linked List:** Since values point to indices, duplicate values mean multiple pointers point to the exact same index, forming a convergence. This creates an inevitable **linked list cycle**. 
* **Floyd's Tortoise & Hare:** We can safely detect the cycle entrance (the duplicate number) in $O(N)$ time and $O(1)$ space without modifying the array or using auxiliary HashSets.

---

## 📋 Problem Description
You are given an array of integers `nums` containing $n + 1$ integers where each integer is in the range $[1, n]$ inclusive. There is exactly one repeated integer in `nums`. Return this repeated integer.

* **Example 1:**
  * Input: `nums = [1,2,3,2,2]`
  * Output: `2`
* **Example 2:**
  * Input: `nums = [1,2,3,4,4]`
  * Output: `4`

### Follow-up & Constraints
* Can you solve the problem without modifying the array `nums` and using $O(1)$ extra space?
* $1 \le n \le 10,000$
* `nums.length == n + 1`
* $1 \le \text{nums[i]} \le n$

---

## ⚠️ Common Pitfalls
1. **Constraint Violations:** Attempting to sort the array or use a `HashSet` violates the $O(1)$ extra space or "do not modify array" follow-up constraints.
2. **Incorrect Pointer Initialization:** Initializing both pointers to index `0` is necessary because index `0` acts as a safe starting entry point outside the cyclic path (since no array value can be `0` given the $[1, n]$ range restriction).

---

## 💻 Optimized Java Implementation ($O(N)$ Time, $O(1)$ Space)

```java
class Solution {
    public int findDuplicate(int[] nums) {
        // Initialize both pointers using nums[0] to enter the pseudo-linked-list structure
        int slow = nums[0];
        int fast = nums[0];
        
        // ==========================================
        // PHASE 1: Find Intersection Point (Cycle Detection)
        // ==========================================
        do {
            slow = nums[slow];         // Tortoise moves 1 step forward
            fast = nums[nums[fast]];   // Hare moves 2 steps forward
        } while (slow != fast);        // CRITICAL: Loop runs until both pointers collide
        
        // ==========================================
        // PHASE 2: Find the Cycle Entrance (The Duplicate Number)
        // ==========================================
        slow = nums[0];                // Reset slow back to the start
        while (slow != fast) {
            slow = nums[slow];         // Both pointers now move at 1 step speed
            fast = nums[fast];         // They will collide precisely at the cycle entrance
        }
        
        return slow; // 'slow' lands directly on the repeated duplicate number
    }
}
