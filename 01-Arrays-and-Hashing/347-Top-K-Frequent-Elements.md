# 347 - Top K Frequent Elements
**Date:** August 23, 2026
**Topic:** Arrays & Hashing
**Pattern:** Bucket Sort (Frequency as Index)

## 📝 Problem Statement
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.
* Constraints: Your algorithm's time complexity must be better than $\mathcal{O}(n \log n)$, where $n$ is the array's size.

---

## 🧠 The $\mathcal{O}(n)$ "Bucket Sort" Framework
Most ranking/sorting problems require $\mathcal{O}(n \log n)$ time using standard sorting, or $\mathcal{O}(n \log k)$ time using a Min-Heap. However, this problem has a hidden constraint that allows for a pure $\mathcal{O}(n)$ solution.

**The Hidden Constraint:** The frequency of any element can *never* exceed the length of the array itself. (e.g., In an array of size 6, the maximum possible frequency is 6).

Because the frequencies are strictly bounded from $1$ to $n$, we do not need to sort them. We can use an Array's **index** to represent the **frequency**.

1. **Count Frequencies:** Build a standard `HashMap<Number, Frequency>`.
2. **Invert into Buckets:** Create an array of Lists. Map each number to the index matching its frequency (e.g., if a number appears 3 times, place it in the List at index 2, assuming a `frequency - 1` mapping).
3. **Extract Top K:** Traverse the array of Lists backwards (from highest frequency down to lowest) and collect the elements until we have `k` items.

---

## 💡 The "Aha!" Moments & Debugging Log
* **The "Unsaved Bucket" Trap:** When pulling a list from an array, adding an item to it, and the list was previously null, you must explicitly assign the newly created list *back* to the array index.
* **Off-By-One Loop Bounds:** If mapping `frequency` to `index - 1`, frequency $1$ maps to index $0$. When traversing backwards, the loop condition *must* be `i >= 0` to avoid skipping the lowest frequencies.
* **Fast Mapping Alternative:** Instead of `freqArray[frequency - 1]`, creating the bucket array with size `nums.length + 1` allows you to map frequency exactly to the index (`freqArray[frequency]`), removing mental math and off-by-one risks.

---

## 💻 Final Java Solution

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        
        // 1. Convert array into frequency map
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int freq = 0;
            if (map.containsKey(nums[i])) {
                freq = map.get(nums[i]);
            }
            map.put(nums[i], ++freq);
        }

        // 2. Convert frequency map to array of lists (Bucket Sort)
        // Array size is nums.length. Frequency f goes to index f - 1.
        List[] freqArray = new ArrayList[nums.length];

        for (Integer key : map.keySet()) {
            Integer frequency = map.get(key);
            List<Integer> list = new ArrayList<>();

            if (freqArray[frequency - 1] != null) {
                list = freqArray[frequency - 1];
            }
            
            list.add(key);
            // Crucial fix: put the updated list back into the array
            freqArray[frequency - 1] = list;
        }

        // 3. Extract Top K by iterating backwards
        int[] finalResult = new int[k];
        int count = 0;
        
        for (int i = freqArray.length - 1; i >= 0; i--) {
            if (freqArray[i] != null) {
                for (int j = 0; j < freqArray[i].size(); j++) {
                    finalResult[count++] = (Integer) freqArray[i].get(j);
                    if (count == k) {
                        return finalResult;
                    }
                }
            }
        }

        return finalResult;
    }
}
