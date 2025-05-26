Tags: [[Data Structures]]
#### **1. What is a Tree Data Structure? Explain its Properties.**

A **tree** is a hierarchical data structure consisting of **nodes** connected by **edges**. It starts with a **root node** and branches out into child nodes.  
**Properties:**

- **Root Node:** The topmost node.
- **Parent & Child Nodes:** Nodes connected directly.
- **Leaf Nodes:** Nodes with no children.
- **Depth:** Distance from the root.
- **Height:** Maximum depth of any node.
#### **2. What are the Different Types of Trees?**

- **Binary Tree:** Each node has at most **two children**.
- **Binary Search Tree (BST):** Left child < Root < Right child.
- **AVL Tree:** Self-balancing BST.
- **Heap:** Used in priority queues.
- **Trie:** Used for word searches.
- **B-Tree:** Used in databases.

#### **3. How Do You Represent a Tree in Programming?**

```java
class Node {
    int data;
    Node left, right;
    Node(int value) {
        data = value;
        left = right = null;
    }
}
```
 
#### **4. Difference Between Binary Tree & General Tree?**

- **Binary Tree:** Each node has **at most two children**.
- **General Tree:** Each node can have **multiple children**.

#### **5. Types of Tree Traversals?**

- **DFS Traversals:** Preorder, Inorder, Postorder.
- **BFS Traversal:** Level-order traversal.

#### **6. Explain DFS (Depth-First Search).**

DFS explores **deep into one branch** before backtracking.  
**Types of DFS Traversals:**

- **Preorder (Root → Left → Right)**
- **Inorder (Left → Root → Right)**
- **Postorder (Left → Right → Root)**

#### **7. Explain BFS (Breadth-First Search).**

BFS explores **level by level**, visiting all nodes at a given depth before moving deeper.

#### **8. Differences Between Preorder, Inorder & Postorder Traversal (With Code).**

```java
void inorder(Node root) {
    if (root != null) {
        inorder(root.left);
        System.out.print(root.data + " ");
        inorder(root.right);
    }
}
```

```java
void preorder(Node root) {
    if (root != null) {
        System.out.print(root.data + " ");
        preorder(root.left);
        preorder(root.right);
    }
}
```

```java
void postorder(Node root) {
    if (root != null) {
        postorder(root.left);
        postorder(root.right);
        System.out.print(root.data + " ");
    }
}
```

#### **9. What is the Height & Depth of a Tree?**

- **Height:** Distance from root to the deepest leaf.
- **Depth:** Distance from a node to the root.

#### **10. What is a Balanced Tree? Why is it Important?**

A tree where height difference between left and right subtrees is **≤ 1**.  
**Importance:** Ensures efficient operations.

#### **11. Complete Tree vs. Full Tree?**

- **Complete Tree:** All levels are filled **except possibly the last**.
- **Full Tree:** Every node has **either 0 or 2 children**.

#### **12. Applications of Trees in Real-World Scenarios?**

- **File Systems**
- **Databases**
- **AI Decision Trees**
- **Network Routing**
- **Expression Evaluation**

#### **13. How Do You Check if Two Trees are Identical?**

```java
boolean isIdentical(Node a, Node b) {
    if (a == null && b == null) return true;
    if (a == null || b == null) return false;
    return (a.data == b.data) && isIdentical(a.left, b.left) && isIdentical(a.right, b.right);
}
```

#### **14. Lowest Common Ancestor (LCA) of Two Nodes?**

```java
Node lca(Node root, int n1, int n2) {
    if (root == null) return null;
    if (root.data == n1 || root.data == n2) return root;
    Node left = lca(root.left, n1, n2);
    Node right = lca(root.right, n1, n2);
    return (left != null && right != null) ? root : (left != null ? left : right);
}
```

#### **15. Diameter of a Tree (Longest Path Between Two Nodes)?**

```java
int diameter(Node root) {
    if (root == null) return 0;
    int leftHeight = height(root.left);
    int rightHeight = height(root.right);
    int leftDiameter = diameter(root.left);
    int rightDiameter = diameter(root.right);
    return Math.max(leftHeight + rightHeight + 1, Math.max(leftDiameter, rightDiameter));
}
```

#### **16. Check if a Tree is Symmetric?**

```java
boolean isSymmetric(Node root) {
    return isMirror(root, root);
}

boolean isMirror(Node t1, Node t2) {
    if (t1 == null && t2 == null) return true;
    if (t1 == null || t2 == null) return false;
    return (t1.data == t2.data) && isMirror(t1.left, t2.right) && isMirror(t1.right, t2.left);
}
```

#### **17. Construct a Tree from Inorder & Preorder/Postorder Traversals?**

Recursively identify **root** and divide into **left and right subtrees**.

#### **18. Serialize & Deserialize a Tree?**

```java
String serialize(Node root) {
    if (root == null) return "null,";
    return root.data + "," + serialize(root.left) + serialize(root.right);
}
```


> [!QUOTE] **References**
> -  https://www.freecodecamp.org/news/all-you-need-to-know-about-tree-data-structures-bceacb85490c/

