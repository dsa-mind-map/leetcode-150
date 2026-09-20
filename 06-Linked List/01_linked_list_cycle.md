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
