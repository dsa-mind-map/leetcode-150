# 6. Reverse Nodes in k-Group (LeetCode 25)

**Alignment:** Pillar 2 (Pointer Re-routing), Pillar 3 (Sentinel Pattern), Block 1 (Reversal Engine)  
**Additional Learning:** Managing state across multiple iterations. Counting total nodes beforehand is crucial to ensure that any remaining tail nodes with a length less than $k$ are left completely untouched.

---

## 🏛️ Core Architectural Concept
To reverse nodes in groups of $k$, we utilize a **Sentinel (Dummy Head)** to safely manage boundary shifts. 
1. **Count Nodes:** First, traverse the list to count total nodes, ensuring we only reverse full $k$-sized groups.
2. **In-Place Group Reversal:** For each group, we use localized pointer manipulation (`pre`, `curr`, `nxt`) to rotate nodes one by one without modifying node values.
3. **Anchor Shift:** Relocate the `pre` pointer to the end of the newly reversed group before processing the next batch.

---

## 📋 Problem Description
Given the head of a singly linked list and a positive integer `k`, reverse the nodes of the list $k$ at a time, and return the modified list. If there are fewer than $k$ nodes left at the end, leave them as they are.

* **Example 1:**
  * Input: `head = [1,2,3,4,5,6]`, `k = 3`
  * Output: `[3,2,1,6,5,4]`
* **Example 2:**
  * Input: `head = [1,2,3,4,5]`, `k = 3`
  * Output: `[3,2,1,4,5]`

### Constraints
* The length of the linked list is $n$.
* $1 \le k \le n \le 5000$
* $0 \le \text{Node.val} \le 100$

---

## ⚠️ Common Pitfalls
Forgetting to update the `pre` pointer to the tail of the newly reversed group before moving on to the next $k$-group. If `pre` is not updated, subsequent reversals will corrupt the list linkages and detach previous groups.

---

## 💻 Optimized Java Implementation

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || k == 1) return head;
        
        // Sentinel dummy node to protect and anchor the head changes
        ListNode dummy = new ListNode(0, head);
        ListNode curr = dummy, nxt = dummy, pre = dummy;
        int count = 0;
        
        // Count total nodes in the linked list
        while (curr.next != null) {
            curr = curr.next;
            count++;
        }
        
        // Iteratively reverse groups of size k as long as enough nodes remain
        while (count >= k) {
            curr = pre.next; // CRITICAL: curr points to the first node of the current group
            nxt = curr.next; // CRITICAL: nxt points to the second node of the current group
            
            // Perform in-place rotation for k - 1 links
            for (int i = 1; i < k; i++) {
                curr.next = nxt.next; // Bypass nxt forward
                nxt.next = pre.next;  // Point nxt backward to the current group head
                pre.next = nxt;       // Bring nxt to the front of the group
                nxt = curr.next;      // Advance nxt to the next candidate node
            }
            
            pre = curr;       // CRITICAL: Shift pre to the tail of the newly reversed group
            count -= k;       // Decrement remaining count by k
        }
        
        return dummy.next; // Return the true head of the modified list
    }
}
