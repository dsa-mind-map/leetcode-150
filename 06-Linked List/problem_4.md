# 4. Reverse Linked List (LeetCode 206)

**Alignment:** Pillar 2 (Pointer Re-routing), Block 1 (The Reversal Engine)  

---

## 📋 Problem Description
Given the beginning of a singly linked list `head`, reverse the list, and return the new beginning of the list.

* **Example 1:**
  * Input: `head = [0,1,2,3]`
  * Output: `[3,2,1,0]`

**Constraints:**
* $0 \le \text{Length of the list} \le 1000$

---

## ⚠️ Common Pitfalls
Forgetting Step 1 of the 3-step dance (`ListNode nextTemp = curr.next`). If you change `curr.next` to `prev` before saving the forward path, you instantly sever the rest of the list.

---

## 💻 Java Implementation

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```