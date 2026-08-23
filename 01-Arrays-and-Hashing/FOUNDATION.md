# 🚀 Java DSA Master Cheat Sheet
**Purpose:** Instant syntax recall for Sorting, Maps, Heaps, and Bucket Sort to achieve zero-friction coding in interviews.

---

## 1. 🛠️ Sorting in Java
Java handles sorting differently based on whether you are sorting a primitive array, an object array, or a Collection (List).

### A. Primitive Arrays (`int[]`, `char[]`)
You cannot use custom comparators (lambdas) on primitives. You must rely on default ascending sort.

        int[] arr = {5, 2, 8, 1};
        Arrays.sort(arr); // Result: [1, 2, 5, 8]

// To sort descending, you must sort ascending first, then manually reverse, 
// OR convert int[] to Integer[] first.

### B. Collections (ArrayList, LinkedList)
Collections can be sorted in place using .sort().

        List<Integer> list = new ArrayList<>(Arrays.asList(5, 2, 8, 1));
        
        // Ascending Order
        list.sort((a, b) -> a - b); 
        
        // Descending Order
        list.sort((a, b) -> b - a);

### C. Strings and Objects (Using .compareTo())
You cannot use a - b math on Strings. You must use .compareTo().
        
        List<String> words = new ArrayList<>(Arrays.asList("zebra", "apple", "mango"));
        // Ascending (A to Z)
        words.sort((a, b) -> a.compareTo(b));
        // Descending (Z to A)
        words.sort((a, b) -> b.compareTo(a));

### D. Sorting 2D Arrays (Critical for "Interval" Problems)
When sorting int[][] (like [[1,3], [2,6], [8,10]]), you must use a comparator.
**Pro-Tip: Always use Integer.compare(a, b) instead of a - b for arrays to prevent integer overflow bugs.**
        
        int[][] intervals = {{8, 10}, {1, 3}, {2, 6}};
        
        // Sort ascending by the first element of each inner array
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        // Result: [[1, 3], [2, 6], [8, 10]]

### 2. 🗺️ Maps (Dictionaries)
Maps are the most important data structure in FAANG interviews. O(1) lookups.

### A. The 3 Ways to Iterate
Never iterate over keys just to look up values later (map.get(key) inside a loop is slow). Use entrySet().
        
        Map<String, Integer> map = new HashMap<>();
        map.put("Alice", 5);
        map.put("Bob", 3);
        
        // 1. Iterate Both Keys and Values (Most Common)
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            String key = entry.getKey();
            int val = entry.getValue();
        }
        
        // 2. Iterate Only Keys
        for (String key : map.keySet()) { ... }
        
        // 3. Iterate Only Values
        for (int val : map.values()) { ... }

### B. The "Big 3" DSA Map Methods
Memorize these to avoid writing bulky if/else statements.
        
        // 1. getOrDefault(key, default) -> Perfect for Counting Frequencies
        map.put(num, map.getOrDefault(num, 0) + 1);
        
        // 2. putIfAbsent(key, value) -> Perfect for Graph/Tree Adjacency Lists
        map.putIfAbsent(node, new ArrayList<>());
        map.get(node).add(neighbor);
        
        // 3. computeIfAbsent(key, mappingFunction) -> Cleaner alternative to putIfAbsent
        map.computeIfAbsent(node, k -> new ArrayList<>()).add(neighbor);

### C. HashMap vs. TreeMap
HashMap: Unordered. O(1) time. (Default choice).
TreeMap: Keys are always sorted ascending. O(logN) time. (Use only when the problem explicitly requires keeping the data continuously sorted by Key).

### 3. 🏔️ Heaps (PriorityQueue)
Heaps are used to continuously track the "Top K" or "Bottom K" elements in a dataset or a live stream.

### A. Initialization
        
        // Min-Heap (Default). Smallest element at the top.
        Queue<Integer> minHeap = new PriorityQueue<>();
        
        // Max-Heap. Largest element at the top.
        Queue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
        
        // Object Heap (e.g., sorting int[] by the second element)
        Queue<int[]> pairHeap = new PriorityQueue<>((a, b) -> a[1] - b[1]);

### B. Core Methods & Complexity

        heap.add(val) - Inserts value and bubbles it to correct position. - O(logN)
        heap.poll() - Removes and returns the top element. - O(logN)
        heap.peek() - Returns the top element without removing it. - O(1)
        heap.size() - Returns the number of elements. - O(1)

### C. Standard "Top K" Template
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

### 4. 🪣 Bucket Sort
Bucket Sort abandons traditional comparison (a > b) and instead uses an Array's Index as the Key/Frequency/Value to achieve O(N) time.

### A. Specific Use Cases
Frequencies: When counting how many times items appear.
Bounded Data: When the problem states "Values range from 1 to 100" or "Array length is N" (meaning max possible frequency is N).

### B. Limitations (When NOT to use)
Massive Ranges: If values range from 1 to 10^9, creating new int[1000000000] will crash memory.
Continuous Data: Cannot be used for floating-point values (3.14 cannot be an array index).

### C. Variation 1: Array of Counts (E.g., Valid Anagram)
Used when the universe of values is tiny and known (e.g., 26 lowercase English letters).
        
        // Convert characters 'a'-'z' to indices 0-25
        int[] charCount = new int[26];
        for (char c : word.toCharArray()) {
            charCount[c - 'a']++; 
        }

### D. Variation 2: Array of Lists (E.g., Top K Frequent)
Used when multiple different items can share the exact same frequency, and the maximum frequency is bound by the array length N.

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



