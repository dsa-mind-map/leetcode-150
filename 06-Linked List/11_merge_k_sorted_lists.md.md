# 11. Merge k Sorted Lists (LeetCode 23)

**Alignment:** Block 5 (Min-Heap Combiner)  
**Additional Learning:** Utilizing Priority Queues (Min-Heaps) to continuously extract the smallest current node across $k$ different lists.

---

## 📋 Problem Description
You are given an array of $k$ linked lists `lists`, where each list is sorted in ascending order. Merge all the linked lists into one sorted linked list and return it.

**Constraints:**
* $0 \le \text{lists.length} \le 10,000$

---

## ⚠️ Common Pitfalls
Passing a completely empty list (`[]`) or an array containing null heads (`[[]]`). You must ensure `list != null` before offering to the Min-Heap. Also, avoid debugging print statements on list nodes which trigger Time Limit Exceeded exceptions.

---

## 💻 Java Implementation

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode dummy = new ListNode(0); 
        ListNode curr = dummy;

        PriorityQueue<ListNode> pq = new PriorityQueue<>((l1, l2) -> l1.val - l2.val);

        for(ListNode list : lists){
            if(list != null){   
                pq.offer(list);
            }
        }

        while(!pq.isEmpty()){
            ListNode minElement = pq.poll();
            curr.next = minElement;
            curr = curr.next;

            if(minElement.next != null){
                pq.offer(minElement.next);
            }
        }

        return dummy.next;
    }
}
```