给定一个 N 叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。

>对于每个节点，都得到其孩子节点的最大深度，再加1即可
```java
/*
// Definition for a Node.
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val,List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
        
    public static int maxDepth(Node root) {
        if(root == null) return 0;
        
        int childMax = 0;//孩子节点的最大值
        for(Node child : root.children) {
            childMax = Math.max(childMax,maxDepth(child));
        }
        
        return 1 + childMax;
    }
}
```
