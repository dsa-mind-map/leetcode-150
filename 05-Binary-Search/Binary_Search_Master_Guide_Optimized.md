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

### 1. Binary Search (LeetCode 704)
> **Question:** Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.
> 
> **Example 1:** `nums = [-1,0,3,5,9,12]`, `target = 9` -> **Output:** `4`
> **Constraints:** All integers in `nums` are unique. `nums` is sorted in ascending order.

```java
public int search(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```
* **Time Complexity:** $O(\log N)$
* **Space Complexity:** $O(1)$

---

## Sub-Pattern 2: Boundary & Insertion Search
**Core Goal:** Finding lower/upper bounds or approximate values where exact matches aren't guaranteed.

### 2. Search Insert Position (LeetCode 35)
> **Question:** Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order. You must write an algorithm with `O(log n)` runtime complexity.
> 
> **Example 1:** `nums = [1,3,5,6]`, `target = 5` -> **Output:** `2`
> **Example 2:** `nums = [1,3,5,6]`, `target = 2` -> **Output:** `1`

```java
public int searchInsert(int[] nums, int target) {
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left;
}
```
* **Time Complexity:** $O(\log N)$
* **Space Complexity:** $O(1)$

---

## Sub-Pattern 3: 2D Matrices & Data Structures
**Core Goal:** Mapping multi-dimensional or custom structural data into a virtual 1D sorted space.

### 3. Search a 2D Matrix (LeetCode 74)
> **Question:** You are given an `m x n` integer matrix `matrix` with the following two properties:
> * Each row is sorted in non-decreasing order.
> * The first integer of each row is greater than the last integer of the previous row.
> Given an integer `target`, return `true` if `target` is in `matrix` or `false` otherwise.

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
* **Time Complexity:** $O(\log(M \times N))$
* **Space Complexity:** $O(1)$

### 4. Time Based Key-Value Store (LeetCode 981)
> **Question:** Design a time-based key-value data structure that can store multiple values for the same key at different timestamps and retrieve the key's value at a certain timestamp. Implement the `TimeMap` class.

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
> **Question:** Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.

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
> **Question:** There is an integer array `nums` sorted in ascending order (with distinct values). Prior to being passed to your function, `nums` is possibly rotated. Given the array `nums` after the rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

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
> **Question:** Koko loves to eat bananas. There are `n` piles of bananas, the `i`-th pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours. Koko can decide her bananas-per-hour eating speed of `k`. Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

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
