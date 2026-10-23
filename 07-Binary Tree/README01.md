
# Binary Tree Master Reference & Implementation Guide

A production-grade, high-density reference manual covering fundamental tree data structures, mental traversal models, structural property invariants, and level-order/projection variations.

---

## 🗺️ Master Problem-to-Pattern Taxonomy

| Pattern Group | Problems Included | Core Algorithmic Technique | Top 1% Expert One-Liner Analysis |
| :--- | :--- | :--- | :--- |
| **1. Linear Traversals** | • [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)<br>• [Binary Tree Preorder Traversal](#binary-tree-preorder-traversal)<br>• [Binary Tree Postorder Traversal](#binary-tree-postorder-traversal) | Recursion / Explicit Stack | *Control execution order by decoupling when you visit the root relative to its subtrees.* |
| **2. Structural DFS (Bottom-Up Accumulation)** | • [Invert Binary Tree](#invert-binary-tree)<br>• [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)<br>• [Diameter of Binary Tree](#diameter-of-binary-tree)<br>• [Balanced Binary Tree](#balanced-binary-tree)<br>• [Same Tree](#same-tree)<br>• [Subtree of Another Tree](#subtree-of-another-tree) | Post-Order DFS with Return Values & Globals | *Drive parent decisions bottom-up by aggregating child heights, boolean flags, or metrics.* |
| **3. Path Property & State Propagation** | • [Count Good Nodes In Binary Tree](#count-good-nodes-in-binary-tree)<br>• [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum)<br>• [House Robber III](#house-robber-iii) | Pre-Order / Post-Order with Ancestor State / Tuples | *Carry ancestor context downward or compute local vs. global profitability to isolate optimal paths.* |
| **4. Level-Order Projections & BFS** | • [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)<br>• [Binary Tree Right Side View](#binary-tree-right-side-view) | Queue + Generational Snapshot (`levelSize`) | *Isolate generational boundaries using level-size snapshots to extract spatial projections.* |
| **5. BST Property & Range Invariants** | • [Validate Binary Search Tree](#validate-binary-search-tree)<br>• [Kth Smallest Element In a Bst](#kth-smallest-element-in-a-bst)<br>• [Lowest Common Ancestor of a Binary Search Tree](#lowest-common-ancestor-of-a-binary-search-tree) | In-Order Traversal or Bounded DFS | *Leverage left < root < right ordering invariants to prune invalid branches or achieve sorted output.* |
| **6. Structural Modification & Construction** | • [Insert into a Binary Search Tree](#insert-into-a-binary-search-tree)<br>• [Delete Node in a BST](#delete-node-in-a-bst)<br>• [Construct Binary Tree From Preorder And Inorder Traversal](#construct-binary-tree-from-preorder-and-inorder-traversal)<br>• [Delete Leaves With a Given Value](#delete-leaves-with-a-given-value)<br>• [Construct Quad Tree](#construct-quad-tree) | Divide & Conquer / Pointer Re-linking | *Rebuild or mutate tree pointers by returning updated child references to parent callers.* |
| **7. Serialization & Complex State Handling** | • [Serialize And Deserialize Binary Tree](#serialize-and-deserialize-binary-tree) | BFS / Pre-Order with Null Markers | *Flatten multidimensional hierarchical topology into a sequential stream using explicit null sentinels.* |

---

## 📋 Detailed Pattern Analysis & Selection Rationale

### Pattern 1: Linear Traversals
* **Included Problems:** [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal), [Binary Tree Preorder Traversal](#binary-tree-preorder-traversal), [Binary Tree Postorder Traversal](#binary-tree-postorder-traversal).
* **Why this pattern?** These problems require visiting every node in a specific sequence (Pre, In, Post) to generate a sequential list. 
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
* **Included Problems:** [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal), [Binary Tree Right Side View](#binary-tree-right-side-view).
* **Why this pattern?** The problem asks for properties grouped by depth level or spatial columns.
* **Expert Rationale:** Capturing `levelSize = queue.size()` locks down generational boundaries to extract spatial projections efficiently.

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

### Permanent Mental Anchors
> 1. **Pre-Push vs. No Pre-Push Axiom (Deferred Work):**
> * **Root-First Traversals (Push Before `while`):** Process root *immediately* upon seeing it (never deferred). Stack acts purely as a work-backlog (`while (!stack.isEmpty())`). *(Pre-Order, 2-Stack Post-Order)*
> * **Left-First Traversals (No Push Before `while`):** Cannot process root yet (left subtree must come first). Pointer `curr` drives loop entry (`while (curr != null || !stack.isEmpty())`) and stashes parents dynamically as it dives down the left spine. *(In-Order, 1-Stack Post-Order)*
> 2. **Push Order Rule (Stack LIFO Property):**
> * To process child $A$ before child $B$, you must push child $B$ **before** child $A$.

---

### Binary Tree Inorder Traversal
> 💡 **Expert Mental Model:** Left spine dive first; process root upon ascent, then pivot right. No pre-push; pointer `curr` drives loop entry.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$

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
        if (root == null) return;
        
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

### Binary Tree Preorder Traversal

> 💡 **Expert Mental Model:** Visit root immediately, push right then left child onto stack so left processes first. Root-first pre-push work list model.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$

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
        if (root == null) return;
        
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

### Binary Tree Postorder Traversal

> 💡 **Expert Mental Model:** Bottom-up guarantee; process both subtrees completely before evaluating parent using dual stacks or single-stack tracking.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(N)$ (2-stack) or $\mathcal{O}(H)$ (1-stack)

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
        if (root == null) return;
        
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

### Invert Binary Tree

> 💡 **Expert Mental Model:** Swap left and right pointers locally at every node during traversal.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$

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

---

### Maximum Depth of Binary Tree

> 💡 **Expert Mental Model:** Height is 1 plus the maximum of left and right child heights.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}

```

---

### Diameter of Binary Tree

> 💡 **Expert Mental Model:** Compute local heights bottom-up while updating a global diameter scoreboard.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$

```java
public class BinaryTreeDiameter {
    private int maxDiameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        maxDiameter = 0;
        calculateHeight(root);
        return maxDiameter;
    }

    private int calculateHeight(TreeNode node) {
        if (node == null) return 0;
        int leftHeight = calculateHeight(node.left);
        int rightHeight = calculateHeight(node.right);
        maxDiameter = Math.max(maxDiameter, leftHeight + rightHeight);
        return 1 + Math.max(leftHeight, rightHeight);
    }
}

```

---

### Balanced Binary Tree

> 💡 **Expert Mental Model:** Bubble up height differences; short-circuit or flag imbalance immediately if subtree delta exceeds 1.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$

```java
class Solution {
    boolean balanced = true;
    public boolean isBalanced(TreeNode root) {
        height(root);
        return balanced;
    }
    public int height(TreeNode root){
        if(root == null) return 0;
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

### Same Tree

> 💡 **Expert Mental Model:** Structural and value equivalence requires simultaneous node matching across both trees.
> ⏱️ **Complexity:** Time $\mathcal{O}(\min(N_1, N_2))$ | Space $\mathcal{O}(\min(H_1, H_2))$

```java
public boolean areEqual(TreeNode root1, TreeNode root2) {
    if (root1 == null && root2 == null) return true;
    if (root1 == null || root2 == null) return false;
    if (root1.val != root2.val) return false;
    return areEqual(root1.left, root2.left) && areEqual(root1.right, root2.right);
}

```

---

### Subtree of Another Tree

> 💡 **Expert Mental Model:** Treat matching values as suspects; anchor a structural check or fall through to explore subtrees.
> ⏱️ **Complexity:** Time $\mathcal{O}(M \times N)$ worst-case | Space $\mathcal{O}(H)$

```java
class Solution {
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if (root == null) return false;
        if (root.val == subRoot.val && isSameTree(root, subRoot)) return true;
        return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
    }
    
    private boolean isSameTree(TreeNode root1, TreeNode root2) {
        if (root1 == null && root2 == null) return true;
        if (root1 == null || root2 == null) return false;
        if (root1.val != root2.val) return false;
        return areEqual(root1.left, root2.left) && areEqual(root1.right, root2.right);
    }
}

```

---

### Lowest Common Ancestor of a Binary Search Tree

> 💡 **Expert Mental Model:** Exploit BST ordering: if both node values are smaller, go left; if larger, go right; else current node is the split point (LCA).
> ⏱️ **Complexity:** Time $\mathcal{O}(H)$ | Space $\mathcal{O}(1)$ iterative
> *(Complete details and code implementation will come soon.)*

---

### Insert into a Binary Search Tree

> 💡 **Expert Mental Model:** Navigate left or right based on BST comparison until hitting null, then attach the new node.
> ⏱️ **Complexity:** Time $\mathcal{O}(H)$ | Space $\mathcal{O}(H)$ (or $\mathcal{O}(1)$ iterative)
> *(Complete details and code implementation will come soon.)*

---

### Delete Node in a BST

> 💡 **Expert Mental Model:** Handle 0, 1, or 2 children cases by replacing the deleted node with the successor from the right subtree.
> ⏱️ **Complexity:** Time $\mathcal{O}(H)$ | Space $\mathcal{O}(H)$
> *(Complete details and code implementation will come soon.)*

---

### Binary Tree Level Order Traversal

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

### Binary Tree Right Side View

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

### Construct Quad Tree

> 💡 **Expert Mental Model:** Quad-tree division requires checking uniformity in 2D grid quadrants and subdividing recursively.
> ⏱️ **Complexity:** Time $\mathcal{O}(N \log N)$ | Space $\mathcal{O}(H)$
> *(Complete details and code implementation will come soon.)*

---

### Count Good Nodes In Binary Tree

> 💡 **Expert Mental Model:** Carry path maximum downward (top-down state propagation) to evaluate local goodness.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$

```java
class Solution {
    public int goodNodes(TreeNode root) {
        return dfs(root, root.val);
    }

    private int dfs(TreeNode node, int maxVal) {
        if (node == null) return 0;
        int nextMax = Math.max(maxVal, node.val);
        return (node.val >= maxVal ? 1 : 0) 
             + dfs(node.left, nextMax) 
             + dfs(node.right, nextMax);
    }
}

```

---

### Validate Binary Search Tree

> 💡 **Expert Mental Model:** Enforce strict valid range bounds (`min < node.val < max`) recursively down every branch.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> *(Complete details and code implementation will come soon.)*

---

### Kth Smallest Element In a Bst

> 💡 **Expert Mental Model:** In-order traversal visits BST nodes in ascending sorted order; stop at count $k$.
> ⏱️ **Complexity:** Time $\mathcal{O}(H + k)$ | Space $\mathcal{O}(H)$
> *(Complete details and code implementation will come soon.)*

---

### Construct Binary Tree From Preorder And Inorder Traversal

> 💡 **Expert Mental Model:** Preorder root dictates the split boundary in Inorder array to form left and right subtrees.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(N)$

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder.length == 0 || inorder.length == 0) {
            return null;
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

### House Robber III

> 💡 **Expert Mental Model:** Return pair of values from child subtrees: max money with root robbed vs. root not robbed.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> *(Complete details and code implementation will come soon.)*

---

### Delete Leaves With a Given Value

> 💡 **Expert Mental Model:** Post-order pruning; remove leaves matching target and propagate null references back to parents.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$
> *(Complete details and code implementation will come soon.)*

---

### Binary Tree Maximum Path Sum

> 💡 **Expert Mental Model:** Discard negative sub-paths (`Math.max(0, path)`) and track global max path passing through current node.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(H)$

```java
class Solution {
    int maxSum = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        pathSum(root);
        return maxSum;
    }

    public int pathSum(TreeNode root){
        if(root == null) return 0;
        int leftMax = Math.max(0, pathSum(root.left));
        int rightMax = Math.max(0, pathSum(root.right));
        maxSum = Math.max(maxSum, root.val + leftMax + rightMax);
        return root.val + Math.max(leftMax, rightMax);
    }
}

```

---

### Serialize And Deserialize Binary Tree

> 💡 **Expert Mental Model:** Flatten tree via pre-order traversal using explicit null markers to enable unambiguous reconstruction.
> ⏱️ **Complexity:** Time $\mathcal{O}(N)$ | Space $\mathcal{O}(N)$
> *(Complete details and code implementation will come soon.)*

```

<FollowUp label="Would you like to add code implementations for any remaining stubbed problems?" query="Show me the complete code implementation for Validate Binary Search Tree."/>

```
