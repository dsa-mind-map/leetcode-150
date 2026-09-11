# Linked List Pattern: Master Reference Guide (Tier 1 & 2 Optimized for DSA & LLD)

Every Linked List problem relies on mastering how to manipulate pointers (`head`, `prev`, `curr`, `next`) without losing references, causing `NullPointerExceptions`, or breaking memory linkages. 

This guide strictly aligns 12 core LeetCode problems to **Foundational Pillars**, **Essential Code Blocks**, and **Composite Workflows**. 

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
```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next; // 1. Save forward path
        curr.next = prev;              // 2. Reverse pointer backward
        prev = curr;                   // 3. Shift prev forward
        curr = nextTemp;               // 4. Shift curr forward
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

### NEW BLOCK 5: The Min-Heap Combiner
```java
PriorityQueue<ListNode> minHeap = new PriorityQueue<>((a, b) -> a.val - b.val);
// Add all heads, then poll and append to dummy tail.
```

---

## 🗺️ Part 3: Composite Workflows in NeetCode 250

* **Workflow 1: Find Mid $\rightarrow$ Reverse Half $\rightarrow$ Merge** (Reorder List, Palindrome Linked List)
* **Workflow 2: Fast Gap ahead of Slow** (Remove Nth Node From End)
* **NEW Workflow 3: Array Indices as Cycle Pointers** (Find the Duplicate Number)

---
---

# 📚 The 12 Master Problems: Concept Alignment & Codebase

## 1. Linked List Cycle (LeetCode 141)
**Alignment:** Pillar 4 (Fast & Slow Pointers), Block 4 (Cycle Detection)
**Additional Learning:** Recognizing that pointer equality (`slow == fast`) denotes a cycle, not value equality.

> **Problem:** 
> Given the beginning of a linked list head, return true if there is a cycle in the linked list. Otherwise, return false.
> 
> There is a cycle in a linked list if at least one node in the list can be visited again by following the next pointer.
> 
> Internally, index determines the index of the beginning of the cycle, if it exists. The tail node of the list will set it's next pointer to the index-th node. If index = -1, then the tail node points to null and no cycle exists.
> 
> Note: index is not given to you as a parameter.
>
> Example 1:
> * Input: head = [1,2,1], index = -1
> * Output: false
> 
> Example 2:
> * Input: head = [1,2,3,4], index = 1
> * Output: true
> * Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
> 
> Example 3:
> * Input: head = [1,2], index = -1
> * Output: false
> 
> Constraints:
> * 0 <= Length of the list <= 1000.
> * -1000 <= Node.val <= 1000
> * index is -1 or a valid index in the linked list.
>
> **HINTS**
> HASHSET - same node came again
> SLOW & FAST pointers - both points to same node ( cycle exists)

**Common Pitfall:** Checking `fast != null` but forgetting to check `fast.next != null` before executing `fast.next.next`, leading to a `NullPointerException`. Comparing node values instead of node references.

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while(fast != null && fast.next != null){
        slow = slow.next; 
        fast = fast.next.next; 
        if(slow == fast) return true;
    }
    return false;
}
```

---

## 2. Remove Nth Node From End of List (LeetCode 19)
**Alignment:** Pillar 3 (Sentinel Pattern), Workflow 2 (Fast Gap ahead of Slow)
**Additional Learning:** Maintaining a fixed sliding window of $N$ nodes between two pointers allows you to find the end-relative node in a single pass.

> **Problem:** 
> Given the head of a linked list and an integer n, remove the nth node from the end of the list and return its head.
> 
> Example 1:
> * Input: head = [1,2,3,4], n = 2
> * Output: [1,2,4]
>
> Example 2:
> * Input: head = [5], n = 1
> * Output: []
>
> Example 3:
> * Input: head = [1,2], n = 2
> * Output: [2]
>
> Constraints:
> * The number of nodes in the list is sz.
> * 1 <= sz <= 30
> * 0 <= Node.val <= 100
> * 1 <= n <= sz

**Common Pitfall:** If the list has 5 nodes and you need to remove the 5th node from the end (the head), navigating without a Sentinel/Dummy node will cause a null pointer or lose the list entirely. Always start both pointers at the Dummy node.

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0, head);
    ListNode slow = dummy, fast = dummy;
    
    // Create the gap of n
    for (int i = 0; i <= n; i++) {
        fast = fast.next;
    }
    
    // Slide the window
    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    
    // Skip the target node
    slow.next = slow.next.next;
    return dummy.next; // Pillar 3: Return dummy.next
}
```

---

## 3. Find the Duplicate Number (LeetCode 287)
**Alignment:** Pillar 4 (Fast & Slow), Block 4 (Cycle Detection), Workflow 3 (Array as Cycle)
**Additional Learning:** Array values restricted from $1$ to $N$ can be treated as pointer references `nums[i] -> nums[nums[i]]`.

> **Problem:** 
> You are given an array of integers nums containing n + 1 integers. Each integer in nums is in the range [1, n] inclusive.
> 
> There is exactly one repeated integer in nums, and every other integer appears at most once.
> Return the repeated integer.
> 
> Example 1:
> * Input: nums = [1,2,3,2,2]
> * Output: 2
> 
> Example 2:
> * Input: nums = [1,2,3,4,4]
> * Output: 4
> 
> **Follow-up: Can you solve the problem without modifying the array nums and using O(1) extra space?**
> 
> Constraints:
> * 1 <= n <= 10,000
> * nums.length == n + 1
> * 1 <= nums[i] <= n

**Common Pitfall:** Attempting to sort or use a HashSet violates the problem constraints. Initializing both pointers to 0 is required since index 0 is guaranteed to be outside the cycle.

```java
public int findDuplicate(int[] nums) {
    int slow = nums[0];
    int fast = nums[0];
    
    // Block 4: Find intersection
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);
    
    // Find cycle entrance
    slow = nums[0];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}
```

---

## 4. Reverse Linked List (LeetCode 206)
**Alignment:** Pillar 2 (Pointer Re-routing), Block 1 (The Reversal Engine)

> **Problem:** 
> Given the beginning of a singly linked list head, reverse the list, and return the new beginning of the list.
> 
> Example 1:
> * Input: head = [0,1,2,3]
> * Output: [3,2,1,0]
>
> Example 2:
> * Input: head = []
> * Output: []
>
> Constraints:
> * 0 <= The length of the list <= 1000.
> * -1000 <= Node.val <= 1000

**Common Pitfall:** Forgetting Step 1 of the 3-step dance (`ListNode nextTemp = curr.next`). If you change `curr.next` to `prev` before saving the forward path, you instantly sever the rest of the list.

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

---

## 5. Reverse Linked List II (LeetCode 92)
**Alignment:** Pillar 2 (Pointer Re-routing), Pillar 3 (Sentinel Pattern), Block 1 (Reversal Engine sub-range)
**Additional Learning:** How to surgically extract a sub-list, reverse it, and patch it back in by retaining pointers to the nodes right before and after the cut.

> **Problem:** 
> You are given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right (1-indexed), and return the reversed list.
> 
> Example 1:
> * Input: head = [1,2,3,4,5], left = 1, right = 3
> * Output: [3,2,1,4,5]
>
> Example 2:
> * Input: head = [1,1], left = 1, right = 1
> * Output: [1,1]
>
> Constraints:
> * The number of nodes in the list is n.
> * 1 <= n <= 500.
> * -500 <= Node.val <= 500
> * 1 <= left <= right <= n
> 
> **Follow up: Could you do it in one pass?**

**Common Pitfall:** Re-wiring the edges. After the reversal engine finishes, `prev` is the new head of the sub-list, but the original `curr` pointer (which was at `left`) is now the tail and must point to the node after `right`.

```java
public ListNode reverseBetween(ListNode head, int left, int right) {
    if (head == null || left == right) return head;
    
    ListNode dummy = new ListNode(0, head);
    ListNode prev = dummy;
    
    // Reach the node right before the sub-list
    for (int i = 0; i < left - 1; i++) {
        prev = prev.next;
    }
    
    // Start sub-range reversal (Modified Block 1)
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

---

## 6. Reverse Nodes in k-Group (LeetCode 25)
**Alignment:** Pillar 2 (Pointer Re-routing), Pillar 3 (Sentinel), Block 1 (Reversal Engine)
**Additional Learning:** Managing state across multiple iterations. Counting nodes before acting is crucial to leave remaining nodes (length < $k$) untouched.

> **Problem:** 
> You are given the head of a singly linked list head and a positive integer k.
> 
> You must reverse the first k nodes in the linked list, and then reverse the next k nodes, and so on. If there are fewer than k nodes left, leave the nodes as they are.
> 
> Return the modified list after reversing the nodes in each group of k.
> 
> You are only allowed to modify the nodes' next pointers, not the values of the nodes.
> 
> Example 1:
> * Input: head = [1,2,3,4,5,6], k = 3
> * Output: [3,2,1,6,5,4]
>
> Example 2:
> * Input: head = [1,2,3,4,5], k = 3
> * Output: [3,2,1,4,5]
>
> Constraints:
> * The length of the linked list is n.
> * 1 <= k <= n <= 5000
> * 0 <= Node.val <= 100

**Common Pitfall:** Forgetting to update the `pre` pointer to the end of the newly reversed group before starting the next $k$-group reversal.

```java
public ListNode reverseKGroup(ListNode head, int k) {
    if (head == null || k == 1) return head;
    
    ListNode dummy = new ListNode(0, head);
    ListNode curr = dummy, nxt = dummy, pre = dummy;
    int count = 0;
    
    // Count total nodes
    while (curr.next != null) {
        curr = curr.next;
        count++;
    }
    
    // Block 1 executed conditionally
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

---

## 7. Merge Two Sorted Lists (LeetCode 21)
**Alignment:** Pillar 3 (Sentinel Pattern), Block 3 (The Two-List Combiner)

> **Problem:** 
> You are given the heads of two sorted linked lists list1 and list2.
> 
> Merge the two lists into one sorted linked list and return the head of the new sorted linked list.
> 
> The new list should be made up of nodes from list1 and list2.
> 
> Example 1:
> * Input: list1 = [1,2,4], list2 = [1,3,5]
> * Output: [1,1,2,3,4,5]
>
> Example 2:
> * Input: list1 = [], list2 = [1,2]
> * Output: [1,2]
>
> Example 3:
> * Input: list1 = [], list2 = []
> * Output: []
>
> Constraints:
> * 0 <= The length of the each list <= 100.
> * -100 <= Node.val <= 100

**Common Pitfall:** Running a loop `while (l1 != null || l2 != null)` requires messy internal null checks. The cleaner approach is `while (l1 != null && l2 != null)` and then appending the remainder directly at the end.

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(0); // Pillar 3
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
    
    // Append remainders
    tail.next = (list1 != null) ? list1 : list2;
    return dummy.next;
}
```

---

## 8. Add Two Numbers (LeetCode 2)
**Alignment:** Pillar 3 (Sentinel Pattern), Block 3 (Combiner with Math Logic)
**Additional Learning:** Simulating elementary addition requires passing a `carry` variable across loop iterations.

> **Problem:** 
> You are given two non-empty linked lists, l1 and l2, where each represents a non-negative integer.
> 
> The digits are stored in reverse order, e.g. the number 321 is represented as 1 -> 2 -> 3 -> in the linked list.
> 
> Each of the nodes contains a single digit. You may assume the two numbers do not contain any leading zero, except the number 0 itself.
> 
> Return the sum of the two numbers as a linked list.
> 
> Example 1:
> * Input: l1 = [1,2,3], l2 = [4,5,6]
> * Output: [5,7,9]
> * Explanation: 321 + 654 = 975.
> 
> Example 2:
> * Input: l1 = [9], l2 = [9]
> * Output: [8,1]
> 
> Constraints:
> * 1 <= l1.length, l2.length <= 100.
> * 0 <= Node.val <= 9

**Common Pitfall:** Forgetting to check if `carry != 0` at the very end. If 99 + 1 = 100, the final '1' will be dropped if the loop terminates early.

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    int carry = 0;
    
    // Notice carry != 0 in the condition
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

---

## 9. Reorder List (LeetCode 143)
**Alignment:** Workflow 1 (Find Mid $\rightarrow$ Reverse Half $\rightarrow$ Merge)
**Additional Learning:** Combining Blocks 1, 2, and 3 cleanly. You must physically split the list into two distinct lists before merging.

> **Problem:** 
> You are given the head of a singly linked-list.
> 
> The positions of a linked list of length = 7 for example, can intially be represented as:
> [0, 1, 2, 3, 4, 5, 6]
> 
> Reorder the nodes of the linked list to be in the following order:
> [0, 6, 1, 5, 2, 4, 3]
> 
> In the general case, label the nodes by their original zero-based positions from 0 to n - 1. After reordering, those original positions appear in this order:
> [0, n-1, 1, n-2, 2, n-3, ...]
> 
> These numbers represent node positions, not the values stored in the nodes.
> 
> You may not modify the values in the list's nodes, but instead you must reorder the nodes themselves.
> 
> Example 1:
> * Input: head = [2,4,6,8]
> * Output: [2,8,4,6]
> 
> Example 2:
> * Input: head = [2,4,6,8,10]
> * Output: [2,10,4,8,6]
> 
> Constraints:
> * 1 <= Length of the list <= 1000.
> * 1 <= Node.val <= 1000

**Common Pitfall:** Not setting `slow.next = null` after finding the middle. If you don't sever the connection, the lists will form a cycle when you attempt to merge them.

```java
public void reorderList(ListNode head) {
    if (head == null || head.next == null) return;
    
    // Block 2: Middle Finder
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Block 1: Reversal Engine (on second half)
    ListNode prev = null, curr = slow.next;
    slow.next = null; // CRITICAL: Cut off first half
    while (curr != null) {
        ListNode nxt = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nxt;
    }
    
    // Block 3: Two-Pointer Merging (Alternating)
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

---

## 10. Copy List with Random Pointer (LeetCode 138)
**Alignment:** Pillar 5 (Multi-Pass Interweaving)
**Additional Learning:** You can avoid a HashMap ($O(N)$ space) for deep copies by cloning nodes and placing them immediately after the original nodes.

> **Problem:** 
> You are given the head of a linked list of length n. Unlike a singly linked list, each node contains an additional pointer random, which may point to any node in the list, or null.
> 
> Create a deep copy of the list.
> 
> The deep copy should consist of exactly n new nodes, each including:
> The original value val of the copied node
> A next pointer to the new node corresponding to the next pointer of the original node
> A random pointer to the new node corresponding to the random pointer of the original node
> Note: None of the pointers in the new list should point to nodes in the original list.
> 
> Return the head of the copied linked list.
> 
> In the examples, the linked list is represented as a list of n nodes. Each node is represented as a pair of [val, random_index] where random_index is the index of the node (0-indexed) that the random pointer points to, or null if it does not point to any node.
> 
> Example 1:
> * Input: head = [[3,null],[7,3],[4,0],[5,1]]
> * Output: [[3,null],[7,3],[4,0],[5,1]]
> 
> Example 2:
> * Input: head = [[1,null],[2,2],[3,2]]
> * Output: [[1,null],[2,2],[3,2]]
> 
> Constraints:
> * 0 <= n <= 100
> * -100 <= Node.val <= 100
> * Node values are not guaranteed to be unique.
> * random is null or is pointing to some node in the linked list.

**Common Pitfall:** Attempting to set `random` pointers in the same loop where you create the cloned nodes. The node that `random` points to might not have been cloned yet. It strictly requires 3 separate passes.

```java
public Node copyRandomList(Node head) {
    if (head == null) return null;
    
    // Pass 1: Interweave cloned nodes
    Node curr = head;
    while (curr != null) {
        Node clone = new Node(curr.val);
        clone.next = curr.next;
        curr.next = clone;
        curr = clone.next;
    }
    
    // Pass 2: Assign random pointers
    curr = head;
    while (curr != null) {
        if (curr.random != null) {
            // Cloned random is right next to original random
            curr.next.random = curr.random.next; 
        }
        curr = curr.next.next;
    }
    
    // Pass 3: Extract the cloned list
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

---

## 11. Merge k Sorted Lists (LeetCode 23)
**Alignment:** Block 5 (Min-Heap Combiner)
**Additional Learning:** Utilizing Priority Queues (Min-Heaps) to continuously extract the smallest current node across $k$ different lists.

> **Problem:** 
> You are given an array of k linked lists lists, where each list is sorted in ascending order.
> 
> Return the sorted linked list that is the result of merging all of the individual linked lists.
> 
> Example 1:
> * Input: lists = [[1,2,4],[1,3,5],[3,6]]
> * Output: [1,1,2,3,3,4,5,6]
>
> Example 2:
> * Input: lists = []
> * Output: []
>
> Example 3:
> * Input: lists = [[]]
> * Output: []
>
> Constraints:
> * 0 <= lists.length <= 10000
> * 0 <= lists[i].length <= 500
> * -10000 <= lists[i][j] <= 10000
> * lists[i] is sorted in ascending order.
> * The sum of lists[i].length will not exceed 10000.

**Common Pitfall:** Passing a completely empty list (`[]`) or an array containing null heads (`[[]]`). You must ensure `node != null` before offering to the Min-Heap.

```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) return null;
    
    // Block 5: Min-Heap Setup
    PriorityQueue<ListNode> minHeap = new PriorityQueue<>((a, b) -> a.val - b.val);
    
    for (ListNode node : lists) {
        if (node != null) {
            minHeap.offer(node);
        }
    }
    
    ListNode dummy = new ListNode(0); // Pillar 3
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

---

## 12. LRU Cache (LeetCode 146)
**Alignment:** Pillar 6 (Doubly Linked Lists & Hashing)
**Additional Learning:** Low-Level Design (LLD). A HashMap provides $O(1)$ key lookups, while a custom Doubly Linked List provides $O(1)$ positional updates (moving a node to the front, removing from the back).

> **Problem:** 
> Implement the Least Recently Used (LRU) cache class LRUCache. The class should support the following operations
> 
> LRUCache(int capacity) Initialize the LRU cache of size capacity.
> int get(int key) Return the value corresponding to the key if the key exists, otherwise return -1.
> void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the introduction of the new pair causes the cache to exceed its capacity, remove the least recently used key.
> A key is considered used if a get or a put operation is called on it.
> 
> Ensure that get and put each run in O(1) average time complexity.
> 
> Example 1:
> * Input:
> * ["LRUCache", [2], "put", [1, 10],  "get", [1], "put", [2, 20], "put", [3, 30], "get", [2], "get", [1]]
> * 
> * Output:
> * [null, null, 10, null, null, 20, -1]
> 
> Explanation:
> * LRUCache lRUCache = new LRUCache(2);
> * lRUCache.put(1, 10);  // cache: {1=10}
> * lRUCache.get(1);      // return 10
> * lRUCache.put(2, 20);  // cache: {1=10, 2=20}
> * lRUCache.put(3, 30);  // cache: {2=20, 3=30}, key=1 was evicted
> * lRUCache.get(2);      // returns 20 
> * lRUCache.get(1);      // return -1 (not found)
> 
> Constraints:
> * 1 <= capacity <= 3000
> * 0 <= key <= 10^4
> * 0 <= value <= 10^5
> * At most 2 * 10^5 calls will be made to get and put.

**Common Pitfall:** Managing the bi-directional wiring poorly. Breaking the `remove()` and `insertToFront()` operations into isolated helper methods prevents spaghetti code and ensures `prev` and `next` remain perfectly synced.

```java
class LRUCache {
    class Node {
        int key, val;
        Node prev, next;
        Node(int k, int v) {
            key = k; val = v;
        }
    }
    
    private final int capacity;
    private final Map<Integer, Node> map;
    private final Node head, tail; // Pillar 3: Sentinels for DLL

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
