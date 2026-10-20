# Binary Tree Master Reference & Implementation Guide

A production-grade, high-density reference manual covering fundamental tree data structures, mental traversal models, structural property invariants, and level-order/projection variations.

---

## 📋 Navigation Index

* [1. TreeNode Class Definition](https://www.google.com/search?q=%231-treenode-class-definition)
* [2. Recursive & Iterative Traversals](https://www.google.com/search?q=%232-recursive--iterative-traversals)
        * [Permanent Mental Anchors](https://www.google.com/search?q=%23permanent-mental-anchors)
        * [A. Recursive Pre-Order Traversal](https://www.google.com/search?q=%23a-recursive-pre-order-traversal)
        * [B. Recursive In-Order Traversal](https://www.google.com/search?q=%23b-recursive-in-order-traversal)
        * [C. Recursive Post-Order Traversal](https://www.google.com/search?q=%23c-recursive-post-order-traversal)
        * [D. Iterative Pre-Order Traversal](https://www.google.com/search?q=%23d-iterative-pre-order-traversal)
        * [E. Iterative In-Order Traversal](https://www.google.com/search?q=%23e-iterative-in-order-traversal)
        * [F. Iterative Post-Order Traversal (2-Stack Approach)](https://www.google.com/search?q=%23f-iterative-post-order-traversal-2-stack-approach)
        * [G. Iterative Post-Order Traversal (1-Stack with Tracking)](https://www.google.com/search?q=%23g-iterative-post-order-traversal-1-stack-with-tracking)


* [3. Tree Properties & Structural Checks (DFS)](https://www.google.com/search?q=%233-tree-properties--structural-checks-dfs)
        * [A. Find Height of a Binary Tree](https://www.google.com/search?q=%23a-find-height-of-a-binary-tree)
        * [B. Find Size of a Binary Tree](https://www.google.com/search?q=%23b-find-size-of-a-binary-tree)
        * [C. Find Node in Binary Tree](https://www.google.com/search?q=%23c-find-node-in-binary-tree)
        * [D. Check If Two Binary Trees Are Equal](https://www.google.com/search?q=%23d-check-if-two-binary-trees-are-equal)
        * [E. Maximum Depth of Binary Tree](https://www.google.com/search?q=%23e-maximum-depth-of-binary-tree)
        * [F. Invert Binary Tree](https://www.google.com/search?q=%23f-invert-binary-tree)


* [4. BFS (Breadth-First Search) & Level Order Variations](https://www.google.com/search?q=%234-bfs-breadth-first-search--level-order-variations)
        * [A. Level-by-Level Core Variations](https://www.google.com/search?q=%23a-level-by-level-core-variations)
        * [1. Standard Level Order Traversal](https://www.google.com/search?q=%231-standard-level-order-traversal)
        * [2. Reverse Level Order Traversal (Bottom-Up)](https://www.google.com/search?q=%232-reverse-level-order-traversal-bottom-up)
        * [3. Zigzag (Spiral) Level Order Traversal](https://www.google.com/search?q=%233-zigzag-spiral-level-order-traversal)


* [B. Vertical Column Projections](https://www.google.com/search?q=%23b-vertical-column-projections)
        * [1. Binary Tree Vertical Order Traversal ($\mathcal{O}(N)$ Optimal)](https://www.google.com/search?q=%231-binary-tree-vertical-order-traversal-on-optimal)
        * [2. Top View of Binary Tree](https://www.google.com/search?q=%232-top-view-of-binary-tree)
        * [3. Bottom View of Binary Tree](https://www.google.com/search?q=%233-bottom-view-of-binary-tree)


* [C. Boundary & Side Views](https://www.google.com/search?q=%23c-boundary--side-views)
        * [1. Left View of Binary Tree](https://www.google.com/search?q=%231-left-view-of-binary-tree)
        * [2. Right View of Binary Tree](https://www.google.com/search?q=%232-right-view-of-binary-tree)





---

## 1. TreeNode Class Definition

```java
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;

    public TreeNode() {}
    public TreeNode(int val) { 
        this.val = val; 
    }
    public TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

```

---

## 2. Recursive & Iterative Traversals

### Permanent Mental Anchors

> 1. **Pre-Push vs. No Pre-Push Axiom (Deferred Work):**
> * **Root-First Traversals (Push Before `while`):** Process root *immediately* upon seeing it (never deferred). Stack acts purely as a work-backlog (`while (!stack.isEmpty())`). *(Pre-Order, 2-Stack Post-Order)*
> * **Left-First Traversals (No Push Before `while`):** Cannot process root yet (left subtree must come first). Pointer `curr` drives loop entry (`while (curr != null || !stack.isEmpty())`) and stashes parents dynamically as it dives down the left spine. *(In-Order, 1-Stack Post-Order)*
> 
> 
> 2. **Push Order Rule (Stack LIFO Property):**
> * To process child $A$ before child $B$, you must push child $B$ **before** child $A$.
> 
> 
> 
> 

---

### A. Recursive Pre-Order Traversal

* **Order:** Root $\rightarrow$ Left $\rightarrow$ Right
* **Concept:** Process current root, then recursively traverse left and right subtrees.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$ stack space

```java
import java.util.ArrayList;
import java.util.List;

public class RecursivePreOrder {

    public List<Integer> preorderRecursive(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        helper(root, result);
        return result;
    }

    private void helper(TreeNode root, List<Integer> result) {
        if (root == null) return;
        
        result.add(root.val);
        helper(root.left, result);
        helper(root.right, result);
    }
}

```

---

### B. Recursive In-Order Traversal

* **Order:** Left $\rightarrow$ Root $\rightarrow$ Right
* **Concept:** Traverse leftmost path fully, process parent, then traverse right subtree.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$ stack space

```java
import java.util.ArrayList;
import java.util.List;

public class RecursiveInOrder {

    public List<Integer> inorderRecursive(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        helper(root, result);
        return result;
    }

    private void helper(TreeNode root, List<Integer> result) {
        if (root == null) return;
        
        helper(root.left, result);
        result.add(root.val);
        helper(root.right, result);
    }
}

```

---

### C. Recursive Post-Order Traversal

* **Order:** Left $\rightarrow$ Right $\rightarrow$ Root
* **Concept:** Process both subtrees bottom-up before processing parent root node.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$ stack space

```java
import java.util.ArrayList;
import java.util.List;

public class RecursivePostOrder {

    public List<Integer> postorderRecursive(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        helper(root, result);
        return result;
    }

    private void helper(TreeNode root, List<Integer> result) {
        if (root == null) return;
        
        helper(root.left, result);
        helper(root.right, result);
        result.add(root.val);
    }
}

```

---

### D. Iterative Pre-Order Traversal

* **Strategy:** Explicit LIFO Stack (Work List Model)
* **Mental Rule:**
* **Push Before `while`:** Process Root immediately; stack holds work-backlog.
* **Push Order:** Push **Right** child first, then **Left** child $\rightarrow$ Left sits on TOP of stack to be popped and processed first ($\text{Root} \rightarrow \text{Left} \rightarrow \text{Right}$).


* **Concept:** Pop top, process root, push right then left child (ensures left popped first).
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$ explicit stack space

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class IterativePreOrder {

    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Stack<TreeNode> stack = new Stack<>();
        stack.push(root); // PRE-PUSH: Stack holds initial work item

        while (!stack.isEmpty()) {
            TreeNode top = stack.pop();
            result.add(top.val);
            
            // Push Right first so Left sits on top (LIFO)
            if (top.right != null) stack.push(top.right);
            if (top.left != null) stack.push(top.left);
        }
        return result;
    }
}

```

---

### E. Iterative In-Order Traversal

* **Strategy:** Explicit Stack with `curr` Pointer (Pointer Navigation Model)
* **Mental Rule:** **No Push Before `while**` — Cannot process root yet; `curr` drives loop entry and dives left while stashing parents on stack.
* **Concept:** Push all left nodes, pop/process top, then pivot to right subtree.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$ explicit stack space

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class IterativeInOrder {

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root; // NO PRE-PUSH: Pointer drives loop entry

        while (curr != null || !stack.isEmpty()) {
            if (curr != null) {
                stack.push(curr);
                curr = curr.left;
            } else {
                TreeNode top = stack.pop();
                result.add(top.val);
                curr = top.right;
            }
        }
        return result;
    }
}

```

---

### F. Iterative Post-Order Traversal (2-Stack Approach)

* **Strategy:** Inverted Pre-Order (Work List Model)
* **Mental Rule:**
* **Push Before `while`:** Primary stack starts with root to drive $\text{Root} \rightarrow \text{Right} \rightarrow \text{Left}$ processing into `stack2`.
* **Push Order:** Push **Left** child first, then **Right** child into `stack1` $\rightarrow$ Right sits on TOP of `stack1`, gets popped and pushed into `stack2` first. Popping `stack2` at the end reverses it to $\text{Left} \rightarrow \text{Right} \rightarrow \text{Root}$.


* **Concept:** Traverse Root-Right-Left into `stack1`, push to `stack2` to reverse order to Left-Right-Root.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(N)$ stack space

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class IterativePostOrderTwoStacks {

    public List<Integer> postorderTraversal2Stacks(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();

        stack1.push(root); // PRE-PUSH: Stack1 starts with root
        while (!stack1.isEmpty()) {
            TreeNode top = stack1.pop();
            stack2.push(top);

            // Push Left first so Right sits on top of stack1
            if (top.left != null) stack1.push(top.left);
            if (top.right != null) stack1.push(top.right);
        }

        while (!stack2.isEmpty()) {
            result.add(stack2.pop().val);
        }

        return result;
    }
}

```

---

### G. Iterative Post-Order Traversal (1-Stack with Tracking)

* **Strategy:** Single Stack with `lastVisited` Pointer (Pointer Navigation Model)
* **Mental Rule:** **No Push Before `while**` — `current` drives left dive; stack evaluates right child on ascent.
* **Concept:** Go left; inspect `peek().right`—pivot if unvisited, else pop and mark visited.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$ optimal stack space

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class IterativePostOrderOneStack {

    public List<Integer> postorderTraversal1Stack(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root; // NO PRE-PUSH: Pointer drives loop entry

        while (current != null || !stack.isEmpty()) {
            if (current != null) {
                stack.push(current);
                current = current.left;
            } else {
                TreeNode peekNode = stack.peek();
                
                if (peekNode.right != null && current != peekNode.right) {
                    current = peekNode.right;
                } else {
                    TreeNode visitedNode = stack.pop();
                    result.add(visitedNode.val);
                    current = visitedNode;
                }
            }
        }
        return result;
    }
}

```

---

## 3. Tree Properties & Structural Checks (DFS)

### A. Find Height of a Binary Tree

* **Convention:** Edges path count ($\text{single node} = 0$).
* **Base Case:** `null` $\rightarrow$ `-1`.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$

```java
public int height(TreeNode root) {
    if (root == null) return -1;
    return Math.max(height(root.left), height(root.right)) + 1;
}

```

---

### B. Find Size of a Binary Tree

* **Formula:** $\text{Left Size} + \text{Right Size} + 1$.
* **Base Case:** `null` $\rightarrow$ `0`.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$

```java
public int findSize(TreeNode root) {
    if (root == null) return 0;
    return findSize(root.left) + findSize(root.right) + 1;
}

```

---

### C. Find Node in Binary Tree

* **Strategy:** Short-circuit `||` search.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$

```java
public boolean findNode(TreeNode root, int target) {
    if (root == null) return false;
    if (root.val == target) return true;
    return findNode(root.left, target) || findNode(root.right, target);
}

```

---

### D. Check If Two Binary Trees Are Equal

* **Invariants:** Match nullability $\rightarrow$ Match `val` $\rightarrow$ Recurse children with `&&`.
* **Time/Space:** $\mathcal{O}(\min(N_1, N_2))$ / $\mathcal{O}(\min(H_1, H_2))$

```java
public boolean areEqual(TreeNode root1, TreeNode root2) {
    if (root1 == null && root2 == null) return true;
    if (root1 == null || root2 == null) return false;
    if (root1.val != root2.val) return false;
    return areEqual(root1.left, root2.left) && areEqual(root1.right, root2.right);
}

```

---

### E. Maximum Depth of Binary Tree

* **Convention:** Node count ($\text{single node} = 1$).
* **Base Case:** `null` $\rightarrow$ `0`.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}

```

---

### F. Invert Binary Tree

#### 1. DFS (Recursive)

* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(H)$

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        TreeNode left = root.left, right = root.right;
        root.left = right;
        root.right = left;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}

```

#### 2. BFS (Iterative Queue)

* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(W)$

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode curr = queue.poll();
            TreeNode left = curr.left, right = curr.right;
            curr.left = right;
            curr.right = left;
            if (curr.left != null) queue.offer(curr.left);
            if (curr.right != null) queue.offer(curr.right);
        }
        return root;
    }
}

```

---

## 4. BFS (Breadth-First Search) & Level Order Variations

> **Universal Level-Size Paradigm:** Always capture `int levelSize = queue.size()` at the start of the outer `while` loop to process nodes level-by-level cleanly in $\mathcal{O}(N)$ Time and $\mathcal{O}(W)$ Space without sentinel markers.

---

### A. Level-by-Level Core Variations

#### 1. Standard Level Order Traversal

* **Strategy:** Standard FIFO Queue with level-size iteration.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(W)$

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) return res;

    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        int levelSize = q.size();
        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < levelSize; i++) {
            TreeNode curr = q.poll();
            level.add(curr.val);

            if (curr.left != null) q.offer(curr.left);
            if (curr.right != null) q.offer(curr.right);
        }
        res.add(level);
    }
    return res;
}

```

---

#### 2. Reverse Level Order Traversal (Bottom-Up)

* **Strategy:** Queue for standard top-down BFS $\rightarrow$ Prepending (`LinkedList.addFirst`) to output `L_bottom -> L_top`.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(W)$

```java
public List<List<Integer>> levelOrderBottom(TreeNode root) {
    LinkedList<List<Integer>> res = new LinkedList<>();
    if (root == null) return res;

    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        int levelSize = q.size();
        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < levelSize; i++) {
            TreeNode curr = q.poll();
            level.add(curr.val);

            if (curr.left != null) q.offer(curr.left);
            if (curr.right != null) q.offer(curr.right);
        }
        res.addFirst(level); // Prepend to reverse level ordering
    }
    return res;
}

```

---

#### 3. Zigzag (Spiral) Level Order Traversal

##### Solution 1: Queue + Direction Flag (LeetCode Standard)

* **Strategy:** Standard BFS queue with `LinkedList.addFirst()` toggling based on level index parity.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(W)$

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) return res;

    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    boolean leftToRight = true;

    while (!q.isEmpty()) {
        int levelSize = q.size();
        LinkedList<Integer> level = new LinkedList<>();

        for (int i = 0; i < levelSize; i++) {
            TreeNode curr = q.poll();
            
            if (leftToRight) level.addLast(curr.val);
            else level.addFirst(curr.val);

            if (curr.left != null) q.offer(curr.left);
            if (curr.right != null) q.offer(curr.right);
        }
        res.add(level);
        leftToRight = !leftToRight;
    }
    return res;
}

```

##### Solution 2: Dual Stacks (Alternate Approach)

* **Strategy:** Two alternating stacks (`s1` for Left $\rightarrow$ Right levels, `s2` for Right $\rightarrow$ Left levels).
* `s1` pops node $\rightarrow$ pushes `left` then `right` child to `s2`.
* `s2` pops node $\rightarrow$ pushes `right` then `left` child to `s1`.


* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(W)$

```java
public List<List<Integer>> zigzagLevelOrderDualStack(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) return res;

    Stack<TreeNode> s1 = new Stack<>(); // Left -> Right level
    Stack<TreeNode> s2 = new Stack<>(); // Right -> Left level
    s1.push(root);

    while (!s1.isEmpty() || !s2.isEmpty()) {
        List<Integer> level1 = new ArrayList<>();
        while (!s1.isEmpty()) {
            TreeNode curr = s1.pop();
            level1.add(curr.val);
            if (curr.left != null) s2.push(curr.left);
            if (curr.right != null) s2.push(curr.right);
        }
        if (!level1.isEmpty()) res.add(level1);

        List<Integer> level2 = new ArrayList<>();
        while (!s2.isEmpty()) {
            TreeNode curr = s2.pop();
            level2.add(curr.val);
            if (curr.right != null) s1.push(curr.right);
            if (curr.left != null) s1.push(curr.left);
        }
        if (!level2.isEmpty()) res.add(level2);
    }
    return res;
}

```

---

### B. Vertical Column Projections

#### 1. Binary Tree Vertical Order Traversal ($\mathcal{O}(N)$ Optimal)

* **Strategy:** BFS with Queue tracking `Pair(node, col)` + `HashMap` + dynamic `minCol`/`maxCol` bounds (avoids $\mathcal{O}(N \log N)$ sorting of `TreeMap`).
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(N)$

```java
public List<List<Integer>> verticalOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) return res;

    Map<Integer, List<Integer>> map = new HashMap<>();
    Queue<Pair> q = new LinkedList<>();
    q.offer(new Pair(root, 0));
    int minCol = 0, maxCol = 0;

    while (!q.isEmpty()) {
        Pair p = q.poll();
        TreeNode node = p.node;
        int col = p.col;

        map.computeIfAbsent(col, k -> new ArrayList<>()).add(node.val);
        minCol = Math.min(minCol, col);
        maxCol = Math.max(maxCol, col);

        if (node.left != null) q.offer(new Pair(node.left, col - 1));
        if (node.right != null) q.offer(new Pair(node.right, col + 1));
    }

    for (int c = minCol; c <= maxCol; c++) {
        res.add(map.get(c));
    }
    return res;
}

private static class Pair {
    TreeNode node;
    int col;
    Pair(TreeNode node, int col) {
        this.node = node;
        this.col = col;
    }
}

```

---

#### 2. Top View of Binary Tree

* **Strategy:** BFS + `HashMap` insertion **only if column absent** (`!map.containsKey(col)`).
* **Invariant:** BFS processes top-to-bottom; first node seen at any column is the topmost.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(N)$

```java
public List<Integer> topView(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;

    Map<Integer, Integer> map = new HashMap<>();
    Queue<Pair> q = new LinkedList<>();
    q.offer(new Pair(root, 0));
    int minCol = 0, maxCol = 0;

    while (!q.isEmpty()) {
        Pair p = q.poll();
        TreeNode node = p.node;
        int col = p.col;

        if (!map.containsKey(col)) {
            map.put(col, node.val); // Store ONLY first node encountered
        }

        minCol = Math.min(minCol, col);
        maxCol = Math.max(maxCol, col);

        if (node.left != null) q.offer(new Pair(node.left, col - 1));
        if (node.right != null) q.offer(new Pair(node.right, col + 1));
    }

    for (int c = minCol; c <= maxCol; c++) {
        res.add(map.get(c));
    }
    return res;
}

```

---

#### 3. Bottom View of Binary Tree

* **Strategy:** BFS + `HashMap` **unconditional overwrite** (`map.put(col, node.val)`).
* **Invariant:** BFS processes top-to-bottom; last node written at any column is the bottom-most.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(N)$

```java
public List<Integer> bottomView(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;

    Map<Integer, Integer> map = new HashMap<>();
    Queue<Pair> q = new LinkedList<>();
    q.offer(new Pair(root, 0));
    int minCol = 0, maxCol = 0;

    while (!q.isEmpty()) {
        Pair p = q.poll();
        TreeNode node = p.node;
        int col = p.col;

        map.put(col, node.val); // Overwrite continuously to capture deepest node

        minCol = Math.min(minCol, col);
        maxCol = Math.max(maxCol, col);

        if (node.left != null) q.offer(new Pair(node.left, col - 1));
        if (node.right != null) q.offer(new Pair(node.right, col + 1));
    }

    for (int c = minCol; c <= maxCol; c++) {
        res.add(map.get(c));
    }
    return res;
}

```

---

### C. Boundary & Side Views

#### 1. Left View of Binary Tree

* **Strategy:** Level-size iteration $\rightarrow$ capture element when index `i == 0`.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(W)$

```java
public List<Integer> leftSideView(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;

    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        int levelSize = q.size();

        for (int i = 0; i < levelSize; i++) {
            TreeNode curr = q.poll();

            if (i == 0) res.add(curr.val); // First node of current level

            if (curr.left != null) q.offer(curr.left);
            if (curr.right != null) q.offer(curr.right);
        }
    }
    return res;
}

```

---

#### 2. Right View of Binary Tree

* **Strategy:** Level-size iteration $\rightarrow$ capture element when index `i == levelSize - 1`.
* **Time/Space:** $\mathcal{O}(N)$ / $\mathcal{O}(W)$

```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;

    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        int levelSize = q.size();

        for (int i = 0; i < levelSize; i++) {
            TreeNode curr = q.poll();

            if (i == levelSize - 1) res.add(curr.val); // Last node of current level

            if (curr.left != null) q.offer(curr.left);
            if (curr.right != null) q.offer(curr.right);
        }
    }
    return res;
}

```
