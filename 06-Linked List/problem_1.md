# 1. Linked List Cycle (LeetCode 141)

**Alignment:** Pillar 4 (Fast & Slow Pointers), Block 4 (Cycle Detection)  
**Additional Learning:** Recognizing that pointer equality (`slow == fast`) denotes a cycle, not value equality.

---

## 📋 Problem Description
Given the beginning of a linked list head, return `true` if there is a cycle in the linked list. Otherwise, return `false`.

There is a cycle in a linked list if at least one node in the list can be visited again by following the next pointer.

* **Example 1:**
  * Input: `head = [1,2,1]`, `index = -1`
  * Output: `false`
* **Example 2:**
  * Input: `head = [1,2,3,4]`, `index = 1`
  * Output: `true` (tail connects to the 1st node, 0-indexed)

**Constraints:**
* $0 \le \text{Length of the list} \le 1000$
* $-1000 \le \text{Node.val} \le 1000$

---

## ⚠️ Common Pitfalls
* Checking `fast != null` but forgetting to check `fast.next != null` before executing `fast.next.next`, leading to a `NullPointerException`.
* Comparing node values instead of node memory references.

---

## 💻 Java Implementation

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while(fast != null && fast.next != null){
        slow = slow.next; 
        fast = fast.next.next; 
        if(slow == fast) return true;
    }
    return false;
}
```