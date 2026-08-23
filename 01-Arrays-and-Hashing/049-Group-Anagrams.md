# 49 - Group Anagrams
**Date:** August 23, 2026
**Topic:** Arrays & Hashing
**Pattern:** The "Grouping Signature" (Hash Map)

## 📝 Problem Statement
Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.
* Constraints: Strings consist of lowercase English letters.

---

## 🧠 The "Grouping Signature" Framework
Whenever a problem asks you to **"Group [items] by [condition],"** your brain must instantly lock onto a Hash Map (`Map<Signature, List<Item>>`).

The only puzzle left is figuring out what the **Signature (Key)** should be. A signature is a normalized version of the data that is identical for all items in the same group.

* **Trigger Word: "Group"** $\rightarrow$ Triggers a Map.
* **Trigger Word: "Anagrams"** $\rightarrow$ Triggers the knowledge that anagrams share the same characters.

### Finding the Signature
How do I make `"eat"`, `"tea"`, and `"ate"` look exactly the same? 

* **Option 1 (The Sorting Approach):** Sort them. They all become `"aet"`. This creates an $\mathcal{O}(k \log k)$ signature. (This is the approach implemented below).
* **Option 2 (The Problem 3 Approach):** Count the frequencies. They all have 1 'a', 1 'e', 1 't'. You can convert a 26-size frequency array into a string (e.g., `"1#0#0#0#1..."`) and use *that* as the key. This creates a faster $\mathcal{O}(k)$ signature!

---

## 💡 The "Aha!" Moments & Debugging Log
* **String Immutability:** You cannot sort a `String` directly in Java because strings are immutable. You must convert it to a `char[]` first, use `Arrays.sort()`, and then construct a new String to use as the Map Key.
* **Preserving the Original:** Do not sort the array elements in place before storing them! We need to return the *original* strings in the final list, so always save a copy of the original string before sorting its characters.
* **Map Instantiation Shortcut:** Instead of manually iterating over `map.values()` to create the final list of lists, Java allows you to pass `map.values()` directly into an `ArrayList` constructor: `new ArrayList<>(map.values())`.

---

## 🧠 The Optimal Mental Model (3-Step Engine)
1. **Walk:** Iterate through the original array of strings one by one.
2. **Sign:** For each string, create its Signature by converting to `char[]`, sorting it, and converting back to a `String`. 
3. **Group:** Check if the Map has this Signature. If not, initialize a new List. Add the *original* string to this Signature's List.

---

## 💻 Final Java Solution (Using Option 1: Sorting)

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // Map to hold the Signature (sorted string) as Key, and List of original strings as Value
        Map<String, List<String>> map = new HashMap<>();

        for (int i = 0; i < strs.length; i++) {
            String originalStr = strs[i];

            // 1. Create the Signature (Convert to char array, sort, convert back to String)
            char[] charArr = originalStr.toCharArray();
            Arrays.sort(charArr);
            String sortedStr = new String(charArr);

            // 2. Initialize the list if this signature hasn't been seen before
            List<String> anagrams = new ArrayList<>();
            if (map.containsKey(sortedStr)) {
                anagrams = map.get(sortedStr);
            }

            // 3. Add the original string to the grouped list and put it back in the map
            anagrams.add(originalStr);
            map.put(sortedStr, anagrams);
        }

        // Convert the map values (Collection of Lists) into a List of Lists
        return new ArrayList<>(map.values());
    }
}
