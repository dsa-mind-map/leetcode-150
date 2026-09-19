# 5. Reverse Linked List II (LeetCode 92)

**Alignment:** Pillar 2 (Pointer Re-routing), Pillar 3 (Sentinel Pattern), Block 1 (Reversal Engine sub-range)  
**Additional Learning:** How to surgically extract a sub-list, reverse it, and patch it back in by retaining pointers to the nodes right before and after the cut.

---

## 📋 Problem Description
Given the head of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right` (1-indexed).

* **Example 1:**
  * Input: `head = [1,2,3,4,5]`, `left = 2`, `right = 4`
  * Output: `[1,4,3,2,5]`

**Constraints:**
* $1 \le n \le 500$
* $1 \le \text{left} \le \text{right} \le n$

---

## ⚠️ Common Pitfalls
Re-wiring edges incorrectly. After the reversal engine finishes, `prev` is the new head of the sub-list, but the original `curr` pointer (which was at `left`) is now the tail and must point to the node after `right`.

---

## 💻 Java Implementation

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        ListNode dummy = new ListNode(0, head); 
        ListNode curr = dummy;

        ListNode prevLeft = null; // node just before left
        for(int i = 0; i < left; i++){
            prevLeft = curr;
            curr = curr.next;
        }

        ListNode leftNode = prevLeft.next; // node at left 

        ListNode prev = null;
        while(curr != null && left <= right){
            ListNode temp = curr.next;
            curr.next = prev;

            prev = curr;
            curr = temp;

            left++;
        }

        leftNode.next = curr; 
        prevLeft.next = prev; 

        return dummy.next;
    }
}
```