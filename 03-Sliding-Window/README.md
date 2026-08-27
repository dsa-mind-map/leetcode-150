# Pattern 3: Sliding Window

**Focus:** Maintaining a contiguous subset of elements (a subarray or substring) and dynamically expanding or shrinking its boundaries to optimize $\mathcal{O}(n^2)$ nested loops into $\mathcal{O}(n)$ linear time.

## 🎯 How to Spot It (The Triggers)
If a problem description contains these two conditions, it is almost certainly a Sliding Window problem:

1. **"Contiguous" / "Substring" / "Subarray":** The elements must be physically adjacent. If the problem allows picking elements from random places (e.g., "subsequence"), Sliding Window will not work.
2. **"Longest" / "Shortest" / "Target Size":** You are asked to optimize a boundary based on a specific constraint or rule.

## 🧠 The Mental Model: The Inchworm
Imagine an inchworm crawling across an array from left to right.
* **The Head (Right Pointer):** Its only job is to move forward to ingest new elements and expand the window.
* **The Tail (Left Pointer):** Its only job is to move forward to shrink the window when a rule is violated.

Because the Head and Tail only ever move forward, they traverse the array exactly once, guaranteeing $\mathcal{O}(n)$ time complexity.

## ⚙️ The Universal 4-Step Engine
Every dynamic Sliding Window problem follows this exact logical heartbeat:

1. **Expand:** The Head takes one step forward and ingests a new element.
2. **Update State:** Record what the Head just added (e.g., update a hash map, frequency array, or running sum).
3. **Validate & Shrink (The `while` loop):** Is the window invalid? While the rule is broken, the Tail steps forward, removing elements from your state until the rule is satisfied again.
4. **Record Answer:** Once the window is valid, check if it's the longest/shortest seen so far and update your global record.

---

## 📐 The 3 Tail Movement Patterns
Sliding window problems fall strictly into one of three behavioral buckets based on how the Tail moves.

### Pattern 1: The Fixed Step (+1)
**The Rule:** The window size is strictly locked to a specific number ($k$). When the window reaches that size, the Tail steps forward by +1 every time the Head steps forward by +1.

* **[Maximum Average Subarray I](643-Maximum-Average-Subarray-I.md):** Find a contiguous subarray of exactly size $k$.
* **[Minimum Recolors to Get K Consecutive Black Blocks](Minimum-Recolors-to-Get K-Consecutive-Black-Blocks.md):** Look for a block of exactly $k$ consecutive elements.
* **Substrings of Size Three with Distinct Characters (1876):** Window size locked to exactly 3.
* **Minimum Difference Between Highest and Lowest of K Scores (1984):** Evaluate chunks of exactly $k$ students.
* **Permutation In String (567):** Permutations must match the exact length of the target string.
* **Sliding Window Maximum (239):** Find the maximum in a sliding window of exactly size $k$.

### Pattern 2: The Squeeze (`while` Loop +1)
**The Rule:** The window size is dynamic. The Tail sits idle while the Head expands. When a rule is broken, a `while` loop triggers, creeping the Tail forward step-by-step until the rule is satisfied again.

* **Longest Substring Without Repeating Characters (3):** When a duplicate is found, the Tail squeezes forward until the duplicate is removed.
* **Longest Repeating Character Replacement (424):** If the window contains more than $k$ non-matching characters, the Tail squeezes forward until back under the limit.
* **Minimum Window Substring (76):** Head expands until all target characters are found. The Tail then squeezes aggressively to make the window as small as possible without losing required characters.

> **Pro-Tip: The "Jump" Optimization**
> In "Squeeze" problems, advancing the Tail by +1 inside a loop can be slow if the target element is far away. Instead of a Hash Set, use a Hash Map to store the exact index of each character. If a duplicate is spotted, instantly jump the Tail: `tail = Math.max(tail, map.get(duplicateChar) + 1);`

### Pattern 3: The Snap (Reset to Head)
**The Rule:** The window size is dynamic. The Tail tracks the start of a streak. When the streak breaks, the old window is useless. The Tail abandons the old window and snaps directly to the Head's position to start fresh.

* **Best Time to Buy And Sell Stock (121):** Tail tracks the lowest buy price. If Head finds a lower price, the old buy price is discarded, and the Tail snaps instantly to the Head.
* **Longest Continuous Increasing Subsequence (674):** Head expands as long as numbers increase. If the next number drops, the streak dies, and the Tail snaps to the Head to start a new streak.

---

## 📚 Trigger Dictionary (State Tracking Archetypes)

| Problem Archetype | Triggers | State Tracking Mechanism |
| :--- | :--- | :--- |
| **Core Basics** | "Maximize profit", chronological constraint. | Simplified dynamic window. Right pointer scans, left pointer jumps to new absolute minimums. |
| **Unique Elements** | "Longest substring", "without repeating". | **Hash Set.** Right adds to set; Left removes from set until duplicates are cleared. |
| **Freq & Math** | "Replace up to $k$ characters", "Longest repeating". | **Frequency Map.** Rule: `(window_size) - (max_freq) <= k`. Shrink when violated. |
| **Anagrams/Perms** | "Permutation of $s1$". | **Fixed-Size Window + Frequency Array.** Slide 1 step at a time, compare frequencies. |
| **Target Matching** | "Minimum window", "Contains every character in target". | **Target Map + `have/need` Counter.** Expand until `have == need`, then shrink to find minimum. |
| **Local Extrema** | "Maximum sliding window", "Size $k$". | **Monotonic Deque.** Maintain strictly decreasing order. Max element is always at the front in $\mathcal{O}(1)$ time. |
