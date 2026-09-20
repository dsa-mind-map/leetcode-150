# 10. Copy List with Random Pointer (LeetCode 138)

**Alignment:** Pillar 5 (Multi-Pass Interweaving)  
**Additional Learning:** You can achieve an optimal $O(1)$ space complexity for deep copies (bypassing the traditional $O(N)$ auxiliary `HashMap`) by interweaving cloned nodes directly adjacent to their original counterparts.

---

## 🏛️ The Three-Phase Framework (O(1) Space Strategy)

* **Phase 1: Interweaving (`curr`, `clone`, `next`)**  
  * *Concept:* Create a new `Node(curr.val)` and zip it directly into the list (`clone.next = curr.next`, then `curr.next = clone`). Every original node is now immediately followed by its clone.
* **Phase 2: Wiring Random Pointers (`random`)**  
  * *Concept:* Hook up the clone's random pointer using its neighbor (`curr.next.random = curr.random.next`). Since the target clone sits right next to the original target, lookups take $O(1)$ time without a hash map.
* **Phase 3: Extraction / Untangling (`cloneHead`, `curr.next`)**  
  * *Concept:* Peel the two lists apart. Restore the original list (`curr.next = clone.next`) and stitch the cloned list together (`clone.next = clone.next.next`). Return the saved starting clone head (`head.next`).

---

## 📋 Problem Description
A linked list of length $n$ is given where each node contains an additional `random` pointer which could point to any node in the list or null. Construct a deep copy of the list.

### Constraints
* $0 \le n \le 100$
* $-100 \le \text{Node.val} \le 100$
* Node values are not guaranteed to be unique.
* `random` is null or points to some node in the linked list.

---

## ⚠️ Common Pitfalls
Attempting to set `random` pointers in the same loop where you create the cloned nodes will fail because the target node that `random` points to might not have been cloned yet. This problem strictly requires separate, multi-pass iterations.

---

## 💻 Optimized Java Implementation ($O(N)$ Time, $O(1)$ Space)

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        
        // ==========================================
        // PHASE 1: The Neighbor Blueprint (Interweaving)
        // ==========================================
        Node curr = head;
        while (curr != null) {
            Node clone = new Node(curr.val);
            clone.next = curr.next; // CRITICAL: Point clone to the remainder of the original list
            curr.next = clone;      // CRITICAL: Insert clone immediately after original node
            curr = clone.next;      // Advance to the next original node
        }
        
        // ==========================================
        // PHASE 2: Setting the Secret Paths (Random Pointers)
        // ==========================================
        curr = head;
        while (curr != null) {
            if (curr.random != null) {
                // CRITICAL: Cloned random points to target clone, which lives right next 
                // to the original target node (curr.random.next)
                curr.next.random = curr.random.next;  
            }
            curr = curr.next.next; // Jump two steps forward to reach the next original node
        }
        
        // ==========================================
        // PHASE 3: The Great Separation (Untangling)
        // ==========================================
        curr = head;
        Node cloneHead = head.next; // CRITICAL: Save the starting head of our deep-copied list
        while (curr != null) {
            Node clone = curr.next;
            curr.next = clone.next; // CRITICAL: Reconnect original node, bypassing the clone
            
            if (clone.next != null) {
                clone.next = clone.next.next; // CRITICAL: Connect clone node to the next clone
            }
            curr = curr.next; // Move curr forward to the next original node
        }
        
        return cloneHead; // Return the fully isolated deep-copied list
    }
}

```

---

## 💻 Alternative Approach: HashMap Implementation ($O(N)$ Time, $O(N)$ Space)

If you prefer a simpler mental model using auxiliary memory:

```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;

        Map<Node, Node> clonedMap = new HashMap<>();

        // Pass 1: Create all clones and map Original -> Clone
        Node curr = head;
        while (curr != null) {
            clonedMap.put(curr, new Node(curr.val));
            curr = curr.next;
        }

        // Pass 2: Wire next and random pointers using map lookups
        curr = head;
        while (curr != null) {
            Node cloneNode = clonedMap.get(curr);
            cloneNode.next = clonedMap.get(curr.next);     // CRITICAL: Map next pointer
            cloneNode.random = clonedMap.get(curr.random); // CRITICAL: Map random pointer
            curr = curr.next;
        }

        return clonedMap.get(head);
    }
}

```

```

```
