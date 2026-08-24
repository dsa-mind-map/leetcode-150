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

        Map<Integer, Integer> map = new HashMap<>();

        int n =  nums.length;

        for(int i=0; i<n; i++){

            if(map.containsKey(nums[i])){
                map.put(nums[i], map.get(nums[i])+1);
            }else{
                map.put(nums[i], 1);
            }

        }

        List[] frequencyArray =  new List[n+1];

        for(Map.Entry entry: map.entrySet()){

            int key = (int)entry.getKey();
            int frequency = (int)entry.getValue();

            List<Integer> freqList = new ArrayList<>();

            if(frequencyArray[frequency] !=null){
               freqList = frequencyArray[frequency]; 
            }

            freqList.add(key);
            frequencyArray[frequency] = freqList;

        }

        int[] finalArray = new int[k];

        int count = 0;

        for(int i=n; i>0; i--){
            List<Integer> freqList = frequencyArray[i];

            if(freqList != null){
                for(int j=0; j<freqList.size(); j++){
                    if(count < k){
                        finalArray[count++] = freqList.get(j);
                    }
            }
            }
            
        }

        return finalArray;



    }
}
