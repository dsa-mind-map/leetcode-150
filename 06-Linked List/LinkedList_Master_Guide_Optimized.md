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
> **Question:**
> Given the beginning of a linked list head, return true if there is a cycle in the linked list. Otherwise, return false.
> 
> There is a cycle in a linked list if at least one node in the list can be visited again by following the next pointer.
> 
> Internally, index determines the index of the beginning of the cycle, if it exists. The tail node of the list will set it's next pointer to the index-th node. If index = -1, then the tail node points to null and no cycle exists.
> 
> Note: index is not given to you as a parameter.
> 
> Example 1:
> 
> 
> 
> * Input: head = [1,2,3,4], index = 1
> * 
> * Output: true
> * Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
> 
> Example 2:
> 
> 
> 
> * Input: head = [1,2], index = -1
> * 
> * Output: false
> Constraints:
> 
> * 0 <= Length of the list <= 1000.
> * -1000 <= Node.val <= 1000
> * index is -1 or a valid index in the linked list.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

class Solution {
    public boolean hasCycle(ListNode head) {

        // starting at the same position(head)
        // moving in the same direction, fast pointer twice as fast as slow pointer.
        // once reaches at the same position (slow == fast) -> cycle exists
        // if not then no cycle.

        ListNode slow = head;
        ListNode fast = head;

        while(fast != null && fast.next != null){ 
            // first & second element exists - first iteration and subsequent iteratrions.
            slow = slow.next; // second element
            fast = fast.next.next; // next of second element. If second element does not exist - then null pointer
            if(slow == fast) return true;

        }

        return false;
    }
}

```
* **Time Complexity:** $O(N)$
* **Space Complexity:** $O(1)$

### 2. Remove Nth Node From End of List (LeetCode 19)
> **Question:**
> Given the head of a linked list and an integer n, remove the nth node from the end of the list and return its head.
> 
> Example 1:
> 
> * Input: head = [1,2,3,4], n = 2
> * 
> * Output: [1,2,4]
>
> Example 2:
> 
> * Input: head = [5], n = 1
> * 
> * Output: []
>
> Example 3:
> 
> * Input: head = [1,2], n = 2
> * 
> * Output: [2]
>
> Constraints:
> 
> * The number of nodes in the list is sz.
> * 1 <= sz <= 30
> * 0 <= Node.val <= 100
> * 1 <= n <= sz

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
> **Question:**
> You are given an array of integers nums containing n + 1 integers. Each integer in nums is in the range [1, n] inclusive.
> 
> There is exactly one repeated integer in nums, and every other integer appears at most once.
> 
> Return the repeated integer.
> 
> Example 1:
> 
> * Input: nums = [1,2,3,2,2]
> * 
> * Output: 2
> 
> Example 2:
> 
> * Input: nums = [1,2,3,4,4]
> * 
> * Output: 4
> 
> **Follow-up: Can you solve the problem without modifying the array nums and using O(1) extra space?
> 
> Constraints:
> 
> * 1 <= n <= 10,000
> * nums.length == n + 1
> * 1 <= nums[i] <= n

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
> **Question:**
> Given the beginning of a singly linked list head, reverse the list, and return the new beginning of the list.
> 
> Example 1:
> 
> * Input: head = [0,1,2,3]
> * 
> * Output: [3,2,1,0]
>
> Example 2:
> 
> * Input: head = []
> * 
> * Output: []
>
> Constraints:
> 
> * 0 <= The length of the list <= 1000.
> *
> * -1000 <= Node.val <= 1000

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
> **Question:**
> You are given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right (1-indexed), and return the reversed list.
> 
> Example 1:
> 
> 
> 
> * Input: head = [1,2,3,4,5], left = 1, right = 3
> * 
> * Output: [3,2,1,4,5]

> Example 2:
> 
> * Input: head = [1,1], left = 1, right = 1
> * 
> * Output: [1,1]

> Constraints:
> 
> * The number of nodes in the list is n.
> * 1 <= n <= 500.
> * -500 <= Node.val <= 500
> * 1 <= left <= right <= n
> 
> **Follow up: Could you do it in one pass?**

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
> **Question:**
> You are given the head of a singly linked list head and a positive integer k.
> 
> You must reverse the first k nodes in the linked list, and then reverse the next k nodes, and so on. If there are fewer than k nodes left, leave the nodes as they are.
> 
> Return the modified list after reversing the nodes in each group of k.
> 
> You are only allowed to modify the nodes' next pointers, not the values of the nodes.
> 
> Example 1:
> 
> 
> 
> * Input: head = [1,2,3,4,5,6], k = 3
> * 
> * Output: [3,2,1,6,5,4]
>
> Example 2:
> 
> 
> 
> * Input: head = [1,2,3,4,5], k = 3
> * 
> * Output: [3,2,1,4,5]
>
> Constraints:
> 
> * The length of the linked list is n.
> * 1 <= k <= n <= 5000
> * 0 <= Node.val <= 100
>

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
> **Question:**
> You are given the heads of two sorted linked lists list1 and list2.
> 
> Merge the two lists into one sorted linked list and return the head of the new sorted linked list.
> 
> The new list should be made up of nodes from list1 and list2.
> 
> Example 1:
> 
> 
> 
> * Input: list1 = [1,2,4], list2 = [1,3,5]
> * 
> * Output: [1,1,2,3,4,5]
>
> Example 2:
> 
> * Input: list1 = [], list2 = [1,2]
> * 
> * Output: [1,2]
>
> Example 3:
> 
> * Input: list1 = [], list2 = []
> * 
> * Output: []
>
> Constraints:
> 
> * 0 <= The length of the each list <= 100.
> * -100 <= Node.val <= 100

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
> **Question:**
> You are given two non-empty linked lists, l1 and l2, where each represents a non-negative integer.
> 
> The digits are stored in reverse order, e.g. the number 321 is represented as 1 -> 2 -> 3 -> in the linked list.
> 
> Each of the nodes contains a single digit. You may assume the two numbers do not contain any leading zero, except the number 0 itself.
> 
> Return the sum of the two numbers as a linked list.
> 
> Example 1:
> 
> 
> 
> * Input: l1 = [1,2,3], l2 = [4,5,6]
> * 
> * Output: [5,7,9]
> 
> Explanation: 321 + 654 = 975.
> 
> Example 2:
> * 
> * Input: l1 = [9], l2 = [9]
> 
> * Output: [8,1]
> 
> Constraints:
> 
> * 1 <= l1.length, l2.length <= 100.
> * 0 <= Node.val <= 9

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
> **Question:**
> You are given the head of a singly linked-list.
> 
> The positions of a linked list of length = 7 for example, can intially be represented as:
> 
> [0, 1, 2, 3, 4, 5, 6]
> 
> Reorder the nodes of the linked list to be in the following order:
> 
> [0, 6, 1, 5, 2, 4, 3]
> 
> In the general case, label the nodes by their original zero-based positions from 0 to n - 1. After reordering, those original positions appear in this order:
> 
> [0, n-1, 1, n-2, 2, n-3, ...]
> 
> These numbers represent node positions, not the values stored in the nodes.
> 
> You may not modify the values in the list's nodes, but instead you must reorder the nodes themselves.
> 
> 
> Example 1:
> 
> * Input: head = [2,4,6,8]
> * 
> * Output: [2,8,4,6]
> 
> Example 2:
> 
> * Input: head = [2,4,6,8,10]
> * 
> * Output: [2,10,4,8,6]
> 
> Constraints:
> 
> * 1 <= Length of the list <= 1000.
> * 1 <= Node.val <= 1000

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
> **Question:**
> You are given the head of a linked list of length n. Unlike a singly linked list, each node contains an additional pointer random, which may point to any node in the list, or null.
> 
> Create a deep copy of the list.
> 
> The deep copy should consist of exactly n new nodes, each including:
> 
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
> 
> 
> 
> * Input: head = [[3,null],[7,3],[4,0],[5,1]]
> * 
> * Output: [[3,null],[7,3],[4,0],[5,1]]
> 
> Example 2:
> 
> 
> 
> * Input: head = [[1,null],[2,2],[3,2]]
> * 
> * Output: [[1,null],[2,2],[3,2]]
> 
> Constraints:
> 
> * 0 <= n <= 100
> * -100 <= Node.val <= 100
> * Node values are not guaranteed to be unique.
> * random is null or is pointing to some node in the linked list.

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
> **Question:**
> You are given an array of k linked lists lists, where each list is sorted in ascending order.
> 
> Return the sorted linked list that is the result of merging all of the individual linked lists.
> 
> Example 1:
> 
> * Input: lists = [[1,2,4],[1,3,5],[3,6]]
> * 
> * Output: [1,1,2,3,3,4,5,6]
>
> Example 2:
> 
> * Input: lists = []
> * 
> * Output: []
>
> Example 3:
> 
> * Input: lists = [[]]
> * 
> * Output: []
>
> Constraints:
> 
> * 0 <= lists.length <= 10000
> * 0 <= lists[i].length <= 500
> * -10000 <= lists[i][j] <= 10000
> * lists[i] is sorted in ascending order.
> * The sum of lists[i].length will not exceed 10000.

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
> **Question:**
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
> 
> * Input:
> * ["LRUCache", [2], "put", [1, 10],  "get", [1], "put", [2, 20], "put", [3, 30], "get", [2], "get", [1]]
> * 
> * Output:
> * [null, null, 10, null, null, 20, -1]
> 
> Explanation:
>
> * LRUCache lRUCache = new LRUCache(2);
> * lRUCache.put(1, 10);  // cache: {1=10}
> * lRUCache.get(1);      // return 10
> * lRUCache.put(2, 20);  // cache: {1=10, 2=20}
> * lRUCache.put(3, 30);  // cache: {2=20, 3=30}, key=1 was evicted
> * lRUCache.get(2);      // returns 20 
> * lRUCache.get(1);      // return -1 (not found)
> 
> Constraints:
> 
> * 1 <= capacity <= 3000
> * 0 <= key <= 10^4
> * 0 <= value <= 10^5
> * At most 2 * 10^5 calls will be made to get and put.

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
