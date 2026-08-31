# Linked List Pattern: Master Reference Guide (Tier 1 & 2 Optimized for DSA & LLD)

Every Linked List problem relies on mastering how to manipulate pointers (`head`, `prev`, `curr`, `next`) without losing references, causing `NullPointerExceptions`, or breaking memory linkages. 

---

## The Master Comparison Table (Tier 1 / Tier 2 Core)

| Sub-Pattern | Problem | Core Technique / What triggers pointer changes? |
| :--- | :--- | :--- |
| **1. Fast & Slow Pointers** | Linked List Cycle (141) | Fast moves 2 steps, slow moves 1 step; they meet if there is a cycle. |
| | Remove Nth Node From End (19) | Fast pointer is offset by $N$ steps before moving both together. |
| | Find the Duplicate Number (287) | Treats array values as pointers to detect cycle intersection. |
| **2. In-Place Reversal** | Reverse Linked List (206) | Iteratively re-wire `curr.next` to point to `prev`. |
| | Reverse Linked List II (92) | Reverses a precise sub-range from position `left` to `right`. |
| | Reverse Nodes in k-Group (25) | Reverses sub-segments of size $k$ conditionally in blocks. |
| **3. Two Pointers / Merging** | Merge Two Sorted Lists (21) | Compare values of both heads and attach the smaller one to a dummy tail. |
| | Add Two Numbers (2) | Simulates elementary math addition, managing carry over digits. |
| **4. Advanced / Composite** | Reorder List (143) | Combines middle finding, second-half reversal, and alternating merge. |
| | Copy List with Random Pointer (138) | Interweaves cloned nodes to resolve random pointers in $O(1)$ space. |
| | Merge K Sorted Lists (23) | Uses a Min-Heap (Priority Queue) or Divide & Conquer to merge multiple lists. |
| **5. Low-Level Design (LLD)** | LRU Cache (146) | Combines a HashMap with a custom Doubly Linked List for $O(1)$ operations. |

---

## Sub-Pattern 1: Fast & Slow Pointers
**Core Goal:** Finding cycles, middle elements, or maintaining a fixed distance offset between nodes.

### 1. Linked List Cycle (LeetCode 141)
> **Question:** Given `head`, the head of a linked list, determine if the linked list has a cycle in it. There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(1)$

### 2. Remove Nth Node From End of List (LeetCode 19)
> **Question:** Given the `head` of a linked list, remove the $n$-th node from the end of the list and return its head.

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0, head);
    ListNode slow = dummy, fast = dummy;
    
    // Move fast n + 1 steps ahead to maintain a gap of n nodes
    for (int i = 0; i <= n; i++) {
        fast = fast.next;
    }
    
    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    
    slow.next = slow.next.next;
    return dummy.next;
}
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(1)$

### 3. Find the Duplicate Number (LeetCode 287)
> **Question:** Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive. There is only one repeated number in `nums`, return this repeated number. You must solve the problem without modifying the array `nums` and uses only constant extra space.

```java
public int findDuplicate(int[] nums) {
    // Treat array indices and values as a linked list cycle problem (Floyd's Tortoise and Hare)
    int slow = nums[0];
    int fast = nums[0];
    
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);
    
    // Find the entrance to the cycle
    slow = nums[0];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(1)$

---

## Sub-Pattern 2: In-Place Reversal
**Core Goal:** Flipping pointer directions iteratively or within restricted ranges.

### 4. Reverse Linked List (LeetCode 206)
> **Question:** Given the `head` of a singly linked list, reverse the list, and return the reversed list.

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(1)$

### 5. Reverse Linked List II (LeetCode 92)
> **Question:** Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return the reversed list.

```java
public ListNode reverseBetween(ListNode head, int left, int right) {
    if (head == null || left == right) return head;
    
    ListNode dummy = new ListNode(0, head);
    ListNode prev = dummy;
    
    for (int i = 0; i < left - 1; i++) {
        prev = prev.next;
    }
    
    ListNode curr = prev.next;
    for (int i = 0; i < right - left; i++) {
        ListNode nextNode = curr.next;
        curr.next = nextNode.next;
        nextNode.next = prev.next;
        prev.next = nextNode;
    }
    
    return dummy.next;
}
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(1)$

### 6. Reverse Nodes in k-Group (LeetCode 25)
> **Question:** Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return the modified list. `k` is a positive integer and is less than or equal to the length of the linked list.

```java
public ListNode reverseKGroup(ListNode head, int k) {
    if (head == null || k == 1) return head;
    
    ListNode dummy = new ListNode(0, head);
    ListNode curr = dummy, nxt = dummy, pre = dummy;
    int count = 0;
    
    while (curr.next != null) {
        curr = curr.next;
        count++;
    }
    
    while (count >= k) {
        curr = pre.next;
        nxt = curr.next;
        for (int i = 1; i < k; i++) {
            curr.next = nxt.next;
            nxt.next = pre.next;
            pre.next = nxt;
            nxt = curr.next;
        }
        pre = curr;
        count -= k;
    }
    
    return dummy.next;
}
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(1)$

---

## Sub-Pattern 3: Two Pointers & Merging
**Core Goal:** Aligning multiple lists or numerical streams to build combined structures.

### 7. Merge Two Sorted Lists (LeetCode 21)
> **Question:** You are given the heads of two sorted linked lists `list1` and `list2`. Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    
    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            tail.next = list1;
            list1 = list1.next;
        } else {
            tail.next = list2;
            list2 = list2.next;
        }
        tail = tail.next;
    }
    
    tail.next = (list1 != null) ? list1 : list2;
    return dummy.next;
}
```
* **Time Complexity:** $O(N + M)$
* **Space Complexity:** $O(1)$

### 8. Add Two Numbers (LeetCode 2)
> **Question:** You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    int carry = 0;
    
    while (l1 != null || l2 != null || carry != 0) {
        int sum = carry;
        if (l1 != null) {
            sum += l1.val;
            l1 = l1.next;
        }
        if (l2 != null) {
            sum += l2.val;
            l2 = l2.next;
        }
        
        carry = sum / 10;
        tail.next = new ListNode(sum % 10);
        tail = tail.next;
    }
    
    return dummy.next;
}
```
* **Time Complexity:** $O(\max(N, M))$
* **Space Complexity:** $O(\max(N, M))$ for the new result list.

---

## Sub-Pattern 4: Advanced & Composite Manipulation
**Core Goal:** Multi-phase traversals, deep copies, and priority queue scheduling.

### 9. Reorder List (LeetCode 143)
> **Question:** You are given the head of a singly linked-list. The list can be represented as: $L_0 ightarrow L_1 ightarrow \dots ightarrow L_{n-1} ightarrow L_n$. Reorder the list to be: $L_0 ightarrow L_n ightarrow L_1 ightarrow L_{n-1} ightarrow L_2 ightarrow L_{n-2} ightarrow \dots$

```java
public void reorderList(ListNode head) {
    if (head == null || head.next == null) return;
    
    // Step 1: Find the middle using Fast & Slow pointers
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Step 2: Reverse the second half
    ListNode prev = null, curr = slow.next;
    slow.next = null; // Cut off first half
    while (curr != null) {
        ListNode nxt = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nxt;
    }
    
    // Step 3: Merge/Zip the two halves together
    ListNode first = head, second = prev;
    while (second != null) {
        ListNode t1 = first.next, t2 = second.next;
        first.next = second;
        second.next = t1;
        first = t1;
        second = t2;
    }
}
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(1)$

### 10. Copy List with Random Pointer (LeetCode 138)
> **Question:** A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`. Construct a deep copy of the list.

```java
public Node copyRandomList(Node head) {
    if (head == null) return null;
    
    // Step 1: Interweave cloned nodes right next to original nodes
    Node curr = head;
    while (curr != null) {
        Node clone = new Node(curr.val);
        clone.next = curr.next;
        curr.next = clone;
        curr = clone.next;
    }
    
    // Step 2: Assign random pointers for cloned nodes
    curr = head;
    while (curr != null) {
        if (curr.random != null) {
            curr.next.random = curr.random.next;
        }
        curr = curr.next.next;
    }
    
    // Step 3: Restore original list and extract the cloned list
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
```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(1)$ (No auxiliary Hash Map required due to interweaving trick).

### 11. Merge k Sorted Lists (LeetCode 23)
> **Question:** You are given an array of `k` linked-lists lists, each linked-list is sorted in ascending order. Merge all the linked-lists into one sorted linked-list and return it.

```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) return null;
    
    PriorityQueue<ListNode> minHeap = new PriorityQueue<>((a, b) -> a.val - b.val);
    
    for (ListNode node : lists) {
        if (node != null) {
            minHeap.offer(node);
        }
    }
    
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    
    while (!minHeap.isEmpty()) {
        ListNode smallest = minHeap.poll();
        tail.next = smallest;
        tail = tail.next;
        
        if (smallest.next != null) {
            minHeap.offer(smallest.next);
        }
    }
    
    return dummy.next;
}
```
* **Time Complexity:** $O(N \log K)$ where $N$ is total nodes across all lists and $K$ is the number of lists.
* **Space Complexity:** $O(K)$ for the Priority Queue.

---

## Sub-Pattern 5: Low-Level Design (LLD / Machine Coding)
**Core Goal:** Building high-performance cache frameworks with explicit time complexity guarantees.

### 12. LRU Cache (LeetCode 146)
> **Question:** Design a data structure that follows the constraints of a Least Recently Used (LRU) cache. Implement the `LRUCache` class with `get` and `put` operations guaranteed to run in $O(1)$ average time complexity.

```java
class LRUCache {
    class Node {
        int key, val;
        Node prev, next;
        Node(int k, int v) {
            key = k;
            val = v;
        }
    }
    
    private final int capacity;
    private final Map<Integer, Node> map;
    private final Node head, tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new HashMap<>();
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        if (!map.containsKey(key)) return -1;
        Node node = map.get(key);
        remove(node);
        insertToFront(node);
        return node.val;
    }
    
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.val = value;
            remove(node);
            insertToFront(node);
        } else {
            if (map.size() == capacity) {
                // Evict LRU node (node right before tail)
                Node lru = tail.prev;
                remove(lru);
                map.remove(lru.key);
            }
            Node newNode = new Node(key, value);
            map.put(key, newNode);
            insertToFront(newNode);
        }
    }
    
    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    
    private void insertToFront(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
}
```
* **Time Complexity:** $O(1)$ for both `get` and `put`.
* **Space Complexity:** $O(C)$ where $C$ is the cache capacity.
