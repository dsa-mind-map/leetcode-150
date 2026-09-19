# 10. Copy List with Random Pointer (LeetCode 138)

**Alignment:** Pillar 5 (Multi-Pass Interweaving)  
**Additional Learning:** You can avoid a HashMap ($O(N)$ space) for deep copies by cloning nodes and placing them immediately after the original nodes.

---

## 📋 Problem Description
A linked list of length $n$ is given where each node contains an additional `random` pointer which could point to any node in the list or null. Construct a deep copy of the list.

**Constraints:**
* $0 \le n \le 100$

---

## ⚠️ Common Pitfalls
Attempting to set `random` pointers in the same loop where you create the cloned nodes. The node that `random` points to might not have been cloned yet, requiring strict multi-pass iteration.

---

## 💻 Java Implementation (Interweaving Approach)

```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        
        // PHASE 1: Interweaving clone nodes right after original nodes
        Node curr = head;
        while (curr != null) {
            Node clone = new Node(curr.val);
            clone.next = curr.next;
            curr.next = clone;
            curr = clone.next;
        }
        
        // PHASE 2: Setting random pointers
        curr = head;
        while (curr != null) {
            if (curr.random != null) {
                curr.next.random = curr.random.next;  
            }
            curr = curr.next.next;
        }
        
        // PHASE 3: Untangling the interweaving
        curr = head;
        Node cloneHead = head.next;
        while (curr != null) {
            Node clone = curr.next;
            curr.next = clone.next;
            if (clone.next != null) {
                clone.next = clone.next.next;
            }
            curr = curr.next;
        }
        
        return cloneHead;
    }
}
```