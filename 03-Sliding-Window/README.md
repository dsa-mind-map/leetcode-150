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

### Group 4: Target Matching (Two Strings)
* **Problem:** [Minimum Window Substring](076-Minimum-Window-Substring.md)
  * **Trigger Identification:** "Minimum window substring" and "every character in $t$ (including duplicates) is included." Matching exact frequencies from another string triggers a **Target Frequency Map** and a `have/need` counter. The expansion trigger is moving `right` to ingest characters until `have == need` (a valid window). The moment the window is valid, it triggers the **Compression Rule**: inch the `left` pointer forward to make the window as small as physically possible while keeping `have == need`, recording the smallest valid size found.
