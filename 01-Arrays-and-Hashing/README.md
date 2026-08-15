# Pattern 1: Arrays & Hashing
**Focus:** Utilizing Hash Maps or Hash Sets to achieve $\mathcal{O}(1)$ lookups by trading space for time.

## 📚 Trigger Dictionary

* **Problem:** [Contains Duplicate](217-Contains-Duplicate.md)
  * **Trigger Identification:** "Appears at least twice." The moment you need to know if you have *seen an element before*, you need a memory structure. A Hash Set is the perfect $\mathcal{O}(1)$ trigger here to track history "on the go."

* **Problem:** [Valid Anagram](242-Valid-Anagram.md)
  * **Trigger Identification:** "Is an anagram of $t$." Anagrams mean exact character frequencies. "Frequencies" triggers the **Pre-Populated (Two-Pass)** rule. You need a complete global inventory of string $s$ before you can compare it to string $t$.

* **Problem:** [Two Sum](001-Two-Sum.md)
  * **Trigger Identification:** "Two numbers that add up to target" and "Return indices." The need for pairs triggers a Hash Map. The need for indices explicitly disqualifies sorting. Triggers the **On-the-Go (One-Pass)** mapping of `value -> index`.

* **Problem:** [Group Anagrams](049-Group-Anagrams.md)
  * **Trigger Identification:** "Group the anagrams together." Grouping requires a categorized mapping. The trigger is realizing that all anagrams share a "signature" (either a sorted version of the string or an array of character counts). This signature becomes the **Key**, and the **Value** is a list of all strings that match it.

* **Problem:** [Top K Frequent Elements](347-Top-K-Frequent-Elements.md)
  * **Trigger Identification:** "Top $K$" and "most frequent." "Frequent" immediately triggers the need for a **Pre-Populated** Hash Map to get a global count of every element first. *(Note: The "Top $K$" part is a secondary trigger for a Heap or Bucket Sort, making this a hybrid problem).*

* **Problem:** [Product of Array Except Self](238-Product-of-Array-Except-Self.md)
  * **Trigger Identification:** "Product of all elements except `nums[i]`" AND the strict constraint "Without using the division operation in $\mathcal{O}(n)$ time." You need information from both sides of the current index simultaneously. This triggers the **Prefix & Postfix Arrays** pattern. You pre-compute the products to the left and the products to the right, then multiply them together.

* **Problem:** [Valid Sudoku](036-Valid-Sudoku.md)
  * **Trigger Identification:** "Determine if a 9x9 board is valid." The rules state no repetitions are allowed in a row, a column, or a 3x3 square. "No repetitions" immediately triggers **Hash Sets**. Because there are three different overlapping rules, it triggers a multi-set architecture (tracking rows, columns, and blocks independently).

* **Problem:** [Encode and Decode Strings](271-Encode-and-Decode-Strings.md)
  * **Trigger Identification:** "Encode a list of strings to a single string... any possible characters." Because the input strings can contain *any* character (including spaces, commas, or slashes), a simple delimiter like `,` will fail. This triggers the **Length-Prefix Pattern** (e.g., encoding `"hello"` as `"5#hello"`), allowing you to safely parse exact character counts regardless of the content.

* **Problem:** [Longest Consecutive Sequence](128-Longest-Consecutive-Sequence.md)
  * **Trigger Identification:** "Longest consecutive sequence" AND "Must run in $\mathcal{O}(n)$ time." A sequence usually implies sorting, but the strict $\mathcal{O}(n)$ constraint explicitly bans sorting ($\mathcal{O}(n \log n)$). This triggers a **Global Hash Set**. You dump everything into a Set for $\mathcal{O}(1)$ lookups, and to keep it $\mathcal{O}(n)$, you only begin counting a sequence if the current number is the *absolute start* of a chain (i.e., you check if `number - 1` does not exist in the Set).
