
# Binary Tree Master Reference & Implementation Guide
*(Consolidated Java Codebase from Phyley CS & Code Campaign Playlists)*

This repository/reference contains all binary tree traversal implementations, structural property checks, and level-order variations taught across both course playlists. Each algorithm is accompanied by its high-level logic and exact source attribution.

---

## 📋 Table of Contents
1. [Node Definition](#node-definition)
2. [DFS (Depth-First Search) Traversals](#1-dfs-depth-first-search-traversals)
   * Recursive Pre-Order, In-Order, Post-Order
   * Iterative Pre-Order
   * Iterative In-Order (Nested While vs. If-Else)
   * Iterative Post-Order (1 Stack & 2 Stacks)
3. [Tree Properties & Structural Checks (DFS)](#2-tree-properties--structural-checks-dfs)
4. [BFS (Breadth-First Search) & Level Order Variations](#3-bfs-breadth-first-search--level-order-variations)

---

Here is the revised code with each traversal split into its own distinct section, accompanied by detailed conceptual breakdowns covering execution flow, time/space complexity, and practical use cases.

---

### 1. TreeNode Class Definition

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

### 2. Recursive Traversals

#### A. Recursive Pre-Order Traversal

* **Order:** Root $\rightarrow$ Left Subtree $\rightarrow$ Right Subtree
* **Concept:** Processes the current parent node before exploring its child subtrees.
* **Time Complexity:** $\mathcal{O}(N)$ — Every node is visited once.
* **Space Complexity:** $\mathcal{O}(H)$ — Implicit call stack overhead, where $H$ is the height of the tree ($\mathcal{O}(\log N)$ for balanced trees, $\mathcal{O}(N)$ worst-case for skewed trees).
* **Use Case:** Serializing/deserializing a binary tree or creating a deep copy of a tree structure.

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
        
        result.add(root.val);      // Process Root
        helper(root.left, result);  // Traverse Left Subtree
        helper(root.right, result); // Traverse Right Subtree
    }
}

```

---

#### B. Recursive In-Order Traversal

* **Order:** Left Subtree $\rightarrow$ Root $\rightarrow$ Right Subtree
* **Concept:** Traverses down the leftmost branch completely before processing the parent node and moving to the right child.
* **Time Complexity:** $\mathcal{O}(N)$
* **Space Complexity:** $\mathcal{O}(H)$
* **Use Case:** Retrieves elements in sorted non-decreasing order when executed on a Binary Search Tree (BST).

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
        
        helper(root.left, result);  // Traverse Left Subtree
        result.add(root.val);      // Process Root
        helper(root.right, result); // Traverse Right Subtree
    }
}

```

---

#### C. Recursive Post-Order Traversal

* **Order:** Left Subtree $\rightarrow$ Right Subtree $\rightarrow$ Root
* **Concept:** Processes all descendant nodes bottom-up before inspecting or modifying the parent node.
* **Time Complexity:** $\mathcal{O}(N)$
* **Space Complexity:** $\mathcal{O}(H)$
* **Use Case:** Deleting a tree safely from bottom to top, computing tree height, or evaluating postfix arithmetic expression trees.

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
        
        helper(root.left, result);  // Traverse Left Subtree
        helper(root.right, result); // Traverse Right Subtree
        result.add(root.val);      // Process Root
    }
}

```

---

### 3. Iterative Traversals

#### A. Iterative In-Order Traversal

* **Strategy:** Explicit Stack with `curr` Pointer
* **Concept:**
1. Drills down along left pointers, pushing each node onto an explicit stack until `curr` becomes `null`.
2. Pops from the stack to process the leftmost unprocessed node (Root phase).
3. Directs `curr = top.right` to evaluate the right branch in the subsequent iteration.


* **Time Complexity:** $\mathcal{O}(N)$
* **Space Complexity:** $\mathcal{O}(H)$ explicit stack space.

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
                curr = curr.left; // Phase 1: Go as left as possible
            } else {
                TreeNode top = stack.pop(); // Phase 2: Process top node
                result.add(top.val);
                curr = top.right; // Phase 3: Move to right subtree
            }
        }
        return result;
    }
}

```

---

#### B. Iterative Post-Order Traversal (2-Stack Approach)

* **Strategy:** Modified Pre-Order Inversion
* **Concept:**
* Standard Pre-Order traverses: **Root $\rightarrow$ Left $\rightarrow$ Right**.
* Reversing stack push operations yields: **Root $\rightarrow$ Right $\rightarrow$ Left**.
* Pushing elements into a secondary stack instead of directly printing them reverses the sequence completely to: **Left $\rightarrow$ Right $\rightarrow$ Root** (Post-Order).


* **Time Complexity:** $\mathcal{O}(N)$
* **Space Complexity:** $\mathcal{O}(N)$ — Requires storage for all tree nodes in the second stack.

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

            // Push left first so right is popped first from stack1
            if (top.left != null) stack1.push(top.left);
            if (top.right != null) stack1.push(top.right);
        }

        // Empty stack2 into result to produce correct Post-Order
        while (!stack2.isEmpty()) {
            result.add(stack2.pop().val);
        }

        return result;
    }
}

```

---

#### C. Iterative Post-Order Traversal (1-Stack with Tracking)

* **Strategy:** Single Stack with `peekNode` & `lastVisited` Pointer
* **Concept:**
1. Descends left as far as possible, pushing nodes onto the stack.
2. Inspects (`peek()`) the top node:
* If it has an unvisited right child (`peekNode.right != null && current != peekNode.right`), pivot to process the right child.
* If the right child is `null` or was just visited (`current == peekNode.right`), pop and process the node. Update `current` to track this node as `visited`.




* **Time Complexity:** $\mathcal{O}(N)$
* **Space Complexity:** $\mathcal{O}(H)$ optimal space usage.

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
                current = current.left; // Phase 1: Go left
            } else {
                TreeNode peekNode = stack.peek();
                
                // Phase 2: Check if unvisited right child exists
                if (peekNode.right != null && current != peekNode.right) {
                    current = peekNode.right;
                } else {
                    // Phase 3: Both left and right subtrees complete
                    TreeNode visitedNode = stack.pop();
                    result.add(visitedNode.val);
                    current = visitedNode; // Mark node as last visited
                }
            }
        }
        return result;
    }
}

```

---

## 2. Tree Properties & Structural Checks (DFS)

### Find Height of a Binary Tree
The philosophy behind recursion here is: If I know the height of my left subtree and my right subtree, I can easily find my own height.
Note: This convention means a tree with just a single root node will have a height of 0, because Math.max(-1, -1) + 1 = 0
```java
public int height(Node root) {
    if (root == null) return -1;
    int leftHeight = height(root.left);
    int rightHeight = height(root.right);
    return Math.max(leftHeight, rightHeight) + 1;
}

```

### Find Size of a Binary Tree
The logic relies on a simple rule: The size of any tree is equal to the size of its left subtree plus the size of its right subtree, plus 1 (for the current node itself).
If the current node is null (meaning we've reached past a leaf node), it contributes 0 to the total count.

```java
public int findSize(Node root) {
    if (root == null) return 0;
    return findSize(root.left) + findSize(root.right) + 1;
}

```

### Find Node in Binary Tree
The philosophy here is straightforward: "Is the target at my current position? If not, check my left branch. If it's still not there, check my right branch."

```java
public boolean findNode(Node root, int target) {
    if (root == null) return false; // root null
    if (root.data == target) return true; // target at my current position
    return findNode(root.left, target) || findNode(root.right, target);
}

```

### Check If Two Binary Trees Are Equal
The philosophy here is: "Two trees are identical if their current roots have the same data, their left subtrees are identical, and their right subtrees are identical."

```java
public boolean areEqual(Node root1, Node root2) {
    if (root1 == null && root2 == null) return true;  // If both current nodes are null
    if (root1 == null || root2 == null) return false; // If one node is null
    if (root1.data != root2.data) return false;       //Data Mismatch

    // on this line, root values are same & we need to check children for equality.
    return areEqual(root1.left, root2.left) && areEqual(root1.right, root2.right); 
}

```

---

## 3. BFS (Breadth-First Search) & Level Order Variations

### Standard Level Order Traversal ( LEFT TO RIGHT)
- Using Queue
- L0
- L1
- L2
- L3
- LEVEL ORDER = L0 - L1 - L2 - L3


```java
public void levelOrder(Node root) {
    if (root == null) return;
    java.util.Queue<Node> q = new java.util.LinkedList<>();
    q.add(root);
    while (!q.isEmpty()) {
        Node current = q.poll();
        System.out.print(current.data + " ");

        // LEFT TO RIGHT
        if (current.left != null) q.add(current.left); // LEFT TO RIGHT
        if (current.right != null) q.add(current.right); //LEFT TO RIGHT

        // RIGHT TO LEFT
        // if(current.right != null) q.add(current.right);
        // if(current.left != null) q.add(current.left);
    }
}

```

### Reverse Level Order Traversal ( LEFT TO RIGHT)
- Using Queue + Stack
- L0
- L1
- L2
- L3
- REVERSE LEVEL ORDER = L3 - L2 - L1 - L0

```java
public void reverseLevelOrder(Node root) {
    if (root == null) return;
    java.util.Queue<Node> q = new java.util.LinkedList<>();
    java.util.Stack<Node> st = new java.util.Stack<>();    // LEVELS ARE REVERSED
    q.add(root);
    while (!q.isEmpty()) {
        Node current = q.poll();
        st.push(current);

        // LEFT TO RIGHT - DIRECTION
        if (current.right != null) q.add(current.right);
        if (current.left != null) q.add(current.left);

        //RIGHT TO LEFT - DIRECTION
        // if(current.left != null) q.add(current.left);
        // if(current.right != null) q.add(current.right);
    }
    while (!st.isEmpty()) {
        System.out.print(st.pop().data + " ");
    }
}

```

### Spiral (Zigzag) Level Order Traversal

```java
public void spiralLevelOrder(Node root) {
    if (root == null) return;
    java.util.Stack<Node> s1 = new java.util.Stack<>();
    java.util.Stack<Node> s2 = new java.util.Stack<>();
    s1.push(root);
    while (!s1.isEmpty() || !s2.isEmpty()) {
        while (!s1.isEmpty()) {
            Node current = s1.pop();
            System.out.print(current.data + " ");
            if (current.left != null) s2.push(current.left);
            if (current.right != null) s2.push(current.right);
        }
        while (!s2.isEmpty()) {
            Node current = s2.pop();
            System.out.print(current.data + " ");
            if (current.right != null) s1.push(current.right);
            if (current.left != null) s1.push(current.left);
        }
    }
}

```

### Print Binary Tree Level by Level (Using Null Delimiter)

```java
public void printLevelByLevel(Node root) {
    if (root == null) return;
    Node delimiter = null;
    java.util.Queue<Node> q = new java.util.LinkedList<>();
    q.add(root);
    q.add(delimiter);

    while (true) {
        Node current = q.poll();
        if (current != delimiter) {
            System.out.print(current.data + " ");
            if (current.left != null) q.add(current.left);
            if (current.right != null) q.add(current.right);
        } else {
            System.out.println();
            if (q.isEmpty()) {
                break;
            } else {
                q.add(delimiter);
            }
        }
    }
}

```
### PRINT/RETURN LEVEL BY LEVEL
```java
import java.util.*;

class Solution {
    // 1. PRINT/RETURN LEVEL BY LEVEL
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> currentLevel = new ArrayList<>(); // Sublist for current level

            for (int i = 0; i < levelSize; i++) {
                TreeNode curr = queue.poll();
                
                currentLevel.add(curr.val); // <--- ADD EVERY NODE

                if (curr.left != null) queue.offer(curr.left);
                if (curr.right != null) queue.offer(curr.right);
            }

            result.add(currentLevel); // <--- Add level list to final result
        }

        return result;
    }
}
```

### Binary Tree Level Order Traversal
Given a binary tree root, return the level order traversal of it as a nested list, where each sublist contains the values of nodes at a particular level in the tree, from left to right.

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {

        List<Integer> list = new ArrayList<>();
        List<List<Integer>> finalList = new ArrayList<>();

        if(root == null) return finalList;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        queue.offer(null); // delimiter


        while(!queue.isEmpty()){
            
            TreeNode curr = queue.poll();
            
            if(curr == null){
                finalList.add(list);
                list = new ArrayList<>();

                if(!queue.isEmpty()) queue.offer(null); // delimiter
            }else{
                list.add(curr.val);
                if(curr.left != null) queue.offer(curr.left);
                if(curr.right !=null) queue.offer(curr.right);
            }

        }

        return finalList;
        
    }
}

```

### Invert Binary Tree

You are given the root of a binary tree root. Invert the binary tree and return its root.

Solution 1: Recursive Approach (DFS / Call Stack)
This approach dives recursively down the tree, swapping the left and right children at each node on the way.

```Java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;
        
        TreeNode left = root.left;
        TreeNode right = root.right;

        root.left = right;
        root.right = left;

        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
}
```
Solution 2: Iterative Approach (BFS / Queue)
This approach avoids recursion by using a Queue to process the tree level-by-level, swapping children iteratively.

```Java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while(!queue.isEmpty()){
            TreeNode curr = queue.poll();

            TreeNode left = curr.left;
            TreeNode right = curr.right;

            curr.left = right;
            curr.right = left;

            if(curr.left != null) queue.offer(curr.left);
            if(curr.right != null) queue.offer(curr.right);
        }

        return root;
    }
}
```

### Maximum Depth of Binary Tree

Given the root of a binary tree, return its depth.

The depth of a binary tree is defined as the number of nodes along the longest path from the root node down to the farthest leaf node.

## Code Review & Solution Evaluation

Both of your solutions for finding the maximum depth of a binary tree are **clever, completely functional, and correct**!

---

### Solution 1: The Wrapper-Function Approach (Recursive DFS)

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
        
    }
}

```

* **Why it's cool:** You adapted the standard height calculation (where a `null` node returns `-1`) into a depth calculation by adding `1` at the entry wrapper and offsetting the base case.
* **Cleaner Alternative:** Typically, to avoid needing a helper function or offsetting with `-1`, you can write it directly as a single function where `null` returns `0`:
```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}

```



---

### Solution 2: Level-Order Traversal with Marker (Iterative BFS)

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        queue.offer(null); // Level marker

        int depth = 0;

        while(!queue.isEmpty()){
            TreeNode curr = queue.poll();
            if(curr == null){
                depth++;
                if(queue.isEmpty()) break;
                queue.offer(null); // Add marker for the next level
            }else{
                if(curr.left != null) queue.offer(curr.left);
                if(curr.right != null) queue.offer(curr.right);
            }
        }
        return depth;
    }
}

```

* **Why it's brilliant:** Using a `null` sentinel marker as a delimiter between levels is a classic and elegant technique for tracking depths/levels in a queue without having to check `queue.size()` loops.

---


### 314. Binary Tree Vertical Order Traversal - Explanation
You are given the root node of a binary tree, return the vertical order traversal of its nodes' values.

For the vertical order traversal, list the nodes column by column starting from the leftmost column and moving to the right.

Within each column, the nodes should be listed in the order they appear from the top of the tree to the bottom.

If two nodes are located at the same row and column, the node that appears to the left should come before the other.

BFS + TREEMAP

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        if (root == null) return new ArrayList<>();

        Map<Integer, List<Integer>> map = new TreeMap<>();
        Queue<Pair<TreeNode, Integer>> queue = new LinkedList<>();
        queue.offer(new Pair<>(root, 0));

        while (!queue.isEmpty()) {
            Pair<TreeNode, Integer> p = queue.poll();
            TreeNode node = p.getKey();
            int pos = p.getValue();

            map.computeIfAbsent(pos, k -> new ArrayList<>()).add(node.val);

            if (node.left != null) queue.offer(new Pair<>(node.left, pos - 1));
            if (node.right != null) queue.offer(new Pair<>(node.right, pos + 1));
        }

        return new ArrayList<>(map.values());
    }
}
```
Time & Space Complexity
Time complexity: O(nlogn)
Space complexity: O(n)

3. Breadth First Search (Optimal) - BFS + MIN-MAX COLUMNS
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        if (root == null) return new ArrayList<>();

        Map<Integer, List<Integer>> map = new HashMap<>();
        Queue<Pair<TreeNode, Integer>> queue = new LinkedList<>();
        queue.offer(new Pair<>(root, 0));
        int minCol = 0, maxCol = 0;

        while (!queue.isEmpty()) {
            Pair<TreeNode, Integer> p = queue.poll();
            TreeNode node = p.getKey();
            int col = p.getValue();

            map.computeIfAbsent(col, x -> new ArrayList<>()).add(node.val);
            minCol = Math.min(minCol, col);
            maxCol = Math.max(maxCol, col);

            if (node.left != null) queue.offer(new Pair<>(node.left, col - 1));
            if (node.right != null) queue.offer(new Pair<>(node.right, col + 1));
        }

        List<List<Integer>> res = new ArrayList<>();
        for (int c = minCol; c <= maxCol; c++) {
            res.add(map.get(c));
        }
        return res;
    }
}
```
Time & Space Complexity
Time complexity: O(n)
Space complexity: O(n)

---

### Top View of Binary Tree
In Top View, we keep the first node encountered at each vertical column (if (!map.containsKey(col))). Because BFS processes level-by-level from top to bottom, the first node seen at any column is the topmost node.

```Java
import java.util.*;

public class Solution {
    // Helper class to hold TreeNode and its horizontal column position
    static class Pair {
        TreeNode node;
        int col;
        Pair(TreeNode node, int col) {
            this.node = node;
            this.col = col;
        }
    }

    public List<Integer> topViewBinaryTree(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Map<Integer, Integer> map = new HashMap<>();
        Queue<Pair> queue = new LinkedList<>();
        queue.offer(new Pair(root, 0));

        int minCol = 0, maxCol = 0;

        while (!queue.isEmpty()) {
            Pair p = queue.poll();
            TreeNode node = p.node;
            int col = p.col;

            // Put ONLY if column hasn't been seen before (keeps topmost node)
            if (!map.containsKey(col)) {
                map.put(col, node.val);
            }

            minCol = Math.min(minCol, col);
            maxCol = Math.max(maxCol, col);

            if (node.left != null) queue.offer(new Pair(node.left, col - 1));
            if (node.right != null) queue.offer(new Pair(node.right, col + 1));
        }

        for (int c = minCol; c <= maxCol; c++) {
            res.add(map.get(c));
        }

        return res;
    }
}

```

### Bottom View of Binary Tree
In Bottom View, we always overwrite the map value (map.put(col, node.val)) whenever a node is processed at a given column. Because BFS processes nodes level-by-level, the last node written to any column will be the bottom-most node.

```Java
import java.util.*;

public class Solution {
    // Helper class to hold TreeNode and its horizontal column position
    static class Pair {
        TreeNode node;
        int col;
        Pair(TreeNode node, int col) {
            this.node = node;
            this.col = col;
        }
    }

    public List<Integer> bottomViewBinaryTree(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Map<Integer, Integer> map = new HashMap<>();
        Queue<Pair> queue = new LinkedList<>();
        queue.offer(new Pair(root, 0));

        int minCol = 0, maxCol = 0;

        while (!queue.isEmpty()) {
            Pair p = queue.poll();
            TreeNode node = p.node;
            int col = p.col;

            // Always overwrite to ensure the LAST (bottom-most) node is stored
            map.put(col, node.val);

            minCol = Math.min(minCol, col);
            maxCol = Math.max(maxCol, col);

            if (node.left != null) queue.offer(new Pair(node.left, col - 1));
            if (node.right != null) queue.offer(new Pair(node.right, col + 1));
        }

        for (int c = minCol; c <= maxCol; c++) {
            res.add(map.get(c));
        }

        return res;
    }
}
```

### Binary Tree Views: Left View & Right View (Standard BFS Implementations)

The canonical, production-grade approach for both Left View and Right View utilizes Level-Order Traversal (Breadth-First Search) with queue.size().

Both algorithms share the exact same traversal structure; they differ only in which node index is captured during each level loop:

Left View: Capture the first element (i == 0).

Right View: Capture the last element (i == levelSize - 1).

1. Left View of Binary Tree

```Java
import java.util.*;

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> leftSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();

            for (int i = 0; i < levelSize; i++) {
                TreeNode curr = queue.poll();

                // Capture the FIRST node of the current level
                if (i == 0) {
                    result.add(curr.val);
                }

                if (curr.left != null) queue.offer(curr.left);
                if (curr.right != null) queue.offer(curr.right);
            }
        }

        return result;
    }
}
```
2. Right View of Binary Tree

```Java
import java.util.*;

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();

            for (int i = 0; i < levelSize; i++) {
                TreeNode curr = queue.poll();

                // Capture the LAST node of the current level
                if (i == levelSize - 1) {
                    result.add(curr.val);
                }

                if (curr.left != null) queue.offer(curr.left);
                if (curr.right != null) queue.offer(curr.right);
            }
        }

        return result;
    }
}
```


