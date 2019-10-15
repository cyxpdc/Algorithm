- 问题
无序数组中最小的k个数

- 大根堆

维护一个k大小的大根堆，代表目前选出的k个最小的数。

堆顶元素是最小的k个数中最大的元素。

然后遍历整个数组，遍历的过程中判断当前数是否比堆顶元素小：

如果是，就把堆顶元素替换成当前数，然后调整堆；

如果不是，则不做任何操作，继续遍历下一个数；

在遍历完成后，堆中的k个数就是TopK。

- BFTER 

1.整个数组逻辑分组，每五个数一组（因为发明者是5个人，不过也有一定道理，因为要取中位数。。。），最后不满5个的，也成组；差不多N/5组    O(1)

2.分为组之后，组内排序，跨组不排序；O（N） 

3.每个数组拿出中位数（最后一个可能为不满5的偶数数组，拿上中位数），组成一个n/5的数组    O(N)

4.调用bfprt(new_arr,new_arr.length/2),返回num（中位数的中位数）   T(N/5)

5.这个num就是划分值,回到原数组，此时，左边右边的规模就能估计出来了， 开始荷兰国旗     O(N)

6.不为等于区域时，要走左右其中一个部分，而左右部分最坏的情况：如图中，3/10N是至少，因为中位数数组中有两个比中位数大，为N/10，这两个在各自的数组又是中位数，因此要加2个，为3/10N；因为可能四、五里面，上面的两个数也比中位数大（蓝圈）,即最多7/10N小    T（7/10N）
![1545546848617](https://github.com/cyxpdc/Algorithm/blob/master/image.png)
```java
public class BFPRT {
    // O(N*logK) 大根堆 找前K小的元素
    public static int[] getMinKNumsByHeap(int[] arr, int k) {
        if (k < 1 || k > arr.length) {
            return arr;
        }
        int[] kHeap = new int[k];//有k个数的大根堆
        //目前选出的k个最小的数
        for (int i = 0; i != k; i++) {
            heapInsert(kHeap, arr[i], i);
        }
        //遍历剩下的元素
        //遍历的过程中看当前数是否比堆顶元素小：
        //如果是，就把堆顶元素替换成当前数，然后调整堆；
        //如果不是，则不做任何操作，继续遍历下一个数；
        //在遍历完成后，堆中的k个数就是所有数组中最小的k个数。
        for (int i = k; i != arr.length; i++) {
            if (arr[i] < kHeap[0]) {
                kHeap[0] = arr[i];
                heapify(kHeap, 0, k);
            }
        }
        return kHeap;
    }

    public static void heapInsert(int[] arr, int value, int index) {
        arr[index] = value;
        while (index != 0) {
            int parent = (index - 1) / 2;
            if (arr[parent] < arr[index]) {
                swap(arr, parent, index);
                index = parent;
            } else {
                break;
            }
        }
    }

    public static void heapify(int[] arr, int index, int heapSize) {
        int left = index * 2 + 1;
        int right = index * 2 + 2;
        int largest = index;
        while (left < heapSize) {
            if (arr[left] > arr[largest]) {
                largest = left;
            }
            if (right < heapSize && arr[right] > arr[largest]) {
                largest = right;
            }
            if (largest != index) {
                swap(arr, largest, index);
            } else {
                break;
            }
            index = largest;
            left = index * 2 + 1;
            right = index * 2 + 2;
        }
    }

    // O(N) BFPRT
    public static int[] getMinKNumsByBFPRT(int[] arr, int k) {
        if (k < 1 || k > arr.length) {
            return arr;
        }
        int minKth = getMinKthByBFPRT(arr, k);
        int[] res = new int[k];
        int index = 0;
        for (int i = 0; i != arr.length; i++) {
            if (arr[i] < minKth) {
                res[index++] = arr[i];
            }
        }
        for (; index != res.length; index++) {
            res[index] = minKth;
        }
        return res;
    }

    public static int getMinKthByBFPRT(int[] arr, int K) {
        int[] copyArr = copyArray(arr);
        return bfprt(copyArr, 0, copyArr.length - 1, K - 1);
    }

    public static int[] copyArray(int[] arr) {
        int[] res = new int[arr.length];
        for (int i = 0; i != res.length; i++) {
            res[i] = arr[i];
        }
        return res;
    }

    public static int bfprt(int[] arr, int begin, int end, int i) {
        if (begin == end) {
            return arr[begin];
        }
        int pivot = medianOfMedians(arr, begin, end);
        int[] pivotRange = partition(arr, begin, end, pivot);
        if (i >= pivotRange[0] && i <= pivotRange[1]) {
            return arr[i];
        } else if (i < pivotRange[0]) {
            return bfprt(arr, begin, pivotRange[0] - 1, i);
        } else {
            return bfprt(arr, pivotRange[1] + 1, end, i);
        }
    }

    public static int medianOfMedians(int[] arr, int begin, int end) {
        int num = end - begin + 1;
        int offset = num % 5 == 0 ? 0 : 1;
        int[] mArr = new int[num / 5 + offset];//中位数组成的数组
        for (int i = 0; i < mArr.length; i++) {
            int beginI = begin + i * 5;
            int endI = beginI + 4;
            mArr[i] = getMedian(arr, beginI, Math.min(end, endI));
        }
        return bfprt(mArr, 0, mArr.length - 1, mArr.length / 2);//找中位数中的中位数
    }
    //划分后，返回中间区域的左右两个下标
    public static int[] partition(int[] arr, int begin, int end, int pivotValue) {
        int small = begin - 1;
        int cur = begin;
        int big = end + 1;
        while (cur != big) {
            if (arr[cur] < pivotValue) {
                swap(arr, ++small, cur++);
            } else if (arr[cur] > pivotValue) {
                swap(arr, cur, --big);
            } else {
                cur++;
            }
        }
        int[] range = new int[2];
        range[0] = small + 1;
        range[1] = big - 1;
        return range;
    }
    //5个数字排序
    public static int getMedian(int[] arr, int begin, int end) {
        insertionSort(arr, begin, end);
        int sum = end + begin;
        int mid = (sum / 2) + (sum % 2);
        return arr[mid];
    }

    public static void insertionSort(int[] arr, int begin, int end) {
        for (int i = begin + 1; i != end + 1; i++) {
            for (int j = i; j != begin; j--) {
                if (arr[j - 1] > arr[j]) {
                    swap(arr, j - 1, j);
                } else {
                    break;
                }
            }
        }
    }

    public static void swap(int[] arr, int index1, int index2) {
        int tmp = arr[index1];
        arr[index1] = arr[index2];
        arr[index2] = tmp;
    }
}
```
