# Binary Tree Master Reference & Implementation Guide

A production-grade, high-density reference manual covering fundamental tree data structures, mental traversal models, structural property invariants, and level-order/projection variations.

---

## 🗺️ Master Problem-to-Pattern Taxonomy

| Pattern Group | Problems Included | Core Algorithmic Technique | Top 1% Expert One-Liner Analysis |
| :--- | :--- | :--- | :--- |
| **1. Linear Traversals & Core Operations** | • [TreeNode Class Definition](#treenode-class-definition)<br>• [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)<br>• [Binary Tree Preorder Traversal](#binary-tree-preorder-traversal)<br>• [Binary Tree Postorder Traversal](#binary-tree-postorder-traversal)<br>• [Find Height of a Binary Tree](#find-height-of-a-binary-tree)<br>• [Find Size of a Binary Tree](#find-size-of-a-binary-tree)<br>• [Find Node in Binary Tree](#find-node-in-binary-tree) | Recursion / Explicit Stack / Simple DFS | *Control execution order by decoupling when you visit the root relative to its subtrees or traversing to aggregate structural scalar counts.* |
| **2. Structural DFS (Bottom-Up Accumulation)** | • [Invert Binary Tree](#invert-binary-tree)<br>• [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)<br>• [Diameter of Binary Tree](#diameter-of-binary-tree)<br>• [Balanced Binary Tree](#balanced-binary-tree)<br>• [Same Tree](#same-tree)<br>• [Subtree of Another Tree](#subtree-of-another-tree) | Post-Order DFS with Return Values & Globals | *Drive parent decisions bottom-up by aggregating child heights, boolean flags, or metrics.* |
| **3. Path Property & State Propagation** | • [Count Good Nodes In Binary Tree](#count-good-nodes-in-binary-tree)<br>• [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum)<br>• [House Robber III](#house-robber-iii) | Pre-Order / Post-Order with Ancestor State / Tuples | *Carry ancestor context downward or compute local vs. global profitability to isolate optimal paths.* |
| **4. Level-Order Projections & BFS** | • [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)<br>• [Reverse Level Order Traversal](#reverse-level-order-traversal)<br>• [Zigzag Level Order Traversal](#zigzag-level-order-traversal)<br>• [Binary Tree Vertical Order Traversal](#binary-tree-vertical-order-traversal)<br>• [Top View of Binary Tree](#top-view-of-binary-tree)<br>• [Bottom View of Binary Tree](#bottom-view-of-binary-tree)<br>• [Left View of Binary Tree](#left-view-of-binary-tree)<br>• [Binary Tree Right Side View](#binary-tree-right-side-view) | Queue + Generational Snapshot (`levelSize`) / Column Map | *Isolate generational boundaries using level-size snapshots or track horizontal distance coordinates to extract spatial projections.* |
| **5. BST Property & Range Invariants** | • [Validate Binary Search Tree](#validate-binary-search-tree)<br>• [Kth Smallest Element In a Bst](#kth-smallest-element-in-a-bst)<br>• [Lowest Common Ancestor of a Binary Search Tree](#lowest-common-ancestor-of-a-binary-search-tree) | In-Order Traversal or Bounded DFS | *Leverage left < root < right ordering invariants to prune invalid branches or achieve sorted output.* |
| **6. Structural Modification & Construction** | • [Insert into a Binary Search Tree](#insert-into-a-binary-search-tree)<br>• [Delete Node in a BST](#delete-node-in-a-bst)<br>• [Construct Binary Tree From Preorder And Inorder Traversal](#construct-binary-tree-from-preorder-and-inorder-traversal)<br>• [Delete Leaves With a Given Value](#delete-leaves-with-a-given-value)<br>• [Construct Quad Tree](#construct-quad-tree) | Divide & Conquer / Pointer Re-linking | *Rebuild or mutate tree pointers by returning updated child references to parent callers.* |
| **7. Serialization & Complex State Handling** | • [Serialize And Deserialize Binary Tree](#serialize-and-deserialize-binary-tree) | BFS / Pre-Order with Null Markers | *Flatten multidimensional hierarchical topology into a sequential stream using explicit null sentinels.* |

---

## 📋 Detailed Pattern Analysis & Selection Rationale

### Pattern 1: Linear Traversals & Core Operations
* **Included Problems:** [TreeNode Class Definition](#treenode-class-definition), [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal), [Binary Tree Preorder Traversal](#binary-tree-preorder-traversal), [Binary Tree Postorder Traversal](#binary-tree-postorder-traversal), [Find Height of a Binary Tree](#find-height-of-a-binary-tree), [Find Size of a Binary Tree](#find-size-of-a-binary-tree), [Find Node in Binary Tree](#find-node-in-binary-tree).
* **Why this pattern?** These problems require visiting every node in a specific sequence (Pre, In, Post) or performing basic structural counts across all nodes.
* **Expert Rationale:** They form the foundational grammar of tree navigation. Mastering recursive and iterative stack implementations trains your brain to handle pointer movement without losing state.

### Pattern 2: Structural DFS (Bottom-Up Accumulation)
* **Included Problems:** [Invert Binary Tree](#invert-binary-tree), [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree), [Diameter of Binary Tree](#diameter-of-binary-tree), [Balanced Binary Tree](#balanced-binary-tree), [Same Tree](#same-tree), [Subtree of Another Tree](#subtree-of-another-tree).
* **Why this pattern?** Each node's property depends entirely on the aggregated results of its left and right subtrees. 
* **Expert Rationale:** We use post-order traversal to ask children for their metrics before computing the parent's state, collapsing exponential checks into $\mathcal{O}(N)$ passes.

### Pattern 3: Path Property & State Propagation
* **Included Problems:** [Count Good Nodes In Binary Tree](#count-good-nodes-in-binary-tree), [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum), [House Robber III](#house-robber-iii).
* **Why this pattern?** The computation requires tracking constraints across ancestors or managing mutually exclusive choices.
* **Expert Rationale:** Employs top-down state propagation or bottom-up dynamic programming tuples to isolate optimal paths.

### Pattern 4: Level-Order Projections & BFS
* **Included Problems:** [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal), [Reverse Level Order Traversal](#reverse-level-order-traversal), [Zigzag Level Order Traversal](#zigzag-level-order-traversal), [Binary Tree Vertical Order Traversal](#binary-tree-vertical-order-traversal), [Top View of Binary Tree](#top-view-of-binary-tree), [Bottom View of Binary Tree](#bottom-view-of-binary-tree), [Left View of Binary Tree](#left-view-of-binary-tree), [Binary Tree Right Side View](#binary-tree-right-side-view).
* **Why this pattern?** The problem asks for properties grouped by depth level or horizontal column coordinate projections.
* **Expert Rationale:** Capturing `levelSize = queue.size()` locks down generational boundaries, while horizontal coordinate tracking enables precise multi-dimensional projections.

### Pattern 5: BST Property & Range Invariants
* **Included Problems:** [Validate Binary Search Tree](#validate-binary-search-tree), [Kth Smallest Element In a Bst](#kth-smallest-element-in-a-bst), [Lowest Common Ancestor of a Binary Search Tree](#lowest-common-ancestor-of-a-binary-search-tree).
* **Why this pattern?** The tree follows the Binary Search Tree invariant ($\text{Left} < \text{Root} < \text{Right}$).
* **Expert Rationale:** Exploits value ordering to prune search spaces or achieve sorted output via in-order traversals.

### Pattern 6: Structural Modification & Construction
* **Included Problems:** [Insert into a Binary Search Tree](#insert-into-a-binary-search-tree), [Delete Node in a BST](#delete-node-in-a-bst), [Construct Binary Tree From Preorder And Inorder Traversal](#construct-binary-tree-from-preorder-and-inorder-traversal), [Delete Leaves With a Given Value](#delete-leaves-with-a-given-value), [Construct Quad Tree](#construct-quad-tree).
* **Why this pattern?** These problems alter tree topology, build trees from arrays, or prune nodes based on leaf conditions.
* **Expert Rationale:** Utilizes Divide & Conquer paradigms and pointer re-linking up the call stack.

### Pattern 7: Serialization & Complex State Handling
* **Included Problems:** [Serialize And Deserialize Binary Tree](#serialize-and-deserialize-binary-tree).
* **Why this pattern?** The tree must be converted into a persistent string format and accurately reconstructed without structural ambiguity.
* **Expert Rationale:** Relies on pre-order traversal with explicit null markers to flatten hierarchical structures.

---

## 📚 Problem Solutions & Mental Models Index

### Part 1: Binary Tree

#### TreeNode Class Definition
> 💡 **Expert Mental Model:** Standard doubly-linked binary tree node with recursive structure pointers.
> ⏱️ **Complexity:** Memory footprint $\mathcal{O}(1)$ per node instance.
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

#### Permanent Mental Anchors
> 1. **Pre-Push vs. No Pre-Push Axiom (Deferred Work):**
> * **Root-First Traversals (Push Before `while`):** Process root *immediately* upon seeing it (never deferred). Stack acts purely as a work-backlog (`while (!stack.isEmpty())`). *(Pre-Order, 2-Stack Post-Order)*
> * **Left-First Traversals (No Push Before `while`):** Cannot process root yet (left subtree must come first). Pointer `curr` drives loop entry (`while (curr != null || !stack.isEmpty())`) and stashes parents dynamically as it dives down the left spine. *(In-Order, 1-Stack Post-Order)*
> 2. **Push Order Rule (Stack LIFO Property):**
> * To process child $A$ before child $B$, you must push child $B$ **before** child $A$.

---

### 🧠 The Top 1% Base Case Derivation Framework
Elite engineers do not guess base cases through trial and error. They derive them systematically using three architectural axioms:
> * **AXIOM 1: The Structural Boundary (Null-Safety):** Intercepts non-existent nodes (`root == null`) before property lookups trigger a `NullPointerException`.
> * **AXIOM 2: The Monoidal Identity Element (Mathematical Neutralization):** Determines what an empty space (`null`) contributes to parent aggregation without corrupting calculations (e.g., additive `0`, offset `-1`, logical `false`).
> * **AXIOM 3: The State-Space Exhaustion (Range/Length Collision):** For array-slicing or bounds-tracking, terminates division when limits cross (`left > right`) or arrays empty out (`length == 0`).

---

#### Binary Tree Inorder Traversal
> 💡 **Expert Mental Model:** Left spine dive first; process root upon ascent, then pivot right. No pre-push; pointer `curr` drives loop entry.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Terminates execution immediately upon hitting a leaf's child since no data values can be bubbled up (`AXIOM 1`).

**Recursive In-Order Traversal**
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
        if (root == null) return; // Base Case Axiom 1 (Structural Boundary)
        helper(root.left, result);
        result.add(root.val);
        helper(root.right, result);
    }
}
```

**Iterative In-Order Traversal**
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class IterativeInOrder {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;
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

#### Binary Tree Preorder Traversal
> 💡 **Expert Mental Model:** Visit root immediately, push right then left child onto stack so left processes first. Root-first pre-push work list model.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Terminates execution immediately upon hitting a leaf's child since no data values can be bubbled up (`AXIOM 1`).

**Recursive Pre-Order Traversal**
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
        if (root == null) return; // Base Case Axiom 1 (Structural Boundary)
        result.add(root.val);
        helper(root.left, result);
        helper(root.right, result);
    }
}
```

**Iterative Pre-Order Traversal**
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class IterativePreOrder {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode top = stack.pop();
            result.add(top.val);
            if (top.right != null) stack.push(top.right);
            if (top.left != null) stack.push(top.left);
        }
        return result;
    }
}
```

---

#### Binary Tree Postorder Traversal
> 💡 **Expert Mental Model:** Bottom-up guarantee; process both subtrees completely before evaluating parent using dual stacks or single-stack tracking.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(N)$ (2-stack) or $\mathcal{O}(H)$ (1-stack)
> 📐 **Top 1% Base Case Derivation:** Terminates execution immediately upon hitting a leaf's child since no data values can be bubbled up (`AXIOM 1`).

**Recursive Post-Order Traversal**
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
        if (root == null) return; // Base Case Axiom 1 (Structural Boundary)
        helper(root.left, result);
        helper(root.right, result);
        result.add(root.val);
    }
}
```

**Iterative Post-Order Traversal (2-Stack Approach)**
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
        stack1.push(root);
        while (!stack1.isEmpty()) {
            TreeNode top = stack1.pop();
            stack2.push(top);
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

**Iterative Post-Order Traversal (1-Stack with Tracking)**
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class IterativePostOrderOneStack {
    public List<Integer> postorderTraversal1Stack(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;
        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;
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

#### Find Height of a Binary Tree
> 💡 **Expert Mental Model:** Calculate subtree maximum height recursively with explicit alignment to edge-count (-1 base) or node-count (0 base) standards.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Edge-count uses `-1` so a leaf node evaluates $1 + (-1) = 0$ edges; node-count uses `0` because an empty tree contains zero nodes (`AXIOM 2`).
>
> Spot on. You've nailed the exact directional mental models:

Height of a node: Measured downward from that node to its deepest leaf (bottom-up).

Depth of a node: Measured upward from the root down to that node (top-down).

Because of this symmetry, when you look at an entire tree:

The max depth of the tree is the depth of its lowest leaf.

The max height of the tree is the height of its root.

**Convention 1: Edge-Count Basis (Single Node = 0)**
```java
public int heightEdgeCount(TreeNode root) {
    if (root == null) return -1; // Base Case Axiom 2 (Offset Neutralization)
    return Math.max(heightEdgeCount(root.left), heightEdgeCount(root.right)) + 1;
}
```

**Convention 2: Node-Count Basis (Single Node = 1)**
```java
public int heightNodeCount(TreeNode root) {
    if (root == null) return 0; // Base Case Axiom 2 (Additive Monoidal Identity)
    return Math.max(heightNodeCount(root.left), heightNodeCount(root.right)) + 1;
}
```

---

#### Find Size of a Binary Tree
> 💡 **Expert Mental Model:** Total node count equals left subtree size plus right subtree size plus 1 for the current node.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** An empty subtree contributes zero nodes to size, acting as the additive identity element (`AXIOM 2`).
```java
public int findSize(TreeNode root) {
    if (root == null) return 0; // Base Case Axiom 2 (Additive Monoidal Identity)
    return findSize(root.left) + findSize(root.right) + 1;
}
```

---

#### Find Node in Binary Tree
> 💡 **Expert Mental Model:** Perform short-circuit OR search across subtrees after checking current root value.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** A `null` branch means the target is absent, acting as a neutral element in logical disjunction (`||`) (`AXIOM 2`).
```java
public boolean findNode(TreeNode root, int target) {
    if (root == null) return false; // Base Case Axiom 2 (Logical Identity)
    if (root.val == target) return true;
    return findNode(root.left, target) || findNode(root.right, target);
}
```

---

#### Invert Binary Tree
> 💡 **Expert Mental Model:** Swap left and right pointers locally at every node during traversal.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Passes an empty reference safely back up to leaf nodes without breaking pointer assignments (`AXIOM 1`).
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null; // Base Case Axiom 1 (Structural Boundary)
        TreeNode left = root.left, right = root.right;
        root.left = right;
        root.right = left;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}
```

---

#### Maximum Depth of Binary Tree
> 💡 **Expert Mental Model:** Height is 1 plus the maximum of left and right child heights.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** An empty tree contains zero depth units, serving as the additive monoidal identity for leaf depth calculation (`AXIOM 2`).
```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0; // Base Case Axiom 2 (Additive Monoidal Identity)
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}
```

---

#### Diameter of Binary Tree
> 💡 **Expert Mental Model:** Compute local heights bottom-up while updating a global diameter scoreboard.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** A `null` child provides a baseline height of `0` to keep span and height-delta calculations mathematically valid (`AXIOM 2`).
```java
public class BinaryTreeDiameter {
    private int maxDiameter = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        maxDiameter = 0;
        calculateHeight(root);
        return maxDiameter;
    }
    private int calculateHeight(TreeNode node) {
        if (node == null) return 0; // Base Case Axiom 2 (Mathematical Neutralization)
        int leftHeight = calculateHeight(node.left);
        int rightHeight = calculateHeight(node.right);
        maxDiameter = Math.max(maxDiameter, leftHeight + rightHeight);
        return 1 + Math.max(leftHeight, rightHeight);
    }
}
```

---

#### Balanced Binary Tree
> 💡 **Expert Mental Model:** Bubble up height differences; short-circuit or flag imbalance immediately if subtree delta exceeds 1.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** A `null` node represents a balanced baseline height of `0` (`AXIOM 2`).
```java
class Solution {
    boolean balanced = true;
    public boolean isBalanced(TreeNode root) {
        height(root);
        return balanced;
    }
    public int height(TreeNode root){
        if(root == null) return 0; // Base Case Axiom 2 (Mathematical Neutralization)
        int leftH = height(root.left);
        int rightH = height(root.right);
        if(Math.abs(leftH - rightH) > 1){
            balanced = false;
        }
        return 1 + Math.max(leftH, rightH);
    }
}
```

---

#### Same Tree
> 💡 **Expert Mental Model:** Structural and value equivalence requires simultaneous node matching across both trees.
> ⏱️ **Complexity:** Time $\mathcal{O}(\min(N_1, N_2))$ | Space $\mathcal{O}(\min(H_1, H_2))$
> 📐 **Top 1% Base Case Derivation:** Dual-node symmetry check; returns `true` only if structural termination aligns simultaneously across both trees (`AXIOM 1`).
```java
public boolean areEqual(TreeNode root1, TreeNode root2) {
    if (root1 == null && root2 == null) return true; // Base Case Axiom 1 (Structural Symmetry)
    if (root1 == null || root2 == null) return false; // Base Case Axiom 1 (Structural Asymmetry)
    if (root1.val != root2.val) return false;
    return areEqual(root1.left, root2.left) && areEqual(root1.right, root2.right);
}
```

---

#### Subtree of Another Tree
> 💡 **Expert Mental Model:** Treat matching values as suspects; anchor a structural check or fall through to explore subtrees.
> ⏱️ **Complexity:** Time $\mathcal{O}(M \times N)$ worst-case | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Search root exhaustion yields `false` (`AXIOM 2`), while the helper `isSameTree` enforces dual-node symmetry termination (`AXIOM 1`).
```java
class Solution {
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if (root == null) return false; // Base Case Axiom 2 (Search Exhaustion)
        if (root.val == subRoot.val && isSameTree(root, subRoot)) return true;
        return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
    }
    private boolean isSameTree(TreeNode root1, TreeNode root2) {
        if (root1 == null && root2 == null) return true; // Base Case Axiom 1 (Structural Symmetry)
        if (root1 == null || root2 == null) return false; // Base Case Axiom 1 (Structural Asymmetry)
        if (root1.val != root2.val) return false;
        return isSameTree(root1.left, root2.left) && isSameTree(root1.right, root2.right);
    }
}
```

---

#### Binary Tree Level Order Traversal
> 💡 **Expert Mental Model:** Snapshot queue size per iteration to process nodes level-by-level cleanly.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(W)$
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

#### Reverse Level Order Traversal
> 💡 **Expert Mental Model:** Standard top-down BFS combined with prepending (`LinkedList.addFirst`) to build bottom-up level order output.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(W)$
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
        res.addFirst(level);
    }
    return res;
}
```

---

#### Zigzag Level Order Traversal
> 💡 **Expert Mental Model:** Standard BFS level-snapshot combined with direction toggling or alternating dual stacks.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(W)$

**Approach 1: Queue + Direction Flag**
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

**Approach 2: Dual Stacks**
```java
public List<List<Integer>> zigzagLevelOrderDualStack(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) return res;
    Stack<TreeNode> s1 = new Stack<>();
    Stack<TreeNode> s2 = new Stack<>();
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

#### Binary Tree Vertical Order Traversal
> 💡 **Expert Mental Model:** BFS queue paired with horizontal coordinate state `(node, col)` and tracked `minCol`/`maxCol` bounds to avoid sort overhead.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(N)$
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

#### Top View of Binary Tree
> 💡 **Expert Mental Model:** BFS traversal inserting into column map **only if column coordinate is unvisited**.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(N)$
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
            map.put(col, node.val);
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

#### Bottom View of Binary Tree
> 💡 **Expert Mental Model:** BFS traversal with **unconditional overwrites** per column key so deepest node wins.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(N)$
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
        map.put(col, node.val);
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

#### Left View of Binary Tree
> 💡 **Expert Mental Model:** Capture first element (`i == 0`) of every level snapshot in standard BFS.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(W)$
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
            if (i == 0) res.add(curr.val);
            if (curr.left != null) q.offer(curr.left);
            if (curr.right != null) q.offer(curr.right);
        }
    }
    return res;
}
```

---

#### Binary Tree Right Side View
> 💡 **Expert Mental Model:** Capture the last element (`i == levelSize - 1`) of every BFS level queue snapshot.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(W)$
```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while (!q.isEmpty()) {
            int levelSize = q.size();
            for (int i = 0; i < levelSize; i++) {
                TreeNode curr = q.poll();
                if (i == levelSize - 1) {
                    res.add(curr.val);
                }
                if (curr.left != null) q.offer(curr.left);
                if (curr.right != null) q.offer(curr.right);
            }
        }
        return res;
    }
}
```

---

#### Construct Quad Tree
> 💡 **Expert Mental Model:** Quad-tree division requires checking uniformity in 2D grid quadrants and subdividing recursively.
> ⏱️ **Complexity:** Time $\mathcal{O}(N \log N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** When standard grid range slicing narrows to a single cell ($1 \times 1$), the region is uniformly leaves (`AXIOM 3`).
> *(Complete details and code implementation will come soon.)*

---

#### Count Good Nodes In Binary Tree
> 💡 **Expert Mental Model:** Carry path maximum downward (top-down state propagation) to evaluate local goodness.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** A dead-end path contributes zero "good node" counts to the global recursive summation (`AXIOM 2`).
```java
class Solution {
    public int goodNodes(TreeNode root) {
        return dfs(root, root.val);
    }
    private int dfs(TreeNode node, int maxVal) {
        if (node == null) return 0; // Base Case Axiom 2 (Additive Identity)
        int nextMax = Math.max(maxVal, node.val);
        return (node.val >= maxVal ? 1 : 0) 
             + dfs(node.left, nextMax) 
             + dfs(node.right, nextMax);
    }
}
```

---

#### Construct Binary Tree From Preorder And Inorder Traversal
> 💡 **Expert Mental Model:** Preorder root dictates the split boundary in Inorder array to form left and right subtrees.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(N)$
> 📐 **Top 1% Base Case Derivation:** An empty array slice signifies an exhausted subtree boundary, stopping array carving (`AXIOM 3`).
```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder.length == 0 || inorder.length == 0) {
            return null; // Base Case Axiom 3 (State-Space Exhaustion)
        }
        TreeNode root = new TreeNode(preorder[0]);
        int mid = -1;
        for (int i = 0; i < inorder.length; i++) {
            if (inorder[i] == preorder[0]) {
                mid = i;
                break;
            }
        }
        int[] leftPreorder = Arrays.copyOfRange(preorder, 1, mid + 1);
        int[] leftInorder = Arrays.copyOfRange(inorder, 0, mid);
        root.left = buildTree(leftPreorder, leftInorder);
        int[] rightPreorder = Arrays.copyOfRange(preorder, mid + 1, preorder.length);
        int[] rightInorder = Arrays.copyOfRange(inorder, mid + 1, inorder.length);
        root.right = buildTree(rightPreorder, rightInorder);
        return root;
    }
}
```

---

#### House Robber III
> 💡 **Expert Mental Model:** Return pair of values from child subtrees: max money with root robbed vs. root not robbed.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Hitting `null` returns a zero-value state tuple `[0, 0]`, neutralizing profit calculation for empty branches (`AXIOM 2`).
> *(Complete details and code implementation will come soon.)*

---

#### Delete Leaves With a Given Value
> 💡 **Expert Mental Model:** Post-order pruning; remove leaves matching target and propagate null references back to parents.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Passes empty reference `null` back up tree when hitting bounds or pruned subtrees (`AXIOM 1`).
> *(Complete details and code implementation will come soon.)*

---

#### Binary Tree Maximum Path Sum
> 💡 **Expert Mental Model:** Discard negative sub-paths (`Math.max(0, path)`) and track global max path passing through current node.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** An empty branch contributes zero sum value, allowing negative paths to be safely neutralized via `Math.max(0, ...)` (`AXIOM 2`).
```java
class Solution {
    int maxSum = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        pathSum(root);
        return maxSum;
    }
    public int pathSum(TreeNode root){
        if(root == null) return 0; // Base Case Axiom 2 (Mathematical Neutralization)
        int leftMax = Math.max(0, pathSum(root.left));
        int rightMax = Math.max(0, pathSum(root.right));
        maxSum = Math.max(maxSum, root.val + leftMax + rightMax);
        return root.val + Math.max(leftMax, rightMax);
    }
}
```

---

#### Serialize And Deserialize Binary Tree
> 💡 **Expert Mental Model:** Flatten tree via pre-order traversal using explicit null markers to enable unambiguous reconstruction.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(N)$
> 📐 **Top 1% Base Case Derivation:** In serialization, hitting `null` appends explicit sentinel value (e.g., `"null,"`) (`AXIOM 1`). In deserialization, hitting `"null"` returns `null` pointer reference (`AXIOM 3`).
> *(Complete details and code implementation will come soon.)*

---

### Part 2: Binary Search Tree

every node in the left subtree is smaller than the root and every node in the right subtree is greater than the root. This is a recursive property.

Sorted array vs Binary Search Tree :  

BST has search, insert & delete - o(logn) 
Array - search o(logn) , insert o(n) and delete o(n)

In most implementations, duplicates are not allowed.

BST Search - target exists, target doesn't exist

BST Insertion

BST Removal - one child, two children and no children

Complete Binary Tree - All levels are completely filled left to right except the last level.

Specialized Tree Types 

heap -  A complete binary tree satisfying min-heap or max-heap property.
min-heap property  : the value of the parent < values of childrens
max-heap property  : the value of the parent > values of childrens

N-ary Tree :

Trie ( Prefix Tree )
Each node represent single character, path from root to a node respresent word.
Called prefix tree because each node store the prefix of a word.
used for auto-complete feature in search engine.

Each node can have more than 2 children. used for file system hierarchy or representing website structure.

#### Lowest Common Ancestor of a Binary Search Tree
> 💡 **Expert Mental Model:** Exploit BST ordering: if both node values are smaller, go left; if larger, go right; else current node is the split point (LCA).
> ⏱️ **Complexity:** Time $\mathcal{O}(H)$ | Space $\mathcal{O}(1)$ iterative
> 📐 **Top 1% Base Case Derivation:** Search boundary returns `null` if search key drifts out of tree bounds (`AXIOM 1`).
> *(Complete details and code implementation will come soon.)*
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        if(root == null || p == null || q == null) return null;
        if(root.val < p.val && root.val < q.val){
            return lowestCommonAncestor(root.right, p,q);
        }else if(root.val > p.val && root.val > q.val ){
            return lowestCommonAncestor(root.left, p,q);
        }else{
            return root;
        }
    }
}
```
---

#### Insert into a Binary Search Tree
> 💡 **Expert Mental Model:** Navigate left or right based on BST comparison until hitting null, then attach the new node.
> ⏱️ **Complexity:** Time $\mathcal{O}(H)$ | Space $\mathcal{O}(H)$ (or $\mathcal{O}(1)$ iterative)
> 📐 **Top 1% Base Case Derivation:** Hitting `null` indicates the insertion position has been reached; returns `new TreeNode(val)` (`AXIOM 1`).
> *(Complete details and code implementation will come soon.)*
```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {

        if(root == null) return new TreeNode(val);

        if(root.val < val){
            root.right = insertIntoBST(root.right, val);
        }else if(root.val > val){
            root.left = insertIntoBST(root.left, val);
        }

        return root;
        
    }
    
}
```
---

#### Delete Node in a BST
> 💡 **Expert Mental Model:** Handle 0, 1, or 2 children cases by replacing the deleted node with the successor from the right subtree.
> ⏱️ **Complexity:** Time $\mathcal{O}(H)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Hitting `null` implies target value is not present in BST; returns `null` safely (`AXIOM 1`).

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {

        if(root == null) return null;

        if(root.val == key){
            if(root.left == null && root.right == null) return null;
            if(root.left == null) return root.right;
            
            if(root.right == null) return root.left;

            //System.out.println(minRight(root.right, root).val);
            TreeNode min = minRight(root.right, root);
            root.val = min.val;
            root.right = deleteNode(root.right, min.val);
        }else if(root.val < key){
            root.right = deleteNode(root.right, key);
        }else if(root.val > key){
            root.left = deleteNode(root.left, key);
        }
        
        return root;

        
    }
    private TreeNode minRight(TreeNode root, TreeNode parent){
        TreeNode curr = root;
        while(curr != null && curr.left != null) {
            curr = curr.left;
        }
        return curr;
    }
}
```

---

#### Validate Binary Search Tree
> 💡 **Expert Mental Model:** Enforce strict valid range bounds (`min < node.val < max`) recursively down every branch.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Reaching `null` means the branch successfully satisfied all range constraints up to its leaf (`AXIOM 2`).
> *(Complete details and code implementation will come soon.)*

---

#### Kth Smallest Element In a Bst
> 💡 **Expert Mental Model:** In-order traversal visits BST nodes in ascending sorted order; stop at count $k$.
> ⏱️ **Complexity:** Time $\mathcal{O}(H + k)$ | Space $\mathcal{O}(H)$
> 📐 **Top 1% Base Case Derivation:** Standard in-order traversal termination on `null` or early stopping when counter hits $k$ (`AXIOM 1 & 3`).
> *(Complete details and code implementation will come soon.)*
