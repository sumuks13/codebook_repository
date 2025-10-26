Tags: [[Data Structures]], [[Graph]]
### **What is a Tree Data Structure? Explain its Properties.**

A **tree** is a hierarchical data structure consisting of **nodes** connected by **edges**. It starts with a **root node** and branches out into child nodes.  

**Properties:**

- **Root Node:** The topmost node.
- **Parent & Child Nodes:** Nodes connected directly.
- **Leaf Nodes:** Nodes with no children.
- **Depth:** Distance from the root.
- **Height:** Maximum depth of any node.
### **What are the Different Types of Trees?**

- **Binary Tree:** Each node has at most **two children**.
- **Binary Search Tree (BST):** Left child < Root < Right child.
- **AVL Tree:** Self-balancing BST.
- **Heap:** Used in priority queues.
- **Trie:** Used for word searches.
- **B-Tree:** Used in databases.

### **How Do You Represent a Tree in Programming?**

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
 
### **Difference Between Binary Tree & General Tree?**

- **Binary Tree:** Each node has **at most two children**.
- **General Tree:** Each node can have **multiple children**.

### **Types of Tree Traversals?**

- **DFS Traversals:** Preorder, Inorder, Postorder.
- **BFS Traversal:** Level-order traversal.

### **Explain DFS (Depth-First Search).**

DFS explores **deep into one branch** before backtracking. 
**Types of DFS Traversals:**

- **Preorder (Root → Left → Right)**

```java
void preorder(Node root) {
    if (root != null) {
        System.out.print(root.data + " ");
        preorder(root.left);
        preorder(root.right);
    }
}
```

- **Inorder (Left → Root → Right)**

```java
void inorder(Node root) {
    if (root != null) {
        inorder(root.left);
        System.out.print(root.data + " ");
        inorder(root.right);
    }
}
```

- **Postorder (Left → Right → Root)**

```java
void postorder(Node root) {
    if (root != null) {
        postorder(root.left);
        postorder(root.right);
        System.out.print(root.data + " ");
    }
}
```

### **Explain BFS (Breadth-First Search).**

BFS explores **level by level**, visiting all nodes at a given depth before moving deeper. Level order traversal is done with the help of Queue.

```java
void levelOrder(TreeNode root) {
    Queue<TreeNode> queue = new LinkedList<>();
                
    if(root == null) return;
    queue.add(root);

    while(!queue.isEmpty()){
	    int currentLevel = queue.size();

        for(int i=0; i<currentLevel; i++){
            TreeNode cur = queue.remove();
                
            if(cur.left != null) queue.add(cur.left);
            if(cur.right != null) queue.add(cur.right);
            
	        System.out.print(cur.data + " ");
        }
    }
}
```

### **What is the Height & Depth of a Tree?**

- **Height:** Distance from root to the deepest leaf.
- **Depth:** Distance from a node to the root.

### **What is a Balanced Tree? Why is it Important?**

A tree where height difference between left and right subtrees is **≤ 1**.  
**Importance:** Ensures efficient operations.

### **Complete Tree vs. Full Tree?**

- **Complete Tree:** All levels are filled **except possibly the last**.
- **Full Tree:** Every node has **either 0 or 2 children**.

### **Applications of Trees in Real-World Scenarios?**

- **File Systems**
- **Databases**
- **AI Decision Trees**
- **Network Routing**
- **Expression Evaluation**

### **Calculate Max Depth of a Binary Tree**

To calculate max depth of a binary tree, 
1. Check the max between depth of left node and depth of right node.
2. Add 1 to include the current node.

Max depth = max(left subtree depth, right subtree depth) + 1

```java
public int maxDepth(TreeNode root) {
        if(root == null)
            return 0;

        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
```

### **How Do You Check if Two Trees are Identical?**

```java
boolean isIdentical(Node a, Node b) {
    if (a == null && b == null) return true;
    if (a == null || b == null) return false;
    return (a.data == b.data) && isIdentical(a.left, b.left) && isIdentical(a.right, b.right);
}
```

### **Lowest Common Ancestor (LCA) of Two Nodes?**

```java
Node lca(Node root, int n1, int n2) {
    if (root == null) return null;
    if (root.data == n1 || root.data == n2) return root;
    
    Node left = lca(root.left, n1, n2);
    Node right = lca(root.right, n1, n2);
    
    if (left != null && right != null) return root;
    return (left != null) ? left : right;
}
```

### **Diameter of a Tree (Longest Path Between Two Nodes)?**

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

### **Check if a Tree is Symmetric?**

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

### **Construct a Tree from Inorder & Preorder/Postorder Traversals?**

Recursively identify **root** and divide into **left and right subtrees**.

### **Serialize & Deserialize a Tree?**

```java
String serialize(Node root) {
    if (root == null) return "null,";
    return root.data + "," + serialize(root.left) + serialize(root.right);
}
```


> [!QUOTE] **References**
> -  https://www.freecodecamp.org/news/all-you-need-to-know-about-tree-data-structures-bceacb85490c/

