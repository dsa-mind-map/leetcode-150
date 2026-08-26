# Pattern 3: Sliding Window
**Focus:** Maintaining a contiguous subset of elements (a subarray or substring) and dynamically expanding or shrinking its boundaries to optimize $\mathcal{O}(n^2)$ nested loops into $\mathcal{O}(n)$ linear time.

2. How to "Spot" it in the wild (The Triggers)
You don't need to guess if a problem is a Sliding Window. The problem description will literally scream it at you if you know the two trigger words:

Trigger 1: "Contiguous" / "Substring" / "Subarray"

The elements must be physically next to each other. If the problem allows you to pick elements from random places (like "subsequence"), Sliding Window is dead. It only works on unbroken chains.

Trigger 2: "Longest" / "Shortest" / "Target Size"

You are asked to optimize a boundary based on a specific rule.

Look at our problem: "Find the length of the longest (Trigger 2) substring (Trigger 1) without repeating characters." It is a perfect match.


3. The Mental Model: The InchwormForget pointers. Imagine an inchworm crawling across an array from left to right. It has a Head and a Tail.The Head (right pointer): Its only job is to move forward and eat new elements, expanding the worm.The Tail (left pointer): Its only job is to move forward to shrink the worm.The inchworm operates on a very simple physical law:The Head always steps forward to eat the next element.If the worm eats something that makes it sick (violates the problem's rule), the Tail must crawl forward, pooping out elements until the worm is healthy again.Because the Head and the Tail only ever move forward, they can only travel the length of the array once. This guarantees $\mathcal{O}(n)$ time.

4. The Universal 4-Step Engine
Every single dynamic Sliding Window problem on earth follows this exact 4-step heartbeat. You don't memorize the code; you just write this logical heartbeat:

Expand: The Head takes one step forward and ingests a new element.

Update State: Record what the Head just ate (e.g., put it in your memory/ledger).

Validate & Shrink (The while loop): Is the worm sick? While the rule is broken, the Tail must step forward, dropping elements out of your memory, until the rule is fixed.

Record Answer: Now that the worm is definitively healthy again, is this the biggest/smallest worm we've seen so far? Update your record.

## 📚 Trigger Dictionary

### Group 1: The Core Window Basics (State Tracking)
* **Problem:** [Best Time to Buy and Sell Stock](121-Best-Time-to-Buy-and-Sell-Stock.md)
  * **Trigger Identification:** "Maximize your profit" and "choose a single day to buy and a different day in the future to sell." The strict chronological constraint (must buy before selling) triggers a one-directional pass. This is a simplified dynamic window where the `right` pointer constantly scans for selling opportunities, and the `left` pointer only jumps forward when it finds a *new absolute lowest price* to buy at.

### Group 2: Dynamic Windows & Hash Sets
* **Problem:** [Longest Substring Without Repeating Characters](003-Longest-Substring-Without-Repeating-Characters.md)
  * **Trigger Identification:** "Longest substring" and "without repeating characters." The word "substring" triggers a contiguous window, and "without repeating" triggers a **Hash Set** to track the current window's state. When the `right` pointer encounters a character already in the Set, it triggers the **Shrink Rule**: aggressively move the `left` pointer forward, removing characters from the Set, until the duplicate is completely cleared.

### Group 3: Advanced Dynamic Windows (Frequencies & Math)
* **Problem:** [Longest Repeating Character Replacement](424-Longest-Repeating-Character-Replacement.md)
  * **Trigger Identification:** "Longest substring containing the same letter" and "replace up to $k$ characters." Needing to know the dominant character in a window triggers a **Frequency Map/Array**. The core mathematical trigger for this specific problem is the formula: `(window_size) - (count_of_most_frequent_char) <= k`. The moment the remaining characters to replace exceed $k$, it triggers the `left` pointer to shrink the window until the formula becomes valid again.

### Group 4: Fixed-Size Windows & Frequencies
* **Problem:** [Permutation In String](567-Permutation-In-String.md)
  * **Trigger Identification:** "Return true if $s2$ contains a permutation of $s1$." A permutation means the exact same character frequencies. Because the permutation must be a contiguous substring that is exactly the length of $s1$, this triggers a **Fixed-Size Window** with a **Frequency Array**. As the window slides one step at a time, you add the right character's frequency, remove the left character's frequency, and compare the current window's frequencies to $s1$'s frequencies.

### Group 5: Target Matching (Two Strings)
* **Problem:** [Minimum Window Substring](076-Minimum-Window-Substring.md)
  * **Trigger Identification:** "Minimum window substring" and "every character in $t$ (including duplicates) is included." Matching exact frequencies from another string triggers a **Target Frequency Map** and a `have/need` counter. The expansion trigger is moving `right` to ingest characters until `have == need` (a valid window). The moment the window is valid, it triggers the **Compression Rule**: inch the `left` pointer forward to make the window as small as physically possible while keeping `have == need`, recording the smallest valid size found.

### Group 6: Advanced Data Structures in Windows
* **Problem:** [Sliding Window Maximum](239-Sliding-Window-Maximum.md)
  * **Trigger Identification:** "Sliding window of size $k$" and "maximum sliding window." Finding the local maximum of a moving window efficiently in $\mathcal{O}(n)$ time triggers a **Monotonic Deque** (Double-ended Queue). You must maintain a strictly decreasing order inside the queue so the maximum element is always instantly available at the front in $\mathcal{O}(1)$ time, while aggressively popping smaller/useless numbers from the back as the window slides.
