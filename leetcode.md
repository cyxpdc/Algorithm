给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

**说明:**

如果你可以运用递归和迭代两种方法解决这个问题，会很加分。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
```



思路1(失败):

想法很美好，可惜AC不过。。。

错误出在判断层数的逻辑上，代码不应该放在这个位置

使用层次遍历，把每一层的节点放到一个数组里面，进行回文比较

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
//层次遍历
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null){
            return false;
        }
        int level = 0;
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode node = queue.poll();
            //判断层数
            if(node == root){
                level = 1;
            }else{
                level++;
            }
            //如果有左节点，入队列，如果没有，当作null入,以保证充满二叉树
            if(node.left != null){
                queue.offer(node.left);
            }else{
                queue.offer(null);
            }
            //如果有右节点，入队列，如果没有，当作null入,以保证充满二叉树
            if(node.right != null){
                queue.offer(node.right);
            }else{
                queue.offer(null);
            }
            //根据层数来判断是否对称
            //注:二叉树第level层上的结点数目最多为2^(level-1) (level≥1)。
            if(level > 1){
                //用来判断每一层节点的数组
                TreeNode[] nodes = new TreeNode[(int)(Math.pow(2,level-1))];
                for(int i = 0;i < Math.pow(2,level-1);i++){
                    TreeNode temp = queue.poll();
                    nodes[i] = temp;
                }
                for(int i = 0;i < nodes.length/2;++i){
                    if(nodes[i].val != nodes[nodes.length-1-i].val){
                        return false;
                    }                   
                }
            }
        }
        if(level == 1){
            return false;
        }
        return true;
    }
}

这个错在下面的数组判断。。。
//层次遍历
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null){
            return false;
        }
        ArrayList<TreeNode> nodeList = new ArrayList<>();
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode node = queue.poll();
            nodeList.add(node);
            if(node.left != null){
                queue.offer(node.left);
            }else{
                queue.offer(null);
            }
            if(node.right != null){
                queue.offer(node.right);
            }else{
                queue.offer(null);
            }
        }
        //[1,2,2,3,4,4,3]
        TreeNode[] nodes = (TreeNode[])nodeList.toArray();
        for(int i = 0;i < nodes.length/2;++i){
            if(nodes[i].val != nodes[nodes.length-1-i].val){
                return false;
            }
        }
        return true;
    }
}
```





leetcode官方题解:(双树)

```java

//递归
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return isMirror(root, root);
    }

    public boolean isMirror(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null) return true;
        if (t1 == null || t2 == null) return false;
        return (t1.val == t2.val)
            && isMirror(t1.left, t2.right)
            && isMirror(t1.right, t2.left);
    }
}
```

```java
//迭代
class Solution {
    public boolean isSymmetric(TreeNode root) {
    Queue<TreeNode> q = new LinkedList<>();
    q.add(root);
    q.add(root);
    while (!q.isEmpty()) {
        TreeNode t1 = q.poll();
        TreeNode t2 = q.poll();
        if (t1 == null && t2 == null) continue;
        if (t1 == null || t2 == null) return false;
        if (t1.val != t2.val) return false;
        q.add(t1.left);
        q.add(t2.right);
        q.add(t1.right);
        q.add(t2.left);
    }
    return true;
    }
}
```

