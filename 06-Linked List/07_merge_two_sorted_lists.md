# 7. Merge Two Sorted Lists (LeetCode 21)

**Alignment:** Pillar 3 (Sentinel Pattern), Block 3 (The Two-List Combiner)  
**Additional Learning:** Efficiently merging two pre-sorted streams side-by-side using a sentinel head to eliminate edge cases.

---

## 🏛️ Core Architectural Concept
When combining two sorted linked lists, managing shifting heads or empty inputs can lead to bugs. By initializing a **Sentinel (Dummy Head)** node, we establish a safe anchor point. We can then compare nodes from `list1` and `list2` iteratively, append the smaller node to our traversal pointer, and attach any remaining list in $O(1)$ time.

---

## 📋 Problem Description
You are given the heads of two sorted linked lists `list1` and `list2`. Merge the two lists into one sorted linked list and return its head.

* **Example 1:**
  * Input: `list1 = [1,2,4]`, `list2 = [1,3,5]`
  * Output: `[1,1,2,3,4,5]`
* **Example 2:**
  * Input: `list1 = []`, `list2 = [1,2]`
  * Output: `[1,2]`
* **Example 3:**
  * Input: `list1 = []`, `list2 = []`
  * Output: `[]`

### Constraints
* $0 \le \text{Length of each list} \le 100$
* $-100 \le \text{Node.val} \le 100$

---

## ⚠️ Common Pitfalls
Using a loop condition like `while (l1 != null || l2 != null)` forces messy internal null checks. The cleaner, idiomatic approach is `while (l1 != null && l2 != null)` to process overlapping nodes, followed by directly attaching the remaining list tail.

---

## 💻 Optimized Java Implementation

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // Sentinel dummy node to protect and anchor the new merged list head
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;

        // Compare and merge nodes while both lists have active elements
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                curr.next = list1; // CRITICAL: Attach smaller node from list1
                list1 = list1.next;
            } else {
                curr.next = list2; // CRITICAL: Attach smaller node from list2
                list2 = list2.next;
            }
            curr = curr.next; // Advance tail pointer
        }

        // CRITICAL: Attach the remaining tail directly from whichever list is still active
        if (list1 != null) {
            curr.next = list1;
        }
        if (list2 != null) {
            curr.next = list2;
        }

        return dummy.next; // Return the true head, skipping the sentinel dummy node
    }
}
