# 9. Reorder List (LeetCode 143)

**Alignment:** Workflow 1 (Find Mid $\rightarrow$ Reverse Half $\rightarrow$ Merge)  
**Additional Learning:** Combining Blocks 1, 2, and 3 cleanly. You must physically split the list into two distinct lists before merging.

---

## 📋 Problem Description
You are given the head of a singly linked-list. Reorder the nodes such that the original index sequence $[0, 1, 2, \dots, n-1]$ becomes $[0, n-1, 1, n-2, \dots]$.

* **Example 1:**
  * Input: `head = [2,4,6,8]`
  * Output: `[2,8,4,6]`

**Constraints:**
* $1 \le \text{Length of the list} \le 1000$

---

## ⚠️ Common Pitfalls
Not setting `slow.next = null` after finding the middle. If you don't sever the connection, the lists will form a cycle when you attempt to merge them.

---

## 💻 Java Implementation

```java
public void reorderList(ListNode head) {
    if (head == null || head.next == null) return;
    
    // Block 2: Middle Finder
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Block 1: Reversal Engine (on second half)
    ListNode prev = null, curr = slow.next;
    slow.next = null; // CRITICAL: Cut off first half
    while (curr != null) {
        ListNode nxt = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nxt;
    }
    
    // Block 3: Two-Pointer Alternating Merging
    ListNode first = head, second = prev;
    while (second != null) {
        ListNode t1 = first.next, t2 = second.next;
        first.next = second;
        second.next = t1;
        first = t1;
        second = t2;
    }
}
```
