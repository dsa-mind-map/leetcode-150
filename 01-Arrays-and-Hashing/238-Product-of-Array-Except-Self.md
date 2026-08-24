# 6. Product of Array Except Self (LeetCode 238)

**Difficulty:** Medium
**Topic:** Arrays & Hashing
**Constraint:** Must run in $\mathcal{O}(n)$ time and $\mathcal{O}(1)$ extra space, without using division.

---

## 🧠 The Mental Model: The "Squash" Technique
The brute force way is to create two arrays (`left[]` and `right[]`), calculate the products for each side, and multiply them together. 

To achieve $\mathcal{O}(1)$ space, we **squash** those arrays into the single output `ans` array by keeping a running tally (a snowball) of the products.

### The Two Passes
1. **Left Product Pass (Left to Right):** 
   Calculate the product of all elements to the left. Store this directly in `ans[i]`.
2. **Right Product Pass (Right to Left):** 
   Calculate the product of all elements to the right. 
   **THE CATCH:** Instead of storing this in a new array, multiply the current `right_product` with the `left_product` that is *already sitting* inside `ans[i]`.

---

## 💻 Optimal Java Solution ($\mathcal{O}(n)$ Time, $\mathcal{O}(1)$ Space)

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];

        // Pass 1: Forward (Calculate Left Products)
        int left_product = 1;
        for (int i = 0; i < n; i++) {
            ans[i] = left_product;                 // Store the left snowball
            left_product = left_product * nums[i]; // Roll snowball forward
        }

        // Pass 2: Backward (Calculate Right Products & Merge)
        int right_product = 1;
        for (int i = n - 1; i >= 0; i--) {
            ans[i] = ans[i] * right_product;         // MULTIPLY with existing left product!
            right_product = right_product * nums[i]; // Roll snowball backward
        }

        return ans;
    }
}
