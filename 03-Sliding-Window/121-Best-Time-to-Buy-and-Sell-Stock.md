# 121. Best Time to Buy and Sell Stock

**Difficulty:** Easy  
**Topic:** Arrays / Sliding Window / State Tracking

---

## 📝 Problem Description

You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day.
You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return the maximum profit you can achieve. If no profit is possible, return `0`.

**Example:**
* **Input:** `prices = [7, 1, 5, 3, 6, 4]`
* **Output:** `5` 
*(Buy on day 2 at price `1`, sell on day 5 at price `6`. Profit = `6 - 1 = 5`)*

---

## 🧠 Approach 1: The Sliding Window (Two Pointers)

### The Mental Model
We look for a pair of days using a contiguous window. The `left` pointer is our buy day, and the `right` pointer is our sell day.
* The `right` pointer *always* marches forward day by day to explore the future.
* If the price today (`right`) is higher than our buy day (`left`), we have a profitable window! We calculate the profit and see if it breaks our record.
* If the price today (`right`) is *cheaper* than our buy day (`left`), our old buy day was a bad deal. We immediately move our `left` pointer all the way to our current `right` pointer to lock in this new, cheaper buy price.

### The Code
```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;

        int start = 0; // buy price at index 0
        int end = 1; // sell price at index 1

        int maxProfit = 0;

        //[2, 9, 1, 3, 6, 10]
        while(start < n && end <n){
            // buying at start
            // selling at end
            int profit = prices[end] - prices[start];

            if(profit > 0){
                // profit
                if(profit > maxProfit) maxProfit = profit;
                // end++; // sell at a new price
            }else{
                // no profit
                start = end; // buy at lower price
                // end++; // sell at a new price
       
            }
            end++; // sell at a new price
        }
        return maxProfit;
    }
}


```

---
## 🧠 Approach 2: State Tracking (The Optimized Minimum)

### The Mental Model (In Plain English)

> *"I will buy at the lowest price possible. I will initialize my `lastBuyPrice` to the maximum integer value, iterate through the array, and keep updating it to a lower value whenever I find one. My buy price is locked in until the price drops again. As I move further, I calculate the profit and update the max profit only if this new profit is more than the last max profit, doing so until I reach the end of the array."*

Instead of physically maintaining two pointers, we just track the **value** of the lowest price we've seen so far (`lastBuyPrice`). As we loop through the array (which acts as our sell day), we either update our lowest known buy price or cash in on a new max profit.

```java
class Solution {
    public int maxProfit(int[] prices) {
        
        int n = prices.length;

        int buyPrice = Integer.MAX_VALUE;

        int maxProfit = 0;

        // [2, 9, 1, 3, 6, 10]
        for(int i=0; i<n; i++){

            int sellPrice = prices[i];
            int profit = sellPrice - buyPrice;

            if(profit > 0){
                if(profit > maxProfit) maxProfit = profit;
            }else{
                buyPrice = sellPrice; // buy at lower price
            }
        }

        return maxProfit;
    }
}

```

---
## ⏱️ Complexity Analysis (For Both Solutions)

* **Time Complexity:** $\mathcal{O}(n)$
  * We only pass through the array exactly once. The `right` pointer (or the loop index `i`) visits every element sequentially.
* **Space Complexity:** $\mathcal{O}(1)$
  * We use zero extra arrays or data structures. Both solutions only allocate a few primitive integer variables (`left`, `right`, `maxProfit`, or `lastBuyPrice`, `profit`), keeping memory usage strictly constant.

