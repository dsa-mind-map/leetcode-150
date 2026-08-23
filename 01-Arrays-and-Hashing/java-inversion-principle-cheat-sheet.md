# 🔄 The Inversion Principle of Hash Maps & Arrays
**Purpose:** Master the fundamental architectural shift between *Counting* and *Ranking* to achieve true $\mathcal{O}(n)$ mastery in coding interviews.

---

## 🧠 The Core Observation: The Inversion Principle
Why do we swap the roles of the **Index** and the **Value** between different phases of an algorithm? 

The answer depends entirely on the goal of your current step: **Are you Counting, or are you Ranking?**

---

## 1. Behavior 1: The "Counting" Phase
When you are ingesting raw, unsorted data and need to aggregate it, you use a direct frequency map or an array-based lookup.

* **Mechanism:** `Index = Item` | `Value = Frequency`
* **The Goal:** Ask the array: *"How many times have I seen this specific item?"*
* **Why Index is the Item:** To count efficiently, you need $\mathcal{O}(1)$ immediate access to an item's tally. By making the item (like a character, `arr['a']`) the index, you can instantly jump to it and increment its value.
* **The Limitation:** This only works if the "universe" of items is small and known in advance (e.g., 26 lowercase English letters or 256 ASCII values). For arbitrary items, a `HashMap` is used instead.

```java
// Example: Behavior 1 (Array of Counts / Valid Anagram)
int[] charCount = new int[26];
for (char c : word.toCharArray()) {
    charCount[c - 'a']++; // Index is the item ('a'-'z'), Value is the frequency
}
```

---

## 2. Behavior 2: The "Ranking" Phase (Bucket Sort)
Once you have successfully gathered your counts, the problem shifts. You no longer care *just* about the frequencies; you need to **sort, group, or rank** the items based on those frequencies.

* **Mechanism:** `Index = Frequency` | `Value = Item(s)`
* **The Goal:** Answer the question: *"Which items share this exact frequency?"* or *"What are the top K items?"*
* **Why Index is the Frequency:** To rank things in pure $\mathcal{O}(n)$ time—bypassing traditional comparison sorting ($\mathcal{O}(n \log n)$)—you need the array itself to act as a timeline or ordering structure. If you want the highest frequency, you need to jump to the right side (the end) of the array. The only way to achieve this layout is if the **Index represents the Frequency**.

```java
// Example: Behavior 2 (Array of Lists / Bucket Sort for Top K)
// Size is N + 1 so the Index (Frequency) safely matches the bounds
List<Integer>[] buckets = new List[nums.length + 1];

for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
    int key = entry.getKey();     // The original item
    int freq = entry.getValue();  // Becomes the Index!
    
    if (buckets[freq] == null) {
        buckets[freq] = new ArrayList<>();
    }
    buckets[freq].add(key);
}
```

---

## ⚡ The "Aha!" Connection: The Complete Pipeline
These two behaviors aren't separate tricks—they are **Step 1 and Step 2 of the exact same data pipeline**.

### Interview Blueprint: "Find the Top K Elements"
If an interviewer asks you to find the most frequent items, you use **both behaviors back-to-back**:

1. **Step 1 (Counting):** Build a frequency map/array where the **Index is the Item** to aggregate counts in $\mathcal{O}(n)$ time.
2. **Step 2 (Ranking):** Invert the data structure. Build a bucket array where the **Index is the Frequency** to group items by their counts. 
3. **Step 3 (Extraction):** Read the bucket array **backward** from highest index to lowest index to extract your Top $K$ elements in total $\mathcal{O}(n)$ time.

```java
// --- THE FULL INVERSION PIPELINE TEMPLATE ---
public List<Integer> topKFrequent(int[] nums, int k) {
    // Phase 1: COUNTING (Index = Item / Value = Frequency)
    Map<Integer, Integer> countMap = new HashMap<>();
    for (int num : nums) {
        countMap.put(num, countMap.getOrDefault(num, 0) + 1);
    }
    
    // Phase 2: RANKING / BUCKETING (Index = Frequency / Value = List of Items)
    List<Integer>[] buckets = new List[nums.length + 1];
    for (Map.Entry<Integer, Integer> entry : countMap.entrySet()) {
        int freq = entry.getValue();
        if (buckets[freq] == null) {
            buckets[freq] = new ArrayList<>();
        }
        buckets[freq].add(entry.getKey());
    }
    
    // Phase 3: EXTRACTION (Read backward for highest rank)
    List<Integer> result = new ArrayList<>();
    for (int i = buckets.length - 1; i >= 0 && result.size() < k; i--) {
        if (buckets[i] != null) {
            for (int num : buckets[i]) {
                result.add(num);
                if (result.size() == k) break;
            }
        }
    }
    return result;
}
```
