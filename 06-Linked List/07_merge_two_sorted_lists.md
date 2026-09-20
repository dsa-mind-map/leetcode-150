# 7. Merge Two Sorted Lists (LeetCode 21)

**Alignment:** Pillar 3 (Sentinel Pattern), Block 3 (The Two-List Combiner)  

---

## 📋 Problem Description
You are given the heads of two sorted linked lists `list1` and `list2`. Merge the two lists into one sorted linked list.

* **Example 1:**
  * Input: `list1 = [1,2,4]`, `list2 = [1,3,5]`
  * Output: `[1,1,2,3,4,5]`

**Constraints:**
* $0 \le \text{Length of each list} \le 100$

---

## ⚠️ Common Pitfalls
Running a loop with `||` creates messy internal checks. Using `while (l1 != null && l2 != null)` followed by appending any list remainder cleanly avoids messy null pointer errors.

---

## 💻 Java Implementation

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;

        while(list1 != null && list2 != null){
            if(list1.val <= list2.val){
               curr.next = list1; 
               list1 = list1.next;
            } else {
                curr.next = list2;
                list2 = list2.next;
            }
            curr = curr.next;
        }

        if(list1 != null) curr.next = list1;
        if(list2 != null) curr.next = list2;

        return dummy.next;
    }
}
```
