tags : [[Algorithms]]

![[Pasted image 20240602194636.png]]

**Inorder** : left, parent, right -> left to right
**Preorder**: parent, left, right
**Postorder**: left, right, parent

```run-java
import java.util.*;

class Driver {
	public static void main(String[] args)
	{
		Node root = new Node(1);
		root.left = new Node(2);
		root.right = new Node(3);
		root.left.left = new Node(4);
		root.left.right = new Node(5);
		root.right.left = new Node(6);
		root.right.right = new Node(7);

		// Function call
		System.out.println("Postorder traversal of binary tree is: ");
		TreeTraversal.printPostorder(root);
		System.out.println("\nPreorder traversal of binary tree is: ");
		TreeTraversal.printPreorder(root);
		System.out.println("\nInorder traversal of binary tree is: ");
		TreeTraversal.printInorder(root);
	}

}
// Structure of a Binary Tree Node
class Node {
	int data;
	Node left, right;
	Node(int v)
	{
		data = v;
		left = right = null;
	}
}

class TreeTraversal {

	static void printPostorder(Node node)
	{
		if (node == null)
			return;

		printPostorder(node.left);
		printPostorder(node.right);
		System.out.print(node.data + " ");
	}

	static void printPreorder(Node node)
	{
		if (node == null)
			return;

		System.out.print(node.data + " ");
		printPreorder(node.left);
		printPreorder(node.right);
		
	}

	static void printInorder(Node node)
	{
		if (node == null)
			return;

		printInorder(node.left);
		System.out.print(node.data + " ");
		printInorder(node.right);
		
	}

}

```
