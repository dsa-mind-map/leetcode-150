# 11. Merge k Sorted Lists (LeetCode 23)

**Alignment:** Block 5 (Min-Heap Combiner)  
**Additional Learning:** Utilizing Priority Queues (Min-Heaps) to continuously extract the smallest current node across $k$ different lists in $O(N \log k)$ time.

---

## 🏛️ Edge Case Breakdown (Array Input Nuances)
* **`null`**: Represents an uninitialized or non-existent linked list.
* **`ListNode[] lists = []`**: The array itself is empty (zero lists provided).
* **`ListNode[] lists = [[]]`**: The array contains one element, but that element is an empty list (`null` head).

---

## 📋 Problem Description
You are given an array of $k$ linked lists `lists`, where each list is sorted in ascending order. Merge all the linked lists into one sorted linked list and return it.

### Constraints
* $0 \le \text{lists.length} \le 10,000$
* $0 \le \text{lists[i].length} \le 500$
* $-10,000 \le \text{lists[i][j]} \le 10,000$
* The sum of `lists[i].length` will not exceed $10,000$.

---

## ⚠️ Common Pitfalls
1. **Null-Pointer Exceptions & Empty Inputs:** Passing an empty array or an array containing null heads (`[[]]`) will throw exceptions if you do not validate `list != null` before pushing items into the Min-Heap.
2. **Time Limit Exceeded (TLE):** Leaving debug statements like `System.out.println(list)` inside loops over large arrays will cause massive I/O overhead and trigger a TLE.

---

## 💻 Optimized Java Implementation

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode dummy = new ListNode(0); // Sentinel head for the merged output
        ListNode curr = dummy;

        // Min-Heap (comparator `list1.val - list2.val` ensures ascending order)
        // Note: Diamond operator <> is mandatory on both sides in older Java versions
        PriorityQueue<ListNode> pq = new PriorityQueue<>((l1, l2) -> l1.val - l2.val);

        // Populate heap with the starting head of each valid list
        for (ListNode list : lists) {
            // CRITICAL: Prevent TLE by avoiding console printing inside tight loops
            // CRITICAL: Guard against [[]] or null entries to avoid NullPointerExceptions
            if (list != null) {   
                pq.offer(list);
            }
        }

        // Extract minimums and replenish the heap
        while (!pq.isEmpty()) {
            ListNode minElement = pq.poll(); // CRITICAL: Extract current global minimum

            curr.next = minElement;          // Append to final list
            curr = curr.next;

            // CRITICAL: Push the next node of the exhausted list into the heap
            if (minElement.next != null) {
                pq.offer(minElement.next);
            }
        }

        return dummy.next; // Return the true head of the merged list
    }
}
