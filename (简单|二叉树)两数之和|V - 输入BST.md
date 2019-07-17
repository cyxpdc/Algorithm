给定一个二叉搜索树和一个目标结果，如果 BST 中存在两个元素且它们的和等于给定的目标结果，则返回 true。

>使用链表存储已出现过的数；使用中序遍历使链表中的元素有序

>https://leetcode.com/problems/two-sum-iv-input-is-a-bst/solution/
```java
public class Solution {
    
    public boolean findTarget(TreeNode root, int k) {
        List<Integer>list = new ArrayList<>();
        inorder(root, list);
        int l = 0, r = list.size() - 1;
        while (l < r) {
            int sum = list.get(l) + list.get(r);
            if (sum == k)
                return true;
            if (sum < k)
                l++;
            else
                r--;
        }
        return false;
    }
    
    private void inorder(TreeNode root, List <Integer> list) {
        if (root == null) return;
        
        inorder(root.left, list);
        list.add(root.val);
        inorder(root.right, list);
    }
}
```
