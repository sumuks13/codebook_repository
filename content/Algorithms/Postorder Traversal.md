tags: [[Algorithms/Algorithms]], [[Depth First Search]]

### **Algorithm**

- Recursively visit **left subtree**, then **right subtree**, then **parent node**.
- (Left → Right → Root )

|                      | **Complexity** | **Explanation**                                   |
| -------------------- | -------------- | ------------------------------------------------- |
| **Time Complexity**  | O(n)           | Every node is visited exactly once                |
| **Space Complexity** | O(h)           | `h` = height of the tree (due to recursion stack) |

```run-java
import java.util.*;
public class Solution {
	public void postorder(TreeNode root) {
	    if (root == null)
		    return;

	    postorder(root.left);
	    postorder(root.right);
	    System.out.print(root.val + " ");
	}
    
    public static void main(String[] args) {

		TreeNode root = new TreeNode(1);
		root.left = new TreeNode(2);
		root.right = new TreeNode(3);
		root.right.left = new TreeNode(4);
	            
		Solution sol = new Solution();
        sol.postorder(root);
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
