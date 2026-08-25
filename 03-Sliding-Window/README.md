# Pattern 3: Sliding Window
**Focus:** Maintaining a contiguous subset of elements (a subarray or substring) and dynamically expanding or shrinking its boundaries to optimize $\mathcal{O}(n^2)$ nested loops into $\mathcal{O}(n)$ linear time.

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
