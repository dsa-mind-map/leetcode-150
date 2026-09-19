# 📚 Linked List Master Problems: Problem Directory

Welcome to the problem directory! This guide serves as a central hub to navigate through the 12 core LeetCode linked list problems, categorized by their structural pillars, common pitfalls, and optimized Java implementations.

---

## 🔗 Quick Navigation Links

### 🚀 Phase 1: Fundamentals & Pointers
1. **[Linked List Cycle (LeetCode 141)](problems/lc_141_linked_list_cycle.md)**
   * **Pillar:** Fast & Slow Pointers
   * **Key Focus:** Detecting reference equality without value collision false positives.
2. **[Remove Nth Node From End of List (LeetCode 19)](problems/lc_019_remove_nth_node_from_end.md)**
   * **Pillar:** Sentinel Pattern & Fast Gap
   * **Key Focus:** Sliding window distance calculation for single-pass deletion.
3. **[Find the Duplicate Number (LeetCode 287)](problems/lc_287_find_the_duplicate_number.md)**
   * **Pillar:** Array as Cycle Pointer
   * **Key Focus:** Pigeonhole principle mapped to Floyd's Cycle Detection algorithm.
4. **[Reverse Linked List (LeetCode 206)](problems/lc_206_reverse_linked_list.md)**
   * **Pillar:** Pointer Re-routing (3-Step Dance)
   * **Key Focus:** Saving forward references before mutating pointers.

### 🔀 Phase 2: Sub-List Manipulation & Re-routing
5. **[Reverse Linked List II (LeetCode 92)](problems/lc_092_reverse_linked_list_ii.md)**
   * **Pillar:** Surgical Sub-List Extraction
   * **Key Focus:** Patching boundaries before and after range reversal.
6. **[Reverse Nodes in k-Group (LeetCode 25)](problems/lc_025_reverse_nodes_in_k_group.md)**
   * **Pillar:** State Management & Count Checks
   * **Key Focus:** Leaving remaining nodes untouched when length $< k$.
7. **[Merge Two Sorted Lists (LeetCode 21)](problems/lc_021_merge_two_sorted_lists.md)**
   * **Pillar:** Sentinel Combiner
   * **Key Focus:** Appending remainder lists efficiently.

### ➕ Phase 3: Composite Workflows & Advanced LLD
8. **[Add Two Numbers (LeetCode 2)](problems/lc_002_add_two_numbers.md)**
   * **Pillar:** Combiner with Math Logic
   * **Key Focus:** Propagating carry bounds past individual list terminations.
9. **[Reorder List (LeetCode 143)](problems/lc_143_reorder_list.md)**
   * **Pillar:** Find Mid $\rightarrow$ Reverse $\rightarrow$ Merge
   * **Key Focus:** Severing the first half list reference to prevent cycles.
10. **[Copy List with Random Pointer (LeetCode 138)](problems/lc_138_copy_list_with_random_pointer.md)**
    * **Pillar:** Multi-Pass Interweaving
    * **Key Focus:** Achieving $O(1)$ space deep copies via neighbor mapping.
11. **[Merge k Sorted Lists (LeetCode 23)](problems/lc_023_merge_k_sorted_lists.md)**
    * **Pillar:** Min-Heap Combiner
    * **Key Focus:** Handling empty head arrays and preventing time-limit exceptions.
12. **[LRU Cache (LeetCode 146)](problems/lc_146_lru_cache.md)**
    * **Pillar:** Doubly Linked Lists & Hashing
    * **Key Focus:** Low-level design invariants for $O(1)$ lookups and updates.

---
*Tip: Select any of the links above to view the detailed problem breakdown, code implementation, and common pitfalls.*

# Linked List Pattern: Master Reference Guide (Tier 1 & 2 Optimized for DSA & LLD)

> **CRITICAL ARCHITECTURAL WARNING:** Every Linked List flaw originates from mutating pointers without verifying null bounds or failing to save forward references before breaking them. Master the foundational rules below to eliminate $O(N)$ runtime crashes and memory leaks.

---

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

5. **Multi-Pass Interweaving**
   * **Concept:** Creating clones without auxiliary space by weaving new nodes directly into the original list.

6. **Doubly Linked Lists & Hashing (LLD)**
   * **Concept:** Combining a HashMap with bi-directional nodes to achieve $O(1)$ lookup and $O(1)$ structural modification.

---

## 💻 Part 2: The Essential Code Blocks

### Block 1: The Reversal Engine (`Prev`, `Curr`, `Next`)
```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next; // 1. Save forward path
        curr.next = prev;             // 2. Reverse pointer backward
        prev = curr;                  // 3. Shift prev forward
        curr = nextTemp;              // 4. Shift curr forward
    }
    return prev; // 'prev' is the new head
}
```

### Block 2: The Tortoise & Hare (Middle Finder)
```java
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
```java
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
```java
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

### Block 5: The Min-Heap Combiner
```java
PriorityQueue<ListNode> minHeap = new PriorityQueue<>((list1, list2) -> list1.val - list2.val);
// Add all heads, then poll and append to dummy tail.
```

---

## ⇄ Part 3: Doubly Linked List (DDL) Operations

### 1. Remove a Node from DDL
```java
public void remove(Node node){
    // CRITICAL: Always handle null checks on boundaries (head/tail sentinels) to avoid NullPointerExceptions.
    node.prev.next = node.next;
    node.next.prev = node.prev;
}
```
```text
        (node.prev.next = node.next)
        ┌────────────────────────────────────────────────────────┐
        │                                                        v
   +----------+             +-------------+             +----------+
   |          |    next     |             |    next     |          |
   |node.prev | ----------> | Target Node | ----------> |node.next |
   |          | <---------- | (   node    ) | <---------- |          |
   +----------+             +-------------+             +----------+
        ^                                                     |
        │                                                     |
        └─────────────────────────────────────────────────────┘
        (node.next.prev = node.prev)
```

### 2. Insert a Node After HEAD into DDL
```java
public void insertAtHead(Node newNode){
    newNode.next = head.next;
    newNode.prev = head;

    head.next.prev = newNode; // FIRST STATEMENT: secure reverse link first!
    head.next = newNode;
}
```
```text
1. INITIAL STATE:
   +------------+                                        +---------------+
   |    head    | <---------------------------->        |   head.next   |
   |  Sentinel  |                                        |  (Old First)  |
   +------------+                                        +---------------+

2. EXECUTING:
   newNode.next = head.next; 
   newNode.prev = head;

   +------------+                                        +---------------+                 +---------------+
   |            |                                        |               | ------(1)-----> |               |
   |    head    |                                        |    newNode    |                 |   head.next   |
   |  Sentinel  | <-------------(2)----------------      |               |                 |               |
   +------------+                                        +---------------+                 +---------------+

3. EXECUTING:
   head.next.prev = newNode; // FIRST STATEMENT
   head.next = newNode;

   +------------+                   +---------------+                 +---------------+
   |            | ----(4)---------> |               |                 |               |
   |    head    |                   |    newNode    |                 |   head.next   |
   |  Sentinel  |                   |               | <------(3)----------  |  (Old First)  |
   +------------+                   +---------------+                 +---------------+
```

### 3. Insert a Node Before TAIL into DDL
```java
public void insertAtTail(Node newNode){
    newNode.next = tail;
    newNode.prev = tail.prev;

    tail.prev.next = newNode; // FIRST STATEMENT: anchor forward pointer safely
    tail.prev = newNode;
}
```
```text
1. INITIAL STATE:
   +---------------+                                     +------------+
   |   tail.prev   | <---------------------------->      |    tail    |
   |  (Old Last)   |                                     |  Sentinel  |
   +---------------+                                     +------------+

2. EXECUTING:
   newNode.next = tail;
   newNode.prev = tail.prev;

   +---------------+                     +---------------+                 +------------+
   |               |                     |               | ------(1)-----> |            |
   |   tail.prev   |                     |    newNode    |                 |    tail    |
   |               | <-------------(2)---- |               |                 |  Sentinel  |
   +---------------+                     +---------------+                 +------------+

3. EXECUTING:
   tail.prev.next = newNode; // FIRST STATEMENT
   tail.prev = newNode;

   +---------------+                     +---------------+                 +------------+
   |               |                     |               | <-------(4)---------  |            |
   |   tail.prev   |                     |    newNode    |                 |    tail    |
   |  (Old Last)   | -----(3)------------> |               |                 |  Sentinel  |
   +---------------+                     +---------------+                 +------------+
```

---

## 🗺️ Part 4: Composite Workflows in NeetCode 250

* **Workflow 1: Find Mid $\rightarrow$ Reverse Half $\rightarrow$ Merge** (Reorder List, Palindrome Linked List)
* **Workflow 2: Fast Gap ahead of Slow** (Remove Nth Node From End)
* **Workflow 3: Array Indices as Cycle Pointers** (Find the Duplicate Number)
