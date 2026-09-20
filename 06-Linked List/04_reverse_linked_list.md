# 4. Reverse Linked List (LeetCode 206)

**Alignment:** Pillar 2 (Pointer Re-routing), Block 1 (The Reversal Engine)  
**Additional Learning:** The foundational "3-Step Dance" for mutating forward-pointing singly linked list references into backward pointers without losing track of remaining elements.

---

## 🏛️ Core Architectural Concept (The 3-Step Dance)
Singly linked lists point strictly in one direction (`curr -> next`). To reverse them in-place, we must coordinate three tracking variables:
1. **`prev`**: Tracks the trailing node (starts at `null` since the new tail's next pointer must be null).
2. **`curr`**: Tracks the active node being processed.
3. **`nextTemp`**: A temporary variable saving the forward path so we don't lose the rest of the list when we re-route the pointer.

---

## 📋 Problem Description
Given the beginning of a singly linked list `head`, reverse the list, and return the new beginning (head) of the reversed list.

* **Example 1:**
  * Input: `head = [0,1,2,3]`
  * Output: `[3,2,1,0]`
* **Example 2:**
  * Input: `head = []`
  * Output: `[]`

### Constraints
* $0 \le \text{Length of the list} \le 1000$
* $-1000 \le \text{Node.val} \le 1000$

---

## ⚠️ Common Pitfalls
Forgetting Step 1 (`ListNode nextTemp = curr.next`). If you modify `curr.next = prev` before saving the forward reference, you instantly sever the connection to the rest of the linked list, resulting in memory leaks or premature termination.

---

## 💻 Optimized Java Implementation

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        
        while (curr != null) {
            ListNode nextTemp = curr.next; // CRITICAL: 1. Save forward path before breaking link
            curr.next = prev;         // CRITICAL: 2. Reverse current pointer backward
            
            prev = curr;              // 3. Shift prev forward to current node
            curr = nextTemp;          // 4. Shift curr forward to saved next node
        }
        
        return prev; // 'prev' lands precisely on the new head of the reversed list
    }
}
