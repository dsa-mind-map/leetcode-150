# The ULTIMATE Trie Mastery Guide: From Zero to Expert

I'm going to teach you like a **World-Class Trie Expert** would. This is the definitive guide where you'll understand not just HOW to solve these problems, but WHY the pattern works.

---

## PRELUDE: The Universal Truth About Tries

Listen carefully:

> **A Trie is not a data structure for storing words. A Trie is a data structure for storing PATHS that lead to words. Every problem you'll encounter is just a different way of traversing those paths.**

This single insight will unlock everything.

---

# PART 1: THE TRIE FOUNDATION

## The Absolute Core (Memorize This)

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];  // a-z
    boolean isWord = false;                   // path ends here = word?
}
```



**That's it. Everything else is variations of this.**

---

## Why This Structure? The Philosophy

```
MENTAL MODEL:

Think of a Trie as a DECISION TREE at a RESTAURANT.

You walk in and see a menu board with 26 letters (a-z).
Each letter points to a hallway (TrieNode).
At the end of each hallway, you see another menu with 26 letters.
You keep walking, following letters.
When you see a ✓ mark (isWord=true), that's a VALID DISH NAME.
You can stop there and eat, or keep going to find longer dishes.

│                                                              │
│  The word "CAT" is BUILT by the PATH:                      │
│  ROOT → [follow index 2] → [follow index 0] → [follow index 19]
│          (represents 'c')   (represents 'a')   (represents 't')

Example: "CAT" restaurant

        START (ROOT)
         │
    See "C" on menu → Walk down C hallway
         │
        [C HALLWAY]
         │
    See "A" on menu → Walk down A hallway
         │
        [A HALLWAY]
         │
    See "T" on menu → Walk down T hallway
         │
        [T HALLWAY]
         │
    See ✓ mark → "CAT" is a valid dish!
         │
    See "S" on menu → We could go to "CATS"
         │
        [S HALLWAY]
         │
    See ✓ mark → "CATS" is also valid!
```

This is literally how a Trie works.

---

## The Critical Insight: Prefix Sharing

```
WITHOUT Trie (Array of strings):
    words = ["CAT", "CAR", "CAP", "DOG", "DOE"]
    
    Memory for each:
    "CAT" → C, A, T
    "CAR" → C, A, R  (C and A duplicated!)
    "CAP" → C, A, P  (C and A duplicated again!)
    "DOG" → D, O, G
    "DOE" → D, O, E  (D and O duplicated!)
    
    Total storage: Wasteful due to repeated prefixes

WITH Trie:
    
                ROOT
                 │
            ┌────┼────┐
            C    D    (others null)
            │    │
            A    O
           ╱│╲   ╱╲
          T R P G  E
          ✓ ✓ ✓ ✓  ✓
    
    Shared: "CA" (used by CAT, CAR, CAP)
    Shared: "DO" (used by DOG, DOE)
    Total storage: 7 nodes instead of repeated letters!
    
    THIS is the power of tries.
```

---

# PART 2: THE THREE FUNDAMENTAL OPERATIONS

## Operation 1: INSERT (The Foundation)

```java
void insert(String word) {
    TrieNode curr = root;
    
    for (char c : word.toCharArray()) {
        int idx = c - 'a';  // Convert 'a'→0, 'b'→1, ..., 'z'→25
        
        if (curr.children[idx] == null) {
            curr.children[idx] = new TrieNode();  // Create path if missing
        }
        
        curr = curr.children[idx];  // Move forward on the path
    }
    
    curr.isWord = true;  // Mark: A word ends here!
}
```

### Visual: What happens when you insert "CAT"

```
STEP 0: Start at root
        ┌────┐
        │ROOT│ curr here
        └────┘

STEP 1: Process 'C' (index 2)
        
        Is children[2] null? YES
        → Create new TrieNode
        → Point children[2] to it
        → Move curr to this new node
        
        ┌────┐
        │ROOT│
        └─┬──┘
          │ children[2]='C'
          ▼
        ┌────┐
        │ C  │ curr here
        └────┘

STEP 2: Process 'A' (index 0)
        
        Is children[0] null? YES
        → Create new TrieNode
        → Point children[0] to it
        → Move curr to this new node
        
        ┌────┐
        │ROOT│
        └─┬──┘
          │
          ▼
        ┌────┐
        │ C  │
        └─┬──┘
          │ children[0]='A'
          ▼
        ┌────┐
        │ A  │ curr here
        └────┘

STEP 3: Process 'T' (index 19)
        
        Is children[19] null? YES
        → Create new TrieNode
        → Point children[19] to it
        → Move curr to this new node
        
        ┌────┐
        │ROOT│
        └─┬──┘
          │
          ▼
        ┌────┐
        │ C  │
        └─┬──┘
          │
          ▼
        ┌────┐
        │ A  │
        └─┬──┘
          │ children[19]='T'
          ▼
        ┌────────┐
        │ T      │ curr here
        │isWord: │
        │TRUE ✓  │
        └────────┘

STEP 4: Mark terminal
        curr.isWord = true
```

**The Golden Rule:** Every insertion creates exactly as many new nodes as needed. If a prefix already exists, you REUSE those nodes.

---

## Operation 2: TRAVERSE Helper (The Navigator)

```java
TrieNode traverse(String s) {
    TrieNode curr = root;
    
    for (char c : s.toCharArray()) {
        int idx = c - 'a';
        
        if (curr.children[idx] == null) {
            return null;  // Path doesn't exist, STOP
        }
        
        curr = curr.children[idx];  // Follow the path
    }
    
    return curr;  // Return where we ended up
}
```

### Visual: What happens when you traverse "CAT"

```
START: curr = root

char='C' (idx=2):
  ┌────┐
  │ROOT│
  └─┬──┘ Is children[2] null? NO ✓
    │
    ▼
  ┌────┐
  │ C  │ curr here

char='A' (idx=0):
  ┌────┐
  │ROOT│
  └─┬──┘
    │
    ▼
  ┌────┐
  │ C  │
  └─┬──┘ Is children[0] null? NO ✓
    │
    ▼
  ┌────┐
  │ A  │ curr here

char='T' (idx=19):
  ┌────┐
  │ROOT│
  └─┬──┘
    │
    ▼
  ┌────┐
  │ C  │
  └─┬──┘
    │
    ▼
  ┌────┐
  │ A  │
  └─┬──┘ Is children[19] null? NO ✓
    │
    ▼
  ┌────────┐
  │ T      │ curr here
  │isWord: │
  │TRUE ✓  │
  └────────┘

RETURN: T node
```

**Key insight:** If ANY character's path doesn't exist, return null immediately. The traversal is LINEAR — O(word length).

---

## Operation 3: QUERY (The Verdict)

This is where the MAGIC happens. The terminal condition changes per problem, but the pattern is always the same.

```java
// PATTERN:
// 1. Traverse to the node
// 2. Check a condition at that node
// 3. Return the verdict

// Different problems = different conditions
```

---

# PART 3: THE FOUR PROBLEMS & THEIR PATTERNS

## PROBLEM 1: Implement Trie (Basic Prefix Tree)

**What does it ask?**
- `search(word)` → Did I insert this EXACT word?
- `startsWith(prefix)` → Can words start with this prefix?

**The Pattern:**
```
search(word):
  1. Traverse to the end of 'word'
  2. Check: Does the node exist AND isWord == true?
  3. Return both conditions

startsWith(prefix):
  1. Traverse to the end of 'prefix'
  2. Check: Does the node exist? (ignore isWord)
  3. Return true if node exists
```

### Implementation

```java
class PrefixTree {
    TrieNode root = new TrieNode();
    
    void insert(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) {
                curr.children[idx] = new TrieNode();
            }
            curr = curr.children[idx];
        }
        curr.isWord = true;
    }
    
    boolean search(String word) {
        TrieNode node = traverse(word);
        return node != null && node.isWord;  // ← The condition!
    }
    
    boolean startsWith(String prefix) {
        TrieNode node = traverse(prefix);
        return node != null;  // ← Different condition!
    }
    
    private TrieNode traverse(String s) {
        TrieNode curr = root;
        for (char c : s.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) return null;
            curr = curr.children[idx];
        }
        return curr;
    }
}
```

### Visual: The Difference

```
Trie state after inserting "CAT" and "CAR":

                ROOT
                 │
                 C
                 │
                 A
                ╱ ╲
               T   R
              ✓    ✓
          (isWord) (isWord)

─────────────────────────────────────────

search("CAT"):
  traverse("CAT") → returns T node
  T != null? YES
  T.isWord? YES ✓
  Result: TRUE

search("CA"):
  traverse("CA") → returns A node
  A != null? YES
  A.isWord? NO ✗
  Result: FALSE ← Word wasn't inserted!

startsWith("CA"):
  traverse("CA") → returns A node
  A != null? YES ✓
  Result: TRUE ← Prefix exists!

startsWith("CAN"):
  traverse("CAN") → can't find N under A → returns null
  null != null? NO
  Result: FALSE ← Prefix doesn't exist!
```

**Key Learning:** The ONLY difference is checking `isWord`. Everything else is identical structure!

---

# PROBLEM 2: Design Add & Search Words Data Structure (with `.`)

## The Game Changer: The Wildcard

**What does it ask?**
- `addWord(word)` → Insert word (same as before)
- `search(word)` → Find word, but `.` means ANY letter

**Example:**
```
After adding "CAT", "CAR", "DOG"
search("C.T") → TRUE (matches CAT)
search("C.R") → TRUE (matches CAR)
search("D.G") → TRUE (matches DOG)
search(".AT") → TRUE (matches CAT)
search("...") → TRUE (matches any 3-letter word)
```

---

## Why traverse() Fails Here

```
PROBLEM with traverse():

search("C.T")
  traverse("C.T"):
    char='C': go to C node ✓
    char='.': WHAT DO WE DO?
    
    '.' means "any letter" (a-z)
    But traverse() only follows ONE path!
    
    We need to try ALL 26 paths!
    ✗ traverse() can't do this
```

**Solution:** Use **DFS (Depth-First Search)** to explore all branches when we hit `.`

---

## The Pattern: DFS on Trie

```
KEY INSIGHT:
  
  Normal characters → follow one path (like traverse)
  '.' wildcard      → try all 26 paths and recurse
  
  This requires DFS because we need to backtrack and try other branches.
```

### Implementation

```java
class WordDictionary {
    TrieNode root = new TrieNode();
    
    void addWord(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) {
                curr.children[idx] = new TrieNode();
            }
            curr = curr.children[idx];
        }
        curr.isWord = true;
    }
    
    boolean search(String word) {
        return dfs(word, 0, root);
    }
    
    private boolean dfs(String word, int idx, TrieNode node) {
        // BASE CASE: Reached end of word
        if (idx == word.length()) {
            return node.isWord;
        }
        
        char c = word.charAt(idx);
        
        if (c == '.') {
            // WILDCARD: Try all 26 letters
            for (TrieNode child : node.children) {
                if (child != null && dfs(word, idx + 1, child)) {
                    return true;  // Found a valid path!
                }
            }
            return false;  // No valid path found
        } else {
            // NORMAL: Follow one path
            int charIdx = c - 'a';
            if (node.children[charIdx] == null) {
                return false;  // Path doesn't exist
            }
            return dfs(word, idx + 1, node.children[charIdx]);
        }
    }
}
```

---

## Visual: DFS Call Stack for "C.T" with Trie ["CAT", "CAR", "DOG"]

```
Trie:
        ROOT
       /    \
      C      D
      │      │
      A      O
     / \     │
    T   R    G
    ✓   ✓    ✓

─────────────────────────────────────────────────────────

search("C.T") call tree:

                    dfs("C.T", 0, ROOT)
                            │
                    char='C' (not wildcard)
                    children[2] exists? YES
                            │
                    dfs("C.T", 1, C_node)
                            │
                    char='.' (WILDCARD!)
                            │
            ┌───────────────┼───────────────┐
            │               │               │
        Loop children:   Loop children:   Loop children:
        A node ✓         (null)           (null)
            │
        dfs("C.T", 2, A_node)
            │
        char='T'
        children[19] exists? YES
            │
        dfs("C.T", 3, T_node)
            │
        idx == 3 == word.length() ✓
        return T_node.isWord = TRUE ✓

RESULT: TRUE
```

---

## Critical Comparison: Problem 1 vs Problem 2

```
PROBLEM 1: search("CAT")          PROBLEM 2: search("C.T")
───────────────────────────────    ────────────────────────

traverse("CAT")                    dfs("C.T", 0, root)
  C_path ✓ →→                        C_path ✓ →→
  A_path ✓ →→→                       . = wildcard
  T_path ✓ →→→→                      loop A: T_path ✓
  return node                        loop B: null
                                     loop ...
                                     return true

Single linear path              Multiple branching paths
O(m) where m=length            O(26^m) worst case
                                (but pruned by trie structure)
```

---

## The DFS Pattern Rule

```
REMEMBER THIS:

When traversing a Trie:
  
  1. If character is FIXED (a-z)
     → Follow that one children slot
     
  2. If character is WILDCARD ('.')
     → Loop through all 26 children
     → Recursively call dfs() on each non-null child
     
  3. Base case: idx reaches word length
     → Check if current node is a valid word
```

---

# PROBLEM 3: Extra Characters in a String

## What Does It Ask?

**Given:**
- A string `s`
- A list of dictionary `words`

**Find:**
- Minimum number of characters in `s` that CANNOT be formed by concatenating words from `words`

**Example:**
```
s = "leetscode"
dictionary = ["leet", "code", "leetcode"]

"leetcode" uses "leet" + "code" (all 9 chars used)
Remaining: "s" = 1 character
Answer: 1

─────────────────────────────────

s = "sayhelloworld"
dictionary = ["say", "hello"]

"say" + "hello" = "sayhello" (8 chars used)
Remaining: "world" = 5 characters
Answer: 5
```

---

## Why Trie Here? The Insight

```
NAIVE APPROACH (slow):
  
  For each position i:
    For each position j before i:
      Check if s[j:i] is in dictionary
      Use: dictionary.contains(substring)
      
  Problem: String.substring() creates new strings
           dictionary.contains() does string comparison
           Multiple substrings = SLOW ✗

TRIE APPROACH (fast):
  
  For each position i:
    For each position j before i:
      Walk the trie backwards from i to j
      At each step, check if trie node exists
      If not, PRUNE immediately (no more words possible)
      If yes but not isWord, keep going
      
  Benefit: Trie REJECTS invalid paths early
           No substring creation needed
           No string comparison needed ✓
```

---

## The DP + Trie Pattern

```
KEY INSIGHT:

This is DP on substrings, where TRIE is the lookup mechanism.

DP[i] = minimum extra characters in s[0:i]

Recurrence:
  dp[i] = min(
    dp[i-1] + 1,                    // Take char i as extra
    dp[j] for all valid j           // If s[j:i] is a word
  )

We use TRIE to find all valid j's efficiently.
```

---

## Implementation

```java
int minimumExtraCharacters(String s, String[] dictionary) {
    // Build trie from dictionary
    TrieNode root = new TrieNode();
    for (String word : dictionary) {
        insert(word, root);
    }
    
    int n = s.length();
    int[] dp = new int[n + 1];
    
    // dp[i] = minimum extra chars in s[0:i]
    
    for (int i = 1; i <= n; i++) {
        // Option 1: Take character at i-1 as extra
        dp[i] = dp[i - 1] + 1;
        
        // Option 2: Try to match backwards using trie
        TrieNode curr = root;
        
        // Walk backwards from position i-1 to 0
        for (int j = i - 1; j >= 0; j--) {
            char c = s.charAt(j);
            int idx = c - 'a';
            
            // Try to follow trie path
            if (curr.children[idx] == null) {
                break;  // Dead end in trie, can't extend further back
            }
            
            curr = curr.children[idx];
            
            // If we found a word, update dp
            if (curr.isWord) {
                // s[j:i] is a valid word
                dp[i] = Math.min(dp[i], dp[j]);
            }
        }
    }
    
    return dp[n];
}

private void insert(String word, TrieNode root) {
    TrieNode curr = root;
    for (char c : word.toCharArray()) {
        int idx = c - 'a';
        if (curr.children[idx] == null) {
            curr.children[idx] = new TrieNode();
        }
        curr = curr.children[idx];
    }
    curr.isWord = true;
}
```

---

## Visual: DP + Trie for "leetscode" with ["leet", "code", "leetcode"]

```
Trie:
    ROOT
    ├─ l
    │  └─ e
    │     └─ e
    │        └─ t ✓ ("leet")
    │           └─ c
    │              └─ o
    │                 └─ d
    │                    └─ e ✓ ("leetcode")
    └─ c
       └─ o
          └─ d
             └─ e ✓ ("code")

─────────────────────────────────────────────────────────

s = "leetscode"
     0123456789

DP computation:

dp[0] = 0 (empty string, 0 extra chars)

i=1, s[0]='l':
  dp[1] = dp[0] + 1 = 1
  Walk trie backwards: j=0, 'l'
    curr.children[11] = 'l' exists ✓
    curr = l_node, isWord? NO
  Result: dp[1] = 1

i=2, s[1]='e':
  dp[2] = dp[1] + 1 = 2
  Walk trie backwards: j=1, 'e'
    curr.children[4] = 'e' exists ✓
    curr = e_node, isWord? NO
  j=0, 'l'
    curr.children[11] = 'l' exists ✓
    (building "le"), isWord? NO
  Result: dp[2] = 2

i=3, s[2]='e':
  dp[3] = dp[2] + 1 = 3
  Walk trie backwards: j=2, 'e'
    curr.children[4] = 'e' exists ✓
    (building "ee"), isWord? NO
  j=1, 'e'
    curr.children[4] = 'e' exists ✓
    (building "ee"), isWord? NO
  j=0, '

## PROBLEM 2: Design Add & Search Words with

