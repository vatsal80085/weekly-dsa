###Problem Statement: Count Unival Subtrees
the question asked by Google

###Problem Description:
Scenario:
In many systems, especially in distributed trees or replicated data structures, it's important to
find substructures that are uniform. A unival tree (short for universal value tree) is a subtree
where all the nodes have the same value.
You are given the root of a binary tree, and your task is to count the number of unival
subtrees present in the tree.
A single node is trivially considered a unival subtree.

###Approach Used :
Used post-order traversal to:
• Recursively check left and right subtrees
• Determining if current node forms a unival subtree:
• Left and right subtrees are unival.
• Node’s value matches children’s (if they exist).
Maintaining a counter to keep track of valid unival subtrees.
Then returning the value is the required solution.

###Key Observations
Every leaf node is a unival subtree.

For a non-leaf node to be unival:

Both its left and right subtrees must be unival (or None).

If present, the values of left and right children must match the current node's value.

```java
class Solution {
    private int count = 0;

    public int countUnivalSubtrees(TreeNode root) {
        isUnival(root);
        return count;
    }

    private boolean isUnival(TreeNode node) {
        if (node == null) {
            return true;
        }

        boolean leftUnival = isUnival(node.left);
        boolean rightUnival = isUnival(node.right);

        if (!leftUnival || !rightUnival) {
            return false;
        }

        if (node.left != null && node.left.val != node.val) {
            return false;
        }

        if (node.right != null && node.right.val != node.val) {
            return false;
        }

        count++;
        return true;
    }
}
```
