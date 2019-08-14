设计一个找到数据流中第K大元素的类（class）。注意是排序后的第K大元素，不是第K个不同的元素。

你的 KthLargest 类需要一个同时接收整数 k 和整数数组nums 的构造器，它包含数据流中的初始元素。

每次调用 KthLargest.add，返回当前数据流中第K大的元素。
>TopK问题
```java
class KthLargest {
    private Queue<Integer> heap;
    private int k;

    public KthLargest(int k, int[] nums) {
        heap = new PriorityQueue<>(k);
        this.k = k;
        for (int num : nums) {
            add(num);
        }
    }

    public int add(int val) {
        heap.add(val);
        if (heap.size() == k + 1) {
            heap.remove();
        }
        return heap.peek();
    }
}
```
