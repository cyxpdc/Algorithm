输入一个链表，按链表从尾到头的顺序返回一个ArrayList。
```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.*;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> res = new ArrayList<>();
        while(listNode != null){
            res.add(0,listNode.val);
            listNode = listNode.next;
        }
        return res;
    }
}
```
