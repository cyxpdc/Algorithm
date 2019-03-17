给定一个 N 叉树，返回其节点值的层序遍历。 (即从左到右，逐层遍历)。

例如，给定一个 3叉树 :

返回其层序遍历:

[
     [1],
     [3,2,4],
     [5,6]
]



>emmm不知道错在哪里
```java
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
    
    public List<List<Integer>> levelOrder(Node root) {
		List<List<Integer>> res = new ArrayList<>();
        if(root != null) {
        	Deque<Node> curQ = new ArrayDeque<>();//当前层次的节点
                Deque<Node> curChildQ = new ArrayDeque<>();//当前层次的孩子节点
                List<Integer> curLevelResult = new ArrayList<>();//当前层次的值
        	curQ.offer(root);
        	while(!curQ.isEmpty()){
                //添加子节点
                while(!curQ.isEmpty()){
                    Node curNode = curQ.poll();
                    if(curNode.children != null){
                        for(Node child : curNode.children){
                            curChildQ.offer(child);
                        }
                    }
                    curLevelResult.add(curNode.val);
                }
                curQ = curChildQ;
                res.add(curLevelResult);
                curChildQ.clear();
                curLevelResult.clear();
            }
        }
    	return res;
    }
}
```
>经过下面解法的启发修改一下，通过成功
>尽量用局部变量
```java
class Solution {
    
    public List<List<Integer>> levelOrder(Node root) {
		List<List<Integer>> res = new ArrayList<>();
        if(root == null) return res;
        Deque<Node> curQ = new ArrayDeque<>();//当前层次的节点
        curQ.offer(root);
        while(!curQ.isEmpty()){
            List<Integer> curLevelResult = new ArrayList<>();//当前层次的值
            Deque<Node> curChildQ = new ArrayDeque<>();//当前层次的孩子节点
            //添加子节点
            while(!curQ.isEmpty()){
                Node curNode = curQ.poll();
                if(curNode.children != null){
                    for(Node child : curNode.children){
                        curChildQ.offer(child);
                    }
                }
                curLevelResult.add(curNode.val);
            }
            curQ = curChildQ;
            res.add(curLevelResult);
        }
    	return res;
    }
}
```
>https://leetcode.com/problems/n-ary-tree-level-order-traversal/discuss/134911/Java-Solution
```java
*/
class Solution{
    
    public List<List<Integer>> levelOrder(Node root) {
            List<List<Integer>> res = new LinkedList<>();
            if (root == null) return res;
            Queue<Node> queue = new LinkedList<>();
            queue.offer(root);
            while (!queue.isEmpty()) {
                List<Integer> curLevel = new LinkedList<>();
                int len = queue.size();
                for (int i = 0; i < len; i++) {
                    Node curr = queue.poll();
                    curLevel.add(curr.val);
                    for (Node c : curr.children)
                        queue.offer(c);
                }
                res.add(curLevel);
            }
            return res;
    }
}
```
