Visit current node
visit left subtree preorder
visit right subtree preorder

// Recursive Pre-Order (Source: Code Campaign & Phyley CS)
Visit current node
visit left subtree preorder
visit right subtree preorder
public static void preOrder(Node root) {
    if (root == null) return;
    System.out.print(root.data + " ");
    preOrder(root.left);
    preOrder(root.right);
}

// Recursive In-Order (Source: Code Campaign & Phyley CS)
visit left subtree In-Order
Visit current node
visit right subtree In-Order
public static void inOrder(Node root) {
    if (root == null) return;
    inOrder(root.left);
    System.out.print(root.data + " ");
    inOrder(root.right);
}

// Recursive Post-Order (Source: Code Campaign & Phyley CS)
visit left subtree Post-Order
visit right subtree Post-Order
Visit current node

public static void postOrder(Node root) {
    if (root == null) return;
    postOrder(root.left);
    postOrder(root.right);
    System.out.print(root.data + " ");
}

// Iterative Pre-Order (Source: Phyley CS & Code Campaign)
public void preOrderIterative(Node root) {
    if (root == null) return;
    java.util.Stack<Node> st = new java.util.Stack<>();
    st.push(root);
    while (!st.isEmpty()) {
        Node current = st.pop();
        System.out.print(current.data + " ");
        if (current.right != null) st.push(current.right);
        if (current.left != null) st.push(current.left);
    }
}

// Iterative In-Order (Source: Phyley CS & Code Campaign)
public void inOrderIterative(Node root) {
    java.util.Stack<Node> st = new java.util.Stack<>();
    Node current = root;
    while (current != null || !st.isEmpty()) {
        while (current != null) {
            st.push(current);
            current = current.left;
        }
        current = st.pop();
        System.out.print(current.data + " ");
        current = current.right;
    }
}

// Iterative Post-Order using 2 Stacks (Source: Phyley CS)
public void postOrderIterative2Stacks(Node root) {
    if (root == null) return;
    java.util.Stack<Node> st = new java.util.Stack<>();
    java.util.Stack<Node> result = new java.util.Stack<>();
    st.push(root);
    while (!st.isEmpty()) {
        Node current = st.pop();
        result.push(current);
        if (current.left != null) st.push(current.left);
        if (current.right != null) st.push(current.right);
    }
    while (!result.isEmpty()) {
        System.out.print(result.pop().data + " ");
    }
}

// Iterative Post-Order using 1 Stack (Source: Phyley CS)
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
