- 问题

给定一个没有重复元素的数组A，定义A上的MaxTree如下：MaxTree的根节点为A中最大的数，根节点的左子树为数组中最大数左边部分的MaxTree，右子树为数组中最大数右边部分的MaxTree。

请根据给定的数组A，设计一个算法构造这个数组的MaxTree。

- 解析

根据单调栈得到信息后：

1 左右都为null的为头节点

2 只有一个为null，直接挂在不为null的节点下

3 如果都不为null，则挂在小的底下

也可以使用大根堆做，直接构造大根堆即可

```java
public class MaxTree {

    public static class Node {
        public int value;
        public Node left;
        public Node right;

        public Node(int val) {
            this.value = val;
        }
    }

    public static Node getMaxTree(int[] arr) {
        Node[] nArr = new Node[arr.length];
        // 用数值构造出节点并保存在数组中
        for (int i = 0; i != arr.length; i++) {
            nArr[i] = new Node(arr[i]);
        }
        Deque<Node> stack = new ArrayDeque<>();
        Map<Node, Node> lBigMap = new HashMap<>();  // 用来保存每个节点左边数值比自己大的第一个节点
        Map<Node, Node> rBigMap = new HashMap<>();  // 用来保存每个节点右边数值比自己大的第一个节点
        for (int i = 0; i < nArr.length; i++) {
            Node curNode = nArr[i];
            //如果栈顶节点比自身节点的数值大，则从栈中弹出，同时构造弹出节点的lBigMap
            while (!stack.isEmpty() && stack.peek().value < curNode.value) {
                popStackAndSetMap(stack, lBigMap);
            }
            stack.push(curNode);  //栈顶结点为空或者比自己节点数值大，入栈
        }
        while (!stack.isEmpty()) {
            popStackAndSetMap(stack, lBigMap);
        }


        for (int i = nArr.length - 1; i >= 0; i--) {   //从右边开始，找右边的第一个比自己数值大的节点
            Node curNode = nArr[i];
            while (!stack.isEmpty() && stack.peek().value < curNode.value) {
                popStackAndSetMap(stack, rBigMap);
            }
            stack.push(curNode);
        }
        while (!stack.isEmpty()) {
            popStackAndSetMap(stack, rBigMap);
        }
        Node head = null;

        for (int i = 0; i < nArr.length; i++) {
            Node curNode = nArr[i];
            Node left = lBigMap.get(curNode);
            Node right = rBigMap.get(curNode);
            if (left == null && right == null) {
                head = curNode;
            } else if (left == null) {
                if (right.left == null) {
                    right.left = curNode;
                } else {
                    right.right = curNode;
                }
            } else if (right == null) {
                if (left.left == null) {
                    left.left = curNode;
                } else {
                    left.right = curNode;
                }
            } else {
                Node parent = left.value > right.value ? left : right;
                if (parent.left == null) {
                    parent.left = curNode;
                } else {
                    parent.right = curNode;
                }
            }
        }
        return head;
    }

    public static void popStackAndSetMap(Deque<Node> stack, Map<Node, Node> map) {
        Node popNode = stack.pop();//当前节点
        if (stack.isEmpty()) {
            map.put(popNode, null);
        } else {
            map.put(popNode, stack.peek());
        }
    }

    public static Node getMaxTree1(int[] arr) {
        Node[] nArr = new Node[arr.length];
        // 用数值构造出节点并保存在数组中
        for (int i = 0; i != arr.length; i++) {
            nArr[i] = new Node(arr[i]);
        }
        //得到信息
        Deque<Node> stack = new ArrayDeque<>();
        Map<Node, Node> lBigMap = new HashMap<>();  // 用来保存每个节点左边数值比自己大的第一个节点
        Map<Node, Node> rBigMap = new HashMap<>();  // 用来保存每个节点右边数值比自己大的第一个节点
        for (int i = 0; i < nArr.length; i++) {
            Node curNode = nArr[i];
            while (!stack.isEmpty() && stack.peek().value < curNode.value) {//如果栈顶节点比自身节点的数值大，则从栈中弹出，同时构造Map
                lBigMap.put(stack.pop(),stack.peek() == null ? null : stack.peek());
                rBigMap.put(stack.pop(),curNode);
            }
            stack.push(curNode);  //栈顶结点为空或者比自己节点数值大，入栈
        }
        while (!stack.isEmpty()) { //自然弹栈
            lBigMap.put(stack.pop(),stack.peek() == null ? null : stack.peek());
            rBigMap.put(stack.pop(),null);
        }

        Node head = null;
        for (int i = 0; i < nArr.length; i++) {
            Node curNode = nArr[i];
            Node left = lBigMap.get(curNode);
            Node right = rBigMap.get(curNode);
            if (left == null && right == null) {
                head = curNode;
            } else if (left == null) {
                if (right.left == null) {
                    right.left = curNode;
                } else {
                    right.right = curNode;
                }
            } else if (right == null) {
                if (left.left == null) {
                    left.left = curNode;
                } else {
                    left.right = curNode;
                }
            } else {
                Node parent = left.value > right.value ? left : right;
                if (parent.left == null) {
                    parent.left = curNode;
                } else {
                    parent.right = curNode;
                }
            }
        }
        return head;
    }
    /**
    * 大根堆
    */
    public static Node getMaxTree2(int[] arr){
        Node[] nArr = new Node[arr.length];
        // 用数值构造出节点并保存在数组中
        for (int i = 0; i != arr.length; i++) {
            nArr[i] = new Node(arr[i]);
        }
        //得到大根堆数组
        for(int i = 1;i < nArr.length;i++){
            heapInsert(nArr,i);
        }
        //连接为二叉树
        for(int i = 0;i < nArr.length;i++){
            Node cur = nArr[i];
            Node left = nArr[2*i+1] == null ? null : nArr[2*i+1];
            Node right = nArr[2*i+2] == null ? null : nArr[2*i+2];
            cur.left = left;
            cur.right = right;
        }
        return nArr[0];
    }

    public static void heapInsert(Node[] arr, int i) {
        while(arr[i].value > arr[(i-1)/2].value){
            swap(arr,i,(i-1)/2);
            i = (i - 1)/2;//重新定位当前操作的节点
        }
    }
    public static void swap(Node[] arr, int i, int j) {
		Node temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
    }
}


```
