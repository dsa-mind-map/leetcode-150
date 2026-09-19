# 2. Remove Nth Node From End of List (LeetCode 19)

**Alignment:** Pillar 3 (Sentinel Pattern), Workflow 2 (Fast Gap ahead of Slow)  
**Additional Learning:** Maintaining a fixed sliding window of $N$ nodes between two pointers allows you to find the end-relative node in a single pass.

---

## 📋 Problem Description
Given the head of a linked list and an integer `n`, remove the $n^{th}$ node from the end of the list and return its head.

* **Example 1:**
  * Input: `head = [1,2,3,4]`, `n = 2`
  * Output: `[1,2,4]`
* **Example 2:**
  * Input: `head = [5]`, `n = 1`
  * Output: `[]`

**Constraints:**
* $1 \le \text{sz} \le 30$
* $1 \le n \le \text{sz}$

---

## ⚠️ Common Pitfalls
If the list has 5 nodes and you need to remove the 5th node from the end (the head), navigating without a Sentinel/Dummy node will cause a null pointer or lose the list entirely. Always start both pointers at the Dummy node.

---

## 💻 Java Implementation (Two-Pointer Approach)

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Dummy node behind the "head" node
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode slow = dummy;
        ListNode fast = dummy; 

        // Fast will reach behind the node "to be deleted"
        while(n > 0){
            fast = fast.next;
            n--;
        }

        // Slow & fast move at the same speed until fast reaches last element
        while(fast != null && fast.next != null){
            fast = fast.next;
            slow = slow.next;
        }
        
        // Slow is the node behind "the node to be deleted"
        slow.next = slow.next.next;

        return dummy.next;
    }
}
```