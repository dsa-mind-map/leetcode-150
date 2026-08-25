# 🚀 Java DSA Master Cheat Sheet
**Purpose:** Instant syntax recall for Sorting, Maps, Heaps, and Bucket Sort to achieve zero-friction coding in interviews.

---
# Java Collection & Array Conversion Guide

This guide covers common and efficient ways to convert between primitive arrays, wrapper object arrays, `List`, and `Set` in Java.

---

## 1. Primitive Array to Object Array (`int[]` -> `Integer[]`)

### Stream Way
```java
int[] primitives = {1, 2, 3, 4, 5};

Integer[] objArray = Arrays.stream(primitives)
                            .boxed()
                            .toArray(Integer[]::new);
```

### Traditional Way
```java
int[] primitives = {1, 2, 3, 4, 5};
Integer[] objArray = new Integer[primitives.length];

for (int i = 0; i < primitives.length; i++) {
    objArray[i] = primitives[i]; // Autoboxing
}
```

---

## 2. Primitive Array to List (`int[]` -> `List<Integer>`)

### Modern Stream Way (Java 16+)
```java
int[] primitives = {1, 2, 3, 4, 5};

List<Integer> list1 = Arrays.stream(primitives)
                            .boxed()
                            .toList();
```

### Traditional Loop Way (Fastest performance, zero stream overhead)
```java
int[] primitives = {1, 2, 3, 4, 5};

List<Integer> list2 = new ArrayList<>(primitives.length);
for (int num : primitives) {
    list2.add(num);
}
```

---

## 3. Object Array to List (`Integer[]` -> `List<Integer>`)

```java
Integer[] objectArray = {1, 2, 3, 4, 5};

// Fixed-size list backed by the array
List<Integer> list = Arrays.asList(objectArray);

// Mutable list (if you need to add/remove elements later)
List<Integer> mutableList = new ArrayList<>(Arrays.asList(objectArray));
```

---

## 4. List to Primitive Array (`List<Integer>` -> `int[]`)

```java
List<Integer> list = new ArrayList<>(List.of(1, 2, 3));

// Modern Java (Java 8+)
int[] arr = list.stream().mapToInt(i -> i).toArray();
```

---

## 5. Object Array or List to Set ( `Integer[]`, `List<Integer>` ->   `Set<Integer>` )

```java
Integer[] numsArray = {1, 2, 3, 2};

// From Array to Set
Set<Integer> set = new HashSet<>(Arrays.asList(numsArray));

// From List to Set
List<Integer> list = List.of(1, 2, 3, 2);
Set<Integer> uniqueSet = new HashSet<>(list);
```
---

## 1. 🛠️ Sorting in Java
Java handles sorting differently based on whether you are sorting a primitive array, an object array, or a Collection (List).

### A. Primitive Arrays (`int[]`, `char[]`)
You cannot use custom lambdas on primitive arrays directly. 

```java
int[] arr = {5, 2, 8, 1};
Arrays.sort(arr); // Ascending: [1, 2, 5, 8]

// INTERVIEW TRICK: To sort primitives descending, you MUST either:
// 1. Sort ascending and run a standard two-pointer reverse loop.
// 2. Convert to Integer[] (Wrapper class) and use Collections.reverseOrder().



int[] primitiveArray = {1, 2, 3, 4, 5};

// Convert int[] to Integer[]
Integer[] objectArray = Arrays.stream(primitiveArray)
                              .boxed()
                              .toArray(Integer[]::new);

Arrays.sort(objectArray, Collections.reverseOrder()); // Descending
```

### B. Collections (ArrayList, LinkedList)
Collections are sorted in-place using `.sort()`.
```java
List<Integer> list = new ArrayList<>(Arrays.asList(5, 2, 8, 1));

// Ascending Order
list.sort((a, b) -> a - b); 

// Descending Order
list.sort((a, b) -> b - a);
```

### C. Strings and Objects (Using `.compareTo()`)
You cannot use `a - b` math on Strings. You must use `.compareTo()`.
```java
List<String> words = new ArrayList<>(Arrays.asList("zebra", "apple", "mango"));

// Ascending (A to Z)
words.sort((a, b) -> a.compareTo(b));

// Descending (Z to A)
words.sort((a, b) -> b.compareTo(a));
```

### D. 2D Arrays & Intervals (Crucial for Matrix/Merge problems)
When sorting `int[][]` (like `[[1,3], [2,6], [8,10]]`), use `Integer.compare()`.

**Interview Trap:** Never use `a[0] - b[0]` to sort arrays if the numbers can be large negative integers. It will cause integer overflow. `Integer.compare` safely prevents this.
```java
int[][] intervals = {{8, 10}, {1, 3}, {2, 6}};

// Sort ascending by the first element of each inner array
Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
// Result: [[1, 3], [2, 6], [8, 10]]
```

## 2. 🗺️ Maps (Dictionaries)
The most important data structure in FAANG interviews. $\mathcal{O}(1)$ lookups.

### A. The 3 Ways to Iterate
Never iterate over keys just to look up values later (calling `map.get(key)` inside a loop is inefficient). Use `entrySet()`.
```java
Map<String, Integer> map = new HashMap<>();

// 1. Iterate Both Keys and Values (Most Common & Efficient)
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    int val = entry.getValue();
}

// 2. Iterate Only Keys
Set<String> keys = map.keySet();

for (String key : map.keySet()) { /* ... */ }


// 3. Iterate Only Values
Collection<Integer> values = map.values();
List<Integer> valueList = new ArrayList<>(values);
        
for (int val : map.values()) { /* ... */ }
```

### B. The "Big 3" Map Methods
Memorize these to avoid writing bulky if/else checks.
```java
// 1. Counting Frequencies cleanly
map.put(num, map.getOrDefault(num, 0) + 1);

// 2. Graph/Tree Adjacency Lists (Basic)
map.putIfAbsent(node, new ArrayList<>());
map.get(node).add(neighbor);

// 3. Graph/Tree Adjacency Lists (Advanced & Cleanest)
map.computeIfAbsent(node, k -> new ArrayList<>()).add(neighbor);


// 3. convert keys to set and values to list
return map.keySet();
return new ArrayList<>(map.values());

```

### C. Map Variations (When to use what)
* **`HashMap`**: Unordered. $\mathcal{O}(1)$ time. (Your 99% default choice).
* **`TreeMap`**: Keys are strictly sorted ascending. $\mathcal{O}(\log n)$ time. (Use when the problem requires maintaining a dynamically sorted list of keys).
* **`map.entrySet()`** returns a Set<Map.Entry<K, V>>, not a List. You cannot call .sort() directly on a Set because Sets do not have an index-based ordering.To make your code syntactically valid in Java, you must wrap the entry set inside an ArrayList constructor first:
       
        Map<Integer, String> map = new HashMap<>();
        // ... populate map ...
        
        // 1. Convert entrySet to a List
        List<Map.Entry<Integer, String>> mapEntryList = new ArrayList<>(map.entrySet());
        
        // 2. Sort using the comparator
        mapEntryList.sort((a, b) -> b.getValue().compareTo(a.getValue()));
* **`LinkedHashMap`**: Maintains insertion order. $\mathcal{O}(1)$ time. (Critical for building an LRU Cache).

## 3. 🏔️ Heaps (PriorityQueue)
Used to continuously track the "Top K" or "Bottom K" elements in a dataset or a live data stream.

### A. Initialization
```java
// Min-Heap (Default). Smallest element bubbles to the top.
Queue<Integer> minHeap = new PriorityQueue<>();

// Max-Heap. Largest element bubbles to the top.
Queue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);

// Object/Array Heap (e.g., sorting an int[] pair by the second element)
Queue<int[]> pairHeap = new PriorityQueue<>((a, b) -> Integer.compare(a[1], b[1]));
```

### B. Core Methods & Complexity

| Method | Action | Time Complexity |
|---|---|---|
| `heap.add(val)` | Inserts value and rebalances heap. | $\mathcal{O}(\log n)$ |
| `heap.poll()` | Removes and returns the top element. | $\mathcal{O}(\log n)$ |
| `heap.peek()` | Looks at the top element without removing it. | $\mathcal{O}(1)$ |
| `heap.size()` | Returns the count of elements. | $\mathcal{O}(1)$ |

### C. Standard "Top K" Template
```java
// Problem: Find the K LARGEST elements in an array.
// Solution: Use a Min-Heap of strict size K. 
Queue<Integer> minHeap = new PriorityQueue<>();
for (int num : nums) {
    minHeap.add(num);
    if (minHeap.size() > k) {
        minHeap.poll(); // Kicks out the smallest element automatically
    }
}
// The Heap now contains exactly the K largest elements.
```

## 4. 🪣 Bucket Sort
Bucket Sort abandons traditional comparison (`a > b`) and instead uses an Array's Index as the Key/Frequency/Value to achieve $\mathcal{O}(n)$ time.

### A. Specific Use Cases
* **Frequencies:** When counting how many times items appear.
* **Bounded Data:** When the problem states "Values range from 1 to 100" or "Array length is $N$" (meaning max possible frequency is $N$).

### B. Limitations (When NOT to use)
* **Massive Ranges:** If values range from $1$ to $10^9$, creating new `int[1000000000]` will crash memory.
* **Continuous Data:** Cannot be used for floating-point values (3.14 cannot be an array index).

### C. Variation 1: Array of Counts (E.g., Valid Anagram)
Used when the universe of values is tiny and known (e.g., 26 lowercase English letters).
```java
// Convert characters 'a'-'z' to indices 0-25
int[] charCount = new int[26];
for (char c : word.toCharArray()) {
    charCount[c - 'a']++; 
}
```

### D. Variation 2: Array of Lists (E.g., Top K Frequent)
Used when multiple different items can share the exact same frequency, and the maximum frequency is bound by the array length $N$.
```java
// Size is N + 1 so the Index exactly matches the Frequency
List<Integer>[] buckets = new List[nums.length + 1];

// Populate Buckets (Frequency becomes the Index)
for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
    int key = entry.getKey();
    int freq = entry.getValue();
    
    if (buckets[freq] == null) {
        buckets[freq] = new ArrayList<>();
    }
    buckets[freq].add(key);
}

// Read the bucket array backward to get the highest frequencies in O(N) time.
```
