# 2. Remove Nth Node From End of List (LeetCode 19)

**Alignment:** Pillar 3 (Sentinel Pattern), Workflow 2 (Fast Gap ahead of Slow / Two-Pass vs One-Pass)  
**Additional Learning:** Maintaining a fixed sliding window of $N$ nodes between two pointers allows you to find and mutate end-relative nodes in a single pass, while protecting edge cases via a sentinel dummy node.

---

## 🏛️ Core Architectural Concepts

### Approach 1: Two-Pass Length Calculation (`TotalLength - N`)
1. **Calculate Length:** Traverse the list once from the dummy node to compute `totalLength`.
2. **Locate Target From Start:** Translate the end-relative index $N$ into a forward-relative index using the formula: $\text{NodeFromStart} = \text{totalLength} - n$.
3. **Mutate Link:** Advance to the node immediately preceding the target and perform `curr.next = curr.next.next`.

### Approach 2: One-Pass Sliding Window (`Fast & Slow Pointers`)
1. **Initialize Gap:** Start both `slow` and `fast` pointers at the sentinel `dummy` node.
2. **Create $N$-Node Offset:** Advance the `fast` pointer $n$ steps ahead, creating a fixed gap of size $n$ between `slow` and `fast`.
3. **Synchronous Traversal:** Move both pointers simultaneously until `fast` reaches the final node. At this point, `slow` rests precisely on the node *just behind* the target to be deleted.

---

## 📋 Problem Description
Given the head of a linked list and an integer $n$, remove the $n$-th node from the end of the list and return its head.

* **Example 1:**
  * Input: `head = [1,2,3,4]`, `n = 2`
  * Output: `[1,2,4]`
* **Example 2:**
  * Input: `head = [5]`, `n = 1`
  * Output: `[]`
* **Example 3:**
  * Input: `head = [1,2]`, `n = 2`
  * Output: `[2]`

### Constraints
* The number of nodes in the list is `sz`.
* $1 \le sz \le 30$
* $0 \le \text{Node.val} \le 100$
* $1 \le n \le sz$

---

## ⚠️ Common Pitfalls
If the list has 5 nodes and you need to remove the 5th node from the end (which is the actual head node itself), navigating without a Sentinel/Dummy node will cause a null pointer exception or lose the reference to the list entirely. Always start both pointers at a `dummy` node placed behind `head`.

---

## 💻 Optimized Implementations

### Approach 1: Two-Pass Length-Based Solution
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Sentinel dummy node placed behind head to handle head-deletion safely
        ListNode dummy = new ListNode(0, head);
        ListNode curr = dummy;

        int totalLength = 0;
        // CRITICAL: Count total nodes (excluding dummy)
        while (curr.next != null) {
            curr = curr.next;
            totalLength++;
        }

        // Translate end-relative n into a start-relative index
        int nodeFromStart = totalLength - n;

        curr = dummy;
        // CRITICAL: Advance curr to land precisely on the node just behind the target
        while (nodeFromStart > 0) {
            curr = curr.next;
            nodeFromStart--;
        }

        // Bypass the target node to delete it
        curr.next = curr.next.next;

        return dummy.next; // Return the protected head reference
    }
}
