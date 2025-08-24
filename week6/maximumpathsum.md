# Problem Title
Maximum Path Sum Between Any Two Nodes in a Binary Tree  

**Company:** Google  

---

## Problem Description
You are given a binary tree where each node contains an integer value.  
Your task is to find the **maximum path sum between any two nodes**.  

### Rules:
- The path must go through **at least one node**.
- The path does not need to pass through the root.
- A path is defined as a sequence of nodes connected by edges.
- Each node can appear only once in the path.

---

## Approach

This is a **post-order traversal** problem.  
For each node, we compute:

1. The maximum path sum **using either the left or right child**.
2. The maximum path sum **including both children plus the current node**.  
3. Use a global variable to track the maximum across all nodes.

---

## Java Solution

```java
// Definition for a binary tree node.
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) {
        this.val = val;
        left = right = null;
    }
}

public class MaxPathSumBinaryTree {
    // Global variable to store the maximum path sum
    private static int maxSum;

    public static int maxPathSum(TreeNode root) {
        maxSum = Integer.MIN_VALUE;
        helper(root);
        return maxSum;
    }

    private static int helper(TreeNode node) {
        if (node == null) return 0;

        // Compute max path on left and right, ignoring negative paths
        int left = Math.max(helper(node.left), 0);
        int right = Math.max(helper(node.right), 0);

        // Path through current node (can include both children)
        int current = node.val + left + right;

        // Update global maximum
        maxSum = Math.max(maxSum, current);

        // Return max path upward including current node + one child
        return node.val + Math.max(left, right);
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(-10);
        root.left = new TreeNode(9);
        root.right = new TreeNode(20);
        root.right.left = new TreeNode(15);
        root.right.right = new TreeNode(7);

        System.out.println("Maximum Path Sum: " + maxPathSum(root));
    }
}
