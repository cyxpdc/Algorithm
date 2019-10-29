>自实现
```java
public class LRU {
    //双向链表节点
    private static class Node<K,V> {
        public K key;
        public V value;
        public Node<K,V> last;//指向头方向的指针
        public Node<K,V> next;//指向尾方向的指针

        public Node(K key,V value) {
            this.key = key;
            this.value = value;
        }
    }
    //双向链表
    private static class NodeDoubleLinkedList<K,V> {
        private Node<K,V> head;
        private Node<K,V> tail;

        public NodeDoubleLinkedList() {
            this.head = null;
            this.tail = null;
        }

        public void addNode(Node<K,V> newNode) {
            if (newNode == null) {
                return;
            }
            if (this.head == null) {
                this.head = newNode;
                this.tail = newNode;
            } else {
                this.tail.next = newNode;
                newNode.last = this.tail;
                this.tail = newNode;
            }
        }

        public void moveNodeToTail(Node<K,V> node) {
            if (this.tail == node) {
                return;
            }
            if (this.head == node) {
                this.head = node.next;
                this.head.last = null;
            } else {
                node.last.next = node.next;
                node.next.last = node.last;
            }
            node.last = this.tail;
            node.next = null;
            this.tail.next = node;
            this.tail = node;
        }

        public Node<K,V> removeHead() {
            if (this.head == null) {
                return null;
            }
            Node<K,V> res = this.head;
            if (this.head == this.tail) {
                this.head = null;
                this.tail = null;
            } else {
                this.head = res.next;
                res.next = null;
                res.last = null;//这一步可以没有，因为原先的head的last本来就为null
                this.head.last = null;
            }
            return res;
        }

    }

    public static class MyCache<K, V> {
        //跟笔记不一样，这里准备了两张表来映射
        private Map<K, Node<K,V>> keyNodeMap;
        private NodeDoubleLinkedList<K,V> nodeList;
        private int capacity;//内存大小

        public MyCache(int capacity) {
            if (capacity < 0) {
                throw new RuntimeException("容量应该大于0");
            }
            this.keyNodeMap = new HashMap<>();
            this.nodeList = new NodeDoubleLinkedList<>();
            this.capacity = capacity;
        }

        public V get(K key) {
            if (this.keyNodeMap.containsKey(key)) {
                Node<K,V> res = this.keyNodeMap.get(key);
                this.nodeList.moveNodeToTail(res);
                return res.value;
            }
            System.out.println("没有这个key");
            return null;
        }

        public void set(K key, V value) {
            if (this.keyNodeMap.containsKey(key)) {
                Node<K,V> node = this.keyNodeMap.get(key);
                node.value = value;
                this.nodeList.moveNodeToTail(node);
            } else {
                Node<K,V> newNode = new Node<>(key,value);
                this.keyNodeMap.put(key, newNode);
                this.nodeList.addNode(newNode);
                if (this.keyNodeMap.size() == this.capacity + 1) {
                    this.removeMostUnusedCache();
                }
            }
        }

        private void removeMostUnusedCache() {
            Node<K,V> removeNode = this.nodeList.removeHead();
            K removeKey = removeNode.key;
            this.keyNodeMap.remove(removeKey);
        }

    }
}
```
>LinkedHashMap
```java
public class LRULinkedMap<K, V> {

    /**
     * 最大缓存大小
     */
    private int capacity;

    private LinkedHashMap<K, V> cacheMap;

    public LRULinkedMap(int capacity){
        this.capacity = capacity;

        cacheMap = new LinkedHashMap(16, 0.75F, true){

            @Override
            protected boolean removeEldestEntry(Entry eldest) {
                if(cacheSize + 1 == cacheMap.size()){
                    return true;
                }else{
                    return false;
                }
            }
        };
    }

    public void put(K key, V value){
        cacheMap.put(key, value);
    }

    public V get(K key){
        return cacheMap.get(key);
    }

    public Collection<Map.Entry<K, V>> getAll(){
        return new ArrayList<Map.Entry<K, V>>(cacheMap.entrySet());
    }
}
```
