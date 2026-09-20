# 8. Add Two Numbers (LeetCode 2)

**Alignment:** Pillar 3 (Sentinel Pattern), Block 3 (Combiner with Math Logic)  
**Additional Learning:** Simulating elementary addition requires passing a `carry` variable across loop iterations.

---

## 📋 Problem Description
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order. Add the two numbers and return the sum as a linked list.

* **Example 1:**
  * Input: `l1 = [1,2,3]`, `l2 = [4,5,6]`
  * Output: `[5,7,9]` ($321 + 654 = 975$)

**Constraints:**
* $1 \le \text{l1.length}, \text{l2.length} \le 100$
* $0 \le \text{Node.val} \le 9$

---

## ⚠️ Common Pitfalls
Forgetting to check if `carry != 0` at the very end of the loop. If $99 + 1 = 100$, the final '1' will be dropped if the loop terminates early.

---

## 💻 Java Implementation

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    int carry = 0;
    
    while (l1 != null || l2 != null || carry != 0) {
        int sum = carry;
        if (l1 != null) {
            sum += l1.val;
            l1 = l1.next;
        }
        if (l2 != null) {
            sum += l2.val;
            l2 = l2.next;
        }
        
        carry = sum / 10;
        tail.next = new ListNode(sum % 10);
        tail = tail.next;
    }
    
    return dummy.next;
}
```
