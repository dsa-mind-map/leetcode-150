# Binary Search Pattern: Master Reference Guide (Tier 1 & 2 Optimized)

Every Binary Search problem operates on a single core principle: **Monotonicity**. If a **search space** is **SORTED or PARTITIONED** such that a condition cleanly divides it into two halves (e.g., `false` followed by `true`), you can eliminate half of the remaining options at every step, reducing linear time $O(N)$ down to logarithmic time $O(\log N)$.

---

## The Master Comparison Table (Tier 1 / Tier 2 Core)

| Sub-Pattern | Problem | What triggers `left = mid + 1`? | What triggers `right = mid` or `mid - 1`? |
| :--- | :--- | :--- | :--- |
| **1. Classic Search** | Binary Search (704) | `nums[mid] < target` | `nums[mid] > target` (or `mid - 1`) |
| **2. Boundary & Insertion** | Search Insert Position (35) | `nums[mid] < target` | `nums[mid] >= target` (or `mid`) |
| **3. Matrix & Structures** | Search a 2D Matrix (74) | `matrix[r][c] < target` | `matrix[r][c] > target` |
| | Time Based Key-Value Store (981) | `timestamps.get(mid) <= time` | `timestamps.get(mid) > time` |
| **4. Rotated Arrays** | Find Min in Rotated Array (153) | `nums[mid] > nums[right]` (Pivot is right) | `nums[mid] <= nums[right]` (Pivot is left/mid) |
| | Search in Rotated Array (33) | Left half unsorted or target outside left sorted range | Right half unsorted or target outside right sorted range |
| **5. Binary Search on Answer** | Koko Eating Bananas (875) | `canFinish(mid)` is `false` (Eat faster) | `canFinish(mid)` is `true` (Eat slower) |

---

## Sub-Pattern 1: Classic Direct Search
**Core Goal:** Finding an exact element or value in a cleanly sorted array or range.

### 1. Binary Search (LeetCode 704) -------------------- DISTINCT & SORTED
> **Question:** 
> You are given an array of distinct integers nums, sorted in ascending order, and an integer target.
> 
> Implement a function to search for target within nums. If it exists, then return its index, otherwise, return -1.
> 
> Your solution must run in 
> O
> (
> l
> o
> g
> n
> )
> O(logn) time.
> 
> **Example 1:**
> 
> * Input: nums = [-1,0,2,4,6,8], target = 4
> * 
> * Output: 3
> * 
> **Example 2:**
> 
> * Input: nums = [-1,0,2,4,6,8], target = 3
> * 
> * Output: -1
> * 
> **Constraints:**
> 
> * 1 <= nums.length <= 10000.
> * -10000 < nums[i], target < 10000
> * All the integers in nums are unique.
> * 
**HINTS**
>
> **search space** [0....n-1]
>
> **after overlapping( start==end)** -> **start will cross the end** or **end will cross the start** if mid is not target.

```java
class Solution {
    public int search(int[] nums, int target) {

        int start = 0;
        int end = nums.length - 1;           // search space [ 0 to n-1]

        while( start <= end ){              // overlapping start & end

            int mid = start + ( end - start ) / 2;

            if(target == nums[mid]){
                return mid;                // found in array
            }else if(target < nums[mid]){
                end = mid - 1;             // target exists in left 
            }else{
                start = mid + 1;           // target exists in right
            }

        }

        return -1; // not found in array
        
    }
}

```
* **Time Complexity:** $O(\log N)$
* **Space Complexity:** $O(1)$

---

## Sub-Pattern 2: Boundary & Insertion Search------------------------- DISTINCT & SORTED
**Core Goal:** Finding lower/upper bounds or approximate values where exact matches aren't guaranteed.

### 2. Search Insert Position (LeetCode 35)
> **Question:**
> You are given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
> 
> You must write an algorithm with O(log n) runtime complexity.
> 
> **Example 1:**
> 
> * Input: nums = [-1,0,2,4,6,8], target = 5
> * 
> * Output: 4
> * 
> **Example 2:**
> 
> * Input: nums = [-1,0,2,4,6,8], target = 10
> * 
> * Output: 6
> * 
> **Constraints:**
> 
> * 1 <= nums.length <= 10,000.
> * -10,000 < nums[i], target < 10,000
> * nums contains distinct values sorted in ascending order.
> * 
**HINTS**
>
> **search space** [0....n]
>
>

```java
class Solution {
    public int searchInsert(int[] nums, int target) {

        int start = 0;
        int end = nums.length;             // search space [0 to n]

        // why "n" because target can be bigger than all elements

        while( start < end ){              // no overlapping of start & end
            
            int mid = start + ( end - start ) / 2;

            if(target <= nums[mid]){      
                end = mid;    // eligible search space = [start to mid]        
                
                // mid-1 can be smaller than the target then target can not be at mid-1 so mid is the possible candidate for correct position
                
            }else if(target > nums[mid]){
                // eligible search space = [mid+1 to end]
                start = mid + 1;
            }

        }

        return start;
        
    }
}
```
* **Time Complexity:** $O(\log N)$
* **Space Complexity:** $O(1)$

---

## Sub-Pattern 3: 2D Matrices & Data Structures
**Core Goal:** Mapping multi-dimensional or custom structural data into a virtual 1D sorted space.

**HINTS**
> **convert 1D index to 2D cordinates ( row, col )**
>
> * row = mid / cols
>
> * col = mid % cols
>
> * index = 6 and cols = 4
>
> * row = 6/4 = 1
> * col = 6%4 = 2
> 
> **convert 2D cordinates ( row, col ) to 1D index**
>
> * index = (row*cols) + col
> *
> * cols = 4 and cordinates (1,2)
> * index = (1*4) + 2 = 6

### 3. Search a 2D Matrix (LeetCode 74)
> **Question:** 
> You are given an m x n 2-D integer array matrix and an integer target.
> 
> Each row in matrix is sorted in non-decreasing order.
> The first integer of every row is greater than the last integer of the previous row.
> Return true if target exists within matrix or false otherwise.
> 
> Can you write a solution that runs in O(log(m * n)) time?
> 
> **Example 1:**
> 
> 
> 
> * Input: matrix = [[1,2,4,8],[10,11,12,13],[14,20,30,40]], target = 10
> * 
> * Output: true
> * 
> **Example 2:**
> 
> 
> 
> * Input: matrix = [[1,2,4,8],[10,11,12,13],[14,20,30,40]], target = 15
> * 
> * Output: false
> * 
> **Constraints:**
> 
> * m == matrix.length
> * n == matrix[i].length
> * 1 <= m, n <= 100
> * -10000 <= matrix[i][j], target <= 10000

```java
public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix == null || matrix.length == 0) return false;
    int m = matrix.length, n = matrix[0].length;
    int left = 0, right = m * n - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        int midVal = matrix[mid / n][mid % n]; // Map 1D index to 2D coordinates
        
        if (midVal == target) {
            return true;
        } else if (midVal < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return false;
}
```
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {

        int rows = matrix.length; // 3
        int cols = matrix[0].length; //4

        for(int i=0; i< rows; i++){

            if(target <= matrix[i][cols-1]){

                int start = 0;
                int end = cols-1;

                while(start <= end){

                    int mid = start + ( end - start ) / 2;

                    if(target == matrix[i][mid]){
                        return true;
                    }else if(target < matrix[i][mid]){
                        end = mid - 1;
                    }else{
                        start = mid + 1;
                    }

                }

            }

        }

        return false;

    }
}


```
* **Time Complexity:** $O(\log(M \times N))$
* **Space Complexity:** $O(1)$

### 4. Time Based Key-Value Store (LeetCode 981)
> **Question:** 
> Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.
> 
> Implement the TimeMap class:
> 
> TimeMap() Initializes the object of the data structure.
> void set(String key, String value, int timestamp) Stores the key key with the value value at the given time timestamp.
> String get(String key, int timestamp) Returns a value such that set was called previously, with timestamp_prev <= timestamp. If there are multiple such values, it returns the value associated with the largest timestamp_prev. If there are no values, it returns "".
> 
> **Example 1:**
> 
> * Input:
> * ["TimeMap", "set", ["alice", "happy", 1], "get", ["alice", 1], "get", ["alice", 2], "set", ["alice", "sad", 3], "get", ["alice", 3]]
> * 
> * Output:
> * [null, null, "happy", "happy", null, "sad"]
> * 
> * Explanation:
> * TimeMap timeMap = new TimeMap();
> * timeMap.set("alice", "happy", 1);  // store the key "alice" and value "happy" along with timestamp = 1.
> * timeMap.get("alice", 1);           // return "happy"
> * timeMap.get("alice", 2);           // return "happy", there is no value stored for timestamp 2, thus we return the value at timestamp 1.
> * timeMap.set("alice", "sad", 3);    // store the key "alice" and value "sad" along with timestamp = 3.
> * timeMap.get("alice", 3);           // return "sad"
> * 
> **Constraints:**
> 
> * 1 <= key.length, value.length <= 100
> * key and value only include lowercase English letters and digits.
> * 0 <= timestamp <= 10^7
> * All the timestamps of set are strictly increasing.
> * At most 2 * 10^5 calls will be made to set and get.

```java
class TimeMap {
    private Map<String, List<Pair>> map;

    private class Pair {
        String value;
        int timestamp;
        Pair(String value, int timestamp) {
            this.value = value;
            this.timestamp = timestamp;
        }
    }

    public TimeMap() {
        map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        map.putIfAbsent(key, new ArrayList<>());
        map.get(key).add(new Pair(value, timestamp));
    }
    
    public String get(String key, int timestamp) {
        if (!map.containsKey(key)) return "";
        List<Pair> list = map.get(key);
        
        int left = 0, right = list.size() - 1;
        String ans = "";
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (list.get(mid).timestamp <= timestamp) {
                ans = list.get(mid).value;
                left = mid + 1; // Look for a closer timestamp to the right
            } else {
                right = mid - 1;
            }
        }
        return ans;
    }
}
```
* **Time Complexity:** $O(1)$ for `set`, $O(\log K)$ for `get` where $K$ is the number of values for a key.
* **Space Complexity:** $O(N)$ total space to store the key-value pairs.

---

## Sub-Pattern 4: Rotated Sorted Arrays
**Core Goal:** Identifying which half of a shifted array remains properly sorted and narrowing down the search.

### 5. Find Minimum in Rotated Sorted Array (LeetCode 153)
> **Question:**
> You are given an array of length n which was originally sorted in ascending order. It has now been rotated between 1 and n times. For example, the array nums = [1,2,3,4,5,6] might become:
> 
> [3,4,5,6,1,2] if it was rotated 4 times.
> [1,2,3,4,5,6] if it was rotated 6 times.
> Notice that rotating the array 4 times moves the last four elements of the array to the beginning. Rotating the array 6 times produces the original array.
> 
> Assuming all elements in the rotated sorted array nums are unique, return the minimum element of this array.
> 
> A solution that runs in O(n) time is trivial, can you write an algorithm that runs in O(log n) time?
> 
> **Example 1:**
> 
> * Input: nums = [3,4,5,6,1,2]
> * 
> * Output: 1
> * 
> **Example 2:**
> 
> * Input: nums = [4,5,0,1,2,3]
> * 
> * Output: 0
> * 
> **Example 3:**
> 
> * Input: nums = [4,5,6,7]
> * 
> * Output: 4
> * 
> **Constraints:**
> 
> * 1 <= nums.length <= 1000
> * -1000 <= nums[i] <= 1000


```java
public int findMin(int[] nums) {
    int left = 0, right = nums.length - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] > nums[right]) {
            left = mid + 1; // Minimum must be in the right half
        } else {
            right = mid; // Minimum is at mid or in the left half
        }
    }
    return nums[left];
}
```
* **Time Complexity:** $O(\log N)$
* **Space Complexity:** $O(1)$

### 6. Search in Rotated Sorted Array (LeetCode 33)
> **Question:**
> You are given an array of length n which was originally sorted in ascending order. It has now been rotated between 1 and n times. For example, the array nums = [1,2,3,4,5,6] might become:
> 
> [3,4,5,6,1,2] if it was rotated 4 times.
> [1,2,3,4,5,6] if it was rotated 6 times.
> Given the rotated sorted array nums and an integer target, return the index of target within nums, or -1 if it is not present.
> 
> You may assume all elements in the sorted rotated array nums are unique,
> 
> A solution that runs in O(n) time is trivial, can you write an algorithm that runs in O(log n) time?
> 
>  **Example 1:**
> 
> * Input: nums = [3,4,5,6,1,2], target = 1
> * 
> * Output: 4
> * 
> **Example 2:**
> 
> * Input: nums = [3,5,6,0,1,2], target = 4
> * 
> * Output: -1
> * 
> **Constraints:**
> 
> * 1 <= nums.length <= 1000
> * -1000 <= nums[i] <= 1000
> * -1000 <= target <= 1000
> * All values of nums are unique.
> * nums is an ascending array that is possibly rotated.

```java
public int search(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) return mid;
        
        // Check if the left half is sorted
        if (nums[left] <= nums[mid]) {
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } 
        // Otherwise, the right half must be sorted
        else {
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    return -1;
}
```
* **Time Complexity:** $O(\log N)$
* **Space Complexity:** $O(1)$

---

## Sub-Pattern 5: Binary Search on Answer
**Core Goal:** When the search space isn't an array, but rather a valid range of possible answers (e.g., speed), you binary search the answer itself and test validity using a helper function.

### 7. Koko Eating Bananas (LeetCode 875)
> **Question:**
> You are given an integer array piles where piles[i] is the number of bananas in the ith pile. You are also given an integer h, which represents the number of hours you have to eat all the bananas.
> 
> You may decide your bananas-per-hour eating rate of k. Each hour, you may choose a pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, you may finish eating the pile but you can not eat from another pile in the same hour.
> 
> Return the minimum integer k such that you can eat all the bananas within h hours.
> 
> **Example 1:**
> 
> * Input: piles = [1,4,3,2], h = 9
> * 
> * Output: 2
> Explanation: With an eating rate of 2, you can eat the bananas in 6 hours. With an eating rate of 1, you would need 10 hours to eat all the bananas (which exceeds h=9), thus the minimum eating rate is 2.
> 
> **Example 2:**
> 
> * Input: piles = [25,10,23,4], h = 4
> * 
> * Output: 25
>
> **Constraints:**
> 
> * 1 <= piles.length <= 10,000
> * piles.length <= h <= 1,000,000,000
> * 1 <= piles[i] <= 1,000,000,000

```java
public int minEatingSpeed(int[] piles, int h) {
    int left = 1, right = 0;
    for (int p : piles) right = Math.max(right, p);
    
    int ans = right;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (canFinish(piles, h, mid)) {
            ans = mid;
            right = mid - 1; // Try a slower speed
        } else {
            left = mid + 1; // Need a faster speed
        }
    }
    return ans;
}

private boolean canFinish(int[] piles, int h, int k) {
    int hours = 0;
    for (int p : piles) {
        hours += (p + k - 1) / k; // Ceiling division
    }
    return hours <= h;
}
```
* **Time Complexity:** $O(N \log M)$ where $N$ is the number of piles and $M$ is the maximum pile size.
* **Space Complexity:** $O(1)$
