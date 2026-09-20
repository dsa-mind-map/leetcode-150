# 9. Reorder List (LeetCode 143)

**Alignment:** Workflow 1 (Find Mid $\rightarrow$ Reverse Half $\rightarrow$ Merge)  
**Additional Learning:** Combines foundational blocks cleanly into an in-place rearrangement. You must physically split the list into two distinct halves before reversing and merging.

---

## 🏛️ The Three-Phase Composite Workflow

* **Phase 1: Find the Middle (`slow` & `fast`)**  
  * *Concept:* Use Tortoise & Hare pointers to find the exact midpoint. `slow` will land on the last node of the first half.
* **Phase 2: Sever and Reverse (`slow.next = null`)**  
  * *Concept:* Cut the connection between the first and second halves to prevent cycles. Reverse the entire second half using standard pointer re-routing (`prev`, `curr`, `nxt`).
* **Phase 3: Alternating Merge (`first` & `second`)**  
  * *Concept:* Interleave nodes from the first half and the reversed second half alternatingly (e.g., $0 \rightarrow n-1 \rightarrow 1 \rightarrow n-2$). Since index 0 (`head`) never changes position, a dummy head is not needed.

---

## 📋 Problem Description
You are given the head of a singly linked list. Reorder the list to follow this positional sequence:
$[0, n-1, 1, n-2, 2, n-3, \dots]$

* **Example 1:**
  * Input: `head = [2,4,6,8]`
  * Output: `[2,8,4,6]`
* **Example 2:**
  * Input: `head = [2,4,6,8,10]`
  * Output: `[2,10,4,8,6]`

### Constraints
* $1 \le \text{Length of the list} \le 1000$
* $1 \le \text{Node.val} \le 1000$

---

## ⚠️ Common Pitfalls
Not setting `slow.next = null` after finding the middle. If you omit this cut, the first and second halves will form a cyclic loop during the alternating merge step, resulting in infinite loops or memory errors.

---

## 💻 Optimized Java Implementation ($O(N)$ Time, $O(1)$ Space)

```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) return;
        
        // ==========================================
        // PHASE 1: Find the Middle (Tortoise & Hare)
        // ==========================================
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;         // Moves 1 step
            fast = fast.next.next;    // Moves 2 steps
        }
        // At this point, 'slow' is the last node of the first half
        
        // ==========================================
        // PHASE 2: Sever & Reverse the Second Half
        // ==========================================
        ListNode prev = null;
        ListNode curr = slow.next;
        slow.next = null; // CRITICAL: Physically sever the list to prevent cycles
        
        while (curr != null) {
            ListNode nxt = curr.next; // CRITICAL: Save forward path
            curr.next = prev;         // CRITICAL: Reverse pointer backward
            prev = curr;              // Shift prev forward
            curr = nxt;               // Shift curr forward
        }
        // 'prev' is now the new head of the reversed second half
        
        // ==========================================
        // PHASE 3: Alternating Merge (In-Place)
        // ==========================================
        ListNode first = head;
        ListNode second = prev;
        
        while (second != null) {
            ListNode t1 = first.next;  // CRITICAL: Save next node of first half
            ListNode t2 = second.next; // CRITICAL: Save next node of second half
            
            first.next = second;       // Link first node to second node
            second.next = t1;          // Link second node back to remainder of first half
            
            first = t1;                // Advance first pointer
            second = t2;               // Advance second pointer
        }
    }
}
