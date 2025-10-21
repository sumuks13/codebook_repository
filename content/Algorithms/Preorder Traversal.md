tags: [[Algorithms/Algorithms]], [[Depth First Search]]

### **Algorithm**

- Visit **parent node first**, then recursively visit **left subtree**, then **right subtree**.
- (Root → Left → Right)

|                      | **Complexity** | **Explanation**                                   |
| -------------------- | -------------- | ------------------------------------------------- |
| **Time Complexity**  | O(n)           | Every node is visited exactly once                |
| **Space Complexity** | O(h)           | `h` = height of the tree (due to recursion stack) |

```java
import java.util.*;
public class Solution {
	public void preorder(TreeNode root) {
	    if (root == null)
		    return;

	    System.out.print(root.val + " ");
	    preorder(root.left);
	    preorder(root.right);
	}
    
    public static void main(String[] args) {

		TreeNode root = new TreeNode(1);
		root.left = new TreeNode(2);
		root.right = new TreeNode(3);
		root.right.left = new TreeNode(4);
	            
		Solution sol = new Solution();
        sol.preorder(root);
    }
}

class TreeNode {
    int val;
    TreeNode left, right;

    TreeNode(int val) {
        this.val = val;
    }

    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```
