https://blog.csdn.net/zxzxzx0119/article/details/81604489

```java
public class LongestSubarrayLessSumAwesomeSolution {
    //最优解 O（N）
    public static int maxLengthAwesome(int[] arr, int aim) {
        if (arr == null || arr.length == 0) {
            return 0;
        }
        int[] min_sum = new int[arr.length];
        int[] min_sum_index = new int[arr.length];
        //最后一个数
        min_sum[arr.length - 1] = arr[arr.length - 1];
        min_sum_index[arr.length-1] = arr.length - 1;
        //从倒数第二个数开始，得到两个信息数组
        for (int i = arr.length - 2; i >= 0; i--) {
            if (min_sum[i + 1] < 0) {//如果其后一个数小于0.则加上
                min_sum[i] = arr[i] + min_sum[i + 1];
                min_sum_index[i] = min_sum_index[i+1];
            } else {//如果大于0，则就是本身
                min_sum[i] = arr[i];
                min_sum_index[i] = i;
            }
        }
        //核心计算
        int R = 0;
        int sum = 0;
        int res = 0;
        //从每个位置开始
        for (int L = 0; L < arr.length; L++) {
            while (R < arr.length && sum + min_sum[R] <= aim) {//如果能扩，就扩出去
                sum += min_sum[R];
                R = min_sum_index[R] + 1;
            }
            res = Math.max(res, R - L);//计算结果，如果没扩出去，R-L为0
            //出循环后，L就扩到了R-1位置，R是用来给下一位判断是否能扩的一块
            sum -= R > L ? arr[L] : 0;//去掉当前位置，如果没扩出去，减0即可
            //有可能没扩出去，而下一步循环L会右移，R不能在L左边，起码要跟L同步，所以有L+1 
            R = Math.max(R, L + 1);
        }
        return res;
    }
}
```
