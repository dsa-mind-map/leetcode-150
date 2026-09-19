
### `02_remove_nth_node_from_end.md`
```markdown
# 2. Remove Nth Node From End of List (LeetCode 19)

**Alignment:** Pillar 3 (Sentinel Pattern), Workflow 2 (Fast Gap ahead of Slow)  
**Additional Learning:** Maintaining a fixed sliding window of $N$ nodes between two pointers allows you to find the end-relative node in a single pass.

---

## 📋 Problem Description
Given the head of a linked list and an integer `n`, remove the `n`-th node from the end of the list and return its head.

* **Example 1:**
  * Input: `head = [1,2,3,4]`, `n = 2`
  * Output: `[1,2,4]`
* **Example 2:**
  * Input: `head = [5]`, `n = 1`
  * Output: `[]`

**Constraints:**
* $1 \le \text{sz} \le 30$
* $0 \le \text{Node.val} \le 100$
* $1 \le n \le \text{sz}$

---

## 💡 Hints & Common Pitfalls
* **Approach 1:** Calculate total length, find length from the start, and traverse `curr` until reaching the node behind the deletion target.
* **Approach 2:** Separate `slow` and `fast` pointers by `n` steps using a dummy head. Move them at the same speed until `fast` hits the end.
* **Common Pitfall:** Removing the head node without a sentinel/dummy node will cause a null pointer or lose references.

---

## 💻 Java Codebase

### Approach A: Length-Based Calculation
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode curr = dummy;
        int totalLength = 0;

        while(curr.next != null){ 
            curr = curr.next;
            totalLength++; 
        }

        int nodeFromStart = totalLength - n; 
        curr = dummy; 
        while(nodeFromStart > 0){
            curr = curr.next;
            nodeFromStart--;
        }
        curr.next = curr.next.next;

        return dummy.next;
    }
}

```
