# 8. Add Two Numbers (LeetCode 2)

**Alignment:** Pillar 3 (Sentinel Pattern), Block 3 (Combiner with Math Logic)  
**Additional Learning:** Simulating elementary school column addition from right to left by maintaining and passing a `carry` variable across loop iterations.

---

## 🏛️ Core Architectural Concept
Since digits are stored in **reverse order** (e.g., $321$ is stored as $1 \rightarrow 2 \rightarrow 3$), the head of each linked list represents the least significant digit (ones place). This aligns perfectly with standard addition flow, allowing us to traverse both lists simultaneously, compute sums digit-by-digit, and propagate any `carry` forward.

---

## 📋 Problem Description
You are given two non-empty linked lists representing two non-negative integers. Add the two numbers and return the sum as a linked list.

* **Example 1:**
  * Input: `l1 = [1,2,3]`, `l2 = [4,5,6]`
  * Output: `[5,7,9]` ($321 + 654 = 975$)
* **Example 2:**
  * Input: `l1 = [9]`, `l2 = [9]`
  * Output: `[8,1]` ($9 + 9 = 18$)

### Constraints
* $1 \le \text{l1.length}, \text{l2.length} \le 100$
* $0 \le \text{Node.val} \le 9$

---

## ⚠️ Common Pitfalls
Forgetting to include `carry != 0` in the loop condition. If you add $99 + 1 = 100$, both lists will be exhausted, but a final carry of `1` remains. Omitting `carry != 0` terminates the loop early, dropping the final leading digit.

---

## 💻 Optimized Java Implementation

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0); // Sentinel head protection
        ListNode tail = dummy;
        int carry = 0;
        
        // CRITICAL: Loop must continue if either list has nodes OR if a leftover carry exists
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
            
            carry = sum / 10;                   // CRITICAL: Extract overflow for the next digit position
            tail.next = new ListNode(sum % 10); // CRITICAL: Append current digit remainder as a new node
            tail = tail.next;                   // Advance tail pointer
        }
        
        return dummy.next; // Return the true head of the resulting linked list
    }
}
