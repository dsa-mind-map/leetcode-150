Here is the complete single markdown file for `foundation.md`, with all Java blocks formatted with `java(start)` and ` (end) as requested:

```markdown
# Linked List Pattern: Master Reference Guide (Tier 1 & 2 Optimized for DSA & LLD)

Every Linked List problem relies on mastering how to manipulate pointers (`head`, `prev`, `curr`, `next`) without losing references, causing `NullPointerExceptions`, or breaking memory linkages. 

This guide strictly aligns core LeetCode problems to **Foundational Pillars**, **Essential Code Blocks**, and **Composite Workflows**. 

---
# 🔗 Linked List Mastery: The Framework

## 🏛️ Part 1: The Foundational Pillars

1. **The "Never Lose the Head" Rule**
   * **Concept:** Unlike arrays, a Linked List relies entirely on references. If you move `head` without a backup, the list is lost in memory.
   * **Rule:** Always use a secondary traversal pointer (`curr = head` or `dummy = head`). Never mutate `head` unless explicitly returning a new one.
2. **The Pointer Re-routing Rule (The 3-Step Dance)**
   * **Concept:** Singly linked lists point forward only. To reverse direction, you must systematically manage pointers.
   * **Rule:** Requires 3 variables: `prev` (behind), `curr` (current), and `next` (ahead).
3. **The Sentinel (Dummy Head) Pattern**
   * **Concept:** Eliminates edge-case bugs when the head changes, gets deleted, or starts empty.
   * **Rule:** Create a fake node (`ListNode dummy = new ListNode(0); dummy.next = head;`) and always return `dummy.next`.
4. **Fast & Slow Pointers (Tortoise & Hare)**
   * **Concept:** Used when list length is unknown.
   * **Rule:** `slow` moves 1 step, `fast` moves 2 steps to find midpoints or detect loops.
5. **NEW PILLAR: Multi-Pass Interweaving**
   * **Concept:** Creating clones without auxiliary space by weaving new nodes directly into the original list.
6. **NEW PILLAR: Doubly Linked Lists & Hashing (LLD)**
   * **Concept:** Combining a HashMap with bi-directional nodes to achieve $O(1)$ lookup and $O(1)$ structural modification.

---

## 💻 Part 2: The Essential Code Blocks

### Block 1: The Reversal Engine (`Prev`, `Curr`, `Next`)
```java(start)
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next; // 1. Save forward path
        curr.next = prev;             // 2. Reverse pointer backward
        prev = curr;                   // 3. Shift prev forward
        curr = nextTemp;               // 4. Shift curr forward
    }
    return prev; // 'prev' is the new head
}

```

### Block 2: The Tortoise & Hare (Middle Finder)

```java(start)
public ListNode findMiddle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow; // 'slow' lands precisely on the middle node
}

```

### Block 3: The Two-List Combiner (Sentinel Pattern)

```java(start)
public ListNode mergeLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(-1); // Sentinel protection
    ListNode tail = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) {
            tail.next = l1;
            l1 = l1.next;
        } else {
            tail.next = l2;
            l2 = l2.next;
        }
        tail = tail.next;
    }
    tail.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}

```

### Block 4: Cycle Detection (Floyd's Algorithm)

```java(start)
public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true; // Collision = Cycle exists
    }
    return false;
}

```

### NEW BLOCK 5: The Min-Heap Combiner

```java(start)
PriorityQueue<ListNode> minHeap = new PriorityQueue<>((list1, list2) -> list1.val - list2.val);
// Add all heads, then poll and append to dummy tail.

```

### remove a node from DDL

```java(start)
    public void remove(Node node){
        // node.prev <-------> node <-------> node.next
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

```

```text
        (node.prev.next = node.next)
        ┌────────────────────────────────────────────────────────┐
        │                                                        v
   +----------+           +-------------+           +----------+
   |          |    next   |             |    next   |          |
   |node.prev | ----------> | Target Node | ----------> |node.next |
   |          | <---------- | (  node   ) | <---------- |          |
   +----------+           +-------------+           +----------+
        ^                                                        |
        │                                                        |
        └────────────────────────────────────────────────────────┘
        (node.next.prev = node.prev)

```

### insert a node after HEAD into DDL

```java(start)
    public void insertAtHead(Node newNode){
        newNode.next = head.next;
        newNode.prev = head;

        head.next.prev = newNode; // FIRST STATEMENT
        head.next = newNode;
    }

```

```text
1. INITIAL STATE:
   +------------+                                    +---------------+
   |    head    | <---------------------------->     |   head.next   |
   |  Sentinel  |                                    |  (Old First)  |
   +------------+                                    +---------------+


2. EXECUTING:
   newNode.next = head.next; 
   newNode.prev = head;

   +------------+                                    +---------------+                     +---------------+
   |            |                                    |               | --------(1)---------> |               |
   |    head    |                                    |   newNode     |                       |   head.next   |
   |  Sentinel  | <-------------(2)----------------  |               |                       |               |
   +------------+                                    +---------------+                     +---------------+


3. EXECUTING:
   head.next.prev = newNode; // FIRST STATEMENT
   head.next = newNode;

   +------------+                   +---------------+                     +---------------+
   |            | ----(4)-------->  |               |                     |               |
   |    head    |                   |   newNode     |                     |   head.next   |
   |  Sentinel  |                   |               | <------(3)----------  |  (Old First)  |
   +------------+                   +---------------+                     +---------------+

```

### insert a node before TAIL into DDL

```java(start)
    public void insertAtTail(Node newNode){
        newNode.next = tail;
        newNode.prev = tail.prev;

        tail.prev.next = newNode; // FIRST STATEMENT
        head.prev = newNode;
    }

```

```text
1. INITIAL STATE:
   +---------------+                                 +------------+
   |   tail.prev   | <---------------------------->  |    tail    |
   |  (Old Last)   |                                 |  Sentinel  |
   +---------------+                                 +------------+


2. EXECUTING:
   newNode.next = tail;
   newNode.prev = tail.prev;

   +---------------+                 +---------------+                     +------------+
   |               |                 |               | --------(1)---------> |            |
   |   tail.prev   |                 |   newNode     |                       |    tail    |
   |               | <-------------(2)----           |               |       |  Sentinel  |
   +---------------+                 +---------------+                     +------------+


3. EXECUTING:
   tail.prev.next = newNode; // FIRST STATEMENT
   tail.prev = newNode;

   +---------------+                 +---------------+                     +------------+
   |               |                 |               | <-------(4)---------  |            |
   |   tail.prev   |                 |   newNode     |                       |    tail    |
   |  (Old Last)   | -----(3)------------>           |               |       |  Sentinel  |
   +---------------+                 +---------------+                     +------------+

```

---

## 🗺️ Part 3: Composite Workflows in NeetCode 250

* **Workflow 1: Find Mid $\rightarrow$ Reverse Half $\rightarrow$ Merge** (Reorder List, Palindrome Linked List)
* **Workflow 2: Fast Gap ahead of Slow** (Remove Nth Node From End)
* **NEW Workflow 3: Array Indices as Cycle Pointers** (Find the Duplicate Number)

---

---

```

```
