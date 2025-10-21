tags: [[Binary Search]], [[Data Structures/Tree]]

### **What is a Binary Search Tree?**

A Binary Search Tree is a special kind of binary tree where each node follows a specific rule:

- The **left child** must contain a value **less than** the parent node.
- The **right child** must contain a value **greater than** the parent node.

This structure helps in efficient searching, insertion, and deletion operations.

### **How do you search for a value in a BST?**

1. Start at the root node.
2. If the value matches the current node, return true.
3. If the value is smaller, move to the left child.
4. If the value is larger, move to the right child.
5. Repeat until you find the value or reach a null node.

```java
boolean search(TreeNode root, int key) {
    if (root == null) return false;
    if (key == root.val) return true;
    if (key < root.val) return search(root.left, key);
    else return search(root.right, key);
}
```

### **How do you insert a value into a BST?**

1. Start at the root.
2. Compare the value with the current node.
3. If the value smaller, go to the left child.
4. If the value larger, go to the right child.
5. If you reach a null spot, create and return a new node there.

```java
TreeNode insert(TreeNode root, int key) {
    if (root == null) return new TreeNode(key);
    if (key < root.val) root.left = insert(root.left, key);
    else root.right = insert(root.right, key);
    return root;
}
```

### **How do you delete a node from a BST?**

Deleting a node involves three cases:

1. **Node has no children**: Simply remove it.
2. **Node has one child**: Replace it with its child.
3. **Node has two children**: 
	1. Find the smallest value in the right subtree (called the inorder successor).
	2. Replace the node’s value with it.
	3. Delete the successor.

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        // Base case
        if (root == null) {
            return root;
        }

        // Go left if key is smaller than the current node
        if (key < root.val) {
            root.left = deleteNode(root.left, key);
        }
        // Go right if key is greater than the current node
        else if (key > root.val) {
            root.right = deleteNode(root.right, key);
        }
        // The current node is the one to be deleted
        else {
            // Case 1: Node has no children (leaf node)
            if (root.left == null && root.right == null) {
	            // Replaces the current node with null
                return null;
            }

            // Case 2: Node has only one child
            else if (root.left == null || root.right == null) {
                // Replace the the current node with the non-null child
                return (root.left == null) ? root.right : root.left;
            }

            // Case 3: Node has two children
            else {
                // Find the inorder successor (smallest value in the right subtree)
                TreeNode successor = root.right;
                while (successor.left != null) {
                    successor = successor.left;
                }

                // Replace current node's value with successor's value
                root.val = successor.val;

                // Delete the successor node from the right subtree
                root.right = deleteNode(root.right, root.val);

                // Return the updated current node
                return root;
            }
        }

        // Return the unchanged root if no deletion occurred at this level
        return root;
    }
}
```

### **How do you find the minimum and maximum value in a BST?**

To find the **minimum**, keep moving to the left child until you reach a node with no left child.  
To find the **maximum**, keep moving to the right child until you reach a node with no right child.

### **How do you check if a Tree is a valid BST?**

You can validate a BST by ensuring every node follows the BST rule across the entire tree: 
- left child < node < right child. 

This is done using recursion with min and max bounds.

```java
boolean isValidBST(TreeNode root, Integer min, Integer max) {
    if (root == null) return true;
    if ((min != null && root.val <= min) || (max != null && root.val >= max)) return false;
    return isValidBST(root.left, min, root.val) && isValidBST(root.right, root.val, max);
}
```

### **What are Ceil and Floor in a BST?**

- **Ceil** is the smallest value in the BST that is **greater than or equal to** a given number.
- **Floor** is the largest value that is **less than or equal to** the number.

These are useful in range queries and nearest-value lookups.

### **How do you print all values in a range?**

To print values between two numbers (say `low` and `high`), use an inorder traversal and only print nodes that fall within the range.

### **How do you find the k-th smallest or largest element?

An inorder traversal gives nodes in sorted order. We keep track of how many nodes we've visited. When the counter reaches k, we return that node's value. 

- The **k-th smallest** is the k-th node visited in inorder.

```java
class Solution {
    private int count = 0;
    private int result = -1;

    public int kthSmallest(TreeNode root, int k) {
        inorder(root, k);
        return result;
    }

    private void inorder(TreeNode node, int k) {
        if (node == null) return;

        inorder(node.left, k);

        count++;
        if (count == k) {
            result = node.val;
            return;
        }

        inorder(node.right, k);
    }
}
```

- The **k-th largest** is the k-th node from the end.

```java
class Solution {
    private int count = 0;
    private int result = -1;

    public int kthLargest(TreeNode root, int k) {
        reverseInorder(root, k);
        return result;
    }

    private void reverseInorder(TreeNode node, int k) {
        if (node == null) return;

        reverseInorder(node.right, k);

        count++;
        if (count == k) {
            result = node.val;
            return;
        }

        reverseInorder(node.left, k);
    }
}
```
