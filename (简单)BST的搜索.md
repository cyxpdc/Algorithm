给定二叉搜索树（BST）的根节点和一个值。 你需要在BST中找到节点值等于给定值的节点。 

返回以该节点为根的子树。 如果节点不存在，则返回 NULL。
>迭代
```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) return null;
        
        while (root != null && val != root.val) {
            root = val < root.val ? root.left : root.right;
        }
        return root;
    }
}
```
>递归
```java
class Solution {
    public TreeNode searchBST(TreeNode node, int val) {
        if (node == null || node.val == val) {
            return node;
        }
        
        if (val < node.val) {
            return searchBST(node.left, val);     
        } else {
            return searchBST(node.right, val);
        }
    }
}
```
