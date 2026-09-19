# 6. Reverse Nodes in k-Group (LeetCode 25)

**Alignment:** Pillar 2 (Pointer Re-routing), Pillar 3 (Sentinel), Block 1 (Reversal Engine)  
**Additional Learning:** Managing state across multiple iterations. Counting nodes before acting is crucial to leave remaining nodes (length < $k$) untouched.

---

## 📋 Problem Description
Given the head of a singly linked list and a positive integer `k`, reverse the nodes of the list $k$ at a time, and return the modified list. If there are fewer than $k$ nodes left, leave them as they are.

* **Example 1:**
  * Input: `head = [1,2,3,4,5,6]`, `k = 3`
  * Output: `[3,2,1,6,5,4]`

**Constraints:**
* $1 \le k \le n \le 5000$

---

## ⚠️ Common Pitfalls
Forgetting to update the `pre` pointer to the end of the newly reversed group before starting the next $k$-group reversal.

---

## 💻 Java Implementation

```java
public ListNode reverseKGroup(ListNode head, int k) {
    if (head == null || k == 1) return head;
    
    ListNode dummy = new ListNode(0, head);
    ListNode curr = dummy, nxt = dummy, pre = dummy;
    int count = 0;
    
    while (curr.next != null) {
        curr = curr.next;
        count++;
    }
    
    while (count >= k) {
        curr = pre.next;
        nxt = curr.next;
        for (int i = 1; i < k; i++) {
            curr.next = nxt.next;
            nxt.next = pre.next;
            pre.next = nxt;
            nxt = curr.next;
        }
        pre = curr;
        count -= k;
    }
    
    return dummy.next;
}
```