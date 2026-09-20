# 5. Reverse Linked List II (LeetCode 92)

**Alignment:** Pillar 2 (Pointer Re-routing), Pillar 3 (Sentinel Pattern), Block 1 (Reversal Engine sub-range)  
**Additional Learning:** How to surgically extract a sub-list, reverse it in-place, and patch it back seamlessly by retaining pointers to the nodes immediately before and after the extraction boundaries.

---

## 🏛️ Core Architectural Concept
To reverse a specific sub-range from index `left` to `right` in a single pass:
1. **Sentinel Protection:** Use a dummy head behind the actual head to handle edge cases where `left = 1` (the head itself gets reversed).
2. **Boundary Tracking:** Traverse forward until `prevLeft` rests right before the `left` position, and `leftNode` points to the starting node of the reversal zone.
3. **Sub-Range Reversal:** Apply standard pointer re-routing (`prev`, `curr`, `temp`) for exactly the specified range.
4. **Boundary Patching:** Reconnect the surrounding pointers so the preceding and succeeding segments correctly bridge to the newly reversed sub-list.

---

## 📋 Problem Description
Given the head of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right` (1-indexed), and return the reversed list.

* **Example 1:**
  * Input: `head = [1,2,3,4,5]`, `left = 2`, `right = 4`
  * Output: `[1,4,3,2,5]`
* **Example 2:**
  * Input: `head = [1,1]`, `left = 1`, `right = 1`
  * Output: `[1,1]`

### Constraints
* The number of nodes in the list is $n$.
* $1 \le n \le 500$
* $-500 \le \text{Node.val} \le 500$
* $1 \le \text{left} \le \text{right} \le n$

---

## ⚠️ Common Pitfalls
Mishandling edge linkages after the sub-range reversal finishes. The original `curr` pointer (which stood at `left`) becomes the new tail of the sub-list and **must** be explicitly wired to point to the remainder of the list (`curr` after the loop), while `prevLeft.next` must point to `prev` (the new head of the reversed sub-list).

---

## 💻 Optimized Java Implementation

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        
        // Sentinel dummy node to protect against head modifications when left = 1
        ListNode dummy = new ListNode(0, head); 
        
        ListNode curr = dummy;
        ListNode prevLeft = null; // Node just before the 'left' position
        
        // Traverse to find 'prevLeft' and the starting node of reversal
        for (int i = 0; i < left; i++) {
            prevLeft = curr;
            curr = curr.next;
        }

        ListNode leftNode = prevLeft.next; // CRITICAL: Save reference to the node at 'left' (will become sub-list tail)

        // Reverse the sub-list from 'left' to 'right'
        ListNode prev = null;
        while (curr != null && left <= right) {
            ListNode temp = curr.next; // CRITICAL: Save forward path
            curr.next = prev;         // CRITICAL: Reverse pointer backward
            
            prev = curr;              // Shift prev forward
            curr = temp;              // Shift curr forward
            left++;
        }

        // CRITICAL: Patch the surrounding boundaries back together
        leftNode.next = curr; // Connect the old sub-list tail to the remaining list
        prevLeft.next = prev; // Connect the pre-left node to the new sub-list head

        return dummy.next; // Return the true head of the modified list
    }
}
