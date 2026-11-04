# The World #1 Backtracking Master Guide

https://neetcode.io/courses/dsa-for-beginners/22

https://neetcode.io/courses/advanced-algorithms/11

https://neetcode.io/courses/advanced-algorithms/12

https://neetcode.io/courses/advanced-algorithms/13

*Architected for First-Principles Recognition, Pattern Mapping, and Zero-Latency Recall*

---

## Introduction: The Backtracking Mental Model

Backtracking is **Systematic Exhaustive Search with Pruning**. Every backtracking problem maps to a state-space tree traversal. To master it to the point where you can write and explain every line under pressure, you analyze every problem through four lenses:

1. **The State (`What is the current configuration?`):** Variables representing your current position (e.g., `path`, `index`, `remaining sum`, `visited array`, `board`).
2. **The Choices (`What can I do next?`):** Valid transitions from the current state to the next state.
3. **The Constraints (`When is a choice illegal?`):** Rules that **prune** the search tree early to eliminate redundant execution paths.
4. **The Goal / Base Case (`When do I record a valid solution?`):** The termination condition signaling a leaf node or valid target has been reached.

---

## Section 1: Comprehensive Taxonomy of Your Complete Problem Set

Your requested problem list comprises elite problems heavily featured across Tier-1 and Tier-2 engineering interviews (Google, Amazon, Meta, Microsoft, Walmart). Here is how every single one maps into the **4 Core Architectural Archetypes**:

| Core Archetype | Defining Characteristic | Mapped Problems from Your List |
| --- | --- | --- |
| **Pattern 1: Subset / Inclusion-Exclusion** | Binary choice per element (Pick/Skip) or building power sets. Order doesn't matter; duplicates require sorting & skipping. | • **Subsets**<br>

<br>• **Subsets II**<br>

<br>• **Sum of All Subsets XOR Total** |
| **Pattern 2: Combination / Target-Driven** | Reusable or sequential index-based selection aiming for a target sum, exact size $k$, or equal partitioning. Order does not matter. | • **Combinations**<br>

<br>• **Combination Sum**<br>

<br>• **Combination Sum II**<br>

<br>• **Matchsticks to Square**<br>

<br>• **Partition to K Equal Sum Subsets** |
| **Pattern 3: Permutation / Arrangement** | Order matters (`[1,2]` $\neq$ `[2,1]`). Branches pick from all unused elements using a `visited` state or index mapping. | • **Permutations**<br>

<br>• **Permutations II**<br>

<br>• **Letter Combinations of a Phone Number** |
| **Pattern 4: Constraint Satisfaction, Grid & Partition** | Mutating grids, partitioning strings, building valid sequences under strict structural or board constraints. | • **Generate Parentheses**<br>

<br>• **Word Search**<br>

<br>• **Palindrome Partitioning**<br>

<br>• **N Queens**<br>

<br>• **N Queens II**<br>

<br>• **Word Break II** |

---

## Section 2: Deep Dive into the 4 Core Patterns (Code, Reason, & Line-by-Line Breakdown)

---

### Archetype 1: Subset Pattern (Inclusion / Exclusion Tree)

*Mapped Problems:* **Subsets**, **Subsets II**, **Sum of All Subsets XOR Total**

#### The Blueprint Solution (Subsets II - handling duplicates)

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums); // Step 1: Sort to group duplicates together
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] nums, int startIndex, List<Integer> currentPath, List<List<Integer>> result) {
    result.add(new ArrayList<>(currentPath)); // Every node in a subset tree is a valid subset
    
    for (int i = startIndex; i < nums.length; i++) {
        // Skip duplicates at the same tree depth
        if (i > startIndex && nums[i] == nums[i - 1]) {
            continue;
        }
        currentPath.add(nums[i]);                    // 1. Choose
        backtrack(nums, i + 1, currentPath, result); // 2. Explore (move index forward)
        currentPath.remove(currentPath.size() - 1);  // 3. Un-choose (Backtrack)
    }
}

```

#### Why Every Line Exists (The "Sleep Test" Explanation)

* `Arrays.sort(nums);`: Required for duplicate handling. Placing identical elements adjacent allows us to check `nums[i] == nums[i-1]`.
* `result.add(new ArrayList<>(currentPath));`: In subset generation, *every* node represents a valid answer, not just leaf nodes. We pass a **deep copy** because `currentPath` is mutated in place.
* `for (int i = startIndex; i < nums.length; i++)`: Iterates through available choices starting from `startIndex` to prevent generating reverse duplicate combinations (e.g., preventing `[2,1]` after `[1,2]`).
* `if (i > startIndex && nums[i] == nums[i-1]) continue;`: The duplicate pruning guard. If the current element matches the previous element *and* we are past the initial branch index (`startIndex`), skip it to avoid duplicate branches.
* `currentPath.add(nums[i])` & `remove(...)`: The classic state mutation and restoration (backtracking heartbeat).

*(Note: For **Sum of All Subsets XOR Total**, the structure is identical, but instead of adding paths to a list, you accumulate an integer XOR sum at every node and sum them up globally).*

---

### Archetype 2: Combination Pattern (Target-Driven Index Iteration)

*Mapped Problems:* **Combinations**, **Combination Sum**, **Combination Sum II**, **Matchsticks to Square**, **Partition to K Equal Sum Subsets**

#### The Blueprint Solution (Combination Sum II - Unique Combinations that sum to Target)

```java
public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    Arrays.sort(candidates); // Sort to enable duplicate skipping and early stopping
    List<List<Integer>> result = new ArrayList<>();
    backtrack(candidates, target, 0, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] candidates, int remain, int startIndex, List<Integer> path, List<List<Integer>> result) {
    if (remain < 0) return; // Base Case 1: Overshot target (Prune branch)
    if (remain == 0) {
        result.add(new ArrayList<>(path)); // Base Case 2: Exact target hit
        return;
    }
    
    for (int i = startIndex; i < candidates.length; i++) {
        if (i > startIndex && candidates[i] == candidates[i - 1]) continue; // Skip duplicates
        if (candidates[i] > remain) break; // Optimization: Since array is sorted, larger elements will also fail
        
        path.add(candidates[i]);
        backtrack(candidates, remain - candidates[i], i + 1, path, result); // i + 1 because each element can be used once
        path.remove(path.size() - 1);
    }
}

```

#### Why Every Line Exists

* `if (remain < 0) return;`: Negative budget means our path is invalid; terminate this branch immediately.
* `if (remain == 0)`: Exact budget match means we found a valid combination. Save a copy and return.
* `if (candidates[i] > remain) break;`: A performance optimization. Because the array is sorted, if `candidates[i]` exceeds the remaining target, all subsequent numbers will too, allowing us to `break` the loop entirely.
* `backtrack(..., i + 1, ...)`: Passing `i + 1` ensures elements are used only once (unlike standard Combination Sum where you pass `i` to allow element reuse).

---

### Archetype 3: Permutation Pattern (Visited Array / Order Matters)

*Mapped Problems:* **Permutations**, **Permutations II**, **Letter Combinations of a Phone Number**

#### The Blueprint Solution (Permutations II - Permutations with Duplicates)

```java
public List<List<Integer>> permuteUnique(int[] nums) {
    Arrays.sort(nums); // Group duplicates
    List<List<Integer>> result = new ArrayList<>();
    boolean[] visited = new boolean[nums.length];
    backtrack(nums, visited, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] nums, boolean[] visited, List<Integer> path, List<List<Integer>> result) {
    if (path.size() == nums.length) {
        result.add(new ArrayList<>(path));
        return;
    }
    
    for (int i = 0; i < nums.length; i++) {
        // If already visited, skip
        if (visited[i]) continue;
        
        // Duplicate handling: if current equals previous, and previous was NOT used in this branch, skip
        if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) continue;
        
        visited[i] = true;
        path.add(nums[i]);
        
        backtrack(nums, visited, path, result);
        
        path.remove(path.size() - 1);
        visited[i] = false; // Reset visited state (Backtracking cleanup)
    }
}

```

#### Why Every Line Exists

* `boolean[] visited`: Unlike combinations where we use `startIndex`, permutations start from index `0` every time because any unused element is a valid choice. We track usage via `visited`.
* `if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) continue;`: The golden rule for Permutations II. `!visited[i - 1]` ensures duplicate elements are processed in strict left-to-right order. If the duplicate to our left hasn't been used yet in the current branch, using the current one would create duplicate permutation branches.
* `visited[i] = false;`: Mandatory state cleanup alongside `path.remove(...)`. Both the path and the visitation marker must be restored.

---

### Archetype 4: Constraint Satisfaction, Grid & Partition Pattern

*Mapped Problems:* **Word Search**, **N Queens**, **N Queens II**, **Palindrome Partitioning**, **Generate Parentheses**, **Word Break II**

#### The Blueprint Solution (Word Search - 2D Grid DFS Backtracking)

```java
public boolean exist(char[][] board, String word) {
    for (int r = 0; r < board.length; r++) {
        for (int c = 0; c < board[0].length; c++) {
            if (dfs(board, word, r, c, 0)) return true;
        }
    }
    return false;
}

private boolean dfs(char[][] board, String word, int r, int c, int index) {
    // Base Case 1: Found all characters successfully
    if (index == word.length()) return true;
    
    // Base Case 2: Out of bounds or character mismatch
    if (r < 0 || c < 0 || r >= board.length || c >= board[0].length || board[r][c] != word.charAt(index)) {
        return false;
    }
    
    char temp = board[r][c];
    board[r][c] = '#'; // Mark as visited in-place
    
    // Explore 4 directions
    boolean found = dfs(board, word, r + 1, c, index + 1) ||
                    dfs(board, word, r - 1, c, index + 1) ||
                    dfs(board, word, r, c + 1, index + 1) ||
                    dfs(board, word, r, c - 1, index + 1);
                    
    board[r][c] = temp; // Un-choose: Restore cell state
    return found;
}

```

#### Why Every Line Exists

* `board[r][c] = '#';`: In-place state mutation. Instead of maintaining an external 2D boolean visited matrix, we temporarily mutate the board grid itself to prevent reusing the same cell in a single word path.
* `board[r][c] = temp;`: The restore step (backtracking). Once all 4 directions are explored, we put the original character back so other starting branches can use this cell.
* Short-circuiting `||`: Combines 4 directional recursive calls; if any direction returns `true`, execution stops and propagates success upward.

---

## Section 3: The Complete Problem-by-Problem Quick-Reference Matrix

| Problem Name | State Structure | Choice Space | Pruning / Base Case |
| --- | --- | --- | --- |
| **Subsets / Subsets II** | `List<Integer> path` | Elements from `startIndex` | `i > startIndex && nums[i] == nums[i-1]` |
| **Sum of All Subsets XOR Total** | `int currentXor`, `startIndex` | Elements from `startIndex` | Accumulate XOR sum at every node |
| **Combination Sum I / II** | `int remain`, `path` | Elements from `startIndex` | `remain < 0`, `remain == 0`, `candidates[i] > remain` |
| **Combinations** | `int k`, `int startIndex`, `path` | Elements from `startIndex` to $n$ | `path.size() == k` |
| **Permutations / II** | `boolean[] visited`, `path` | All elements from index `0` | `visited[i]`, duplicate skip via `!visited[i-1]` |
| **Letter Combinations of a Phone Number** | `int index`, `StringBuilder path` | Letters mapped to digit at `index` | `index == digits.length()` |
| **Generate Parentheses** | `int open`, `int close`, `String s` | Add `'('` or `')'` | `open < n`, `close < open` |
| **Word Search** | Grid coordinates `(r, c)`, `index` | 4 directions `(r±1, c±1)` | Out of bounds, char mismatch, cell marked `#` |
| **Palindrome Partitioning** | `String s`, `startIndex`, `path` | Substrings starting from `startIndex` | Check if substring is palindrome |
| **Matchsticks to Square / Partition to K Subsets** | `int bucketIndex`, `int currentSum`, `boolean[] visited` | Array elements across $k$ buckets | Sum exceeds target bucket size |
| **N Queens / N Queens II** | Row index, column/diagonal sets | Columns in current row | Diagonal/column conflict hashsets |
| **Word Break II** | `String s`, `int startIndex`, `List<String> path` | Valid dictionary word prefixes from `startIndex` | Reached end of string `startIndex == s.length()` |
