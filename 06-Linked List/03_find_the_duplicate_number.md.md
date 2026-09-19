# 3. Find the Duplicate Number (LeetCode 287)

**Alignment:** Pillar 4 (Fast & Slow), Block 4 (Cycle Detection), Workflow 3 (Array as Cycle)  
**Additional Learning:** Array values restricted from $1$ to $N$ can be treated as pointer references `nums[i] -> nums[nums[i]]`.

---

## 📋 Problem Description
You are given an array of integers `nums` containing $n + 1$ integers. Each integer in `nums` is in the range $[1, n]$ inclusive, with exactly one repeated integer. Return the repeated integer.

* **Example 1:**
  * Input: `nums = [1,2,3,2,2]`
  * Output: `2`

**Constraints:**
* $1 \le n \le 10,000$
* `nums.length == n + 1`

> **Note:** Because of the constraints, the Pigeonhole Principle guarantees that a cycle is always present—fast will never run out of bounds.

---

## ⚠️ Common Pitfalls
Attempting to sort or use a HashSet violates the problem constraints. Initializing both pointers to `nums[0]` is required since index 0 points safely into the cycle mapping.

---

## 💻 Java Implementation

```java
public int findDuplicate(int[] nums) {
    int slow = nums[0];
    int fast = nums[0];
    
    // Block 4: Find intersection point
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);
    
    // Find cycle entrance
    slow = nums[0];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}
```