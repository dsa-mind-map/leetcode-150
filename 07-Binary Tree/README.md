
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

## Node Definition
```java
class Node {
    int data;
    Node left;
    Node right;

    Node(int val) {
        data = val;
        left = null;
        right = null;
    }
}

```

---

## 1. DFS (Depth-First Search) Traversals

### Recursive Pre-Order

* **Logic:** Visit current node $\rightarrow$ Visit left subtree preorder $\rightarrow$ Visit right subtree preorder.
* **Sources:** Code Campaign & Phyley CS

```java
public static void preOrder(Node root) {
    if (root == null) return;
    System.out.print(root.data + " ");
    preOrder(root.left);
    preOrder(root.right);
}

```

### Recursive In-Order

* **Logic:** Visit left subtree In-Order $\rightarrow$ Visit current node $\rightarrow$ Visit right subtree In-Order.
* **Sources:** Code Campaign & Phyley CS

```java
public static void inOrder(Node root) {
    if (root == null) return;
    inOrder(root.left);
    System.out.print(root.data + " ");
    inOrder(root.right);
}

```

### Recursive Post-Order

* **Logic:** Visit left subtree Post-Order $\rightarrow$ Visit right subtree Post-Order $\rightarrow$ Visit current node.
* **Sources:** Code Campaign & Phyley CS

```java
public static void postOrder(Node root) {
    if (root == null) return;
    postOrder(root.left);
    postOrder(root.right);
    System.out.print(root.data + " ");
}

```

### Iterative Pre-Order

* **Logic:** While stack is not empty:
1. Keep popping & printing top.
2. Push right child, then left child (ensuring left is processed first due to LIFO).


* **Sources:** Phyley CS & Code Campaign

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
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();

        if(root == null) return result;

        Stack<TreeNode> stack = new Stack<>();

        stack.push(root);

        while(!stack.isEmpty()){
            TreeNode top = stack.pop();
            result.add(top.val);
            if(top.right != null) stack.push(top.right);
            if(top.left != null) stack.push(top.left); // LIFO
        }
        return result; 
    }
}

```


### Iterative In-Order (Code Campaign Style - If-Else)

* **Logic:** Uses an explicit branching condition inside the main traversal loop to handle stack pushes and right shifts.
* **Source:** Code Campaign (Video #3)

```java/**
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
    public List<Integer> inorderTraversal(TreeNode root) {
 
        List<Integer> result = new ArrayList<>();
        if(root == null) return result;

        Stack<TreeNode> stack = new Stack<>();

        TreeNode curr = root;
        while(curr != null || !stack.isEmpty()){
            
            if(curr != null){
                stack.push(curr);
                curr = curr.left;
            }else{
                
                TreeNode top = stack.pop(); // last of left nodes
                result.add(top.val);
                curr = top.right; // move to the right node -> while loop iterate on this node in next iteration
            }
        }
        
        return result;
    }
}

```

### Iterative Post-Order using 2 Stacks

* **Logic:**
1. Keep popping from primary stack & push to result stack.
2. Push left child, then right child to primary stack.
3. Pop all elements from the result stack to print in correct post-order.


* **Source:** Phyley CS

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
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();

        if(root == null) return result;

        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();

        stack1.push(root);
        while(!stack1.isEmpty()){
            TreeNode top = stack1.pop();
            stack2.push(top);
            if(top.left != null) stack1.push(top.left);
            if(top.right != null) stack1.push(top.right);
        }

        while(!stack2.isEmpty()){
            result.add(stack2.pop().val);
        }

        return result;
        
    }
}

```

### Iterative Post-Order using 1 Stack

* **Logic:** Simulates recursive stack unwinding via a single stack, tracking right-child node states to process `Left -> Right -> Root`.
* **Source:** Phyley CS

```java
public static void postOrderIterative1Stack(Node root) {
    if (root == null) return;

    Stack<Node> st = new Stack<>();
    Node current = root;

    while (current != null || !st.isEmpty()) {

        // Phase 1: Dive all the way to the leftmost node, 
        // pushing every node along the way onto the stack.
        if (current != null) {
            st.push(current);
            current = current.left;
        } 
        // Phase 2: We hit a null left child. Now we need to inspect 
        // the top of the stack to see what to do next.
        else {
            Node peekNode = st.peek();

            // Case A: If the node at the top of the stack has a right child 
            // and we haven't visited it yet, pivot and go down that right subtree.
            if (peekNode.right != null && current != peekNode.right) {
                current = peekNode.right;
            } 
            // Case B: The node has no right child, OR its right child 
            // has already been visited. That means this node is fully done!
            else {
                Node visitedNode = st.pop();
                System.out.print(visitedNode.data + " ");
                
                // Keep track of what we just processed so the outer loop 
                // knows we are coming up from a right child.
                current = visitedNode; 
            }
        }
    }
}
```

```java
public void postOrderIterative1Stack(Node root) {
    if (root == null) return;
    java.util.Stack<Node> st = new java.util.Stack<>();
    Node current = root;
    while (current != null || !st.isEmpty()) {
        if (current != null) {
            st.push(current);
            current = current.left;
        } else {
            Node temp = st.peek().right;
            if (temp == null) {
                temp = st.pop();
                System.out.print(temp.data + " ");
                while (!st.isEmpty() && temp == st.peek().right) {
                    temp = st.pop();
                    System.out.print(temp.data + " ");
                }
            } else {
                current = temp;
            }
        }
    }
}

```

---

## 2. Tree Properties & Structural Checks (DFS)

### Find Height of a Binary Tree

```java
public int height(Node root) {
    if (root == null) return -1;
    int leftHeight = height(root.left);
    int rightHeight = height(root.right);
    return Math.max(leftHeight, rightHeight) + 1;
}

```

### Find Size of a Binary Tree

```java
public int findSize(Node root) {
    if (root == null) return 0;
    return findSize(root.left) + findSize(root.right) + 1;
}

```

### Find Node in Binary Tree

```java
public boolean findNode(Node root, int target) {
    if (root == null) return false;
    if (root.data == target) return true;
    return findNode(root.left, target) || findNode(root.right, target);
}

```

### Check If Two Binary Trees Are Equal

```java
public boolean areEqual(Node root1, Node root2) {
    if (root1 == null && root2 == null) return true;
    if (root1 == null || root2 == null) return false;
    if (root1.data != root2.data) return false;
    return areEqual(root1.left, root2.left) && areEqual(root1.right, root2.right);
}

```

---

## 3. BFS (Breadth-First Search) & Level Order Variations

### Standard Level Order Traversal

```java
public void levelOrder(Node root) {
    if (root == null) return;
    java.util.Queue<Node> q = new java.util.LinkedList<>();
    q.add(root);
    while (!q.isEmpty()) {
        Node current = q.poll();
        System.out.print(current.data + " ");
        if (current.left != null) q.add(current.left);
        if (current.right != null) q.add(current.right);
    }
}

```

### Reverse Level Order Traversal

```java
public void reverseLevelOrder(Node root) {
    if (root == null) return;
    java.util.Queue<Node> q = new java.util.LinkedList<>();
    java.util.Stack<Node> st = new java.util.Stack<>();
    q.add(root);
    while (!q.isEmpty()) {
        Node current = q.poll();
        st.push(current);
        if (current.right != null) q.add(current.right);
        if (current.left != null) q.add(current.left);
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

```

```
