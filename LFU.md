两个map加一个链表
```java
public class LFU {
    //挂在次数节点下面的链表节点
    public static class Node {
        public Integer key;
        public Integer value;
        public Integer times;//其次数节点的次数，用来表示次数节点，节约了空间
        public Node up;//上头节点
        public Node down;//下头节点

        public Node(int key, int value, int times) {
            this.key = key;
            this.value = value;
            this.times = times;
        }
    }

    public static class LFUCache {
        //加上次数节点的整个竖向链表（次数节点是虚拟的，用Node里面的times来表示，可以节省次数节点的空间）
        public static class NodeList {
            public Node head;//挂在次数节点下面的链表头
            public Node tail;//挂在次数节点下面的链表尾
            public NodeList last;
            public NodeList next;
            //不接受空，有node才建次数节点
            public NodeList(Node node) {
                head = node;
                tail = node;
            }
            //这里是头部优先级高，跟笔记相反，都行
            public void addNodeFromHead(Node newHead) {
                newHead.down = head;
                head.up = newHead;
                head = newHead;
            }

            public boolean isEmpty() {
                return head == null;
            }
            //删掉任意节点
            public void deleteNode(Node node) {
                if (head == tail) {
                    head = null;
                    tail = null;
                } else {//不止一个
                    if (node == head) {//头
                        head = node.down;
                        head.up = null;
                    } else if (node == tail) {//尾
                        tail = node.up;
                        tail.down = null;
                    } else {//中间
                        node.up.down = node.down;
                        node.down.up = node.up;
                    }
                }
                //帮助JVM释放
                node.up = null;
                node.down = null;
            }
        }
        //小链表做完了，轮到整体链表
        private int capacity;//总容量
        private int size;//当前key数量
        //key-Node（Node里面也有key），方便查找得到Node
        private HashMap<Integer, Node> records;
        //节点属于哪个NodeList，，方便查找得到NodeList，用来处理set时节点更换次数节点
        private HashMap<Node, NodeList> heads;
        //整体链表的头链表，因为删除的时候，是要删除最小次数节点的尾，所以记录头链表方便查找
        private NodeList headList;
        //构造方法
        public LFUCache(int capacity) {
            this.capacity = capacity;
            this.size = 0;
            this.records = new HashMap<>();
            this.heads = new HashMap<>();
            headList = null;
        }
        //先判断有没有这个key，如果没有，就新加到1下，没1就new1；
        //如果有，改变key对应的value，然后先判断在哪个次数下（要解决的效率重点）
        //然后拿出这个key（调整这个次数下的其他节点，如果没有，删掉次数节点），判断次数加1的节点是否存在
        //若存在，直接挂尾，若不存在，new后再挂（可能需要调整，因为可能为2，且有1有3，需要插入中间）
        //如果超了，删除最小次数节点下面的头节点，如果只有一个次数节点，也是删头
        public void set(int key, int value) {
            //如果key存在
            if (records.containsKey(key)) {
                Node node = records.get(key);
                node.value = value;
                node.times++;
                NodeList curNodeList = heads.get(node);
                move(node, curNodeList);
            } else {//如果key不存在
                if (size == capacity) {//达到容量了，删掉头链表的尾
                    //删掉尾
                    Node node = headList.tail;
                    headList.deleteNode(node);
                    //如果删掉节点后，如果当前次数节点没有下挂节点了，需要删掉
                    modifyHeadList(headList);
                    records.remove(node.key);
                    heads.remove(node);
                    size--;
                }
                Node newNode = new Node(key, value, 1);
                if (headList == null) {
                    headList = new NodeList(newNode);
                } else {
                    //头链表的次数节点是否等于新节点的次数，即1
                    if (headList.head.times.equals(newNode.times)) {
                        headList.addNodeFromHead(newNode);
                    } else {
                        //建出次数节点为1的链表
                        NodeList newList = new NodeList(newNode);
                        newList.next = headList;
                        headList.last = newList;
                        headList = newList;
                    }
                }
                records.put(key, newNode);
                heads.put(newNode, headList);
                size++;
            }
        }
        //node来自oldNodeList，需要放在新的list上去
        private void move(Node node, NodeList oldNodeList) {
            oldNodeList.deleteNode(node);
            //如果老链表被删掉了，则需要连接其前后，所以得到它前面的链表，如果老链表是头链表，则pre为null
            //如果没被删掉，则连接的就是其本身和后链表
            NodeList preList = modifyHeadList(oldNodeList) ? oldNodeList.last
                    : oldNodeList;
            NodeList nextList = oldNodeList.next;
            //如果老链表的next为空，new一个出来后，节点挂在这下面
            if (nextList == null) {
                NodeList newList = new NodeList(node);
                if (preList != null) {
                    preList.next = newList;
                }
                newList.last = preList;
                if (headList == null) {//如果此时头链表为null，即整体链表只有new，就指向new
                    headList = newList;
                }
                heads.put(node, newList);
            } 
            //如果老链表的next不为空，判断次数是否与这个next相同
            else {
                //相同直接挂上去
                if (nextList.head.times.equals(node.times)) {
                    nextList.addNodeFromHead(node);
                    heads.put(node, nextList);
                } 
                //不同则new一个出来挂上去，连接好pre和new和next
                else {
                    NodeList newList = new NodeList(node);
                    if (preList != null) {
                        preList.next = newList;
                    }
                    newList.last = preList;
                    newList.next = nextList;
                    nextList.last = newList;
                    //因为此时的nextList之前有链表了，所以不能为头链表
                    //所以需要重新定位头链表
                    //发生在pre为null的时候,即next原来前面没有链表了，为头，有了new在前面后，new就需要为头链表
                    if (headList == nextList) {
                        headList = newList;
                    }
                    heads.put(node, newList);
                }
            }
        }
        //删除一个节点后，判断这个list是否要删掉
        private boolean modifyHeadList(NodeList nodeList) {
            if (nodeList.isEmpty()) {
                //如果是头链表
                if (headList == nodeList) {
                    headList = nodeList.next;
                    if (headList != null) {
                        headList.last = null;
                    }
                }
                //如果在中间或最后
                else {
                    nodeList.last.next = nodeList.next;
                    if (nodeList.next != null) {
                        nodeList.next.last = nodeList.last;
                    }
                }
                return true;
            }
            return false;
        }

        public int get(int key) {
            if (!records.containsKey(key)) {
                throw new RuntimeException("不存在这个节点");
            }
            Node node = records.get(key);
            node.times++;
            NodeList curNodeList = heads.get(node);
            move(node, curNodeList);//调整节点所在的次数节点
            return node.value;
        }
    }
}
```
